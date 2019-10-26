---
ms.openlocfilehash: 22ef5d0f91a4f61ca75203f677bcc14e91d77dc1
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394323"
---
### <a name="http-response-body-infrastructure-changes"></a><span data-ttu-id="c67c2-101">HTTP: cambios en la infraestructura del cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="c67c2-101">HTTP: Response body infrastructure changes</span></span>

<span data-ttu-id="c67c2-102">La infraestructura que respalda un cuerpo de respuesta HTTP ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="c67c2-102">The infrastructure backing an HTTP response body has changed.</span></span> <span data-ttu-id="c67c2-103">Si usa `HttpResponse` directamente, no debe realizar ningún cambio en el código.</span><span class="sxs-lookup"><span data-stu-id="c67c2-103">If you're using `HttpResponse` directly, you shouldn't need to make any code changes.</span></span> <span data-ttu-id="c67c2-104">Siga leyendo si ajusta o reemplaza `HttpResponse.Body`, o si accede a `HttpContext.Features`.</span><span class="sxs-lookup"><span data-stu-id="c67c2-104">Read further if you're wrapping or replacing `HttpResponse.Body` or accessing `HttpContext.Features`.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="c67c2-105">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="c67c2-105">Version introduced</span></span>

<span data-ttu-id="c67c2-106">3.0</span><span class="sxs-lookup"><span data-stu-id="c67c2-106">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="c67c2-107">Comportamiento anterior</span><span class="sxs-lookup"><span data-stu-id="c67c2-107">Old behavior</span></span>

<span data-ttu-id="c67c2-108">Había tres API asociadas al cuerpo de respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="c67c2-108">There were three APIs associated with the HTTP response body:</span></span>

- `IHttpResponseFeature.Body`
- `IHttpSendFileFeature.SendFileAsync`
- `IHttpBufferingFeature.DisableResponseBuffering`

#### <a name="new-behavior"></a><span data-ttu-id="c67c2-109">Comportamiento nuevo</span><span class="sxs-lookup"><span data-stu-id="c67c2-109">New behavior</span></span>

<span data-ttu-id="c67c2-110">Si reemplaza `HttpResponse.Body`, reemplaza todo el elemento `IHttpResponseBodyFeature` por un contenedor en torno a la secuencia dada mediante `StreamResponseBodyFeature` con el fin de proporcionar implementaciones predeterminadas para todas las API esperadas.</span><span class="sxs-lookup"><span data-stu-id="c67c2-110">If you replace `HttpResponse.Body`, it replaces the entire `IHttpResponseBodyFeature` with a wrapper around your given stream using `StreamResponseBodyFeature` to provide default implementations for all of the expected APIs.</span></span> <span data-ttu-id="c67c2-111">Al volver a establecer la secuencia original, este cambio se revierte.</span><span class="sxs-lookup"><span data-stu-id="c67c2-111">Setting back the original stream reverts this change.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="c67c2-112">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="c67c2-112">Reason for change</span></span>

<span data-ttu-id="c67c2-113">La motivación es combinar las API del cuerpo de la respuesta en una única interfaz de características nueva.</span><span class="sxs-lookup"><span data-stu-id="c67c2-113">The motivation is to combine the response body APIs into a single new feature interface.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="c67c2-114">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="c67c2-114">Recommended action</span></span>

<span data-ttu-id="c67c2-115">Use `IHttpResponseBodyFeature` donde anteriormente usaba `IHttpResponseFeature.Body`, `IHttpSendFileFeature` o `IHttpBufferingFeature`.</span><span class="sxs-lookup"><span data-stu-id="c67c2-115">Use `IHttpResponseBodyFeature` where you previously were using `IHttpResponseFeature.Body`, `IHttpSendFileFeature`, or `IHttpBufferingFeature`.</span></span>

#### <a name="category"></a><span data-ttu-id="c67c2-116">Categoría</span><span class="sxs-lookup"><span data-stu-id="c67c2-116">Category</span></span>

<span data-ttu-id="c67c2-117">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c67c2-117">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="c67c2-118">API afectadas</span><span class="sxs-lookup"><span data-stu-id="c67c2-118">Affected APIs</span></span>

- <xref:Microsoft.AspNetCore.Http.Features.IHttpBufferingFeature?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature.Body?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature?displayProperty=nameWithType>
 
<!-- 

#### Affected APIs

- `T:Microsoft.AspNetCore.Http.Features.IHttpBufferingFeature`
- `P:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature.Body`
- `T:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature`

-->