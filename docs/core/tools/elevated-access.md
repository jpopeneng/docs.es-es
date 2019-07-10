---
title: Acceso con privilegios elevados para comandos de dotnet
description: Obtenga información sobre los procedimientos recomendados para los comandos de dotnet que requieren acceso con privilegios elevados.
author: wli3
ms.date: 06/26/2019
ms.openlocfilehash: 3d874a76eadbf5330c4e5efe4e86bfeca0a9b504
ms.sourcegitcommit: 52e588dc2ee74d484cd07ac60076be25cbf777ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67410633"
---
# <a name="elevated-access-for-dotnet-commands"></a><span data-ttu-id="5e275-103">Acceso con privilegios elevados para comandos de dotnet</span><span class="sxs-lookup"><span data-stu-id="5e275-103">Elevated access for dotnet commands</span></span>

<span data-ttu-id="5e275-104">Los procedimientos recomendados de desarrollo de software sirven de guía a los desarrolladores para escribir software que requiera la menor cantidad de privilegios.</span><span class="sxs-lookup"><span data-stu-id="5e275-104">Software development best practices guide developers to writing software that requires the least amount of privilege.</span></span> <span data-ttu-id="5e275-105">Sin embargo, algunos programas, como las herramientas de supervisión de rendimiento, requieren permiso del administrador debido a las reglas del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="5e275-105">However, some software, like performance monitoring tools, requires admin permission because of operating system rules.</span></span> <span data-ttu-id="5e275-106">En las siguientes instrucciones se describen los escenarios admitidos para escribir dicho software con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e275-106">The following guidance describes supported scenarios for writing such software with .NET Core.</span></span> 

<span data-ttu-id="5e275-107">Los siguientes comandos se pueden ejecutar con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="5e275-107">The following commands can be run elevated:</span></span>

- <span data-ttu-id="5e275-108">Comandos `dotnet tool`, como [dotnet tool install](dotnet-tool-install.md).</span><span class="sxs-lookup"><span data-stu-id="5e275-108">`dotnet tool` commands, such as [dotnet tool install](dotnet-tool-install.md).</span></span>
- `dotnet run --no-build`

<span data-ttu-id="5e275-109">No se recomienda ejecutar otros comandos con privilegios elevados.</span><span class="sxs-lookup"><span data-stu-id="5e275-109">We don't recommend running other commands elevated.</span></span> <span data-ttu-id="5e275-110">En concreto, no se recomienda usar privilegios elevados con los comandos que usa MSBuild, como [dotnet restore](dotnet-restore.md), [dotnet build](dotnet-build.md) y [dotnet run](dotnet-run.md).</span><span class="sxs-lookup"><span data-stu-id="5e275-110">In particular, we don't recommend elevation with commands that use MSBuild, such as [dotnet restore](dotnet-restore.md), [dotnet build](dotnet-build.md), and [dotnet run](dotnet-run.md).</span></span> <span data-ttu-id="5e275-111">Los problemas de administración de permisos son el principal problema cuando un usuario cambia varias veces entre una cuenta restringida y otra raíz después de emitir comandos de dotnet.</span><span class="sxs-lookup"><span data-stu-id="5e275-111">The primary issue is permission management problems when a user transitions back and forth between root and a restricted account after issuing dotnet commands.</span></span> <span data-ttu-id="5e275-112">Como usuario restringido, es posible que no tenga acceso al archivo generado por un usuario raíz.</span><span class="sxs-lookup"><span data-stu-id="5e275-112">You may find as a restricted user that you don't have access to the file built by a root user.</span></span> <span data-ttu-id="5e275-113">Hay maneras de resolver esta situación, pero, para empezar, no es necesario que surjan.</span><span class="sxs-lookup"><span data-stu-id="5e275-113">There are ways to resolve this situation, but they're unnecessary to get into in the first place.</span></span>

<span data-ttu-id="5e275-114">Puede ejecutar comandos como raíz siempre y cuando no cambie repetidas veces entre una cuenta restringida y otra raíz.</span><span class="sxs-lookup"><span data-stu-id="5e275-114">You can run commands as root as long as you don’t transition back and forth between root and a restricted account.</span></span> <span data-ttu-id="5e275-115">Por ejemplo, los contenedores de Docker se ejecutan como raíz de forma predeterminada, por lo que tienen esta característica.</span><span class="sxs-lookup"><span data-stu-id="5e275-115">For example, Docker containers run as root by default, so they have this characteristic.</span></span>

## <a name="global-tool-installation"></a><span data-ttu-id="5e275-116">Instalación de herramienta global</span><span class="sxs-lookup"><span data-stu-id="5e275-116">Global tool installation</span></span>

