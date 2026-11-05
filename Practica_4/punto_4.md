Simular la atención en un locutorio con 10 cabinas telefónicas, el cual tiene un empleado 
que se encarga de atender a N clientes. Al llegar, cada cliente espera hasta que el empleado 
le indique a qué cabina ir, la usa y luego se dirige al empleado para pagarle. El empleado 
atiende a los clientes en el orden en que hacen los pedidos. A cada cliente se le entrega un 
ticket factura por la operación.  
a) Implemente una solución para el problema descrito. 
b) Modifique la solución implementada para que el empleado dé prioridad a los que 
terminaron de usar la cabina sobre los que están esperando para usarla. 
Nota: maximizar la concurrencia; suponga que hay una función Cobrar() llamada por el 
empleado que simula que el empleado le cobra al cliente.

chan Mensajes(int, int, int);        # (tipo, idCliente, cabina): 1 = pedido, 2 = pago
chan Asignada[1..N](int);
chan Ticket[1..N](text);

Process Cliente[id: 1..N] {
    int cabina;
    text ticket;
    send Mensajes(1, id, 0);             # llegué, pido cabina
    receive Asignada[id](cabina);        # espero que me digan cuál
    delay(tiempoLlamada);                # uso la cabina
    send Mensajes(2, id, cabina);        # voy a pagar
    receive Ticket[id](ticket);
}

Process Empleado {
    int tipo, idC, cab;
    text t;
    cola libres = [1..10];               # cabinas libres (local)
    cola pendientes = [];                # clientes esperando cabina (local, FIFO)
    while (true) {
        receive Mensajes(tipo, idC, cab);
        if (tipo == 1) {
            if (not vacia(libres)) {
                cab = sacar(libres);
                send Asignada[idC](cab);
            } else {
                push(pendientes, idC);
            }
        } else {
            Cobrar();
            t = generarTicket();
            send Ticket[idC](t);
            if (not vacia(pendientes)) {         # la cabina liberada va al primero que espera
                idC = sacar(pendientes);
                send Asignada[idC](cab);
            } else {
                push(libres, cab);
            }
        }
    }
}



b)

chan Pedido(int);
chan Pago(int, int);               # (idCliente, cabina)
chan Asignada[1..N](int);
chan Ticket[1..N](text);
chan Libres(int);

Process Cliente[id: 1..N] {
    int cabina;
    text ticket;
    send Pedido(id);
    receive Asignada[id](cabina);
    delay(tiempoLlamada);
    send Pago(id, cabina);
    receive Ticket[id](ticket);
}

Process Empleado {
    int idC, cab;
    int libres = 10;
    text t;
    for (i = 1..10) send Libres(i);
    while (true) {
        if (not empty(Pago)) {
            receive Pago(idC, cab);
            Cobrar();
            t = generarTicket();
            send Ticket[idC](t);
            send Libres(cab);
            libres++;
        } else if (not empty(Pedido) and libres > 0) {
            receive Pedido(idC);
            receive Libres(cab);
            libres--;
            send Asignada[idC](cab);
        }
    }
}