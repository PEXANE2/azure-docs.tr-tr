---
title: 'Öğretici: Azure işlevlerini modül olarak dağıtma-Azure IoT Edge'
description: Bu öğreticide, bir Azure işlevini IoT Edge modülü olarak geliştirirsiniz ve bir uç cihaza dağıtırsınız.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 8edd6028e73690829129168d912b312247d8b880
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2019
ms.locfileid: "74664123"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules"></a>Öğretici: Azure işlevlerini IoT Edge modülleri olarak dağıtma

İş mantığınızı doğrudan Azure IoT Edge cihazlarınıza uygulayan kodu dağıtmak için Azure İşlevleri'ni kullanabilirsiniz. Bu öğreticide, benzetimi yapılan IoT Edge cihazındaki algılayıcı verilerini filtreleyen bir Azure işlevi oluşturma ve dağıtma işlemlerinde size yol gösterilir. [Windows](quickstart.md)'da veya [Linux](quickstart-linux.md)'ta bir simülasyon cihazına Azure IoT Edge dağıtma hızlı başlangıçlarında oluşturduğunuz simülasyon IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Visual Studio Code kullanarak Azure işlevi oluşturma.
> * VS Code ve Docker kullanarak Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinde yayımlama.
> * Kapsayıcı kayıt defterindeki modülü IoT Edge cihazınıza dağıtma.
> * Filtrelenmiş verileri görüntüleme.

<center>

![diyagram-öğretici mimarisi, aşama ve dağıtım işlev modülü](./media/tutorial-deploy-function/functions-architecture.png)
</center>

>[!NOTE]
>Azure IoT Edge üzerindeki Azure İşlevi modülleri genel [önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) aşamasındadır. 

Bu öğreticide oluşturacağınız Azure işlevi, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İşlev, yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde Azure IoT Hub'a iletileri gönderir. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce, Linux kapsayıcı geliştirmesi için geliştirme ortamınızı ayarlamak üzere önceki öğreticiden çıkmalısınız: [Linux cihazları için IoT Edge modülleri](tutorial-develop-for-linux.md)geliştirme. Bu öğreticiyi tamamlayarak aşağıdaki önkoşulların yerine gelmelidir: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran bir Linux cihazı](quickstart-linux.md)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Azure IoT araçlarıyla](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)yapılandırılmış [Visual Studio Code](https://code.visualstudio.com/) .
* Linux kapsayıcılarını çalıştırmak için yapılandırılmış [Docker CE](https://docs.docker.com/install/) .

Azure Işlevleri ile ' de bir IoT Edge modülü geliştirmek için, aşağıdaki ek önkoşulları geliştirme makinenize yüklersiniz: 

* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

Önkoşullara yüklediğiniz Visual Studio Code için Azure IoT araçları, bazı kod şablonlarının yanı sıra yönetim özellikleri sağlar. Bu bölümde Visual Studio Code'u kullanarak Azure işlevi içeren bir IoT Edge çözümü oluşturacaksınız. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi kodunuzla özelleştirebilmeniz için bir C# işlev çözüm şablonu oluşturun.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. **View (Görünüm)**  > **Command Palette (Komut Paleti)** öğesini seçerek VS Code komut paletini açın.

3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için **Functionsolution**gibi açıklayıcı bir ad girin veya varsayılan değeri kabul edin. |
   | Modül şablonunu seçin | **Azure işlevleri C#** 'ni seçin. |
   | Modül adı sağlayın | Modülünüze **CSharpFunction** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. Son dize, \<\>. azurecr.io/CSharpFunction kayıt defteri adı gibi görünür. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-deploy-function/repository.png)

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Hedef mimarinizi seçin

Şu anda Visual Studio Code Linux AMD64 ve Linux ARM32v7 cihazları için C modülleri geliştirebilir. Kapsayıcı oluşturulup her mimari türü için farklı çalıştığından, her çözümle hedeflediğiniz mimariyi seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paleti ' ni açın ve Azure IoT Edge için arama yapın **: Edge çözümü Için varsayılan hedef platformunu ayarla**veya pencerenin altındaki yan çubukta kısayol simgesini seçin. 

2. Komut paletinde, seçenekler listesinden hedef mimariyi seçin. Bu öğreticide, IoT Edge cihaz olarak bir Ubuntu sanal makinesi kullanıyoruz, bu nedenle varsayılan **AMD64**'yi tutacağız. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Modülün iletileri IoT Hub iletmek için önce, daha fazla kod ekleyelim.

1. Visual Studio Code 'de, > **Csharpfunction** > **modüller** 'i açın.

