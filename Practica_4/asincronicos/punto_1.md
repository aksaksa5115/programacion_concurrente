1. Suponga que N clientes llegan a la cola de un banco y que serán atendidos por sus 
empleados. Analice el problema y defina qué procesos, recursos y canales/comunicaciones 
serán necesarios/convenientes para resolverlo. Luego, resuelva considerando las siguientes 
situaciones: 
a. Existe un único empleado, el cual atiende por orden de llegada. 
b. Ídem a) pero considerando que hay 2 empleados para atender, ¿qué debe 
modificarse en la solución anterior? 
c. Ídem b) pero considerando que, si no hay clientes para atender, los empleados 
realizan tareas administrativas durante 15 minutos. ¿Se puede resolver sin usar 
procesos adicionales? ¿Qué consecuencias implicaría? 

a y b)

chan pedido(int);
chan atendido[0..N-1](bool);

Process Cliente[id: 0..N-1] {
    bool ok;
    send pedido(id);
    receive atendido[id](ok);
}

Process Empleado (id: 1..2){
    int idC;
    bool ok = true;
    while (true) {
        receive pedido(idC);
        delay(tiempoAtencion);
        send atendido[idC](ok);
    }
}

c)

chan pedido(int);
chan atendido[0..N-1](bool);

Process Cliente[id: 0..N-1] {
    bool ok;
    send pedido(id);
    receive atendido[id](ok);
}

Process Empleado (id: 1..2){
    int idC;
    bool ok = true;
    while (true) {
        if(empty(pedido)){
            delay(900)
        } else {
            receive pedido(idC);
            delay(tiempoAtencion);
            send atendido[idC](ok);
        }
    }
}