---
title: 'Cómo: Enumerar los almacenes de almacenamiento aislado'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- enumerating stores
- data storage using isolated storage, enumerating stores
- storing data using isolated storage, enumerating stores
- stores, enumerating
- isolated storage, enumerating stores
- data stores, enumerating
ms.assetid: 0fcf279a-f241-48f0-8034-2e3d331f1fcb
ms.openlocfilehash: 732f121e6b1977a960cab207f8d56cd2a551383c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291869"
---
# <a name="how-to-enumerate-stores-for-isolated-storage"></a>Cómo: Enumerar los almacenes de almacenamiento aislado
Puede enumerar todos los almacenes aislados para el usuario actual con el uso del método estático <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A?displayProperty=nameWithType>. Este método adopta un valor <xref:System.IO.IsolatedStorage.IsolatedStorageScope> y devuelve un enumerador <xref:System.IO.IsolatedStorage.IsolatedStorageFile>. Para enumerar los almacenes, debe tener el permiso <xref:System.Security.Permissions.IsolatedStorageFilePermission> que especifica el valor <xref:System.Security.Permissions.IsolatedStorageContainment.AdministerIsolatedStorageByUser>. Si se llama al método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A> con el valor <xref:System.IO.IsolatedStorage.IsolatedStorageScope.User>, devuelve una matriz de objetos <xref:System.IO.IsolatedStorage.IsolatedStorageFile> definidos para el usuario actual.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo de código siguiente se obtiene un almacén aislado por usuario y ensamblado, se crean unos cuantos archivos y se recuperan los archivos mediante el método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A>.  
  
 [!code-csharp[Conceptual.IsolatedStorage#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source2.cs#2)]
 [!code-vb[Conceptual.IsolatedStorage#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source2.vb#2)]  
  
## <a name="see-also"></a>Vea también

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [Almacenamiento aislado](isolated-storage.md)
