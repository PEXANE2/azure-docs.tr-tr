---
title: IoT Hub verileri Azure 'a alma Veri Gezgini
description: Bu makalede, IoT Hub Azure Veri Gezgini 'a veri alma (yükleme) hakkında bilgi edineceksiniz.
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/27/2019
ms.openlocfilehash: 327fd5352a3f067638c7f9ceb51e2de9e284d845
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71947841"
---
# <a name="ingest-data-from-iot-hub-into-azure-data-explorer-preview"></a>IoT Hub verileri Azure Veri Gezgini 'a alma (Önizleme)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir bir veri araştırma hizmetidir. Azure Veri Gezgini, büyük bir veri akışı platformu ve IoT alma hizmeti olan IoT Hub alma (veri yükleme) sağlar.

## <a name="prerequisites"></a>Prerequisites

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

* *TestDB*veritabanı adıyla [bir test kümesi ve veritabanı](create-cluster-database-portal.md) oluşturun.

* Bir cihazın benzetimini yapmak için [örnek bir uygulama](https://github.com/Azure-Samples/azure-iot-samples-csharp) ve belgeler.

* Örnek uygulamayı çalıştırmak için [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) .

## <a name="create-an-iot-hub"></a>IoT Hub 'ı oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device-to-the-iot-hub"></a>IoT Hub bir cihazı kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini hedef tablo oluşturma

Artık Azure Veri Gezgini 'da IoT Hub 'Larının veri göndereceği bir tablo oluşturursunuz. Tabloyu, [**ön**](#prerequisites)koşullarda sağlanan kümede ve veritabanında oluşturursunuz.

1. Azure portal kümenize gidin ve **sorgu**' yı seçin.

    ![Portalda ADX sorgusu](media/ingest-data-iot-hub/adx-initiate-query.png)

1. Aşağıdaki komutu pencereye kopyalayın ve alınan verileri alacak tabloyu (TestTable) oluşturmak için **Çalıştır** ' ı seçin.

    ```Kusto
    .create table TestTable (temperature: real, humidity: real)
    ```
    
    ![Oluşturma sorgusu Çalıştır](media/ingest-data-iot-hub/run-create-query.png)

1. Aşağıdaki komutu pencereye kopyalayın ve gelen JSON verilerini tablonun sütun adlarıyla ve veri türleriyle (TestTable) eşlemek için **Çalıştır** ' ı seçin.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"humidity","path":"$.humidity","datatype":"real"},{"column":"temperature","path":"$.temperature","datatype":"real"}]'
    ```

## <a name="connect-azure-data-explorer-table-to-iot-hub"></a>Azure Veri Gezgini tablosunu IoT Hub 'a bağlama

Artık Azure Veri Gezgini IoT Hub bağlanıyorsunuz. Bu bağlantı tamamlandığında, IoT Hub 'ına akan veriler [oluşturduğunuz hedef tabloya](#create-a-target-table-in-azure-data-explorer)akar.

1. IoT Hub dağıtımının başarılı olduğunu doğrulamak için araç çubuğunda **Bildirimler** ' i seçin.

1. Oluşturduğunuz küme altında **veritabanları** ' nı seçin ve ardından **TestDB**oluşturduğunuz veritabanını seçin.
    
    ![Test veritabanını seçin](media/ingest-data-iot-hub/select-database.png)

1. **Veri** alımı ' nı seçin ve **veri bağlantısı ekleyin**. Ardından, aşağıdaki bilgilerle formu doldurun. İşiniz bittiğinde **Oluştur** ' u seçin.

    ![IoT Hub bağlantısı](media/ingest-data-iot-hub/iot-hub-connection.png)

    **Veri kaynağı**:

    **Ayarlanmasını** | **Alan açıklaması**
    |---|---|
    | Veri bağlantısı adı | Azure Veri Gezgini oluşturmak istediğiniz bağlantının adı
    | IoT Hub | IoT Hub adı |
    | Paylaşılan erişim ilkesi | Paylaşılan erişim ilkesinin adı. Okuma izinlerine sahip olmalıdır |
    | Tüketici grubu |  IoT Hub yerleşik uç noktasında tanımlanan Tüketici grubu |
    | Olay sistemi özellikleri | Olay [sistemi özellikleri IoT Hub](/azure/iot-hub/iot-hub-devguide-messages-construct#system-properties-of-d2c-iot-hub-messages) olay iletisi başına birden çok kayıt varsa, sistem özellikleri ilk birine eklenir.|
    | | 

    > [!NOTE]
    > [El ile yük devretme](/azure/iot-hub/iot-hub-ha-dr#manual-failover)durumunda, veri bağlantısını yeniden oluşturmanız gerekir.

    **Hedef tablo**:

    Alınan verileri yönlendirmeye yönelik iki seçenek vardır: *statik* ve *dinamik*. 
    Bu makalede, tablo adını, veri biçimini ve eşlemeyi belirttiğiniz statik yönlendirme kullanırsınız. Bu nedenle, **verilerim yönlendirme bilgilerini içeren verileri** işaretsiz olarak bırak.

     **Ayarlanmasını** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *TestTable* | **TestDB**'de oluşturduğunuz tablo. |
    | Veri biçimi | *NESNESINDE* | Desteklenen biçimler şunlardır avro, CSV, JSON, çok SATıRLı JSON, PSV, SOHSV, SCSV, TSV, TSVE ve TXT. |
    | Sütun eşleme | *TestMapping* | **TestDB**'de oluşturduğunuz ve gelen JSON verilerini **TestDB**'nin sütun adlarıyla ve veri türleriyle eşleyen eşleme. JSON, çok SATıRLı JSON ve AVRO için gereklidir ve diğer biçimler için isteğe bağlıdır.|
    | | |

    > [!NOTE]
    > * Verilerim ' i seçin, verilerin [örnek uygulama](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) açıklamalarında görüldüğü gibi gerekli yönlendirme bilgilerini içerdiği dinamik yönlendirmeyi kullanmak için **yönlendirme bilgilerini içerir** . Hem statik hem de dinamik özellikler ayarlandıysa, dinamik özellikler statik olanları geçersiz kılar. 
    > * Yalnızca veri bağlantısını oluşturduktan sonra sıraya alınan olaylar alınır.

## <a name="generate-sample-data-for-testing"></a>Test için örnek veri oluşturma

Sanal cihaz uygulaması, IoT Hub 'ınızdaki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisini gönderir.

1. Örnek C# projeyi https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip ' den INDIRIN ve ZIP arşivini ayıklayın.

1. Yerel bir Terminal penceresinde, örnek C# projenin kök klasörüne gidin. Ardından **iot-hub\Quickstarts\simulated-Device** klasörüne gidin.

1. **SimulatedDevice.cs** dosyasını istediğiniz bir metin düzenleyicisinde açın.

    @No__t-0 değişkeninin değerini, cihaz bağlantı dizesiyle [bir cihazı IoT Hub kaydet](#register-a-device-to-the-iot-hub)öğesinden değiştirin. Sonra değişikliklerinizi **SimulatedDevice.cs** dosyasına kaydedin.

1. Yerel Terminal penceresinde, sanal cihaz uygulaması için gerekli paketleri yüklemek üzere aşağıdaki komutları çalıştırın:

    ```cmd/sh
    dotnet restore
    ```

1. Yerel Terminal penceresinde, sanal cihaz uygulamasını derlemek ve çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

    Aşağıdaki ekran görüntüsünde, sanal cihaz uygulamasının IoT Hub 'ınıza telemetri gönderdiği çıkış gösterilmektedir:

    ![Sanal cihazı Çalıştır](media/ingest-data-iot-hub/simulated-device.png)

## <a name="review-the-data-flow"></a>Veri akışını gözden geçirme

Uygulama verileri ürettiğinde, artık IoT Hub 'ından kümenizdeki tabloya veri akışını görebilirsiniz.

1. Azure portal, IoT Hub 'ınız altında, uygulama çalışırken ani bir etkinlik görürsünüz.

    ![IoT Hub ölçümleri](media/ingest-data-iot-hub/iot-hub-metrics.png)

1. Şimdiye kadar kaç tane ileti yapıldığını denetlemek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

    ```Kusto
    TestTable
    | count
    ```

1. İletilerin içeriğini görmek için aşağıdaki sorguyu çalıştırın:

    ```Kusto
    TestTable
    ```

    Sonuç kümesi:
    
    ![Alınan veri sonuçlarını göster](media/ingest-data-iot-hub/show-ingested-data.png)

    > [!NOTE]
    > * Azure Veri Gezgini, alım işlemini iyileştirmek için tasarlanan veri alımı için toplama (toplu işlem) ilkesine sahiptir. İlke, varsayılan olarak 5 dakika veya 500 MB veri olarak yapılandırılır, bu nedenle bir gecikmeyle karşılaşabilirsiniz. Toplama seçenekleri için bkz. [toplu işlem ilkesi](/azure/kusto/concepts/batchingpolicy) . 
    > * Tablonuzu, akışı destekleyecek şekilde yapılandırın ve yanıt süresi içinde gecikme süresini kaldırın. [Akış ilkesi](/azure/kusto/concepts/streamingingestionpolicy)bölümüne bakın. 

## <a name="clean-up-resources"></a>Kaynakları Temizleme

IoT Hub yeniden kullanmayı planlamıyorsanız, maliyetlerden kaçınmak için **Test-Hub-RG**'yi temizleyin.

1. Azure portal, en solda bulunan **kaynak grupları** ' nı seçin ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Sol menü daraltılmışsa, şunu seçin ![Genişlet düğmesi](media/ingest-data-event-hub/expand.png) genişletin.

   ![Silinecek kaynak grubunu seçin](media/ingest-data-event-hub/delete-resources-select.png)

1. **Test-Resource-Group**altında, **kaynak grubunu sil**' i seçin.

1. Yeni pencerede, silinecek kaynak grubunun adını yazın (*Test-Hub-RG*) ve ardından **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini 'de verileri sorgulama](web-query-data.md)
