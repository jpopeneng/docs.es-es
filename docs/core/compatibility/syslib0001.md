---
title: Advertencia SYSLIB0001
description: Obtenga información sobre las obsolescencias que generan la advertencia en tiempo de compilación SYSLIB0001.
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 58c16879b7d91598ea0848bb0ba95f8fa0200cfe
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92333182"
---
# <a name="syslib0001-the-utf-7-encoding-is-insecure"></a>SYSLIB0001: la codificación UTF-7 no es segura

La codificación UTF-7 ya no se usa de forma generalizada en la mayoría de las aplicaciones, y muchas especificaciones ahora [prohíben su uso](https://security.stackexchange.com/a/68609/3573) en el intercambio. En ocasiones, también se [usa como vector de ataque](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=utf-7) en aplicaciones que no anticipan el encuentro de datos codificados en UTF-7. Microsoft advierte contra el uso de <xref:System.Text.UTF7Encoding?displayProperty=nameWithType>, porque no proporciona detección de errores.

Por tanto, las siguientes API se han marcado como obsoletas a partir de .NET 5.0. El uso de estas API genera una advertencia `SYSLIB0001` en tiempo de compilación.

- Propiedad<xref:System.Text.Encoding.UTF7?displayProperty=nameWithType>
- Constructores <xref:System.Text.UTF7Encoding.%23ctor%2A>

## <a name="workarounds"></a>Soluciones alternativas

- Si usa <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> o <xref:System.Text.UTF7Encoding> en su propio protocolo o formato de archivo:

  Cambie al uso de <xref:System.Text.Encoding.UTF8?displayProperty=nameWithType> o <xref:System.Text.UTF8Encoding>. UTF-8 es un estándar del sector y es compatible con numerosos lenguajes, sistemas operativos y entornos de ejecución. El uso de UTF-8 facilita el mantenimiento futuro del código y hace que sea más interoperable con el resto del ecosistema.

- Si va a comparar una instancia de <xref:System.Text.Encoding> con <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType>:

  Considere mejor la posibilidad de realizar una comprobación con la página de códigos UTF-7 conocidos, que es `65000`. De esta manera, evita la advertencia y también se controlan algunos casos extremos, como, por ejemplo, si alguien llamó a `new UTF7Encoding()` o incluyó el tipo en una subclase.

  ```csharp
  void DoSomething(Encoding enc)
  {
      // Don't perform the check this way.
      // It produces a warning and misses some edge cases.
      if (enc == Encoding.UTF7)
      {
          // Encoding is UTF-7.
      }

      // Instead, perform the check this way.
      if (enc != null && enc.CodePage == 65000)
      {
          // Encoding is UTF-7.
      }
  }
  ```

## <a name="see-also"></a>Vea también

- [Las rutas de acceso al código UTF-7 están obsoletas](3.1-5.0.md#utf-7-code-paths-are-obsolete)