<span data-ttu-id="5e275-117">Las instrucciones siguientes muestran la manera recomendada para instalar, ejecutar y desinstalar las herramientas de .NET Core que requieren privilegios elevados para ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="5e275-117">The following instructions demonstrate the recommended way to install, run, and uninstall .NET Core tools that require elevated permissions to execute.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5e275-118">Windows</span><span class="sxs-lookup"><span data-stu-id="5e275-118">Windows</span></span>](#tab/windows)

### <a name="install-the-global-tool"></a><span data-ttu-id="5e275-119">Instalación de la herramienta global</span><span class="sxs-lookup"><span data-stu-id="5e275-119">Install the global tool</span></span>

<span data-ttu-id="5e275-120">Si la carpeta `%ProgramFiles%\dotnet-tools` ya existe, siga este procedimiento para comprobar si el grupo "Usuarios" tiene permiso para escribir o modificar ese directorio:</span><span class="sxs-lookup"><span data-stu-id="5e275-120">If the folder `%ProgramFiles%\dotnet-tools` already exists, do the following to check whether the "Users" group has permission to write or modify that directory:</span></span>

* <span data-ttu-id="5e275-121">Haga clic con el botón derecho en la carpeta `%ProgramFiles%\dotnet-tools` y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="5e275-121">Right-click the `%ProgramFiles%\dotnet-tools` folder and select **Properties**.</span></span> <span data-ttu-id="5e275-122">Se abrirá el cuadro de diálogo **Propiedades comunes**.</span><span class="sxs-lookup"><span data-stu-id="5e275-122">The **Common Properties** dialog box opens.</span></span> 
* <span data-ttu-id="5e275-123">Seleccione la pestaña **Seguridad**. En **Nombres de grupos o usuarios**, compruebe si el grupo "Usuarios" tiene permiso para escribir o modificar el directorio.</span><span class="sxs-lookup"><span data-stu-id="5e275-123">Select the **Security** tab. Under **Group or user names**, check whether the “Users” group has permission to write or modify the directory.</span></span> 
* <span data-ttu-id="5e275-124">Si el grupo "Usuarios" puede escribir o modificar el directorio, al instalar las herramientas, use otro nombre de directorio de *dotnet-tools*.</span><span class="sxs-lookup"><span data-stu-id="5e275-124">If the "Users" group can write or modify the directory, use a different directory name when installing the tools rather than *dotnet-tools*.</span></span>

<span data-ttu-id="5e275-125">Para instalar las herramientas, ejecute el siguiente comando en un símbolo del sistema con privilegios elevados.</span><span class="sxs-lookup"><span data-stu-id="5e275-125">To install tools, run the following command in elevated prompt.</span></span> <span data-ttu-id="5e275-126">Creará la carpeta *dotnet-tools* durante la instalación.</span><span class="sxs-lookup"><span data-stu-id="5e275-126">It will create the *dotnet-tools* folder during the installation.</span></span>

```cmd
dotnet tool install PACKAGEID --tool-path "%ProgramFiles%\dotnet-tools".
```

### <a name="run-the-global-tool"></a><span data-ttu-id="5e275-127">Ejecución de la herramienta global</span><span class="sxs-lookup"><span data-stu-id="5e275-127">Run the global tool</span></span>

<span data-ttu-id="5e275-128">**Opción 1** Use la ruta de acceso completa con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="5e275-128">**Option 1** Use the full path with elevated prompt:</span></span>

```cmd
"%ProgramFiles%\dotnet-tools\TOOLCOMMAND"
```

<span data-ttu-id="5e275-129">**Opción 2** Agregue la carpeta recién creada a `%Path%`.</span><span class="sxs-lookup"><span data-stu-id="5e275-129">**Option 2** Add the newly created folder to `%Path%`.</span></span> <span data-ttu-id="5e275-130">Solo necesita realizar esta operación una vez.</span><span class="sxs-lookup"><span data-stu-id="5e275-130">You only need to do this operation once.</span></span>

```cmd
setx Path "%Path%;%ProgramFiles%\dotnet-tools\"
```

<span data-ttu-id="5e275-131">Y ejecute con:</span><span class="sxs-lookup"><span data-stu-id="5e275-131">And run with:</span></span>

```cmd
TOOLCOMMAND
```

### <a name="uninstall-the-global-tool"></a><span data-ttu-id="5e275-132">Desinstalación de la herramienta global</span><span class="sxs-lookup"><span data-stu-id="5e275-132">Uninstall the global tool</span></span>

<span data-ttu-id="5e275-133">En un símbolo del sistema con privilegios elevados, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="5e275-133">In an elevated prompt, type the following command:</span></span>

