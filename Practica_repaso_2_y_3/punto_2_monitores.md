2) Resolver el siguiente problema. En una empresa trabajan 20 vendedores ambulantes que forman 5
equipos de 4 personas cada uno (cada vendedor conoce previamente a qué equipo pertenece). Cada
equipo se encarga de vender un producto diferente. Las personas de un equipo se deben juntar antes de comenzar a trabajar. Luego cada integrante del equipo trabaja independientemente del resto
vendiendo ejemplares del producto correspondiente. Al terminar cada integrante del grupo debe
conocer la cantidad de ejemplares vendidos por el grupo. Nota: maximizar la concurrencia.



monitor Equipo[e: 1..5] {
    int llegaron = 0, terminaron = 0, total = 0
    cond inicio, fin

    procedure llegar() {
        llegaron++
        if (llegaron == 4)
            signal_all(inicio)          # el último despierta a los demás
        else
            wait(inicio)
    }

    procedure terminar(ventas: int, out totalGrupo: int) {
        total = total + ventas
        terminaron++
        if (terminaron == 4)
            signal_all(fin)             # ya sumaron los 4
        else
            wait(fin)
        totalGrupo = total              # recién acá el total está completo
    }
}

process Vendedor[id: 1..20] {
    int eq = equipo_de(id)              # cada uno conoce su equipo
    int mias, totalGrupo

    Equipo[eq].llegar()                 # me junto con mi equipo
    delay(vender)                       # vendo por mi cuenta
    mias = cantidad_vendida()
    Equipo[eq].terminar(mias, totalGrupo)
}