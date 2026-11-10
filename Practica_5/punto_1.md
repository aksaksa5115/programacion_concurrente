1. Se requiere modelar un puente de un único sentido que soporta hasta 5 unidades de peso. 
El peso de los vehículos depende del tipo: cada auto pesa 1 unidad, cada camioneta pesa 2 
unidades y cada camión 3 unidades. Suponga que hay una cantidad innumerable de 
vehículos (A autos, B camionetas y C camiones). Analice el problema y defina qué tareas, 
recursos y sincronizaciones serán necesarios/convenientes para resolverlo. 
a. Realice la solución suponiendo que todos los vehículos tienen la misma prioridad. 
b. Modifique la solución para que tengan mayor prioridad los camiones que el resto de los 
vehículos. 

a)
procedure puente1 is

    TASK PUENTE IS
        ENTRY entraAuto
        ENTRY entraCamioneta
        ENTRY entraCamion
        ENTRY salir (P: IN integer);
    END PUENTE;

    TASK TYPE AUTO

    TASK TYPE CAMIONETA

    TASK TYPE CAMION

    arrAutos: ARRAY (1..A) OF AUTO
    arrCamionetas: ARRAY (1..B) OF CAMIONETA
    arrCamiones: ARRAY (1..C) OF CAMION

    TASK BODY PUENTE IS:
        peso: integer := 0
    BEGIN
        LOOP
            SELECT
                WHEN (peso + 1 <= 5) => ACCEPT entraAuto
                    peso := peso + 1
            OR
                WHEN (peso + 2 <= 5) => ACCEPT entraCamioneta
                    peso := peso + 2
            OR
                WHEN (peso + 3 <= 5) => ACCEPT entraCamion
                    peso := peso + 3
            OR
                ACCEPT salir (P: IN integer) DO
                    peso := peso - P
                END salir
            END SELECT
        END LOOP
    END PUENTE

    TASK BODY AUTO IS
    BEGIN
        PUENTE.entrarAuto
        DELAY("PASA AUTO")
        PUENTE.salir(1)
    END AUTO;

    TASK BODY CAMIONETA IS
    BEGIN
        PUENTE.entrarCamioneta
        DELAY("PASA CAMIONETA")
        PUENTE.salir(2)
    END CAMIONETA;

    TASK BODY CAMION IS
    BEGIN
        PUENTE.entrarCamion
        DELAY("PASA CAMION")
        PUENTE.salir(3)
    END CAMION

b)
procedure puente1 is

    TASK PUENTE IS
        ENTRY entraAuto
        ENTRY entraCamioneta
        ENTRY entraCamion
        ENTRY salir (P: IN integer);
    END PUENTE;

    TASK TYPE AUTO

    TASK TYPE CAMIONETA

    TASK TYPE CAMION

    arrAutos: ARRAY (1..A) OF AUTO
    arrCamionetas: ARRAY (1..B) OF CAMIONETA
    arrCamiones: ARRAY (1..C) OF CAMION

    TASK BODY PUENTE IS:
        peso: integer := 0
    BEGIN
        LOOP
            SELECT
                WHEN (peso + 3 <= 5) => ACCEPT entraCamion
                    peso := peso + 3
            OR
                WHEN (peso + 2 <= 5 AND entraCamion'count = 0) => ACCEPT entraCamioneta
                    peso := peso + 2
            OR
                WHEN (peso + 1 <= 5 AND entraCamion'count = 0) => ACCEPT entraAuto
                    peso := peso + 1
            OR
                ACCEPT salir (P: IN integer) DO
                    peso := peso - P
                END salir
            END SELECT
        END LOOP
    END PUENTE

    TASK BODY AUTO IS
    BEGIN
        PUENTE.entrarAuto
        DELAY("PASA AUTO")
        PUENTE.salir(1)
    END AUTO;

    TASK BODY CAMIONETA IS
    BEGIN
        PUENTE.entrarCamioneta
        DELAY("PASA CAMIONETA")
        PUENTE.salir(2)
    END CAMIONETA;

    TASK BODY CAMION IS
    BEGIN
        PUENTE.entrarCamion
        DELAY("PASA CAMION")
        PUENTE.salir(3)
    END CAMION