Simular una atención en una Salita Medica para vacunar contra el coronavirus. Hay UNA enfermera
encargada de vacunar a 30 pacientes. Cada paciente tiene un turno asignado (valor entre 1 y 30 ya conocido
por el paciente). La enfermera atiende a los pacientes de acuerdo al turno que cada uno tiene asignado. Cada
paciente al llegar espera a que sea su turno y se dirige al consultorio para que la enfermera lo vacune y luego
se retira. Nota: suponer que existe una función Vacunar() que simula la atención del paciente por parte de la
enfermera. Todos los procesos DEBEN terminar.

Monitor vacunacion
    int siguiente = 0;
    bool presente = false;
    cond espera[1..30], hayPaciente, fin;

    procedure llegar(int turno){
        if (siguiente != turno){
            wait(espera[turno])
        }
        presente = true
        signal(hayPaciente);
        wait(fin)
    }

    procedure llamar(int turno){
        siguiente = turno
        signal(espera[turno])
        while (not presente){
            wait(hayPaciente)
        }
        presente = false
    }

    procedure terminar(){
        signal(fin)
    }

End Monitor

process clientes(id: 1..30){
    vacunacion.llegar(id)
}

process enfermera(){
    for (int i=1; i < 30; i++){
        vacunacion.llamar(i)
        vacunar()
        vacunar.terminar()
    }
}