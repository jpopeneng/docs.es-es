---
title: ICorDebugModule3 (Interfaz)
ms.date: 03/30/2017
api_name:
- ICorDebugModule3
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule3
helpviewer_keywords:
- ICorDebugModule3 interface [.NET Framework debugging]
ms.assetid: 0b69f945-263a-4e11-8512-89d27f6ea296
topic_type:
- apiref
ms.openlocfilehash: 69fd3e2df4a4eafe91cc025f28e1387cc443ea04
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83212313"
---
# <a name="icordebugmodule3-interface"></a>ICorDebugModule3 (Interfaz)
Crea un lector de símbolos para un módulo dinámico.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
interface ICorDebugModule3 : IUnknown  
{  
    HRESULT CreateReaderForInMemorySymbols  
      (  
      [in] REFIID riid,  
      [out][iid_is(riid)] void **  ppObj  
      );  
};  
```  
  
## <a name="methods"></a>Métodos  
  
|Método|Descripción|  
|------------|-----------------|  
|[ICorDebugModule3::CreateReaderForInMemorySymbols (Método)](icordebugmodule3-createreaderforinmemorysymbols-method.md)|Crea un lector de símbolos (normalmente la [interfaz ISymUnmanagedReader](../diagnostics/isymunmanagedreader-interface.md)) para un módulo dinámico.|  
  
## <a name="remarks"></a>Observaciones  
 Esta interfaz extiende lógicamente las interfaces "ICorDebugModule" y "ICorDebugModule2".  
  
> [!NOTE]
> Esta interfaz no admite que se la llame de forma remota, ya sea entre procesos o entre equipos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../get-started/system-requirements.md).  
  
 **Encabezado:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versiones:** 4,5, 4, 3,5 SP1
  
## <a name="see-also"></a>Consulte también

- [ICorDebugRemoteTarget (Interfaz)](icordebugremotetarget-interface.md)
- [ICorDebug (Interfaz)](icordebug-interface.md)

- [Interfaces para depuración](debugging-interfaces.md)
