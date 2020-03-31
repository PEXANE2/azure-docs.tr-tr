---
title: Azure Veri Fabrikası'nda Azure Veri Gezgini denetim komutlarını kullanma
description: Bu konuda, Azure Veri Fabrikası'nda Azure Veri Gezgini denetim komutlarını kullanın
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/15/2019
ms.openlocfilehash: 20da2d54ea54674656b2c1006d094c63133baf79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72264496"
---
# <a name="use-azure-data-factory-command-activity-to-run-azure-data-explorer-control-commands"></a>Azure Veri Gezgini denetim komutlarını çalıştırmak için Azure Veri Fabrikası komut etkinliğini kullanma

[Azure Veri Fabrikası](/azure/data-factory/) (ADF), veriler üzerinde bir dizi etkinlik gerçekleştirmenize olanak tanıyan bulut tabanlı bir veri tümleştirme hizmetidir. Veri hareketini ve veri dönüşümlerini düzenlemek ve otomatikleştirmek için veri tabanlı iş akışları oluşturmak için ADF'yi kullanın. Azure Veri Fabrikası'ndaki **Azure Veri Gezgini Komutu** etkinliği, Bir ADF iş akışı içinde [Azure Veri Gezgini denetim komutlarını](/azure/kusto/concepts/#control-commands) çalıştırmanızı sağlar. Bu makalede, bir arama etkinliği ve Bir Azure Veri Gezgini komut etkinliği içeren ForEach etkinliği içeren bir ardışık yapı oluşturma yı öğretir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)
* Bir veri kaynağı.
* [Bir veri fabrikası](data-factory-load-data.md#create-a-data-factory)

## <a name="create-a-new-pipeline"></a>Yeni bir ardışık hat lar oluşturma

1. **Yazar** kalem aracını seçin. 
1. Açılan kaynaktan **+** **Pipeline'ı** seçip seçerek yeni bir ardışık kaynak oluştur.

   ![yeni boru hattı oluşturmak](media/data-factory-command-activity/create-pipeline.png)

## <a name="create-a-lookup-activity"></a>Arama etkinliği oluşturma

Bir [arama etkinliği,](/azure/data-factory/control-flow-lookup-activity) Azure Veri Fabrikası tarafından desteklenen herhangi bir veri kaynağından bir veri kümesi alabilir. Arama etkinliğinden çıktı bir ForEach veya başka bir etkinlikte kullanılabilir.

1. **Etkinlikler** bölmesinde, **Genel** **altında, Arama** etkinliğini seçin. Sürükleyin ve sağdaki ana tuval içine bırakın.
 
    ![arama etkinliğini seçin](media/data-factory-command-activity/select-activity.png)

1. Tuval şimdi oluşturduğunuz Arama etkinliğini içerir. İlgili parametreleri değiştirmek için tuvalin altındaki sekmeleri kullanın. **Genel**olarak, etkinliği yeniden adlandırın. 

    ![arama etkinliğini edin](media/data-factory-command-activity/edit-lookup-activity.PNG)

    > [!TIP]
    > Boru hattı özelliklerini görüntülemek için boş tuval alanına tıklayın. Ardışık mayısu yeniden adlandırmak için **Genel** sekmesini kullanın. Boru hattımızın adı *boru hattı-4-docs.*

### <a name="create-an-azure-data-explorer-dataset-in-lookup-activity"></a>Arama etkinliğinde Azure Veri Gezgini veri kümesi oluşturma

1. Ayarlar'da, önceden oluşturulmuş Azure Veri Gezgini **Kaynak veri setinizi**seçin veya yeni bir veri kümesi oluşturmak için **+ Yeni'yi** seçin. **Settings**
 
    ![arama ayarlarına veri kümesi ekleme](media/data-factory-command-activity/lookup-settings.png)

1. **Yeni Veri Kümesi** penceresinden Azure Veri **Gezgini (Kusto)** veri kümesini seçin. Yeni veri kümesini eklemek için **Devam** et'i seçin.

   ![yeni veri kümesini seçin](media/data-factory-command-activity/select-new-dataset.png) 

1. Yeni Azure Veri Gezgini veri kümesi parametreleri **Ayarlar'da**görülebilir. Parametreleri güncelleştirmek için **Edit'i**seçin.

    ![Azure Veri Gezgini veri seti ile arama ayarları](media/data-factory-command-activity/lookup-settings-with-adx-dataset.png)

1. **AzureDataExplorerTable** yeni sekmesi ana tuvalde açılır. 
    * **Genel'i** seçin ve veri kümesi adını ayarlayın. 
    * Veri kümesi özelliklerini ayarlamak için **Bağlantı'yı** seçin. 
    * Açılan hizmetten **Bağlantılı hizmeti** seçin veya yeni bir bağlantılı hizmet oluşturmak için **+ Yeni'yi** seçin.

    ![Azure Veri Gezgini veri kümesi özelliklerini ayarlama](media/data-factory-command-activity/adx-dataset-properties-edit-connections.png)

1. Yeni bağlantılı bir hizmet oluştururken, **Yeni Bağlantılı Hizmet (Azure Veri Gezgini)** sayfası açılır:

    ![ADX yeni bağlantılı hizmet](media/data-factory-command-activity/adx-new-linked-service.png)

   * Azure Veri Gezgini bağlantılı hizmet için **Ad'ı** seçin. Gerekirse **Açıklama** ekleyin.
   * **Tümleştirme çalışma süresi yle**Bağlan'da, gerekirse geçerli ayarları değiştirin. 
   * **Hesap seçiminde iki** yöntemden birini kullanarak kümenizi seçin: 
        * Azure **abonelik** radyosundan seçin ve **Azure abonelik** hesabınızı seçin. Ardından **Kümenizi**seçin. Açılan listenin yalnızca kullanıcıya ait kümeleri listeleeceğini unutmayın.
        * Bunun yerine, **el ile** radyo yu gir düğmesini seçin ve **Bitiş Noktanızı** girin (küme URL'nizi).
    * **Kiracıyı**belirtin.
    * **Hizmet ana kimliğini**girin. Asıl kimliğin, kullanılan komutun gerektirdiği izin düzeyine göre yeterli izinlere sahip olması gerekir.
    * **Servis ana anahtarı nı** seçin ve Servis Ana **anahtarını**girin.
    * Açılan menüden **Veritabanınızı** seçin. Alternatif olarak, Denetim Kutusunu **Edit'i** seçin ve veritabanı adınızı girin.
    * Oluşturduğunuz bağlantılı servis bağlantısını sınamak için **Test Bağlantısı'nı** seçin. Kurulumunuza bağlanabilirseniz, yeşil bir onay işareti **Bağlantısı başarılı** görünür.
    * Bağlantılı hizmet oluşturmayı tamamlamak için **Finish'i** seçin.

1. **AzureDataExplorerTable** > **Bağlantısı'nda**bağlantılı bir hizmet ayarladıktan sonra **Tablo** adı ekleyin. Verilerin düzgün sunulduğundan emin olmak için **Verileri Önizleme'yi**seçin.

   Veri kümeniz artık hazır ve ardışık alanınızı düzenlemeye devam edebilirsiniz.

### <a name="add-a-query-to-your-lookup-activity"></a>Arama etkinliğinize sorgu ekleme

1. **Pipeline-4-docs** > **Ayarlar'da** **Sorgu** metin kutusuna örneğin bir sorgu ekleyin:

    ```kusto
    ClusterQueries
    | where Database !in ("KustoMonitoringPersistentDatabase", "$systemdb")
    | summarize count() by Database
    ```

1. Sorgu **zaman arasını** veya **Hiçbir kesintive** **İlk satır yalnızca** özelliklerini gerektiği gibi değiştirin. Bu akışta, varsayılan **Sorgu zaman ını** tutar ve onay kutularının denetimini geri alıyoruz. 

    ![Arama etkinliğinin son ayarları](media/data-factory-command-activity/lookup-activity-final-settings.png)

## <a name="create-a-for-each-activity"></a>Herkes İçin etkinlik oluşturma 

[For-Each](/azure/data-factory/control-flow-for-each-activity) etkinliği, bir koleksiyon üzerinde yinelemek ve belirtilen etkinlikleri bir döngü içinde yürütmek için kullanılır. 

1. Şimdi boru hattına her biri için bir etkinlik ekleyin. Bu etkinlik, Arama etkinliğinden döndürülen verileri işler. 
    * **Etkinlikler** bölmesinde, **Yineleme & Koşullular**altında, **ForEach** etkinliğini seçin ve sürükleyip tuvale bırakın.
    * Arama etkinliğinin çıktısı ile bunları bağlamak için tuvaldeki ForEach etkinliğinin girişi arasında bir çizgi çizin.

        ![ForEach etkinliği](media/data-factory-command-activity/for-each-activity.png)

1.  Tuvaldeki ForEach etkinliğini seçin. Aşağıdaki **Ayarlar** sekmesinde:
    * Arama sonuçlarının sıralı bir işleme için **Sıralı** onay kutusunu işaretleyin veya paralel işleme oluşturmak için işaretsiz bırakın.
    * **Toplu Iş sayısını**ayarla.
    * **Maddelerde,** çıktı değerine aşağıdaki başvuruyu sağlayın: * @activity('Arama1').output.value*

       ![ForEach etkinliği ayarları](media/data-factory-command-activity/for-each-activity-settings.png)

## <a name="create-an-azure-data-explorer-command-activity-within-the-foreach-activity"></a>ForEach etkinliği içinde bir Azure Veri Gezgini Komutu etkinliği oluşturun

1. ForEach içindeki etkinlikleri belirtmek için yeni bir tuvalde açmak için tuvaldeki ForEach etkinliğini çift tıklatın.
1. **Etkinlikler** bölmesinde, **Azure Veri Gezgini**altında, **Azure Veri Gezgini Komutu etkinliğini** seçin ve sürükleyip tuvale bırakın.

    ![Azure Veri Gezgini komut etkinliği](media/data-factory-command-activity/adx-command-activity.png)

1.  **Bağlantı** sekmesinde, daha önce oluşturulmuş aynı Bağlantılı Hizmeti seçin.

    ![azure veri gezgini komutu etkinlik bağlantısı sekmesi](media/data-factory-command-activity/adx-command-activity-connection-tab.png)

1. **Komut** sekmesinde aşağıdaki komutu sağlayın:

    ```kusto
    .export
    async compressed
    into csv h"http://<storageName>.blob.core.windows.net/data/ClusterQueries;<storageKey>" with (
    sizeLimit=100000,
    namePrefix=export
    )
    <| ClusterQueries | where Database == "@{item().Database}"
    ```

    **Komut,** Azure Veri Gezgini'ne belirli bir sorgunun sonuçlarını sıkıştırılmış bir biçimde bir blob depolama alanına dışa aktarmasını bildirir. Eşsenkronize çalışır (async değiştirici kullanarak).
    Sorgu, Arama etkinliği sonucundaki her satırın veritabanı sütununa gider. **Komut zaman ayarı** değişmeden bırakılabilir.

    ![komut etkinliği](media/data-factory-command-activity/command.png)   

    > [!NOTE]
    > Komut etkinliği aşağıdaki sınırlara sahiptir:
    > * Boyut sınırı: 1 MB yanıt boyutu
    > * Zaman sınırı: 20 dakika (varsayılan), 1 saat (maksimum).
    > * Gerekirse, ortaya çıkan boyutu/zamanı azaltmak için [AdminThenQuery'yi](/azure/kusto/management/index#combining-queries-and-control-commands)kullanarak sonuca bir sorgu ekleyebilirsiniz.

1.  Boru hattı hazır. Ardışık hatlar adını tıklatarak ana ardışık görünüme geri dönebilirsiniz.

    ![Azure Veri Gezgini komut ardışık](media/data-factory-command-activity/adx-command-pipeline.png)

1. Ardışık bölümü yayımlamadan önce **Hata Ayıklama'yı** seçin. Ardışık hat ilerlemesi **Çıktı** sekmesinde izlenebilir.

    ![azure veri gezgini komutu etkinlik çıktısı](media/data-factory-command-activity/command-activity-output.png)

1. **Tümlerini Yayımla** ve ardından ardışık hattı çalıştırmak için **tetikleyici ekleyebilirsiniz.** 

## <a name="control-command-outputs"></a>Denetim komutu çıktıları

Komut etkinlik çıktısının yapısı aşağıda ayrıntılı olarak açıklanmıştır. Bu çıktı, ardışık ardışık işlemdeki bir sonraki etkinlik tarafından kullanılabilir.

### <a name="returned-value-of-a-non-async-control-command"></a>Async olmayan bir denetim komutunun döndürülen değeri

Eşit olmayan bir denetim komutunda, döndürülen değerin yapısı Arama etkinliği sonucunun yapısına benzer. Alan `count` döndürülen kayıt sayısını gösterir. Sabit bir `value` dizi alanı, bir kayıt listesi içerir. 

```json
{ 
    "count": "2", 
    "value": [ 
        { 
            "ExtentId": "1b9977fe-e6cf-4cda-84f3-4a7c61f28ecd", 
            "ExtentSize": 1214.0, 
            "CompressedSize": 520.0 
        }, 
        { 
            "ExtentId": "b897f5a3-62b0-441d-95ca-bf7a88952974", 
            "ExtentSize": 1114.0, 
            "CompressedSize": 504.0 
        } 
    ] 
} 
```
 
### <a name="returned-value-of-an-async-control-command"></a>Async denetim komutunun döndürülen değeri

Bir async denetim komutunda, etkinlik, async işlemi tamamlanana veya zaman dışarı çıkana kadar işlem tablosunu arka planda yoklar. Bu nedenle, döndürülen değer, `.show operations OperationId` verilen **OperationId** özelliği için sonucu içerecektir. İşlemin başarılı bir şekilde tamamlanıp tamamlanınmasını doğrulamak için **Durum** ve **Durum** özelliklerinin değerlerini denetleyin.

```json
{ 
    "count": "1", 
    "value": [ 
        { 
            "OperationId": "910deeae-dd79-44a4-a3a2-087a90d4bb42", 
            "Operation": "TableSetOrAppend", 
            "NodeId": "", 
            "StartedOn": "2019-06-23T10:12:44.0371419Z", 
            "LastUpdatedOn": "2019-06-23T10:12:46.7871468Z", 
            "Duration": "00:00:02.7500049", 
            "State": "Completed", 
            "Status": "", 
            "RootActivityId": "f7c5aaaf-197b-4593-8ba0-e864c94c3c6f", 
            "ShouldRetry": false, 
            "Database": "MyDatabase", 
            "Principal": "<some principal id>", 
            "User": "<some User id>" 
        } 
    ] 
}
``` 

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veri [Fabrikası'nı kullanarak verileri Azure Veri Gezgini'ne](data-factory-load-data.md)kopyalama hakkında bilgi edinin.
* [Veritabanından Azure Veri Gezgini'ne toplu kopya için Azure Veri Fabrikası şablonu](data-factory-template.md)kullanma hakkında bilgi edinin.
