Existen N personas que deben imprimir un trabajo cada una. Resolver cada ítem usando 
semáforos: 
a) Implemente una solución suponiendo que existe una única impresora compartida por 
todas las personas, y las mismas la deben usar de a una persona a la vez, sin importar 
el orden. Existe una función Imprimir(documento) llamada por la persona que simula el 
uso de la impresora. Sólo se deben usar los procesos que representan a las Personas. 
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada. 
c) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el 
orden dado por el identificador del proceso (la persona X no puede usar la impresora 
hasta que no haya terminado de usarla la persona X-1). 
d) Modifique la solución de (b) para el caso en que además hay un proceso Coordinador 
que le indica a cada persona que es su turno de usar la impresora. 
e) Modificar la solución (d) para el caso en que sean 5 impresoras. El coordinador le 
indica a la persona cuándo puede usar una impresora, y cual debe usar.  

a)

sem mutex = 1

process persona(id: 1..N){
    P(mutex)
    imprimir(documento)
    V(mutex)
}

b)
sem fila[P] = ([P], 0)
boolean libre = true
cola c
sem mutex = 1
process persona(id: 1..N){
    idAux;
    P(mutex)
    if (not libre){
        c.push(id)
        V(mutex)
        P(fila[id])  
    } else {
        libre = false
        V(mutex)
    }
    imprimir(documento)
    P(mutex)
    if (c.isEmpty()){
        libre = true
    } else { 
        idAux = c.pop()
        V(fila[idAux])
    }
    V(mutex)
}

c)
sem turno[1..N] = ([N], 0);

Process Persona [id: 1..N]
  if (id > 1)
    P(turno[id]);          // la 1 no espera a nadie
  Imprimir(documento);
  if (id < N)
    V(turno[id + 1]);
End Process;

d)
Cola c;                           // FIFO de ids, en orden de llegada
sem mutex = 1;
sem hayPersonas = 0;
sem turno[1..N] = ([N], 0);
sem termino = 0;

Process Persona [id: 1..N]
  P(mutex);
  c.push(id);                     // me anoto en la fila
  V(mutex);
  V(hayPersonas);                 // aviso al Coordinador que hay alguien
  P(turno[id]);                   // espero a que me autorice
  Imprimir(documento);            // fuera de la sección crítica
  V(termino);                     // aviso que terminé
End Process;

Process Coordinador
  int idAux;
  for (i = 1; i <= N; i++) {      // cada persona imprime una sola vez
    P(hayPersonas);               // espero que haya alguien en la fila
    P(mutex);
    c.pop(idAux);                 // la primera en llegar
    V(mutex);
    V(turno[idAux]);              // le indico que es su turno
    P(termino);                   // espero a que termine de imprimir
  }
End Process;

e)

Cola c;                               // FIFO de ids (orden de llegada)
Cola libres;                          // inicialmente contiene 1..5
int asignada[1..N];                   // impresora asignada a cada persona
sem mutex = 1, mutexLibres = 1;
sem hayPersonas = 0;
sem hayImpresora = 5;
sem turno[1..N] = ([N], 0);

Process Persona [id: 1..N]
  int imp;
  P(mutex);
  c.push(id);                         // me anoto en la fila
  V(mutex);
  V(hayPersonas);                     // aviso al Coordinador
  P(turno[id]);                       // espero autorización
  imp = asignada[id];                 // la impresora que me indicó
  Imprimir(documento, imp);           // fuera de toda sección crítica
  P(mutexLibres);
  libres.push(imp);                   // devuelvo la impresora
  V(mutexLibres);
  V(hayImpresora);                    // aviso: hay una impresora libre más
End Process;

Process Coordinador
  int idAux, imp;
  for (i = 1; i <= N; i++) {
    P(hayPersonas);                   // espero que haya alguien en la fila
    P(hayImpresora);                  // espero que haya una impresora libre
    P(mutex);
    c.pop(idAux);                     // la primera en llegar
    V(mutex);
    P(mutexLibres);
    libres.pop(imp);                  // una impresora libre
    V(mutexLibres);
    asignada[idAux] = imp;            // se la comunico a la persona
    V(turno[idAux]);                  // le indico que puede imprimir
  }
End Process;



