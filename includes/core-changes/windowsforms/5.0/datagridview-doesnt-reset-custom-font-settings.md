---
ms.openlocfilehash: 9c84c9f844a2a7efb9633c11634b069ef6b6033b
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804872"
---
### <a name="datagridview-no-longer-resets-fonts-for-customized-cell-styles"></a>DataGridView ya no restablece las fuentes para los estilos de celda personalizados

Cuando la fuente ambiente cambia, <xref:System.Windows.Forms.DataGridViewElement.DataGridView> ya no restablece las fuentes predeterminadas de estilo de celda para que coincidan con la fuente ambiente si se ha personalizado la fuente del estilo de celda.

#### <a name="change-description"></a>Descripción del cambio

En versiones anteriores de .NET, si la fuente ambiente cambia, <xref:System.Windows.Forms.DataGridViewElement.DataGridView> restablece e invalida las fuentes definidas por el usuario en las propiedades <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> y <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle>.

A partir de .NET 5.0, si se configuran las opciones de fuente en las propiedades <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> o <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle>, esas opciones se conservan incluso cuando cambia la fuente ambiente. Para cualquiera de estas propiedades en las que no se personalice la fuente, la fuente cambiará para que coincida con la configuración de la fuente ambiente.

#### <a name="reason-for-change"></a>Motivo del cambio

Con el [cambio de la fuente predeterminada en .NET Core 3.0](../../../../docs/core/compatibility/winforms.md#default-control-font-changed-to-segoe-ui-9-pt), también cambian los valores de fuente predeterminados para los distintos estilos de celda. Este comportamiento no es el deseado para las aplicaciones que se basan en el estilo personalizado en sus controles <xref:System.Windows.Forms.DataGridViewElement.DataGridView> y ha impedido la migración de estas aplicaciones de .NET Framework a .NET 5.0.

#### <a name="version-introduced"></a>Versión introducida

.NET 5.0 RC2

#### <a name="recommended-action"></a>Acción recomendada

No es necesario que realice ninguna acción. Pero si ha personalizado la fuente en las propiedades <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> o <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle>, y quiere que la fuente coincida con la fuente ambiente, establezca <xref:System.Windows.Forms.DataGridViewCellStyle.Font?displayProperty=nameWithType> en `null` para cada propiedad.

#### <a name="category"></a>Categoría

- Windows Forms

#### <a name="affected-apis"></a>API afectadas

- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Windows.Forms.DataGridView.DefaultCellStyle`
- `P:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle`
- `P:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle`

-->
