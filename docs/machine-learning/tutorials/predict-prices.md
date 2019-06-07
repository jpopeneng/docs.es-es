---
title: 'Tutorial: Predicción de precios mediante regresión'
description: En este tutorial se muestra cómo compilar un modelo de regresión con ML.NET para predecir precios, en concreto, las tarifas de taxi de Nueva York.
author: jralexander
ms.author: johalex
ms.date: 05/09/2019
ms.topic: tutorial
ms.custom: mvc, seodec18, title-hack-0516
ms.openlocfilehash: 40f70b6d89bf19ae0b20cb00d56e9f7dceb48f61
ms.sourcegitcommit: 4735bb7741555bcb870d7b42964d3774f4897a6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66377786"
---
# <a name="tutorial-predict-prices-using-regression-with-mlnet"></a><span data-ttu-id="a1133-103">Tutorial: Predicción de precios mediante regresión con ML.NET</span><span class="sxs-lookup"><span data-stu-id="a1133-103">Tutorial: Predict prices using regression with ML.NET</span></span>

<span data-ttu-id="a1133-104">En este tutorial se muestra cómo compilar un [modelo de regresión](../resources/glossary.md#regression) con ML.NET para predecir precios, en concreto, las tarifas de taxi de Nueva York.</span><span class="sxs-lookup"><span data-stu-id="a1133-104">This tutorial illustrates how to build a [regression model](../resources/glossary.md#regression) using ML.NET to predict prices, specifically, New York City taxi fares.</span></span>

<span data-ttu-id="a1133-105">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="a1133-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a1133-106">Preparar y entender los datos</span><span class="sxs-lookup"><span data-stu-id="a1133-106">Prepare and understand the data</span></span>
> * <span data-ttu-id="a1133-107">Cargar y transformar los datos</span><span class="sxs-lookup"><span data-stu-id="a1133-107">Load and transform the data</span></span>
> * <span data-ttu-id="a1133-108">Elegir un algoritmo de aprendizaje</span><span class="sxs-lookup"><span data-stu-id="a1133-108">Choose a learning algorithm</span></span>
> * <span data-ttu-id="a1133-109">Entrenar el modelo</span><span class="sxs-lookup"><span data-stu-id="a1133-109">Train the model</span></span>
> * <span data-ttu-id="a1133-110">Evaluar el modelo</span><span class="sxs-lookup"><span data-stu-id="a1133-110">Evaluate the model</span></span>
> * <span data-ttu-id="a1133-111">Usar el modelo para las predicciones</span><span class="sxs-lookup"><span data-stu-id="a1133-111">Use the model for predictions</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1133-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a1133-112">Prerequisites</span></span>

* <span data-ttu-id="a1133-113">[Visual Studio 2017 15.6 o posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) con la carga de trabajo "Desarrollo multiplataforma de .NET Core" instalada.</span><span class="sxs-lookup"><span data-stu-id="a1133-113">[Visual Studio 2017 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="a1133-114">Creación de una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="a1133-114">Create a console application</span></span>

1. <span data-ttu-id="a1133-115">Cree una **aplicación de consola de .NET Core** denominada "TaxiFarePrediction".</span><span class="sxs-lookup"><span data-stu-id="a1133-115">Create a **.NET Core Console Application** called "TaxiFarePrediction".</span></span>

1. <span data-ttu-id="a1133-116">Cree un directorio denominado *Datos* en el proyecto para almacenar el conjunto de datos y los archivos de modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-116">Create a directory named *Data* in your project to store the data set and model files.</span></span>

1. <span data-ttu-id="a1133-117">Instale el paquete NuGet **Microsoft.ML**:</span><span class="sxs-lookup"><span data-stu-id="a1133-117">Install the **Microsoft.ML** NuGet Package:</span></span>

    <span data-ttu-id="a1133-118">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a1133-118">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="a1133-119">Elija "nuget.org" como origen del paquete, seleccione la pestaña **Examinar**, busque **Microsoft.ML**, seleccione el paquete en la lista y seleccione el botón **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="a1133-119">Choose "nuget.org" as the Package source, select the **Browse** tab, search for **Microsoft.ML**, select the package in the list, and select the **Install** button.</span></span> <span data-ttu-id="a1133-120">Seleccione el botón **Aceptar** en el cuadro de diálogo **Vista previa de cambios** y, a continuación, seleccione el botón **Acepto** del cuadro de diálogo **Aceptación de la licencia** en caso de que esté de acuerdo con los términos de licencia de los paquetes mostrados.</span><span class="sxs-lookup"><span data-stu-id="a1133-120">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span> <span data-ttu-id="a1133-121">Haga lo mismo con el paquete NuGet **Microsoft.ML.FastTree**.</span><span class="sxs-lookup"><span data-stu-id="a1133-121">Do the same for the **Microsoft.ML.FastTree** Nuget package.</span></span>

## <a name="prepare-and-understand-the-data"></a><span data-ttu-id="a1133-122">Preparar y entender los datos</span><span class="sxs-lookup"><span data-stu-id="a1133-122">Prepare and understand the data</span></span>

1. <span data-ttu-id="a1133-123">Descargue los conjuntos de datos [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-train.csv) y [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-test.csv) y guárdelos en la carpeta *Datos* que ha creado en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="a1133-123">Download the [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-train.csv) and the [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/master/test/data/taxi-fare-test.csv) data sets and save them to the *Data* folder you've created at the previous step.</span></span> <span data-ttu-id="a1133-124">Usaremos estos conjuntos de datos para entrenar el modelo de aprendizaje automático y, después, evaluar su precisión.</span><span class="sxs-lookup"><span data-stu-id="a1133-124">We use these data sets to train the machine learning model and then evaluate how accurate the model is.</span></span> <span data-ttu-id="a1133-125">Estos conjuntos de datos proceden originalmente del [conjunto de datos NYC TLC Taxi Trip](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml).</span><span class="sxs-lookup"><span data-stu-id="a1133-125">These data sets are originally from the [NYC TLC Taxi Trip data set](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml).</span></span>

1. <span data-ttu-id="a1133-126">En el **Explorador de soluciones**, haga clic con el botón derecho en cada uno de los archivos \*.csv y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="a1133-126">In **Solution Explorer**, right-click each of the \*.csv files and select **Properties**.</span></span> <span data-ttu-id="a1133-127">En **Avanzadas**, cambie el valor de **Copiar en el directorio de salida** por **Copiar si es posterior**.</span><span class="sxs-lookup"><span data-stu-id="a1133-127">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

1. <span data-ttu-id="a1133-128">Abra el conjunto de datos **taxi-fare-train.csv** y consulte los encabezados de columna de la primera fila.</span><span class="sxs-lookup"><span data-stu-id="a1133-128">Open the **taxi-fare-train.csv** data set and look at column headers in the first row.</span></span> <span data-ttu-id="a1133-129">Eche un vistazo a cada una de las columnas.</span><span class="sxs-lookup"><span data-stu-id="a1133-129">Take a look at each of the columns.</span></span> <span data-ttu-id="a1133-130">Estudie los datos y decida qué columnas son **características** y cuál es la **etiqueta**.</span><span class="sxs-lookup"><span data-stu-id="a1133-130">Understand the data and decide which columns are **features** and which one is the **label**.</span></span>

<span data-ttu-id="a1133-131">`label` es la columna que quiere predecir.</span><span class="sxs-lookup"><span data-stu-id="a1133-131">The `label` is the column you want to predict.</span></span> <span data-ttu-id="a1133-132">Las `Features` identificadas son las entradas que proporciona al modelo para predecir la `Label`.</span><span class="sxs-lookup"><span data-stu-id="a1133-132">The identified `Features`are the inputs you give the model to predict the `Label`.</span></span>

<span data-ttu-id="a1133-133">El conjunto de datos proporcionado contiene las columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1133-133">The provided data set contains the following columns:</span></span>

* <span data-ttu-id="a1133-134">**vendor_id:** el identificador del taxista es una característica.</span><span class="sxs-lookup"><span data-stu-id="a1133-134">**vendor_id:** The ID of the taxi vendor is a feature.</span></span>
* <span data-ttu-id="a1133-135">**rate_code:** el tipo de tarifa del viaje en taxi es una característica.</span><span class="sxs-lookup"><span data-stu-id="a1133-135">**rate_code:** The rate type of the taxi trip is a feature.</span></span>
* <span data-ttu-id="a1133-136">**passenger_count:** el número de pasajeros en el recorrido es una característica.</span><span class="sxs-lookup"><span data-stu-id="a1133-136">**passenger_count:** The number of passengers on the trip is a feature.</span></span>
* <span data-ttu-id="a1133-137">**trip_time_in_secs:** la cantidad de tiempo que tardó el viaje.</span><span class="sxs-lookup"><span data-stu-id="a1133-137">**trip_time_in_secs:** The amount of time the trip took.</span></span> <span data-ttu-id="a1133-138">Por ejemplo, si quiere calcular la tarifa del viaje antes de que termine</span><span class="sxs-lookup"><span data-stu-id="a1133-138">You want to predict the fare of the trip before the trip is completed.</span></span> <span data-ttu-id="a1133-139">y aún no conoce la duración del viaje,</span><span class="sxs-lookup"><span data-stu-id="a1133-139">At that moment you don't know how long the trip would take.</span></span> <span data-ttu-id="a1133-140">el tiempo del viaje no es una característica, por lo que deberá excluir esta columna del modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-140">Thus, the trip time is not a feature and you'll exclude this column from the model.</span></span>
* <span data-ttu-id="a1133-141">**trip_distance:** la distancia del viaje es una característica.</span><span class="sxs-lookup"><span data-stu-id="a1133-141">**trip_distance:** The distance of the trip is a feature.</span></span>
* <span data-ttu-id="a1133-142">**payment_type:** el método de pago (efectivo o tarjeta de crédito) es una característica.</span><span class="sxs-lookup"><span data-stu-id="a1133-142">**payment_type:** The payment method (cash or credit card) is a feature.</span></span>
* <span data-ttu-id="a1133-143">**fare_amount:** la tarifa de taxi total pagada es la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="a1133-143">**fare_amount:** The total taxi fare paid is the label.</span></span>

## <a name="create-data-classes"></a><span data-ttu-id="a1133-144">Crear clases de datos</span><span class="sxs-lookup"><span data-stu-id="a1133-144">Create data classes</span></span>

<span data-ttu-id="a1133-145">Cree clases para los datos de entrada y las predicciones:</span><span class="sxs-lookup"><span data-stu-id="a1133-145">Create classes for the input data and the predictions:</span></span>

1. <span data-ttu-id="a1133-146">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y, a continuación, seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a1133-146">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>
1. <span data-ttu-id="a1133-147">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione **Clase** y cambie el campo **Nombre** a *TaxiTrip.cs*.</span><span class="sxs-lookup"><span data-stu-id="a1133-147">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *TaxiTrip.cs*.</span></span> <span data-ttu-id="a1133-148">A continuación, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="a1133-148">Then, select the **Add** button.</span></span>
1. <span data-ttu-id="a1133-149">Agregue las siguientes directivas `using` al nuevo archivo:</span><span class="sxs-lookup"><span data-stu-id="a1133-149">Add the following `using` directives to the new file:</span></span>

   [!code-csharp[AddUsings](~/samples/machine-learning/tutorials/TaxiFarePrediction/TaxiTrip.cs#1 "Add necessary usings")]

<span data-ttu-id="a1133-150">Quite la definición de clase existente y agregue el código siguiente, que tiene dos clases `TaxiTrip` y `TaxiTripFarePrediction`, al archivo *TaxiTrip.cs*:</span><span class="sxs-lookup"><span data-stu-id="a1133-150">Remove the existing class definition and add the following code, which has two classes `TaxiTrip` and `TaxiTripFarePrediction`, to the *TaxiTrip.cs* file:</span></span>

[!code-csharp[DefineTaxiTrip](~/samples/machine-learning/tutorials/TaxiFarePrediction/TaxiTrip.cs#2 "Define the taxi trip and fare predictions classes")]

<span data-ttu-id="a1133-151">`TaxiTrip` es la clase de datos de entrada y tiene definiciones para cada una de las columnas del conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-151">`TaxiTrip` is the input data class and has definitions for each of the data set columns.</span></span> <span data-ttu-id="a1133-152">Use el atributo <xref:Microsoft.ML.Data.LoadColumnAttribute> para especificar los índices de las columnas de origen en el conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-152">Use the <xref:Microsoft.ML.Data.LoadColumnAttribute> attribute to specify the indices of the source columns in the data set.</span></span>

<span data-ttu-id="a1133-153">La clase `TaxiTripFarePrediction` representa los resultados predichos.</span><span class="sxs-lookup"><span data-stu-id="a1133-153">The `TaxiTripFarePrediction` class represents predicted results.</span></span> <span data-ttu-id="a1133-154">Tiene un único campo flotante, `FareAmount`, con un atributo `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> aplicado.</span><span class="sxs-lookup"><span data-stu-id="a1133-154">It has a single float field, `FareAmount`, with a `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> attribute applied.</span></span> <span data-ttu-id="a1133-155">En el caso de la tarea de regresión, la columna **Score** contiene valores de etiqueta predichos.</span><span class="sxs-lookup"><span data-stu-id="a1133-155">In case of the regression task the **Score** column contains predicted label values.</span></span>

> [!NOTE]
> <span data-ttu-id="a1133-156">Use el tipo `float` para representar los valores de punto flotante en las clases de entrada y predicción de datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-156">Use the `float` type to represent floating-point values in the input and prediction data classes.</span></span>

### <a name="define-data-and-model-paths"></a><span data-ttu-id="a1133-157">Definir rutas de acceso de datos y modelos</span><span class="sxs-lookup"><span data-stu-id="a1133-157">Define data and model paths</span></span>

<span data-ttu-id="a1133-158">Agregue las siguientes instrucciones `using` adicionales a la parte superior del archivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a1133-158">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#1 "Add necessary usings")]

<span data-ttu-id="a1133-159">Deberá crear tres campos que contengan las rutas de acceso a los archivos con los conjuntos de datos y el archivo para guardar el modelo:</span><span class="sxs-lookup"><span data-stu-id="a1133-159">You need to create three fields to hold the paths to the files with data sets and the file to save the model:</span></span>

* <span data-ttu-id="a1133-160">`_trainDataPath` contiene la ruta de acceso al archivo con el conjunto de datos utilizado para entrenar el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-160">`_trainDataPath` contains the path to the file with the data set used to train the model.</span></span>
* <span data-ttu-id="a1133-161">`_testDataPath` contiene la ruta de acceso al archivo con el conjunto de datos utilizado para evaluar el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-161">`_testDataPath` contains the path to the file with the data set used to evaluate the model.</span></span>
* <span data-ttu-id="a1133-162">`_modelPath` contiene la ruta de acceso al archivo en el que se almacena el modelo entrenado.</span><span class="sxs-lookup"><span data-stu-id="a1133-162">`_modelPath` contains the path to the file where the trained model is stored.</span></span>

<span data-ttu-id="a1133-163">Agregue el código siguiente justo encima del método `Main` para especificar esas rutas de acceso y la variable `_textLoader`:</span><span class="sxs-lookup"><span data-stu-id="a1133-163">Add the following code right above the `Main` method to specify those paths and for the `_textLoader` variable:</span></span>

[!code-csharp[InitializePaths](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#2 "Define variables to store the data file paths")]

<span data-ttu-id="a1133-164">Todas las operaciones de ML.NET se inician en la [clase MLContext](xref:Microsoft.ML.MLContext).</span><span class="sxs-lookup"><span data-stu-id="a1133-164">All ML.NET operations start in the [MLContext class](xref:Microsoft.ML.MLContext).</span></span> <span data-ttu-id="a1133-165">La inicialización de `mlContext` crea un entorno de ML.NET que se puede compartir entre los objetos del flujo de trabajo de creación de modelos.</span><span class="sxs-lookup"><span data-stu-id="a1133-165">Initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="a1133-166">Como concepto, se parece a `DBContext` en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a1133-166">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="a1133-167">Inicializar variables en Main</span><span class="sxs-lookup"><span data-stu-id="a1133-167">Initialize variables in Main</span></span>

<span data-ttu-id="a1133-168">Reemplace la línea `Console.WriteLine("Hello World!")` del método `Main` por el siguiente código para declarar e inicializar la variable `mlContext`:</span><span class="sxs-lookup"><span data-stu-id="a1133-168">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the `mlContext` variable:</span></span>

[!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#3 "Create the ML Context")]

<span data-ttu-id="a1133-169">Agregue lo siguiente como la siguiente línea de código en el método `Main` para llamar al método `Train`:</span><span class="sxs-lookup"><span data-stu-id="a1133-169">Add the following as the next line of code in the `Main` method to call the `Train` method:</span></span>

[!code-csharp[Train](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#5 "Train your model")]

<span data-ttu-id="a1133-170">El método `Train()` ejecuta las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1133-170">The `Train()` method executes the following tasks:</span></span>

* <span data-ttu-id="a1133-171">Carga los datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-171">Loads the data.</span></span>
* <span data-ttu-id="a1133-172">Extrae y transforma los datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-172">Extracts and transforms the data.</span></span>
* <span data-ttu-id="a1133-173">Entrena el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-173">Trains the model.</span></span>
* <span data-ttu-id="a1133-174">Devuelve el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-174">Returns the model.</span></span>

<span data-ttu-id="a1133-175">El método `Train` entrena el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-175">The `Train` method trains the model.</span></span> <span data-ttu-id="a1133-176">Cree ese método justo debajo de `Main` utilizando el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-176">Create that method just below `Main`, using the following code:</span></span>

```csharp
public static ITransformer Train(MLContext mlContext, string dataPath)
{

}
```

## <a name="load-and-transform-data"></a><span data-ttu-id="a1133-177">Cargar y transformar datos</span><span class="sxs-lookup"><span data-stu-id="a1133-177">Load and transform data</span></span>

<span data-ttu-id="a1133-178">ML.NET usa la [clase IDataView](xref:Microsoft.ML.IDataView) como una forma flexible y eficaz de describir datos tabulares de texto o numéricos.</span><span class="sxs-lookup"><span data-stu-id="a1133-178">ML.NET uses the [IDataView class](xref:Microsoft.ML.IDataView) as a flexible, efficient way of describing numeric or text tabular data.</span></span> <span data-ttu-id="a1133-179">`IDataView` puede cargar archivos de texto o en tiempo real (por ejemplo, una base de datos SQL o archivos de registro).</span><span class="sxs-lookup"><span data-stu-id="a1133-179">`IDataView` can load either text files or in real time (for example, SQL database or log files).</span></span> <span data-ttu-id="a1133-180">Agregue el código siguiente a la primera línea del método `Train()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-180">Add the following code as the first line of the `Train()` method:</span></span>

[!code-csharp[LoadTrainData](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#6 "loading training dataset")]

<span data-ttu-id="a1133-181">Como quiere predecir la tarifa de carreras de taxi, la columna `FareAmount` es la `Label` que va a predecir (la salida del modelo). Use la clase de transformación `CopyColumnsEstimator` para copiar `FareAmount` y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-181">As you want to predict the taxi trip fare, the `FareAmount` column is the `Label` that you will predict (the output of the model)Use the `CopyColumnsEstimator` transformation class to copy `FareAmount`, and add the following code:</span></span> 

[!code-csharp[CopyColumnsEstimator](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#7 "Use the CopyColumnsEstimator")]

<span data-ttu-id="a1133-182">El algoritmo que entrena el modelo requiere características **numéricas**, por lo que debe transformar los datos de categorías (`VendorId`, `RateCode` y `PaymentType`) en números (`VendorIdEncoded`, `RateCodeEncoded` y `PaymentTypeEncoded`).</span><span class="sxs-lookup"><span data-stu-id="a1133-182">The algorithm that trains the model requires **numeric** features, so you have to transform the categorical data (`VendorId`, `RateCode`, and `PaymentType`) values into numbers (`VendorIdEncoded`, `RateCodeEncoded`, and `PaymentTypeEncoded`).</span></span> <span data-ttu-id="a1133-183">Para ello, use la clase de transformación [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer), que asigna diferentes valores de clave numéricos a los distintos valores de cada una de las columnas y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-183">To do that, use the [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer) transformation class, which assigns different numeric key values to the different values in each of the columns, and add the following code:</span></span>

[!code-csharp[OneHotEncodingEstimator](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#8 "Use the OneHotEncodingEstimator")]

<span data-ttu-id="a1133-184">En el último paso para la preparación de los datos, se combinan todas las columnas de característica en la columna **Características** mediante la clase de transformación `mlContext.Transforms.Concatenate`.</span><span class="sxs-lookup"><span data-stu-id="a1133-184">The last step in data preparation combines all of the feature columns into the **Features** column using the `mlContext.Transforms.Concatenate` transformation class.</span></span> <span data-ttu-id="a1133-185">De forma predeterminada, un algoritmo de aprendizaje solo procesa las características de la columna **Features**.</span><span class="sxs-lookup"><span data-stu-id="a1133-185">By default, a learning algorithm processes only features from the **Features** column.</span></span> <span data-ttu-id="a1133-186">Agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-186">Add the following code:</span></span>

[!code-csharp[ColumnConcatenatingEstimator](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#9 "Use the ColumnConcatenatingEstimator")]

## <a name="choose-a-learning-algorithm"></a><span data-ttu-id="a1133-187">Elegir un algoritmo de aprendizaje</span><span class="sxs-lookup"><span data-stu-id="a1133-187">Choose a learning algorithm</span></span>

<span data-ttu-id="a1133-188">Este problema consiste en predecir la tarifa de una carrera de taxi en la ciudad de Nueva York.</span><span class="sxs-lookup"><span data-stu-id="a1133-188">This problem is about predicting a taxi trip fare in New York City.</span></span> <span data-ttu-id="a1133-189">A primera vista, puede parecer que simplemente depende de la distancia recorrida.</span><span class="sxs-lookup"><span data-stu-id="a1133-189">At first glance, it may seem to depend simply on the distance traveled.</span></span> <span data-ttu-id="a1133-190">Sin embargo, los taxistas de Nueva York cobran distintas cantidades por otros factores, como llevar pasajeros adicionales o pagar con tarjeta de crédito en lugar de efectivo.</span><span class="sxs-lookup"><span data-stu-id="a1133-190">However, taxi vendors in New York charge varying amounts for other factors such as additional passengers or paying with a credit card instead of cash.</span></span> <span data-ttu-id="a1133-191">Quiere predecir el valor de precio, que es un valor real, en función de los demás factores del conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-191">You want to predict the price value, which is a real value, based on the other factors in the dataset.</span></span> <span data-ttu-id="a1133-192">Para ello, se elige una tarea de aprendizaje automático de [regresión](../resources/glossary.md#regression).</span><span class="sxs-lookup"><span data-stu-id="a1133-192">To do that, you choose a [regression](../resources/glossary.md#regression) machine learning task.</span></span>

<span data-ttu-id="a1133-193">Anexe la tarea de aprendizaje automático [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) a las definiciones de transformación de datos al agregar esto como siguiente línea de código de `Train()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-193">Append the [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) machine learning task to the data transformation definitions by adding the following as the next line of code in `Train()`:</span></span>

[!code-csharp[FastTreeRegressionTrainer](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#10 "Add the FastTreeRegressionTrainer")]

## <a name="train-the-model"></a><span data-ttu-id="a1133-194">Entrenar el modelo</span><span class="sxs-lookup"><span data-stu-id="a1133-194">Train the model</span></span>

<span data-ttu-id="a1133-195">Ajuste el modelo a la `dataview` de entrenamiento y devuelva el modelo entrenado agregando la siguiente línea de código en el método `Train()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-195">Fit the model to the training `dataview` and return the trained model by adding the following line of code in the `Train()` method:</span></span>

[!code-csharp[TrainModel](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#11 "Train the model")]

<span data-ttu-id="a1133-196">El método [Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) entrena el modelo al transformar el conjunto de datos y aplicar el aprendizaje.</span><span class="sxs-lookup"><span data-stu-id="a1133-196">The [Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) method trains your model by transforming the dataset and applying the training.</span></span>

<span data-ttu-id="a1133-197">Devuelva el modelo entrenado con la siguiente línea de código en el método `Train()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-197">Return the trained model with the following line of code in the `Train()` method:</span></span>

[!code-csharp[ReturnModel](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#12 "Return the model")]

## <a name="evaluate-the-model"></a><span data-ttu-id="a1133-198">Evaluar el modelo</span><span class="sxs-lookup"><span data-stu-id="a1133-198">Evaluate the model</span></span>

<span data-ttu-id="a1133-199">Luego evalúe el rendimiento del modelo con los datos de prueba de control de calidad y validación.</span><span class="sxs-lookup"><span data-stu-id="a1133-199">Next, evaluate your model performance with your test data for quality assurance and validation.</span></span> <span data-ttu-id="a1133-200">Cree el método `Evaluate()`, justo después de `Train()`, con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-200">Create the `Evaluate()` method, just after `Train()`, with the following code:</span></span>

```csharp
private static void Evaluate(MLContext mlContext, ITransformer model)
{

}
```

<span data-ttu-id="a1133-201">El método `Evaluate` ejecuta las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1133-201">The `Evaluate` method executes the following tasks:</span></span>

* <span data-ttu-id="a1133-202">Carga el conjunto de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="a1133-202">Loads the test dataset.</span></span>
* <span data-ttu-id="a1133-203">Crea el evaluador de regresión.</span><span class="sxs-lookup"><span data-stu-id="a1133-203">Creates the regression evaluator.</span></span>
* <span data-ttu-id="a1133-204">Evalúa el modelo y crea las métricas.</span><span class="sxs-lookup"><span data-stu-id="a1133-204">Evaluates the model and creates metrics.</span></span>
* <span data-ttu-id="a1133-205">Muestra las métricas.</span><span class="sxs-lookup"><span data-stu-id="a1133-205">Displays the metrics.</span></span>

<span data-ttu-id="a1133-206">Agregue una llamada al método nuevo desde el método `Main`, justo debajo de la llamada al método `Train`, utilizando el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-206">Add a call to the new method from the `Main` method, right under the `Train` method call, using the following code:</span></span>

[!code-csharp[CallEvaluate](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#14 "Call the Evaluate method")]

<span data-ttu-id="a1133-207">Cargue el conjunto de datos de prueba con el método [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A).</span><span class="sxs-lookup"><span data-stu-id="a1133-207">Load the test dataset using the [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) method.</span></span> <span data-ttu-id="a1133-208">Evalúe el modelo con este conjunto de datos como control de calidad al agregar el código siguiente en el método `Evaluate`:</span><span class="sxs-lookup"><span data-stu-id="a1133-208">Evaluate the model using this dataset as a quality check by adding the following code in the `Evaluate` method:</span></span>

[!code-csharp[LoadTestDataset](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#15 "Load the test dataset")]

<span data-ttu-id="a1133-209">Luego transforme los datos de `Test` al agregar el código siguiente a `EvaluateModel()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-209">Next, transform the `Test` data by adding the following code to `EvaluateModel()`:</span></span>

[!code-csharp[PredictWithTransformer](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#16 "Predict using the Transformer")]

<span data-ttu-id="a1133-210">El método [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) realiza predicciones para las filas de entrada del conjunto de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="a1133-210">The [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method makes predictions for the test dataset input rows.</span></span>

<span data-ttu-id="a1133-211">El método `RegressionContext.Evaluate` calcula las métricas de calidad para `PredictionModel` mediante el conjunto de datos especificado.</span><span class="sxs-lookup"><span data-stu-id="a1133-211">The `RegressionContext.Evaluate` method computes the quality metrics for the `PredictionModel` using the specified dataset.</span></span> <span data-ttu-id="a1133-212">Devuelve un objeto <xref:Microsoft.ML.Data.RegressionMetrics> que contiene las métricas totales calculadas por evaluadores de regresión.</span><span class="sxs-lookup"><span data-stu-id="a1133-212">It returns a <xref:Microsoft.ML.Data.RegressionMetrics> object that contains the overall metrics computed by regression evaluators.</span></span> 

<span data-ttu-id="a1133-213">Para mostrar estos elementos a fin de determinar la calidad del modelo, debe obtener primero las métricas.</span><span class="sxs-lookup"><span data-stu-id="a1133-213">To display these to determine the quality of the model, you need to get the metrics first.</span></span> <span data-ttu-id="a1133-214">Agregue el siguiente código como la siguiente línea en el método `Evaluate`:</span><span class="sxs-lookup"><span data-stu-id="a1133-214">Add the following code as the next line in the `Evaluate` method:</span></span>

[!code-csharp[ComputeMetrics](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#17 "Compute Metrics")]

<span data-ttu-id="a1133-215">Una vez que se ha establecido la predicción, el método [Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) valora el modelo, que compara los valores predichos con las `Labels` reales del conjunto de datos de prueba y devuelve métricas sobre el rendimiento del modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-215">Once you have the prediction set, the [Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) method assesses the model, which compares the predicted values with the actual `Labels` in the test dataset and returns metrics on how the model is performing.</span></span>

<span data-ttu-id="a1133-216">Agregue el código siguiente para evaluar el modelo y generar las métricas de evaluación:</span><span class="sxs-lookup"><span data-stu-id="a1133-216">Add the following code to evaluate the model and produce the evaluation metrics:</span></span>

```csharp
Console.WriteLine();
Console.WriteLine($"*************************************************");
Console.WriteLine($"*       Model quality metrics evaluation         ");
Console.WriteLine($"*------------------------------------------------");
```

<span data-ttu-id="a1133-217">[RSquared](../resources/glossary.md#coefficient-of-determination) es otra métrica de evaluación de modelos de regresión.</span><span class="sxs-lookup"><span data-stu-id="a1133-217">[RSquared](../resources/glossary.md#coefficient-of-determination) is another evaluation metric of the regression models.</span></span> <span data-ttu-id="a1133-218">RSquared muestra valores comprendidos entre 0 y 1.</span><span class="sxs-lookup"><span data-stu-id="a1133-218">RSquared takes values between 0 and 1.</span></span> <span data-ttu-id="a1133-219">Cuanto más se acerque el valor a 1, mejor será el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-219">The closer its value is to 1, the better the model is.</span></span> <span data-ttu-id="a1133-220">Agregue el código siguiente al método `Evaluate` para mostrar el valor de RSquared:</span><span class="sxs-lookup"><span data-stu-id="a1133-220">Add the following code into the `Evaluate` method to display the RSquared value:</span></span>

[!code-csharp[DisplayRSquared](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#18 "Display the RSquared metric.")]

<span data-ttu-id="a1133-221">[RMS](../resources/glossary.md##root-of-mean-squared-error-rmse) es una de las métricas de evaluación del modelo de regresión.</span><span class="sxs-lookup"><span data-stu-id="a1133-221">[RMS](../resources/glossary.md##root-of-mean-squared-error-rmse) is one of the evaluation metrics of the regression model.</span></span> <span data-ttu-id="a1133-222">Cuanto menor sea su valor, mejor será el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-222">The lower it is, the better the model is.</span></span> <span data-ttu-id="a1133-223">Agregue el código siguiente al método `Evaluate` para mostrar el valor de RMS:</span><span class="sxs-lookup"><span data-stu-id="a1133-223">Add the following code into the `Evaluate` method to display the RMS value:</span></span>

[!code-csharp[DisplayRMS](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#19 "Display the RMS metric.")]

## <a name="use-the-model-for-predictions"></a><span data-ttu-id="a1133-224">Usar el modelo para las predicciones</span><span class="sxs-lookup"><span data-stu-id="a1133-224">Use the model for predictions</span></span>

<span data-ttu-id="a1133-225">Cree el método `TestSinglePrediction`, justo después del método `Evaluate`, mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-225">Create the `TestSinglePrediction` method, just after the `Evaluate` method, using the following code:</span></span>

```csharp
private static void TestSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

<span data-ttu-id="a1133-226">El método `TestSinglePrediction` ejecuta las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1133-226">The `TestSinglePrediction` method executes the following tasks:</span></span>

* <span data-ttu-id="a1133-227">Crea un único comentario de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="a1133-227">Creates a single comment of test data.</span></span>
* <span data-ttu-id="a1133-228">Predice la tarifa según los datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="a1133-228">Predicts fare amount based on test data.</span></span>
* <span data-ttu-id="a1133-229">Combina datos de prueba y predicciones para la generación de informes.</span><span class="sxs-lookup"><span data-stu-id="a1133-229">Combines test data and predictions for reporting.</span></span>
* <span data-ttu-id="a1133-230">Muestra los resultados de la predicción.</span><span class="sxs-lookup"><span data-stu-id="a1133-230">Displays the predicted results.</span></span>

<span data-ttu-id="a1133-231">Agregue una llamada al método nuevo desde el método `Main`, justo debajo de la llamada al método `Evaluate`, utilizando el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1133-231">Add a call to the new method from the `Main` method, right under the `Evaluate` method call, using the following code:</span></span>

[!code-csharp[CallTestSinglePrediction](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#20 "Call the TestSinglePrediction method")]

<span data-ttu-id="a1133-232">Use `PredictionEngine` para predecir la tarifa al agregar el código siguiente a `TestSinglePrediction()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-232">Use the `PredictionEngine` to predict the fare by adding the following code to `TestSinglePrediction()`:</span></span>

[!code-csharp[MakePredictionEngine](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#22 "Create the PredictionFunction")]

<span data-ttu-id="a1133-233">La [clase PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) es una API de conveniencia que permite pasar una sola instancia de datos y luego realizar una predicción sobre ella.</span><span class="sxs-lookup"><span data-stu-id="a1133-233">The [PredictionEngine class](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to pass a single instance of data and then perform a prediction on it.</span></span>

<span data-ttu-id="a1133-234">En este tutorial se utiliza un viaje de prueba en esta clase.</span><span class="sxs-lookup"><span data-stu-id="a1133-234">This tutorial uses one test trip within this class.</span></span> <span data-ttu-id="a1133-235">Más adelante, puede agregar otros escenarios para experimentar con el modelo.</span><span class="sxs-lookup"><span data-stu-id="a1133-235">Later you can add other scenarios to experiment with the model.</span></span> <span data-ttu-id="a1133-236">Agregue un viaje para probar la predicción de costo del modelo entrenado en el método `TestSinglePrediction()` mediante la creación de una instancia de `TaxiTrip`:</span><span class="sxs-lookup"><span data-stu-id="a1133-236">Add a trip to test the trained model's prediction of cost in the `TestSinglePrediction()` method by creating an instance of `TaxiTrip`:</span></span>

[!code-csharp[PredictionData](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#23 "Create test data for single prediction")]

<span data-ttu-id="a1133-237">Luego, prediga el importe del servicio según una instancia única de los datos de carreras de taxi y páselo a la clase `PredictionEngine` agregando lo siguiente como próximas líneas de código en el método `TestSinglePrediction()`:</span><span class="sxs-lookup"><span data-stu-id="a1133-237">Next, predict the fare based on a single instance of the taxi trip data and pass it to the `PredictionEngine` by adding the following as the next lines of code in the `TestSinglePrediction()` method:</span></span>

[!code-csharp[Predict](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#24 "Create a prediction of taxi fare")]

<span data-ttu-id="a1133-238">La función [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) realiza una predicción sobre una única instancia de datos.</span><span class="sxs-lookup"><span data-stu-id="a1133-238">The [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) function makes a prediction on a single instance of data.</span></span>

<span data-ttu-id="a1133-239">Para mostrar la tarifa prevista del viaje especificado, agregue el código siguiente al método `TestSinglePrediction`:</span><span class="sxs-lookup"><span data-stu-id="a1133-239">To display the predicted fare of the specified trip, add the following code into the `TestSinglePrediction` method:</span></span>

[!code-csharp[Predict](~/samples/machine-learning/tutorials/TaxiFarePrediction/Program.cs#25 "Display the prediction.")]

<span data-ttu-id="a1133-240">Ejecute el programa para ver la tarifa de taxi predicha para su caso de prueba.</span><span class="sxs-lookup"><span data-stu-id="a1133-240">Run the program to see the predicted taxi fare for your test case.</span></span>

<span data-ttu-id="a1133-241">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="a1133-241">Congratulations!</span></span> <span data-ttu-id="a1133-242">Ya ha creado correctamente un modelo de aprendizaje automático para predecir tarifas de viajes en taxi, ha evaluado su precisión y lo ha usado para hacer predicciones.</span><span class="sxs-lookup"><span data-stu-id="a1133-242">You've now successfully built a machine learning model for predicting taxi trip fares, evaluated its accuracy, and used it to make predictions.</span></span> <span data-ttu-id="a1133-243">Puede encontrar el código fuente de este tutorial en el repositorio [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TaxiFarePrediction) de GitHub.</span><span class="sxs-lookup"><span data-stu-id="a1133-243">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TaxiFarePrediction) GitHub repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1133-244">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a1133-244">Next steps</span></span>

<span data-ttu-id="a1133-245">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="a1133-245">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a1133-246">Preparar y entender los datos</span><span class="sxs-lookup"><span data-stu-id="a1133-246">Prepare and understand the data</span></span>
> * <span data-ttu-id="a1133-247">Crear una canalización de aprendizaje</span><span class="sxs-lookup"><span data-stu-id="a1133-247">Create a learning pipeline</span></span>
> * <span data-ttu-id="a1133-248">Cargar y transformar los datos</span><span class="sxs-lookup"><span data-stu-id="a1133-248">Load and transform the data</span></span>
> * <span data-ttu-id="a1133-249">Elegir un algoritmo de aprendizaje</span><span class="sxs-lookup"><span data-stu-id="a1133-249">Choose a learning algorithm</span></span>
> * <span data-ttu-id="a1133-250">Entrenar el modelo</span><span class="sxs-lookup"><span data-stu-id="a1133-250">Train the model</span></span>
> * <span data-ttu-id="a1133-251">Evaluar el modelo</span><span class="sxs-lookup"><span data-stu-id="a1133-251">Evaluate the model</span></span>
> * <span data-ttu-id="a1133-252">Usar el modelo para las predicciones</span><span class="sxs-lookup"><span data-stu-id="a1133-252">Use the model for predictions</span></span>

<span data-ttu-id="a1133-253">Siga con el siguiente tutorial para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a1133-253">Advance to the next tutorial to learn more.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a1133-254">Agrupación en clústeres de iris</span><span class="sxs-lookup"><span data-stu-id="a1133-254">Iris clustering</span></span>](iris-clustering.md)