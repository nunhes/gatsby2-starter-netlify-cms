---
templateKey: blog-post
title: Bases de datos XML. Características e tipos
date: 2022-05-13T07:35:34.705Z
description: >-
  As bases de datos XML son unha das moitas posibilidades que existen na
  actualidade para almacenar información. Pero, cales son as peculiaridades
  destas bases de datos? En que se diferencian do modelo de bases de datos
  relacionais? 
tags:
  - XML
  - bases de datos
---


[TOC]

## **Que é unha base de datos XML?**

Unha base de **datos XML** é un método de almacenamento de información que permite almacenar datos en **formato XML** . Normalmente consisten en bases de [datos](https://ayudaleyprotecciondatos.es/bases-de-datos/) de tipo documento e permiten organizar e exportar datos XML.

En realidade deberíamos falar de bases de **datos con XML** . Noutras palabras, XML non é un xestor de bases de datos, senón un metalenguaxe co que se almacenan os datos. Grazas a XML, podes crear regras e relacións semánticas sinxelas que che permitan definir e organizar a forma en que se estruturan os datos.



A función principal do uso de **XML nunha base** de datos é proporcionar unha linguaxe estruturada que sexa máis fácil de ler e comprender. Isto dá algunhas vantaxes, principalmente cando se trata de intercambiar información. Por outra banda, a partir de bases de datos XML pódense xerar ficheiros con outras extensións para ser compartidos e a información máis fácil de ler, por exemplo documentos PDF.

## **Características das bases de datos XML**

**As bases de datos** XML teñen unha serie de **características** que as diferencian do resto:

- Usan a linguaxe XML ou Extensible Markup Language, un metalinguaxe deseñado polo W3C para almacenar datos en forma lexible.
- A información está ordenada xerarquicamente.
- Os datos incorporan etiquetas e marcas que definen os datos, é dicir, explican que é cada conxunto de datos e que significa.
- As bases de datos que usan XML poden albergar distintos tipos de datos.
- Os datos preséntanse por orde. É dicir, nun **documento XML** a orde na que aparecen os elementos é a orde dos datos, o que non ocorre nas bases de datos relacionais baseadas en rexistros e columnas.

## **Vantaxes e inconvenientes das bases de datos creadas con XML**

A creación de bases de datos en linguaxe XML ten diversas vantaxes e inconvenientes que veremos a continuación.

### Vantaxes

As principais **vantaxes das bases de datos XML** son as seguintes:

- Son fáciles de ler.
- Os documentos XML son fáciles de procesar.
- É unha linguaxe que ten unha gran compatibilidade con SGML
- A linguaxe XML é sinxela de estruturar para que as distintas partes dun documento se poidan diferenciar facilmente.
- Pódese importar e exportar a outras aplicacións, programas e formatos.
- Para aqueles que non dominan por completo XML, hai analizadores que che permiten corrixir erros de síntese, como XML Copy Editor.
- Os documentos pódense actualizar simplemente engadindo novas etiquetas,

### **Desvantaxes**

Pola súa banda, tamén é necesario mencionar algúns dos **inconvenientes das bases de datos XML** :

- Son máis lentos e requiren que os datos sexan comprimidos para funcionar máis rápido.
- As buscas son máis lentas que nunha base de datos relacional, e que deben organizarse mediante texto e etiquetas.
- Existe unha certa limitación respecto dos xestores de bases de datos que poden usar a linguaxe XML.
- As bases de datos creadas con documentos XML non están preparadas para o almacenamento de información a gran escala.
- Pode haber problemas para garantir a seguridade dos datos. Por exemplo, non se poden configurar para definir quen pode actualizar, engadir ou eliminar información da base de datos.

## **Tipos base de datos XML**

Existen dous tipos principais de bases de datos que usan XML: bases de datos XML activadas e bases de datos XML nativas.

### **XML Base de datos activada (XML- enabled)**

Trátase de bases de datos relacionais, nas que a información se almacena en **táboas** . As táboas divídense en filas, que conteñen os rexistros, e columnas, que conteñen os campos.

Unha base de [datos relacional](https://ayudaleyprotecciondatos.es/bases-de-datos/relacional/) habilitada para XML permite **obter consultas en formato XML** , polo que se enmarcan dentro da denominada base de datos habilitada para XML.

### **Base de datos XML nativa (NXD)**

A diferenza das bases de datos relacionais, este tipo de base de datos XML nativa non ten campos nin táboas, senón que almacena documentos XML. É por iso que tamén se chaman **bases de datos centradas en documentos** .

Neste sentido, estas bases de datos almacenan e recuperan documentos do mesmo xeito que o faría XML. É necesario empregar modelos capaces de construír expresións que poidan procesar un documento XML.O máis utilizado é **Xpath** , aínda que hai outros como XML Infoset.

## **Como facer unha base de datos xml?**

Un dos mellores métodos para aprender a facer unha base de datos en formato XML é consultar algún dos titoriais que existen en Internet. Aquí vos deixamos un vídeo tutorial para crear unha base de datos XML con BaseX:



## **Principais diferenzas entre bases de datos XML e bases de datos relacionais**

XML e as bases de datos relacionais difieren en varios aspectos, que vemos a continuación:

- Por unha banda, nas bases de datos que utilizan XML, a información organízase xerarquicamente. Non obstante, nos datos relacionais, os datos preséntanse en función de relacións lóxicas.
- Nas bases de datos NoSQL que usan a linguaxe XML, os datos conteñen etiquetas que describen os propios datos. En cambio, isto non ocorre nas bases de datos relacionais.
- Por outra banda, os mesmos documentos nunha base de datos XML poden conter distintos tipos de datos. Porén, nas bases de datos relacionais, a tipoloxía dos datos vén marcada pola definición de cada columna. É dicir, os datos que aparecen na mesma columna son sempre do mesmo tipo.
- Por último, nunha base de datos XML os datos preséntanse ordenados. Non obstante, no modelo relacional, a orde das filas non está definida, a non ser que se inclúa unha etiqueta de orde nunha das columnas.

## **Exemplo de bases de datos en XML**

Por exemplo, imaxina que queres crear unha base de datos XML que inclúa información de contacto. Na seguinte táboa mostrámosche un exemplo no que se engadiron dous contactos, cada un con información sobre o nome, a empresa e o teléfono.

*<?xml version=»1.0″?>*
*<contact-info>*

<contacto1><name>Pepito Pérez</name><company>Pepito SL</company><phone>645 236 XXX</phone></contacto1><contacto2><name>Fulano García</name><company> Fulanito SL</company><phone>619 8396 XXX</phone></contacto2>

*</contact-info>*

Con este exemplo chegamos ao final do artigo. Podes atopar moita máis información sobre este tema na nosa sección de artigos de base de datos.