1. **CSharpFunction.cs** dosyasının içeriğini aşağıdaki kodla değiştirin. Bu kod, çevresel ve makine sıcaklığı hakkında telemetri alır ve makine sıcaklığı tanımlı bir eşiğin üzerinde ise IoT Hub ileti yalnızca ' a iletir.

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

Bu bölümde, Visual Studio Code görüntüleri kayıt defterinize gönderebilmeleri için, kapsayıcı kayıt defterinizin kimlik bilgilerini ikinci kez (ilk olarak IoT Edge çözümünüzün **. env** dosyasında) sağlarsınız.

1. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın. 

2. Tümleşik terminale aşağıdaki komutu girerek kapsayıcı kayıt defterinizde oturum açın. Önceki adımlarda Azure kapsayıcı kayıt defterinden kopyaladığınız kullanıcı adını ve oturum açma sunucusunu kullanın.

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```

    Parola istendiğinde, kapsayıcı kayıt defteriniz için parolayı yapıştırın (Terminal penceresinde görünmez) ve **ENTER**tuşuna basın.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

3. VS Code gezgininde deployment.template.json dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

Çözümünüzü derlemek için Visual Studio Code söyleirken öncelikle dağıtım şablonundaki bilgileri alır ve **config**adlı yeni klasörde bir Deployment. JSON dosyası oluşturur. Ardından, tümleşik terminalde iki komut çalıştırır: `docker build` ve `docker push`. Bu iki komut kodunuzu derler, işlevlerle kapsayıcı oluşturur ve kodu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

## <a name="view-your-container-image"></a>Kapsayıcı görüntünüzü açma

Kapsayıcı görüntünüz, kapsayıcı kayıt defterinize gönderildiğinde Visual Studio Code işlemin başarılı olduğunu belirten bir ileti görüntüler. İşlemin başarılı olduğunu kendiniz onaylamak isterseniz kayıt defterindeki görüntüye bakabilirsiniz. 

1. Azure portalında Azure kapsayıcı kayıt defterinize göz atın. 
2. **Depolar**'ı seçin.
3. Listede **csharpfunction** deposunu görüyor olmalısınız. Diğer ayrıntıları görmek için bu depoyu seçin.
4. **Etiketler** bölümünde **0.0.1-amd64** etiketini görmeniz gerekir. Bu etiket, derlediğiniz görüntünün sürümünü ve platformunu gösterir. Bu değerler CSharpFunction klasöründeki module.json dosyasında belirlenir. 

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

Hızlı başlangıçlarda yaptığınız gibi işlev modülünüzü IoT Edge cihazına dağıtmak için Azure portalını kullanabilirsiniz. Ayrıca modülleri Visual Studio Code'un içinden de dağıtabilir ve izleyebilirsiniz. Aşağıdaki bölümlerde, önkoşullarda listelenen VS Code için Azure IoT araçları kullanılır. Uzantıyı henüz yüklemediyseniz, şimdi yükleyin. 

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device**'ı (Tek bir cihaz için dağıtım oluştur) seçin. 

3. **CSharpFunction** modülünü içeren çözüm klasörüne göz atın. Yapılandırma klasörünü açın, **Deployment. JSON** dosyasını seçin ve ardından **kenar dağıtım bildirimini Seç**' i seçin.

4. **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü yenileyin. **SimulatedTemperatureSensor** modülü ve **$edgeAgent** ve **$EdgeHub**birlikte çalışan yeni **csharpişlevini** görmeniz gerekir. Yeni modüllerin gösterilmesi birkaç dakika sürebilir. IoT Edge cihazınızın yeni dağıtım bilgilerini IoT Hub alma, yeni kapsayıcıları başlatma ve sonra durumu IoT Hub olarak bildirme. 

   ![Dağıtılan modülleri VS Code'da görüntüleme](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Azure IoT Hub çalıştırarak IoT Hub 'ınıza gelen tüm iletileri görebilirsiniz: komut paletinde **yerleşik olay uç noktasını Izlemeye başlayın** .

IoT hub'ınıza belirli bir cihazdan gelen iletilerin gösterilmesi için görünüme filtre de uygulayabilirsiniz. **Azure IoT Hub cihazları** bölümünde cihaza sağ tıklayın ve **Izlemeyi Başlat yerleşik olay uç noktası**' nı seçin.

İzleme iletilerini durdurmak için, komut paletinde **Azure IoT Hub: yerleşik olay uç noktasını Izlemeyi durdur** komutunu çalıştırın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir Azure işlevi modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazırsanız, [Visual Studio Code için Azure IoT Edge nasıl geliştirileceği](how-to-vs-code-develop-module.md)hakkında daha fazla bilgi edinebilirsiniz. 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yolları öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Stream Analytics'te kayan pencere kullanarak ortalamaları bulma](tutorial-deploy-stream-analytics.md)
