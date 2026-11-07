4. En una exposición aeronáutica hay un simulador de vuelo (que debe ser usado con 
exclusión mutua) y un empleado encargado de administrar su uso. Hay P personas que 
esperan a que el empleado las deje acceder al simulador de a una por vez, la usan por un 
rato y luego se retiran. 
a) Implemente una solución donde el empleado sólo se ocupa de garantizar la exclusión 
mutua (sin importar el orden). 
b) Modifique la solución anterior para que el empleado los deje acceder según el orden de 
su identificador (hasta que la persona i no lo haya usado, la persona i+1 debe esperar). 
c) Modifique la solución a) para que el empleado considere el orden de llegada para dar 
acceso al simulador. 
Nota: cada persona usa sólo una vez el simulador.  


a) 

process persona(id: 1..P){
    bool paso = false
    empleado ! llegue(id)
    empleado ? pasar(paso)
    // usa simulador
    empleado ! mefui(true)
}

process empleado(){
    int idA
    bool sefue
    for (int i=1; i<=P; i++){
        persona[*] ? llegue(idA)
        persona[idA] ! pasar(true)
        persona[idA] ? mefui(sefue)
    }
}

b)

process persona(id: 1..P){
    bool paso = false
    empleado ? pasar(paso)
    // usa simulador
    empleado ! mefui(true)
}

process empleado(){
    int idA
    bool sefue
    for (int i=1; i<=P; i++){
        persona[i] ! pasar(true)
        persona[i] ? mefui(sefue)
    }
}

c)

Process Persona[id: 1..P] {
    Coordinador ! llegue(id);
    Empleado ? pasar();
    # usa el simulador
    Empleado ! mefui();
}

Process Coordinador {
    cola Fila;
    int idP, cant = 0;
    do (cant < P); Persona[*] ? llegue(idP) ->
            push(Fila, idP);
            cant = cant + 1;
       (not empty(Fila)); Empleado ? libre() ->
            pop(Fila, idP);
            Empleado ! dejarPasar(idP);
    od
}

Process Empleado {
    int idA;
    for (i = 1..P) {
        Coordinador ! libre();
        Coordinador ? dejarPasar(idA);
        Persona[idA] ! pasar();
        Persona[idA] ? mefui();
    }
}