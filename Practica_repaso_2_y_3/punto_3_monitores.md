Resolver el siguiente problema. En una montaña hay 30 escaladores que en una parte de la subida deben utilizar un único paso de a uno a la vez y de acuerdo con el orden de llegada al mismo. Nota: sólo se pueden utilizar procesos que representen a los escaladores; cada escalador usa sólo una vez el paso.


Monitor escalar
    bool libre = true
    cond cola
    int esperando = 0

    procedure llegar(){
        if (not libre){
            esperando += 1
            wait(cola)
        } else {
            libre = false
        }
    }

    procedure salir(){
        if (esperando > 0){
            esperando += -1
            signal(cola)
        } else {
            libre = true
        }

    }

End Monitor

process escalador(id:1..30){

    escalar.llegar()
    escalar.salir()
}