Implemente una solución para el siguiente problema. Un sistema debe validar un conjunto de 10000 transacciones que se encuentran disponibles en una estructura de datos. Para ello, el sistema dispone de 7 workers, los cuales trabajan colaborativamente validando de a 1 transacción por vez cada uno.
Cada validación puede tomar un tiempo diferente y para realizarla los workers disponen de la función Validar(t), la cual retorna como resultado un número entero entre 0 al 9. Al finalizar el procesamiento, el último worker en terminar debe informar la cantidad de transacciones por cada resultado de la función de validación. Nota: maximizar la concurrencia.

sem mutexT = 1          # protege el índice de la próxima transacción
sem mutexR = 1          # protege los resultados globales y el contador de workers terminados
int sig = 1
int resultados[0..9] = ([10] 0)
int terminaron = 0

process Worker[id: 1..7] {
    int t, r, i
    int locales[0..9] = ([10] 0)
    bool seguir = true

    while (seguir) {
        P(mutexT)
        if (sig <= 10000) {
            t = sig                     # tomo la próxima transacción
            sig++
        } else
            seguir = false              # no quedan transacciones
        V(mutexT)

        if (seguir) {
            r = Validar(transaccion[t]) # fuera de la sección crítica
            locales[r]++                # cuento en mi arreglo local
        }
    }

    P(mutexR)
    for (i = 0; i <= 9; i++)
        resultados[i] = resultados[i] + locales[i]
    terminaron++
    if (terminaron == 7)                # soy el último en terminar
        Informar(resultados)
    V(mutexR)
}