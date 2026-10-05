Resolver con SENTENCIAS AWAIT (<> y <await B; S>). Un sistema operativo mantiene 
5 instancias de un recurso almacenadas en una cola, cuando un proceso necesita usar una 
instancia del recurso la saca de la cola, la usa y cuando termina de usarla la vuelve a depositar.  

PRECONDICION
- Cola inicializada con 5 elementos (instancias del recurso).
- haya_recurso := 5  (cantidad de instancias disponibles)

process proceso(id = 0..P-1){
    <await (haya_recurso);
        haya_recurso -= 1 
        recurso = pop(cola)
        >

    usar_recurso(recurso)

    <push(recurso, cola)
        haya_recurso++>
}
