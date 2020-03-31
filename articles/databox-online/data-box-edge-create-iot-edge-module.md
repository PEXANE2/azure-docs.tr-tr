---
title: Azure Veri Kutusu Kenarı için C# IoT Edge modülü | Microsoft Dokümanlar
description: Veri Kutusu Kenarı'nızda dağıtılabilen bir C# IoT Edge modülü nasıl geliştireceğinizi öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/06/2019
ms.author: alkohli
ms.openlocfilehash: 3aa1190fb713c2fbdedcb1ce84a65d4263693827
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942558"
---
# <a name="develop-a-c-iot-edge-module-to-move-files-on-data-box-edge"></a>Data Box Edge'deki dosyaları taşımak için c# IoT Edge modülü geliştirin

Bu makalede, Veri Kutusu Edge aygıtınızla dağıtım için bir IoT Edge modülü oluşturma konusunda sizi hızlandırır. Azure Data Box Edge verileri işlemenizi ve ağ üzerinden Azure'a göndermenizi sağlayan bir depolama çözümüdür.

Verileri Azure'a aktarırken dönüştürmek için Veri Kutusu Edge'inizle Azure IoT Edge modüllerini kullanabilirsiniz. Bu makalede kullanılan modül, bir dosyayı yerel bir paylaşımdan Veri Kutusu Kenarı aygıtınızdaki bulut paylaşımına kopyalama mantığını uygular.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Modüllerinizi (Docker görüntüleri) depolamak ve yönetmek için bir konteyner kayıt defteri oluşturun.
> * Veri Kutusu Kenarı aygıtınızda dağıtmak için bir IoT Edge modülü oluşturun.


## <a name="about-the-iot-edge-module"></a>IoT Edge modülü hakkında

Veri Kutusu Kenarı aygıtınız IoT Edge modüllerini dağıtabilir ve çalıştırabilir. Kenar modülleri, bir aygıttan ileti almak, iletiyi dönüştürmek veya bir IoT Hub'ına ileti göndermek gibi belirli bir görevi gerçekleştiren Docker kapsayıcılarıdır. Bu makalede, dosyaları yerel bir paylaşımdan Veri Kutusu Kenarı aygıtınızdaki bulut paylaşımına kopyalayan bir modül oluşturursunuz.

1. Dosyalar, Veri Kutusu Kenarı aygıtınızdaki yerel paylaşıma yazılır.
2. Dosya olay oluşturucuyerel paylaşıma yazılmış her dosya için bir dosya olayı oluşturur. Dosya değiştirildiğinde dosya olayları da oluşturulur. Dosya olayları daha sonra IoT Edge Hub'ına (IoT Edge çalışma zamanında) gönderilir.
3. IoT Edge özel modülü, dosya için göreli bir yol da içeren bir dosya olayı nesnesi oluşturmak için dosya olayını işler. Modül, göreli dosya yolunu kullanarak mutlak bir yol oluşturur ve dosyayı yerel paylaşımdan bulut paylaşımına kopyalar. Modül daha sonra dosyayı yerel paylaşımdan siler.

