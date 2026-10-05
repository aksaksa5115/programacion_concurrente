Una empresa de turismo posee un micro con capacidad para 50 personas. Hay un único vendedor que
atiende a los C clientes (C > 50) que intentan comprar un pasaje de acuerdo al orden de llegada (suponga
que la atención de un cliente tarda un par de minutos), si aún hay lugares disponibles, le indica el número de
asiento que le tocó. Cada cliente, luego de ser atendidos por el vendedor, se dirige al micro para subir en
caso de que le hayan dado un asiento. El micro espera a que los 50 pasajeros hayan subido para realizar el
viaje.

Cola fila;
int asiento[1..C];
sem mutex = 1;
sem hayClientes = 0;
sem atendido[1..C] = ([C], 0);
sem subio = 0;

procedure cliente[id: 1..C](){
    P(mutex)
    fila.push(id)
    V(mutex)
    V(hayClientes)
    P(atendido[id])
    if (asiento[id] > 0){
        V(subio)
    }
}

procedure vendedor(){
    int idAux;
    int ocupados = 0
    for (int i=1; i < C; i++){
        P(hayPersonas)
        P(mutex)
        idAux = fila.pop()
        V(mutex)
        delay(atencion)
        if (ocupados < 50){
            ocupados += 1
            asiento[idAux] = ocupados
        } else {
            asiento[idAux] = 0
        }
        V(atendido[idAux])
    }
}

procedure colectivo(){
    for (int i=1; i < 50; i++){
        P(subio)
    }
    delay(viaje)
}