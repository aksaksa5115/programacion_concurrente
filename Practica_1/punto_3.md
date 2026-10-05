a)

int cant= 0; int pri_ocupada= 0; int pri_vacia=0; it buffer[N]; 
Process Productor::{
    //produce el elemento 
    <await (cant < N); cant++ 
    buffer[pri_vacia] = elemento;>
    pri_vacia = (pri_vacia + 1) MOD n;
    }
}
Process Consumidor::{
	while(true) {
        <await(cant > 0); cant –; 
        elemento= buffer[pri_ocupada>;
        pri_ocupada= (pri_ocupada) + 1) mod N;
        //consume elemento;
	}
}
b)

int cant= 0; int pri_ocupada= 0; int pri_vacia=0; it buffer[N]; 
Process Productor [id: 0...P-1] {
	while (true) {
        //produce el elemento 
        <await (cant < N); cant++ 
        buffer[pri_vacia] = elemento;
        pri_vacia = (pri_vacia + 1) MOD n;>
    }
}
Process Consumidor [id: 0...C-1] {
	while(true){
		{<await(cant > 0); cant –; 
		elemento= buffer[pri_ocupada];
		pri_ocupada= (pri_ocupada) + 1) mod N >
		//consume elemento;
	}
}