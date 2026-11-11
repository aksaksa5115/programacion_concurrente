2) Resolver el siguiente problema. En un negocio de cobros digitales hay P personas que deben pasar 
por la única caja de cobros para realizar el pago de sus boletas. Las personas son atendidas de acuerdo 
con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que 
pagan más. Adicionalmente, las personas ancianas tienen prioridad sobre los dos casos anteriores. 
Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los 
recibos de pago. 

procedure negocio IS

    TASK TYPE personas

    TASK cajero IS
        ENTRY caja(boletas: IN text; pago: IN double: vuelto: OUT double; recibos: OUT text)
        ENTRY prioridad(boletas: IN text; pago: IN double: vuelto: OUT double; recibos: OUT text)
        ENTRY prioridadAlta(boletas: IN text; pago: IN double: vuelto: OUT double; recibos: OUT text)

    TASK BODY persona iS
        anciano: bool
        cant_boletas: integer
    BEGIN
        estadoPersona(anciano, cant_boletas)
        IF (cant_boletas < 5) DO
            cajero.prioridad()
        ELSE IF (anciano) DO
            cajero.prioridadAlta()
        ELSE DO
            cajero.caja()
        END DO
    END persona

    TASK BODY cajero IS

    BEGIN
        LOOP
            SELECT
                WHEN ((prioridadAlta'count = 0) AND (prioridad'count = 0) AND (caja'count > 0))
                    ACCEPT caja()
                END DO
            OR
                WHEN (prioridadAlta'count > 0) DO
                    ACCEPT prioridadAlta()
                END DO
            OR 
                WHEN ((prioridadAlta'count = 0) AND (prioridad'count > 0)) DO
                    ACCEPT prioridad()
                END DO
            END SELECT
        END LOOP
    END cajero