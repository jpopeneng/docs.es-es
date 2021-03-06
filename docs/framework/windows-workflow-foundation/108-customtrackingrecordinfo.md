---
title: 108 - CustomTrackingRecordInfo
ms.date: 03/30/2017
ms.assetid: 5bee501e-4e00-42cd-b949-e88009c3d4e8
ms.openlocfilehash: 7398693208ca09aa1f9f56ef5354b86bb67f26a9
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62052812"
---
# <a name="108---customtrackingrecordinfo"></a>108 - CustomTrackingRecordInfo
## <a name="properties"></a>Propiedades  
  
|||  
|-|-|  
|Id.|108|  
|Palabras clave|UserEvents, EndToEndMonitoring, Troubleshooting, HealthMonitoring, WFTracking|  
|Nivel|Información|  
|Canal|Microsoft-Windows-Application Server-Applications/Analytic|  
  
## <a name="description"></a>Descripción  
 El participante de seguimiento de ETW emite este evento cuando una actividad dentro de una instancia de flujo de trabajo emite CustomTrackingRecord con información de nivel.  
  
## <a name="message"></a>Mensaje  
 TrackRecord = CustomTrackingRecord, InstanceID = %1, RecordNumber=%2, EventTime=%3, Name=%4, ActivityName=%5, ActivityId=%6, ActivityInstanceId=%7, ActivityTypeName=%8, Data=%9, Annotations=%10, ProfileName = %11  
  
## <a name="details"></a>Detalles  
  
|Nombre del elemento de datos|Tipo del elemento de datos|Descripción|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|El id. de instancia del flujo de trabajo.|  
|RecordNumber|xs:long|El número de secuencia del registro emitido.|  
|EventTime|xs:dateTime|La hora en UTC cuando se emitió el evento.|  
|Name|xs:string|El nombre de la clase CustomTrackingRecord.|  
|ActivityName|xs:string|El nombre de la actividad que emitió la clase CustomTrackingRecord.|  
|ActivityId|xs:string|El id. de la actividad que emitió la clase CustomTrackingRecord.|  
|ActivityInstance|xs:string|El id. de instancia de la actividad que emitió la clase CustomTrackingRecord.|  
|ActivityTypeName|xs:string|El nombre de la actividad que emitió la clase CustomTrackingRecord.|  
|Datos|xs:string|Los datos a los que se realizó el seguimiento con este evento.  Los valores se almacenan en un elemento xml con el formato \<elementos >\< nombre del elemento = "dataName" Type = "> dataValue\</artículo >\</Items >.  Si no se realizara el seguimiento de ningún dato, la cadena contendría \<elementos / >. El tamaño del evento ETW está limitado por el tamaño de búfer de ETW o la carga útil máxima para un evento ETW. Si el tamaño del evento supera los límites de ETW, el evento se trunca quitando las anotaciones y reemplazando el valor de datos con \<elementos >... \</Items >.  Los tipos siguientes están almacenados como valores posibles ya que los devuelve ToString(); string,char,bool,int,short,long,uint,ushort,ulong,System.Single,float,double,System.Guid,System.DateTimeOffset,System.DateTime.  Todos los demás tipos se serializan con System.Runtime.Serialization.NetDataContractSerializer.|  
|Anotaciones|xs:string|Las anotaciones que se agregaron a este evento.  Los valores se almacenan en un elemento xml con el formato \<elementos >\< nombre del elemento = "annotationName" Type = "> annotationValue\</artículo >\</Items >.  Si se especifica ninguna anotación, la cadena contendría \<elementos / >. El tamaño del evento ETW está limitado por el tamaño de búfer de ETW o la carga útil máxima para un evento ETW. Si el tamaño del evento supera los límites de ETW, el evento se trunca quitando las anotaciones y reemplazando el valor de anotación con \<elementos >... \</Items >.|  
|ProfileName|xs:string|El nombre o el perfil de seguimiento que dio como resultado que se emitiera este evento.|  
|HostReference|xs:string|En el caso de los servicios hospedados en web, este campo identifica de manera única el servicio en la jerarquía web.  Su formato se define como ' ruta de acceso Virtual de sitio Web de nombre de la aplicación&#124;ruta de acceso Virtual del servicio&#124;NombreServicio ' ejemplo: ' Default Web Site/CalculatorApplication&#124;/CalculatorService.svc&#124;CalculatorService'|  
|AppDomain|xs:string|La cadena devuelta por AppDomain.CurrentDomain.FriendlyName.|
