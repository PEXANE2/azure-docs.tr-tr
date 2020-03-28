---
title: Öğretici - Azure IoT Edge kullanarak verileri SQL modülüyle depolama
description: Bu öğretici, sql server modülü ile IoT Edge cihazınızda verileri yerel olarak nasıl depolayacağımı gösterir
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3d1b5ea9a9f78bc8a83159a34026d58d7a8cc89b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78944264"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Öğretici: SQL Server veritabanları ile uç cihazlarda veri depolama

Azure IoT Edge çalıştıran bir Linux aygıtında veri depolamak için bir SQL Server modülü dağıtın.

Uç cihazlarda veri depolamak ve sorgulamak için Azure IoT Edge ve SQL Server işlevlerini kullanın. Azure IoT Edge, bir aygıt çevrimdışı olduğunda iletileri önbelleğe almak ve bağlantı yeniden oluşturulduğunda iletmek için temel depolama özelliklerine sahiptir. Ancak sorgu verilerini yerel ortamda sorgulayabilme gibi daha gelişmiş depolama özelliklerine ihtiyaç duyabilirsiniz. IoT Edge aygıtlarınız, IoT Hub'a bağlantı sağlamak zorunda kalmadan daha karmaşık bilgi işlem gerçekleştirmek için yerel veritabanlarını kullanabilir.

