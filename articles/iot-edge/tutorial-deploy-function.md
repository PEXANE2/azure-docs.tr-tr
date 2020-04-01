---
title: 'Öğretici: Azure işlevlerini modül olarak dağıtma - Azure IoT Edge'
description: Bu öğreticide, IoT Edge modülü olarak bir Azure işlevi geliştirip kenar aygıtına dağıtMış olursunuz.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: f909ca12ce080fc5d1241bcc649c041361e405a7
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421173"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules"></a>Öğretici: Azure işlevlerini IoT Edge modülleri olarak dağıtma

İş mantığınızı doğrudan Azure IoT Edge cihazlarınıza uygulayan kodu dağıtmak için Azure İşlevleri'ni kullanabilirsiniz. Bu öğreticide, benzetimi yapılan IoT Edge cihazındaki algılayıcı verilerini filtreleyen bir Azure işlevi oluşturma ve dağıtma işlemlerinde size yol gösterilir. [Windows](quickstart.md)'ta veya [Linux](quickstart-linux.md)'ta bir simülasyon cihazına Azure IoT Edge dağıtma hızlı başlangıçlarında oluşturduğunuz simülasyon IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Visual Studio Code kullanarak Azure işlevi oluşturma.
> * VS Code ve Docker kullanarak Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinde yayımlama.
> * Kapsayıcı kayıt defterindeki modülü IoT Edge cihazınıza dağıtma.
> * Filtrelenmiş verileri görüntüleme.

<center>

![Diyagram - Öğretici mimari: sahne ve dağıtma işlevi modülü](./media/tutorial-deploy-function/functions-architecture.png)
</center>

Bu öğreticide oluşturacağınız Azure işlevi, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İşlev, yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde Azure IoT Hub'a iletileri gönderir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici başlamadan önce, Linux konteyner geliştirme için geliştirme ortamını ayarlamak için önceki öğretici ile gitmiş olmalıdır: [Linux cihazları için IoT Edge modülleri geliştirin.](tutorial-develop-for-linux.md) Bu öğreticiyi tamamlayarak, aşağıdaki ön koşulları yerine koymalısınız:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart-linux.md) bir Linux aygıtı
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile yapılandırılan [Görsel Stüdyo Kodu.](https://code.visualstudio.com/)
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak için yapılandırılmıştır.

Azure İşlevleri ile birlikte bir IoT Edge modülü geliştirmek için geliştirme makinenize aşağıdaki ek ön koşulları yükleyin:

* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

Ön koşullara yüklediğiniz Görsel Stüdyo Kodu için Azure IoT Araçları, yönetim özelliklerinin yanı sıra bazı kod şablonları da sağlar. Bu bölümde Visual Studio Code'u kullanarak Azure işlevi içeren bir IoT Edge çözümü oluşturacaksınız.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi kodunuzla özelleştirebileceğiniz bir C# İşlev çözüm şablonu oluşturun.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. Komut Paletini Görüntüle'yi **View** > seçerek VS Kodu komut**paletini**açın.

3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | **Çözümünüzün**işleviniz için açıklayıcı bir ad girin veya varsayılanı kabul edin. |
   | Modül şablonunu seçin | **Azure İşlevlerini**Seçin - C# . |
   | Modül adı sağlayın | Modülünüze **CSharpFunction** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. Son dize \<kayıt defteri\>adı .azurecr.io/CSharpFunction gibi görünüyor. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-deploy-function/repository.png)

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Hedef mimarinizi seçin

Şu anda Visual Studio Code, Linux AMD64 ve Linux ARM32v7 cihazları için C modülleri geliştirebilir. Kapsayıcı inşa edilmiş ve her mimari türü için farklı çalıştırDığınızdan, her çözümde hangi mimariyi hedeflediğinizi seçmeniz gerekir. Varsayılan Linux AMD64 olduğunu.

1. Komut paletini açın ve **Azure IoT Edge: Edge Solution için Varsayılan Hedef Platformu'nu ayarlayın**veya pencerenin altındaki yan çubuktaki kısayol simgesini seçin.

2. Komut paletinde, seçenekler listesinden hedef mimarisini seçin. Bu öğretici için, biz IoT Edge cihaz olarak bir Ubuntu sanal makine kullanıyoruz, bu yüzden varsayılan **amd64**devam edecektir.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Modülün iletileri IoT Hub'ına göndermeden önce kenarda işleyebilmeleri için bazı ek kodlar ekleyelim.

1. Visual Studio Code'da**CSharpFunction** > **CSharpFunction.cs** **modüllerini** > açın.

1. **CSharpFunction.cs** dosyasının içeriğini aşağıdaki kodla değiştirin. Bu kod ortam ve makine sıcaklığı hakkında telemetri alır ve iletiyi yalnızca makine sıcaklığı tanımlı bir eşiğin üzerindeyse IoT Hub'a iletir.

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

Önceki bölümde, bir IoT Edge çözümü oluşturdunuz ve kabul edilebilir eşiğin altında bildirilen makine sıcaklıklarına sahip iletileri filtrelemek için **CSharpFunction'ı** değiştirdiniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

