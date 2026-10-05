Implemente una solución para el siguiente problema. Se debe simular el uso de una máquina expendedora de gaseosas con capacidad para 100 latas por parte de U usuarios. Además, existe un repositor encargado de reponer las latas de la máquina. Los usuarios usan la máquina según el orden de llegada. Cuando les toca usarla, sacan una lata y luego se retiran. En el caso de que la máquina se quede sin latas, entonces le debe avisar al repositor para que cargue nuevamente la máquina en forma completa. Luego de la recarga, saca una botella y se retira. Nota: maximizar la concurrencia; mientras se reponen las latas se debe permitir que otros usuarios puedan agregarse a la fila.

sem mutexM = 1
sem mutexR = 1
int gaseosas = 100
boolean libre = true
cola c
sem espera[P] = ([P], 0);


process usuarios(id: 1..u){
    int idAux;
    P(mutexC)
    if not (libre){
        cola.push(id)
        V(mutexC)
        P(espera[id])
    } else {
        libre = false
        V(mutexC)
    }
    if (gaseosas == 0){
        P(mutexR)
        delay(reponer)
        gaseosas = 100
        gaseosas += -1
        V(mutexR)
    } 
    gaseosas += -1
    if (c.isEmpty()){
        libre = true
    } else{
        P(mutexC)
        idAux = c.pop()
        V(mutexC)
        V(espera[idAux])
    }


    usuario bloquea la maquina

    usuario verifica que haya gaseosa
    si hay gaseosa, saca una y libera la maquina
    si no hay, avisa a un repartidor
    espera a que cargue
    y saca una gaseosa
    y libera la maquina
}