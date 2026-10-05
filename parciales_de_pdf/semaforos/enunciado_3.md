En una competencia culinaria se juntan 1 jurado y C concursantes. Una vez que todos los concursantes
llegaron, el jurado les asigna la receta que deben realizar. A continuación, los concursantes cocinan el plato
pedido (a cada uno le lleva un tiempo variable) y lo exhiben ante el jurado en el orden en que van terminando.
El jurado asigna un puntaje a cada concursante, el cual lo guarda para su CV

Cola fila;                              // concursantes en orden de terminación
int receta[1..C], puntaje[1..C];
sem llegaron = 0;
sem recetaLista[1..C] = ([C], 0);
sem mutex = 1;                          // protege la cola
sem hayListo = 0;
sem evaluado[1..C] = ([C], 0);

Process Concursante [id: 1..C]
  int miPuntaje;
  V(llegaron);                          // aviso que llegué
  P(recetaLista[id]);                   // espero que el jurado me asigne la receta
  delay(cocinar(receta[id]));           // tiempo variable, fuera de toda sección crítica
  P(mutex);
  fila.push(id);                        // me anoto en orden de terminación
  V(mutex);
  V(hayListo);                          // aviso al jurado
  P(evaluado[id]);                      // espero mi puntaje
  miPuntaje = puntaje[id];              // lo guardo para mi CV
End Process;

Process Jurado
  int idAux;
  for (i = 1; i <= C; i++)
    P(llegaron);                        // espero que lleguen los C
  for (i = 1; i <= C; i++) {
    receta[i] = asignarReceta();
    V(recetaLista[i]);                  // la receta ya está cargada
  }
  for (i = 1; i <= C; i++) {
    P(hayListo);                        // espero que haya alguien para evaluar
    P(mutex);
    fila.pop(idAux);                    // el primero que terminó
    V(mutex);
    puntaje[idAux] = Evaluar(idAux);    // fuera del mutex
    V(evaluado[idAux]);                 // el puntaje ya está cargado
  }
End Process;