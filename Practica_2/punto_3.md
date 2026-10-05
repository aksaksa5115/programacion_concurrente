3. Un sistema operativo mantiene 5 instancias de un recurso almacenadas en una cola. 
Además, existen P procesos que necesitan usar una instancia del recurso. Para eso, deben 
sacar la instancia de la cola antes de usarla. Una vez usada, la instancia debe ser encolada 
nuevamente para su reúso.

sem instancias = 5
sem mutex = 1

process usar_recurso(id: 1..P){
    P(instancias)

        P(mutex)
            desencolar(instancia)
        V(mutex)

        delay(usa_instancia)

        P(mutex)
            encolar(instancia)
        V(mutex)

    V(instancias)
}