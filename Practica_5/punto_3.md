3. Se dispone de un sistema compuesto por 1 central y 2 procesos periféricos, que se 
comunican continuamente. Se requiere modelar su funcionamiento considerando las 
siguientes condiciones: - - 
La central siempre comienza su ejecución tomando una señal del proceso 1; luego 
toma aleatoriamente señales de cualquiera de los dos indefinidamente. Al recibir una 
señal de proceso 2, recibe señales del mismo proceso durante 3 minutos. 
Los procesos periféricos envían señales continuamente a la central. La señal del 
proceso 1 será considerada vieja (se deshecha) si en 2 minutos no fue recibida. Si la 
señal del proceso 2 no puede ser recibida inmediatamente, entonces espera 1 minuto y 
vuelve a mandarla (no se deshecha). 


procedure SistemaCompuesto is

   TASK Central IS
      ENTRY Senal1;
      ENTRY Senal2;
      ENTRY Fin3Minutos;
   END Central;

   TASK Proceso1;
   TASK Proceso2;

   TASK Timer IS
      ENTRY Iniciar;
   END Timer;

   TASK BODY Timer IS
   BEGIN
      LOOP
         ACCEPT Iniciar;
         delay 180.0;
         Central.Fin3Minutos;
      END LOOP;
   END Timer;

   TASK BODY Central IS
   BEGIN
      ACCEPT Senal1;                     -- sin cuerpo: no bloquea a Proceso1
      LOOP
         SELECT
            ACCEPT Senal1;
         OR
            ACCEPT Senal2;
            Timer.Iniciar;               -- arranca los 3 minutos
            LOOP
               SELECT
                  ACCEPT Senal2;
               OR
                  ACCEPT Fin3Minutos;
                  EXIT;                  -- terminaron los 3 minutos
               END SELECT;
            END LOOP;
         END SELECT;
      END LOOP;
   END Central;

   TASK BODY Proceso1 IS
   BEGIN
      LOOP
         SELECT
            Central.Senal1;
         OR DELAY 120.0;
            null;                        -- señal vieja, se descarta
         END SELECT;
      END LOOP;
   END Proceso1;

   TASK BODY Proceso2 IS
   BEGIN
      LOOP
         SELECT
            Central.Senal2;
         ELSE
            delay 60.0;                  -- espera 1 minuto y reintenta
         END SELECT;
      END LOOP;
   END Proceso2;

BEGIN
   null;
END SistemaCompuesto;

