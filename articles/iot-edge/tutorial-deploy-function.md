---
title: Tutorial deploy an Azure function to a device - Azure IoT Edge | Microsoft Docs
description: In this tutorial, you develop an Azure function as an IoT Edge module, then deploy it to an edge device.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1db64c2cc68dc9c47d4f2cf5f63eb0667907d737
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456722"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules"></a>Tutorial: Deploy Azure functions as IoT Edge modules

İş mantığınızı doğrudan Azure IoT Edge cihazlarınıza uygulayan kodu dağıtmak için Azure İşlevleri'ni kullanabilirsiniz. Bu öğreticide, benzetimi yapılan IoT Edge cihazındaki algılayıcı verilerini filtreleyen bir Azure işlevi oluşturma ve dağıtma işlemlerinde size yol gösterilir. [Windows](quickstart.md)'da veya [Linux](quickstart-linux.md)'ta bir simülasyon cihazına Azure IoT Edge dağıtma hızlı başlangıçlarında oluşturduğunuz simülasyon IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Visual Studio Code kullanarak Azure işlevi oluşturma.
> * VS Code ve Docker kullanarak Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinde yayımlama.
> * Kapsayıcı kayıt defterindeki modülü IoT Edge cihazınıza dağıtma.
> * Filtrelenmiş verileri görüntüleme.

<center>

![Diagram - Tutorial architecture, stage and deploy function module](./media/tutorial-deploy-function/functions-architecture.png)
</center>

>[!NOTE]
>Azure IoT Edge üzerindeki Azure İşlevi modülleri genel [önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) aşamasındadır. 

Bu öğreticide oluşturacağınız Azure işlevi, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İşlev, yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde Azure IoT Hub'a iletileri gönderir. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Before beginning this tutorial, you should have gone through the previous tutorial to set up your development environment for Linux container development: [Develop IoT Edge modules for Linux devices](tutorial-develop-for-linux.md). By completing that tutorial, you should have the following prerequisites in place: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Linux device running Azure IoT Edge](quickstart-linux.md)
* A container registry, like [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) configured with the [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) configured to run Linux containers.

To develop an IoT Edge module in with Azure Functions, install the following additional prerequisites on your development machine: 

* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

The Azure IoT Tools for Visual Studio Code that you installed in the prerequisites provides management capabilities as well as some code templates. Bu bölümde Visual Studio Code'u kullanarak Azure işlevi içeren bir IoT Edge çözümü oluşturacaksınız. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Create a C# Function solution template that you can customize with your own code.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. **View (Görünüm)**  > **Command Palette (Komut Paleti)** öğesini seçerek VS Code komut paletini açın.

3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Enter a descriptive name for your solution, like **FunctionSolution**, or accept the default. |
   | Modül şablonunu seçin | Choose **Azure Functions - C#** . |
   | Modül adı sağlayın | Modülünüze **CSharpFunction** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. The final string looks like \<registry name\>.azurecr.io/CSharpFunction. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-deploy-function/repository.png)

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Select your target architecture

Currently, Visual Studio Code can develop C modules for Linux AMD64 and Linux ARM32v7 devices. You need to select which architecture you're targeting with each solution, because the container is built and run differently for each architecture type. The default is Linux AMD64. 

1. Open the command palette and search for **Azure IoT Edge: Set Default Target Platform for Edge Solution**, or select the shortcut icon in the side bar at the bottom of the window. 

2. In the command palette, select the target architecture from the list of options. For this tutorial, we're using an Ubuntu virtual machine as the IoT Edge device, so will keep the default **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Let's add some additional code so that the module processes the messages at the edge before forwarding them to IoT Hub.

1. In Visual Studio Code, open **modules** > **CSharpFunction** > **CSharpFunction.cs**.

