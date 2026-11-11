1) En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras 
compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera 
impresora que se encuentre libre: 
a) Implemente un programa que permita resolver el problema anterior usando PMA. 
b) Resuelva el mismo problema anterior pero ahora usando PMS.


a) PMA
Chan documento(txt);

Process Empleado(id: 1..100){
    txt doc;
    while (true) {
        doc := RealizarDocumento();
        send documento(doc);
    }
}

Process Impresora(id: 1..5){
    txt doc;
    while (true) {
        receive documento(doc);
        Imprimir(doc);
    }
}

b) PMS
    Process Empleado[id: 1..100] {
    text doc;
    while (true) {
        doc = realizarDocumento();
        Coordinador ! documento(doc);
    }
}

Process Impresora[id: 1..5] {
    text doc;
    while (true) {
        Coordinador ! libre(id);
        Coordinador ? imprimir(doc);
        imprimirDocumento(doc);
    }
}

Process Coordinador {
    cola fila;
    text doc;
    int idI;
    do Empleado[] ? documento(doc) ->
            push(fila, doc);
       (not empty(fila)); Impresora[] ? libre(idI) ->
            pop(fila, doc);
            Impresora[idI] ! imprimir(doc);
    od
}