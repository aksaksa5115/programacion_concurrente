En una empresa de software hay N personas que prueban un nuevo producto 
para encontrar errores, cuando encuentran uno generan un reporte para que un 
empleado corrija el error (las personas no deben reciben ninguna respuesta). El 
empleado toma los reportes de acuerdo al orden de llegada, los evalúan y hace 
las correcciones necesarias.

tenemos N personas osea N procesos persona

tenemos 1 empleado osea 1 proceso empleado

un chan para los errores

chan errores(error)

process persona(id :0..N-1){
    // prueba producto
    while(true){
        // encuentro error
        // hago informe
        send.errores(error)
    }
}

process empleado(id: 1){
    while(true){
        informe = receive.errores()
        // evalua informe
        // realiza correcciones
    }
}