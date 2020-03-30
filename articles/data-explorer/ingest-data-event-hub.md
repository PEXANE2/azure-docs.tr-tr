---
title: Olay Hub'ından Azure Veri Gezgini'ne veri alma
description: Bu makalede, Olay Hub'ından Azure Veri Gezgini'ne nasıl veri öttürecek (yüklenir) öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: bb9357ca4388bd1fb7ae3e3704cf4112d07c1105
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77188198"
---
# <a name="ingest-data-from-event-hub-into-azure-data-explorer"></a>Olay Hub'ından Azure Veri Gezgini'ne veri alma

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-hub.md)
> * [C #](data-connection-event-hub-csharp.md)
> * [Python](data-connection-event-hub-python.md)
> * [Azure Resource Manager şablonu](data-connection-event-hub-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, büyük veri akış platformu ve olay ekleme hizmeti olan Event Hubs'dan veri eklemeyi (veri yüklemeyi) destekler. [Olay Hub'ları](/azure/event-hubs/event-hubs-about) saniyede milyonlarca olayı neredeyse gerçek zamanlı olarak işleyebilir. Bu makalede, bir olay hub'ı oluşturur, Azure Veri Gezgini'nden ona bağlanır ve sistem üzerinden veri akışını görürsünüz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir test kümesi ve veritabanı.](create-cluster-database-portal.md)
* Veri üreten ve olay merkezine gönderen örnek bir [uygulama.](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) Örnek uygulamayı sisteminize indirin.
* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) örnek uygulamayı çalıştırmak için.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bu makalede, örnek veri oluşturmak ve bir olay hub'ına gönderin. İlk adım bir olay hub'ı oluşturmaktır. Bunun için Azure portalda bir Azure Resource Manager şablonu kullanacaksınız.

