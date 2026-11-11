1) Resolver el siguiente problema. La página web del Banco Central exhibe las diferentes cotizaciones 
del dólar oficial de 20 bancos del país, tanto para la compra como para la venta. Existe una tarea 
programada que se ocupa de actualizar la página en forma periódica y para ello consulta la cotización 
de cada uno de los 20 bancos. Cada banco dispone de una API, cuya única función es procesar las 
solicitudes de aplicaciones externas. La tarea programada consulta de a una API por vez, esperando 
a lo sumo 5 segundos por su respuesta. Si pasado ese tiempo no respondió, entonces se mostrará 
vacía la información de ese banco. 

procedure bancoCentral is

    task type banco is
        entry consulta (compra: out integer; venta: out integer);
    end banco;

    arrBanco: array (1..20) of banco;

    task tarea;

    task body banco is
    begin
        loop
            accept consulta (compra: out integer; venta: out integer) do
                compra := api.procesarCompra();
                venta  := api.procesarVenta();
            end consulta;
        end loop;
    end banco;

    task body tarea is
        compra: array (1..20) of integer;
        venta:  array (1..20) of integer;
        comp, vent: integer;
    begin
        loop
            for i in 1..20 loop
                select
                    arrBanco(i).consulta(comp, vent);
                    compra(i) := comp;
                    venta(i)  := vent;
                or delay 5.0;
                    compra(i) := -1;   -- vacío
                    venta(i)  := -1;
                end select;
            end loop;
            -- actualizar la página con compra y venta
            delay 60.0;   -- período entre actualizaciones
        end loop;
    end tarea;

begin
    null;
end bancoCentral;
