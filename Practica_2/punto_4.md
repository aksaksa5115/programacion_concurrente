4. Suponga que existe una BD que puede ser accedida por 6 usuarios como máximo al 
mismo tiempo. Además, los usuarios se clasifican como usuarios de prioridad alta y 
usuarios de prioridad baja. Por último, la BD tiene la siguiente restricción: 
• no puede haber más de 4 usuarios con prioridad alta al mismo tiempo usando la BD. 
• no puede haber más de 5 usuarios con prioridad baja al mismo tiempo usando la BD. 
Indique si la solución presentada es la más adecuada. Justifique la respuesta.

Var 
total: sem := 6; 
alta: sem := 4; 
baja: sem := 5;

Process Usuario-Alta [I:1..L]::  
{    P (total); // P(alta)
P (alta); // P(total)
//usa la BD 
V(total); 
V(alta); 
}

Process Usuario-Baja [I:1..K]::  
{    
P (total); // P(baja)
P (baja); // P(total)
//usa la BD 
V(total); 
V(baja); 
} 


no es la mas adecuada, ya que una persona podria consumir una instancia de total
y quedarse esperando a que se libere una instancia de alta, entonces esta 
reteniendo una instancia de mas. Para prevenir esto hay que invertir el orden
de solicitud de los semaforos