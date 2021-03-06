---
ms.openlocfilehash: 48d2f1b5fa2eced30d3c9fb65804268904f11ab8
ms.sourcegitcommit: a69d548f90a03e105ee6701236c38390ecd9ccd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90065074"
---
### <a name="unicode-category-changed-for-some-latin-1-characters"></a>Categoría Unicode modificada para algunos caracteres del alfabeto latino 1

Los métodos <xref:System.Char> ahora devuelven la categoría Unicode correcta para los caracteres del alfabeto latino 1. La categoría coincide con la del estándar Unicode.

#### <a name="change-description"></a>Descripción del cambio

En versiones anteriores de .NET, los métodos <xref:System.Char> usaban una lista fija de categorías Unicode para los caracteres del alfabeto latino 1. Pero el estándar Unicode ha cambiado las categorías de algunos de estos caracteres desde que se implementaron esas API, lo que ha creado una discrepancia. Además, también existía una discrepancia entre las API <xref:System.Char> y <xref:System.Globalization.CharUnicodeInfo>, que siguen el estándar Unicode. En .NET 5.0 y versiones posteriores, los métodos <xref:System.Char> usan y devuelven la categoría Unicode que coincide con el estándar Unicode para todos los caracteres.

En la tabla siguiente se muestran los caracteres cuyas categorías Unicode han cambiado en .NET 5.0:

| Carácter    | Categoría Unicode<br>en versiones anteriores de .NET Core | Categoría Unicode<br>en .NET 5.0 y versiones posteriores |
|:------------:|:---------------------------------------------:|:--------------------------------------------------:|
| § (\u00a7)   | `OtherSymbol`                                 | `OtherPunctuation`                                 |
| ª (\u00aa)   | `LowercaseLetter`                             | `OtherLetter`                                      |
| SHY (\u00ad) | `DashPunctuation`                             | `Format`                                           |
| ¶ (\u00b6)   | `OtherSymbol`                                 | `OtherPunctuation`                                 |
| º (\u00ba)   | `LowercaseLetter`                             | `OtherLetter`                                      |

#### <a name="version-introduced"></a>Versión introducida

.NET 5.0 RC1

#### <a name="recommended-action"></a>Acción recomendada

Si tiene código que obtiene la categoría de caracteres Unicode mediante la clase <xref:System.Char> y supone que la categoría nunca cambia, puede que tenga que actualizarlo.

#### <a name="reason-for-change"></a>Motivo del cambio

Este cambio se ha llevado a cabo para que las categorías devueltas por el tipo <xref:System.Char> sean coherentes con el estándar Unicode y el tipo <xref:System.Globalization.CharUnicodeInfo>.

#### <a name="category"></a>Categoría

- Bibliotecas de Core .NET
- Globalización

#### <a name="affected-apis"></a>API afectadas

- <xref:System.Char.GetUnicodeCategory%2A?displayProperty=fullName>
- <xref:System.Char.IsLetter%2A?displayProperty=fullName>
- <xref:System.Char.IsPunctuation%2A?displayProperty=fullName>
- <xref:System.Char.IsSymbol%2A?displayProperty=fullName>
- <xref:System.Char.IsLower%2A?displayProperty=fullName>

Además, cualquier clase que dependa de <xref:System.Char> para obtener la categoría de caracteres Unicode, por ejemplo, <xref:System.Text.RegularExpressions.Regex>, se ve afectada por este cambio.

<!--

#### Affected APIs

- `Overload:System.Char.GetUnicodeCategory`
- `Overload:System.Char.IsLetter`
- `Overload:System.Char.IsPunctuation`
- `Overload:System.Char.IsSymbol`
- `Overload:System.Char.IsLower`

-->
