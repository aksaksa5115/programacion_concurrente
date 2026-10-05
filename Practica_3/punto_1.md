 Se dispone de un puente por el cual puede pasar un solo auto a la vez. Un auto pide permiso 
para pasar por el puente, cruza por el mismo y luego sigue su camino.

a. ¿El código funciona correctamente? 
Justifique su respuesta. 
b. ¿Se podría simplificar el programa? ¿Sin 
monitor? ¿Menos procedimientos? ¿Sin 
variable condition? En caso afirmativo, 
rescriba el código. 
c. ¿La solución original respeta el orden de 
llegada de los vehículos? Si rescribió el código 
en el punto b), ¿esa solución respeta el orden 
de llegada?

Monitor  Puente 
    cond cola;  
    int cant= 0; 
 
    Procedure entrarPuente () 
         while ( cant > 0) wait (cola); 
         cant = cant + 1;    
    end; 
 
    Procedure salirPuente () 
        cant = cant – 1; 
        signal(cola); 
    end; 
End Monitor;  
 
Process Auto [a:1..M] 
   Puente. entrarPuente (); 
   “el auto cruza el puente” 
   Puente. salirPuente(); 
End Process; 

a) es correcta, no hay busy waiting ya que al chequear while si cant > 0 el wait duerme al proceso auto
cant se incrementa cuando un auto entra y se decrementa cuando sale un auto

b) si, se podria resumir eliminando todo ya que el monitor en si hace exclusion mutua
podrias resumir todo en una unica funcion dentro del monitor que simbolize el tiempo que tarda
el auto en cruzar el puente
Monitor  Puente 
    procedure cruzarPuente(){
        delay(cruza_puente)
    }
End Monitor;

c) no, es aleatorio y b) tampoco, sigue siendo aleatorio