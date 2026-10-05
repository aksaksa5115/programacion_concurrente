1. Existen N personas que deben ser chequeadas por un detector de metales antes de poder 
ingresar al avión.  
a. Analice el problema y defina qué procesos, recursos y semáforos/sincronizaciones 
serán necesarios/convenientes para resolverlo. 
b. Implemente una solución que modele el acceso de las personas a un detector (es decir, 
si el detector está libre la persona lo puede utilizar; en caso contrario, debe esperar).  
c. Modifique su solución para el caso que haya tres detectores.  
d. Modifique la solución anterior para el caso en que cada persona pueda pasar más de 
una vez, siendo aleatoria esa cantidad de veces. 


a. 1 semaforo para indicar la cantidad de detectores disponibles
    una cantidad N > 0 de personas que simbolizan los procesos que usan los detectores
    la operacion P decrementa el semaforo y la operacion V lo incrementa
    si el sem > 0 entonces hay un detector disponible asique una persona (proceso)
    puede usarlo

b. y c.
sem detector = 1; // 3 para el caso de 3 semaforos, no cambia mas nada

process persona(id: 0..N-1){
    P(detector)
        delay()
    V(detector)
}

d.
process persona (id: 0..N-1){
    for i: 1 to random(N){
        P(detector)
            delay()
        V(detector)
    }
}