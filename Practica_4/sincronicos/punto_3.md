3. En un examen final hay N alumnos y P profesores. Cada alumno resuelve su examen, lo 
entrega y espera a que alguno de los profesores lo corrija y le indique la nota. Los 
profesores corrigen los exámenes respetando el orden en que los alumnos van entregando.  
a) Considerando que P=1. 
b) Considerando que P>1. 
c) Ídem b) pero considerando que los alumnos no comienzan a realizar su examen hasta 
que todos hayan llegado al aula. 
Nota: maximizar la concurrencia; no generar demora innecesaria; todos los procesos deben 
terminar su ejecución

a)

Process Alumno[id: 1..N] {
    text examen;
    int nota;
    examen = resolverExamen();
    Coordinador ! entrego(id, examen);
    Profesor ? nota(nota);
}

Process Coordinador {
    cola Fila;
    int idA, cant = 0;
    text examen;
    do (cant < N); Alumno[*] ? entrego(idA, examen) ->
            push(Fila, (idA, examen));
            cant = cant + 1;
       (not empty(Fila)); Profesor ? libre() ->
            pop(Fila, (idA, examen));
            Profesor ! corregir(idA, examen);
    od
}

Process Profesor {
    int idA, resultado;
    text examen;
    for (i = 1..N) {
        Coordinador ! libre();
        Coordinador ? corregir(idA, examen);
        resultado = corregirExamen(examen);
        Alumno[idA] ! nota(resultado);
    }
}


b)

Process Alumno[id: 1..N] {
    text examen;
    int nota;
    examen = resolverExamen();
    Coordinador ! entrego(id, examen);
    Profesor[*] ? nota(nota);
}

Process Coordinador {
    cola Fila;
    int idP, idA, cant = 0;
    text examen;
    do (cant < N); Alumno[*] ? entrego(idA, examen) ->
            push(Fila, (idA, examen));
            cant = cant + 1;
       (not empty(Fila)); Profesor[*] ? libre(idP) ->
            pop(Fila, (idA, examen));
            Profesor[idP] ! corregir(idA, examen);
    od
    for (i = 1..P) {
        Profesor[*] ? libre(idP);
        Profesor[idP] ! corregir(-1, "fin");
    }
}

Process Profesor[id: 1..P] {
    int idA, resultado;
    text examen;
    Coordinador ! libre(id);
    Coordinador ? corregir(idA, examen);
    while (examen <> "fin") {
        resultado = corregirExamen(examen);
        Alumno[idA] ! nota(resultado);
        Coordinador ! libre(id);
        Coordinador ? corregir(idA, examen);
    }
}

c)

Process Alumno[id: 1..N] {
    text examen;
    int nota;
    Coordinador ! llegue();
    Coordinador ? empiecen();
    examen = resolverExamen();
    Coordinador ! entrego(id, examen);
    Profesor[*] ? nota(nota);
}

Process Coordinador {
    cola Fila;
    int idP, idA, cant = 0;
    text examen;
    for (i = 1..N) Alumno[*] ? llegue();
    for (i = 1..N) Alumno[i] ! empiecen();
    do (cant < N); Alumno[*] ? entrego(idA, examen) ->
            push(Fila, (idA, examen));
            cant = cant + 1;
       (not empty(Fila)); Profesor[*] ? libre(idP) ->
            pop(Fila, (idA, examen));
            Profesor[idP] ! corregir(idA, examen);
    od
    for (i = 1..P) {
        Profesor[*] ? libre(idP);
        Profesor[idP] ! corregir(-1, "fin");
    }
}