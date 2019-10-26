---
ms.openlocfilehash: a916af91670dc9c5ceb2ff759cd8ae308fb2c2dc
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394135"
---
### <a name="kestrel-connection-adapters-removed"></a><span data-ttu-id="07e6f-101">Kestrel: se han quitado los adaptadores de conexión</span><span class="sxs-lookup"><span data-stu-id="07e6f-101">Kestrel: Connection adapters removed</span></span>

<span data-ttu-id="07e6f-102">Como parte de la migración de las API de "pubternal" a `public`, se ha quitado de Kestrel el concepto de un elemento `IConnectionAdapter`.</span><span class="sxs-lookup"><span data-stu-id="07e6f-102">As part of the move to move "pubternal" APIs to `public`, the concept of an `IConnectionAdapter` was removed from Kestrel.</span></span> <span data-ttu-id="07e6f-103">Los adaptadores de conexión se han reemplazado por el middleware de conexión, que es similar al middleware HTTP en la canalización de ASP.NET Core, pero para conexiones de nivel inferior.</span><span class="sxs-lookup"><span data-stu-id="07e6f-103">Connection adapters are being replaced with connection middleware (similar to HTTP middleware in the ASP.NET Core pipeline, but for lower-level connections).</span></span> <span data-ttu-id="07e6f-104">HTTPS y el registro de conexiones se han movido de los adaptadores de conexión al middleware de conexión.</span><span class="sxs-lookup"><span data-stu-id="07e6f-104">HTTPS and connection logging have moved from connection adapters to connection middleware.</span></span> <span data-ttu-id="07e6f-105">Estos métodos de extensión deberían seguir funcionando sin problemas, pero los detalles de implementación han cambiado.</span><span class="sxs-lookup"><span data-stu-id="07e6f-105">Those extension methods should continue to work seamlessly, but the implementation details have changed.</span></span>

<span data-ttu-id="07e6f-106">Para obtener más información, vea [aspnet/AspNetCore#11412](https://github.com/aspnet/AspNetCore/pull/11412).</span><span class="sxs-lookup"><span data-stu-id="07e6f-106">For more information, see [aspnet/AspNetCore#11412](https://github.com/aspnet/AspNetCore/pull/11412).</span></span> <span data-ttu-id="07e6f-107">También puede consultar [aspnet/AspNetCore#11475](https://github.com/aspnet/AspNetCore/issues/11475).</span><span class="sxs-lookup"><span data-stu-id="07e6f-107">For discussion, see [aspnet/AspNetCore#11475](https://github.com/aspnet/AspNetCore/issues/11475).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="07e6f-108">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="07e6f-108">Version introduced</span></span>

<span data-ttu-id="07e6f-109">3.0</span><span class="sxs-lookup"><span data-stu-id="07e6f-109">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="07e6f-110">Comportamiento anterior</span><span class="sxs-lookup"><span data-stu-id="07e6f-110">Old behavior</span></span>

<span data-ttu-id="07e6f-111">Los componentes de extensibilidad de Kestrel se creaban mediante el uso de `IConnectionAdapter`.</span><span class="sxs-lookup"><span data-stu-id="07e6f-111">Kestrel extensibility components were created using `IConnectionAdapter`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="07e6f-112">Comportamiento nuevo</span><span class="sxs-lookup"><span data-stu-id="07e6f-112">New behavior</span></span>

<span data-ttu-id="07e6f-113">Los componentes de extensibilidad de Kestrel se crean como [middleware](https://github.com/aspnet/AspNetCore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f).</span><span class="sxs-lookup"><span data-stu-id="07e6f-113">Kestrel extensibility components are created as [middleware](https://github.com/aspnet/AspNetCore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f).</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="07e6f-114">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="07e6f-114">Reason for change</span></span>

<span data-ttu-id="07e6f-115">Este cambio está pensado para proporcionar una arquitectura de extensibilidad más flexible.</span><span class="sxs-lookup"><span data-stu-id="07e6f-115">This change is intended to provide a more flexible extensibility architecture.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="07e6f-116">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="07e6f-116">Recommended action</span></span>

<span data-ttu-id="07e6f-117">Convierta las implementaciones de `IConnectionAdapter` para usar el nuevo patrón de middleware, tal como se muestra [aquí](https://github.com/aspnet/AspNetCore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f).</span><span class="sxs-lookup"><span data-stu-id="07e6f-117">Convert any implementations of `IConnectionAdapter` to use the new middleware pattern as shown [here](https://github.com/aspnet/AspNetCore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f).</span></span>

#### <a name="category"></a><span data-ttu-id="07e6f-118">Categoría</span><span class="sxs-lookup"><span data-stu-id="07e6f-118">Category</span></span>

<span data-ttu-id="07e6f-119">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07e6f-119">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="07e6f-120">API afectadas</span><span class="sxs-lookup"><span data-stu-id="07e6f-120">Affected APIs</span></span>

`Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal.IConnectionAdapter`

<!-- 

#### Affected APIs

`T:Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal.IConnectionAdapter`

-->