---
title: Tutorial store data with SQL module - Azure IoT Edge | Microsoft Docs
description: Bir SQL Server modülüyle IoT Edge cihazınızda yerel veri depolamayı öğrenin
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5a3133100621cee2e786c4001df02f2316b1e4ec
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457057"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Öğretici: SQL Server veritabanları ile uç cihazlarda veri depolama

Deploy a SQL Server module to store data on a Linux device running Azure IoT Edge.

Uç cihazlarda veri depolamak ve sorgulamak için Azure IoT Edge ve SQL Server işlevlerini kullanın. Azure IoT Edge has basic storage capabilities to cache messages if a device goes offline, and then forward them when the connection is reestablished. Ancak sorgu verilerini yerel ortamda sorgulayabilme gibi daha gelişmiş depolama özelliklerine ihtiyaç duyabilirsiniz. Your IoT Edge devices can use local databases to perform more complex computing without having to maintain a connection to IoT Hub. 

Bu makalede bir IoT Edge cihazına SQL Server veritabanı dağıtma yönergeleri yer almaktadır. IoT Edge cihazında çalışan Azure İşlevleri, gelen verileri yapılandırıp veritabanına gönderir. Bu makaledeki adımlar MySQL veya PostgreSQL gibi kapsayıcılarda çalışan diğer veritabanlarına da uygulanabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Visual Studio Code kullanarak Azure İşlevi oluşturma
> * IoT Edge cihazınıza bir SQL veritabanı dağıtma
> * Visual Studio Code uygulamasını kullanarak modülleri derleme ve IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Before beginning this tutorial, you should have gone through the previous tutorial to set up your development environment for Linux container development: [Develop IoT Edge modules for Linux devices](tutorial-develop-for-linux.md). By completing that tutorial, you should have the following prerequisites in place: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Linux device running Azure IoT Edge](quickstart-linux.md)
* A container registry, like [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) configured with the [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) configured to run Linux containers.

This tutorial uses an Azure Functions module to send data to the SQL Server. To develop an IoT Edge module with Azure Functions, install the following additional prerequisites on your development machine: 

