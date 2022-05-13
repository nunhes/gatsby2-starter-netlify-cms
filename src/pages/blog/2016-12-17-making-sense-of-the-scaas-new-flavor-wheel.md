---
templateKey: blog-post
title: >-
  Como migrar bases de datos PostgreSQL a MySQL usando o asistente de migración
  de MySQL Workbench
date: 2016-12-17T15:04:10.000Z
description: >-
  Na súa versión 5.2.41, MySQL Workbench presentou un novo módulo do asistente
  de migración. Este módulo permíte migrar de xeito sinxelo e rápido bases de
  datos de varios produtos RDBMS a MySQL.
tags:
  - flavor
  - tasting
---


MySQL Workbench 5.2.41 presentou o novo módulo do asistente de migración. Este módulo permítelle migrar de xeito sinxelo e rápido bases de datos de varios produtos RDBMS a MySQL. A partir de Workbench 5.2.44 pode migrar bases de datos desde Microsoft SQL Server, PostgreSQL e Sybase Adaptive Server Enterprise. Tamén prevé migracións xenéricas, é dicir, migracións doutros RDBMS que non están explícitamente compatibles, sempre que teñan un controlador ODBC ben comportado. Máis sobre isto nunha próxima publicación...

Ademais, pode usar o asistente de migración para realizar migracións de bases de datos de MySQL a MySQL, que se poden usar para tarefas como copiar unha base de datos en servidores ou migrar datos en diferentes versións de MySQL.

