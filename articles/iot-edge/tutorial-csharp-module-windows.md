---
title: Öğretici - Azure IoT Edge kullanarak Windows için C# modülü geliştirin
description: Bu öğretici, C# koduna sahip bir IoT Edge modülü oluşturmanızı ve bir Windows IoT Edge aygıtına nasıl dağıtabileceğinizi gösterir.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom:
- mvc
- amqp
ms.openlocfilehash: b71db71ac61e0dcd65a2546b2164610e618dab18
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733503"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-windows-devices"></a>Öğretici: Windows aygıtları için c# IoT Edge modülü geliştirin

C# kodunu geliştirmek ve Azure IoT Edge çalıştıran bir Windows aygıtına dağıtmak için Visual Studio'yı kullanın.

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * C# SDK'yı temel alan bir IoT Edge modülü oluşturmak için Visual Studio'yu kullanın.
> * Bir Docker resmi oluşturmak ve kayıt defterinize yayınlamak için Visual Studio ve Docker'ı kullanın.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğretici, **Visual Studio 2019'u** kullanarak **C#** modülü nasıl geliştirilip bir **Windows cihazına**dağıtılanın nasıl gösteriş yapılacağını göstermektedir. Linux aygıtları için modüller geliştiriyorsanız, bunun yerine [Linux aygıtları için bir C# IoT Edge modülü geliştirin.](tutorial-csharp-module.md)

C# modülleri geliştirme ve Windows aygıtlarına dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın:

