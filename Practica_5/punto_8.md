8. Hay un sistema de reconocimiento de huellas dactilares de la policía que tiene 8 Servidores 
para realizar el reconocimiento, cada uno de ellos trabajando con una Base de Datos propia; 
a su vez hay un Especialista que utiliza indefinidamente. El sistema funciona de la siguiente 
manera: el Especialista toma una imagen de una huella (TEST) y se la envía a los servidores 
para que cada uno de ellos le devuelva el código y el valor de similitud de la huella que más 
se asemeja a TEST en su BD; al final del procesamiento, el especialista debe conocer el 
código de la huella con mayor valor de similitud entre las devueltas por los 8 servidores. 
Cuando ha terminado de procesar una huella comienza nuevamente todo el ciclo. Nota: 
suponga que existe una función Buscar(test, código, valor) que utiliza cada Servidor donde 
recibe como parámetro de entrada la huella test, y devuelve como parámetros de salida el 
código y el valor de similitud de la huella más parecida a test en la BD correspondiente. 
Maximizar la concurrencia y no generar demora innecesaria.

procedure Sistema is

   TASK TYPE Servidor IS
      ENTRY Escanear (Huella: IN texto);
   END Servidor;

   arrServidores: ARRAY (1..8) OF Servidor;

   TASK Especialista IS
      ENTRY Resultado (Codigo: IN integer; Similitud: IN real);
   END Especialista;

   TASK BODY Servidor IS
      test: texto;
      codigo: integer;
      similitud: real;
   BEGIN
      LOOP
         ACCEPT Escanear (Huella: IN texto) DO
            test := Huella;                       -- copia el parámetro
         END Escanear;

         Buscar(test, codigo, similitud);
         Especialista.Resultado(codigo, similitud);
      END LOOP;
   END Servidor;

   TASK BODY Especialista IS
      muestra: texto;
      mejorCodigo: integer;
      mejorSimilitud: real;
   BEGIN
      LOOP
         muestra := tomarHuella();
         mejorSimilitud := 0.0;                   -- se reinicia en cada ciclo

         FOR i IN 1..8 LOOP
            arrServidores(i).Escanear(muestra);   -- se aceptan enseguida
         END LOOP;

         FOR i IN 1..8 LOOP                       -- resultados en orden de llegada
            ACCEPT Resultado (Codigo: IN integer; Similitud: IN real) DO
               IF (Similitud > mejorSimilitud) THEN
                  mejorSimilitud := Similitud;
                  mejorCodigo := Codigo;
               END IF;
            END Resultado;
         END LOOP;

         -- aquí el especialista conoce mejorCodigo
      END LOOP;
   END Especialista;

BEGIN
   null;
END Sistema;
        