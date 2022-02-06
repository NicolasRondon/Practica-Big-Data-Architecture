# Practica-Big-Data-Architecture

Se requiere un sistema capaz de procesar para mas de 100 000 usuarios informacion sobre distintos cultivos y noticias asociadas a los mismos (Incluso en un futuro se espera que al sistema se le peuda añadir un modelo capaz de predecir los precios de insumos y cultivos en distintas centrales de abasto)


1. Necesitamos obtener la informacion de distintos sitios relacionados con noticias agro (ejemplo: https://www.agronegocios.co): Para esto desarrollamos una **Cloud Function** que hace crawling y scrapping a estos sitios, a su vez esta funcion se ejecuta a travez del servicio **Cloud Scheduler** todos los días a las 6am para traer las nuevas noticias publicadas.
2. El DANE(Departamento Nacional de Estadistica de Colombia) pone a disposicion informacion sobre cultivos y productores en Colombia, a travez de su api nos conectamos por medio de una cloud function para extraer esa inforamcion.
3. Los datos resultantes de las funciones anteriores deben ser almacenadas en algun lugar en este caso se decidio usar  **BigQuery** 
4. A tra vez de cloud DataFlow ingestamos los datos a un Compute Engine de **ElasticSearch*
5. La informacion de ubicacion, nombre y que cultivos tiene un usuario se guarda en una base de datos Postgresql gracias al servicio **CloudSql**
6. En la apliacion queremos que el usuario pueda visualizar informacion acerca de sus cultivos y para eso necesitamos tener una estructura para las graficas debido a la variabilidad que puede tener esta estructura por tipo de grafica o tipo de inforamcion a presentar se decidio usar una base de datos basada en documentos en este caso aprovechando que toda la infraestructura esta en google se elegio usar **Firestore**
7. Todas estas fuentes de datos deben llegar a una aplicacion para visualizar, se decidio levantar esta aplicacion en un **AppEngine** por ser su momento inicial y para ahorrar costos ya que solo habra algun cobro cuando alguna peticion la levante despues de procesar volvera a dormir. A esa instancia de **AppEngine** conectamos elastic, firesotre, cloudsql para entregar los datos finalmente al productor.
