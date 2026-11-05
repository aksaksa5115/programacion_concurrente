5. Resolver la administración de 3 impresoras de una oficina. Las impresoras son usadas por N 
administrativos, los cuales están continuamente trabajando y cada tanto envían documentos 
a imprimir. Cada impresora, cuando está libre, toma un documento y lo imprime, de 
acuerdo con el orden de llegada.  
a) Implemente una solución para el problema descrito. 
b) Modifique la solución implementada para que considere la presencia de un director de 
oficina que también usa las impresoras, el cual tiene prioridad sobre los 
administrativos. 
c) Modifique la solución (a) considerando que cada administrativo imprime 10 trabajos y 
que todos los procesos deben terminar su ejecución. 
d) Modifique la solución (b) considerando que tanto el director como cada administrativo 
imprimen 10 trabajos y que todos los procesos deben terminar su ejecución. 
e) Si la solución al ítem d) implica realizar Busy Waiting, modifíquela para evitarlo. 
Nota: ni los administrativos ni el director deben esperar a que se imprima el documento. 

5a)

chan Documentos(text);

Process Administrativo[id: 1..N] {
    text doc;
    while (true) {
        doc = generarDocumento();      # trabaja
        send Documentos(doc);          # lo manda a imprimir y sigue
    }
}


Process Impresora[id: 1..3] {
    text doc;
    while (true) {
        receive Documentos(doc);       # FIFO: orden de llegada
        delay(tiempoImpresion);        # imprime
    }
}

5b)

chan Documentos(text);
chan DocumentosJefe(text);
chan Hay(int);                 # aviso de que hay un documento (el valor no importa)
chan Libre(int);               # la impresora avisa que está libre
chan Trabajo[1..3](text);      # el coordinador le entrega el documento

Process Administrativo[id: 1..N] {
    text doc;
    while (true) {
        doc = generarDocumento();
        send Documentos(doc);
        send Hay(1);
    }
}

Process Director {
    text doc;
    while (true) {
        doc = generarDocumento();
        send DocumentosJefe(doc);
        send Hay(1);
    }
}

Process Coordinador {
    int idI, aux;
    text doc;
    while (true) {
        receive Hay(aux);                      # se bloquea hasta que haya algún documento
        receive Libre(idI);                    # se bloquea hasta que haya impresora libre
        if (not empty(DocumentosJefe)) {       # prioridad al director
            receive DocumentosJefe(doc);
        } else {
            receive Documentos(doc);           # seguro hay uno: Hay lo garantiza
        }
        send Trabajo[idI](doc);
    }
}

Process Impresora[id: 1..3] {
    text doc;
    while (true) {
        send Libre(id);
        receive Trabajo[id](doc);
        delay(tiempoImpresion);
    }
}

5c)

chan Documentos(text);
chan Fin(int);

Process Administrativo[id: 1..N] {
    text doc;
    for (i = 1..10) {
        doc = generarDocumento();
        send Documentos(doc);
    }
    send Fin(id);                      # avisa que terminó de enviar
}

Process Coordinador {
    int aux;
    for (i = 1..N) receive Fin(aux);   # espera a que terminen todos
    for (i = 1..3) send Documentos("FIN");   # una señal de fin por impresora
}

Process Impresora[id: 1..3] {
    text doc;
    receive Documentos(doc);
    while (doc <> "FIN") {
        delay(tiempoImpresion);
        receive Documentos(doc);
    }
}