---
title: Procedimiento para ver el contenido de un ensamblado
ms.date: 08/20/2019
helpviewer_keywords:
- assembly manifest, viewing information
- Ildasm.exe
- MSIL Disassembler
- assemblies [.NET Framework], viewing contents
- viewing assembly information
- MSIL
- viewing MSIL information
ms.assetid: fb7baaab-4c0d-47ad-8fd3-4591cf834709
author: rpetrusha
ms.author: ronpet
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 8f27afafde0b83dfe886d218f3148d8ff07b30cb
ms.sourcegitcommit: 7b1ce327e8c84f115f007be4728d29a89efe11ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70972526"
---
# <a name="how-to-view-assembly-contents"></a><span data-ttu-id="afbb3-102">Procedimiento para ver el contenido de un ensamblado</span><span class="sxs-lookup"><span data-stu-id="afbb3-102">How to: View assembly contents</span></span>
<span data-ttu-id="afbb3-103">Se puede usar [Ildasm.exe (Desensamblador de IL)](../../framework/tools/ildasm-exe-il-disassembler.md) para ver la información del Lenguaje intermedio de Microsoft (MSIL) de un archivo.</span><span class="sxs-lookup"><span data-stu-id="afbb3-103">You can use the [Ildasm.exe (IL Disassembler)](../../framework/tools/ildasm-exe-il-disassembler.md) to view Microsoft intermediate language (MSIL) information in a file.</span></span> <span data-ttu-id="afbb3-104">Si el archivo que se examina es un ensamblado, esta información puede incluir los atributos del ensamblado además de referencias a otros módulos y ensamblados.</span><span class="sxs-lookup"><span data-stu-id="afbb3-104">If the file being examined is an assembly, this information can include the assembly's attributes, as well as references to other modules and assemblies.</span></span> <span data-ttu-id="afbb3-105">Esta información puede ser útil para determinar si un archivo es un ensamblado o forma parte de uno y si el archivo tiene referencias a otros módulos o ensamblados.</span><span class="sxs-lookup"><span data-stu-id="afbb3-105">This information can be helpful in determining whether a file is an assembly or part of an assembly, and whether the file has references to other modules or assemblies.</span></span>  
  
<span data-ttu-id="afbb3-106">Para mostrar el contenido de un ensamblado mediante *Ildasm.exe*, escriba **ildasm** \<*nombre del ensamblado*> en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="afbb3-106">To display the contents of an assembly using *Ildasm.exe*, type **ildasm** \<*assembly name*> at a command prompt.</span></span> <span data-ttu-id="afbb3-107">Por ejemplo, el comando siguiente desensambla el ensamblado *Hello.exe*.</span><span class="sxs-lookup"><span data-stu-id="afbb3-107">For example, the following command disassembles the *Hello.exe* assembly.</span></span>  

```cmd
ildasm Hello.exe  
```  

<span data-ttu-id="afbb3-108">Para ver la información del manifiesto del ensamblado, haga doble clic en el icono **Manifiesto** en la ventana del desensamblador de MSIL.</span><span class="sxs-lookup"><span data-stu-id="afbb3-108">To view assembly manifest information, double-click the **Manifest** icon in the MSIL Disassembler window.</span></span>  
  
## <a name="example"></a><span data-ttu-id="afbb3-109">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="afbb3-109">Example</span></span>  
<span data-ttu-id="afbb3-110">El ejemplo siguiente se inicia con un programa básico “Hola mundo”.</span><span class="sxs-lookup"><span data-stu-id="afbb3-110">The following example starts with a basic "Hello World" program.</span></span> <span data-ttu-id="afbb3-111">Después de compilar el programa, use *Ildasm.exe* para desensamblar el ensamblado *Hello.exe* y ver el manifiesto del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="afbb3-111">After compiling the program, use *Ildasm.exe* to disassemble the *Hello.exe* assembly and view the assembly manifest.</span></span>  

```cpp
using namespace System;

class MainApp
{
public:
    static void Main()
    {
        Console::WriteLine("Hello World using C++/CLI!");
    }
};

int main()
{
    MainApp::Main();
}
```

```csharp
using System;

class MainApp
{
    public static void Main()
    {
        Console.WriteLine("Hello World using C#!");
    }
}
```

```vb
Imports System

Class MainApp
    Public Shared Sub Main()
        Console.WriteLine("Hello World using Visual Basic!")
    End Sub
End Class
```

