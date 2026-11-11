9. Una empresa de limpieza se encarga de recolectar residuos en una ciudad por medio de 3 
camiones. Hay P personas que hacen reclamos continuamente hasta que uno de los camiones pase por su casa. 
Cada persona hace un reclamo y espera a lo sumo 15 minutos 
a que llegue un camión; si no pasa, vuelve a hacer el reclamo y a esperar a lo sumo 15 
minutos a que llegue un camión; y así sucesivamente hasta que el camión llegue y recolecte 
los residuos. Sólo cuando un camión llega, es cuando deja de hacer reclamos y se retira. 
Cuando un camión está libre la empresa lo envía a la casa de la persona que más reclamos 
ha hecho sin ser atendido. Nota: maximizar la concurrencia.

procedure Empresa is

   TASK Coordinador IS
      ENTRY Reclamo (IdP: IN integer);
      ENTRY Libre (Casa: OUT integer);     -- el camión pide su próxima casa
   END Coordinador;

   TASK TYPE Camion;

   TASK TYPE Persona IS
      ENTRY Ident (IdP: IN integer);
      ENTRY Llegue;
   END Persona;

   arrCamiones: ARRAY (1..3) OF Camion;
   arrPersonas: ARRAY (1..P) OF Persona;

   TASK BODY Coordinador IS
      reclamos: ARRAY (1..P) OF integer := (OTHERS => 0);
      hayPendientes: integer := 0;         -- cantidad de personas con reclamos sin atender
   BEGIN
      LOOP
         SELECT
            ACCEPT Reclamo (IdP: IN integer) DO
               IF (reclamos(IdP) = 0) THEN
                  hayPendientes := hayPendientes + 1;
               END IF;
               reclamos(IdP) := reclamos(IdP) + 1;
            END Reclamo;
         OR
            WHEN (hayPendientes > 0) =>
            ACCEPT Libre (Casa: OUT integer) DO
               Casa := casaConMasReclamos(reclamos);
               reclamos(Casa) := 0;        -- ya no tiene reclamos pendientes
               hayPendientes := hayPendientes - 1;
            END Libre;
         END SELECT;
      END LOOP;
   END Coordinador;

   TASK BODY Camion IS
      casa: integer;
   BEGIN
      LOOP
         Coordinador.Libre(casa);          -- espera hasta que haya a quién atender
         Persona_llegada(casa);            
      END LOOP;
   END Camion;

   TASK BODY Persona IS
      id: integer;
      atendido: boolean := false;
   BEGIN
      ACCEPT Ident (IdP: IN integer) DO
         id := IdP;
      END Ident;

      WHILE (NOT atendido) LOOP
         Coordinador.Reclamo(id);          -- hace el reclamo
         SELECT
            ACCEPT Llegue;
            atendido := true;
         OR DELAY 900.0;
            null;                          -- pasaron 15 min: reclama de nuevo
         END SELECT;
      END LOOP;
   END Persona;

BEGIN
   FOR i IN 1..P LOOP
      arrPersonas(i).Ident(i);
   END LOOP;
END Empresa;