Bu bölümde, Visual Studio Code'un görüntüleri kayıt defterinize aktarabilmesi için geliştirme makinenizden yerel olarak oturum açmak için ikinci kez (ilki IoT Edge çözümünüzün **.env** dosyasındaydı) konteyner kayıt defterinizin kimlik bilgilerini sağlarsınız.

1. **View** > **Terminalini**seçerek VS Code entegre terminalini açın.

2. Tümleşik terminale aşağıdaki komutu girerek kapsayıcı kayıt defterinizde oturum açın. Önceki adımlarda Azure kapsayıcı kayıt defterinden kopyaladığınız kullanıcı adını ve oturum açma sunucusunu kullanın.

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```

    Parola için istendiğinde, konteyner kayıt defteriniz için parolayı yapıştırın (terminal penceresinde görünmez) ve **Enter**tuşuna basın.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

3. VS Code gezgininde deployment.template.json dosyasına sağ tıklayın ve **IoT Edge çözümünü Oluştur ve İti**

Visual Studio Code'a çözümünüzü oluşturmasını söylediğiniz zaman, önce dağıtım şablonundaki bilgileri alır ve **config**adlı yeni bir klasörde bir deployment.json dosyası oluşturur. Sonra entegre terminalde iki komut `docker build` çalışır: ve `docker push`. Yapı komutu kodunuzu oluşturur ve işlevleri kapsayıcılaştırır. Ardından, push komutu, çözümü başharfe aldığınızda belirttiğiniz kapsayıcı kayıt defterine kodu iter.

## <a name="view-your-container-image"></a>Kapsayıcı görüntünüzü açma

Kapsayıcı görüntünüz, kapsayıcı kayıt defterinize gönderildiğinde Visual Studio Code işlemin başarılı olduğunu belirten bir ileti görüntüler. İşlemin başarılı olduğunu kendiniz onaylamak isterseniz kayıt defterindeki görüntüye bakabilirsiniz.

1. Azure portalında Azure kapsayıcı kayıt defterinize göz atın.
2. **Depoları**seçin.
3. Listede **csharpfunction** deposunu görüyor olmalısınız. Diğer ayrıntıları görmek için bu depoyu seçin.
4. **Etiketler** bölümünde **0.0.1-amd64** etiketini görmeniz gerekir. Bu etiket, derlediğiniz görüntünün sürümünü ve platformunu gösterir. Bu değerler CSharpFunction klasöründeki module.json dosyasında belirlenir.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

Hızlı başlangıçlarda yaptığınız gibi işlev modülünüzü IoT Edge cihazına dağıtmak için Azure portalını kullanabilirsiniz. Ayrıca modülleri Visual Studio Code'un içinden de dağıtabilir ve izleyebilirsiniz. Aşağıdaki bölümlerde, ön koşullarda listelenen VS Kodu için Azure IoT Araçları kullanılır. Uzantıyı henüz yüklemediyseniz, şimdi yükleyin.

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin.

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device**'ı (Tek bir cihaz için dağıtım oluştur) seçin.

3. **CSharpFunction** modülünü içeren çözüm klasörüne göz atın. Config klasörünü açın, **deployment.json** dosyasını seçin ve ardından **Kenar Dağıtım Bildirimini Seçin'i**seçin.

4. **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü yenileyin. Yeni **CSharpFunction** **Simüle TemperatureSensor** modülü ve **$edgeAgent** ve **$edgeHub**ile birlikte çalışan görmelisiniz. Yeni modüllerin ortaya olması birkaç dakika sürebilir. IoT Edge aygıtınızın yeni dağıtım bilgilerini IoT Hub'ından alması, yeni kapsayıcıları başlatması ve durumu IoT Hub'ına bildirmesi gerekir.

   ![Dağıtılan modülleri VS Code'da görüntüleme](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

IoT hub'ınıza gelen tüm iletileri, Komut paletinde **Azure IoT Hub'ını** çalıştırarak görebilirsiniz: Yerleşik Etkinlik Bitiş Noktasını İzlemeye Başla.

IoT hub'ınıza belirli bir cihazdan gelen iletilerin gösterilmesi için görünüme filtre de uygulayabilirsiniz. **Azure IoT Hub Aygıtları** bölümündeki aygıta sağ tıklayın ve **Yerleşik Etkinlik Bitiş Noktası'nı Başlat'ı**seçin.

İletileri izlemeyi durdurmak için, komut paletinde **Azure IoT Hub komutunu çalıştırın: Yerleşik Olay Bitiş Noktasını Izlemeyi Durdurun.**

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir Azure işlevi modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [Visual Studio Code için Azure IoT Edge ile](how-to-vs-code-develop-module.md)nasıl geliştirebileceğiniz hakkında daha fazla bilgi edinebilirsiniz.

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yolları öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Stream Analytics'te kayan pencere kullanarak ortalamaları bulma](tutorial-deploy-stream-analytics.md)