![Azure IoT Edge modülü Veri Kutusu Kenarı'nda nasıl çalışır?](./media/data-box-edge-create-iot-edge-module/how-module-works-1.png)

Dosya bulut paylaşımına girdiğinde, otomatik olarak Azure Depolama hesabınıza yüklenir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- Çalışan bir Veri Kutusu Kenarı aygıtı.

    - Aygıtın ilişkili bir IoT Hub kaynağı da vardır.
    - Aygıt, Edge bilgi işlem rolünü yapılandırmıştır.
    Daha fazla bilgi için, Veri Kutusu Edge'iniz için [yapılandırılan bilgi işlem](data-box-edge-deploy-configure-compute.md#configure-compute) bilgisine gidin.

- Aşağıdaki geliştirme kaynakları:

    - [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
    - [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).
    - [Visual Studio Code için Azure IoT Edge uzantısı.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
    - [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).
    - [Docker CE](https://store.docker.com/editions/community/docker-ce-desktop-windows). Yazılımı indirmek ve yüklemek için bir hesap oluşturmanız gerekebilir.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteri, Azure’da özel Docker kapsayıcısı görüntülerinizi depolayıp yönetebileceğiniz özel bir Docker kayıt defteridir. Bulutta kullanılabilen iki popüler Docker kayıt defteri hizmeti Azure Konteyner Kayıt Defteri ve Docker Hub'dır. Bu makalede, Kapsayıcı Kayıt Defteri kullanır.

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. **Kapsayıcı Kayıt > Kapsayıcılar > kaynak oluştur'u**seçin. **Oluştur'u**tıklatın.
3. Sağlamak:

   1. Azure'da 5 ila 50 alfasayısal karakter içeren benzersiz bir **Kayıt Defteri adı.**
   2. **Abonelik**seçin.
   3. Yeni oluşturun veya varolan bir **Kaynak grubu**seçin.
   4. Bir **Konum** seçin. Bu konumun Veri Kutusu Kenarı kaynağıyla ilişkili yle aynı olmasını öneririz.
   5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
   6. SKU'yu **Temel**olarak ayarlayın.

      ![Kapsayıcı kayıt defteri oluşturma](./media/data-box-edge-create-iot-edge-module/create-container-registry-1.png)
 
4. **Oluştur'u**seçin.
5. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin.

    ![Erişim tuşlarını alın](./media/data-box-edge-create-iot-edge-module/get-access-keys-1.png)
 
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Docker görüntüsünü daha sonra kayıt defterinize yayımlamak ve kayıt defteri kimlik bilgilerini Azure IoT Edge çalışma süresine eklemek için bu değerleri kullanırsınız.


## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma

Aşağıdaki adımlar .NET Core 2.1 SDK'ya dayalı bir IoT Edge modülü projesi oluşturur. Proje, Visual Studio Code ve Azure IoT Edge uzantısını kullanır.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir C# çözüm şablonu oluşturun.

1. Visual Studio Code'da, VS Kodu komut paletini açmak için > Komut Paleti'ni **görüntüle'yi** seçin.
2. Komut paletinde **Azure: Sign in** komutunu girip çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.
3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin:

    1. Çözümü oluşturmak istediğiniz klasörü seçin.
    2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
    
        ![Yeni çözüm oluşturma 1](./media/data-box-edge-create-iot-edge-module/create-new-solution-1.png)

    3. Modül şablonu olarak **C# Module** girişini seçin.
    4. Varsayılan modül adını atamak istediğiniz adla değiştirin, bu durumda **FileCopyModule'dir.**
    
        ![Yeni çözüm oluşturma 2](./media/data-box-edge-create-iot-edge-module/create-new-solution-2.png)

    5. Önceki bölümde oluşturduğunuz kapsayıcı kayıt defterini, ilk modülünüz için görüntü deposu olarak belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın.

        Son dize `<Login server name>/<Module name>`gibi görünüyor . Bu örnekte, dize: `mycontreg2.azurecr.io/filecopymodule`.

        ![Yeni çözüm oluşturma 3](./media/data-box-edge-create-iot-edge-module/create-new-solution-3.png)

4. Dosya **> Aç Klasörüne**gidin.

    ![Yeni çözüm oluşturun 4](./media/data-box-edge-create-iot-edge-module/create-new-solution-4.png)

5. Daha önce oluşturduğunuz **EdgeSolution** klasörüne göz atın ve işaret edin. VS Code penceresi, IoT Edge çözüm çalışma alanınızı beş üst düzey bileşeniyle yükler. Bu makalede **.vscode** klasörünü, **.gitignore** dosyasını, **.env** dosyasını ve **deployment.template.json** dosyasını yönetmezsiniz.
    
    Değiştirdiğiniz tek bileşen modüller klasörüdür. Bu klasör, modülünüzü kapsayıcı görüntüsü olarak oluşturmak için modülünüzün C# koduna ve Docker dosyalarına sahiptir.

    ![Yeni çözüm oluşturma 5](./media/data-box-edge-create-iot-edge-module/create-new-solution-5.png)

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code explorer'da **FileCopyModule > Program.cs > açık modüller.**
2. **FileCopyModule ad alanının**üst kısmında, daha sonra kullanılan türleri için aşağıdaki ifadeleri kullanarak ekleyin. **Microsoft.Azure.Devices.Client.Transport.Mqtt,** IoT Edge Hub'a ileti göndermek için kullanılan bir protokoldür.

    ```
    namespace FileCopyModule
    {
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using Newtonsoft.Json;
    ```
3. Program sınıfına **GirişFolderPath** ve **OutputFolderPath** değişkenini ekleyin.

    ```
    class Program
        {
            static int counter;
            private const string InputFolderPath = "/home/input";
            private const string OutputFolderPath = "/home/output";
    ```

4. Önceki adımdan hemen sonra, ileti gövdesini tanımlamak için **FileEvent** sınıfını ekleyin.

    ```
    /// <summary>
    /// The FileEvent class defines the body of incoming messages. 
    /// </summary>
    private class FileEvent
    {
        public string ChangeType { get; set; }

        public string ShareRelativeFilePath { get; set; }

        public string ShareName { get; set; }
    }
    ```

5. **Init yönteminde,** kod bir **ModuleClient** nesnesi oluşturur ve yapılandırır. Bu nesne, ileti göndermek ve almak için MQTT protokolünü kullanarak modülün yerel Azure IoT Edge çalışma süresine bağlanmasını sağlar. Init yönteminde kullanılan bağlantı dizesi, modüle IoT Edge çalışma zamanı tarafından sağlanır. Kod, **giriş1** bitiş noktası üzerinden bir IoT Edge hub'ından ileti almak için bir FileCopy geri arama kaydeder. **Init yöntemini** aşağıdaki kodla değiştirin.

    ```
    /// <summary>
    /// Initializes the ModuleClient and sets up the callback to receive
    /// messages containing file event information
    /// </summary>
    static async Task Init()
    {
        MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
        ITransportSettings[] settings = { mqttSetting };

        // Open a connection to the IoT Edge runtime
        ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
        await ioTHubModuleClient.OpenAsync();
        Console.WriteLine("IoT Hub module client initialized.");

        // Register callback to be called when a message is received by the module
        await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FileCopy, ioTHubModuleClient);
    }
    ```

6. **PipeMessage yönteminin** kodunu kaldırın ve yerine **FileCopy**kodunu ekleyin.

    ```
        /// <summary>
        /// This method is called whenever the module is sent a message from the IoT Edge Hub.
        /// This method deserializes the file event, extracts the corresponding relative file path, and creates the absolute input file path using the relative file path and the InputFolderPath.
        /// This method also forms the absolute output file path using the relative file path and the OutputFolderPath. It then copies the input file to output file and deletes the input file after the copy is complete.
        /// </summary>
        static async Task<MessageResponse> FileCopy(Message message, object userContext)
        {
            int counterValue = Interlocked.Increment(ref counter);

            try
            {
                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                if (!string.IsNullOrEmpty(messageString))
                {
                    var fileEvent = JsonConvert.DeserializeObject<FileEvent>(messageString);

                    string relativeFileName = fileEvent.ShareRelativeFilePath.Replace("\\", "/");
                    string inputFilePath = InputFolderPath + relativeFileName;
                    string outputFilePath = OutputFolderPath + relativeFileName;

                    if (File.Exists(inputFilePath))                
                    {
                        Console.WriteLine($"Moving input file: {inputFilePath} to output file: {outputFilePath}");
                        var outputDir = Path.GetDirectoryName(outputFilePath);
                        if (!Directory.Exists(outputDir))
                        {
                            Directory.CreateDirectory(outputDir);
                        }

                        File.Copy(inputFilePath, outputFilePath, true);
                        Console.WriteLine($"Copied input file: {inputFilePath} to output file: {outputFilePath}");
                        File.Delete(inputFilePath);
                        Console.WriteLine($"Deleted input file: {inputFilePath}");
                    } 
                    else
                    {
                        Console.WriteLine($"Skipping this event as input file doesn't exist: {inputFilePath}");   
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Caught exception: {0}", ex.Message);
                Console.WriteLine(ex.StackTrace);
            }

            Console.WriteLine($"Processed event.");
            return MessageResponse.Completed;
        }
    ```

7. Bu dosyayı kaydedin.
8. Ayrıca bu proje için [varolan bir kod örneğini indirebilirsiniz.](https://azure.microsoft.com/resources/samples/data-box-edge-csharp-modules/?cdn=disable) Daha sonra bu örnekteki **program.cs** dosyaya karşı kaydettiğiniz dosyayı doğrulayabilirsiniz.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümde, bir IoT Edge çözümü oluşturdunuz ve dosyaları yerel paylaşımdan bulut paylaşımına kopyalamak için FileCopyModule'e kod eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. VSCode'da, yeni bir Visual Studio Code entegre terminali açmak için Terminal > New Terminal'e gidin.
2. Entegre terminale aşağıdaki komutu girerek Docker'da oturum açın.

    `docker login <ACR login server> -u <ACR username>`

    Konteyner kayıt defterinizden kopyaladığınız giriş sunucusunu ve kullanıcı adını kullanın.

    ![IoT Edge çözümü oluşturma ve itme](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-1.png)

2. Parola için istendiğinde, parolayı girin. Ayrıca, Azure portalındaki konteyner kayıt defterinizdeki **Access Keys'ten** giriş sunucusu, kullanıcı adı ve parola değerlerini de alabilirsiniz.
 
3. Kimlik bilgileri sağlandıktan sonra, modül resminizi Azure konteyner kayıt defterinize taşıyabilirsiniz. VS Code Explorer'da **module.json** dosyasına sağ tıklayın ve **IoT Edge'i Oluştur ve İtiyorum çözümünü**seçin.

    ![IoT Edge çözümü oluşturma ve itme](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-2.png)
 
    Visual Studio Code'a çözümünüzü oluşturmasını söylediğiniz zaman, entegre terminalde iki komut çalışır: docker build ve docker itme. Bu iki komut kodunuzu derler, CSharpModule.dll ile kapsayıcı oluşturur ve ardından kodu, çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir.

    Modül platformlarını seçmeniz istenir. Linux'a karşılık gelen *amd64'ü* seçin.

    ![Platform seçin](./media/data-box-edge-create-iot-edge-module/select-platform.png)

    > [!IMPORTANT] 
    > Yalnızca Linux modülleri desteklenir.

    Göz ardı edebileceğiniz aşağıdaki uyarıyı görebilirsiniz:

    *Program.cs(77,44): uyarı CS1998: Bu async yöntemi 'bekliyor' operatörleri yoksun ve senkronize çalışacaktır. Engellemeyapmayan API çağrılarını beklemek için 'bekleme' işlecini kullanmayı veya arka plan iş parçacığı üzerinde CPU'ya bağlı çalışma yapmak için 'Task.Run(...)' beklemeyi düşünün.*

4. VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Resim adresi, module.json dosyasında bulunan ve biçimi `<repository>:<version>-<platform>`olan bilgilerden oluşturulmuş. Bu makale için, gibi `mycontreg2.azurecr.io/filecopymodule:0.0.1-amd64`görünmelidir .

## <a name="next-steps"></a>Sonraki adımlar

Bu modülü Veri Kutusu Kenarı'nda dağıtmak ve çalıştırmak için [modül ekle'deki](data-box-edge-deploy-configure-compute.md#add-a-module)adımlara bakın.
