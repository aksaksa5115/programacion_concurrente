Existen N vehículos que deben pasar por un puente de acuerdo con el orden de llegada.  
Considere que el puente no soporta más de 50000 kg y que cada vehículo cuenta con su 
propio peso (ningún vehículo supera el peso soportado por el puente). 

Monitor Puente
  int peso = 0;
  cond espera[1..N];
  Cola fila;                       // FIFO de (id, kg)

  Procedure entrar(id: in int, kg: in int)
    if (fila.isEmpty() and peso + kg <= 50000)
      peso = peso + kg;
    else {
      fila.push(id, kg);
      wait(espera[id]);            // al despertar, mi peso ya fue sumado
    }
  end;

  Procedure salir(kg: in int)
    peso = peso - kg;
    while (not fila.isEmpty() and peso + fila.primero().kg <= 50000) {
      p = fila.pop();
      peso = peso + p.kg;          // reservo su lugar
      signal(espera[p.id]);
    }
  end;
End Monitor;

Process Vehiculo [id: 1..N]
  int kg = pesoDe(id);             // cada vehículo conoce su peso
  Puente.entrar(id, kg);
  delay(tiempoCruce);              // fuera del monitor
  Puente.salir(kg);
End Process;