1) Resolver el siguiente problema. En una elección estudiantil, se utiliza una máquina para voto
electrónico. Existen N Personas que votan y una Autoridad de Mesa que les da acceso a la máquina
de acuerdo con el orden de llegada, aunque ancianos y embarazadas tienen prioridad sobre el resto.
La máquina de voto sólo puede ser usada por una persona a la vez. Nota: la función Votar() permite
usar la máquina

monitor Mesa {
    cond hayPersona                     # la Autoridad espera acá si no hay nadie
    cond turno[1..N]                    # cada persona espera su turno en su propia condición
    cond fin                            # la Autoridad espera acá a que la persona termine de votar
    cola prioritarios, normales

    procedure llegar(id: int, prioridad: bool) {
        if (prioridad) push(prioritarios, id)
        else push(normales, id)
        signal(hayPersona)              # aviso a la Autoridad que hay alguien
        wait(turno[id])                 # espero que me den acceso a la máquina
    }

    procedure atender() {               # lo llama la Autoridad
        int sig
        while (empty(prioritarios) and empty(normales))
            wait(hayPersona)
        if (not empty(prioritarios)) pop(prioritarios, sig)
        else pop(normales, sig)
        signal(turno[sig])              # le doy acceso a esa persona
        wait(fin)                       # espero que termine de votar
    }

    procedure terminar() {
        signal(fin)                     # aviso que liberé la máquina
    }
}

process Persona[id: 1..N] {
    bool prioridad = ...                # true si es anciano o embarazada
    Mesa.llegar(id, prioridad)
    Votar()                             # fuera del monitor
    Mesa.terminar()
}

process Autoridad {
    for (i = 1; i <= N; i++)
        Mesa.atender()
}