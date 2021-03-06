---
ms.openlocfilehash: 450bfc56c99a3df9be71be2ef7df6e4e12d4ed76
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497903"
---
### <a name="a-concurrentdictionary-serialized-in-net-framework-45-with-netdatacontractserializer-cannot-be-deserialized-by-net-framework-451-or-452"></a>Un elemento ConcurrentDictionary serializado en .NET Framework 4.5 con NetDataContractSerializer no se puede deserializar en .NET Framework 4.5.1 ni 4.5.2

#### <a name="details"></a>Detalles

Debido a los cambios internos en el tipo, los objetos <xref:System.Collections.Concurrent.ConcurrentDictionary%602> que se serializaron con .NET Framework 4.5 mediante <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName> no se pueden deserializar en .NET Framework 4.5.1 o 4.5.2. Tenga en cuenta que a la inversa sí funciona (la serialización con .NET Framework 4.5.x y la deserialización con .NET Framework 4.5). Del mismo modo, la serialización entre todas las versiones 4.x funciona con .NET Framework 4.6. La serialización y deserialización con una única versión de .NET Framework no se ve afectada.

#### <a name="suggestion"></a>Sugerencia

Si es necesario serializar y deserializar <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName> entre .NET Framework 4.5 y 4.5.1/4.5.2, se debe usar un serializador alternativo como <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=fullName> o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> en lugar de <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName>. Como alternativa, dado que este problema se corrige en .NET Framework 4.6, se podría resolver mediante la actualización a esa versión de .NET Framework.

| Nombre    | Valor       |
|:--------|:------------|
| Ámbito   |Secundaria|
|Versión|4.5.1|
|Tipo|Tiempo de ejecución|

#### <a name="affected-apis"></a>API afectadas

No detectable a través del análisis de la API.

<!--

#### Affected APIs

Not detectable via API analysis.

-->
