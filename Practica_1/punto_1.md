1. Para el siguiente programa concurrente suponga que todas las variables están inicializadas en 
0 antes de empezar. Indique cual/es de las siguientes opciones son verdaderas: 
a) En algún caso el valor de x al terminar el programa es 56. 
b) En algún caso el valor de x al terminar el programa es 22. 
c) En algún caso el valor de x al terminar el programa es 23. 

P1::                   P2::                    P3::
    if(x=0) then           if(x>0) then            X:= (X*3) + (X*2) + 1;
        Y:= 4*2;                X:= X + 1;
        X:= y + 2;

p1:	1- Load y, reg1
    2- Add 2, reg1
    3- Store reg1, x	

p2:	4- Load x,reg2
	5- Add 1, reg2
	6- Store reg2, x

p3:	7- Load x, reg3
	8- Load x, reg4
	9- Multi  reg3, 3, reg 3
    10- Multi reg4, 2, reg4
    11- Add reg3, reg4, reg5
    12- Add 1, reg5
    13- Store x, reg5 

a) V. Es posible que x termine con el valor 56 si se ejecuta el P1, P2 y luego el P3
b) V. Es posible que x termine con el valor 22 si el orden de las instrucciones es 1- 2- 7- 3- 8- 9- 10- 11- 12- 13 - 4- 5- 6
c) V. Es posible que x termine con el valor 23 si el orden de las instrucciones es 1-2-7-3-4-5-6-8-9-10-11-12-13
