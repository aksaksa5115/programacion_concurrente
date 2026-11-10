6. En una playa hay 5 equipos de 4 personas cada uno (en total son 20 personas donde cada 
una conoce previamente a que equipo pertenece). Cuando las personas van llegando 
esperan con los de su equipo hasta que el mismo esté completo (hayan llegado los 4 
integrantes), a partir de ese momento el equipo comienza a jugar. El juego consiste en que 
cada integrante del grupo junta 15 monedas de a una en una playa (las monedas pueden ser 
de 1, 2 o 5 pesos) y se suman los montos de las 60 monedas conseguidas en el grupo. Al 
finalizar cada persona debe conocer el grupo que más dinero junto. Nota: maximizar la 
concurrencia. Suponga que para simular la búsqueda de una moneda por parte de una 
persona existe una función Moneda() que retorna el valor de la moneda encontrada.

procedure playa IS

    TASK coordinador IS
        ENTRY totalEquipo(monto: IN integer; equipo: IN integer)
        ENTRY ganador(gano: OUT integer)
    END coordinador
    
    TASK TYPE equipo IS
        ENTRY IDent(id: IN integer)
        ENTRY llegue;
        ENTRY EMPEZAR;
        ENTRY miTotal(monto: IN integer)
    END equipo

    arrEquipo: ARRAY(1..5) OF equipo

    TASK TYPE persona IS
        ENTRY IDent (equipo: IN integer);
    END persona

    arrPersona: ARRAY(1..20) OF persona


    TASK BODY persona IS
        equipo: integer
        suma: integer
        ganador: integer
    BEGIN
        suma:= 0
        ACCEPT IDent(eq: IN integer) DO
            equipo := eq
        END IDent

        arrEquipos(equipo).llegue
        arrEquipos(equipo).comenzar

        FOR i IN 1..15 LOOP
            suma := suma + Moneda()
        END LOOP

        arrEquipos(equipo).miTotal(suma)
        coordinador.ganador(ganador)
    END persona

    TASK BODY equipo IS
        id: integer
        total: integer
        equipo_id: integer
    BEGIN
        total:= 0
        ACCEPT IDent(ID: IN integer) DO
            equipo_id: ID
        END IDent

        FOR i IN 1..4 LOOP
            ACCEPT llegue
        END LOOP

        FOR i IN 1..4 LOOP
            ACCEPT comenzar
        END LOOP

        FOR i IN 1..4 LOOP
            ACCEPT MiTotal(Monto: IN integer) DO
                total:= total + Monto
            END MiTotal
        END LOOP

        coordinador.totalEquipo(total, equipo_id)
    END equipo

    TASK BODY coordinador IS
        maximo: integer
        ganador: integer
    BEGIN
        maximo:= 0
        FOR i IN 1..5 LOOP
            ACCEPT totalEquipo(monto: IN integer; equipo: IN integer) DO
                IF (monto > maximo) THEN
                    maximo:= monto
                    ganador:= eq
                END IF;
            END totalEquipo
        END LOOP

        FOR i IN 1..20 LOOP
            ACCEPT ganador(g: OUT integer) DO
                g:= ganador
            END ganador
        END LOOP
    END coordinador

    
    