Bu makalede bir IoT Edge cihazına SQL Server veritabanı dağıtma yönergeleri yer almaktadır. IoT Edge cihazında çalışan Azure İşlevleri, gelen verileri yapılandırıp veritabanına gönderir. Bu makaledeki adımlar MySQL veya PostgreSQL gibi kapsayıcılarda çalışan diğer veritabanlarına da uygulanabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Visual Studio Code kullanarak Azure İşlevi oluşturma
> * IoT Edge cihazınıza bir SQL veritabanı dağıtma
> * Visual Studio Code uygulamasını kullanarak modülleri derleme ve IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici başlamadan önce, Linux konteyner geliştirme için geliştirme ortamını ayarlamak için önceki öğretici ile gitmiş olmalıdır: [Linux cihazları için IoT Edge modülleri geliştirin.](tutorial-develop-for-linux.md) Bu öğreticiyi tamamlayarak, aşağıdaki ön koşulları yerine koymalısınız:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart-linux.md)bir AMD64 Linux cihazı.
  * Raspberry gibi ARM aygıtları SQL Server çalıştıramaz. BIR ARM aygıtında SQL kullanmak istiyorsanız, önizlemede [Azure SQL Database Edge'i](https://azure.microsoft.com/services/sql-database-edge/) denemek için kaydolabilirsiniz.
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile yapılandırılan [Görsel Stüdyo Kodu.](https://code.visualstudio.com/)
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak için yapılandırılmıştır.

Bu öğretici, SQL Server'a veri göndermek için bir Azure İşlevleri modülü kullanır. Azure İşlevleri içeren bir IoT Edge modülü geliştirmek için geliştirme makinenize aşağıdaki ek ön koşulları yükleyin:

* [C# Visual Studio Code için (OmniSharp tarafından desteklenmektedir) Uzantısı Visual Studio Code için](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

Bir veritabanına veri göndermek için verileri doğru şekilde yapılandırıp tabloda kaydeden bir modüle ihtiyacınız vardır.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Aşağıdaki adımlar, Visual Studio Code ve Azure IoT Araçlarını kullanarak bir IoT Edge işlevini nasıl oluşturabileceğinizi gösterir.

1. Visual Studio Code'u açın.

2. **Komut** > uytunu seçerek VS Kodu komut**paletini**açın.

3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu yazıp çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin:

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | **SqlSolution**gibi çözümünüz için açıklayıcı bir ad girin veya varsayılanı kabul edin. |
   | Modül şablonunu seçin | **Azure İşlevlerini**Seçin - C# . |
   | Modül adı sağlayın | Modülünüze **sqlFunction** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son dize \<kayıt defteri\>adı .azurecr.io/sqlfunction gibi görünüyor. |

   VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler.

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

1. VS Code explorer'da**sqlFunction** > **sqlFunction.cs** **modüllerini** > açın.

2. Dosyanın tüm içeriğini aşağıdaki kodla değiştirin:

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
   using Sql = System.Data.SqlClient;

   namespace Functions.Samples
   {
       public static class sqlFunction
       {
           [FunctionName("sqlFunction")]
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

                   //Store the data in SQL db
                   const string str = "<sql connection string>";
                   using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
                   {
                       conn.Open();
                       var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
                       var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");";
                       using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
                       {
                           //Execute the command and log the # rows affected.
                           var rows = await cmd.ExecuteNonQueryAsync();
                           logger.LogInformation($"{rows} rows were updated");
                       }
                   }

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threashold.
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

3. Satır 35'te string ** \<sql\> bağlantı dizesini** aşağıdaki dizeyle değiştirin. **Veri Kaynağı** özelliği, henüz var olmayan SQL Server kapsayıcısı'na başvurur. Bir sonraki bölümde **SQL** adı ile oluşturacaksınız.

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

4. **sqlFunction.cs** dosyasını kaydedin.

5. **sqlFunction.csproj** dosyasını açın.

6. Paket başvuru grubunu bulun ve SqlClient'ı eklemek için yeni bir tane ekleyin.

   ```csproj
   <PackageReference Include="System.Data.SqlClient" Version="4.5.1"/>
   ```

7. **sqlFunction.csproj** dosyasını kaydedin.

## <a name="add-the-sql-server-container"></a>SQL Server kapsayıcısını ekleme

[Dağıtım bildirimi](module-composition.md), IoT Edge çalışma zamanının hangi modüllerinin IoT Edge cihazınıza yükleneceğini bildirir. Kodu önceki bölümde özelleştirilmiş bir İşlev modülü yapmak için sağladınız, ancak SQL Server modülü zaten oluşturulmuş ve Azure Marketi'nde kullanılabilir. Tek yapmanız gereken IoT Edge çalışma zamanına bunu dahil etmek ve cihazınızda yapılandırmaktır.

1. Visual Studio Code'da Komut paletini **seçerek** > komut**paletini**açın.

2. Komut paletinde Azure IoT Edge komutunu yazın ve **çalıştırın: IoT Edge modül**ekle. Komut paletinde, yeni bir modül eklemek için aşağıdaki bilgileri sağlayın:

   | Alan | Değer |
   | ----- | ----- |
   | Dağıtım şablonu dosyasını seçin | Komut paleti, geçerli çözüm klasörünüzde deployment.template.json dosyasını vurgular. O dosyayı seç.  |
   | Modül şablonunu seçin | **Azure Marketi'nden Modül'u**seçin. |

3. Azure IoT Edge modül pazarda SQL **Server Modülünü**arayın ve seçin.

4. Modül adını **sql**olarak değiştirin , tüm küçük harf. Bu ad, sqlFunction.cs dosyasındaki bağlantı dizesinde bildirilen kapsayıcı adıyla eşleşir.

5. Modülü çözümünüze eklemek için **İçe Aktar'ı** seçin.

6. Çözüm klasörünüzde **deployment.template.json** dosyasını açın.

7. **Modüller** bölümünü bulun. Üç modül görmelisin. *Simüle Edilen Sıcaklık Sensörü* modülü varsayılan olarak yeni çözümlere dahildir ve diğer modüllerinizle birlikte kullanılacak test verilerini sağlar. SqlFunction *sqlFunction* modülü, başlangıçta oluşturduğunuz ve yeni kodla güncelleştirdiğiniz modüldür. Son olarak, *sql* modülü Azure Marketi'nden alındı.

   >[!Tip]
   >SQL Server modülü, dağıtım bildiriminin ortam değişkenlerinde ayarlanmış varsayılan bir parolayla birlikte gelir. Üretim ortamında bir SQL Server kapsayıcısı oluşturduğunuzda [varsayılan sistem yöneticisi parolasını değiştirmeniz gerekir](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker).

8. **Deployment.template.json** dosyasını kapatın.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümlerde tek modüle sahip bir çözüm oluşturdunuz ve dağıtım bildirimi şablonuna başka bir modül daha eklediniz. SQL Server modülü Microsoft tarafından genel olarak barındırılır, ancak Işlevler modülündeki kodu kapsayıcılaştırmanız gerekir. Bu bölümde, çözümü oluşturur, sqlFunction modülü için kapsayıcı görüntüleri oluşturur ve görüntüyü konteyner kayıt defterinize itersiniz.

1. Visual Studio Code'da, **View** > **Terminali'ni**seçerek entegre terminali açın.  

1. Görüntüleri kayıt defterinize gönderebilmeniz için kapsayıcı kayıt defterinizi Visual Studio Code'a ekleyin. .env dosyasına eklediğiniz aynı Azure Kapsayıcı Kayıt Defteri (ACR) kimlik bilgilerini kullanın. Tümleşik terminale aşağıdaki komutu girin:

    ```csh/sh
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

    --password-stdin parametresinin kullanılmasını öneren bir güvenlik uyarısı görebilirsiniz. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) komutu başvurusuna bakın.

1. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayın ve **IoT Edge çözümünü Oluştur ve İti**

Visual Studio Code'a çözümünüzü oluşturmasını söylediğiniz zaman, önce dağıtım şablonundaki bilgileri alır ve **config**adlı yeni bir klasörde bir deployment.json dosyası oluşturur. Daha sonra, tümleşik terminalde `docker build` iki `docker push`komut çalışır: ve . Yapı komutu kodunuzu oluşturur ve modülü konteynerleştirir. Ardından, push komutu, çözümü başharfe aldığınızda belirttiğiniz kapsayıcı kayıt defterine kodu iter.

sqlFunction modülünün başarıyla konteyner kayıt defterinize itildiğini doğrulayabilirsiniz. Azure portalında konteyner kayıt defterinize gidin. **Depoları** seçin ve **sqlFunction**için arama yapın. Diğer iki modül, SimulatedTemperatureSensor ve sql, depoları microsoft kayıt defterlerinde olduğundan konteyner kayıt defterinize itilmez.

## <a name="deploy-the-solution-to-a-device"></a>Çözümü bir cihaza dağıtma

IoT Hub üzerinden bir cihazda modül ayarlayabilirsiniz ancak IoT Hub ve cihazlara Visual Studio Code aracılığıyla da erişebilirsiniz. Bu bölümde IoT Hub'ınıza erişim ayarlarını yapacak ve ardından çözümünüzü IoT Edge cihazınıza dağıtmak için VS Code uygulamasını kullanacaksınız.

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin.

2. Dağıtımınızla hedeflemek istediğiniz cihaza sağ tıklayıp **Create deployment for single device** (Tek cihaz için dağıtım oluştur) öğesini seçin.

3. Dosya gezgininde, çözümünüz içindeki **config** klasörüne gidin ve **deployment.amd64'ü**seçin. **Kenar dağıtım bildirimini seç'i**tıklatın.

   Deployment.template.json dosyasını dağıtım bildirimi olarak kullanmayın.

Dağıtım başarılı olursa VS Code çıkışında onay iletisi yazdırılır.

VS Kodu'nun Azure IoT Hub Cihazları bölümünde cihazınızın durumunu yenileyin. Yeni modüller listelenir ve kapsayıcılar yüklendikçe ve başlatılırken önümüzdeki birkaç dakika içinde çalışıyor olarak rapor lanmaya başlar. Tüm modüllerin cihazınızda çalışıp çalışmadığını da kontrol edebilirsiniz. IoT Edge cihazında modüllerin durumunu görmek için aşağıdaki komutu çalıştırın.

   ```cmd/sh
   iotedge list
   ```

## <a name="create-the-sql-database"></a>SQL veritabanını oluşturma

Dağıtım bildirimini cihazınıza uyguladığınızda üç modül çalışmaya başlar. Simüle Edilen TemperatureSensor modülü simüle edilmiş ortam verileri üretir. sqlFunction modülü verileri alır ve veritabanı için biçimlendirir. Bu bölüm, sıcaklık verilerini depolamak için kullanılacak SQL veritabanını ayarlama sürecinde size yardımcı olacaktır.

IoT Edge cihazınızda aşağıdaki komutları çalıştırın. Bu komutlar cihazınızda çalışan **sql** modülüne bağlanır ve gönderilen sıcaklık verilerini tutmak için bir veritabanı ve tablo oluşturur.

1. IoT Edge cihazınızdaki bir komut satırı aracında veritabanınıza bağlanın.

      ```bash
      sudo docker exec -it sql bash
      ```

2. SQL komut aracını açın.

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Veritabanınızı oluşturun:

      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. Tablonuzu tanımlayın.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

SQL Server Docker dosyanızı özelleştirerek SQL Server örneğinizin birden fazla IoT Edge cihazına otomatik olarak dağıtılmasını sağlayabilirsiniz. Daha fazla bilgi için bkz. [Microsoft SQL Server kapsayıcısı demo proje](https://github.com/twright-msft/mssql-node-docker-demo-app).

## <a name="view-the-local-data"></a>Yerel verileri görüntüleme

Tablonuz oluşturulduktan sonra sqlFunction modülü verileri IoT Edge cihazınızdaki yerel bir SQL Server 2017 veritabanında depolamaya başlar.

SQL komut aracında biçimlendirilmiş tablo verilerinizi görüntülemek için aşağıdaki komutu çalıştırın:

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Yerel veritabanının içeriğini görüntüleme](./media/tutorial-store-data-sql-server/view-data.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Edge cihazınız tarafından oluşturulan ham verileri filtreleme kodunu içeren bir Azure İşlevleri modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [Visual Studio Code için Azure IoT Edge ile Azure İşlevlerini](how-to-develop-csharp-function.md)Nasıl Geliştirebileceğiniz hakkında daha fazla bilgi edinebilirsiniz.

Kenardan başka bir depolama yöntemi denemek istiyorsanız, IoT Edge'de Azure Blob Depolama'nın nasıl kullanılacağı hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [IoT Edge'de Azure Blob Depolama ile verileri kenarda depolama](how-to-store-data-blob.md)
