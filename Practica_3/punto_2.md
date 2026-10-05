2. Existen N procesos que deben leer información de una base de datos administrada por un 
motor que admite un número limitado de consultas simultáneas. 
a) Analice el problema y defina qué procesos, recursos y monitores/sincronizaciones 
serán necesarios/convenientes para resolverlo. 
b) Implemente el acceso a la base de datos por parte de los procesos, sabiendo que el 
motor de la base de datos puede atender a lo sumo 5 consultas de lectura simultáneas. 

a) Procesos: N lectores (activos, cada uno hace su consulta).
Recurso: el motor de la base de datos, con capacidad de 5 consultas simultáneas.
Monitor: uno solo, Acceso, que controla la cantidad de lugares libres. La base de datos en sí no necesita ser un monitor, porque el motor ya atiende las consultas concurrentes internamente.
Sincronización: exclusión mutua para modificar el contador (la da el monitor) y sincronización por condición para bloquear a los lectores cuando no hay lugar.
b)
Monitor acceso
    cond cola;
    int disponibles = 5

    procedure dar_acceso(){
        while (disponibles == 0) wait(cola)
        disponibles += -1
    } 

    procedure salir(){
        disponibles += 1
        signal(cola)
    }

end Monitor 

process pedir_acceso(id: 1..N){
    acceso.dar_acceso()
    delay(realizar_consulta)
    acceso.salir()
}

