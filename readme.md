# MEMORIA MILVUS

## 1. Cálculo de los embeddings
borrar--------------
<img  src=""  width="250"/>
borrar---------------

Para hacer el cálculo de los embedding, en esta práctica, se usa el modelo CLIP. Lo hacemos por batches de la siguiente manera:

<img  src="https://lh7-us.googleusercontent.com/M5D5zFizWVnOvjMDsvUgETbb8UAPFTHP3qTgpaR3psACH7SmPWxbegDF3hrGibBol8JrX2-BaPaCUoJE1HCJ-yYWo-PZ-ERGl_BKvRfvnTebFKRR0Yk5HniDMuETEvP2PfjnpWN1_6MUT4MT32WFM6U"  width="450"/>


## 2. Creación de la bbdd y su esquema en Milvus

Usamos pymilvus para crear la conexión con la base de datos que hemos levantado con el docker y creamos un esquema nuevo.

<img  src="https://lh7-us.googleusercontent.com/-YBPv0kaODqC2VxaLKSnnQvXN9B6GGzbakcTS2b3HHy7E-baCIIJbI-l6VurIXXrVWr1vZk8r_xjVcMqCx7Y1st1_y0YJ7TqX5gvdK__v1Pp1MwQQJUl5s_59WDZMx4JT_tpw-ABsc_fYzR0Tqu_QxM"  width="300"/>

Añadimos todas las columnas de la tabla y les ponemos sus parámetros (en este caso nos hemos quedado con el AUTOINDEX y con la métrica de COSENO)

<img  src="https://lh7-us.googleusercontent.com/bM9rjf3553CzOv0d8CNpTgPP-ibRORDPxT5hWS7XPgZCkWgitZDOE8TIbuAnm2bBxoonQCicrGb34XZD2-uNs5RUjrI-btb5S_CTozxNTKY-NmZC3b3H74Msa4flbR77CAxx35__VGfqdWvP6OVI9bk"  width="550"/>


## 3. Búsqueda por imágenes o texto

Una vez hemos subido los embeddings de las imágenes con las correspondientes rutas hacia cada una de ellas ya podemos crear las funciones para hacer las querys, tanto por imágenes como por textos.

<img  src="https://lh7-us.googleusercontent.com/N0R8RivEsAX2Wo_FWV56iiOMyXWDCc2Yo3k-9rQ5bksF0SUdd1IuYP1cY3Pk87bhDPrK-wzgeS4lwOvKxd6FS3A1DWhnq0AidILKEMyQ3KMF_HGxBrqrCQETNxlt9B85bvge5_Ang-jtzYr_obgQiEs"  width="550"/>


<img  src="https://lh7-us.googleusercontent.com/N5GlYhXTH-yDFSQ1wgDj9Sz2BuI1ScTgxZ7ZB63F9SEUNVQPeQv0-IwqcwCefgGzA96xfauNsxGSsgRNPBVwh24g9bjHWhL48dfJucpgD5kUXUjUndYpNEAxGYPeGRHgK5Gyu79alocI-fT6Cr8c65E"  width="500"/>

Lo que hace la primera función es calcular el embedding de la imagen que hayamos puesto como query y luego buscamos con ese embedding los más parecidos que tengamos almacenados en Milvus (mediante la métrica de coseno), cogemos los paths de las imágenes de estos embeddings y los mostramos. Hacemos lo mismo con la función de búsqueda por texto, ya que Clip puede calcular los embeddings tanto de imágenes como de textos.

Vamos a ver los resultados que hemos obtenido.
Para la búsqueda por imágenes hemos escogido una foto de LeBron James:
<img  src="https://lh7-us.googleusercontent.com/Zh7794R4UA_9GYiXk4tLO29Nj4it1v8PyJm2GzEQjrndkxzdpwKbWrIyFu7w_CW19ZvGFc2ShLaot4XRTD2EGUD_jkHmtQ3Or5kbkAZ5eI3SN4gt2llnYHbxgeHqc816AP5mJ_O_-ha4248aiuvXN3s"  width="400"/>

Y para la búsqueda por texto hemos buscado "A portrait of blond men":
<img  src="https://lh7-us.googleusercontent.com/mkDGXoaCZ3LUBbc-IB-huFpWf-2R8QTfyUacINdJ535wNNzpj45k8zpTRADnI1bXn60YXFuKgquYd56Roh28M5Kh1mBC2pQlsXmqy9OADAKTRPyJ709steGFkBpCpWCCNn8k-sUtQoEE5v06ET15xZ4"  width="400"/>

Podemos ver que los resultados tienen sentido según las búsquedas que hemos hecho.

## 4. Testeo de rendimiento
Si bien la métrica COSENO para calcular las distancias no es la que más rápido funcione, sí que es la recomendada para trabajar con embeddings, como es en este caso, por lo que vamos a hacer las pruebas con esta distancia.

Lo que si que hemos cambiado son los tipo de índice, hemos probado FLAT, IVF_FLAT y HNSW replicando el código que hemos usado pero cambiando la parte del índice, por ejemplo:
<img  src="https://lh7-us.googleusercontent.com/CTmuYPs8JMvI_JpI5W7m8x55BTRElRU80k_6Ehf-N1mQyX-m3E0rlg3oc9ZuZchOpNiVYM67I3UItc_exQyzS0udJrfpww_OP2-SJOn1dMMy6_czm3JwoeH3p_N0mv5Yd1VqrnjYm9Kirv-mq79HbnY"  width="350"/>

Al final hemos visto que el FLAT es el que mejor nos funciona, en concreto como tenemos una base de datos pequeña, aunque este tipo de índice es muy exhaustivo, el tiempo de ejecución es muy parecido a los demás, y como es muy preciso, nos viene muy bien para este caso, que no tenemos muchos avatares para recomendar


