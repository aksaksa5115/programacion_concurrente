1. Suponga que existe un antivirus distribuido que se compone de R procesos robots 
Examinadores y 1 proceso Analizador. Los procesos Examinadores están buscando 
continuamente posibles sitios web infectados; cada vez que encuentran uno avisan la 
dirección, esperan la respuesta del Analizador, y luego continúan buscando. El proceso 
Analizador se encarga de hacer todas las pruebas necesarias con cada uno de los sitios 
encontrados por los robots para determinar si están o no infectados, para luego responderle 
al Examinador.  
a) Analice el problema y defina qué procesos, recursos y comunicaciones serán 
necesarios/convenientes para resolverlo. 
b) Implemente una solución con PMS donde el Analizador no considere el orden 
de los pedidos de los Examinadores. 
c) Implemente una solución con PMS donde el Analizador sí considere el orden de 
los pedidos de los Examinadores. 
d) Si los Examinadores no deben esperar la respuesta del Analizador, ¿cambiaría 
algo de la solución al inciso b)? ¿por qué? 

a) 
Procesos: Examinador(1..R) Analizador(id:1)
Recursos: El Examinador le pasa la URL al Analizador y este le devuelve un resultado
Comunicaciones: Examinador --> Analizador, Analizador --> Examinador

b)

Process Examinador[id: 1..R] {
    text url, respuesta;
    while (true) {
        url = buscarSitioSospechoso();
        Analizador ! sitio(id, url);
        Analizador ? resultado(respuesta);
    }
}

Process Analizador {
    int idE;
    text url, respuesta;
    while (true) {
        Examinador[*] ? sitio(idE, url);
        respuesta = analizar(url);
        Examinador[idE] ! resultado(respuesta);
    }
}

c)

Process Examinador[id: 1..R] {
    text url, respuesta;
    while (true) {
        url = buscarSitioSospechoso();
        Analizador ! sitio(id, url);
        Analizador ? resultado(respuesta);
    }
}

Process coordinador(){
    cola [txt, int];
    while (true){

    }
}

Process Analizador {
    int idE;
    text url, respuesta;
    while (true) {
        Examinador[*] ? sitio(idE, url);
        respuesta = analizar(url);
        Examinador[idE] ! resultado(respuesta);
    }
}