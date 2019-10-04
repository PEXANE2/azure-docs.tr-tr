---
title: Olay Hub 'ından Azure Veri Gezgini veri alma
description: Bu makalede, Olay Hub 'ından Azure Veri Gezgini veri alma (yükleme) hakkında bilgi edineceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/17/2019
ms.openlocfilehash: a83e2163c9aa970932f2eea8e2e04a715107ac7f
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71950265"
---
# <a name="ingest-data-from-event-hub-into-azure-data-explorer"></a>Olay Hub 'ından Azure Veri Gezgini veri alma

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir bir veri araştırma hizmetidir. Azure Veri Gezgini, büyük bir veri akışı platformu ve olay alma hizmeti olan Event Hubs alma (veri yükleme) sağlar. [Event Hubs](/azure/event-hubs/event-hubs-about) , yaklaşık gerçek zamanlı olarak saniyede milyonlarca olayı işleyebilir. Bu makalede, bir olay hub 'ı oluşturur, Azure Veri Gezgini 'a bağlanırsınız ve sistem aracılığıyla veri akışını görürsünüz.

## <a name="prerequisites"></a>Prerequisites

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

* [Bir test kümesi ve veritabanı](create-cluster-database-portal.md).

* Veri üreten ve onu bir olay hub 'ına Gönderen [örnek bir uygulama](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) . Örnek uygulamayı sisteminize indirin.

* Örnek uygulamayı çalıştırmak için [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) .

## <a name="sign-in-to-the-azure-portal"></a>Azure portal oturum açın

