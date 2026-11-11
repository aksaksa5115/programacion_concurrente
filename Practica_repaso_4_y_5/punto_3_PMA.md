3) Resolver el siguiente problema con PMA. En un negocio de cobros digitales hay P personas que 
deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son 
atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos 
de 5 boletas de los que pagan más. Adicionalmente, las personas embarazadas tienen prioridad sobre 
los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les 
devuelve el vuelto y los recibos de pago. 


chan cobro(int)
chan cobroPrioridad(int)
chan cobroPrioridadAlta(int)
chan pagar[1..P](int)

process persona(id: 1..P){
    cant_boletas: int
    embarazada: bool
    vuelto: int
    recibo: text

    situacionDeLaPersona(cant_boletas, embarazada)
    if (embaradaza = true){
        send cobroPrioridadAlta(id)
    } else if (cant_boletas < 5)  {
        send cobroPrioridad(id)
    } else {
        send cobro(id)
    }
    received pagar(vuelto, recibo)
}

process cajero(){
    idP: int

    while (true){
        if (empty(cobroPrioridadAlta) && (empty(cobroPrioridad)) && (not empty(cobro))) ->
            received cobro(idP)
            send pagar[idP](vuelto, recibo)
            
        [] (not empty(cobroPrioridadAlta)) ->
            received cobroPrioridadAlta(idP)
            send pagar[idP](vuelto, recibo)

        [] (not empty(cobroPrioridad) && (empty(cobroPrioridadAlta))) ->
            received cobroPrioridad(idP)
            send pagar[idP](vuelto, recibo)

        fi
    }
}