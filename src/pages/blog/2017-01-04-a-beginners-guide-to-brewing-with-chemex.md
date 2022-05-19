---
templateKey: blog-post
title: Transformación de datos con XML e XSL
date: 2022-01-04T15:04:00.000Z
description: >-
  Converter un conxunto de datos procedentes dunha base de datos XML (xa sexa
  nun só documento ou varios documentos interconectados) notros formatos máis
  axeitados para presentar (táboas, listas) ou expoñer información (paragrafos)
tags:
  - XML
  - XSL
---
# Transformación de datos con XML e XSL

- **Obxectivo:** 
     Converter un conxunto de datos procedentes dunha base de datos XML (xa sexa nun só documento ou varios documentos interconectados) notros formatos máis axeitados para presentar (táboas, listas) ou expoñer información (paragrafos)

:eye: *Debido a cambios recentes nas políticas de seguridade dos navegadores web, estes xa non poden executar código XSL de arquivos locais. Para ver as transformacións deberás executar os arquivos dende un servidor - \*AMP, LiveServer, Golive, Server2000,...-* 

[TOC]

## Introducción

Supón que un compañeiro de traballo chámate por teléfono pedíndoche que o substitúas nun seminario sobre as relacións cos escravos no Novo Mundo, e con só un día de marxe. Decides recoller unha selección de fontes primarias para traballar na clase, atopas algunhas páxinas web e algúns libros con bos materiais, pero escanealas todas ou copiar e pegar a información nun novo documento vaiche levar demasiado tempo. Ademais, o estilo da bibliografía difire e as citas son inconsistentes, polo que comezas a preguntarte se xuntar todo ese material ten algún sentido. Atopas unha páxina web permite descargar unha [XML](https://es.wikipedia.org/wiki/Extensible_Markup_Language) de todo o material, pero hai tantos rexistros e tantos [metadatos](https://es.wikipedia.org/wiki/Metadatos) que non é doado atopar rapidamente a información que desexas.

Ou quizais... atopaches unha edición antiga de Inscriptions of Roman Tripolitania (1952) e queres facer unha análise estatística da aparición de certas frases en determinados contextos. Afortunadamente, o King's College London publicou unha [versión dixital do texto](https://irt.kcl.ac.uk/irt2009/) con imaxes, traducións e información sobre a localización das inscricións. Podes explorar o material usando a función "Buscar na páxina" do teu navegador, pero editar a información no formato necesario para a análise leva tempo.

Imaxina agora que inicias un novo proxecto consistente no estudo dun catálogo de poxas de libros do século XVII. Comezas rexistrando os detalles da publicación e a listaxe da poxa nun documento de Word ou Excel. Un mes despois o vicerreitor da túa universidade convídache a dar unha charla. O decano da túa facultade propón que fagas algunhas diapositivas ou notas para facilitar a comprensión do proxecto. Xa tes algunhas conclusións preliminares, pero os datos están espallados en varios lugares e unificar o formato da información require máis tempo do que tes.

Nas tres situacións descritas, saber como funciona XML e [XSL](https://es.wikipedia.org/wiki/Extensible_Stylesheet_Language) aforraríache tempo e esforzo. 

Vexamos pois como converter un conxunto de datos históricos dunha base de datos XML [^1] (un só documento ou varios documentos interconectados) noutros formatos máis axeitados para presentar (táboas, listas) ou expor información (parágrafos). Se desexa filtrar a información contida nunha base de datos ou engadir cabeceiras ou paxinación, XSL ofrece aos historiadores a posibilidade de reconfigurar os datos para acomodar os cambios ás necesidades de investigación ou publicación.

Este artigo abrangue os seguintes aspectos:

- **Editores**: ferramentas necesarias para crear follas de estilo XSL

- **Procesadores**: ferramentas necesarias para aplicar instrucións da folla de estilo XSL aos arquivos XML

- **Elixir e preparar datos XML**: Como conectar a base de datos con instrucións de transformación XSL

O titorial tamén serve como guía para crear as transformacións máis comúns:

- **Imprimir valores**: como imprimir ou presentar datos
- **repeticións for-each(en bucle)**: como presentar datos específicos en cada un dos obxectos ou rexistros existentes
- **ordear resultados**: como presentar os datos nunha determinada orde
- **Filtrar resultados**: como seleccionar que obxectos ou rexistros se van presentar

## Que é XML?

A linguaxe de marcado extensible ( e**X**tensible **M**arkup **L**anguage , xeralmente abreviado como "XML") é ***un método moi flexible de codificación e estruturación de datos***. A diferenza marcado de hipertexto (abreviado como "HTML"), que ten un vocabulario predeterminado, XML é extensible; é dicir, pódese ampliar para incluír as etiquetas necesarias para, por exemplo, identificar tantas seccións e subseccións como queira.

Unha base de datos pode estar formada por un ou máis documentos XML cunha estrutura básica. Cada sección do arquivo está contida nun elemento , é dicir, unha categoría ou nome co que se identifica o tipo de datos que se manexan. Así, coma se fosen matrioshkas, cada nivel de elementos está contido noutro. O elemento <raiz> é precisamente iso: a raíz do documento, é dicir, o elemento que contén o resto dos elementos e entidades; considérase, ao contido del, fillo nel. Do mesmo xeito, o elemento que contén un elemento fillo chámase pai ( parent ). Por exemplo:

```xml
<raiz>
  <pai>
    <fillo></fillo>
  </pai>
</raiz>
```

(Ter en conta que estes nomes - ``raiz``, ``pai`` e ``fillo``- son completamente arbitrarios. Poderiamos chamalos de calquera outra maneira. O importante aquí son as relacións de continencia.)

Segundo as regras da nosa base de datos, os **elementos** poden ter valores (texto ou numérico) ou un determinado número de elementos fillos.

```xml
<raiz>
  <pai>
    <fillo_1>valor</fillo_1>
    <fillo_2>valor</fillo_2>
    <fillo_3>valor</fillo_3>
  </pai>
</raiz>
```

Tamén poden ter **atributos** , algo así como os **metadatos** do elemento. Os atributos axudan a distinguir, por exemplo, entre diferentes tipos de valores sen ter que crear un novo tipo de elemento. Por exemplo:

```xml
<raiz>
  <nome>
    <apelido>García</apelido>
    <nome tipo="formal">Cristina</nome>
    <nome tipo="informal">Cris</nome>
  </nome>
</raiz>
```

Se tes acceso a unha *base de datos* XML ou queres almacenar datos nunha, podes usar **XSL** para ordenar, filtrar e presentar información de (case) todas as formas imaxinables. Por exemplo, podes abrir un arquivo XML como Word (.docx) ou Excel (.xslx), inspeccionalo e, a continuación, eliminar a información engadida por Microsoft por defecto, como a localización xeográfica do creador do documento. Se queres saber máis sobre XML, recomendámosche ler unha explicación máis detallada da súa estrutura e uso en humanidades na Text Encoding Initiative .

## Que é XSL?

A linguaxe de follas de estilo extensible ( e**X**tensible **S**tylesheet **L**anguage , abreviado como "XSL") é o complemento natural de XML. En xeral, proporciona instrucións de procesamento. En certo modo, poderiamos dicir que XSL é análogo ás follas de estilo (Cascading Stylesheets para abreviar "CSS") necesarias para renderizar arquivos HTML. Ambos os dous idiomas permítenche transformar texto plano nun formato de texto enriquecido, así como determinar o seu deseño e aparencia tanto en pantalla como na impresión, sen ter que alterar os arquivos orixinais. Nun nivel máis avanzado, tamén permiten ordenar e filtrar información segundo criterios específicos e crear ou ver outros datos derivados do arquivo orixinal.

***Ao separar os datos (XML) das instrucións de procesamento (XSL), é posible refinar e modificar a presentación sen correr o risco de corromper a estrutura dos arquivos.*** Ademais, podemos crear máis dunha folla de estilo , para que se utilicen dependendo do obxectivo para transformar un único arquivo fonte. Na práctica, isto significa que só tes que actualizar os datos nun só lugar e despois exportar diferentes documentos[^2]. 



###### NOTAS:

[^1]: Nota do tradutor: Segundo [Wikipedia](https://es.wikipedia.org/wiki/Base_de_datos_XML), unha base de datos XML é un programa “que da persistencia a datos almacenados en formato XML. Estes datos poden ser interrogados, exportados e serializados”. Poden distinguirse dous tipos: bases de datos habilitadas (por exemplo, unha basee de datos relacional clásica que acepta XML como formato de entrada e saída) e bases de datos nativas (é dicir, que utilizan documentos XML como unidade de almacenamento) como [eXist](https://exist-db.org/exist/apps/homepage/index.html) ou [BaseX](https://basex.org/). Neste tutorial, sin embargo, a autora, a miúdo, non distingue entre o continente (a programación) e o contido da base de datos XML (os documentos). [↩](#fnref:1)

[^2]:


---
*- ref.:*
- *https://programminghistorian.org/es/lecciones/transformacion-datos-xml-xsl*

#### Bibliografía:

- Hunter, David et al. Beginning XML, 4a ed. Indianapolis, IN: Wiley, 2007.
- Kay, Michael, XSLT 2.0 and XPATH 2.0: Programmer’s Reference. Indianapolis, IN: Wiley, 2011.
- Kelly, David J. XSLT Jumpstarter: Level the Learning Curve and Put Your XML to Work. Raleigh, NC: Peloria Press, May 2015.
- Mangano, Sal. XSLT Cookbook, 2a ed. Sebstopol, CA: O’Reilly, 2006.
- Riley, Jenn. Understanding Metadata: What is Metadata, and What is For? NISO, 2017. Publicación web.
- Tennison, Jeni. Beginning XSLT 2.0. From Novice to Professional. Nueva York: Apress, 2005.
- Tidwell, Doug. XSLT, 2a ed. Sebstopol, CA: O’Reilly, 2008.

