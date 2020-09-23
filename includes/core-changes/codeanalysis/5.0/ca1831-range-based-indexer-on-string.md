---
ms.openlocfilehash: 4e937f56f6315ce2abf76dd56989f4d2c4059f22
ms.sourcegitcommit: a69d548f90a03e105ee6701236c38390ecd9ccd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90065198"
---
### <a name="ca1831-use-asspan-instead-of-range-based-indexers-for-string"></a><span data-ttu-id="d629e-101">CA1831: Uso de AsSpan en lugar de indizadores basados en intervalos para una cadena</span><span class="sxs-lookup"><span data-stu-id="d629e-101">CA1831: Use AsSpan instead of Range-based indexers for string</span></span>

<span data-ttu-id="d629e-102">La regla [CA1831](/visualstudio/code-quality/ca1831) del analizador de código de .NET está habilitada de forma predeterminada a partir de .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="d629e-102">.NET code analyzer rule [CA1831](/visualstudio/code-quality/ca1831) is enabled, by default, starting in .NET 5.0.</span></span> <span data-ttu-id="d629e-103">Genera una advertencia de compilación para cualquier código donde se usa un indizador basado en <xref:System.Range> en una cadena, pero no está prevista ninguna copia.</span><span class="sxs-lookup"><span data-stu-id="d629e-103">It produces a build warning for any code where a <xref:System.Range>-based indexer is used on a string, but no copy was intended.</span></span>

#### <a name="change-description"></a><span data-ttu-id="d629e-104">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="d629e-104">Change description</span></span>

<span data-ttu-id="d629e-105">A partir de .NET 5.0, el SDK de .NET incluye [analizadores de código fuente de .NET](../../../../docs/fundamentals/productivity/code-analysis.md).</span><span class="sxs-lookup"><span data-stu-id="d629e-105">Starting in .NET 5.0, the .NET SDK includes [.NET source code analyzers](../../../../docs/fundamentals/productivity/code-analysis.md).</span></span> <span data-ttu-id="d629e-106">Varias de estas reglas están habilitadas de forma predeterminada, incluida la regla [CA1831](/visualstudio/code-quality/ca1831).</span><span class="sxs-lookup"><span data-stu-id="d629e-106">Several of these rules are enabled, by default, including [CA1831](/visualstudio/code-quality/ca1831).</span></span> <span data-ttu-id="d629e-107">Si el proyecto contiene código que infringe esta regla y está configurado para tratar las advertencias como errores, este cambio podría interrumpir la compilación.</span><span class="sxs-lookup"><span data-stu-id="d629e-107">If your project contains code that violates this rule and is configured to treat warnings as errors, this change could break your build.</span></span>

<span data-ttu-id="d629e-108">La regla CA1831 busca instancias en las que se usa un indizador basado en <xref:System.Range> en una cadena, pero donde no está prevista ninguna copia.</span><span class="sxs-lookup"><span data-stu-id="d629e-108">Rule CA1831 finds instances where a <xref:System.Range>-based indexer is used on a string, but no copy was intended.</span></span> <span data-ttu-id="d629e-109">Si el indizador basado en <xref:System.Range> se usa directamente en una cadena para generar una conversión implícita, se crea una copia innecesaria de la parte solicitada de la cadena.</span><span class="sxs-lookup"><span data-stu-id="d629e-109">If the <xref:System.Range>-based indexer is used directly on a string to produce an implicit cast, then an unnecessary copy of the requested portion of the string is created.</span></span> <span data-ttu-id="d629e-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d629e-110">For example:</span></span>

```csharp
ReadOnlySpan<char> slice = str[1..3];
```

<span data-ttu-id="d629e-111">CA1831 sugiere, en su lugar, el uso del indizador basado en <xref:System.Range> en un *intervalo* de la cadena.</span><span class="sxs-lookup"><span data-stu-id="d629e-111">CA1831 suggests using the <xref:System.Range>-based indexer on a *span* of the string, instead.</span></span> <span data-ttu-id="d629e-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d629e-112">For example:</span></span>

```csharp
ReadOnlySpan<char> slice = str.AsSpan()[1..3];
```

#### <a name="version-introduced"></a><span data-ttu-id="d629e-113">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="d629e-113">Version introduced</span></span>

<span data-ttu-id="d629e-114">5.0 (versión preliminar 8)</span><span class="sxs-lookup"><span data-stu-id="d629e-114">5.0 Preview 8</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="d629e-115">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="d629e-115">Recommended action</span></span>

- <span data-ttu-id="d629e-116">Para corregir el código y evitar asignaciones innecesarias, llame a <xref:System.MemoryExtensions.AsSpan(System.String)> o <xref:System.MemoryExtensions.AsMemory(System.String)> antes de usar el indizador basado en <xref:System.Range>.</span><span class="sxs-lookup"><span data-stu-id="d629e-116">To correct your code and avoid unnecessary allocations, call <xref:System.MemoryExtensions.AsSpan(System.String)> or <xref:System.MemoryExtensions.AsMemory(System.String)> before using the <xref:System.Range>-based indexer.</span></span> <span data-ttu-id="d629e-117">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d629e-117">For example:</span></span>

  ```csharp
  ReadOnlySpan<char> slice = str.AsSpan()[1..3];
  ```

- <span data-ttu-id="d629e-118">Si no quiere cambiar el código, puede deshabilitar la regla estableciendo su gravedad en `suggestion` o en `none`.</span><span class="sxs-lookup"><span data-stu-id="d629e-118">If you don't want to change your code, you can disable the rule by setting its severity to `suggestion` or `none`.</span></span> <span data-ttu-id="d629e-119">Para obtener más información, vea el artículo [Configuración de reglas de análisis de código](../../../../docs/fundamentals/productivity/configure-code-analysis-rules.md).</span><span class="sxs-lookup"><span data-stu-id="d629e-119">For more information, see [Configure code analysis rules](../../../../docs/fundamentals/productivity/configure-code-analysis-rules.md).</span></span>

- <span data-ttu-id="d629e-120">Para deshabilitar completamente el análisis de código, establezca `EnableNETAnalyzers` en `false` en el archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d629e-120">To disable code analysis completely, set `EnableNETAnalyzers` to `false` in your project file.</span></span> <span data-ttu-id="d629e-121">Para obtener más información, vea [EnableNETAnalyzers](../../../../docs/core/project-sdk/msbuild-props.md#enablenetanalyzers).</span><span class="sxs-lookup"><span data-stu-id="d629e-121">For more information, see [EnableNETAnalyzers](../../../../docs/core/project-sdk/msbuild-props.md#enablenetanalyzers).</span></span>

#### <a name="category"></a><span data-ttu-id="d629e-122">Categoría</span><span class="sxs-lookup"><span data-stu-id="d629e-122">Category</span></span>

<span data-ttu-id="d629e-123">Análisis de código</span><span class="sxs-lookup"><span data-stu-id="d629e-123">Code analysis</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="d629e-124">API afectadas</span><span class="sxs-lookup"><span data-stu-id="d629e-124">Affected APIs</span></span>

- <xref:System.Range?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Range`

-->