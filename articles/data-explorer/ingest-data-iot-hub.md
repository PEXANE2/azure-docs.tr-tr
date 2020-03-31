---
title: IoT Hub'dan Azure Veri Gezgini'ne veri alma
description: Bu makalede, IoT Hub'dan Azure Veri Gezgini'ne nasıl veri öttürecek (yüklenir) öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 78455c90bab694b77a5e4a56d0b40518867d8d8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77188358"
---
# <a name="ingest-data-from-iot-hub-into-azure-data-explorer"></a>IoT Hub'dan Azure Veri Gezgini'ne veri alma 

> [!div class="op_single_selector"]
> * [Portal](ingest-data-iot-hub.md)
> * [C #](data-connection-iot-hub-csharp.md)
> * [Python](data-connection-iot-hub-python.md)
> * [Azure Resource Manager şablonu](data-connection-iot-hub-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, büyük bir veri akışı platformu ve IoT alma hizmeti olan IoT Hub'dan alım (veri yükleme) sunar.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Veritabanı adı *testdb*ile [bir test kümesi ve veritabanı](create-cluster-database-portal.md) oluşturun.
* [Bir](https://github.com/Azure-Samples/azure-iot-samples-csharp) aygıtı taklit etmek için örnek bir uygulama ve dokümantasyon.
* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) örnek uygulamayı çalıştırmak için.

## <a name="create-an-iot-hub"></a>Iot Hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device-to-the-iot-hub"></a>Aygıtı IoT Hub'ına kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde hedef tablo oluşturma

Artık Azure Veri Gezgini'nde IoT Hub'larının veri göndereceği bir tablo oluşturun. Tabloyu kümede ve [**Önkoşullar'da**](#prerequisites)verilen veritabanında oluşturursunuz.

1. Azure portalında kümenize gidin ve **Sorgula'yı**seçin.

    ![Portalda ADX sorgusu](media/ingest-data-iot-hub/adx-initiate-query.png)

1. Aşağıdaki komutu pencereye kopyalayın ve yutulan verileri alacak tabloyu (TestTable) oluşturmak için **Çalıştır'ı** seçin.

    ```Kusto
    .create table TestTable (temperature: real, humidity: real)
    ```
    
    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-iot-hub/run-create-query.png)

1. Aşağıdaki komutu pencereye kopyalayın ve gelen JSON verilerini tablonun sütun adları ve veri türleri (TestTable) ile eşlemek için **Çalıştır'ı** seçin.

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"humidity","path":"$.humidity","datatype":"real"},{"column":"temperature","path":"$.temperature","datatype":"real"}]'
    ```

## <a name="connect-azure-data-explorer-table-to-iot-hub"></a>Azure Veri Gezgini tablosunu IoT hub'ına bağlama

Artık Azure Veri Gezgini'nden IoT Hub'ına bağlanın. Bu bağlantı tamamlandığında, iot hub'ına akan veriler [oluşturduğunuz hedef tabloya](#create-a-target-table-in-azure-data-explorer)akar.

1. IoT Hub dağıtımının başarılı olduğunu doğrulamak için araç çubuğundaki **Bildirimler'i** seçin.

1. Oluşturduğunuz kümenin **altında, Veritabanları'nı** seçin ve ardından **testdb**oluşturduğunuz veritabanını seçin.
    
    ![Test veritabanını seçme](media/ingest-data-iot-hub/select-database.png)

1. **Veri alımını** seçin ve **veri bağlantısı ekleyin.** Daha sonra formu aşağıdaki bilgilerle doldurun. Bittiğinde **Oluştur'u** seçin.

    ![IoT Hub bağlantısı](media/ingest-data-iot-hub/iot-hub-connection.png)

    **Veri Kaynağı**:

    **Ayar** | **Alan açıklaması**
    |---|---|
    | Veri bağlantısı adı | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı
    | IoT Hub | IoT Hub adı |
    | Paylaşılan erişim ilkesi | Paylaşılan erişim ilkesinin adı. İzinleri okumuş olmalı |
    | Tüketici grubu |  IoT Hub'Da tanımlanan tüketici grubu yerleşik bitiş noktası |
    | Olay sistemi özellikleri | [IoT Hub olay sistemi özellikleri.](/azure/iot-hub/iot-hub-devguide-messages-construct#system-properties-of-d2c-iot-hub-messages) Sistem özellikleri eklerken, seçili özellikleri içerecek şekilde tablo şeması ve [eşleme](/azure/kusto/management/mappings) [oluşturun](/azure/kusto/management/create-table-command) veya [güncelleştirin.](/azure/kusto/management/alter-table-command) | | | 

    > [!NOTE]
    > El ile [başarısız](/azure/iot-hub/iot-hub-ha-dr#manual-failover)olması durumunda, veri bağlantısını yeniden oluşturmanız gerekir.

    **Hedef tablo**:

    Yutulan verileri yönlendirmek için iki seçenek vardır: *statik* ve *dinamik.* 
    Bu makale için, tablo adını, veri biçimini ve eşlemi belirttiğiniz statik yönlendirmeyi kullanırsınız. Bu nedenle, verilerimi terk etmek, yönlendirme bilgilerini seçilmeden **içerir.**

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *TestTable* | **Testdb'de**oluşturduğunuz tablo. |
    | Veri biçimi | *Json* | Desteklenen formatlar Avro, CSV, JSON, MULTILINE JSON, PSV, SOHSV, SCSV, TSV, TSVE ve TXT'dir. |
    | Sütun eşleme | *TestMapping* | **Testdb'de**oluşturduğunuz [haritalama,](/azure/kusto/management/mappings) gelen JSON verilerini **testdb'nin**sütun adlarına ve veri türlerine eşleyen. JSON, MULTILINE JSON ve AVRO için gereklidir ve diğer biçimler için isteğe bağlıdır.|
    | | |

    > [!NOTE]
    > * **Verilerim'i** seçin, verilerinizin [örnek uygulama](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) yorumlarında görüldüğü gibi gerekli yönlendirme bilgilerini içerdiği dinamik yönlendirme bilgilerini kullanır. Hem statik hem de dinamik özellikler ayarlanmışsa, dinamik özellikler statik özellikleri geçersiz kılar. 
    > * Yalnızca veri bağlantısını oluşturduktan sonra sıraya giren olaylar yutulür.

[!INCLUDE [data-explorer-container-system-properties](../../includes/data-explorer-container-system-properties.md)]

## <a name="generate-sample-data-for-testing"></a>Test için örnek veri oluşturma

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisi gönderir.

1. https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip adresinden örnek C# projesini indirin ve ZIP arşivini ayıklayın.

1. Yerel terminal penceresinde, örnek C# projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device** klasörüne gidin.

1. **SimulatedDevice.cs** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `s_connectionString` Değişkenin [değerini, bir aygıtı Kaydedin'den IoT Hub'ına](#register-a-device-to-the-iot-hub)kaydeden aygıt bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.cs** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli paketleri yükleyin:

    ```cmd/sh
    dotnet restore
    ```

1. Yerel terminal penceresinde, aşağıdaki komutu çalıştırarak simülasyon cihazı uygulamasını derleyip çalıştırın:

    ```cmd/sh
    dotnet run
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/ingest-data-iot-hub/simulated-device.png)