* [C# for Visual Studio Code (powered by OmniSharp) extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download). 

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

Bir veritabanına veri göndermek için verileri doğru şekilde yapılandırıp tabloda kaydeden bir modüle ihtiyacınız vardır. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

The following steps show you how to create an IoT Edge function using Visual Studio Code and the Azure IoT Tools.

1. Visual Studio Code'u açın.

2. **View (Görünüm)**  > **Command palette (Komut paleti)** öğesini seçerek VS Code komut paletini açın.

3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu yazıp çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Enter a descriptive name for your solution, like **SqlSolution**, or accept the default. |
   | Modül şablonunu seçin | Choose **Azure Functions - C#** . |
   | Modül adı sağlayın | Modülünüze **sqlFunction** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>The final string looks like \<registry name\>.azurecr.io/sqlfunction. |

   VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. 
   
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

1. In the VS Code explorer, open **modules** > **sqlFunction** > **sqlFunction.cs**.

2. Replace the entire contents of the file with the following code:

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

3. In line 35, replace the string **\<sql connection string\>** with the following string. The **Data Source** property references the SQL Server container, which doesn't exist yet but you will create it with the name **SQL** in the next section. 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

4. Save the **sqlFunction.cs** file. 

5. Open the **sqlFunction.csproj** file.

6. Find the group of package references, and add a new one to include SqlClient. 

   ```csproj
   <PackageReference Include="System.Data.SqlClient" Version="4.5.1"/>
   ```

7. Save the **sqlFunction.csproj** file.

## <a name="add-the-sql-server-container"></a>Add the SQL Server container

[Dağıtım bildirimi](module-composition.md), IoT Edge çalışma zamanının hangi modüllerinin IoT Edge cihazınıza yükleneceğini bildirir. You provided the code to make a customized Function module in the previous section, but the SQL Server module is already built and available in the Azure Marketplace. Tek yapmanız gereken IoT Edge çalışma zamanına bunu dahil etmek ve cihazınızda yapılandırmaktır. 

1. In Visual Studio Code, open the command palette by selecting **View** > **Command palette**.

2. In the command palette, type and run the command **Azure IoT Edge: Add IoT Edge module**. In the command palette, provide the following information to add a new module: 

   | Alan | Değer | 
   | ----- | ----- |
   | Dağıtım şablonu dosyasını seçin | The command palette highlights the deployment.template.json file in your current solution folder. Select that file.  |
   | Modül şablonunu seçin | Select **Module from Azure Marketplace**. |

3. In the Azure IoT Edge module marketplace, search for and select **SQL Server Module**. 

4. Change the module name to **sql**, all lowercase. This name matches the container name declared in the connection string in the sqlFunction.cs file. 

5. Select **Import** to add the module to your solution. 

6. In your solution folder, open the **deployment.template.json** file. 

7. Find the **modules** section. You should see three modules. The module *SimulatedTemperatureSensor* is included by default in new solutions, and provides test data to use with your other modules. The module *sqlFunction* is the module that you initially created and updated with new code. Finally, the module *sql* was imported from the Azure Marketplace. 

   >[!Tip]
   >The SQL Server module comes with a default password set in the environment variables of the deployment manifest. Üretim ortamında bir SQL Server kapsayıcısı oluşturduğunuzda [varsayılan sistem yöneticisi parolasını değiştirmeniz gerekir](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker).

8. Close the **deployment.template.json** file.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümlerde tek modüle sahip bir çözüm oluşturdunuz ve dağıtım bildirimi şablonuna başka bir modül daha eklediniz. The SQL Server module is hosted publicly by Microsoft, but you need to containerize the code in the Functions module. In this section, you build the solution, create container images for the sqlFunction module, and push the image to your container registry. 

1. Visual Studio Code'da **Görünüm** > **Terminal**'i seçerek tümleşik terminali açın.  

1. Görüntüleri kayıt defterinize gönderebilmeniz için kapsayıcı kayıt defterinizi Visual Studio Code'a ekleyin. Use the same Azure Container Registry (ACR) credentials that you added to the .env file. Tümleşik terminale aşağıdaki komutu girin:

    ```csh/sh
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```
    
    You might see a security warning recommending the use of the --password-stdin parameter. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. For more information, see the [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) command reference. 

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

When you tell Visual Studio Code to build your solution, it first takes the information in the deployment template and generates a deployment.json file in a new folder named **config**. Then, it runs two commands in the integrated terminal: `docker build` and `docker push`. These two commands build your code, containerize the module, and then push the code to the container registry that you specified when you initialized the solution. 

You can verify that the sqlFunction module was successfully pushed to your container registry. In the Azure portal, navigate to your container registry. Select **repositories** and search for **sqlFunction**. The other two modules, SimulatedTemperatureSensor and sql, won't be pushed to your container registry because you're already pointing to their repositories in the Microsoft registries.

## <a name="deploy-the-solution-to-a-device"></a>Çözümü bir cihaza dağıtma

IoT Hub üzerinden bir cihazda modül ayarlayabilirsiniz ancak IoT Hub ve cihazlara Visual Studio Code aracılığıyla da erişebilirsiniz. Bu bölümde IoT Hub'ınıza erişim ayarlarını yapacak ve ardından çözümünüzü IoT Edge cihazınıza dağıtmak için VS Code uygulamasını kullanacaksınız. 

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

2. Dağıtımınızla hedeflemek istediğiniz cihaza sağ tıklayıp **Create deployment for single device** (Tek cihaz için dağıtım oluştur) öğesini seçin. 

3. In the file explorer, navigate to the **config** folder inside your solution and choose **deployment.amd64**. **Select Edge deployment manifest** (Edge dağıtım bildirimini seç) öğesine tıklayın. 

   Do not use the deployment.template.json file as a deployment manifest.

Dağıtım başarılı olursa VS Code çıkışında onay iletisi yazdırılır. 

Refresh the status of your device in the Azure IoT Hub Devices section of VS Code. The new modules are listed and will start to report as running over the next few minutes as the containers are installed and started. Tüm modüllerin cihazınızda çalışıp çalışmadığını da kontrol edebilirsiniz. IoT Edge cihazında modüllerin durumunu görmek için aşağıdaki komutu çalıştırın. 

   ```cmd/sh
   iotedge list
   ```

## <a name="create-the-sql-database"></a>SQL veritabanını oluşturma

Dağıtım bildirimini cihazınıza uyguladığınızda üç modül çalışmaya başlar. The SimulatedTemperatureSensor module generates simulated environment data. sqlFunction modülü verileri alır ve veritabanı için biçimlendirir. Bu bölüm, sıcaklık verilerini depolamak için kullanılacak SQL veritabanını ayarlama sürecinde size yardımcı olacaktır. 

Run the following commands on your IoT Edge device. These commands connect to the **sql** module running on your device and create a database and table to hold the temperature data being sent to it. 

1. In a command-line tool on your IoT Edge device, connect to your database. 
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

   ![View contents of local database](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Edge cihazınız tarafından oluşturulan ham verileri filtreleme kodunu içeren bir Azure İşlevleri modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile Azure İşlevleri Geliştirme](how-to-develop-csharp-function.md) hakkında daha fazla bilgi edinebilirsiniz. 

If you want to try another storage method at the edge, read about how to use Azure Blob Storage on IoT Edge. 

> [!div class="nextstepaction"]
> [IoT Edge'de Azure Blob Depolama ile verileri kenarda depolama](how-to-store-data-blob.md)
