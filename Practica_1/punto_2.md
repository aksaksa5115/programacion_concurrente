2) Realice una solución concurrente de grano grueso (utilizando <> y/o <await B; S>) para el 
siguiente problema. Dado un número N verifique cuántas veces aparece ese número en un 
arreglo de longitud M. Escriba las pre-condiciones que considere necesarias. 

Precondiciones: 
- K procesos Buscar.
- Arreglo inicializado con M elementos.
- M mod K = 0. 

int total=0; int buscado=N; int arreglo [M]; int cantBuscar = M DIV K;

Process Buscar [id: 0..K-1] {
	int aux = 0;
	int ini = id * cantBuscar;
	for i = ini .. (ini + cantBuscar - 1) {
        if (arreglo[i] = buscado) then
            aux++;
    }
    <total = total + aux>
}