Xa describimos nunha publicación anterior [como usar o asistente de migración para migrar unha base de datos de Microsoft SQL Server a MySQL](https://dev.mysql.com/blog-archive/?p=1350) . Neste post imos migrar unha base de datos PostgreSQL a MySQL usando o asistente de migración.

Así que ensuciamos as nosas mans e percorremos o asistente de migración para migrar unha base de datos PostgreSQL a MySQL. No resto desta publicación supoño que tes:

- Unha instancia de PostgreSQL en execución na que tes acceso adecuado á base de datos que queres migrar. (A partir de agora chamarei a esta base de datos a base de datos de *orixe* ). Teño unha instancia de PostgreSQL en execución nun ordenador (unha caixa Ubuntu 12.04) na miña rede local. Instalei encima a base de [datos de mostras](http://pgfoundry.org/frs/?group_id=1000150&release_id=998) de Pagila de [pgFoundry](http://pgfoundry.org/) . Estou usando o usuario estándar de `postgres` , que ten todos os privilexios. Podes usar calquera versión de PostgreSQL que teñas a man, pero ten en conta que o asistente de migración admite oficialmente PostgreSQL 8.0 e posteriores, polo que é posible que as versións máis antigas de PostgreSQL non funcionen.
- Unha instancia de MySQL Server en execución con acceso de usuario adecuado. O asistente de migración admite versións de MySQL a partir da 5.0, así que asegúrate de ter unha versión compatible. Para este tutorial estou usando MySQL Server 5.5.27 CE instalado no mesmo PC Ubuntu onde se está a executar PostgreSQL.
- MySQL Workbench 5.2.44 ou posterior. Esta vez vou usar Workbench para Linux. Teño Workbench funcionando noutro ordenador con Ubuntu 12.10 instalado.

Comecemos agora...

## Descargue, compile (só Linux e Mac) e instale un controlador ODBC de PostgreSQL

Debe instalar un controlador ODBC para PostgreSQL na máquina onde instalou MySQL Workbench. Siga [as instrucións aquí](https://dev.mysql.com/blog-archive/set-up-and-configure-postgresql-odbc-drivers-for-the-mysql-workbench-migration-wizard/) .

## Abra MySQL Workbench e inicie o asistente de migración

Desde a pantalla principal de MySQL Workbench pode iniciar o asistente de migración facendo clic no iniciador de migración de base de datos no panel de Workbench Central ou a través de Base de datos > Migrar no menú principal.

[![wb_pantalla_inicial](http://wb.fabforce.eu/wp-content/uploads/wb_initial_screen-800x424.png)](http://wb.fabforce.eu/wp-content/uploads/wb_initial_screen.png)

Debería aparecer unha nova pestana que mostra a páxina Visión xeral do asistente de migración.

[![Páxina de descrición xeral do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-overview-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-overview-page.png)

Lea atentamente a sección Requisitos previos. Podes ler alí que necesitas un controlador ODBC para o teu RDBMS de orixe instalado. Se instalou o controlador psqlODBC como se explica na sección anterior, xa está listo.

## Configure os parámetros para conectarse á base de datos de orixe

Fai clic no botón Iniciar a migración na páxina Visión xeral para avanzar á páxina Selección de orixe. Nesta páxina cómpre proporcionar a información sobre o RDBMS que está a migrar, o controlador ODBC a utilizar e os parámetros para a conexión. O nome do controlador ODBC é o que configurou cando rexistrou o controlador psqlODBC co xestor de controladores (psqlODBC, por exemplo).

Se abre a caixa combinada Sistema de base de datos atopará unha lista dos RDBMS admitidos. Seleccione *PostgreSQL* da lista. Xusto debaixo hai outra caixa combinada chamada Conexión almacenada. Listará as configuracións de conexión gardadas para ese RDBMS. Podes gardar conexións marcando a caixa de verificación na parte inferior da páxina e dándolles un nome da túa preferencia.

O seguinte cadro combinado é para a selección do Método de conexión . Esta vez imos seleccionar *ODBC (parámetros introducidos manualmente)* da lista xa que imos escribir manualmente os parámetros para a nosa conexión PostgreSQL. Outras alternativas son as fontes de datos ODBC e as cadeas de conexión ODBC.

Agora é o momento de poñer os parámetros para a súa conexión. No campo de texto Controlador, escriba o nome do controlador ODBC do paso anterior (psqlODBC). Encha os parámetros restantes (nome de host, porto, nome de usuario, contrasinal e base de datos) cos valores axeitados. O controlador psqlODBC non permite conectarse sen especificar un nome de base de datos, así que asegúrese de poñer o nome da súa base de datos antes de tentar conectarse. Neste punto deberías ter algo así:

[![Páxina de selección de orixe do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-source-selection-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-source-selection-page.png)

Fai clic no botón Probar conexión para comprobar a conexión coa túa instancia de PostgreSQL. Se estableces os parámetros correctos, deberías ver unha mensaxe informando dun intento de conexión exitoso.

## Configure os parámetros para conectarse á base de datos de destino

Fai clic no botón Seguinte para ir á páxina de selección de destino. Unha vez alí, configure os parámetros para conectarse á súa instancia de MySQL Server. Cando remates, fai clic no botón Probar conexión e verifica que podes conectarte correctamente a ela.

[![Páxina de selección de destino do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-target-selection-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-target-selection-page.png)

## Seleccione o esquema(ta) para migrar

Fai clic no botón Seguinte para pasar á páxina seguinte. O asistente de migración comunicarase coa súa instancia de PostgreSQL para obter unha lista dos esquemas na súa base de datos de orixe.

[![Obtendo a lista de esquemas no asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-fetching-schemata-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-fetching-schemata.png)

Verifique que todas as tarefas remataron correctamente e prema no botón Seguinte para avanzar. Daráselle unha lista de esquemas para seleccionar os que quere migrar. A páxina de selección de esquemas terá o seguinte aspecto:

[![Páxina de selección de esquemas do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-schema-selection-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-schema-selection-page.png)

Seleccione a base de datos de mostra de *Pagila* da lista e o seu esquema predeterminado *public* . Agora mira as opcións a continuación. Unha base de datos PostgreSQL está formada por un catálogo e un ou máis esquemas. MySQL só admite un esquema en cada base de datos (para ser máis precisos, unha base de datos MySQL *é*un esquema) polo que temos que indicarlle ao asistente de migración como xestionar a migración de esquemas na nosa base de datos fonte. Podemos manter todos os esquemas tal e como están (o asistente de migración creará unha base de datos por esquema) ou combinalos nunha única base de datos MySQL. As dúas últimas opcións son para especificar como se debe facer a fusión: ou eliminar os nomes dos esquemas (o asistente de migración xestionará as posibles colisións de nomes que poidan aparecer no camiño) ou ben engadindo o nome do esquema aos nomes dos obxectos da base de datos como prefixo. . Seleccione a segunda opción xa que só temos un esquema e non estamos especialmente interesados en manter o seu nome *público* sen sentido .

## Seleccione os obxectos para migrar

Vaia á páxina seguinte usando o botón Seguinte. Debería ver a enxeñaría inversa do esquema seleccionado en curso. Neste punto, o asistente de migración está a recuperar información relevante sobre os obxectos da base de datos implicados (nomes de táboas, columnas de táboas, claves primarias e externas, índices, activadores, vistas, etc.). Presentarase unha páxina que mostra o progreso como se mostra na imaxe de abaixo.

[![Enxeñería inversa dos esquemas de orixe o asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-reverse-engineering-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-reverse-engineering.png)

Pode levar algún tempo, dependendo da velocidade da túa conexión co servidor, da carga do teu servidor PostgreSQL e da carga da túa máquina local. Agarda a que remate e comproba que todo foi ben. Despois pasa á páxina seguinte. Na páxina Obxectos de orixe terás unha lista cos obxectos que se recuperaron e que están dispoñibles para a migración. Será así:

[![Páxina de obxectos fonte do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-source-objects-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-source-objects-page.png)

Como podes ver, o asistente de migración descubriu obxectos de táboa na nosa base de datos de orixe. Se fai clic no botón Mostrar selección, terás a oportunidade de seleccionar exactamente cal deles queres migrar como se mostra aquí:

[![Páxina de obxectos fonte do asistente de migración de MySQL Workbench (ampliada)](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-source-objects-page2-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-source-objects-page2.png)

Os elementos da lista da dereita son os que se van migrar. Teña en conta como pode usar a caixa de filtros para filtrar facilmente a lista (tamén se permiten comodíns alí). Usando os botóns de frecha podes filtrar os obxectos que non queres migrar. Ao final, non esqueza limpar a caixa de texto do filtro para comprobar a lista completa dos obxectos seleccionados. Imos migrar todos os obxectos da táboa, así que asegúrate de que todos estean na lista Obxectos para migrar e de que a caixa de verificación Migrar obxectos de táboa estea marcada. Na maioría das veces quererá migrar todos os obxectos do esquema de todos os xeitos, polo que só pode facer clic en Seguinte.

## Revisa a migración proposta

Ir á páxina seguinte. Verás o progreso da migración alí. Neste punto, o asistente de migración está a converter os obxectos seleccionados nos seus obxectos equivalentes en MySQL e a crear o código MySQL necesario para crealos no servidor de destino. Deixa que remate e pasa á páxina seguinte. Quizais teñas que esperar un pouco antes de que a páxina de edición manual estea lista, pero terás algo así:

[![Páxina de edición manual do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-manual-editing-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-manual-editing-page.png)

Como podes ver na imaxe superior hai un cadro combinado chamado Ver . Ao usalo pode cambiar a forma en que se mostran os obxectos da base de datos migrados. Bótalle unha ollada tamén ao botón *Mostrar código e mensaxes* . Se fai clic nel podes ver (e editar!) o código MySQL xerado que corresponde ao obxecto seleccionado. Ademais, pode facer dobre clic nunha fila na árbore de obxectos e editar o nome do obxecto de destino. Supoña que quere que a base de datos resultante teña outro nome. Non hai problema: fai dobre clic na fila Pagila e renomeala.

[![Páxina de edición manual do asistente de migración de MySQL Workbench (todos os obxectos)](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-manual-editing-page2-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-manual-editing-page2.png)

Unha opción interesante no cadro combinado Ver é a de Asignacións de *columnas* . Mostrarache todas as columnas da táboa e permitirache revisar e corrixir individualmente a asignación de tipos de columnas, valores predeterminados e outros atributos.

[![Páxina de edición manual do asistente de migración de MySQL Workbench (asignacións de columnas)](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-manual-editing-page3-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-manual-editing-page3.png)

## Execute o código MySQL resultante para crear os obxectos da base de datos

Move á páxina Opcións de creación de obxectivos. Será así:

[![Páxina de creación de destino do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-target-creation-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-target-creation-page.png)

Como podes ver alí, ofrécese as opcións de executar o código xerado no RDBMS de destino (a súa instancia de MySQL do segundo paso) ou simplemente descargalo nun ficheiro de script SQL. Déixao como se mostra na imaxe e pasa á páxina seguinte. O código PostgreSQL migrado executarase no servidor MySQL de destino. Podes ver o seu progreso na páxina Crear esquemas:

[![Crear páxina de esquemas do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-create-schemata-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-create-schemata-page.png)

Unha vez que remate a creación dos esquemas e dos seus obxectos, pode pasar á páxina Crear resultados de destino. Presentarache unha lista cos obxectos creados e se houbo erros ao crealos. Revísao e asegúrate de que todo saíu ben. Debería verse así:

[![Crear páxina de resultados de destino do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-create-target-results-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-create-target-results-page.png)

Aínda pode editar o código de migración usando a caixa de códigos á dereita e gardar os cambios facendo clic no botón Aplicar. Teña en conta que aínda necesitaría recrear os obxectos co código modificado para poder realizar realmente os cambios. Isto faise facendo clic no botón Recrear obxectos. Pode que teña que editar o código xerado se fallou a súa execución. Despois podes corrixir manualmente o código SQL e volver executalo todo. Neste tutorial non estamos a cambiar nada, así que deixa o código tal e como está e pasa á páxina Configuración da transferencia de datos facendo clic no botón Seguinte.

## Transfire os datos á base de datos MySQL

Os seguintes pasos do asistente de migración son para a transferencia de datos da base de datos de orixe SQL Server á base de datos MySQL que acaba de crear. A páxina Configuración da transferencia de datos permítelle configurar este proceso.

[![Páxina de configuración da transferencia de datos do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-data-transfer-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-data-transfer-page.png)

Aquí hai dous conxuntos de opcións. O primeiro permítelle realizar unha transferencia en directo e/ou volcar os datos nun ficheiro por lotes que pode executar máis tarde. O outro conxunto de opcións ofrécelle unha forma de afinar este proceso.

Deixa os valores predeterminados para as opcións desta páxina como se mostra na imaxe superior e pasa á transferencia de datos real pasando á páxina seguinte. Copiar os datos tardará un pouco. Neste punto, a páxina de progreso correspondente parecerá familiar:

[![Creando esquemas de destino no asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-create-schemata-page1-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-create-schemata-page1.png)

Unha vez que remate, pasa á páxina seguinte. Presentarase unha páxina de informe que resume todo o proceso:

[![Páxina de informes do asistente de migración de MySQL Workbench](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-report-page-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-report-page.png)

E iso debería ser. Fai clic no botón Finalizar para pechar o asistente de migración.

## Un pequeno paso de verificación

Agora que a base de datos Pagila foi migrada con éxito, vexamos os resultados. Abra unha páxina do Editor SQL asociada á súa instancia de MySQL Server e consulte a base de datos de Pagila. Podes probar algo como `SELECT * FROM pagila.actors` . Deberías conseguir algo así:

[![Consulta na base de datos de destino para verificar a migración](http://web.archive.org/web/20150317040350im_/http://wb.fabforce.eu/wp-content/uploads/migration-verification-query-800x552.png)](http://wb.fabforce.eu/wp-content/uploads/migration-verification-query.png)

## Conclusións

A estas alturas deberías ter unha idea bastante boa das capacidades do asistente de migración e deberías estar preparado para usalo nas túas propias migracións. A [documentación](http://dev.mysql.com/doc/workbench/en/wb-migration.html) oficial tamén está aí para ti e sempre podes facer calquera pregunta nos comentarios desta publicación ou no [foro oficial de migración](http://forums.mysql.com/list.php?104) . Vive moito e prospera!





---

Tradución aproximada @nunhes

https://dev.mysql.com/blog-archive/how-to-migrate-postgresql-databases-to-mysql-using-the-mysql-workbench-migration-wizard/

Publicado orixinalmente o 21 de novembro de 2012 por [Milosz Bodzek](https://dev.mysql.com/blog-archive/?author=Milosz Bodzek)
Categoría: [Workbench](https://dev.mysql.com/blog-archive/?cat=Workbench)
Etiquetas: [migración](https://dev.mysql.com/blog-archive/?tag=migration) , [tutorial](https://dev.mysql.com/blog-archive/?tag=tutorial)
