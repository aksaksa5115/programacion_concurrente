En un sistema para acreditar carreras universitarias, hay UN Servidor que atiende pedidos 
de U Usuarios de a uno a la vez y de acuerdo con el orden en que se hacen los pedidos. 
Cada usuario trabaja en el documento a presentar, y luego lo envía al servidor; espera la 
respuesta de este que le indica si está todo bien o hay algún error. Mientras haya algún error, 
vuelve a trabajar con el documento y a enviarlo al servidor. Cuando el servidor le responde 
que está todo bien, el usuario se retira. Cuando un usuario envía un pedido espera a lo sumo 
2 minutos a que sea recibido por el servidor, pasado ese tiempo espera un minuto y vuelve a 
intentarlo (usando el mismo documento).

procedure Carreras is

   TASK Servidor IS
      ENTRY Pedido (Documento: IN texto; Respuesta: OUT texto);
   END Servidor;

   TASK TYPE Usuario;
   arrUsuarios: ARRAY (1..U) OF Usuario;

   TASK BODY Servidor IS
   BEGIN
      LOOP
         ACCEPT Pedido (Documento: IN texto; Respuesta: OUT texto) DO
            Respuesta := revisarDocumento(Documento);   -- "ok" o descripción del error
         END Pedido;
      END LOOP;
   END Servidor;

   TASK BODY Usuario IS
      documento: texto;
      respuesta: texto;
      aprobado:  boolean := false;
      recibido:  boolean;
   BEGIN
      WHILE (NOT aprobado) LOOP
         documento := trabajarDocumento(respuesta);     -- corrige según los errores
         recibido := false;
         WHILE (NOT recibido) LOOP
            SELECT
               Servidor.Pedido(documento, respuesta);
               recibido := true;
            OR DELAY 120.0;
               delay 60.0;                              -- espera 1 min y reintenta
            END SELECT;
         END LOOP;
         IF (respuesta = "ok") THEN
            aprobado := true;
         END IF;
      END LOOP;
   END Usuario;

BEGIN
   null;
END Carreras;