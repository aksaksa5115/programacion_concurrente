2) Resolver el siguiente problema con PMS. En la estación de trenes hay una terminal de SUBE que 
debe ser usada por P personas de acuerdo con el orden de llegada. Cuando la persona accede a la 
terminal, la usa y luego se retira para dejar al siguiente. Nota: cada Persona usa sólo una vez la 
terminal. 


Process Persona[id: 1..P] {
    Coordinador ! llegue(id);
    Terminal ? pasar();
    # usa la terminal
    Terminal ! mefui();
}

Process Coordinador {
    cola fila;
    int idP, cant = 0;
    do (cant < P); Persona[*] ? llegue(idP) ->
            push(fila, idP);
            cant = cant + 1;
       (not empty(fila)); Terminal ? libre() ->
            pop(fila, idP);
            Terminal ! siguiente(idP);
    od
}

Process Terminal {
    int idP;
    for (i = 1..P) {
        Coordinador ! libre();
        Coordinador ? siguiente(idP);
        Persona[idP] ! pasar();
        Persona[idP] ? mefui();
    }
}