## <a name="review-the-data-flow"></a>Veri akışını inceleme

Uygulama veri oluştururken, artık IoT hub'ından kümenizdeki tabloya veri akışını görebilirsiniz.

1. Azure portalında, IoT hub'ınızın altında, uygulama çalışırken etkinlikteki ani artışı görürsünüz.

    ![IoT Hub ölçümleri](media/ingest-data-iot-hub/iot-hub-metrics.png)

1. Veritabanına ulaşan ileti sayısını denetlemek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

    ```Kusto
    TestTable
    | count
    ```

1. İletilerin içeriğini görmek için aşağıdaki sorguyu çalıştırın:

    ```Kusto
    TestTable
    ```

    Sonuç kümesi:
    
    ![Yutulan veri sonuçlarını gösterme](media/ingest-data-iot-hub/show-ingested-data.png)

    > [!NOTE]
    > * Azure Veri Gezgini'nin, veri alımı için bir toplama (toplu laştırma) ilkesi vardır ve bu politika, alma işlemini optimize etmek için tasarlanmıştır. İlke varsayılan olarak 5 dakika veya 500 MB veriye yapılandırılmıştır, böylece bir gecikme deneyimi yaşayabilirsiniz. Toplama seçenekleri için [toplu laştırma ilkesine](/azure/kusto/concepts/batchingpolicy) bakın. 
    > * Tablonuzu akışı destekleyecek şekilde yapılandırın ve yanıt süresindeki gecikmeyi kaldırın. [Akış ilkesine](/azure/kusto/concepts/streamingingestionpolicy)bakın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Hub'ınızı tekrar kullanmayı düşünmüyorsanız, tahakkuk eden maliyetleri önlemek için **test-hub-rg'yi**temizleyin.

1. Azure portalında, en solda bulunan **Kaynak grupları**’nı ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Soldaki menü daraltılmışsa, genişletmek için ![Genişletme düğmesi](media/ingest-data-event-hub/expand.png) öğesine tıklayın.

   ![Silinecek kaynak grubunu seçin](media/ingest-data-event-hub/delete-resources-select.png)

1. **test-resource-group** altında **Kaynak grubunu sil**'i seçin.

1. Yeni pencerede silinecek kaynak grubunun adını yazın (*test-hub-rg*) ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini'nde verileri sorgula](web-query-data.md)
