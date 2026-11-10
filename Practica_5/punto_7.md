7. Se debe calcular el valor promedio de un vector de 1 millón de números enteros que se 
encuentra distribuido entre 10 procesos Worker (es decir, cada Worker tiene un vector de 
100 mil números). Para ello, existe un Coordinador que determina el momento en que se 
debe realizar el cálculo de este promedio y que, además, se queda con el resultado. Nota: 
maximizar la concurrencia; este cálculo se hace una sola vez. 

procedure Promedio is

   TASK TYPE Worker IS
      ENTRY Comenzar;
   END Worker;

   arrWorkers: ARRAY (1..10) OF Worker;

   TASK Coordinador IS
      ENTRY MiValor (Valor: IN long_integer);
   END Coordinador;

   TASK BODY Worker IS
      vec: ARRAY (1..100_000) OF integer := InicializarVector;
      miTotal: long_integer := 0;
   BEGIN
      ACCEPT Comenzar;                      -- espera la orden del Coordinador
      FOR i IN 1..100_000 LOOP
         miTotal := miTotal + long_integer(vec(i));
      END LOOP;
      Coordinador.MiValor(miTotal);
   END Worker;

   TASK BODY Coordinador IS
      total: long_integer := 0;
      promedio: float;
   BEGIN
      FOR i IN 1..10 LOOP                   -- él decide el momento de empezar
         arrWorkers(i).Comenzar;
      END LOOP;

      FOR i IN 1..10 LOOP                   -- recibe parciales en orden de llegada
         ACCEPT MiValor (Valor: IN long_integer) DO
            total := total + Valor;
         END MiValor;
      END LOOP;

      promedio := float(total) / 1_000_000.0;
   END Coordinador;

BEGIN
   null;
END Promedio;