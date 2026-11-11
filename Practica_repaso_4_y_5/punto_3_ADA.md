3) Resolver el siguiente problema. La oficina central de una empresa de venta de indumentaria debe 
calcular cuántas veces fue vendido cada uno de los artículos de su catálogo. La empresa se compone 
de 100 sucursales y cada una de ellas maneja su propia base de datos de ventas. La oficina central 
cuenta con una herramienta que funciona de la siguiente manera: ante la consulta realizada para un 
artículo determinado, la herramienta envía el identificador del artículo a las sucursales, para que cada 
una calcule cuántas veces fue vendido en ella. Al final del procesamiento, la herramienta debe 
conocer cuántas veces fue vendido en total, considerando todas las sucursales. Cuando ha terminado 
de procesar un artículo comienza con el siguiente (suponga que la herramienta tiene una función 
generarArtículo() que retorna el siguiente ID a consultar). Nota: maximizar la concurrencia. Existe 
una función ObtenerVentas(ID) que retorna la cantidad de veces que fue vendido el artículo con 
identificador ID en la base de la sucursal que la llama. 

procedure empresa IS

    TASK TYPE sucursal IS
        ENTRY ropa(id: IN integer)
    END sucursal

    arrSucursal: ARRAY(1..100) OF sucursal

    TASK oficina IS
        ENTRY venta(ventas: IN integer)
    END oficina

    TASK BODY oficina IS
        idR: integer
        cant_ventas: integer
    BEGIN
        LOOP
            cant_ventas:= 0
            idR:= generarArticulo()

            FOR i IN 1..100 DO
                arrSucursal(i).ropa(idR)
            END LOOP

            FOR i IN 1..100 DO
                ACCEPT venta(ventas: IN integer) DO
                    cant_ventas:= cant_ventas + ventas
                END venta
            END LOOP
            guardarVentas(idR, cant_ventas)
        END LOOP
    END oficina

    TASK BODY sucursal IS
        idR: integer
        cant_vendida: integer
    BEGIN
        LOOP
            ACCEPT ropa(id: IN integer) DO
                idR:= id
            END ropa
            cant_vendida:= ObtenerVentas(idR)
            oficina.venta(cant_vendida)
        END LOOP
    END sucursal