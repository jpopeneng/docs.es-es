---
title: Analizador de compatibilidad de plataformas
description: Un analizador de Roslyn que puede ayudar a detectar problemas de compatibilidad de plataformas en aplicaciones y bibliotecas multiplataforma.
author: buyaa-n
ms.date: 09/17/2020
ms.openlocfilehash: 4e842e5bbe90dd5006d9b27d0365f908b6441997
ms.sourcegitcommit: 1274a1a4a4c7e2eaf56b38da76ef7cec789726ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2020
ms.locfileid: "91406592"
---
# <a name="platform-compatibility-analyzer"></a><span data-ttu-id="421a7-103">Analizador de compatibilidad de plataformas</span><span class="sxs-lookup"><span data-stu-id="421a7-103">Platform compatibility analyzer</span></span>

<span data-ttu-id="421a7-104">Probablemente ha oído hablar del lema de "One .NET": una única plataforma unificada para crear cualquier tipo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="421a7-104">You've probably heard the motto of "One .NET": a single, unified platform for building any type of application.</span></span> <span data-ttu-id="421a7-105">El SDK de .NET 5.0 incluye ASP.NET Core, Entity Framework Core, WinForms, WPF, Xamarin y ML.NET, y agregará compatibilidad con más plataformas a lo largo del tiempo.</span><span class="sxs-lookup"><span data-stu-id="421a7-105">The .NET 5.0 SDK includes ASP.NET Core, Entity Framework Core, WinForms, WPF, Xamarin, and ML.NET, and will add support for more platforms over time.</span></span> <span data-ttu-id="421a7-106">.NET 5.0 se esfuerza por proporcionar una experiencia en la que no tenga que preocuparse de los distintos tipos de .NET, pero no intenta abstraer completamente el sistema operativo (SO) subyacente.</span><span class="sxs-lookup"><span data-stu-id="421a7-106">.NET 5.0 strives to provide an experience where you don't have to reason about the different flavors of .NET, but doesn't attempt to fully abstract away the underlying operating system (OS).</span></span> <span data-ttu-id="421a7-107">Podrá seguir llamando a las API específicas de la plataforma, por ejemplo, P/Invokes, WinRT o los enlaces de Xamarin para iOS y Android.</span><span class="sxs-lookup"><span data-stu-id="421a7-107">You'll continue to be able to call platform-specific APIs, for example, P/Invokes, WinRT, or the Xamarin bindings for iOS and Android.</span></span>

<span data-ttu-id="421a7-108">Pero el uso de API específicas de la plataforma en un componente significa que el código deje de funcionar en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="421a7-108">But using platform-specific APIs on a component means the code no longer works across all platforms.</span></span> <span data-ttu-id="421a7-109">Necesitábamos una manera de detectar esto en tiempo de diseño para que los desarrolladores obtuvieran diagnósticos al utilizar accidentalmente API específicas de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="421a7-109">We needed a way to detect this at design time so developers get diagnostics when they inadvertently use platform-specific APIs.</span></span> <span data-ttu-id="421a7-110">Para lograr este objetivo, .NET 5.0 presenta el [analizador de compatibilidad de plataformas](/visualstudio/code-quality/ca1416) y API complementarias para ayudar a los desarrolladores a identificar y usar las API específicas de la plataforma cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="421a7-110">To achieve this goal, .NET 5.0 introduces the [platform compatibility analyzer](/visualstudio/code-quality/ca1416) and complementary APIs to help developers identify and use platform-specific APIs where appropriate.</span></span>
<span data-ttu-id="421a7-111">Entre las nuevas API se incluyen:</span><span class="sxs-lookup"><span data-stu-id="421a7-111">The new APIs include:</span></span>

