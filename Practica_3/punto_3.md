3. Existen N personas que deben fotocopiar un documento. La fotocopiadora sólo puede ser 
usada por una persona a la vez. Analice el problema y defina qué procesos, recursos y 
monitores serán necesarios/convenientes, además de las posibles sincronizaciones requeridas 
para resolver el problema. Luego, resuelva considerando las siguientes situaciones: 
a) Implemente una solución suponiendo que no importa el orden de uso. Existe una función 
Fotocopiar() que simula el uso de la fotocopiadora.  
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada. 
c) Modifique la solución de (b) para el caso en que se deba dar prioridad de acuerdo con la 
edad de cada persona (cuando la fotocopiadora está libre, la debe usar la persona de mayor 
edad entre las que estén esperando para usarla). 
d) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden 
dado por el identificador del proceso (la persona X no puede usar la fotocopiadora hasta 
que no haya terminado de usarla la persona X-1). 
e) Modifique la solución de (b) para el caso en que además haya un Empleado que le indica 
a cada persona cuándo debe usar la fotocopiadora. 
f) Modificar la solución (e) para el caso en que sean 10 fotocopiadoras. El empleado le indica 
a la persona qué fotocopiadora usar y cuándo hacerlo. 

a)

Monitor fotocopiadora
    cond cola
    int disponible = 1
    array ids = array[1..N] of int
    int indice = 1

    procedure usar_impresora(){
        ids[i] = persona.id

        while (disponible == 0) wait (cola)
        disponible += -1
    }

    procedure dejar_impresora(){
        disponible += 1
        signal(cola)
    }

end Monitor

process personas (id: 1..N){
    fotocopiadora.usar_impresora()
    delay(fotocopiar)
    fotocopiadora.dejar_impresora()
}

b)

Monitor Fotocopiadora 
    bool libre = true;    
    cond cola;    
    int esperando = 0; // Contador de personas en espera (reemplaza a empty)

    Procedure pedir () {        
        if (not libre) {            
            esperando = esperando + 1;            
            wait(cola); // Se bloquea en la cola FIFO de la variable condición        
        }
        else {            
            libre = false; // Solo el primero que encuentra la fotocopiadora libre la ocupa        
            }    
    }

    Procedure liberar () {        
        if (esperando > 0) {            
            esperando = esperando - 1;            
            signal(cola); // Despierta al primero en orden de llegada (NO se pone libre = true)
        }        
        else {            
            libre = true; // Solo si no hay nadie esperando, se marca como libre        
            }    
        }
end Monitor

Process Persona[id: 1..N] {    
    Fotocopiadora.pedir();    
    Fotocopiar(); // Simulación del uso fuera del monitor para maximizar la concurrencia    
    Fotocopiadora.liberar();
}

c)

Monitor fotocopiadora
    bool libre = True
    cond espera[1..N];
    queue cola;

    procedure pedir(int id, int edad){
        if (libre){
            libre = False
        } else {
            cola.insertarOrdenadoDescendente(id, edad)
            wait(espera[id])
        }
    }

    procedure terminar(){
        if (cola.isEmpty()){
            libre = True
        } else {
            next = cola.pop()
            signal(espera[next.id])
        }
    }
End Monitor
Process Persona[id: 1..N] {    
    Fotocopiadora.pedir(id, edad);    
    Fotocopiar(); // Simulación del uso fuera del monitor para maximizar la concurrencia    
    Fotocopiadora.terminar();
}

d)
Monitor fotocopiadora
    cond cola[1..N]
    int indice = 1

    procedure usar_impresora(int id){
        if (id != indice)
            wait(cola[id])
    }

    procedure dejar_impresora(){
        indice += 1
        if (id <= N)
        signal(cola[indice])
    }

end Monitor
Process Persona[id: 1..N] {    
    Fotocopiadora.pedir(id);    
    Fotocopiar(); // Simulación del uso fuera del monitor para maximizar la concurrencia    
    Fotocopiadora.terminar();
}

e)
TA MAL
Monitor Fotocopiadora 
    bool libre = true;    
    cond cola;    
    int esperando = 0; // Contador de personas en espera (reemplaza a empty)
    empleado emp

    Procedure pedir () {        
        if (not libre) {            
            esperando = esperando + 1;            
            wait(cola); // Se bloquea en la cola FIFO de la variable condición        
        }
        else if (emp.permite()) {      
            libre = false; // Solo el primero que encuentra la fotocopiadora libre la ocupa        
        }   
    }

    Procedure liberar () {        
        if (esperando > 0 && emp.permite()) {            
            esperando = esperando - 1;            
            signal(cola); // Despierta al primero en orden de llegada (NO se pone libre = true)
        }        
        else {            
            libre = true; // Solo si no hay nadie esperando, se marca como libre        
            }    
        }
end Monitor

Process Persona[id: 1..N] {    
    Fotocopiadora.pedir();    
    Fotocopiar(); // Simulación del uso fuera del monitor para maximizar la concurrencia    
    Fotocopiadora.liberar();
}