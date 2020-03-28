---
title: "Öğretici: Olay Hub'ları verilerini veri ambarına gönderme - Olay Izgara"
description: "Öğretici: Verileri BIR SQL Veri Ambarına geçirmek için Azure Olay Izgarave Olay Hub'larının nasıl kullanılacağını açıklar. Bir Yakalama dosyasını almak için bir Azure İşlevi kullanır."
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: spelluru
ms.openlocfilehash: 6f5bd129b175210cd5b9415a65b8db06d904e24d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73718188"
---
# <a name="tutorial-stream-big-data-into-a-data-warehouse"></a>Öğretici: Büyük verileri veri ambarına aktarın
Azure [Olay Ağıt,](overview.md) uygulamalardan ve hizmetlerden gelen bildirimlere (olaylara) tepki vermenizi sağlayan akıllı bir olay yönlendirme hizmetidir. Örneğin, bir Azure Blob depolama sına veya Azure Veri Gölü Depolamasına yakalanan Olay Hub'ları verilerini işlemek ve verileri diğer veri depolarına geçirmek için bir Azure İşlevi'ni tetikleyebilir. Bu [Olay Hub'ları ve Olay Kılavuz tümleştirme örneği,](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) yakalanan Olay Hub'ları verilerini blob depolamadan bir SQL Veri Ambarına sorunsuz bir şekilde geçirmek için Olay Kılavuzlu Olay Hub'larının nasıl kullanılacağını gösterir.

![Uygulamaya genel bakış](media/event-grid-event-hubs-integration/overview.png)

Bu diyagram, bu öğreticide oluşturduğunuz çözümün iş akışını görüntüler: 

1. Azure etkinlik hub'ına gönderilen veriler bir Azure blob depolama alanında yakalanır.
2. Veri yakalama tamamlandığında, bir olay oluşturulur ve bir Azure olay ızgarasına gönderilir. 
3. Olay ızgarası bu olay verilerini bir Azure işlev uygulamasına iletir.
4. İşlev uygulaması, depodaki blob'u almak için olay verilerindeki blob URL'sini kullanır. 
5. İşlev uygulaması blob verilerini bir Azure SQL veri ambarına aktarır. 

Bu makalede, aşağıdaki adımları izleyin:

> [!div class="checklist"]
> * Altyapıyı dağıtmak için Azure Kaynak Yöneticisi şablonu kullanın: etkinlik merkezi, depolama hesabı, işlev uygulaması, BIR SQL veri ambarı.
> * Veri ambarında bir tablo oluşturun.
> * İşlev uygulamasına kod ekleyin.
> * Etkinliğe abone olun. 
> * Etkinlik merkezine veri gönderen uygulamayı çalıştırın.
> * Geçirilen verileri veri ambarında görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* .NET masaüstü geliştirme, Azure geliştirme, ASP.NET ve web geliştirme, Node.js geliştirme ve Python geliştirme gibi iş yükleriyle visual [studio 2019.](https://www.visualstudio.com/vs/)
* [EventHubsCaptureEventGridDemo örnek projesini](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) bilgisayarınıza indirin.

## <a name="deploy-the-infrastructure"></a>Altyapıyı dağıtma
Bu adımda, kaynak [yöneticisi şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json)ile gerekli altyapıyı dağıtMış sınız. Şablonu dağıttığınızda, aşağıdaki kaynaklar oluşturulur:

* Yakalama özelliği etkin leştirilmiş olay hub'ı.
* Yakalanan dosyaların depolama hesabı. 
* İşlev uygulaması barındırma için uygulama hizmeti planı
* Olayı işlemek için işlev uygulaması
* Veri ambarını barındırmak için SQL Server
* Geçirilen verileri depolamak için SQL Veri Ambarı

### <a name="launch-azure-cloud-shell-in-azure-portal"></a>Azure portalında Azure Bulut Shell'i başlatın

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Üstteki **Bulut Kabuğu** düğmesini seçin.

    ![Azure portalında](media/event-grid-event-hubs-integration/azure-portal.png)
3. Bulut Kabuğu'nun tarayıcının alt kısmında açıldığını görürsünüz.

    ![Cloud Shell](media/event-grid-event-hubs-integration/launch-cloud-shell.png) 
4. Bulut Kabuğu'nda, **Bash** ve **PowerShell**arasında seçim yapmak için bir seçenek görürseniz, **Bash'i**seçin. 
5. Bulut Kabuğu'nu ilk kez kullanıyorsanız, **depolama oluştur'u**seçerek bir depolama hesabı oluşturun. Azure Bulut Kabuğu, bazı dosyaları depolamak için bir Azure depolama hesabı gerektirir. 

    ![Bulut kabuğu için depolama alanı oluşturma](media/event-grid-event-hubs-integration/create-storage-cloud-shell.png)
6. Bulut Kabuğu nun başlatılmasını bekleyin. 

    ![Bulut kabuğu için depolama alanı oluşturma](media/event-grid-event-hubs-integration/cloud-shell-initialized.png)


### <a name="use-azure-cli"></a>Azure CLI kullanma

1. Aşağıdaki CLI komutunu çalıştırarak bir Azure kaynak grubu oluşturun: 
    1. Aşağıdaki komutu Bulut Kabuğu penceresine kopyalayıp yapıştırın

        ```azurecli
        az group create -l eastus -n <Name for the resource group>
        ```
    1. **Kaynak grubu** için bir ad belirtin
    2. **ENTER**tuşuna basın. 

        Örnek aşağıda verilmiştir:
    
        ```azurecli
        user@Azure:~$ az group create -l eastus -n ehubegridgrp
        {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/ehubegridgrp",
          "location": "eastus",
          "managedBy": null,
          "name": "ehubegridgrp",
          "properties": {
            "provisioningState": "Succeeded"
          },
          "tags": null
        }
        ```
2. Önceki bölümde belirtilen tüm kaynakları (olay merkezi, depolama hesabı, fonksiyonlar uygulaması, SQL veri ambarı) aşağıdaki CLI komutunu çalıştırarak dağıtın: 
    1. Komutu kopyalayıp Bulut Kabuğu penceresine yapıştırın. Alternatif olarak, seçtiğiniz bir düzenleyiciye kopyalamak/yapıştırmak, değerleri ayarlamak ve ardından komutu Bulut Kabuğu'na kopyalamak isteyebilirsiniz. 

        ```azurecli
        az group deployment create \
            --resource-group rgDataMigrationSample \
            --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
            --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
        ```
    2. Aşağıdaki varlıklar için değerleri belirtin:
        1. Daha önce oluşturduğunuz kaynak grubunun adı.
        2. Olay hub ad alanının adı. 
        3. Olay merkezinin adı. Değeri olduğu gibi bırakabilirsiniz (hubdatamigration).
        4. SQL sunucusunun adı.
        5. SQL kullanıcı ve şifre adı. 
        6. SQL veri ambarı adı
        7. Depolama hesabının adı. 
        8. İşlev uygulamasının adı. 
    3.  Komutu çalıştırmak için Bulut Kabuğu penceresinde **ENTER** tuşuna basın. Bir sürü kaynak oluşturduğunuz için bu işlem biraz zaman alabilir. Komutun sonucu olarak, herhangi bir hata olmadığından emin olun. 
    

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

1. Azure Bulut Shell'de PowerShell moduna geçin. Azure Bulut Shell'in sol üst köşesindeaşağı ok seçin ve **PowerShell'i**seçin.

    ![PowerShell'e Geç](media/event-grid-event-hubs-integration/select-powershell-cloud-shell.png)
2. Aşağıdaki komutu çalıştırarak bir Azure kaynak grubu oluşturun: 
    1. Aşağıdaki komutu bulut kabuğu penceresine kopyalayıp yapıştırın.

        ```powershell
        New-AzResourceGroup -Name rgDataMigration -Location westcentralus
        ```
    2. **Kaynak grubu**için bir ad belirtin.
    3. ENTER tuşuna basın. 
3. Önceki bölümde belirtilen tüm kaynakları (olay merkezi, depolama hesabı, fonksiyonlar uygulaması, SQL veri ambarı) aşağıdaki komutu çalıştırarak dağıtın:
    1. Komutu kopyalayıp Bulut Kabuğu penceresine yapıştırın. Alternatif olarak, seçtiğiniz bir düzenleyiciye kopyalamak/yapıştırmak, değerleri ayarlamak ve ardından komutu Bulut Kabuğu'na kopyalamak isteyebilirsiniz. 

        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
        ```
    2. Aşağıdaki varlıklar için değerleri belirtin:
        1. Daha önce oluşturduğunuz kaynak grubunun adı.
        2. Olay hub ad alanının adı. 
        3. Olay merkezinin adı. Değeri olduğu gibi bırakabilirsiniz (hubdatamigration).
        4. SQL sunucusunun adı.
        5. SQL kullanıcı ve şifre adı. 
        6. SQL veri ambarı adı
        7. Depolama hesabının adı. 
        8. İşlev uygulamasının adı. 
    3.  Komutu çalıştırmak için Bulut Kabuğu penceresinde **ENTER** tuşuna basın. Bir sürü kaynak oluşturduğunuz için bu işlem biraz zaman alabilir. Komutun sonucu olarak, herhangi bir hata olmadığından emin olun. 

### <a name="close-the-cloud-shell"></a>Bulut Kabuğunu Kapat 
Bulut **Kabuğu** penceresinin sağ üst köşesindeki portal (veya) **X** düğmesini seçerek bulut kabuğunu kapatın. 

### <a name="verify-that-the-resources-are-created"></a>Kaynakların oluşturulduğunu doğrulama

1. Azure portalında, sol menüdeki **Kaynak gruplarını** seçin. 
2. Arama kutusuna kaynak grubunuzun adını girerek kaynak gruplarının listesini filtreleyin. 
3. Listedeki kaynak grubunu seçin.

    ![Kaynak grubunuzu seçin](media/event-grid-event-hubs-integration/select-resource-group.png)
4. Kaynak grubunda aşağıdaki kaynakları gördüğünüzden onaylayın:

    ![Kaynak grubundaki kaynaklar](media/event-grid-event-hubs-integration/resources-in-resource-group.png)

### <a name="create-a-table-in-sql-data-warehouse"></a>SQL Veri Ambarında tablo oluşturma
[CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) komut dosyasını çalıştırarak veri ambarınızda bir tablo oluşturun. Komut dosyasını çalıştırmak için Visual Studio'yu veya portaldaki Sorgu Düzenleyicisi'ni kullanabilirsiniz. Aşağıdaki adımlar, Sorgu Düzenleyicisi'ni nasıl kullanacağınızı gösterir: 

1. Kaynak grubundaki kaynaklar listesinde SQL veri ambarınızı seçin. 
2. SQL veri ambarı sayfasında, sol menüde **Sorgu düzenleyicisini (önizleme)** seçin. 

    ![SQL veri ambarı sayfası](media/event-grid-event-hubs-integration/sql-data-warehouse-page.png)
2. SQL sunucusu için **kullanıcı** ve **parola** adını girin ve **Tamam'ı**seçin. 

    ![SQL Server kimlik doğrulaması](media/event-grid-event-hubs-integration/sql-server-authentication.png)
4. Sorgu penceresinde, aşağıdaki SQL komut dosyasını kopyalayın ve çalıştırın: 

    ```sql
    CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
        [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
        [MeasureTime] datetime NULL, 
        [GeneratedPower] float NULL, 
        [WindSpeed] float NULL, 
        [TurbineSpeed] float NULL
    )
    WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    ```

    ![SQL sorgusunu çalıştır](media/event-grid-event-hubs-integration/run-sql-query.png)
5. Verilerin öğreticinin sonunda oluşturulduğunu doğrulamak için bu sekmeyi veya pencereyi açık tutun. 


## <a name="publish-the-azure-functions-app"></a>Azure İşlevleri uygulamasını yayımlama

1. Visual Studio’yu başlatın.
2. Ön koşulların bir parçası olarak [GitHub'dan](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) indirdiğiniz **EventHubsCaptureEventGridDemo.sln** çözümünün açın.
3. Çözüm Gezgini’nde **FunctionEGDWDumper**’a sağ tıklayın ve **Yayımla**’yı seçin.

   ![İşlev uygulaması yayımlama](media/event-grid-event-hubs-integration/publish-function-app.png)
4. Aşağıdaki ekranı görürseniz **Başlat'ı**seçin. 

   ![Yayımla düğmesini başlat](media/event-grid-event-hubs-integration/start-publish-button.png) 
5. **Yayımlama hedef** sayfasını seç'te **varolan** seçeneği seçin ve **Profil Oluştur'u**seçin. 

   ![Yayımlama hedefi seçme](media/event-grid-event-hubs-integration/publish-select-existing.png)
6. Uygulama Hizmeti sayfasında, **Azure aboneliğinizi**seçin, kaynak grubunuzdaki **işlev uygulamasını** seçin ve **Tamam'ı**seçin. 

   ![Uygulama Hizmeti sayfası](media/event-grid-event-hubs-integration/publish-app-service.png) 
1. Visual Studio profili yapılandırdığında **Yayımla**’yı seçin.

   ![Yayımla’yı seçme](media/event-grid-event-hubs-integration/select-publish.png)

İşlevi yayımladıktan sonra olaya abone olmaya hazır olursunuz.

## <a name="subscribe-to-the-event"></a>Olaya abone olma

1. Yeni bir sekmede veya bir web tarayıcısının yeni penceresinde Azure [portalına](https://portal.azure.com)gidin.
2. Azure portalında, sol menüdeki **Kaynak gruplarını** seçin. 
3. Arama kutusuna kaynak grubunuzun adını girerek kaynak gruplarının listesini filtreleyin. 
4. Listedeki kaynak grubunu seçin.

    ![Kaynak grubunuzu seçin](media/event-grid-event-hubs-integration/select-resource-group.png)
4. Listedeki Uygulama Hizmeti planını seçin. 
5. Uygulama Hizmeti Planı sayfasında, sol menüdeki **Uygulamalar'ı** seçin ve işlev uygulamasını seçin. 

    ![Fonksiyonlar uygulamanızı seçin](media/event-grid-event-hubs-integration/select-function-app-app-service-plan.png)
6. İşlev uygulamasını genişletin, işlevlerinizi genişletin ve ardından işlevinizi seçin. 

    ![Azure işlevinizi seçin](media/event-grid-event-hubs-integration/select-function-add-button.png)
7. Araç çubuğunda **Olay Kılavuz ekle aboneliğini** seçin. 
8. Olay **Izgara Aboneliği Oluştur** sayfasında aşağıdaki işlemleri yapın: 
    1. KONU **AYRINTILARI** bölümünde aşağıdaki işlemleri yapın:
        1. Azure aboneliğinizi seçin.
        2. Azure kaynak grubunu seçin.
        3. Olay Hub'ları ad alanını seçin.
    2. EVENT **SUBSCRIPTION DETAILS** sayfasında, abonelik için bir ad girin (örneğin: captureEventSub) ve **Oluştur'u**seçin. 

        ![Olay Izgara aboneliği oluşturma](media/event-grid-event-hubs-integration/create-event-subscription.png)

## <a name="run-the-app-to-generate-data"></a>Veri oluşturmak için uygulama çalıştırma
Olay hub’ı, SQL veri ambarı, Azure işlev uygulaması ve olay aboneliğinizi ayarlamayı tamamladınız. Olay hub’ı için veri oluşturan bir uygulamayı çalıştırmadan önce birkaç değeri yapılandırmanız gerekir.

1. Azure portalında, daha önce yaptığınız gibi kaynak grubunuza gidin. 
2. Olay Hub'ları ad alanını seçin.
3. Olay **Hub'ları Ad Alanı** sayfasında, sol menüde **Paylaşılan erişim ilkeleri'ni** seçin.
4. İlkeler listesinde **RootManageSharedAccessKey'i** seçin. 
5. **Bağlantı dizesi birincil anahtar** metin kutusunun yanındaki kopya düğmesini seçin. 

    ![Olay göbeği ad alanı için bağlantı dizesi](media/event-grid-event-hubs-integration/get-connection-string.png)
1. Visual Studio çözümüne geri dön. 
2. WindTurbineDataGenerator projesinde **program.cs** dosyasını açın.
5. İki sabit değeri değiştirin. **EventHubConnectionString** için kopyalanan değeri kullanın. Olay hub’ının adı için **hubdatamigration** kullanın. Olay hub'ı için farklı bir ad kullandıysanız, bu adı belirtin. 

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Çözümü derleyin. **WindTurbineGenerator.exe** uygulamasını çalıştırın. 
7. Birkaç dakika sonra, veri ambarınızdaki tabloda geçirilen verileri sorgulayın.

    ![Sorgu sonuçları](media/event-grid-event-hubs-integration/query-results.png)

### <a name="event-data-generated-by-the-event-hub"></a>Olay merkezi tarafından oluşturulan olay verileri
Event Grid, olay verilerini abonelere dağıtır. Aşağıdaki örnek, bir olay hub'ı üzerinden veri akışı bir blob yakalanan oluşturulan olay verilerini gösterir. Özellikle, nesnedeki `fileUrl` özelliğin `data` depodaki blob'a işaret ettigini fark edin. İşlev uygulaması yakalanan verilerle blob dosyasını almak için bu URL'yi kullanır.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```


## <a name="next-steps"></a>Sonraki adımlar

* Azure mesajlaşma servislerindeki farklar hakkında bilgi sahibi olmak için, bkz. [iletiler teslim eden Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Hubs Capture’a giriş için bkz. [Azure portalını kullanarak Event Hubs Capture’ı etkinleştirme](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Örneği ayarlama ve çalıştırma hakkında daha fazla bilgi için bkz. [Event Hubs Capture ve Event Grid örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).
