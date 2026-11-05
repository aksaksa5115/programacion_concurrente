2. Se desea modelar el funcionamiento de un banco en el cual existen 5 cajas para realizar 
pagos. Existen P clientes que desean hacer un pago. Para esto, cada uno selecciona la caja 
donde hay menos personas esperando; una vez seleccionada, espera a ser atendido. En cada 
caja, los clientes son atendidos por orden de llegada por los cajeros. Luego del pago, se les 
entrega un comprobante. Nota: maximizar la concurrencia. 


chan Coord(int, int);          # (tipo, dato): tipo 1 = llegó cliente (dato = idCliente)
                               #               tipo 2 = caja terminó (dato = nroCaja)
chan SiguienteCaja[1..P](int);
chan ColaCaja[1..5](int);
chan Atendido[1..P](text);

Process Cliente[id: 1..P] {
    int c;
    text comprobante;
    send Coord(1, id);
    receive SiguienteCaja[id](c);       # le dicen a qué caja ir
    send ColaCaja[c](id);               # se pone en la cola de esa caja
    receive Atendido[id](comprobante);
}

Process Coordinador {
    int cant[1..5] = ([5] 0);           # personas asignadas a cada caja
    int tipo, dato, pos;
    while (true) {
        receive Coord(tipo, dato);
        if (tipo == 1) {
            pos = cajaConMenosGente(cant);
            cant[pos]++;
            send SiguienteCaja[dato](pos);
        } else {
            cant[dato]--;               # la caja terminó con un cliente
        }
    }
}

Process Caja[id: 1..5] {
    int idCliente;
    text comprobante;
    while (true) {
        receive ColaCaja[id](idCliente);   # FIFO: orden de llegada
        delay(tiempoPago);
        comprobante = generarComprobante();
        send Atendido[idCliente](comprobante);
        send Coord(2, id);                 # avisa que liberó un lugar
    }
}