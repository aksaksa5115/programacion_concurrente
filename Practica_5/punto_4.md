4. En una clínica existe un médico de guardia que recibe continuamente peticiones de 
atención de las E enfermeras que trabajan en su piso y de las P personas que llegan a la 
clínica ser atendidos.  
Cuando una persona necesita que la atiendan espera a lo sumo 5 minutos a que el médico lo 
haga, si pasado ese tiempo no lo hace, espera 10 minutos y vuelve a requerir la atención del 
médico. Si no es atendida tres veces, se enoja y se retira de la clínica. 
Cuando una enfermera requiere la atención del médico, si este no lo atiende inmediatamente 
le hace una nota y se la deja en el consultorio para que esta resuelva su pedido en el 
momento que pueda (el pedido puede ser que el médico le firme algún papel). Cuando la 
petición ha sido recibida por el médico o la nota ha sido dejada en el escritorio, continúa 
trabajando y haciendo más peticiones.
El médico atiende los pedidos dándole prioridad a los enfermos que llegan para ser atendidos. 
Cuando atiende un pedido, recibe la solicitud y la procesa durante un cierto tiempo. Cuando 
está libre aprovecha a procesar las notas dejadas por las enfermeras.

process hospital IS

    TASK guardia IS
        ENTRY atender;
        ENTRY peticion(nota: IN text)
    END guardia

    TASK buzon IS
        ENTRY recibir(nota: IN text)
        ENTRY retirar(nota: OUT text)
    END buzon

    TASK TYPE enfermera IS

    arrEnfermera = ARRAY(1..E) OF enfermera

    TASK TYPE persona IS

    arrPersona = ARRAY(1..P) OF persona



    TASK BODY guardia IS
        nota = texto
    BEGIN
        LOOP
            SELECT
                WHEN (atender'count == 0) => ACCEPT peticion(nota: IN text) DO
                    resolver(nota)
                END peticion
            OR 
                ACCEPT atender() DO
                    arreglarPersona()
                END atender
            ELSE WHEN (atender'count == 0) =>
                SELECT
                    buzon.retirar(nota)
                    resolver(nota)
                ELSE
                    NULL
                END SELECT
            END SELECT
        END LOOP
    END guardia

    TASK BODY buzon IS
        notas: colaDeTextos
        N: text
    BEGIN
        LOOP
            SELECT
                ACCEPT recibir(nota: IN text) DO
                    agregar(notas, nota)
                END recibir
            OR
                WHEN (NOT vacia(notas)) =>
                ACCEPT retirar(notas: OUT text) DO
                    N := sacar(notas)
                END retirar
            END SELECT
        END LOOP
    END buzon


    TASK BODY enfermera IS
        nota = texto
    BEGIN
        SELECT
            guardia.peticion(nota)
        ELSE
            buzon.recibir(nota)
        END SELECT
    END enfermera


    TASK BODY persona IS
        cont: integer
        atendido: bool
    BEGIN
        cont := 0
        atendido = false
        WHILE (cont < 3) and (not atendido) LOOP 
            cont := cont + 1
            SELECT
                guardia.atender()
                atendido := TRUE
            OR DELAY(300)
            END SELECT
            DELAY(600)
            IF (cont == 3) DO
                BREAK
            FI
        END LOOP
    END persona