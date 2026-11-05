Se debe modelar el funcionamiento de una casa de comida rápida, en la cual trabajan 2 
cocineros y 3 vendedores, y que debe atender a C clientes. El modelado debe considerar 
que: 
- Cada cliente realiza un pedido y luego espera a que se lo entreguen. 
- Los pedidos que hacen los clientes son tomados por cualquiera de los vendedores y se 
lo pasan a los cocineros para que realicen el plato. Cuando no hay pedidos para atender, 
los vendedores aprovechan para reponer un pack de bebidas de la heladera (tardan entre 
1 y 3 minutos para hacer esto). 
- Repetidamente cada cocinero toma un pedido pendiente dejado por los vendedores, lo 
cocina y se lo entrega directamente al cliente correspondiente. 
Nota: maximizar la concurrencia.


chan Pedido(int);              # clientes -> coordinador
chan Libre(int);               # vendedor avisa que está libre
chan Asignacion[1..3](int);    # coordinador -> vendedor (idCliente, o -1 si no hay)
chan Cocinar(int);             # vendedores -> cocineros
chan Entrega[1..C](bool);      # cocinero -> cliente

Process Cliente[id: 1..C] {
    bool ok;
    send Pedido(id);
    receive Entrega[id](ok);
}

Process Coordinador {
    int idV, idC;
    while (true) {
        receive Libre(idV);
        if (empty(Pedido)) {
            idC = -1;
        } else {
            receive Pedido(idC);
        }
        send Asignacion[idV](idC);
    }
}

Process Vendedor[id: 1..3] {
    int idC;
    while (true) {
        send Libre(id);
        receive Asignacion[id](idC);
        if (idC == -1) {
            delay(random(60, 180));    # repone un pack de bebidas (1 a 3 min)
        } else {
            send Cocinar(idC);         # se lo pasa a los cocineros
        }
    }
}

Process Cocinero[id: 1..2] {
    int idC;
    while (true) {
        receive Cocinar(idC);          # el primer pedido pendiente (FIFO)
        delay(tiempoCocina);           # prepara el plato
        send Entrega[idC](true);       # se lo entrega directo al cliente
    }
} 