- <span data-ttu-id="421a7-112"><xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> para anotar las API como específicas de la plataforma y <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute> para anotar las API como no compatibles en un sistema operativo determinado.</span><span class="sxs-lookup"><span data-stu-id="421a7-112"><xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> to annotate APIs as being platform-specific and <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute> to annotate APIs as being unsupported on a particular OS.</span></span> <span data-ttu-id="421a7-113">Estos atributos pueden incluir opcionalmente el número de versión y ya se han aplicado a algunas API específicas de la plataforma en las bibliotecas .NET básicas.</span><span class="sxs-lookup"><span data-stu-id="421a7-113">These attributes can optionally include the version number, and have already been applied to some platform-specific APIs in the core .NET libraries.</span></span>
- <span data-ttu-id="421a7-114">Los métodos estáticos `Is<Platform>()` y `Is<Platform>VersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)` en la clase <xref:System.OperatingSystem?displayProperty=nameWithType> para llamar a API específicas de la plataforma de forma segura.</span><span class="sxs-lookup"><span data-stu-id="421a7-114">`Is<Platform>()` and `Is<Platform>VersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)` static methods in the <xref:System.OperatingSystem?displayProperty=nameWithType> class for safely calling platform-specific APIs.</span></span> <span data-ttu-id="421a7-115">Por ejemplo, se puede usar <xref:System.OperatingSystem.IsWindows?displayProperty=nameWithType> para proteger una llamada a una API específica de Windows y se puede usar <xref:System.OperatingSystem.IsWindowsVersionAtLeast%2A?displayProperty=nameWithType>() para proteger una llamada API específica de Windows con control de versiones.</span><span class="sxs-lookup"><span data-stu-id="421a7-115">For example, <xref:System.OperatingSystem.IsWindows?displayProperty=nameWithType> can be used to guard a call to a Windows-specific API, and <xref:System.OperatingSystem.IsWindowsVersionAtLeast%2A?displayProperty=nameWithType>() can be used to guard a versioned Windows-specific API call.</span></span> <span data-ttu-id="421a7-116">Consulte estos [ejemplos](#guard-platform-specific-apis-with-guard-methods) sobre cómo se pueden usar estos métodos como protecciones de las referencias de API específicas de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="421a7-116">See these [examples](#guard-platform-specific-apis-with-guard-methods) of how these methods can be used as guards of platform-specific API references.</span></span>

> [!TIP]
> <span data-ttu-id="421a7-117">El analizador de compatibilidad de plataformas actualiza y reemplaza la característica de [detección de problemas multiplataforma](../../standard/analyzers/api-analyzer.md#discover-cross-platform-issues) del [analizador de API de .NET](../../standard/analyzers/api-analyzer.md).</span><span class="sxs-lookup"><span data-stu-id="421a7-117">The platform compatibility analyzer upgrades and replaces the [Discovering cross-platform issues](../../standard/analyzers/api-analyzer.md#discover-cross-platform-issues) feature of the [.NET API analyzer](../../standard/analyzers/api-analyzer.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="421a7-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="421a7-118">Prerequisites</span></span>

<span data-ttu-id="421a7-119">El analizador de compatibilidad de plataformas es uno de los analizadores de calidad de código de Roslyn.</span><span class="sxs-lookup"><span data-stu-id="421a7-119">The platform compatibility analyzer is one of the Roslyn code quality analyzers.</span></span> <span data-ttu-id="421a7-120">A partir de .NET 5.0, estos analizadores se [incluyen con el SDK de .NET](../../fundamentals/productivity/code-analysis.md).</span><span class="sxs-lookup"><span data-stu-id="421a7-120">Starting in .NET 5.0, these analyzers are [included with the .NET SDK](../../fundamentals/productivity/code-analysis.md).</span></span> <span data-ttu-id="421a7-121">El analizador de compatibilidad de plataformas está habilitado de forma predeterminada solo para los proyectos que tienen como destino `net5.0` o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="421a7-121">The platform compatibility analyzer is enabled by default only for projects that target `net5.0` or a later version.</span></span> <span data-ttu-id="421a7-122">Sin embargo, puede [habilitar](/visualstudio/code-quality/ca1416.md#configurability) para los proyectos que tienen como destino otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="421a7-122">However, you can [enable](/visualstudio/code-quality/ca1416.md#configurability) it for projects that target other frameworks.</span></span>

## <a name="how-the-analyzer-determines-platform-dependency"></a><span data-ttu-id="421a7-123">Cómo determina el analizador la dependencia de plataformas</span><span class="sxs-lookup"><span data-stu-id="421a7-123">How the analyzer determines platform dependency</span></span>

- <span data-ttu-id="421a7-124">Una **API sin atributos** se considera que funciona en **todas las plataformas de sistema operativo**.</span><span class="sxs-lookup"><span data-stu-id="421a7-124">An **unattributed API** is considered to work on **all OS platforms**.</span></span>
- <span data-ttu-id="421a7-125">Una API marcada con `[SupportedOSPlatform("platform")]` solo se considera portable a la `platform` del sistema operativo especificado.</span><span class="sxs-lookup"><span data-stu-id="421a7-125">An API marked with `[SupportedOSPlatform("platform")]` is considered only portable to the specified OS `platform`.</span></span>
  - <span data-ttu-id="421a7-126">El atributo se puede aplicar varias veces para indicar la **compatibilidad con varias plataformas** (`[SupportedOSPlatform("windows"), SupportedOSPlatform("Android6.0")]`).</span><span class="sxs-lookup"><span data-stu-id="421a7-126">The attribute can be applied multiple times to indicate **multiple platform support** (`[SupportedOSPlatform("windows"), SupportedOSPlatform("Android6.0")]`).</span></span>
  - <span data-ttu-id="421a7-127">El analizador generará una **advertencia** si se hace referencia a las API específicas de la plataforma sin un **contexto de plataforma** adecuado:</span><span class="sxs-lookup"><span data-stu-id="421a7-127">The analyzer will produce a **warning** if platform-specific APIs are referenced without a proper **platform context**:</span></span>
    - <span data-ttu-id="421a7-128">**Produce una advertencia** si el proyecto no tiene como destino la plataforma compatible (por ejemplo, una llamada de API específica de Windows y los destinos del proyecto `<TargetFramework>net5.0-ios14.0</TargetFramework>`).</span><span class="sxs-lookup"><span data-stu-id="421a7-128">**Warns** if the project doesn't target the supported platform (for example, a Windows-specific API call and the project targets `<TargetFramework>net5.0-ios14.0</TargetFramework>`).</span></span>
    - <span data-ttu-id="421a7-129">**Advierte** si el proyecto tiene varios destinos (`<TargetFramework>net5.0</TargetFramework>`).</span><span class="sxs-lookup"><span data-stu-id="421a7-129">**Warns** if the project is multi-targeted (`<TargetFramework>net5.0</TargetFramework>`).</span></span>
    - <span data-ttu-id="421a7-130">**No advierte** si se hace referencia a la API específica de la plataforma dentro de un proyecto destinado a cualquiera de las **plataformas especificadas** (por ejemplo, para una llamada de API específica de Windows y el proyecto tiene como destino `<TargetFramework>net5.0-windows</TargetFramework>`).</span><span class="sxs-lookup"><span data-stu-id="421a7-130">**Doesn't warn** if the platform-specific API is referenced within a project that targets any of the **specified platforms** (for example, for a Windows-specific API call and the project targets `<TargetFramework>net5.0-windows</TargetFramework>`).</span></span>
    - <span data-ttu-id="421a7-131">**No advierte** si la llamada API específica de la plataforma está protegida por los métodos de comprobación de plataforma correspondientes (por ejemplo, `if(OperatingSystem.IsWindows())`).</span><span class="sxs-lookup"><span data-stu-id="421a7-131">**Doesn't warn** if the platform-specific API call is guarded by corresponding platform-check methods (for example, `if(OperatingSystem.IsWindows())`).</span></span>
    - <span data-ttu-id="421a7-132">**No advierte** si se hace referencia a la API específica de la plataforma desde el mismo contexto específico de la plataforma (**el sitio de llamada también tiene un atributo** con `[SupportedOSPlatform("platform")`).</span><span class="sxs-lookup"><span data-stu-id="421a7-132">**Doesn't warn** if the platform-specific API is referenced from the same platform-specific context (**call site also attributed** with `[SupportedOSPlatform("platform")`).</span></span>
- <span data-ttu-id="421a7-133">Una API marcada con `[UnsupportedOSPlatform("platform")]` se considera no compatible solo en la `platform` del sistema operativo especificado, pero se admite para todas las demás plataformas.</span><span class="sxs-lookup"><span data-stu-id="421a7-133">An API marked with `[UnsupportedOSPlatform("platform")]` is considered unsupported only on the specified OS `platform` but supported for all other platforms.</span></span>
  - <span data-ttu-id="421a7-134">El atributo se puede aplicar varias veces con diferentes plataformas, por ejemplo, `[UnsupportedOSPlatform("iOS"), UnsupportedOSPlatform("Android6.0")]`.</span><span class="sxs-lookup"><span data-stu-id="421a7-134">The attribute can be applied multiple times with different platforms, for example, `[UnsupportedOSPlatform("iOS"), UnsupportedOSPlatform("Android6.0")]`.</span></span>
  - <span data-ttu-id="421a7-135">El analizador genera una **advertencia** solo si la `platform` es efectiva para el sitio de llamada:</span><span class="sxs-lookup"><span data-stu-id="421a7-135">The analyzer produces a **warning** only if the `platform` is effective for the call site:</span></span>
    - <span data-ttu-id="421a7-136">**Advierte** si el proyecto tiene como destino la plataforma que se atribuye como no compatible (por ejemplo, si la API tiene el atributo `[UnsupportedOSPlatform("windows")]` y el sitio de llamada tiene como destino `<TargetFramework>net5.0-windows</TargetFramework>`).</span><span class="sxs-lookup"><span data-stu-id="421a7-136">**Warns** if the project targets the platform that's attributed as unsupported (for example, if the API is attributed with `[UnsupportedOSPlatform("windows")]` and the call site targets `<TargetFramework>net5.0-windows</TargetFramework>`).</span></span>
    - <span data-ttu-id="421a7-137">**Advierte** si el proyecto tiene varios destinos y el `platform` está incluido en el grupo de elementos predeterminado de [MSBuild `<SupportedPlatform>`](https://github.com/dotnet/sdk/blob/master/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.SupportedPlatforms.props), o la `platform` se incluye manualmente en el grupo de elementos `MSBuild` \<SupportedPlatform>:</span><span class="sxs-lookup"><span data-stu-id="421a7-137">**Warns** if the project is multi-targeted and the `platform` is included in the default [MSBuild `<SupportedPlatform>`](https://github.com/dotnet/sdk/blob/master/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.SupportedPlatforms.props) items group, or the `platform` is manually included within the `MSBuild` \<SupportedPlatform> items group:</span></span>

      ```XML
      <ItemGroup>
          <SupportedPlatform Include="platform" />
      </ItemGroup>
      ```

    - <span data-ttu-id="421a7-138">**No advierte** si está creando una aplicación que no tiene como destino la plataforma no compatible o que tiene varios destinos, y la plataforma no se incluye en el grupo de elementos predeterminados de [MSBuild `<SupportedPlatform>`](https://github.com/dotnet/sdk/blob/master/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.SupportedPlatforms.props).</span><span class="sxs-lookup"><span data-stu-id="421a7-138">**Doesn't warn** if you're building an app that doesn't target the unsupported platform or is multi-targeted and the platform is not included in the default [MSBuild `<SupportedPlatform>`](https://github.com/dotnet/sdk/blob/master/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.SupportedPlatforms.props) items group.</span></span>
- <span data-ttu-id="421a7-139">Se puede crear una instancia de ambos atributos con o sin números de versión como parte del nombre de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="421a7-139">Both attributes can be instantiated with or without version numbers as part of the platform name.</span></span>
  - <span data-ttu-id="421a7-140">Los números de versión están en el formato de `major.minor[.build[.revision]]`; `major.minor` es obligatorio y las partes `build` y `revision` son opcionales.</span><span class="sxs-lookup"><span data-stu-id="421a7-140">Version numbers are in the format of `major.minor[.build[.revision]]`; `major.minor` is required and the `build` and `revision` parts are optional.</span></span> <span data-ttu-id="421a7-141">Por ejemplo, "Windows 7.0" indica la versión 7.0 de Windows, pero "Windows" se interpreta como Windows 0.0.</span><span class="sxs-lookup"><span data-stu-id="421a7-141">For example, "Windows7.0" indicates Windows version 7.0, but "Windows" is interpreted as Windows 0.0.</span></span>

<span data-ttu-id="421a7-142">Para más información, consulte los [ejemplos de cómo funcionan los atributos y qué diagnósticos causan](#examples-of-how-the-attributes-work-and-what-diagnostics-they-produce).</span><span class="sxs-lookup"><span data-stu-id="421a7-142">For more information, see [examples of how the attributes work and what diagnostics they cause](#examples-of-how-the-attributes-work-and-what-diagnostics-they-produce).</span></span>

### <a name="advanced-scenarios-for-combining-attributes"></a><span data-ttu-id="421a7-143">Escenarios avanzados para combinar atributos</span><span class="sxs-lookup"><span data-stu-id="421a7-143">Advanced scenarios for combining attributes</span></span>

- <span data-ttu-id="421a7-144">Si hay una combinación de los atributos `[SupportedOSPlatform]` y `[UnsupportedOSPlatform]`, todos los atributos se agrupan por identificador de plataforma de sistema operativo:</span><span class="sxs-lookup"><span data-stu-id="421a7-144">If a combination of `[SupportedOSPlatform]` and `[UnsupportedOSPlatform]` attributes are present, all attributes are grouped by OS platform identifier:</span></span>
  - <span data-ttu-id="421a7-145">**Lista Solo compatibles**.</span><span class="sxs-lookup"><span data-stu-id="421a7-145">**Supported only list**.</span></span> <span data-ttu-id="421a7-146">Si la versión más antigua de cada plataforma de sistema operativo es un atributo `[SupportedOSPlatform]`, se considera que la API solo es compatible con las plataformas de la lista y no es compatible con todas las demás plataformas.</span><span class="sxs-lookup"><span data-stu-id="421a7-146">If the lowest version for each OS platform is a `[SupportedOSPlatform]` attribute, the API is considered to only be supported by the listed platforms and unsupported by all other platforms.</span></span> <span data-ttu-id="421a7-147">Los atributos de `[UnsupportedOSPlatform]` opcionales para cada plataforma solo pueden tener una versión más alta de la versión mínima admitida, lo que indica que la API se ha quitado a partir de la versión especificada.</span><span class="sxs-lookup"><span data-stu-id="421a7-147">The optional `[UnsupportedOSPlatform]` attributes for each platform can only have higher version of the minimum supported version, which denotes that the API is removed starting from the specified version.</span></span>

    ```csharp
    // The API only supported on Windows 8.0 and later, not supported for all other.
    // The API is removed/unsupported from version 10.0.19041.0.
    [SupportedOSPlatform("windows8.0")]
    [UnsupportedOSPlatform("windows10.0.19041.0")]
    public void ApiSupportedFromWindows80SupportFromCertainVersion();
    ```

  - <span data-ttu-id="421a7-148">**Lista Solo no compatibles**</span><span class="sxs-lookup"><span data-stu-id="421a7-148">**Unsupported only list**.</span></span> <span data-ttu-id="421a7-149">Si la versión más antigua de cada plataforma de sistema operativo es un atributo `[UnsupportedOSPlatform]`, se considera que la API no es compatible solo con las plataformas de la lista y es compatible con todas las demás plataformas.</span><span class="sxs-lookup"><span data-stu-id="421a7-149">If the lowest version for each OS platform is an `[UnsupportedOSPlatform]` attribute, then the API is considered to only be unsupported by the listed platforms and supported by all other platforms.</span></span> <span data-ttu-id="421a7-150">La lista podría tener el atributo `[SupportedOSPlatform]` con la misma plataforma pero con una versión superior, lo que indica que la API se admite a partir de esa versión.</span><span class="sxs-lookup"><span data-stu-id="421a7-150">The list could have `[SupportedOSPlatform]` attribute with the same platform but a higher version, which denotes that the API is supported starting from that version.</span></span>
  
    ```csharp
    // The API was unsupported on Windows until version 10.0.19041.0.
    // The API is considered supported everywhere else without constraints.
    [UnsupportedOSPlatform("windows")]
    [SupportedOSPlatform("windows10.0.19041.0")]
    public void ApiSupportedFromWindows8UnsupportFromWindows10();
    ```

  - <span data-ttu-id="421a7-151">**Lista de incoherentes**:</span><span class="sxs-lookup"><span data-stu-id="421a7-151">**Inconsistent list**.</span></span> <span data-ttu-id="421a7-152">Si la versión más antigua de algunas plataformas es `[SupportedOSPlatform]` mientras es `[UnsupportedOSPlatform]` para otras plataformas, se considera incoherente, lo que no se admite para el analizador.</span><span class="sxs-lookup"><span data-stu-id="421a7-152">If the lowest version for some platforms is `[SupportedOSPlatform]` while it is `[UnsupportedOSPlatform]` for other platforms, is considered inconsistent, which is not supported for the analyzer.</span></span>
  - <span data-ttu-id="421a7-153">Si las versiones más bajas de los atributos `[SupportedOSPlatform]` y `[UnsupportedOSPlatform]` son iguales, el analizador considera la plataforma como parte de la **lista Solo compatibles**.</span><span class="sxs-lookup"><span data-stu-id="421a7-153">If the lowest versions of `[SupportedOSPlatform]` and `[UnsupportedOSPlatform]` attributes are equal, the analyzer considers the platform as part of the **Supported only list**.</span></span>
- <span data-ttu-id="421a7-154">Los atributos de plataforma se pueden aplicar a tipos, miembros (métodos, campos, propiedades y eventos) y ensamblados con un nombre de plataforma o versión diferentes.</span><span class="sxs-lookup"><span data-stu-id="421a7-154">Platform attributes can be applied to types, members (methods, fields, properties, and events) and assemblies with different platform name and / or version.</span></span>
  - <span data-ttu-id="421a7-155">Los atributos aplicados en el `target` de nivel superior afectan a todos sus miembros y tipos.</span><span class="sxs-lookup"><span data-stu-id="421a7-155">Attributes applied at the top level `target` affect all of its members and types.</span></span>
  - <span data-ttu-id="421a7-156">Solo se aplican los atributos de nivel secundario si se adhieren a la regla de "las anotaciones secundarias pueden limitar la compatibilidad de plataformas, pero no pueden ampliarla".</span><span class="sxs-lookup"><span data-stu-id="421a7-156">Child level attributes only apply if they adhere to the rule "child annotations can narrow the platforms support, but they cannot widen it".</span></span>
    - <span data-ttu-id="421a7-157">Cuando el elemento primario tiene la lista **Solo compatibles**, los atributos de los miembros secundarios no podían agregar una nueva compatibilidad con la plataforma, ya que podría extenderse el soporte primario; una nueva compatibilidad con la plataforma solo se puede agregar al propio elemento primario.</span><span class="sxs-lookup"><span data-stu-id="421a7-157">When parent has **Supported only** list, then child member attributes could not add a new platform support as that would be extending parent support, a new platform support can only added to the parent itself.</span></span> <span data-ttu-id="421a7-158">Pero puede tener el atributo `Supported` para la misma plataforma con versiones posteriores, ya que esto limitará la compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="421a7-158">But it can have `Supported` attribute for same platform with later versions as that will narrow the support.</span></span> <span data-ttu-id="421a7-159">También puede tener el atributo `Unsupported` con la misma plataforma, lo que también limitará la compatibilidad con los elementos primarios.</span><span class="sxs-lookup"><span data-stu-id="421a7-159">Also it can have `Unsupported` attribute with same platform as that will also narrow parent support.</span></span>
    - <span data-ttu-id="421a7-160">Cuando el elemento primario tiene una lista **Solo no compatibles**, los atributos de miembro secundario podrían agregar una nueva compatibilidad con la plataforma, ya que sería la compatibilidad con el elemento primario de restricción, pero no puede tener el atributo `Supported` para la misma plataforma que en el elemento primario, lo que ampliaría el soporte primario.</span><span class="sxs-lookup"><span data-stu-id="421a7-160">When parent has **Unsupported only** list, then child member attributes could add a new platform support as that would be narrowing parent support, but it cannot have `Supported` attribute for same platform as in the parent, that would extend parent support.</span></span> <span data-ttu-id="421a7-161">La compatibilidad con la misma plataforma solo se puede agregar al nivel primario en el que se aplicó el atributo `Unsupported` original.</span><span class="sxs-lookup"><span data-stu-id="421a7-161">Support for the same platform can be added only to parent level where the original `Unsupported` attribute applied.</span></span>
  - <span data-ttu-id="421a7-162">Si se aplica `[SupportedOSPlatform("platformVersion")]` más de una vez para una API con el mismo nombre de `platform`, el analizador solo tiene en cuenta la que tiene la versión mínima.</span><span class="sxs-lookup"><span data-stu-id="421a7-162">If `[SupportedOSPlatform("platformVersion")]` is applied more than once for an API with the same `platform` name only the one with minimum version is considered by the analyzer.</span></span>
  - <span data-ttu-id="421a7-163">Si se aplica `[UnsupportedOSPlatform("platformVersion")]` más de dos veces para una API con el mismo nombre de `platform`, el analizador solo tiene en cuenta las dos con las versiones mínimas.</span><span class="sxs-lookup"><span data-stu-id="421a7-163">If `[UnsupportedOSPlatform("platformVersion")]` is applied more than twice for an API with the same `platform` name, only the two with earliest versions are considered by the analyzer.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="421a7-164">No se espera que una API admitida inicialmente pero no admitida (retirada) en una versión posterior pueda volver a admitirse en una versión incluso posterior.</span><span class="sxs-lookup"><span data-stu-id="421a7-164">An API that was supported initially but unsupported (removed) in a later version is not expected to get resupported in an even later version.</span></span>

### <a name="examples-of-how-the-attributes-work-and-what-diagnostics-they-produce"></a><span data-ttu-id="421a7-165">Ejemplos de cómo funcionan los atributos y qué diagnóstico producen</span><span class="sxs-lookup"><span data-stu-id="421a7-165">Examples of how the attributes work and what diagnostics they produce</span></span>

  ```csharp
  // An API supported only on Windows.
  [SupportedOSPlatform("Windows")]
  public void WindowsOnlyApi() { }

  // an API supported on Windows and Linux.
  [SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("Linux")]
  public void SupportedOnWindowsAndLinuxOnly() { }

  // an API only supported on Windows 8.0 and later, not supported for all other.
  // an API is removed/unsupported from version 10.0.19041.0.
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void ApiSupportedFromWindows8UnsupportFromWindows10() { }

  // an Assembly supported on Windows, the API added from version 10.0.19041.0.
  [assembly: SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("windows10.0.19041.0")]
  public void AssemblySupportedOnWindowsApiSupportedFromWindows10() { }

  public void Caller()
  {
      WindowsOnlyApi(); // warns: 'WindowsOnlyApi' is supported on 'windows'

      // warns: 'SupportedOnWindowsAndLinuxOnly' is supported on 'Windows'
      // warns: 'SupportedOnWindowsAndLinuxOnly' is supported on 'Linux'
      SupportedOnWindowsAndLinuxOnly();

      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 8.0 and later  
      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is unsupported on 'windows' 10.0.19041.0 and later
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 10.0.19041.0 and later
      // for same platform analyzer only warn for the latest version.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  // an API not supported on android but supported on all other.
  [UnsupportedOSPlatform("android")]  
  public void DoesNotWorkOnAndroid() { }

  // an API was unsupported on Windows until version 8.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  public void StartedWindowsSupportFromVersion8() { }

  // an API was unsupported on Windows until version8.0.
  // Then the API is removed (unsupported) from version 10.0.19041.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void StartedWindowsSupportFrom8UnsupportedFrom10() { }

  public void Caller2()
  {
      DoesNotWorkOnAndroid(); // warns 'DoesNotWorkOnAndroid' is unsupported on 'android'

      // warns:'StartedWindowsSupportFromVersion8' is unsupported on 'windows'  
      // warns:'StartedWindowsSupportFromVersion8' is supported on 'windows' 8.0 and later
      StartedWindowsSupportFromVersion8();

      // warns:'StartedWindowsSupportFrom8UnsupportedFrom10' is unsupported on 'windows'  
      // warns:'StartedWindowsSupportFrom8UnsupportedFrom10' is supported on 'windows' 8.0 and later
      // even there were 3 diagnostics found analyzer warn only for the first 2.
      StartedWindowsSupportFrom8UnsupportedFrom10();
  }
  ```

## <a name="handle-reported-warnings"></a><span data-ttu-id="421a7-166">Control de advertencias detectadas</span><span class="sxs-lookup"><span data-stu-id="421a7-166">Handle reported warnings</span></span>

<span data-ttu-id="421a7-167">La manera recomendada de tratar estos diagnósticos es asegurarse de que solo se llama a las API específicas de la plataforma cuando se ejecuta en una plataforma adecuada.</span><span class="sxs-lookup"><span data-stu-id="421a7-167">The recommended way to deal with these diagnostics is to make sure you only call platform-specific APIs when running on an appropriate platform.</span></span> <span data-ttu-id="421a7-168">A continuación se muestran las opciones que puede usar para solucionar las advertencias; elija lo que sea más adecuado para su situación:</span><span class="sxs-lookup"><span data-stu-id="421a7-168">Following are the options you can use to address the warnings; choose whichever is most appropriate for your situation:</span></span>

- <span data-ttu-id="421a7-169">**Proteja la llamada**.</span><span class="sxs-lookup"><span data-stu-id="421a7-169">**Guard the call**.</span></span> <span data-ttu-id="421a7-170">Para ello, puede llamar condicionalmente al código en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="421a7-170">You can achieve this by conditionally calling the code at run time.</span></span> <span data-ttu-id="421a7-171">Compruebe si se está ejecutando en una `Platform` deseada mediante el uso de uno de los métodos de comprobación de plataforma, por ejemplo, `OperatingSystem.Is<Platform>()` o `OperatingSystem.Is<Platform>VersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)`.</span><span class="sxs-lookup"><span data-stu-id="421a7-171">Check whether you’re running on a desired `Platform` by using one of platform-check methods, for example, `OperatingSystem.Is<Platform>()` or `OperatingSystem.Is<Platform>VersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)`.</span></span>

- <span data-ttu-id="421a7-172">**Marque el sitio de llamada como específico de la plataforma**.</span><span class="sxs-lookup"><span data-stu-id="421a7-172">**Mark the call site as platform-specific**.</span></span> <span data-ttu-id="421a7-173">También puede elegir marcar sus propias API como específicas de la plataforma y, por tanto, reenviar los requisitos a los autores de llamadas de forma eficaz.</span><span class="sxs-lookup"><span data-stu-id="421a7-173">You can also choose to mark your own APIs as being platform-specific, thus effectively just forwarding the requirements to your callers.</span></span> <span data-ttu-id="421a7-174">Marque el método o tipo contenedor o todo el ensamblado con los mismos atributos que la llamada dependiente de la plataforma a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="421a7-174">Mark the containing method or type or the entire assembly with the same attributes as the referenced platform-dependent call.</span></span> <span data-ttu-id="421a7-175">[Ejemplos](#mark-call-site-as-platform-specific).</span><span class="sxs-lookup"><span data-stu-id="421a7-175">[Examples](#mark-call-site-as-platform-specific).</span></span>

- <span data-ttu-id="421a7-176">**Aserción del sitio de llamada con comprobación de la plataforma**.</span><span class="sxs-lookup"><span data-stu-id="421a7-176">**Assert the call site with platform check**.</span></span> <span data-ttu-id="421a7-177">O bien, si no quiere sobrecargar una instrucción `if` adicional en tiempo de ejecución, llame a <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType> en su lugar:</span><span class="sxs-lookup"><span data-stu-id="421a7-177">If you don't want the overhead of an additional `if` statement at run time, use <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType>.</span></span> <span data-ttu-id="421a7-178">[Ejemplo](#assert-the-call-site-with-platform-check).</span><span class="sxs-lookup"><span data-stu-id="421a7-178">[Example](#assert-the-call-site-with-platform-check).</span></span>

- <span data-ttu-id="421a7-179">**Elimine el código**.</span><span class="sxs-lookup"><span data-stu-id="421a7-179">**Delete the code**.</span></span> <span data-ttu-id="421a7-180">Por lo general, no lo que desea porque significa que perderá fidelidad cuando los usuarios de Windows usen el código.</span><span class="sxs-lookup"><span data-stu-id="421a7-180">Generally not what you want because it means you lose fidelity when your code is used by Windows users.</span></span> <span data-ttu-id="421a7-181">En los casos en los que existe una alternativa multiplataforma, es probable que sea mejor usarlo en las API específicas de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="421a7-181">For cases where a cross-platform alternative exists, you’re likely better off using that over platform-specific APIs.</span></span>

- <span data-ttu-id="421a7-182">**Suprima la advertencia**.</span><span class="sxs-lookup"><span data-stu-id="421a7-182">**Suppress the warning**.</span></span> <span data-ttu-id="421a7-183">También puede suprimir la advertencia, ya sea mediante editor.config o `#pragma warning disable ca1416`.</span><span class="sxs-lookup"><span data-stu-id="421a7-183">You can also simply suppress the warning, either via editor.config or `#pragma warning disable ca1416`.</span></span> <span data-ttu-id="421a7-184">Sin embargo, esta opción debe ser el último recurso cuando se usan API específicas de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="421a7-184">However, this option should be a last resort when using platform-specific APIs.</span></span>

### <a name="guard-platform-specific-apis-with-guard-methods"></a><span data-ttu-id="421a7-185">Restricción de API específicas de la plataforma con métodos de restricción</span><span class="sxs-lookup"><span data-stu-id="421a7-185">Guard platform-specific APIs with guard methods</span></span>

<span data-ttu-id="421a7-186">El nombre de la plataforma del método de restricción debe coincidir con el nombre de la plataforma de API dependiente de la plataforma que llama.</span><span class="sxs-lookup"><span data-stu-id="421a7-186">The guard method's platform name should match with the calling platform-dependent API platform name.</span></span> <span data-ttu-id="421a7-187">Si la cadena de la plataforma de la API de llamada incluye la versión:</span><span class="sxs-lookup"><span data-stu-id="421a7-187">If the platform string of the calling API includes the version:</span></span>

- <span data-ttu-id="421a7-188">En el caso del atributo `[SupportedOSPlatform("platformVersion")]`, la `version` de la plataforma del método de restricción debe ser mayor o igual que la `Version` de la plataforma que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="421a7-188">For the `[SupportedOSPlatform("platformVersion")]` attribute, the guard method platform `version` should be greater than or equal to the calling platform's `Version`.</span></span>
- <span data-ttu-id="421a7-189">En el caso del atributo `[UnsupportedOSPlatform("platformVersion")]`, la `version` de la plataforma del método de restricción debe ser menor o igual que la `Version` de la plataforma que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="421a7-189">For the `[UnsupportedOSPlatform("platformVersion")]` attribute, the guard method's platform `version` should be less than or equal to the calling platform's `Version`.</span></span>

  ```csharp
  public void CallingSupportedOnlyApis() // Allow list calls
  {
      if (OperatingSystem.IsWindows())
      {
          WindowsOnlyApi(); // will not warn
      }

      if (OperatingSystem.IsLinux())
      {
          SupportedOnWindowsAndLinuxOnly(); // will not warn, within one of the supported context
      }

      // Can use &&, || logical operators to guard combined attributes
      if (OperatingSystem.IsWindowsVersionAtLeast(8) && !OperatingSystem.IsWindowsVersionAtLeast(10.0.19041)))
      {
          ApiSupportedFromWindows8UnsupportFromWindows10();
      }

      if (OperatingSystem.IsWindowsVersionAtLeast(10, 0, 1903))
      {
          AssemblySupportedOnWindowsApiSupportedFromWindows10(); // Only need to check latest supported version
      }
  }

  public void CallingUnsupportedApis()
  {
      if (!OperatingSystem.IsAndroid())
      {
          DoesNotWorkOnAndroid(); // will not warn
      }

      if (!OperatingSystem.IsWindows() || OperatingSystem.IsWindowsVersionAtLeast(8))
      {
          StartedWindowsSupportFromVersion8(); // will not warn
      }

      if (!OperatingSystem.IsWindows() || // supported all other platforms
         (OperatingSystem.IsWindowsVersionAtLeast(8) && !OperatingSystem.IsWindowsVersionAtLeast(10, 0, 1903)))
      {
          StartedWindowsSupportFrom8UnsupportedFrom10(); // will not warn
      }
  }
  ```

- <span data-ttu-id="421a7-190">Si necesita restringir un código destinado a netstandard o netcoreapp, donde las nuevas API de <xref:System.OperatingSystem> no están disponibles, se puede usar la API de <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform%2A?displayProperty=nameWithType> y el analizador respetará su cumplimiento.</span><span class="sxs-lookup"><span data-stu-id="421a7-190">if you need to guard a code that targets netstandard or netcoreapp where new <xref:System.OperatingSystem> APIs are not available <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform%2A?displayProperty=nameWithType> API can be used and will be respected by the analyzer.</span></span> <span data-ttu-id="421a7-191">Pero no es tan optimizado como las nuevas API agregadas en <xref:System.OperatingSystem>.</span><span class="sxs-lookup"><span data-stu-id="421a7-191">But it's not as optimized as the new APIs added in <xref:System.OperatingSystem>.</span></span> <span data-ttu-id="421a7-192">En caso de que la plataforma no se admita en la estructura <xref:System.Runtime.InteropServices.OSPlatform>, puede usar <xref:System.Runtime.InteropServices.OSPlatform.Create%2A?displayProperty=nameWithType> ("plataforma"), que también respeta el analizador.</span><span class="sxs-lookup"><span data-stu-id="421a7-192">In case the platform is not supported in <xref:System.Runtime.InteropServices.OSPlatform> struct, you can use <xref:System.Runtime.InteropServices.OSPlatform.Create%2A?displayProperty=nameWithType>("platform") which is also respected by the analyzer.</span></span>

  ```csharp
  public void CallingSupportedOnlyApis()
  {
      if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
      {
          SupportedOnWindowsAndLinuxOnly(); // will not warn
      }

      if (RuntimeInformation.IsOSPlatform(OSPlatform.Create("browser")))
      {
          ApiOnlySupportedOnBrowser(); // call of browser specific API
      }
  }
  ```

### <a name="mark-call-site-as-platform-specific"></a><span data-ttu-id="421a7-193">Marcado del sitio de llamada como específico de la plataforma</span><span class="sxs-lookup"><span data-stu-id="421a7-193">Mark call site as platform-specific</span></span>

<span data-ttu-id="421a7-194">Los nombres de la plataforma deben coincidir con la API dependiente de la plataforma que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="421a7-194">Platform names should match the calling platform-dependent API.</span></span> <span data-ttu-id="421a7-195">Si la cadena de la plataforma incluye una versión:</span><span class="sxs-lookup"><span data-stu-id="421a7-195">If the platform string includes a version:</span></span>

- <span data-ttu-id="421a7-196">En el caso del atributo `[SupportedOSPlatform("platformVersion")]`, la `version` de la plataforma del sitio de llamada debe ser mayor o igual que la `Version` de la plataforma que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="421a7-196">For the `[SupportedOSPlatform("platformVersion")]` attribute, the call site platform `version` should be greater than or equal to the calling platform's `Version`</span></span>
- <span data-ttu-id="421a7-197">En el caso del atributo `[UnsupportedOSPlatform("platformVersion")]`, la `version` de la plataforma del sitio de llamada debe ser menor o igual que la `Version` de la plataforma que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="421a7-197">For the `[UnsupportedOSPlatform("platformVersion")]` attribute, the call site platform `version` should be less than or equal to the calling platform's `Version`</span></span>

  ```csharp
  // an API supported only on Windows.
  [SupportedOSPlatform("windows")]
  public void WindowsOnlyApi() { }

  // an API supported on Windows and Linux.
  [SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("Linux")]
  public void SupportedOnWindowsAndLinuxOnly() { }

  // an API only supported on Windows 8.0 and later, not supported for all other.
  // an API is removed/unsupported from version 10.0.19041.0.
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void ApiSupportedFromWindows8UnsupportFromWindows10() { }

  // an Assembly supported on Windows, the API added from version 10.0.19041.0.
  [assembly: SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("windows10.0.19041.0")]
  public void AssemblySupportedOnWindowsApiSupportedFromWindows10() { }

  [SupportedOSPlatform("windows8.0")] // call site attributed Windows 8.0 or above.
  public void Caller()
  {
      WindowsOnlyApi(); // will not warn as call site is for Windows.

      // will not warn as call site is for Windows.
      SupportedOnWindowsAndLinuxOnly();

      // will not warn for the API's [SupportedOSPlatform("windows8.0")] attribute.
      // Warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is unsupported on 'windows' 10.0.19041.0 and later
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 10.0.19041.0 and later
      // as the call site version is lower than calling version.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  [SupportedOSPlatform("windows11.0")] // call site attributed with windows 11.0 or above.
  public void Caller2()
  {
      // Warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is unsupported on 'windows' 10.0.19041.0 and later
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // will not warn as call site version higher than calling API.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")] // call site supports Windows from version 8.0 to 10.0.19041.0.
  public void Caller3()
  {
      // will not warn as caller has exact same attributes.
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // Warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 10.0.19041.0 and later
      // As call site stopped support from that version.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  // an API not supported on Android but supported on all other.
  [UnsupportedOSPlatform("android")]  
  public void DoesNotWorkOnAndroid() { }

  // an API was unsupported on Windows until version 8.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  public void StartedWindowsSupportFromVersion8() { }

  // an API was unsupported on Windows until version8.0.
  // Then the API is removed (unsupported) from version 10.0.19041.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void StartedWindowsSupportFrom8UnsupportedFrom10() { }

  [UnsupportedOSPlatform("windows")] // Caller no support Windows for any version.
  public void Caller4()
  {
      DoesNotWorkOnAndroid(); // warns 'DoesNotWorkOnAndroid' is unsupported on 'Android'.

      // will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFromVersion8();

      // same, will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFrom8UnsupportedFrom10();
  }

  [UnsupportedOSPlatform("windows")]
  [UnsupportedOSPlatform("android")] // Caller not support Windows and Android for any version.
  public void Caller4()
  {
      DoesNotWorkOnAndroid(); // will not warn as call site not supports Android.

      // will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFromVersion8();

      // same, will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFrom8UnsupportedFrom10();
  }
  ```

### <a name="assert-the-call-site-with-platform-check"></a><span data-ttu-id="421a7-198">Aserción del sitio de llamada con comprobación de la plataforma</span><span class="sxs-lookup"><span data-stu-id="421a7-198">Assert the call-site with platform check</span></span>

<span data-ttu-id="421a7-199">Todas las comprobaciones condicionales que se usan en los [ejemplos de restricción de la plataforma](#guard-platform-specific-apis-with-guard-methods) también se pueden usar como condición para <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="421a7-199">All the conditional checks used in the [platform guard examples](#guard-platform-specific-apis-with-guard-methods) can also be used as the condition for <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType>.</span></span>

  ```csharp
  // An API supported only on Linux.
  [SupportedOSPlatform("linux")]
  public void LinuxOnlyApi() { }

  public void Caller()
  {
      Debug.Assert(OperatingSystem.IsLinux());

      LinuxOnlyApi(); // will not warn
  }
  ```

## <a name="see-also"></a><span data-ttu-id="421a7-200">Consulte también</span><span class="sxs-lookup"><span data-stu-id="421a7-200">See also</span></span>

- [<span data-ttu-id="421a7-201">Nombres de plataformas de destino en .NET 5</span><span class="sxs-lookup"><span data-stu-id="421a7-201">Target Framework Names in .NET 5</span></span>](https://github.com/dotnet/designs/blob/master/accepted/2020/net5/net5.md)
- [<span data-ttu-id="421a7-202">Anotación de API específicas de la plataforma y detección de su uso</span><span class="sxs-lookup"><span data-stu-id="421a7-202">Annotating platform-specific APIs and detecting its use</span></span>](https://github.com/dotnet/designs/blob/master/accepted/2020/platform-checks/platform-checks.md)
- [<span data-ttu-id="421a7-203">Anotación de las API como no compatibles en plataformas específicas</span><span class="sxs-lookup"><span data-stu-id="421a7-203">Annotating APIs as unsupported on specific platforms</span></span>](https://github.com/dotnet/designs/blob/master/accepted/2020/platform-exclusion/platform-exclusion.md)
- [<span data-ttu-id="421a7-204">Analizador de compatibilidad de plataformas CA1416</span><span class="sxs-lookup"><span data-stu-id="421a7-204">CA1416 Platform compatibility analyzer</span></span>](/visualstudio/code-quality/ca1416)
- [<span data-ttu-id="421a7-205">Analizador de API en .NET</span><span class="sxs-lookup"><span data-stu-id="421a7-205">.NET API analyzer</span></span>](../../standard/analyzers/api-analyzer.md)