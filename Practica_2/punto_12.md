Simular la atención en una Terminal de Micros que posee 3 puestos para hisopar a 150 
pasajeros. En cada puesto hay una Enfermera que atiende a los pasajeros de acuerdo 
con el orden de llegada al mismo. Cuando llega un pasajero, se dirige al Recepcionista, 
quien le indica qué puesto es el que tiene menos gente esperando. Luego se dirige al puesto 
y espera a que la enfermera correspondiente lo llame para hisoparlo. Finalmente, se retira.
a) Implemente una solución considerando los procesos Pasajeros, Enfermera y 
Recepcionista.
b) Modifique la solución anterior para que sólo haya procesos Pasajeros y Enfermera, 
siendo los pasajeros quienes determinan por su cuenta qué puesto tiene menos 
personas esperando. 
Nota: suponga que existe una función Hisopar() que simula la atención del pasajero por 
parte de la enfermera correspondiente.

boolean libre = True
int indice = 1
sem fila[1..150] = ([1..150], 0);
cola c[0, 0, 0]

process pasajeros(id: 1..150){
    if (libre){
        libre = false
        recepcionista(id)
    } else{
        wait(fila[id])
    }

    pasajero llega (ocupa al recepcionista)
    se dirige al puesto mas vacio
}

process enfermera(id: 1..3){
    recibe persona y la atiende en orden de llegada
}

process recepcionista(int id){
    
    calcula puesto mas vacio
    se lo dice a la persona
}