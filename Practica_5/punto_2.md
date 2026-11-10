2. Se quiere modelar el funcionamiento de un banco, al cual llegan clientes que deben realizar 
un pago y retirar un comprobante. Existe un único empleado en el banco, el cual atiende de 
acuerdo con el orden de llegada.  
a) Implemente una solución donde los clientes llegan y se retiran sólo después de haber sido 
atendidos. 
b) Implemente una solución donde los clientes se retiran si esperan más de 10 minutos para 
realizar el pago. 
c) 
Implemente una solución donde los clientes se retiran si no son atendidos 
inmediatamente. 
d) Implemente una solución donde los clientes esperan a lo sumo 10 minutos para ser 
atendidos. Si pasado ese lapso no fueron atendidos, entonces solicitan atención una vez más 
y se retiran si no son atendidos inmediatamente. 

a)

procedure Banco1 is

   TASK Empleado IS
      ENTRY Pago (Monto: IN real; Comprobante: OUT texto);
   END Empleado;

   TASK TYPE Cliente;
   arrClientes: ARRAY (1..C) OF Cliente;

   TASK BODY Empleado IS
   BEGIN
      LOOP
         ACCEPT Pago (Monto: IN real; Comprobante: OUT texto) DO
            Comprobante := generarComprobante(Monto);
         END Pago;
      END LOOP;
   END Empleado;

   TASK BODY Cliente IS
      comp: texto;
   BEGIN
      Empleado.Pago(100.0, comp);
   END Cliente;

BEGIN
   null;
END Banco1;


b)
procedure Banco1 is

   TASK Empleado IS
      ENTRY Pago (Monto: IN real; Comprobante: OUT texto);
   END Empleado;

   TASK TYPE Cliente;
   arrClientes: ARRAY (1..C) OF Cliente;

   TASK BODY Empleado IS
   BEGIN
      LOOP
         ACCEPT Pago (Monto: IN real; Comprobante: OUT texto) DO
            Comprobante := generarComprobante(Monto);
         END Pago;
      END LOOP;
   END Empleado;

   TASK BODY Cliente IS
      comp: texto;
   BEGIN
      SELECT
        Empleado.Pago(100.0, comp);
      OR DELAY 600
        NULL
      END SELECT
   END Cliente;

BEGIN
   null;
END Banco1;

c)

procedure Banco1 is

   TASK Empleado IS
      ENTRY Pago (Monto: IN real; Comprobante: OUT texto);
   END Empleado;

   TASK TYPE Cliente;
   arrClientes: ARRAY (1..C) OF Cliente;

   TASK BODY Empleado IS
   BEGIN
      LOOP
         ACCEPT Pago (Monto: IN real; Comprobante: OUT texto) DO
            Comprobante := generarComprobante(Monto);
         END Pago;
      END LOOP;
   END Empleado;

   TASK BODY Cliente IS
      comp: texto;
   BEGIN
      SELECT
        Empleado.Pago(100.0, comp);
      ELSE
        NULL
      END SELECT
   END Cliente;

BEGIN
   null;
END Banco1;

d)

procedure Banco1 is

   TASK Empleado IS
      ENTRY Pago (Monto: IN real; Comprobante: OUT texto);
   END Empleado;

   TASK TYPE Cliente;
   arrClientes: ARRAY (1..C) OF Cliente;

   TASK BODY Empleado IS
   BEGIN
      LOOP
         ACCEPT Pago (Monto: IN real; Comprobante: OUT texto) DO
            Comprobante := generarComprobante(Monto);
         END Pago;
      END LOOP;
   END Empleado;

   TASK BODY Cliente IS
      comp: texto;
   BEGIN
      SELECT
         Empleado.Pago(100.0, comp);
         atendido := true;
      OR DELAY 600.0;
         null;
      END SELECT;

      IF (NOT atendido) THEN
         SELECT
            Empleado.Pago(100.0, comp);
         ELSE
            null;               
         END SELECT;
      END IF;
   END Cliente;
   END Cliente;

BEGIN
   null;
END Banco1;