| C# | Visual Studio Code | Visual Studio 2017/2019 |
| -- | ------------------ | ------------------ |
| **Windows AMD64 geliştirmek** | ![VS KoduwinAMD64 için C# modülleri geliştirin](./media/tutorial-c-module/green-check.png) | ![Visual Studio'da WinAMD64 için C# modülleri geliştirin](./media/tutorial-c-module/green-check.png) |
| **Windows AMD64 hata ayıklama** |   | ![Visual Studio WinAMD64 için Hata Ayıklama C# modülleri](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici başlamadan önce, geliştirme ortamını ayarlamak için önceki öğretici ile gitmiş olmalıdır, [Bir Windows aygıtı için bir IoT Edge modülü geliştirin.](tutorial-develop-for-windows.md) Bu öğretici tamamladıktan sonra, zaten aşağıdaki ön koşullara sahip olmalıdır:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart.md)bir Windows aygıtı.
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Visual Studio 2019,](https://docs.microsoft.com/visualstudio/install/install-visual-studio) [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) uzantısı ile yapılandırıldı.
* [Docker Desktop,](https://docs.docker.com/docker-for-windows/install/) Windows kapsayıcılarını çalıştırmak için yapılandırıldı.

> [!TIP]
> Visual Studio 2017 kullanıyorsanız (sürüm 15.7 veya üzeri), Visual Studio pazar yerinden VS 2017 için [Azure IoT Edge Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) indirip yükleyin

## <a name="create-a-module-project"></a>Modül projesi oluşturma

Aşağıdaki adımlar, Visual Studio ve Azure IoT Edge Araçları uzantısını kullanarak bir IoT Edge modül projesi oluşturur. Bir proje şablonu oluşturulduktan sonra, modülün bildirilen özelliklerine göre iletileri filtreleyebilmeleri için yeni kod ekleyin.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Azure IoT Edge Araçları, Visual Studio'daki desteklenen tüm IoT Edge modül dilleri için proje şablonları sağlar. Bu şablonlar, IoT Edge'i sınamak veya şablonu kendi iş mantığınızla özelleştirmeniz için bir başlangıç noktası vermek için bir çalışma modülü dağıtmanız gereken tüm dosyalara ve kodlara sahiptir.

1. Visual Studio 2019'u başlatın ve **Yeni Proje Oluştur'u**seçin.

2. **IoT Edge'i** arayın ve **Azure IoT Edge (Windows amd64)** projesini seçin. **İleri**’ye tıklayın.

   ![Yeni bir Azure IoT Edge projesi oluşturun](./media/tutorial-csharp-module-windows/new-project.png)

3. **CSharpTutorialApp**gibi açıklayıcı bir şey için proje ve çözüm rename . Projeyi oluşturmak için **Oluştur'u** tıklatın.

   ![Yeni bir Azure IoT Edge projesini yapılandırma](./media/tutorial-csharp-module-windows/configure-project.png)

4. Projenizi aşağıdaki değerlerle yapılandırın:

   | Alan | Değer |
   | ----- | ----- |
   | Şablon seçin | **C# modülünü**seçin. |
   | Modül proje adı | Modülünüze **CSharpModule** adını verin. |
   | Docker görüntü deposu | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz modül proje ad değerinden önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son resim deposu kayıt defteri \<adı\>.azurecr.io/csharpmodule gibi görünüyor. |

   ![Projenizi hedef aygıt, modül türü ve konteyner kayıt defteri için yapılandırın](./media/tutorial-csharp-module-windows/add-application-and-module.png)

5. Projeyi oluşturmak için **Ekle'yi** seçin.

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Dağıtım bildirimi, kapsayıcı kayıt defterinizin kimlik bilgilerini IoT Edge çalışma zamanıile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. Azure konteyner kayıt defterinizin **Erişim tuşları** bölümündeki kimlik bilgilerini kullanın.

1. Visual Studio çözüm gezgininde **deployment.template.json** dosyasını açın.

2. İstenilen $edgeAgent özelliklerinde **kayıt kimlik bilgileri** özelliğini bulun. Projeyi oluştururken sağladığınız bilgilerden kayıt defteri adresinizotomatik olarak doldurulmalı ve ardından kullanıcı adı ve parola alanları değişken adlar içermelidir. Örneğin:

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "$CONTAINER_REGISTRY_USERNAME_<registry name>",
       "password": "$CONTAINER_REGISTRY_PASSWORD_<registry name>",
       "address": "<registry name>.azurecr.io"
     }
   }
   ```

3. Modül çözümünde **.env** dosyasını açın. (Çözüm Gezgini'nde varsayılan olarak gizlenir, bu nedenle görüntülemek için **Tüm Dosyaları Göster** düğmesini seçmeniz gerekebilir.) .env dosyası, deployment.template.json dosyasında gördüğünüz kullanıcı adı ve parola değişkenlerini içermelidir.

4. Azure kapsayıcı kayıt defterinizden **Kullanıcı Adı** ve **Parola** değerlerini ekleyin.

5. Değişikliklerinizi .env dosyasına kaydedin.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Varsayılan modül kodu, giriş kuyruğundaki iletileri alır ve bunları bir çıkış kuyruğu boyunca ileterek geçirir. Modülün iletileri IoT Hub'ına göndermeden önce kenarda işleyebilmeleri için bazı ek kodlar ekleyelim. Modülü, her iletideki sıcaklık verilerini analiz edecek şekilde güncelleştirin ve iletiyi yalnızca sıcaklık belirli bir eşiği aşarsa IoT Hub'a gönderir.

1. Visual Studio'da **CSharpModule** > **Program.cs'ı**açın.

2. **CSharpModule** ad alanının en üst kısmına daha sonra kullanılan türler için üç **using** deyimi yazın:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

3. Sayaç değişkeninden sonra **sıcaklıkThreshold** değişkenini **Program** sınıfına ekleyin. SıcaklıkEşik değişkeni, IoT hub'ına gönderilecek veriler için ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

4. Değişken bildirimlerden sonra **MessageBody,** **Machine**ve **Ambient** sınıflarını **Program** sınıfına ekleyin. Bu sınıflar gelen iletilerin gövdesi için beklenen şemayı tanımlar.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
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
    ```

5. **Init** yöntemini bulun. Bu yöntem, modülün ileti gönderip almak için yerel Azure IoT Edge çalışma süresine bağlanmasını sağlayan bir **ModuleClient** nesnesi oluşturur ve yapılandırır. Kod **ayrıca, giriş1** bitiş noktası üzerinden bir IoT Edge hub'ından ileti almak için bir geri arama kaydeder.

   Tüm Init yöntemini aşağıdaki kodla değiştirin:

   ```csharp
   static async Task Init()
   {
       AmqpTransportSettings amqpSetting = new AmqpTransportSettings(TransportType.Amqp_Tcp_Only);
       ITransportSettings[] settings = { amqpSetting };

       // Open a connection to the Edge runtime
       ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
       await ioTHubModuleClient.OpenAsync();
       Console.WriteLine("IoT Hub module client initialized.");

       // Read the TemperatureThreshold value from the module twin's desired properties
       var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
       await OnDesiredPropertiesUpdate(moduleTwin.Properties.Desired, ioTHubModuleClient);

       // Attach a callback for updates to the module twin's desired properties.
       await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

       // Register a callback for messages that are received by the module.
       await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
   }
   ```

   Bu güncelleştirilmiş Init yöntemi, ModuleClient ile IoT Edge çalışma süresi bağlantısını yine de ayarlar, ancak yeni işlevler de ekler. Bu **sıcaklıkEşik** değeri almak için modül ikiz istenen özelliklerini okur. Daha sonra, modül ikiz istenen özellikleri gelecekteki güncellemeleri için dinleyen bir geri arama oluşturur. Bu geri arama ile, modül ikizindeki sıcaklık eşiğini uzaktan güncelleştirebilirsiniz ve değişiklikler modüle dahil edilir.

   Güncelleştirilmiş Init yöntemi de varolan **SetInputMessageHandlerAsync** yöntemini değiştirir. Örnek kodda, *input1'deki* gelen iletiler *PipeMessage* işlevi ile işlenir, ancak aşağıdaki adımlarda oluşturabileceğimiz *FilterMessages* işlevini kullanmak için bunu değiştirmek istiyoruz.

6. **Program** sınıfına yeni bir **onDesiredPropertiesUpdate** yöntemi ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```csharp
    static Task OnDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

7. Örnek **PipeMessage** yöntemini kaldırın ve yeni bir **FilterMessages** yöntemiyle değiştirin. Modül IoT Edge hub'ından bir ileti aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler. Ayrıca iletiye **MessageType** özelliğini ekleyip değerini **Alert** olarak ayarlar.

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);
        try
        {
            ModuleClient moduleClient = (ModuleClient)userContext;
            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get the message body.
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                using(var filteredMessage = new Message(messageBytes))
                {
                    foreach (KeyValuePair<string, string> prop in message.Properties)
                    {
                        filteredMessage.Properties.Add(prop.Key, prop.Value);
                    }

                    filteredMessage.Properties.Add("MessageType", "Alert");
                    await moduleClient.SendEventAsync("output1", filteredMessage);
                }
            }

            // Indicate that the message treatment is completed.
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed.
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed.
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

8. Program.cs dosyasını kaydedin.

9. IoT Edge çözümünde **deployment.template.json** dosyasını açın. Bu dosya, IoT Edge aracısına hangi modüllerin dağıtılabildiğini söyler, bu durumda **Simüle Sıcaklık Sensörü** ve **CSharpModule**, ve IoT Edge hub'ına aralarındaki iletileri nasıl yönlendireceklerini söyler.

10. **CSharpModule** modül ikizini dağıtım bildirimine ekleyin. Aşağıdaki JSON içeriğini **modulesContent** bölümünün en altına, **$edgeHub** modül ikizinden sonra ekleyin:

    ```json
       "CSharpModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
    ```

    ![Dağıtım şablonuna modül ikizi ekleme](./media/tutorial-csharp-module-windows/module-twin.png)

11. deployment.template.json dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Modülünüzü oluşturun ve itin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve **CSharpModule** modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyen kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. Geliştirme makinenizde Docker'da oturum açabilmek için aşağıdaki komutu kullanın. Azure konteyner kayıt defterinizdeki kullanıcı adını, parolayı ve giriş sunucusunu kullanın. Bu değerleri Azure portalındaki kayıt defterinizin **Erişim tuşları** bölümünden alabilirsiniz.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   'nin kullanılmasını öneren bir güvenlik `--password-stdin`uyarısı alabilirsiniz. Bu en iyi uygulama üretim senaryoları için önerilir, ancak bu öğretici kapsamı dışındadır. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvurusuna bakın.

2. Visual Studio çözüm gezgininde, oluşturmak istediğiniz proje adını sağ tıklatın. Varsayılan adı **AzureIotEdgeApp1** ve bir Windows modülü oluşturuyorsanız, uzantısı **Windows.Amd64**olmalıdır.

3. **IoT Edge Modülleri Oluştur ve İtme'yi**seçin.

   Yapı ve itme komutu üç işlem başlatır. İlk olarak, dağıtım şablonundaki ve diğer çözüm dosyalarındaki bilgiler dışında oluşturulmuş, tam dağıtım bildirimini tutan **config** adlı çözümde yeni bir klasör oluşturur. İkinci olarak, `docker build` hedef mimariniz için uygun dockerfiledayalı kapsayıcı görüntü oluşturmak için çalışır. Daha sonra, `docker push` görüntü deposunu konteyner kayıt defterinize itmek için çalışır.

## <a name="deploy-modules-to-device"></a>Modülleri aygıta dağıtma

Modül projesini IoT Edge aygıtınıza dağıtmak için Visual Studio bulut gezgini ve Azure IoT Edge Araçları uzantısını kullanın. Zaten senaryonuz için hazırlanmış bir dağıtım bildiriminiz var, config klasöründe **deployment.json** dosyası. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IoT Edge cihazınızın çalışır durumda olduğundan emin olun.

1. Visual Studio bulut gezgininde, IoT aygıtları listenizi görmek için kaynakları genişletin.

2. Dağıtımı almak istediğiniz IoT Edge aygıtının adını sağ tıklatın.

3. **Dağıtım Oluştur'u**seçin.

4. Dosya gezgini'nde, çözümünüzün config klasöründe **deployment.windows-amd64** dosyasını seçin.

5. Cihazınızın altında listelenen dağıtılmış modülleri görmek için bulut gezginini yenileyin.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

İletileri IoT Hub'ınıza ulaştıklarında görüntülemek için IoT Edge Tools uzantısını kullanabilirsiniz.

1. Visual Studio bulut gezgininde, IoT Edge cihazınızın adını seçin.

2. **Eylemler** listesinde, **Yerleşik Olay Bitiş Noktası'nı İzlemeye Başla'yı**seçin.

3. IoT Hub'ınıza gelen iletileri görüntüleyin. CSharpModule kodunda yaptığımız değişiklikler ileti göndermeden önce makine sıcaklığının 25 dereceye ulaşmasını beklediğinden, iletilerin gelmesi biraz zaman alabilir. Ayrıca, bu sıcaklık eşiğine ulaşan iletilere ileti türü **Uyarısı** da ekler.

   ![IoT Hub'a gelen iletileri görüntüleme](./media/tutorial-csharp-module-windows/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>Modül ikizini edin

Sıcaklık eşiğini 25 derece olarak ayarlamak için CSharpModule modül ikizini kullandık. Modül kodunu güncelleştirmek zorunda kalmadan işlevselliği değiştirmek için modül ikizi kullanabilirsiniz.

1. Visual Studio'da **deployment.windows-amd64.json** dosyasını açın. (Deployment.template dosyası değil. Çözüm gezginindeki config dosyasında dağıtım bildirimini görmüyorsanız, gezgin araç çubuğundaki **tüm dosyaları göster** simgesini seçin.)

2. CSharpModule ikizini bulun ve **sıcaklık** değerini eşik parametresini yeni bir sıcaklığa değiştirin 5 derece ile en son bildirilen sıcaklıktan 10 derece daha yüksek.

3. **Deployment.windows-amd64.json** dosyasını kaydedin.

4. Güncelleştirilmiş dağıtım bildirimini cihazınıza uygulamak için dağıtım adımlarını yeniden izleyin.

5. Gelen aygıttan buluta iletileri izleyin. İletilerin yeni sıcaklık eşiğine ulaşılına kadar durduğunu görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Aksi takdirde, ücretlendirmeleri önlemek için bu makalede kullandığınız yerel yapılandırmaları ve Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [kendi IoT Edge modüllerinizi geliştirme](module-development.md) veya Visual [Studio ile modülgeliştirme](how-to-visual-studio-develop-module.md)hakkında daha fazla bilgi edinebilirsiniz. Simüle edilen sıcaklık modülü de dahil olmak üzere IoT Edge modülleri örnekleri için [IoT Edge modül örneklerine](https://github.com/Azure/iotedge/tree/master/edge-modules)bakın.

Azure IoT Edge'in, verileri işlemek ve en uçta analiz etmek için Azure bulut hizmetlerini dağıtmanıza nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Fonksiyonlar](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Makine Öğrenme](tutorial-deploy-machine-learning.md)
> [Özel Vizyon Hizmeti](tutorial-deploy-custom-vision.md)
