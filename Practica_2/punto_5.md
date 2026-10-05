5. En una empresa de logística de paquetes existe una sala de contenedores donde se 
preparan las entregas. Cada contenedor puede almacenar un paquete y la sala cuenta con 
capacidad para N contenedores. Resuelva considerando las siguientes situaciones: 
a) La empresa cuenta con 2 empleados: un empleado Preparador que se ocupa de 
preparar los paquetes y dejarlos en los contenedores; un empleado Entregador 
que se ocupa de tomar los paquetes de los contenedores y realizar las entregas. 
Tanto el Preparador como el Entregador trabajan de a un paquete por vez. 
b) Modifique la solución a) para el caso en que haya P empleados Preparadores. 
c) Modifique la solución a) para el caso en que haya E empleados Entregadores. 
d) Modifique la solución a) para el caso en que haya P empleados Preparadores y E 
empleados Entregadores.


paquete buf[N]
sem vacio = N, lleno = 0
sem mutexE = 1;
int ocupado = 0

process Preparador(id: 1..P) {
    while (true) {
        delay(preparar_paquete)
        P(vacio)
        P(mutexP)
        buf[libre] = paquete
        libre = (libre + 1) mod N 
        P(mutexP)
        V(lleno)
    }
}

process Entregador(id: 1..E) {
    while (true) {
        P(lleno)
        P(mutexE)
        paquete = buf[ocupado]
        ocupado = (ocupado + 1) mod N 
        V(mutexE)
        V(vacio)
        delay(realizar_entrega)
    }
}
