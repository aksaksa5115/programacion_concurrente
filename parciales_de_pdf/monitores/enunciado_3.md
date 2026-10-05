Simular el uso de un puente de un solo carril y sentido por donde puede pasar un vehículo a la vez.
Hay N vehículos donde algunos son autos y otros ambulancias. Los vehículos deben pasar de acuerdo
al orden de llegada, pero siempre dando prioridad a las ambulancias. Suponga que cada vehículo tarda 5
minutos en pasar por el puente.


Monitor Puente
    cola vehiculos
    boolean libre = true
    cond pasar
    cond vehiculos[1..N]
    cola ambulancias

    procedure llegar(vehiculo v){
        if(libre){
            libre = false
        } else{
            if(v.isAmbulance){
                ambulancias.push(v.id)
            } else{
                vehiculos.push(v.id)
            }
            wait(vehiculos[v.id])
        }
        signal(pasar)
    }

    procedure pasar(){
        int idAux;
        if (not ambulancias.isEmpty){
            idAux = ambulancias.push()
            signal(vehiculos[idAux])
        } else if (not vehiculos.isEmpty){
            idAux = fila.pop()
            signal(vehiculos[idAux])
        } else {
            libre = true
        }
    }
End Monitor

process vehiculos(id: 1..N){
    Puente.llegar(v)
    Puente.pasar()
}