1. Olay hub'ı oluşturmak için dağıtımı başlatmak için aşağıdaki düğmeyi kullanın. Bu makaledeki adımların geri kalanını takip etmek için **yeni pencerede Aç'ı**sağ tıklatın ve seçin.

    [![Azure'a Dağıt](media/ingest-data-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

    **Azure'a dağıtma** düğmesi Azure portalda doldurmanız gereken bir form sayfasını açar.

    ![Azure’a Dağıt](media/ingest-data-event-hub/deploy-to-azure.png)

1. Olay hub'ının oluşturulmasını istediğiniz aboneliği seçin ve *test-hub-rg* adlı bir kaynak grubu oluşturun.

    ![Kaynak grubu oluşturma](media/ingest-data-event-hub/create-resource-group.png)

1. Formu aşağıdaki bilgilerle doldurun.

    ![Dağıtım formu](media/ingest-data-event-hub/deployment-form.png)

    Aşağıdaki tabloda yer almayan ayarlar için varsayılan değerleri kullanın.

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Olay hub'ınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *test-hub-rg* | Yeni bir kaynak grubu oluşturun. |
    | Konum | *Batı ABD* | Bu makale için *Batı ABD'yi* seçin. Üretim sisteminde ihtiyaçlarınıza en uygun bölgeyi seçmeniz gerekir. En iyi performans için Kusto kümesiyle aynı Konumda olay hub ad alanını oluşturun (en önemlisi, yüksek iş geleni olan olay hub ad alanları için).
    | Ad alanı adı | Benzersiz bir ad alanı adı | Ad alanınızı tanımlayan benzersiz bir ad seçin. Örneğin, *mytestnamespace*. Girdiğiniz adın sonuna *servicebus.windows.net* etki alanı adı eklenir. Ad yalnızca küçük harf, sayı ve kısa çizgi içerebilir. Ad bir harf ile başlamalı ve harf veya sayı ile bitmelidir. Değer uzunluğu 6 ile 50 karakter arasında olmalıdır.
    | Olay hub'ı adı | *test-hub* | Olay hub'ı benzersiz bir kapsayıcı kapsamı sunan ad alanında bulunur. Olay hub'ı adının ad alanında benzersiz olması gerekir. |
    | Tüketici grubu adı | *test-group* | Tüketici grupları birden fazla tüketici uygulamasının ayrı olay akışı görünümüne sahip olmasını sağlar. |
    | | |

1. **Satın al**'ı seçerek aboneliğinizde kaynak oluşturduğunuzu onaylayın.

1. Araç çubuğunda **Bildirimler**’i seçip sağlama işlemini izleyin. Dağıtımın başarıyla tamamlanması birkaç dakika sürebilir ancak beklemeden bir sonraki adıma geçebilirsiniz.

    ![Bildirimler](media/ingest-data-event-hub/notifications.png)

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde hedef tablo oluşturma

Şimdi Azure Veri Gezgini'nde Event Hubs tarafından gönderilen verilerin ekleneceği tabloyu oluşturacaksınız. Tabloyu **Önkoşullar** bölümünde sağlanan kümede ve veritabanında oluşturacaksınız.

1. Azure portalda kümenize gidip **Sorgu**'yu seçin.

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-hub/query-explorer-link.png)

1. Aşağıdaki komutu pencereye kopyalayın ve yutulan verileri alacak tabloyu (TestTable) oluşturmak için **Çalıştır'ı** seçin.

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```

    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-event-hub/run-create-query.png)

1. Aşağıdaki komutu pencereye kopyalayın ve gelen JSON verilerini tablonun sütun adları ve veri türleri (TestTable) ile eşlemek için **Çalıştır'ı** seçin.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.timeStamp","datatype":"datetime"},{"column":"Name","path":"$.name","datatype":"string"},{"column":"Metric","path":"$.metric","datatype":"int"},{"column":"Source","path":"$.source","datatype":"string"}]'
    ```

## <a name="connect-to-the-event-hub"></a>Olay hub'ına bağlanma

Şimdi Azure Veri Gezgini'nden olay hub'ına bağlanabilirsiniz. Bağlantı kurulduğunda olay hub'ına giden veriler bu makalede oluşturduğunuz test tablosuna iletilir.

1. Olay hub'ı dağıtımının başarılı olduğundan emin olmak için araç çubuğunda **Bildirimler**'i seçin.

1. Oluşturduğunuz kümenin altında **Veritabanları**'nı ve ardından **TestDatabase** girişini seçin.

    ![Test veritabanını seçme](media/ingest-data-event-hub/select-test-database.png)

1. **Veri alımını** seçin ve **veri bağlantısı ekleyin.** Daha sonra formu aşağıdaki bilgilerle doldurun. Bittiğinde **Oluştur'u** seçin.

    ![Olay hub'ı bağlantısı](media/ingest-data-event-hub/event-hub-connection.png)

    **Veri Kaynağı:**

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *test-hub-connection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | Olay hub’ı ad alanı | Benzersiz bir ad alanı adı | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | Olay hub'ı | *test-hub* | Oluşturduğunuz olay hub'ı. |
    | Tüketici grubu | *test-group* | Oluşturduğunuz olay hub'ında tanımlanan tüketici grubu. |
    | Olay sistemi özellikleri | İlgili özellikleri seçme | [Olay Hub sistemi özellikleri.](/azure/service-bus-messaging/service-bus-amqp-protocol-guide#message-annotations) Olay iletisi başına birden çok kayıt varsa, sistem özellikleri ilkine eklenir. Sistem özellikleri eklerken, seçili özellikleri içerecek şekilde tablo şeması ve [eşleme](/azure/kusto/management/mappings) [oluşturun](/azure/kusto/management/create-table-command) veya [güncelleştirin.](/azure/kusto/management/alter-table-command) |
    | Sıkıştırma | *Yok* | Olay Hub iletileri yükünün sıkıştırma türü. Desteklenen sıkıştırma türleri: *Yok, GZip*.|
    | | |

    **Hedef tablo:**

    Yutulan verileri yönlendirmek için iki seçenek vardır: *statik* ve *dinamik.* 
    Bu makale için, tablo adını, veri biçimini ve eşlemi belirttiğiniz statik yönlendirmeyi kullanırsınız. Bu nedenle, verilerimi terk etmek, yönlendirme bilgilerini seçilmeden **içerir.**

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *TestTable* | **TestDatabase** içinde oluşturduğunuz tablo. |
    | Veri biçimi | *Json* | Desteklenen formatlar Avro, CSV, JSON, MULTILINE JSON, PSV, SOHSV, SCSV, TSV, TSVE, TXT, ORC ve PARKE'dir. |
    | Sütun eşleme | *TestMapping* | **TestDatabase'de**oluşturduğunuz ve gelen JSON verilerini **TestTable'ın**sütun adlarına ve veri türlerine eşleyen [eşleme.](/azure/kusto/management/mappings) JSON veya MULTILINE JSON için gereklidir ve diğer biçimler için isteğe bağlıdır.|
    | | |

    > [!NOTE]
    > * **Verilerim'i** seçin, verilerinizin [örnek uygulama](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) yorumlarında görüldüğü gibi gerekli yönlendirme bilgilerini içerdiği dinamik yönlendirme bilgilerini kullanır. Hem statik hem de dinamik özellikler ayarlanmışsa, dinamik özellikler statik özellikleri geçersiz kılar. 
    > * Yalnızca veri bağlantısını oluşturduktan sonra sıraya giren olaylar yutulür.
    > * Sıkıştırma türünü [örnek uygulamada](https://github.com/Azure-Samples/event-hubs-dotnet-ingest)görüldüğü gibi dinamik özelliklerle de ayarlayabilirsiniz.
    > * Avro, ORC ve PARKE formatları ile etkinlik sistemi özellikleri GZip sıkıştırma yükünde desteklenmez.

[!INCLUDE [data-explorer-container-system-properties](../../includes/data-explorer-container-system-properties.md)]

## <a name="copy-the-connection-string"></a>Bağlantı dizesini kopyalayın

Önkoşullarda listelenen [örnek uygulamayı](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) çalıştırdığınızda olay hub'ı ad alanının bağlantı dizesine ihtiyacınız olacaktır.

1. Oluşturduğunuz olay hub'ı ad alanında **Paylaşılan erişim ilkeleri**'ni ve ardından **RootManageSharedAccessKey** girişini seçin.

    ![Paylaşılan erişim ilkeleri](media/ingest-data-event-hub/shared-access-policies.png)

1. **Bağlantı dizekopya - birincil anahtar**. Bir sonraki bölümde bunu yapıştıracaksınız.

    ![Bağlantı dizesi](media/ingest-data-event-hub/connection-string.png)

## <a name="generate-sample-data"></a>Örnek veri oluşturma

Veri oluşturmak için indirdiğiniz [örnek uygulamayı](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) kullanın.

1. Örnek uygulama çözümünü Visual Studio'da açın.

1. *program.cs* dosyasında `connectionString` sabitini kopyaladığınız olay hub'ı ad alanı bağlantı dizesi ile güncelleştirin.

    ```csharp
    const string eventHubName = "test-hub";
    // Copy the connection string ("Connection string-primary key") from your Event Hub namespace.
    const string connectionString = @"<YourConnectionString>";
    ```

1. Uygulamayı derleyin ve çalıştırın. Uygulama olay hub'ına ileti gönderir ve o da on saniyede bir durumu yazdırır.

1. Uygulama birkaç ileti gönderdikten sonra bir sonraki adıma geçerek olay hub'ına ve test tablosuna veri akışını inceleyin.

## <a name="review-the-data-flow"></a>Veri akışını inceleme

Uygulama tarafından oluşturulan verilerin olay hub'ından kümenizdeki tabloya iletildiğini görebilirsiniz.

1. Uygulama çalışırken Azure portalda olay hub'ınızın etkinliğinin zirve yaptığını görürsünüz.

    ![Olay hub'ı grafiği](media/ingest-data-event-hub/event-hub-graph.png)

1. Veritabanına ulaşan ileti sayısını denetlemek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

    ```Kusto
    TestTable
    | count
    ```

1. İletilerin içeriğini görmek için aşağıdaki sorguyu çalıştırın:

    ```Kusto
    TestTable
    ```

    Sonuç kümesi aşağıdaki gibi görünmelidir:

    ![İleti sonuç kümesi](media/ingest-data-event-hub/message-result-set.png)

    > [!NOTE]
    > * Azure Veri Gezgini'nin, veri alımı için bir toplama (toplu laştırma) ilkesi vardır ve bu politika, alma işlemini optimize etmek için tasarlanmıştır. İlke varsayılan olarak 5 dakika veya 500 MB veriye yapılandırılmıştır, böylece bir gecikme deneyimi yaşayabilirsiniz. Toplama seçenekleri için [toplu laştırma ilkesine](/azure/kusto/concepts/batchingpolicy) bakın. 
    > * Olay Hub'ı alımı, 10 saniye veya 1 MB'lık Olay Hub yanıt süresini içerir. 
    > * Tablonuzu akışı destekleyecek şekilde yapılandırın ve yanıt süresindeki gecikmeyi kaldırın. [Akış ilkesine](/azure/kusto/concepts/streamingingestionpolicy)bakın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Olay hub'ınızı daha sonra kullanmayı planlamıyorsanız ek maliyet oluşmasını önlemek için **test-hub-rg** adlı kaynak grubunu kaldırın.

1. Azure portalında, en solda bulunan **Kaynak grupları**’nı ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Soldaki menü daraltılmışsa, genişletmek için ![Genişletme düğmesi](media/ingest-data-event-hub/expand.png) öğesine tıklayın.

   ![Silinecek kaynak grubunu seçin](media/ingest-data-event-hub/delete-resources-select.png)

1. **test-resource-group** altında **Kaynak grubunu sil**'i seçin.

1. Yeni pencerede silinecek kaynak grubunun adını yazın (*test-hub-rg*) ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini'nde verileri sorgula](web-query-data.md)