```cmd
dotnet tool uninstall PACKAGEID --tool-path "%ProgramFiles%\dotnet-tools"
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="5e275-134">Linux</span><span class="sxs-lookup"><span data-stu-id="5e275-134">Linux</span></span>](#tab/linux)

[!INCLUDE [elevated-access-unix](../../../includes/elevated-access-unix.md)]

# <a name="macostabmacos"></a>[<span data-ttu-id="5e275-135">macOS</span><span class="sxs-lookup"><span data-stu-id="5e275-135">macOS</span></span>](#tab/macos)

[!INCLUDE [elevated-access-unix](../../../includes/elevated-access-unix.md)]

---

## <a name="local-tools"></a><span data-ttu-id="5e275-136">Herramientas locales</span><span class="sxs-lookup"><span data-stu-id="5e275-136">Local tools</span></span>

<span data-ttu-id="5e275-137">Las herramientas locales se limitan al árbol de subdirectorio, por usuario.</span><span class="sxs-lookup"><span data-stu-id="5e275-137">Local tools are scoped per subdirectory tree, per user.</span></span> <span data-ttu-id="5e275-138">Cuando se ejecutan con privilegios elevados, las herramientas locales comparten un entorno de usuario con privilegios restringidos en el entorno con privilegios elevados.</span><span class="sxs-lookup"><span data-stu-id="5e275-138">When run elevated, local tools share a restricted user environment to the elevated environment.</span></span> <span data-ttu-id="5e275-139">En Linux y macOS, esto da como resultado archivos que establecen con acceso de solo usuario raíz.</span><span class="sxs-lookup"><span data-stu-id="5e275-139">In Linux and macOS, this results in files being set with root user-only access.</span></span> <span data-ttu-id="5e275-140">Si el usuario cambia a una cuenta restringida, el usuario ya no podrá acceder a los archivos ni escribir en ellos.</span><span class="sxs-lookup"><span data-stu-id="5e275-140">If the user switches back to a restricted account, the user can no longer access or write to the files.</span></span> <span data-ttu-id="5e275-141">Por tanto, no se recomienda instalar las herramientas que necesiten permisos elevados como herramientas locales.</span><span class="sxs-lookup"><span data-stu-id="5e275-141">So installing tools that require elevation as local tools isn't recommended.</span></span> <span data-ttu-id="5e275-142">En su lugar, use la opción `--tool-path` y las instrucciones anteriores para las herramientas globales.</span><span class="sxs-lookup"><span data-stu-id="5e275-142">Instead, use the `--tool-path` option and the previous guidelines for global tools.</span></span>

## <a name="elevation-during-development"></a><span data-ttu-id="5e275-143">Elevación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="5e275-143">Elevation during development</span></span>

<span data-ttu-id="5e275-144">Durante el desarrollo, puede que necesite acceso con privilegios elevados para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e275-144">During development, you may need elevated access to test your application.</span></span> <span data-ttu-id="5e275-145">Este escenario es común para las aplicaciones de IoT, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5e275-145">This scenario is common for IoT apps, for example.</span></span> <span data-ttu-id="5e275-146">Se recomienda que compile la aplicación sin la elevación y, a continuación, la ejecute con privilegios elevados.</span><span class="sxs-lookup"><span data-stu-id="5e275-146">We recommend that you build the application without elevation and then run it with elevation.</span></span> <span data-ttu-id="5e275-147">Hay unos pocos patrones, como los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5e275-147">There are a few patterns, as follows:</span></span>

- <span data-ttu-id="5e275-148">Uso de archivo ejecutable generado (proporciona el mejor rendimiento de inicio):</span><span class="sxs-lookup"><span data-stu-id="5e275-148">Using generated executable (it provides the best startup performance):</span></span>

   ```bash
   dotnet build
   sudo ./bin/Debug/netcoreapp3.0/APPLICATIONNAME
   ```
    
- <span data-ttu-id="5e275-149">Mediante el comando [dotnet run](dotnet-run.md) con la marca `—no-build` para evitar que se generen nuevos archivos binarios:</span><span class="sxs-lookup"><span data-stu-id="5e275-149">Using the [dotnet run](dotnet-run.md) command with the `—no-build` flag to avoid generating new binaries:</span></span>

   ```bash
   dotnet build
   sudo dotnet run --no-build
   ```

## <a name="see-also"></a><span data-ttu-id="5e275-150">Vea también</span><span class="sxs-lookup"><span data-stu-id="5e275-150">See also</span></span>

* [<span data-ttu-id="5e275-151">Información general sobre las herramientas globales de .NET Core</span><span class="sxs-lookup"><span data-stu-id="5e275-151">.NET Core Global Tools overview</span></span>](global-tools.md)