<span data-ttu-id="afbb3-112">Al ejecutar el comando *ildasm.exe* en el ensamblado *Hello.exe* y hacer doble clic en el icono **Manifiesto** en la ventana del desensamblador de MSIL, se produce lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="afbb3-112">Running the command *ildasm.exe* on the *Hello.exe* assembly and double-clicking the **Manifest** icon in the MSIL Disassembler window produces the following output:</span></span>  

```output
// Metadata version: v4.0.30319  
.assembly extern mscorlib  
{  
  .publickeytoken = (B7 7A 5C 56 19 34 E0 89 )                         // .z\V.4..  
  .ver 4:0:0:0  
}  
.assembly Hello  
{  
  .custom instance void [mscorlib]System.Runtime.CompilerServices.CompilationRelaxationsAttribute::.ctor(int32) = ( 01 00 08 00 00 00 00 00 )   
  .custom instance void [mscorlib]System.Runtime.CompilerServices.RuntimeCompatibilityAttribute::.ctor() = ( 01 00 01 00 54 02 16 57 72 61 70 4E 6F 6E 45 78   // ....T..WrapNonEx  
                                                                                                             63 65 70 74 69 6F 6E 54 68 72 6F 77 73 01 )       // ceptionThrows.  
  .hash algorithm 0x00008004  
  .ver 0:0:0:0  
}  
.module Hello.exe  
// MVID: {7C2770DB-1594-438D-BAE5-98764C39CCCA}  
.imagebase 0x00400000  
.file alignment 0x00000200  
.stackreserve 0x00100000  
.subsystem 0x0003       // WINDOWS_CUI  
.corflags 0x00000001    //  ILONLY  
// Image base: 0x00600000  
```  
  
 <span data-ttu-id="afbb3-113">En la tabla siguiente se explica cada directiva del manifiesto del ensamblado *Hello.exe* usado en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="afbb3-113">The following table describes each directive in the assembly manifest of the *Hello.exe* assembly used in the example.</span></span>  
  
