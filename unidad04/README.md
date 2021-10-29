# Unidad 04 - Diseño de Aplicaciones

¡Bienvenidos!

Esta práctica se corresponde a la unidad teórica número 4 donde se vieron algunas herramientas extras para optimizar el diseño de aplicaciones en la plataforma, siempre cumpliendo con los atributos de calidad de las aplicaciones _cloud-native_.

## Enunciado

#### Parte 1

La organización que lo contrató comenzó a incorporar otras soluciones a la plataforma, producto de la migración que se encuentran llevando a cabo ahora mismo.

Teniendo una variedad de aplicaciones en el cluster, comienza a ser una tarea compleja **identificarlas de una forma simple y unívoca** o incluso agruparlas para aplicarles cierta configuración.

¿Qué haria para solventar esta problemática? Una pista parecería venir por el lado de etiquetas/anotaciones, con lo cual dejamos a responsabilidad del lector etiquetar o anotar a la aplicación monolítica de hoteles, segun corresponda, para diferenciarla de otras aplicaciones que puedan ser desplegadas en el cluster.

Acá es importante que se comunique en la entrega qué criterios se utilizaron a la hora de catalogar la aplicación.

#### Parte 2

¡El equipo de desarrollo liberó una nueva versión de la aplicación monolítica de hoteles!

Esta vez, nos llegó la información que la imagen Docker de la aplicación tiene una nueva etiqueta: `ghcr.io/go-elevate/k8s4arch-hotels:monolith-v2`

Nos solicitan que diseñemos el manifiesto Kubernetes para **actualizar la versión de la aplicación**, teniendo en cuenta que:

- la nueva versión no está del todo validada, puede contener errores, por lo cual tenemos que tener un plan para volver a la versión estable rápidamente
- la actualización debería garantizar alta disponibilidad, no podemos dejar a los usuarios sin poder reservar hoteles, de eso depende el negocio de la organización
- no se piensa adicionar ninguna herramienta que nos ayude con la transparencia en la actualización de las aplicaciones de cara a los usuarios (canary, blue/green), con lo cual debería ser una estrategia soportada nativamente por la plataforma

¿Qué estrategia de despliegue considera óptima para este caso? Implemente los cambios necesarios en el YAML para soportar esta necesidad con la configuración particular según su criterio.

#### Parte 3

Actualizamos la aplicación, ¡perfecto! Pero descubrimos que debido a una mala decisión de diseño, la misma es bastante lenta para arrancar. Seguramente el problema radique por algún procesamiento costoso que provoca una **indisponibilidad temporal cuando se inicializa el contexto aplicativo**. 

Nos solicitan una vez más una intervención como Arquitectos. El problema parece ser claro: _necesitamos pedirle a Kubernetes que demore un tiempo previo a redigirir tráfico a la aplicación_.

¿Cómo podemos asegurar eso sin afectar la salud general de la aplicación? Implemente los cambios necesarios en el manifiesto YAML correspondiente.  

Como dato adicional nos informan que el frontend del monolito expone el contenido en el **puerto 80 por http**.

#### Parte 4

El equipo de desarrollo comenzó el proceso de desacoplamiento de la solución, con lo cual quieren tener un ambiente bajo de pruebas en Kubernetes para validar que el backend funciona como esperan.

Recordemos que la funcionalidad de la aplicación es reservar hoteles sobre un catálogo de alternativas posibles en LATAM. La gente del negocio solicitó tener un **reporte diario de que hoteles se reservan y por cuantos usuarios**.

En base a esta necesidad, nos solicitan realizar los cambios necesarios para soportar la generación, exportación y centralización de esos reportes en un almacenamiento externo. Usted, como Arquitecto, solicitó al equipo de desarrollo que implemente la solución acoplada al backend, para evitar generar codebase o imágenes nuevas, pero en el fondo sabe que el _Pod_ no es el mejor objeto para modelar la solución a esa problemática.

¿Cómo haria para modelar esta necesidad en Kubernetes? Genere el manifiesto YAML acorde a la estrategia elegida.

Datos útiles:

- la imagen a utilizar es una generada exclusivamente para el backend de hoteles, la etiqueta es: `ghcr.io/go-elevate/k8s4arch-hotels-backend:slim`
- el comando a ejecutar dentro de la imagen para generar el reporte es `python report.py`, es decir si uno ejecutara la aplicación con docker, utilizaría este comando: `docker run --entrypoint python ghcr.io/go-elevate/k8s4arch-hotels-backend:slim report.py` 
- el reporte deberá generarse en forma diaria como se indicó, por la madrugada, 2 AM.
- no es necesario considerar aspectos de paralelismo

Esta es la información, a modo de ejemplo, que deberían ver en los logs de la imagen, si se configuró el manifiesto exitosamente:

```bash
report-1616210160-47k72 daily-report Hotel Entre Cielos had 14 reservations. Hotel is at 87.5% of its full capacity
report-1616210160-47k72 daily-report Hotel Casa Turquesa had 7 reservations. Hotel is at 77.77777777777779% of its full capacity
report-1616210160-47k72 daily-report Hotel Hotel Huacalera had 24 reservations. Hotel is at 75.0% of its full capacity
report-1616210160-47k72 daily-report Hotel Luma Casa de Montaña had 3 reservations. Hotel is at 37.5% of its full capacity
report-1616210160-47k72 daily-report Hotel Alto Atacama had 6 reservations. Hotel is at 14.285714285714285% of its full capacity
```

## Entrega y Devolución

Con respecto a esta entrega, se espera del alumno:

- **un manifiesto que contenga únicamente la solución propuesta para la primer parte**. Nota: es recomendable reutilizar el mismo archivo de la práctica pasada y adaptarlo.
- **un único manifiesto para la parte 2 y 3** 
- **un manifiesto separado para la parte 4**  

El docente corregirá los manifiestos YAML, probándolos contra su entorno configurado de Kubernetes. En base a los resultados que arroje ese estado deseado, será la nota correspondiente para el alumno.  

La publicación oficial de la práctica será por la plataforma Google Classroom, donde se generará un material con devolución pautada para una fecha en particular y que es calificable.


## Aclaraciones

Al alumno se le proporcionó:

- un manual teórico con toda la información relevante a la unidad.
- un manual estilo guía o _walkthrough_ con información que puede utilizar a modo de soporte para la resolución de la ejercitación en cuestión.

Si alguno de estos documentos no está en su poder, contactarse con el personal docente para resolver la situación.