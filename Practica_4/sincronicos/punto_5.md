5. En un estadio de fútbol hay una máquina expendedora de gaseosas que debe ser usada por 
E Espectadores de acuerdo con el orden de llegada. Cuando el espectador accede a la 
máquina en su turno, usa la máquina y luego se retira para dejar al siguiente. Nota: cada 
espectador usa la máquina sólo una vez. 


Process Espectador[id: 1..E] {
    Coordinador ! llegue(id);
    Maquina ? pasar();
    # usa la máquina
    Maquina ! mefui();
}

Process Coordinador {
    cola Fila;
    int idE, cant = 0;
    do (cant < E); Espectador[*] ? llegue(idE) ->
            push(Fila, idE);
            cant = cant + 1;
       (not empty(Fila)); Maquina ? libre() ->
            pop(Fila, idE);
            Maquina ! dejarPasar(idE);
    od
}

Process Maquina {
    int idA;
    for (i = 1..E) {
        Coordinador ! libre();
        Coordinador ? dejarPasar(idA);
        Espectador[idA] ! pasar();
        Espectador[idA] ? mefui();
    }
}