1. Replace the contents of the **CSharpFunction.cs** file with the following code. This code receives telemetry about ambient and machine temperature, and only forwards the message on to IoT Hub if the machine temperature is above a defined threshold.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Client;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.EdgeHub;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;
   using Newtonsoft.Json;

   namespace Functions.Samples
   {
       public static class CSharpFunction
       {
           [FunctionName("CSharpFunction")]
           public static async Task FilterMessageAndSendMessage(
               [EdgeHubTrigger("input1")] Message messageReceived,
               [EdgeHub(OutputName = "output1")] IAsyncCollector<Message> output,
               ILogger logger)
           {
               const int temperatureThreshold = 20;
               byte[] messageBytes = messageReceived.GetBytes();
               var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

               if (!string.IsNullOrEmpty(messageString))
               {
                   logger.LogInformation("Info: Received one non-empty message");
                   // Get the body of the message and deserialize it.
                   var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threshold.
                       using (var filteredMessage = new Message(messageBytes))
                       {
                            // Copy the properties of the original message into the new Message object.
                            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                            {filteredMessage.Properties.Add(prop.Key, prop.Value);}
                            // Add a new property to the message to indicate it is an alert.
                            filteredMessage.Properties.Add("MessageType", "Alert");
                            // Send the message.       
                            await output.AddAsync(filteredMessage);
                            logger.LogInformation("Info: Received and transferred a message with temperature above the threshold");
                       }
                   }
               }
           }
       }
       //Define the expected schema for the body of incoming messages.
       class MessageBody
       {
           public Machine machine {get; set;}
           public Ambient ambient {get; set;}
           public string timeCreated {get; set;}
       }
       class Machine
       {
           public double temperature {get; set;}
           public double pressure {get; set;}
       }
       class Ambient
       {
           public double temperature {get; set;}
           public int humidity {get; set;}
       }
   }
   ```

1. Dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve **CSharpFunction** modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtrelemek için kod eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

In this section, you provide the credentials for your container registry for the second time (the first was in the **.env** file of your IoT Edge solution) by signing in locally from your development machine so that Visual Studio Code can push images to your registry.

1. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın. 

2. Tümleşik terminale aşağıdaki komutu girerek kapsayıcı kayıt defterinizde oturum açın. Önceki adımlarda Azure kapsayıcı kayıt defterinden kopyaladığınız kullanıcı adını ve oturum açma sunucusunu kullanın.

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```

    When you're prompted for the password, paste the password (it won't be visible in the terminal window) for your container registry and press **Enter**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

3. VS Code gezgininde deployment.template.json dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

When you tell Visual Studio Code to build your solution, it first takes the information in the deployment template and generates a deployment.json file in a new folder named **config**. Then it runs two commands in the integrated terminal: `docker build` and `docker push`. Bu iki komut kodunuzu derler, işlevlerle kapsayıcı oluşturur ve kodu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

## <a name="view-your-container-image"></a>Kapsayıcı görüntünüzü açma

Kapsayıcı görüntünüz, kapsayıcı kayıt defterinize gönderildiğinde Visual Studio Code işlemin başarılı olduğunu belirten bir ileti görüntüler. İşlemin başarılı olduğunu kendiniz onaylamak isterseniz kayıt defterindeki görüntüye bakabilirsiniz. 

1. Azure portalında Azure kapsayıcı kayıt defterinize göz atın. 
2. **Depolar**'ı seçin.
3. Listede **csharpfunction** deposunu görüyor olmalısınız. Diğer ayrıntıları görmek için bu depoyu seçin.
4. **Etiketler** bölümünde **0.0.1-amd64** etiketini görmeniz gerekir. Bu etiket, derlediğiniz görüntünün sürümünü ve platformunu gösterir. Bu değerler CSharpFunction klasöründeki module.json dosyasında belirlenir. 

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

Hızlı başlangıçlarda yaptığınız gibi işlev modülünüzü IoT Edge cihazına dağıtmak için Azure portalını kullanabilirsiniz. Ayrıca modülleri Visual Studio Code'un içinden de dağıtabilir ve izleyebilirsiniz. The following sections use the Azure IoT Tools for VS Code that was listed in the prerequisites. Uzantıyı henüz yüklemediyseniz, şimdi yükleyin. 

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device**'ı (Tek bir cihaz için dağıtım oluştur) seçin. 

3. **CSharpFunction** modülünü içeren çözüm klasörüne göz atın. Open the config folder, select the **deployment.json** file, and then choose **Select Edge Deployment Manifest**.

4. **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü yenileyin. You should see the new **CSharpFunction** running along with the **SimulatedTemperatureSensor** module and the **$edgeAgent** and **$edgeHub**. It may take a few moments for the new modules to show up. Your IoT Edge device has to retrieve its new deployment information from IoT Hub, start the new containers, and then report the status back to IoT Hub. 

   ![Dağıtılan modülleri VS Code'da görüntüleme](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

You can see all of the messages that arrive at your IoT hub by running **Azure IoT Hub: Start Monitoring Built-in Event Endpoint** in the command palette.

IoT hub'ınıza belirli bir cihazdan gelen iletilerin gösterilmesi için görünüme filtre de uygulayabilirsiniz. Right-click the device in the **Azure IoT Hub Devices** section and select **Start Monitoring Built-in Event Endpoint**.

To stop monitoring messages, run the command **Azure IoT Hub: Stop Monitoring Built-in Event Endpoint** in the command palette. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir Azure işlevi modülü oluşturdunuz. When you're ready to build your own modules, you can learn more about how to [Develop with Azure IoT Edge for Visual Studio Code](how-to-vs-code-develop-module.md). 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yolları öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Stream Analytics'te kayan pencere kullanarak ortalamaları bulma](tutorial-deploy-stream-analytics.md)
