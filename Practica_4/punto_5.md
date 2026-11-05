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
chan impresion[1..3](txt)
chan libre(int)


Process Administrativo[id: 1..N] {
    text doc;
    while (true) {
        doc = generarDocumento();      # trabaja
        send Documentos(doc);          # lo manda a imprimir y sigue
    }
}


process coordinador(){
    txt doc;
    int idI;
    while (true)
        receive libre(idI)
        if (!empty(DocumentosJefe)){
            receive DocumentosJefe(doc)
        } else if (!empty(Documentos)){
            receive Documentos(doc)
        }
        send Impresion[idI](doc)
}

Process Impresora[id: 1..3] {
    text doc;
    while (true) {
        send libre(id);
        receive Impresion[id](doc);       # FIFO: orden de llegada
        delay(tiempoImpresion);        # imprime
    }
}