[Azure Portal](https://portal.azure.com/)oturum açın.

## <a name="create-an-event-hub"></a>Olay Hub 'ı oluşturma

Bu makalede, örnek veri oluşturur ve bir olay hub 'ına gönderebilirsiniz. İlk adım bir olay hub 'ı oluşturmaktır. Bunu, Azure portal bir Azure Resource Manager şablonu kullanarak yapabilirsiniz.

1. Bir olay hub 'ı oluşturmak için, dağıtımı başlatmak üzere aşağıdaki düğmeyi kullanın. Sağ tıklayıp **Yeni pencerede aç**' ı seçerek bu makaledeki adımların geri kalanını izleyebilirsiniz.

    [![Azure 'a dağıtın](media/ingest-data-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

    **Azure 'A dağıt** düğmesi, bir dağıtım formunu doldurmanız için sizi Azure Portal götürür.

    ![Azure’a dağıtma](media/ingest-data-event-hub/deploy-to-azure.png)

1. Olay Hub 'ını oluşturmak istediğiniz aboneliği seçin ve *Test-Hub-RG*adlı bir kaynak grubu oluşturun.

    ![Kaynak grubu oluşturma](media/ingest-data-event-hub/create-resource-group.png)

1. Aşağıdaki bilgilerle formu doldurun.

    ![Dağıtım formu](media/ingest-data-event-hub/deployment-form.png)

    Aşağıdaki tabloda listelenmeyen tüm ayarlar için varsayılan değerleri kullanın.

    **Ayarlanmasını** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Aboneliğiniz | Aboneliğiniz | Olay Hub 'ınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *Test-Hub-RG* | Yeni bir kaynak grubu oluşturun. |
    | Konum | *Batı ABD* | Bu makale için *Batı ABD* seçin. Bir üretim sistemi için gereksinimlerinizi en iyi karşılayan bölgeyi seçin. En iyi performansı elde etmek için kusto kümesiyle aynı konumda Olay Hub 'ı ad alanı oluşturun (yüksek aktarım hızı olan olay hub 'ı ad alanları için en önemli).
    | Ad alanı adı | Benzersiz bir ad alanı adı | Ad alanınızı tanımlayan benzersiz bir ad seçin. Örneğin, *mytestnamespace*. *ServiceBus.Windows.net* etki alanı adı, sağladığınız ada eklenir. Ad yalnızca harf, sayı ve kısa çizgi içerebilir. Ad bir harfle başlamalı ve bir harf veya rakam ile bitmelidir. Değer 6 ila 50 karakter uzunluğunda olmalıdır.
    | Olay Hub 'ı adı | *Test-Hub* | Olay Hub 'ı, benzersiz bir kapsam kapsayıcısı sağlayan ad alanı altında bulunur. Olay Hub 'ı adı ad alanı içinde benzersiz olmalıdır. |
    | Tüketici grubu adı | *test-Grup* | Tüketici grupları, birden çok tüketen uygulamanın her birinin olay akışının ayrı bir görünümüne sahip olmasını sağlar. |
    | | |

1. Aboneliğinizde kaynak oluşturduğunuz anlamına gelen **satın alma**' yı seçin.

1. Hazırlama işlemini izlemek için araç çubuğunda **Bildirimler** ' i seçin. Dağıtımın başarılı olması birkaç dakika sürebilir, ancak şimdi bir sonraki adıma geçebilirsiniz.

    ![Bildirimler](media/ingest-data-event-hub/notifications.png)

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini hedef tablo oluşturma

Artık Azure Veri Gezgini 'da Event Hubs veri gönderecek bir tablo oluşturursunuz. Tabloyu, **ön**koşullarda sağlanan kümede ve veritabanında oluşturursunuz.

1. Azure portal kümenize gelin ve ardından **sorgu**' yı seçin.

    ![Sorgu uygulaması bağlantısı](media/ingest-data-event-hub/query-explorer-link.png)

1. Aşağıdaki komutu pencereye kopyalayın ve alınan verileri alacak tabloyu (TestTable) oluşturmak için **Çalıştır** ' ı seçin.

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```

    ![Oluşturma sorgusu Çalıştır](media/ingest-data-event-hub/run-create-query.png)

1. Aşağıdaki komutu pencereye kopyalayın ve gelen JSON verilerini tablonun sütun adlarıyla ve veri türleriyle (TestTable) eşlemek için **Çalıştır** ' ı seçin.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.timeStamp","datatype":"datetime"},{"column":"Name","path":"$.name","datatype":"string"},{"column":"Metric","path":"$.metric","datatype":"int"},{"column":"Source","path":"$.source","datatype":"string"}]'
    ```

## <a name="connect-to-the-event-hub"></a>Olay Hub 'ına bağlanma

Artık Azure Veri Gezgini Olay Hub 'ına bağlanıyorsunuz. Bu bağlantı olduğunda, Olay Hub 'ına akan veriler, bu makalede daha önce oluşturduğunuz test tablosuna akar.

1. Olay Hub 'ının dağıtımının başarılı olduğunu doğrulamak için araç çubuğunda **Bildirimler** ' i seçin.

1. Oluşturduğunuz küme altında **veritabanları** ' nı ve ardından **TestDatabase**' i seçin.

    ![Test veritabanını seçin](media/ingest-data-event-hub/select-test-database.png)

1. **Veri** alımı ' nı seçin ve **veri bağlantısı ekleyin**. Ardından, aşağıdaki bilgilerle formu doldurun. İşiniz bittiğinde **Oluştur** ' u seçin.

    ![Olay Hub 'ı bağlantısı](media/ingest-data-event-hub/event-hub-connection.png)

    Veri kaynağı:

    **Ayarlanmasını** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *Test-Hub-bağlantı* | Azure Veri Gezgini içinde oluşturmak istediğiniz bağlantının adı.|
    | Olay Hub 'ı ad alanı | Benzersiz bir ad alanı adı | Daha önce seçtiğiniz ad alanınızı tanımlayan ad. |
    | Olay Hub 'ı | *Test-Hub* | Oluşturduğunuz Olay Hub 'ı. |
    | Tüketici grubu | *test-Grup* | Oluşturduğunuz Olay Hub 'ında tanımlanan Tüketici grubu. |
    | Olay sistemi özellikleri | İlgili özellikleri seçin | [Olay Hub 'ı sistem özellikleri](/azure/service-bus-messaging/service-bus-amqp-protocol-guide#message-annotations) |
    | | |

    Hedef tablo:

    Alınan verileri yönlendirmeye yönelik iki seçenek vardır: *statik* ve *dinamik*. 
    Bu makalede, tablo adını, veri biçimini ve eşlemeyi belirttiğiniz statik yönlendirme kullanırsınız. Bu nedenle, **verilerim yönlendirme bilgilerini içeren verileri** işaretsiz olarak bırak.

     **Ayarlanmasını** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *TestTable* | **TestDatabase**'te oluşturduğunuz tablo. |
    | Veri biçimi | *NESNESINDE* | Desteklenen biçimler şunlardır avro, CSV, JSON, çok SATıRLı JSON, PSV, SOHSV, SCSV, TSV, TSVE ve TXT. Desteklenen sıkıştırma seçenekleri: GZip |
    | Sütun eşleme | *TestMapping* | **TestDatabase**'te oluşturduğunuz ve gelen JSON verilerini **TestTable**'ın sütun adlarıyla ve veri türleriyle eşleyen eşleme. JSON, çok SATıRLı JSON veya AVRO için gereklidir ve diğer biçimler için isteğe bağlıdır.|
    | | |

    > [!NOTE]
    > * Verilerim ' i seçin, verilerin [örnek uygulama](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) açıklamalarında görüldüğü gibi gerekli yönlendirme bilgilerini içerdiği dinamik yönlendirmeyi kullanmak için **yönlendirme bilgilerini içerir** . Hem statik hem de dinamik özellikler ayarlandıysa, dinamik özellikler statik olanları geçersiz kılar. 
    > * Yalnızca veri bağlantısını oluşturduktan sonra sıraya alınan olaylar alınır.

## <a name="copy-the-connection-string"></a>Bağlantı dizesini Kopyala

Önkoşul bölümünde listelenen [Örnek uygulamayı](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) çalıştırdığınızda, Olay Hub 'ı ad alanı için bağlantı dizesine ihtiyacınız vardır.

1. Oluşturduğunuz Olay Hub 'ı ad alanı altında, **paylaşılan erişim ilkeleri**' ni ve ardından **RootManageSharedAccessKey**' ı seçin.

    ![Paylaşılan erişim ilkeleri](media/ingest-data-event-hub/shared-access-policies.png)

1. **Bağlantı dizesini Kopyala-birincil anahtar**. Bunu bir sonraki bölüme yapıştırmanız gerekir.

    ![Bağlantı dizesi](media/ingest-data-event-hub/connection-string.png)

## <a name="generate-sample-data"></a>Örnek veri üretme

Veri oluşturmak için indirdiğiniz [Örnek uygulamayı](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) kullanın.

1. Visual Studio 'da örnek uygulama çözümünü açın.

1. *Program.cs* dosyasında `connectionString` sabitini, Olay Hub 'ı ad alanından kopyaladığınız bağlantı dizesiyle güncelleştirin.

    ```csharp
    const string eventHubName = "test-hub";
    // Copy the connection string ("Connection string-primary key") from your Event Hub namespace.
    const string connectionString = @"<YourConnectionString>";
    ```

1. Uygulamayı derleyin ve çalıştırın. Uygulama, iletileri olay hub 'ına gönderir ve her on saniyede bir durum yazdırır.

1. Uygulama birkaç ileti gönderdikten sonra, bir sonraki adıma geçin: Olay Hub 'ınıza ve test tablonuza veri akışını gözden geçirme.

## <a name="review-the-data-flow"></a>Veri akışını gözden geçirme

Uygulama verileri ürettiğinde, artık bu verilerin Olay Hub 'ından kümenizdeki tabloya akışını görebilirsiniz.

1. Azure portal, Olay Hub 'ınız altında, uygulama çalışırken ani artış ' u görürsünüz.

    ![Olay Hub 'ı grafiği](media/ingest-data-event-hub/event-hub-graph.png)

1. Şimdiye kadar kaç tane ileti yapıldığını denetlemek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

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
    > * Azure Veri Gezgini, alım işlemini iyileştirmek için tasarlanan veri alımı için toplama (toplu işlem) ilkesine sahiptir. İlke, varsayılan olarak 5 dakika veya 500 MB veri olarak yapılandırılır, bu nedenle bir gecikmeyle karşılaşabilirsiniz. Toplama seçenekleri için bkz. [toplu işlem ilkesi](/azure/kusto/concepts/batchingpolicy) . 
    > * Olay Hub 'ı alımı, 10 saniyelik veya 1 MB Olay Hub 'ı yanıt süresini içerir. 
    > * Tablonuzu, akışı destekleyecek şekilde yapılandırın ve yanıt süresi içinde gecikme süresini kaldırın. [Akış ilkesi](/azure/kusto/concepts/streamingingestionpolicy)bölümüne bakın. 

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Olay Hub 'ınızı yeniden kullanmayı planlamıyorsanız, maliyetleri ortadan kaldırmak için **Test-Hub-RG**'yi temizleyin.

1. Azure portal, en solda bulunan **kaynak grupları** ' nı seçin ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Sol menü daraltılmışsa, şunu seçin ![Genişlet düğmesi](media/ingest-data-event-hub/expand.png) genişletin.

   ![Silinecek kaynak grubunu seçin](media/ingest-data-event-hub/delete-resources-select.png)

1. **Test-Resource-Group**altında, **kaynak grubunu sil**' i seçin.

1. Yeni pencerede, silinecek kaynak grubunun adını yazın (*Test-Hub-RG*) ve ardından **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini 'de verileri sorgulama](web-query-data.md)
