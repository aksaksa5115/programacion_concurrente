2. En un laboratorio de genética veterinaria hay 3 empleados. El primero de ellos 
continuamente prepara las muestras de ADN; cada vez que termina, se la envía al segundo 
empleado y vuelve a su trabajo. El segundo empleado toma cada muestra de ADN 
preparada, arma el set de análisis que se deben realizar con ella y espera el resultado para 
archivarlo. Por último, el tercer empleado se encarga de realizar el análisis y devolverle el 
resultado al segundo empleado.


Process Empleado1 {
    text muestra;
    while (true) {
        muestra = prepararADN();
        Empleado2 ! muestra(muestra);
    }
}

Process Empleado2 {
    text muestra, setAnalisis, resultado;
    while (true) {
        Empleado1 ? muestra(muestra);
        setAnalisis = armarSetAnalisis(muestra);
        Empleado3 ! analizar(muestra, setAnalisis);
        Empleado3 ? resultado(resultado);
        archivar(resultado);
    }
}

Process Empleado3 {
    text muestra, setAnalisis, resultado;
    while (true) {
        Empleado2 ? analizar(muestra, setAnalisis);
        resultado = realizarAnalisis(muestra, setAnalisis);
        Empleado2 ! resultado(resultado);
    }
}