|<span data-ttu-id="afbb3-114">Directiva</span><span class="sxs-lookup"><span data-stu-id="afbb3-114">Directive</span></span>|<span data-ttu-id="afbb3-115">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="afbb3-115">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="afbb3-116">**.assembly extern \<** *nombre del ensamblado* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-116">**.assembly extern \<** *assembly name* **>**</span></span>|<span data-ttu-id="afbb3-117">Especifica otro ensamblado que contiene elementos a los que hace referencia el módulo actual (en este ejemplo, `mscorlib`).</span><span class="sxs-lookup"><span data-stu-id="afbb3-117">Specifies another assembly that contains items referenced by the current module (in this example, `mscorlib`).</span></span>|  
|<span data-ttu-id="afbb3-118">**.publickeytoken \<** *token* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-118">**.publickeytoken \<** *token* **>**</span></span>|<span data-ttu-id="afbb3-119">Especifica el token de la clave real del ensamblado al que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="afbb3-119">Specifies the token of the actual key of the referenced assembly.</span></span>|  
|<span data-ttu-id="afbb3-120">**.ver \<** *número de versión* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-120">**.ver \<** *version number* **>**</span></span>|<span data-ttu-id="afbb3-121">Especifica el número de versión del ensamblado al que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="afbb3-121">Specifies the version number of the referenced assembly.</span></span>|  
|<span data-ttu-id="afbb3-122">**.assembly \<** *nombre del ensamblado* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-122">**.assembly \<** *assembly name* **>**</span></span>|<span data-ttu-id="afbb3-123">Especifica el nombre del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="afbb3-123">Specifies the assembly name.</span></span>|  
|<span data-ttu-id="afbb3-124">**.hash algorithm \<** *valor int32* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-124">**.hash algorithm \<** *int32 value* **>**</span></span>|<span data-ttu-id="afbb3-125">Especifica el algoritmo hash usado.</span><span class="sxs-lookup"><span data-stu-id="afbb3-125">Specifies the hash algorithm used.</span></span>|  
|<span data-ttu-id="afbb3-126">**.ver \<** *número de versión* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-126">**.ver \<** *version number* **>**</span></span>|<span data-ttu-id="afbb3-127">Especifica el número de versión del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="afbb3-127">Specifies the version number of the assembly.</span></span>|  
|<span data-ttu-id="afbb3-128">**.module \<** *nombre del archivo* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-128">**.module \<** *file name* **>**</span></span>|<span data-ttu-id="afbb3-129">Especifica el nombre de los módulos que componen el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="afbb3-129">Specifies the name of the modules that make up the assembly.</span></span> <span data-ttu-id="afbb3-130">En este ejemplo, el ensamblado consta de un único archivo.</span><span class="sxs-lookup"><span data-stu-id="afbb3-130">In this example, the assembly consists of only one file.</span></span>|  
|<span data-ttu-id="afbb3-131">**.subsystem \<** *valor* **>**</span><span class="sxs-lookup"><span data-stu-id="afbb3-131">**.subsystem \<** *value* **>**</span></span>|<span data-ttu-id="afbb3-132">Especifica el entorno de aplicación necesario para el programa.</span><span class="sxs-lookup"><span data-stu-id="afbb3-132">Specifies the application environment required for the program.</span></span> <span data-ttu-id="afbb3-133">En este ejemplo, el valor 3 indica que este ejecutable se ejecuta desde una consola.</span><span class="sxs-lookup"><span data-stu-id="afbb3-133">In this example, the value 3 indicates that this executable is run from a console.</span></span>|  
|<span data-ttu-id="afbb3-134">**.corflags**</span><span class="sxs-lookup"><span data-stu-id="afbb3-134">**.corflags**</span></span>|<span data-ttu-id="afbb3-135">De momento, un campo reservado de los metadatos.</span><span class="sxs-lookup"><span data-stu-id="afbb3-135">Currently a reserved field in the metadata.</span></span>|  
  
 <span data-ttu-id="afbb3-136">Un manifiesto de ensamblado puede contener varias directivas diferentes, según el contenido del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="afbb3-136">An assembly manifest can contain a number of different directives, depending on the contents of the assembly.</span></span> <span data-ttu-id="afbb3-137">Para obtener una lista completa de las directivas del manifiesto del ensamblado, vea la documentación de ECMA, especialmente "Partition II: Metadata Definition and Semantics (Partición II: definición y semántica de los metadatos)" y "Partition III: CIL Instruction Set (Partición III: conjunto de instrucciones CIL)”.</span><span class="sxs-lookup"><span data-stu-id="afbb3-137">For an extensive list of the directives in the assembly manifest, see the ECMA documentation, especially "Partition II: Metadata Definition and Semantics" and "Partition III: CIL Instruction Set."</span></span> <span data-ttu-id="afbb3-138">La documentación está disponible en línea.</span><span class="sxs-lookup"><span data-stu-id="afbb3-138">The documentation is available online.</span></span> <span data-ttu-id="afbb3-139">Vea [ECMA C# and Common Language Infrastructure Standards](https://go.microsoft.com/fwlink/?LinkID=99212) (Estándares de ECMA C# y Common Language Infrastructure) en MSDN y [Standard ECMA-335-Common Language Infrastructure (CLI)](https://go.microsoft.com/fwlink/?LinkID=65552) (Estándar ECMA-335: Common Language Infrastructure [CLI]) en el sitio web de ECMA International.</span><span class="sxs-lookup"><span data-stu-id="afbb3-139">See [ECMA C# and Common Language Infrastructure Standards](https://go.microsoft.com/fwlink/?LinkID=99212) on MSDN and [Standard ECMA-335 - Common Language Infrastructure (CLI)](https://go.microsoft.com/fwlink/?LinkID=65552) on the ECMA International website.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="afbb3-140">Vea también</span><span class="sxs-lookup"><span data-stu-id="afbb3-140">See also</span></span>

- [<span data-ttu-id="afbb3-141">Dominios de aplicación y ensamblados</span><span class="sxs-lookup"><span data-stu-id="afbb3-141">Application domains and assemblies</span></span>](../../framework/app-domains/application-domains.md#application-domains-and-assemblies)
- [<span data-ttu-id="afbb3-142">Temas sobre dominios de aplicación y ensamblados</span><span class="sxs-lookup"><span data-stu-id="afbb3-142">Application domains and assemblies how-to topics</span></span>](../../framework/app-domains/application-domains-and-assemblies-how-to-topics.md)
- [<span data-ttu-id="afbb3-143">Ildasm.exe (Desensamblador de IL)</span><span class="sxs-lookup"><span data-stu-id="afbb3-143">Ildasm.exe (IL Disassembler)</span></span>](../../framework/tools/ildasm-exe-il-disassembler.md)