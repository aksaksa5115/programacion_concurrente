Un sistema de control cuenta con 4 procesos que realizan chequeos en forma 
colaborativa. Para ello, reciben el historial de fallos del día anterior (por simplicidad, de 
tamaño N). De cada fallo, se conoce su número de identificación (ID) y su nivel de 
gravedad (0=bajo, 1=intermedio, 2=alto, 3=crítico). Resuelva considerando las siguientes 
situaciones:
a) Se debe imprimir en pantalla los ID de todos los errores críticos (no importa el 
orden). 
b) Se debe calcular la cantidad de fallos por nivel de gravedad, debiendo quedar los 
resultados en un vector global. 
c) Ídem b) pero cada proceso debe ocuparse de contar los fallos de un nivel de 
gravedad determinado. 

a)
sem vector_libre = 1
vector cantidad_fallos = [0, 0, 0, 0] of int
vector fallos_totales = [1..N] of fallos

process control(id = 1..4){
    pos_inicial = (id - 1) * (N / 4) + 1;
    for int i = pos_inicial to pos_inicial + N/4 - 1 {

        delay(tiempo_chequeo)
        if fallos_totales(i).gravedad = 3{
            print(fallos_totales(i).id)
        }
        P(vector_libre)
            cantidad_fallos[fallos_totales[i].gravedad] += 1
        V(vector_libre)
    }
}

c)

process control2(id = 1..4){
    for int i = 1 to N{

        delay(tiempo_chequeo)
        if (id - 1 = fallos_totales[i].gravedad){
                cantidad_fallos[fallos_totales[i].gravedad] += 1
        }
    }
}
