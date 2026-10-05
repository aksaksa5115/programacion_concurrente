1) Resolver los problemas siguientes:
a) En una estación de trenes, asisten P personas que deben realizar una carga de su tarjeta SUBE
en la terminal disponible. La terminal es utilizada en forma exclusiva por cada persona de acuerdo
con el orden de llegada. Implemente una solución utilizando únicamente procesos Persona. Nota:
la función UsarTerminal() le permite cargar la SUBE en la terminal disponible.
b) Resuelva el mismo problema anterior pero ahora considerando que hay T terminales disponibles.
Las personas realizan una única fila y la carga la realizan en la primera terminal que se libera.
Recuerde que sólo debe emplear procesos Persona. Nota: la función UsarTerminal(t) le permite
cargar la SUBE en la terminal t.

Cola c;
boolean libre = true;
sem mutex = 1;
sem espera[P] = ([P], 0);

Process Persona [id: 0..P-1] {
	int idAux;
	P (mutex);
	if not libre {
		c.push(id);
		V (mutex);
		P (espera[id]);
    } else {
        libre = false;
        V (mutex);
    }
    UsarTerminal();
    P (mutex);
    if c.isEmpty() {
        libre = true;
    } else {
        c.pop(idAux);
        V (espera[idAux]);
    }
    V (mutex);
}

b)

Cola terminalesLibres;          // inicialmente contiene 1..T
Cola esperaTerminal;            // FIFO de ids
sem mutex = 1;                  // protege ambas colas
sem espera[0..P-1] = ([P], 0);
int vecTerminal[0..P-1];        // terminal asignada a cada persona

Process Persona [id: 0..P-1]
  int terminal, idAux;

  P(mutex);
  if (not terminalesLibres.isEmpty()) {
    terminalesLibres.pop(terminal);
    vecTerminal[id] = terminal;
    V(mutex);
  } else {
    esperaTerminal.push(id);
    V(mutex);                   // soltar ANTES de bloquearse
    P(espera[id]);              // al despertar, vecTerminal[id] ya está cargado
  }

  UsarTerminal(vecTerminal[id]);   // fuera de la sección crítica

  P(mutex);
  if (esperaTerminal.isEmpty()) {
    terminalesLibres.push(vecTerminal[id]);   // nadie espera: la terminal queda libre
  } else {
    esperaTerminal.pop(idAux);
    vecTerminal[idAux] = vecTerminal[id];     // le paso MI terminal
    V(espera[idAux]);                         // traspaso directo
  }
  V(mutex);
End Process;