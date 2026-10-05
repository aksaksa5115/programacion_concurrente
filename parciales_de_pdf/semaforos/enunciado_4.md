En un examen final hay P alumnos y 3 profesores. Cuando todos los alumnos y los profesores han llegado
comienza el examen. Para esto uno de los profesores (el primero en llegar) le da el examen a cada alumno.
Cada alumno resuelve su examen, lo entrega y espera que alguno de los profesores lo corrija y le indique la
nota. Los profesores corrigen los examenes respetando el orden en que los alumnos van entregando.

Cola entregas;
int nota[1..P];
int numProf = 0, tomadas = 0;
sem llegaron = 0;
sem comienzo[1..P] = ([P], 0);
sem mutexProf = 1, mutexCola = 1, mutexTom = 1;
sem hayEntrega = 0;
sem corregido[1..P] = ([P], 0);

Process Alumno [id: 1..P]
  int miNota;
  V(llegaron);                          // aviso que llegué
  P(comienzo[id]);                      // espero que me den el examen
  delay(resolver);                      // fuera de toda sección crítica
  P(mutexCola);
  entregas.push(id);                    // en orden de entrega
  V(mutexCola);
  V(hayEntrega);                        // aviso a los profesores
  P(corregido[id]);                     // espero mi nota
  miNota = nota[id];
End Process;

Process Profesor [id: 1..3]
  int orden, idAux;
  boolean seguir = true;
  V(llegaron);                          // aviso que llegué
  P(mutexProf);
  numProf = numProf + 1;
  orden = numProf;                      // 1 = primero en llegar
  V(mutexProf);

  if (orden == 1) {
    for (i = 1; i <= P + 3; i++)
      P(llegaron);                      // espero a que lleguen todos
    for (i = 1; i <= P; i++)
      V(comienzo[i]);                   // reparto el examen a cada alumno
  }

  while (seguir) {
    P(mutexTom);
    if (tomadas == P)
      seguir = false;                   // ya se repartieron todas las correcciones
    else
      tomadas = tomadas + 1;            // reservo una corrección para mí
    V(mutexTom);
    if (seguir) {
      P(hayEntrega);                    // espero que haya un examen entregado
      P(mutexCola);
      entregas.pop(idAux);              // el primero en haberse entregado
      V(mutexCola);
      nota[idAux] = Corregir(idAux);    // fuera de toda sección crítica
      V(corregido[idAux]);              // la nota ya está cargada
    }
  }
End Process;