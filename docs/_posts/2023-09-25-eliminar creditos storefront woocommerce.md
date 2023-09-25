---
layout: single
toc: true
title: "Eliminar crédicos 'Construido con Storefront y WooCommerce'"
date: 2023-09-07
categories:
  - WordPress
  - WooCommerce filters
---

Para eliminar los créditos de la plantilla sin necesidad de usar plugins cambiarémos la lógica de renderizado del filtro `storefront_credit_link`. Para esto iremos a la sección `Apariencia` => `Editor de archivos de tema` dentro del administrador WordPress. Y busarás el archivo `inc/storefront-template-functions.php`

En este archivo buscarás la línea `if ( apply_filters( 'storefront_credit_link', true ) ) { ... }` y cambiarás el operador lógico `true`, por `false` para que no renderice los créditos