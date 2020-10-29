---
ms.openlocfilehash: 584014572ab799e1e3ab2f02c9fbf4fe6b0c17e7
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804936"
---
### <a name="blazor-updated-browser-support"></a><span data-ttu-id="2bbd2-101">Blazor: compatibilidad con exploradores actualizada</span><span class="sxs-lookup"><span data-stu-id="2bbd2-101">Blazor: Updated browser support</span></span>

<span data-ttu-id="2bbd2-102">En ASP.NET Core 5.0 se presentan [nuevas características de Blazor](https://github.com/dotnet/aspnetcore/issues/21514) y algunas son incompatibles con los exploradores más antiguos.</span><span class="sxs-lookup"><span data-stu-id="2bbd2-102">ASP.NET Core 5.0 introduces [new Blazor features](https://github.com/dotnet/aspnetcore/issues/21514), some of which are incompatible with older browsers.</span></span> <span data-ttu-id="2bbd2-103">La lista de exploradores admitidos por Blazor en ASP.NET Core 5.0 se ha actualizado en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="2bbd2-103">The list of browsers supported by Blazor in ASP.NET Core 5.0 has been updated accordingly.</span></span>

<span data-ttu-id="2bbd2-104">Para obtener información, vea la incidencia de GitHub [n.º 26475 (dotnet/aspnetcore)](https://github.com/dotnet/aspnetcore/issues/26475).</span><span class="sxs-lookup"><span data-stu-id="2bbd2-104">For discussion, see GitHub issue [dotnet/aspnetcore#26475](https://github.com/dotnet/aspnetcore/issues/26475).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="2bbd2-105">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="2bbd2-105">Version introduced</span></span>

<span data-ttu-id="2bbd2-106">5.0</span><span class="sxs-lookup"><span data-stu-id="2bbd2-106">5.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2bbd2-107">Comportamiento anterior</span><span class="sxs-lookup"><span data-stu-id="2bbd2-107">Old behavior</span></span>

<span data-ttu-id="2bbd2-108">Blazor Server es compatible con Microsoft Internet Explorer 11 con polyfill suficientes.</span><span class="sxs-lookup"><span data-stu-id="2bbd2-108">Blazor Server supports Microsoft Internet Explorer 11 with sufficient polyfills.</span></span> <span data-ttu-id="2bbd2-109">Blazor Server y Blazor WebAssembly también funcionan en [Microsoft Edge (versión anterior)](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy).</span><span class="sxs-lookup"><span data-stu-id="2bbd2-109">Blazor Server and Blazor WebAssembly are also functional in [Microsoft Edge Legacy](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2bbd2-110">Comportamiento nuevo</span><span class="sxs-lookup"><span data-stu-id="2bbd2-110">New behavior</span></span>

<span data-ttu-id="2bbd2-111">Blazor Server en ASP.NET Core 5.0 no es compatible con Microsoft Internet Explorer 11.</span><span class="sxs-lookup"><span data-stu-id="2bbd2-111">Blazor Server in ASP.NET Core 5.0 isn't supported with Microsoft Internet Explorer 11.</span></span> <span data-ttu-id="2bbd2-112">Blazor Server y Blazor WebAssembly no son totalmente funcionales en Microsoft Edge (versión anterior).</span><span class="sxs-lookup"><span data-stu-id="2bbd2-112">Blazor Server and Blazor WebAssembly aren't fully functional in Microsoft Edge Legacy.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="2bbd2-113">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="2bbd2-113">Reason for change</span></span>

<span data-ttu-id="2bbd2-114">Las nuevas características de Blazor en ASP.NET Core 5.0 no son compatibles con estos exploradores más antiguos, cuyo uso está disminuyendo.</span><span class="sxs-lookup"><span data-stu-id="2bbd2-114">New Blazor features in ASP.NET Core 5.0 are incompatible with these older browsers, and use of these older browsers is diminishing.</span></span> <span data-ttu-id="2bbd2-115">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="2bbd2-115">For more information, see the following resources:</span></span>

* [<span data-ttu-id="2bbd2-116">La compatibilidad de Windows con Microsoft Edge (versión anterior) también finaliza el 9 de marzo de 2021</span><span class="sxs-lookup"><span data-stu-id="2bbd2-116">Windows support for Microsoft Edge Legacy is also ending on March 9, 2021</span></span>](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy)
* [<span data-ttu-id="2bbd2-117">Las aplicaciones y servicios de Microsoft 365 finalizarán la compatibilidad con Microsoft Internet Explorer 11 el 17 de agosto de 2021</span><span class="sxs-lookup"><span data-stu-id="2bbd2-117">Microsoft 365 apps and services will end support for Microsoft Internet Explorer 11 by August 17, 2021</span></span>](/lifecycle/announcements/m365-ie11-microsoft-edge-legacy)

#### <a name="recommended-action"></a><span data-ttu-id="2bbd2-118">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="2bbd2-118">Recommended action</span></span>

<span data-ttu-id="2bbd2-119">Actualice estos exploradores antiguos al [nuevo Microsoft Edge basado en Chromium](https://www.microsoft.com/edge).</span><span class="sxs-lookup"><span data-stu-id="2bbd2-119">Upgrade from these older browsers to the [new, Chromium-based Microsoft Edge](https://www.microsoft.com/edge).</span></span> <span data-ttu-id="2bbd2-120">En el caso de las aplicaciones de Blazor que necesiten admitir estos exploradores antiguos, use ASP.NET Core 3.1.</span><span class="sxs-lookup"><span data-stu-id="2bbd2-120">For Blazor apps that need to support these older browsers, use ASP.NET Core 3.1.</span></span> <span data-ttu-id="2bbd2-121">La lista de exploradores admitidos para Blazor en ASP.NET Core 3.1 no ha cambiado y está documentada en [Plataformas admitidas](/aspnet/core/blazor/supported-platforms?view=aspnetcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="2bbd2-121">The supported browsers list for Blazor in ASP.NET Core 3.1 hasn't changed and is documented at [Supported platforms](/aspnet/core/blazor/supported-platforms?view=aspnetcore-3.1).</span></span>

#### <a name="category"></a><span data-ttu-id="2bbd2-122">Categoría</span><span class="sxs-lookup"><span data-stu-id="2bbd2-122">Category</span></span>

<span data-ttu-id="2bbd2-123">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2bbd2-123">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="2bbd2-124">API afectadas</span><span class="sxs-lookup"><span data-stu-id="2bbd2-124">Affected APIs</span></span>

<span data-ttu-id="2bbd2-125">None</span><span class="sxs-lookup"><span data-stu-id="2bbd2-125">None</span></span>

<!--

#### Affected APIs

Not detectable via API analysis

-->