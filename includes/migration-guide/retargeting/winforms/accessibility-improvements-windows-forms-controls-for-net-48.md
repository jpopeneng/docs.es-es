---
ms.openlocfilehash: a9d2a28d85668cd8b6b2b7370e805ebf9c5b2843
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67803461"
---
### <a name="accessibility-improvements-in-windows-forms-controls-for-net-48"></a><span data-ttu-id="75030-101">Mejoras de accesibilidad en controles de Windows Forms para .NET 4.8</span><span class="sxs-lookup"><span data-stu-id="75030-101">Accessibility improvements in Windows Forms controls for .NET 4.8</span></span>

|   |   |
|---|---|
|<span data-ttu-id="75030-102">Detalles</span><span class="sxs-lookup"><span data-stu-id="75030-102">Details</span></span>|<span data-ttu-id="75030-103">El marco de trabajo de Windows Forms continúa mejorando su funcionamiento con las tecnologías de accesibilidad para ofrecer una mejor compatibilidad con los clientes de Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="75030-103">The Windows Forms Framework is continuing to improve how it works with accessibility technologies to better support Windows Forms customers.</span></span> <span data-ttu-id="75030-104">Entre otros, se incluyen los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="75030-104">These include the following changes:</span></span><ul><li><span data-ttu-id="75030-105">Cambios para mejorar la presentación durante el modo de contraste alto.</span><span class="sxs-lookup"><span data-stu-id="75030-105">Changes to improve display during High Contrast mode.</span></span></li><li><span data-ttu-id="75030-106">Cambios en la interacción con Narrador.</span><span class="sxs-lookup"><span data-stu-id="75030-106">Changes to interaction with Narrator.</span></span></li><li><span data-ttu-id="75030-107">Cambios en la jerarquía Accesible (mejorando la navegación a través del árbol de automatización de la interfaz de usuario).</span><span class="sxs-lookup"><span data-stu-id="75030-107">Changes in the Accessible hierarchy (improving navigation through the UI Automation tree).</span></span></li></ul>|
|<span data-ttu-id="75030-108">Sugerencia</span><span class="sxs-lookup"><span data-stu-id="75030-108">Suggestion</span></span>|<span data-ttu-id="75030-109"><strong>Cómo participar o no en estos cambios</strong> Para que la aplicación se beneficie de estos cambios, se debe ejecutar en .NET Framework 4.8.</span><span class="sxs-lookup"><span data-stu-id="75030-109"><strong>How to opt in or out of these changes</strong>In order for the application to benefit from these changes, it must run on the .NET Framework 4.8.</span></span> <span data-ttu-id="75030-110">La aplicación puede optar por recibir estos cambios de cualquiera de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="75030-110">The application can opt in into these changes in either of the following ways:</span></span><ul><li><span data-ttu-id="75030-111">Se recompila para tener .NET Framework 4.8 como destino.</span><span class="sxs-lookup"><span data-stu-id="75030-111">It is recompiled to target the .NET Framework 4.8.</span></span> <span data-ttu-id="75030-112">Estos cambios de accesibilidad están habilitados de forma predeterminada para las aplicaciones de Windows Forms destinadas a .NET Framework 4.8.</span><span class="sxs-lookup"><span data-stu-id="75030-112">These accessibility changes are enabled by default on Windows Forms applications that target the .NET Framework 4.8.</span></span></li><li><span data-ttu-id="75030-113">Tiene como destino .NET Framework 4.7.2 o una versión anterior, y no participa en los comportamientos de accesibilidad heredados mediante la adición del [modificador de AppContext](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) siguiente a la sección <code>&lt;runtime&gt;</code> del archivo app.config y estableciéndolo en <code>false</code>, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="75030-113">It targets the .NET Framework 4.7.2 or earlier version and opts out of the legacy accessibility behaviors by adding the following [AppContext switch](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) to the <code>&lt;runtime&gt;</code> section of the app config file and setting it to <code>false</code>, as the following example shows.</span></span></li></ul><pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;startup&gt;&#13;&#10;&lt;supportedRuntime version=&quot;v4.0&quot; sku=&quot;.NETFramework,Version=v4.7&quot;/&gt;&#13;&#10;&lt;/startup&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre><span data-ttu-id="75030-114">Tenga en cuenta que para participar en las características de accesibilidad agregadas en .NET Framework 4.8, también se debe participar en las características de accesibilidad de .NET Framework 4.7.1 y 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="75030-114">Note that to opt in to the accessibility features added in .NET Framework 4.8, you must also opt in to accessibility features of .NET Framework 4.7.1 and 4.7.2 as well.</span></span> <span data-ttu-id="75030-115">Las aplicaciones destinadas a .NET Framework 4.8 o una versión posterior, y cuando se quiere conservar el comportamiento de accesibilidad heredado, se puede participar en el uso de las características de accesibilidad heredadas si se establece explícitamente este modificador de AppContext en <code>true</code>. Para habilitar la compatibilidad con la invocación de la información sobre herramientas del teclado es necesario es necesario agregar la línea <code>Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false</code> al valor AppContextSwitchOverrides:</span><span class="sxs-lookup"><span data-stu-id="75030-115">Applications that target the .NET Framework 4.8 and want to preserve the legacy accessibility behavior can opt in to the use of legacy accessibility features by explicitly setting this AppContext switch to <code>true</code>.Enabling the keyboard ToolTip invocation support requires adding the <code>Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false</code> line to the AppContextSwitchOverrides value:</span></span><pre><code class="lang-xml">&#39;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false&quot; /&gt;&#39;&#13;&#10;</code></pre><span data-ttu-id="75030-116">Tenga en cuenta que habilitar esta característica requiere participar en las características de accesibilidad de .NET Framework 4.7.1 - 4.8 mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="75030-116">Note that enabling this feature requires opting in to the aforementioned accessibility features of .NET Framework 4.7.1 - 4.8.</span></span> <span data-ttu-id="75030-117">Además, si no participa en algunas de las características de accesibilidad, pero sí en las características de visualización de la información sobre herramientas, se producirá un tiempo de ejecución <xref:System.NotSupportedException> en el primer acceso a estas características.</span><span class="sxs-lookup"><span data-stu-id="75030-117">Also, if any of the accessibility features are not opted in but the tooltip display feature is opted in, a runtime <xref:System.NotSupportedException> will be thrown on the first access to these features.</span></span> <span data-ttu-id="75030-118">El mensaje de excepción indica que las informaciones sobre herramientas del teclado requieren mejoras de accesibilidad de nivel 3 para habilitarse. <strong>Uso de colores definidos por el sistema operativo en los temas de contraste alto</strong></span><span class="sxs-lookup"><span data-stu-id="75030-118">The exception message indicates that keyboard ToolTips require accessibility improvements of level 3 to be enabled.<strong>Use of OS-defined colors in High Contrast themes</strong></span></span><ul><li><span data-ttu-id="75030-119">Temas de contraste alto mejorados.</span><span class="sxs-lookup"><span data-stu-id="75030-119">Improved high-contrast themes.</span></span></li></ul><span data-ttu-id="75030-120"><strong>Compatibilidad mejorada con Narrador</strong></span><span class="sxs-lookup"><span data-stu-id="75030-120"><strong>Improved Narrator support</strong></span></span><ul><li><span data-ttu-id="75030-121">Ahora el Narrador anuncia la dirección de ordenación de <xref:System.Windows.Forms.DataGridViewColumn> al anunciar un nombre accesible de un <xref:System.Windows.Forms.DataGridViewCell>.</span><span class="sxs-lookup"><span data-stu-id="75030-121">Narrator now announces the sort direction of the <xref:System.Windows.Forms.DataGridViewColumn> when announcing an accessible name of a <xref:System.Windows.Forms.DataGridViewCell>.</span></span></li></ul><span data-ttu-id="75030-122"><strong>Compatibilidad de accesibilidad mejorada con CheckedListBox</strong></span><span class="sxs-lookup"><span data-stu-id="75030-122"><strong>Improved CheckedListBox Accessibility support</strong></span></span><ul><li><span data-ttu-id="75030-123">Compatibilidad mejorada con Narrador para el control de <xref:System.Windows.Forms.CheckedListBox>.</span><span class="sxs-lookup"><span data-stu-id="75030-123">Improved Narrator support for the <xref:System.Windows.Forms.CheckedListBox> control.</span></span> <span data-ttu-id="75030-124">Al navegar por el control <xref:System.Windows.Forms.CheckedListBox> utilizando el teclado, el Narrador se centra en el elemento <xref:System.Windows.Forms.CheckedListBox> y lo anuncia.</span><span class="sxs-lookup"><span data-stu-id="75030-124">When navigating to the <xref:System.Windows.Forms.CheckedListBox> control using the keyboard, Narrator focuses the <xref:System.Windows.Forms.CheckedListBox> item and announces it.</span></span></li><li><span data-ttu-id="75030-125">Un control CheckedListBox vacío tiene ahora un rectángulo de foco dibujado para un primer elemento virtual cuando el control se enfoca.</span><span class="sxs-lookup"><span data-stu-id="75030-125">An empty CheckedListBox control now has a focus rectangle drawn for a virtual first item when the control becomes focused.</span></span></li></ul><span data-ttu-id="75030-126"><strong>Compatibilidad de accesibilidad mejorada con ComboBox</strong></span><span class="sxs-lookup"><span data-stu-id="75030-126"><strong>Improved ComboBox Accessibility support</strong></span></span><ul><li><span data-ttu-id="75030-127">Compatibilidad de la automatización de la interfaz de usuario habilitada para el control <xref:System.Windows.Forms.ComboBox>, con la capacidad para usar notificaciones de automatización de la interfaz de usuario y otras características de automatización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="75030-127">Enabled UI Automation support for the <xref:System.Windows.Forms.ComboBox> control, with the ability to use UI Automation notifications and other UI Automation features.</span></span></li></ul><span data-ttu-id="75030-128"><strong>Compatibilidad de accesibilidad de DataGridView mejorada</strong></span><span class="sxs-lookup"><span data-stu-id="75030-128"><strong>Improved DataGridView Accessibility support</strong></span></span><ul><li><span data-ttu-id="75030-129">Compatibilidad de la automatización de la interfaz de usuario habilitada para el control <xref:System.Windows.Forms.DataGridView>, con capacidad para usar notificaciones de automatización de la interfaz de usuario y otras características de automatización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="75030-129">Enabled UI Automation support for <xref:System.Windows.Forms.DataGridView> control with ability to use UI Automation notifications and other UI Automation features.</span></span></li><li><span data-ttu-id="75030-130">El elemento de automatización de la interfaz de usuario que corresponde a <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl> o <xref:System.Windows.Forms.DataGridViewTextBoxEditingControl> ahora es un elemento secundario de la celda de edición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="75030-130">The UI Automation element which corresponds to the <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl> or <xref:System.Windows.Forms.DataGridViewTextBoxEditingControl> is now a child of corresponding editing cell.</span></span></li></ul><span data-ttu-id="75030-131"><strong>Compatibilidad de accesibilidad mejorada con LinkLabel</strong></span><span class="sxs-lookup"><span data-stu-id="75030-131"><strong>Improved LinkLabel Accessibility support</strong></span></span><ul><li><span data-ttu-id="75030-132">Accesibilidad del control <xref:System.Windows.Forms.LinkLabel> mejorada: El Narrador anuncia el estado deshabilitado para el vínculo si el correspondiente control <xref:System.Windows.Forms.LinkLabel> está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="75030-132">Improved <xref:System.Windows.Forms.LinkLabel> control accessibility: Narrator announces the disabled state for the link if the corresponding <xref:System.Windows.Forms.LinkLabel> control is disabled.</span></span></li></ul><span data-ttu-id="75030-133"><strong>Compatibilidad de accesibilidad mejorada con ProgressBar</strong></span><span class="sxs-lookup"><span data-stu-id="75030-133"><strong>Improved ProgressBar Accessibility support</strong></span></span><ul><li><span data-ttu-id="75030-134">Compatibilidad de la automatización de la interfaz de usuario habilitada para el control <xref:System.Windows.Forms.ProgressBar> con la capacidad para usar notificaciones de automatización de la interfaz de usuario y otras características de automatización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="75030-134">Enabled UI Automation support for the <xref:System.Windows.Forms.ProgressBar> control with the ability to use UI Automation notifications and other UI Automation features.</span></span> <span data-ttu-id="75030-135">Los desarrolladores ahora pueden usar las notificaciones de automatización de la interfaz de usuario que Narrador puede anunciar para indicar el progreso.</span><span class="sxs-lookup"><span data-stu-id="75030-135">Developers are now able to use UI Automation notifications which Narrator can announce to indicate progress.</span></span></li></ul><span data-ttu-id="75030-136">Para obtener información general sobre eventos de la automatización de la interfaz de usuario, incluidos los eventos de notificación de automatización de la interfaz de usuario, consulte la [Información general sobre eventos de la automatización de la interfaz de usuario](https://docs.microsoft.com/en-us/windows/desktop/WinAuto/uiauto-eventsoverview).<strong>Compatibilidad de accesibilidad mejorada con PropertyGrid</strong></span><span class="sxs-lookup"><span data-stu-id="75030-136">For an overview of UI automation events overview, including UI automation notification events, see the [UI Automation Events Overview](https://docs.microsoft.com/en-us/windows/desktop/WinAuto/uiauto-eventsoverview).<strong>Improved PropertyGrid Accessibility support</strong></span></span><ul><li><span data-ttu-id="75030-137">Compatibilidad de la automatización de la interfaz de usuario habilitada para el control <xref:System.Windows.Forms.PropertyGrid>, con la capacidad para usar notificaciones de automatización de la interfaz de usuario y otras características de automatización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="75030-137">Enabled UI Automation support for the <xref:System.Windows.Forms.PropertyGrid> control, with the ability to use UI Automation notifications and other UI Automation features.</span></span></li><li><span data-ttu-id="75030-138">El elemento de automatización de la interfaz de usuario que corresponde a la propiedad que actualmente se edita es ahora un elemento secundario del elemento de automatización de la interfaz de usuario del correspondiente elemento de propiedad.</span><span class="sxs-lookup"><span data-stu-id="75030-138">The UI Automation element which corresponds to the currently edited property is now a child of the corresponding property item UI Automation element.</span></span></li><li><span data-ttu-id="75030-139">El elemento de propiedad de automatización de la interfaz de usuario es ahora un elemento secundario del correspondiente elemento de categoría, si el control primario <xref:System.Windows.Forms.PropertyGrid> se establece en la vista por categorías.</span><span class="sxs-lookup"><span data-stu-id="75030-139">The UI Automation property item element is now a child of the corresponding category element if the parent <xref:System.Windows.Forms.PropertyGrid> control is set to category view.</span></span></li></ul><span data-ttu-id="75030-140"><strong>Compatibilidad mejorada con ToolStrip</strong></span><span class="sxs-lookup"><span data-stu-id="75030-140"><strong>Improved ToolStrip support</strong></span></span><ul><li><span data-ttu-id="75030-141">Compatibilidad de la automatización de la interfaz de usuario habilitada para el control <xref:System.Windows.Forms.ToolStrip>, con la capacidad para usar notificaciones de automatización de la interfaz de usuario y otras características de automatización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="75030-141">Enabled UI Automation support for the <xref:System.Windows.Forms.ToolStrip> control, with the ability to use UI Automation notifications and other UI Automation features.</span></span></li><li><span data-ttu-id="75030-142">Navegación mejorada a través de elementos <xref:System.Windows.Forms.ToolStrip>.</span><span class="sxs-lookup"><span data-stu-id="75030-142">Improved navigation through <xref:System.Windows.Forms.ToolStrip> items.</span></span></li><li><span data-ttu-id="75030-143">En el modo de elementos, el enfoque de Narrador no desaparecerá y no pasará a los elementos ocultos.</span><span class="sxs-lookup"><span data-stu-id="75030-143">In items mode, Narrator focus does not disappear and does not go to hidden items.</span></span></li></ul><span data-ttu-id="75030-144"><strong>Indicaciones visuales mejoradas</strong></span><span class="sxs-lookup"><span data-stu-id="75030-144"><strong>Improved Visual cues</strong></span></span><ul><li><span data-ttu-id="75030-145">Un control <xref:System.Windows.Forms.CheckedListBox> vacío ahora muestra un indicador de enfoque cuando recibe el enfoque.</span><span class="sxs-lookup"><span data-stu-id="75030-145">An empty <xref:System.Windows.Forms.CheckedListBox> control now displays a focus indicator when it receives focus.</span></span></li></ul><span data-ttu-id="75030-146">Nota: La compatibilidad de automatización de la interfaz de usuario está habilitada para los controles en tiempo de ejecución pero no se utiliza en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="75030-146">Note: UI automation support is enabled for controls in runtime but is not used in design time.</span></span> <span data-ttu-id="75030-147">Para obtener información general de la automatización de la interfaz de usuario, vea la [información general sobre la Automatización de la interfaz de usuario](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-overview).</span><span class="sxs-lookup"><span data-stu-id="75030-147">For an overview of UI automation, see the [UI Automation Overview](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-overview).</span></span></p><span data-ttu-id="75030-148"><strong>Información sobre herramientas de invocación de controles con un teclado</strong></span><span class="sxs-lookup"><span data-stu-id="75030-148"><strong>Invoking controls' ToolTips with a keyboard</strong></span></span><ul><li><span data-ttu-id="75030-149">Ahora se puede invocar el control de información sobre herramientas centrándose en el control con el teclado.</span><span class="sxs-lookup"><span data-stu-id="75030-149">Control tooltip can now be invoked by focusing the control with keyboard.</span></span> <span data-ttu-id="75030-150">Esta característica debe habilitarse explícitamente para la aplicación (consulte la sección <strong>&quot;Cómo participar o no en estos cambios&quot;</strong>)</span><span class="sxs-lookup"><span data-stu-id="75030-150">This feature needs to be enabled explicitly for the application (see section <strong>&quot;How to opt in or out of these changes&quot;</strong>)</span></span></li></ul>|
|<span data-ttu-id="75030-151">Ámbito</span><span class="sxs-lookup"><span data-stu-id="75030-151">Scope</span></span>|<span data-ttu-id="75030-152">Major</span><span class="sxs-lookup"><span data-stu-id="75030-152">Major</span></span>|
|<span data-ttu-id="75030-153">Versión</span><span class="sxs-lookup"><span data-stu-id="75030-153">Version</span></span>|<span data-ttu-id="75030-154">4.8</span><span class="sxs-lookup"><span data-stu-id="75030-154">4.8</span></span>|
|<span data-ttu-id="75030-155">Tipo</span><span class="sxs-lookup"><span data-stu-id="75030-155">Type</span></span>|<span data-ttu-id="75030-156">Redestinación</span><span class="sxs-lookup"><span data-stu-id="75030-156">Retargeting</span></span>|
