¿Qué ventaja tiene disparar el pipeline con un tag en lugar de con cada push?
el uso de tag es para el versionamiento a nivel de aplicacion terminada, el uso de cada push en una rama es para 
el desarrollo de una caracteristica adicional a una aplicacion que puede ser que no este terminanda, es decir
que pueda tener la necesidad de incorporar desarrollo de caracteristicas de mas desarrolladores.

 ¿Cómo cambia el flujo de trabajo del equipo?
el flujo no cambia, se incorpora el uso de tag al proceso de despliegue

¿Cuál es la diferencia entre usar latest y una versión específica como 1.0.0 al desplegar en producción?
latest es el ultimo release, una version especifica puede ser menor a la ultima release.

El proyecto anterior ya tenía 6 tags creados. ¿Qué significaría haber tenido este workflow desde el principio?
pueden ser 6 versiones releases o releases candidatas o versiones en procesos de releases, depende el contexto 
y el descriptivo que se le de en la aplicacion. se recuerda que cada version cumple con la nomenclatura 
V *.*.* , 