Resolver con monitores el siguiente problema. En un sistema operativo se ejecutan 20 procesos que
periódicamente realizan cierto cómputo mediante la función Procesar(). Los resultados de dicha función son
persistidos en un archivo, para lo que se requiere de acceso al subsistema de E/S. Sólo un proceso a la vez
puede hacer uso del subsistema de E/S, y el acceso al mismo se define por la prioridad del proceso (menos
valor indica mayor prioridad).

Monitor subsistema
    bool libre = true
    cola fila
    cond espera[1..20]

    procedure pedir(int id, int prioridad){
        if (libre){
            libre = false;
        } else {
            fila.insertarOrdenado(id, prioridad)
            wait(espera[id])
        }
    }

    procedure liberar(){
        if (fila.isEmpty()){
            libre = true
        } else {
            persona sig = fila.pop()
            signal(espera[sig.id])
        }
    }
End Monitor

process proceso(id: 1..20){
    int prioridad = prioridad(id)
    int resultado;
    while (true){
        resultado = procesar()
        subsistema.pedir(id, prioridad)
        Persistir(resultado)
        subsistema.liberar()
    }
}