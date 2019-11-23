---
title: 'Tutorial: Monitor an IoT device space - Azure Digital Twins| Microsoft Docs'
description: Learn how to provision your spatial resources and monitor the working conditions with Azure Digital Twins by using the steps in this tutorial.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 11/13/2019
ms.openlocfilehash: 80fd1275f3bf9585ff8e40a94d0de2d422baec71
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383235"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins-preview"></a>Tutorial: Provision your building and monitor working conditions with Azure Digital Twins Preview

This tutorial demonstrates how to use Azure Digital Twins Preview to monitor your spaces for desired temperature conditions and comfort level. After you [configure your sample building](tutorial-facilities-setup.md), you can provision your building and run custom functions on your sensor data by using the steps in this tutorial.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Define conditions to monitor.
> * Create a user-defined function (UDF).
> * Simulate sensor data.
> * Get results of a user-defined function.

## <a name="prerequisites"></a>Önkoşullar

This tutorial assumes that you have [finished your Azure Digital Twins setup](tutorial-facilities-setup.md). Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir Digital Twins örneği. 
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp). 
- [.NET Core SDK version 2.1.403 or later](https://www.microsoft.com/net/download) on your development machine to build and run the sample. Run `dotnet --version` to verify that the right version is installed. 
- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

> [!TIP]
> Use a unique Digital Twins instance name if you're provisioning a new instance.

## <a name="define-conditions-to-monitor"></a>İzleme koşullarını tanımlama

You can define a set of specific conditions to monitor in the device or sensor data, called *matchers*. You can then define functions called *user-defined functions*. User-defined functions execute custom logic on data that comes from your spaces and devices, when the conditions specified by the matchers occur. For more information, read [Data processing and user-defined functions](concepts-user-defined-functions.md). 

From the **occupancy-quickstart** sample project, open the file **src\actions\provisionSample.yaml** in Visual Studio Code. **matchers** türü ile başlayan bölümü bulun. Each entry under this type creates a matcher with the specified **Name**. The matcher will monitor a sensor of type **dataTypeValue**. Notice how it relates to the space named *Focus Room A1*, which has a **devices** node that contains a few sensors. To provision a matcher that will track one of these sensors, make sure that its **dataTypeValue** matches the sensor's **dataType**. 

Add the following matcher below the existing matchers. Make sure the keys are aligned and spaces are not replaced by tabs. These lines are also present in the *provisionSample.yaml* file as commented-out lines. You can uncomment them by removing the `#` character in front of each line.

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

This matcher will track the `SAMPLE_SENSOR_TEMPERATURE` sensor that you added in [the first tutorial](tutorial-facilities-setup.md). 

## <a name="create-a-user-defined-function"></a>Kullanıcı tanımlı işlev oluşturma

You can use user-defined functions to customize the processing of your sensor data. They're custom JavaScript code that can run within your Azure Digital Twins instance, when specific conditions as described by the matchers occur. You can create matchers and user-defined functions for each sensor that you want to monitor. For more information, read [Data processing and user-defined functions](concepts-user-defined-functions.md). 

In the sample *provisionSample.yaml* file, look for a section that begins with the type **userdefinedfunctions**. This section provisions a user-defined function with a given **Name**. This UDF acts on the list of matchers under **matcherNames**. UDF için kendi JavaScript dosyanızı **script** bölümünde sağlayabilirsiniz.

Ayrıca **roleassignments** adlı bölüme de dikkat edin. It assigns the Space Administrator role to the user-defined function. This role allows it to access the events that come from any of the provisioned spaces. 

1. *provisionSample.yaml* adlı dosyanın `matcherNames` düğümüne aşağıdaki satırı ekleyerek veya var olan satırın açıklamasını kaldırarak UDF'yi sıcaklık eşleştiricisini içerecek şekilde yapılandırın:

    ```yaml
            - Matcher Temperature
    ```

1. Open the file **src\actions\userDefinedFunctions\availability.js** in your editor. This is the file referenced in the **script** element of *provisionSample.yaml*. The user-defined function in this file looks for conditions when no motion is detected in the room and carbon dioxide levels are below 1,000 ppm. 

   Modify the JavaScript file to monitor temperature and other conditions. Add the following lines of code to look for conditions when no motion is detected in the room, carbon dioxide levels are below 1,000 ppm, and temperature is below 78 degrees Fahrenheit.

   > [!NOTE]
   > This section modifies the file *src\actions\userDefinedFunctions\availability.js* so you can learn in detail one way to write a user-defined function. However, you can choose to directly use the file [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) in your setup. Bu dosya, öğretici için gerekli olan tüm değişikliklere sahiptir. If you use this file instead, make sure to use the correct file name for the **script** key in [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

    a. Dosyanın en üstünde, `// Add your sensor type here` açıklamasının altına sıcaklık için şu satırları ekleyin:

    ```JavaScript
        var temperatureType = "Temperature";
        var temperatureThreshold = 78;
    ```

    b. Add the following lines after the statement that defines `var motionSensor`, below the comment `// Add your sensor variable here`:

     ```JavaScript
        var temperatureSensor = otherSensors.find(function(element) {
            return element.DataType === temperatureType;
        });
    ```

    c. Add the following line after the statement that defines `var carbonDioxideValue`, below the comment `// Add your sensor latest value here`:

    ```JavaScript
        var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
    ```

    d. Şu kod satırlarını `// Modify this line to monitor your sensor value` açıklamasının altından kaldırın:

     ```JavaScript
        if(carbonDioxideValue === null || motionValue === null) {
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
            return;
        }
    ```

    Bunları şu satırlarla değiştirin:

    ```JavaScript
        if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
            return;
        }
    ```

    e. Şu kod satırlarını `// Modify these lines as per your sensor` açıklamasının altından kaldırın:

    ```JavaScript
        var availableFresh = "Room is available and air is fresh";
        var noAvailableOrFresh = "Room is not available or air quality is poor";
    ```

    Bunları şu satırlarla değiştirin:

    ```JavaScript
        var alert = "Room with fresh air and comfortable temperature is available.";
        var noAlert = "Either room is occupied, or working conditions are not right.";
    ```

    f. Şu *if-else* kod bloğunu `// Modify this code block for your sensor` açıklamasının altından kaldırın:

    ```JavaScript
        // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
        if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
            log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);
        }
        else {
            log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

            // Set up custom notification for poor air quality
            parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
        }
    ```

    Sonrasında şu *if-else* bloğuyla değiştirin:

    ```JavaScript
        // If sensor values are within range and room is available
        if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
            log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

            // Set up notification for this alert
            parentSpace.Notify(JSON.stringify(alert));
        }
        else {
            log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
        }
    ```

    Değiştirilen UDF, bir odanın kullanılabilir durumda olması, karbondioksit ve sıcaklık sınırlarının kabul edilen sınırlar içinde olması durumunu izler. Bu koşul yerine getirildiğinde `parentSpace.Notify(JSON.stringify(alert));` deyimiyle bir bildirim oluşturur. Sağlanan koşuldan bağımsız olarak izlenen alanın değerini ayarlayacak ve aşağıdaki iletiyi görüntüleyecektir.

    g. Dosyayı kaydedin.

1. Open a command window, and go to the folder **occupancy-quickstart\src**. Run the following command to provision your spatial intelligence graph and user-defined function:

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > To prevent unauthorized access to your Digital Twins Management API, the **occupancy-quickstart** application requires you to sign in with your Azure account credentials. It saves your credentials for a brief period, so you might not need to sign in every time you run it. The first time this program runs, and when your saved credentials expire after that, the application directs you to a sign-in page and gives a session-specific code to enter on that page. Azure hesabınızda oturum açmak için yönergeleri izleyin.

1. After your account is authenticated, the application starts creating a sample spatial graph as configured in *provisionSample.yaml*. Wait until the provisioning finishes. It will take a few minutes. After that, observe the messages in the command window and notice how your spatial graph is created. Notice how the application creates an IoT hub at the root node or the `Venue`.

1. From the output in the command window, copy the value of `ConnectionString`, under the `Devices` section, to your clipboard. You'll need this value to simulate the device connection in the next section.

    [![Provision sample](./media/tutorial-facilities-udf/run-provision-sample.png)](./media/tutorial-facilities-udf/run-provision-sample.png#lightbox)

> [!TIP]
> If you get an error message similar to "The I/O operation has been aborted because of either a thread exit or an application request" in the middle of the provisioning, try running the command again. This might happen if the HTTP client timed out from a network issue.

## <a name="simulate-sensor-data"></a>Sensör verilerinin simülasyonunu yapma

In this section, you'll use the project named *device-connectivity* in the sample. You'll simulate sensor data for detecting motion, temperature, and carbon dioxide. Bu proje, sensörler için rastgele değerler oluşturur ve bunları cihaz bağlantı dizesini kullanarak IoT hub'a gönderir.

1. In a separate command window, go to the Azure Digital Twins sample and then to the **device-connectivity** folder.

1. Projenizin bağımlılıklarının doğru olduğundan emin olmak için şu komutu çalıştırın:

    ```cmd/sh
    dotnet restore
    ```

1. Open the [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) file in your editor, and edit the following values:

   a. **DeviceConnectionString**: Bir önceki bölümde çıktı penceresinde görünen `ConnectionString` değerini atayın. Copy this string completely, within the quotes, so the simulator can connect properly with the IoT hub.

   b. **HardwareId** within the **Sensors** array: Because you're simulating events from sensors provisioned to your Azure Digital Twins instance, the hardware ID and the names of the sensors in this file should match the `sensors` node of the *provisionSample.yaml* file.

      Add a new entry for the temperature sensor. The **Sensors** node in *appsettings.json* should look like the following:

      ```JSON
      "Sensors": [{
        "DataType": "Motion",
        "HardwareId": "SAMPLE_SENSOR_MOTION"
      },{
        "DataType": "CarbonDioxide",
        "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
      },{
        "DataType": "Temperature",
        "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
      }]
      ```

1. Sıcaklık, hareket ve karbondioksit için cihaz olayı simülasyonunu başlatmak üzere şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE]
   > Because the simulation sample does not directly communicate with your Digital Twins instance, it does not require you to authenticate.

## <a name="get-results-of-the-user-defined-function"></a>Get results of the user-defined function

Örneğiniz cihaz ve sensör verilerini her aldığında kullanıcı tanımlı işlev çalışır. This section queries your Azure Digital Twins instance to get the results of the user-defined function. You'll see in near real time, when a room is available, that the air is fresh and temperature is right. 

1. Open the command window that you used to provision the sample, or a new command window, and go to the **occupancy-quickstart\src** folder of the sample again.

1. Aşağıdaki komutu çalıştırın ve istendiğinde oturum açın:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

The output window shows how the user-defined function runs and intercepts events from the device simulation. 

   [![Output for the UDF](./media/tutorial-facilities-udf/udf-running.png)](./media/tutorial-facilities-udf/udf-running.png#lightbox)

If the monitored condition is met, the user-defined function sets the value of the space with the relevant message, as we saw [earlier](#create-a-user-defined-function). The `GetAvailableAndFreshSpaces` function prints out the message on the console.

## <a name="clean-up-resources"></a>Kaynakları temizleme

If you want to stop exploring Azure Digital Twins at this point, feel free to delete resources created in this tutorial:

1. From the left menu in the [Azure portal](https://portal.azure.com), select **All resources**, select your Digital Twins resource group, and select **Delete**.

    > [!TIP]
    > If you experienced trouble deleting your Digital Twins instance, a service update has been rolled out with the fix. Please retry deleting your instance.

2. If necessary, delete the sample applications on your work machine.

## <a name="next-steps"></a>Sonraki adımlar

Now that you have provisioned your spaces and created a framework to trigger custom notifications, you can go to either of the following tutorials:

> [!div class="nextstepaction"]
> [Öğretici: Logic Apps'i kullanarak Azure Digital Twins alanlarınızdan bildirim alma](tutorial-facilities-events.md)

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirme ve analiz etme](tutorial-facilities-analyze.md)
