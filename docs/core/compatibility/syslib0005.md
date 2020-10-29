---
title: Advertencia SYSLIB0005
description: Obtenga información sobre la obsolescencia que genera la advertencia en tiempo de compilación SYSLIB0005.
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 8a9893d81c781335014c8b970c460b5a4241ed18
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92333179"
---
# <a name="syslib0005-the-global-assembly-cache-gac-is-not-supported"></a><span data-ttu-id="e4985-103">SYSLIB0005: no se admite la caché global de ensamblados (GAC)</span><span class="sxs-lookup"><span data-stu-id="e4985-103">SYSLIB0005: The global assembly cache (GAC) is not supported</span></span>

<span data-ttu-id="e4985-104">.NET Core y .NET 5.0 y versiones posteriores eliminan el concepto de caché global de ensamblados (GAC) presente en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e4985-104">.NET Core and .NET 5.0 and later versions eliminate the concept of the global assembly cache (GAC) that was present in .NET Framework.</span></span> <span data-ttu-id="e4985-105">Para ayudar a los desarrolladores a apartarse de estas API, algunas relacionadas con GAC se han marcado como obsoletas a partir de .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="e4985-105">To help steer developers away from these APIs, some GAC-related APIs are marked as obsolete, starting in .NET 5.0.</span></span> <span data-ttu-id="e4985-106">El uso de estas API genera una advertencia `SYSLIB0005` en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="e4985-106">Using these APIs generates warning `SYSLIB0005` at compile time.</span></span>

<span data-ttu-id="e4985-107">Las siguientes API relacionadas con GAC se han marcado como obsoletas:</span><span class="sxs-lookup"><span data-stu-id="e4985-107">The following GAC-related APIs are marked obsolete:</span></span>

- <xref:System.Reflection.Assembly.GlobalAssemblyCache?displayProperty=nameWithType>

  <span data-ttu-id="e4985-108">Las bibliotecas y aplicaciones no deben usar la API <xref:System.Reflection.Assembly.GlobalAssemblyCache> para realizar determinaciones sobre el comportamiento en tiempo de ejecución, ya que siempre devuelve `false` en .NET Core y .NET 5+.</span><span class="sxs-lookup"><span data-stu-id="e4985-108">Libraries and apps should not use the <xref:System.Reflection.Assembly.GlobalAssemblyCache> API to make determinations about run-time behavior, as it always returns `false` in .NET Core and .NET 5+.</span></span>

## <a name="workaround"></a><span data-ttu-id="e4985-109">Solución alternativa</span><span class="sxs-lookup"><span data-stu-id="e4985-109">Workaround</span></span>

<span data-ttu-id="e4985-110">Si la aplicación consulta a la propiedad <xref:System.Reflection.Assembly.GlobalAssemblyCache>, considere la posibilidad de eliminar la llamada.</span><span class="sxs-lookup"><span data-stu-id="e4985-110">If your application queries the <xref:System.Reflection.Assembly.GlobalAssemblyCache> property, consider removing the call.</span></span> <span data-ttu-id="e4985-111">Si usa el valor <xref:System.Reflection.Assembly.GlobalAssemblyCache> para elegir entre un flujo de "ensamblado en GAC" frente a un flujo de "ensamblado no en GAC" en tiempo de ejecución, vuelva a considerar si el flujo sigue teniendo sentido para una aplicación .NET 5+.</span><span class="sxs-lookup"><span data-stu-id="e4985-111">If you use the <xref:System.Reflection.Assembly.GlobalAssemblyCache> value to choose between an "assembly in the GAC"-flow vs. an "assembly not in the GAC"-flow at run time, reconsider whether the flow still makes sense for a .NET 5+ application.</span></span>

## <a name="see-also"></a><span data-ttu-id="e4985-112">Vea también</span><span class="sxs-lookup"><span data-stu-id="e4985-112">See also</span></span>

- [<span data-ttu-id="e4985-113">Caché global de ensamblados</span><span class="sxs-lookup"><span data-stu-id="e4985-113">Global assembly cache</span></span>](../../framework/app-domains/gac.md)