Resolver con semáforos el siguiente problema. En una planta verificadora de vehículos, existen 7 estaciones
donde se dirigen 150 vehículos para ser verificados. Cuando un vehículo llega a la planta, el coordinador de la
planta le indica a qué estación debe dirigirse. El coordinador selecciona la estación que tenga menos
vehículos en ese momento. Una vez que el vehículo sabe qué estación le fue asignada, se dirige a la misma y
espera a que lo llamen para verificar. Luego de la revisión, la estación le entrega un comprobante que indica
si pasó la revisión o no. Más allá del resultado, el vehículo se retira de la planta. Nota: maximizar la
concurrencia. Resolución guardada.

Cola llegadas;                        // vehículos esperando asignación
Cola fila[1..7];                      // fila de cada estación
int cantidad[1..7] = ([7], 0);        // vehículos asignados a cada estación
int estacionDe[1..150];
int comprobante[1..150];
sem mutexLleg = 1, hayVehiculo = 0;
sem mutexFila[1..7] = ([7], 1);
sem hayEnFila[1..7] = ([7], 0);
sem mutexCant = 1;
sem asignado[1..150] = ([150], 0);
sem fin[1..150] = ([150], 0);


process vehiculo(id: 1..150){
    P(mutexLleg)
    llegadas.push(id)
    V(mutexLleg)
    V(hayVehiculo)
    P(asignado[id])
    P(fin[id])

}

process coordinador(){
    int idAux, e;
    for (i=1; i <= 150; i++){
        P(hayVehiculo)

        P(mutexLleg)
        idaux = llegadas.pop()
        V(mutexLleg)

        P(mutexCant)
        e = estacion_mas_vacia(cantidad)
        cantidad[e] += 1
        V(mutexCant)

        estacionDe[idAux] = e
        V(asignado[idAux])

        P(mutexFila[e])
        fila[e].push(idAux)
        V(mutexFila[e])
        V(hayEnFila(e))
    }
    for (i=1; i <= 7; i++){
        P(mutexFila[i])
        fila.push(0)
        V(mutexFila[i])
        V(hayEnFila[i])
    }
}

process estacion[e: 1..7](){
    int idAux
    P(hayEnFila[e])
    P(mutexFila[e])
    idAux = fila[e].pop()
    V(mutexFila(e))
    while (idAux != 0){
        comprobante[idAux] = verificar(idAux)
        P(mutexCant)
        cantidad[e] += -1
        V(mutexCant)
        V(fin[idAux])
        P(hayEnFila[e])
        P(mutexFila[e])
        idAux = fila[e].pop()
        V(mutexFila[e])
    }
}