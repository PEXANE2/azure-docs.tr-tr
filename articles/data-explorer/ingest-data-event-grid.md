---
title: Azure Veri Gezgini'ne Azure Blob'ları Alma
description: Bu makalede, Olay Izgara aboneliğini kullanarak depolama hesabı verilerini Azure Veri Gezgini'ne nasıl göndereceğinizi öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ec218b1638183db463ff09488c988cad64d78c6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370449"
---
# <a name="ingest-blobs-into-azure-data-explorer-by-subscribing-to-event-grid-notifications"></a>Olay Ağı bildirimlerine abone olarak Azure Veri Gezgini'ne blobs alma

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-grid.md)
> * [C #](data-connection-event-grid-csharp.md)
> * [Python](data-connection-event-grid-python.md)
> * [Azure Resource Manager şablonu](data-connection-event-grid-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve ölçeklenebilir bir veri arama hizmetidir. Blob kaplarına yazılmış lekelerden sürekli yutma (veri yüklemesi) sunar. 

Bu makalede, bir [Azure Olay Ağı](/azure/event-grid/overview) aboneliğini nasıl ayarlayabileceğinizi ve olayları bir etkinlik merkezi aracılığıyla Azure Veri Gezgini'ne nasıl yönlendirdiğinizi öğreneceksiniz. Başlamak için, Azure Etkinlik Hub'larına bildirim gönderen bir olay ızgarası aboneliğiolan bir depolama hesabınız olmalıdır. Ardından bir Olay Izgara veri bağlantısı oluşturur ve sistem deki veri akışını görürsünüz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/)oluşturun.
* [Bir küme ve veritabanı.](create-cluster-database-portal.md)
* [Bir depolama hesabı.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)
* [Bir olay hub'ı.](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)

## <a name="create-an-event-grid-subscription-in-your-storage-account"></a>Depolama hesabınızda Olay Ağı aboneliği oluşturma

1. Azure portalında depolama hesabınızı bulun.
1. Etkinlikler**Etkinlik Aboneliği'ni** **Events** > seçin.

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-grid/create-event-grid-subscription.png)

1. **Temel** sekmesindeki **Olay Aboneliği Oluştur** penceresinde aşağıdaki değerleri sağlayın:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Adı | *test-grid-bağlantısı* | Oluşturmak istediğiniz olay ızgarasının adı.|
    | Olay Şeması | *Olay Kılavuz şeması* | Olay ızgarası için kullanılması gereken şema. |
    | Konu Türü | *Depolama hesabı* | Olay ızgarası konusunun türü. |
    | Konu Kaynağı | *gridteststorage* | Depolama hesabınızın adı. |
    | Tüm etkinlik türlerine abone olun | *Temizleyin* | Tüm olaylarhakkında bilgi almayın. |
    | Tanımlanan Olay Türleri | *Oluşturulan blob* | Hangi özel olaylar için haberdar almak için. |
    | Uç Nokta Türü | *Olay hub’ları* | Olayları gönderdiğiniz bitiş noktası türü. |
    | Uç Nokta | *test-hub* | Oluşturduğunuz olay hub'ı. |
    | | |

1. Belirli bir kapsayıcıdan dosyaları izlemek istiyorsanız **Filtreler** sekmesini seçin. Bildirimlerin filtrelerini aşağıdaki gibi ayarlayın:
    * **Konu Alan ile başlar** blob *konteynerin in gerçek* önekidir. Uygulanan desen *ile başlar*gibi, birden fazla kapsayıcı yayılabilir. Joker karakterlere izin verilmez.
     Aşağıdaki gibi *ayarlanmalıdır:* *`/blobServices/default/containers/`*[kapsayıcı öneki]
    * Konu Biter Alan **blob** *gerçek* sonek. Joker karakterlere izin verilmez.

## <a name="create-a-target-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde hedef tablo oluşturma

Etkinlik Hub'larının veri göndereceği Azure Veri Gezgini'nde bir tablo oluşturun. Önkoşullarda hazırlanan küme ve veritabanında tablo oluşturun.

1. Azure portalda kümenizin altında **Sorgu**'yu seçin.

    ![Sorgu uygulama bağlantısı](media/ingest-data-event-grid/query-explorer-link.png)

1. Aşağıdaki komutu pencereye kopyalayın ve yutulan verileri alacak tabloyu (TestTable) oluşturmak için **Çalıştır'ı** seçin.

    ```kusto
    .create table TestTable (TimeStamp: datetime, Value: string, Source:string)
    ```

    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-event-grid/run-create-table.png)

1. Aşağıdaki komutu pencereye kopyalayın ve gelen JSON verilerini tablonun sütun adları ve veri türleri (TestTable) ile eşlemek için **Çalıştır'ı** seçin.

    ```kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.TimeStamp"},{"column":"Value","path":"$.Value"},{"column":"Source","path":"$.Source"}]'
    ```

## <a name="create-an-event-grid-data-connection-in-azure-data-explorer"></a>Azure Veri Gezgini'nde Olay Ağı veri bağlantısı oluşturma

Artık Azure Veri Gezgini'nden Olay Izgarasına bağlanın, böylece blob kapsayıcısına akan veriler test tablosuna aktarılır. 

1. Olay hub'ı dağıtımının başarılı olduğundan emin olmak için araç çubuğunda **Bildirimler**'i seçin.

1. Oluşturduğunuz kümenin **altında, Veritabanları** > **TestDatabase'i**seçin.

    ![Test veritabanını seçme](media/ingest-data-event-grid/select-test-database.png)

1. **Veri alımını** > seçin**Veri bağlantısı ekleyin.**

    ![Veri alımı](media/ingest-data-event-grid/data-ingestion-create.png)

1.  Bağlantı türünü seçin: **Blob Depolama**.

1. Formu aşağıdaki bilgilerle doldurun ve **Oluştur'u**seçin.

    ![Olay hub'ı bağlantısı](media/ingest-data-event-grid/create-event-grid-data-connection.png)

     Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *test-hub-connection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | Depolama hesabı aboneliği | Abonelik kimliğiniz | Depolama hesabınızın bulunduğu abonelik kimliği.|
    | Depolama hesabı | *gridteststorage* | Daha önce oluşturduğunuz depolama hesabının adı.|
    | Event Grid | *test-grid-bağlantısı* | Oluşturduğunuz olay ızgarasının adı. |
    | Olay Hub'ı adı | *test-hub* | Oluşturduğunuz olay hub'ı. Bir olay ızgarası seçtiğinizde bu alan otomatik olarak doldurulur. |
    | Tüketici grubu | *test-group* | Oluşturduğunuz olay merkezinde tanımlanan tüketici grubu. |
    | | |

    Hedef tablo:

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *TestTable* | **TestDatabase** içinde oluşturduğunuz tablo. |
    | Veri biçimi | *Json* | Desteklenen formatlar Avro, CSV, JSON, MULTILINE JSON, PSV, SOH, SCSV, TSV, RAW ve TXT'dir. Desteklenen sıkıştırma seçenekleri: Zip ve GZip |
    | Sütun eşleme | *TestMapping* | **TestDatabase** içinde oluşturduğunuz ve gelen JSON verilerini **TestTable** tablosunun sütun adları ve veri türleriyle eşleyen eşleme.|
    | | |
    
## <a name="generate-sample-data"></a>Örnek veri oluşturma

Azure Veri Gezgini ve depolama hesabı bağlı olduğuna göre, örnek veriler oluşturabilir ve blob depolama alanına yükleyebilirsiniz.

Azure Depolama kaynaklarıyla etkileşimde kalmak için birkaç temel Azure CLI komutu veren küçük bir kabuk komutdosyasıyla çalışırız. Bu komut dosyası, depolama hesabınızda yeni bir kapsayıcı oluşturur, varolan bir dosyayı (blob olarak) bu kapsayıcıya yükler ve sonra kapsayıcıdaki lekeleri listeler. Komut dosyasını doğrudan portalda yürütmek için [Azure Cloud Shell'i](https://docs.microsoft.com/azure/cloud-shell/overview) kullanabilirsiniz.

Verileri bir dosyaya kaydedin ve bu komut dosyasıyla yükleyin:

```json
{"TimeStamp": "1987-11-16 12:00","Value": "Hello World","Source": "TestSource"}
```

```azurecli
#!/bin/bash
### A simple Azure Storage example script

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export file_to_upload=<file_to_upload>
    export destination_file=<destination_file>

    echo "Creating the container..."
    az storage container create --name $container_name

    echo "Uploading the file..."
    az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name --metadata "rawSizeBytes=1024"

    echo "Listing the blobs..."
    az storage blob list --container-name $container_name --output table

    echo "Done"
```

> [!NOTE]
> En iyi yutma performansını elde etmek için, yutma için gönderilen sıkıştırılmış lekelerin *sıkıştırılmamış* boyutu iletilmelidir. Olay Izgara bildirimleri yalnızca temel ayrıntıları içerdiğinden, boyut bilgilerinin açıkça iletilmesi gerekir. Sıkıştırılmamış boyut bilgileri, `rawSizeBytes` blob meta verilerindeki özelliği baytlarda *sıkıştırılmamış* veri boyutuyla ayarlayarak sağlanabilir.

### <a name="ingestion-properties"></a>Yutma özellikleri

Blob meta verileri ile blob yutma Yutma [özelliklerini](https://docs.microsoft.com/azure/kusto/management/data-ingestion/#ingestion-properties) belirtebilirsiniz.

Bu özellikler ayarlanabilir:

|**Özellik** | **Özellik açıklaması**|
|---|---|
| `rawSizeBytes` | Ham (sıkıştırılmamış) verilerin boyutu. Avro/ORC/Parke için bu, biçime özel sıkıştırma uygulanmadan önceki boyutdur.|
| `kustoTable` |  Varolan hedef tablonun adı. `Data Connection` Bıçağın `Table` üzerindeki seti geçersiz kılar. |
| `kustoDataFormat` |  Veri biçimi. `Data Connection` Bıçağın `Data format` üzerindeki seti geçersiz kılar. |
| `kustoIngestionMappingReference` |  Kullanılacak mevcut yutma eşlemesi adı. `Data Connection` Bıçağın `Column mapping` üzerindeki seti geçersiz kılar.|
| `kustoIgnoreFirstRecord` | `true`Ayarlanırsa, Kusto blob ilk satırı yok sayar. Üstbilgileri yok saymak için tabular biçimli verileri (CSV, TSV veya benzeri) kullanın. |
| `kustoExtentTags` | Ortaya çıkan ölçüde eklenecek [etiketleri](/azure/kusto/management/extents-overview#extent-tagging) temsil eden dize. |
| `kustoCreationTime` |  ISO 8601 dizesi olarak biçimlendirilmiş blob için [$IngestionTime](/azure/kusto/query/ingestiontimefunction?pivots=azuredataexplorer) geçersiz kılar. Dolum için kullanın. |

> [!NOTE]
> Azure Veri Gezgini, posta daki lekeleri silmez.
> Lekeleri beş güne kadar koruyun.
> Blob silme işlemini yönetmek için [Azure Blob depolama yaşam döngüsünü](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal) kullanın. 

## <a name="review-the-data-flow"></a>Veri akışını inceleme

> [!NOTE]
> Azure Veri Gezgini'nin, veri alım işlemini en iyi duruma getirmek için tasarlanmış bir toplama (toplu laştırma) ilkesi vardır.
Varsayılan olarak, ilke 5 dakika ya göre yapılandırılır.
Gerekirse daha sonra poliçeyi değiştirebilirsiniz. Bu makalede, birkaç dakikalık bir gecikme bekleyebilirsiniz.

1. Azure portalında, etkinlik ızgaranızın altında, uygulama çalışırken etkinlikteki ani artışı görürsünüz.

    ![Olay ızgara grafiği](media/ingest-data-event-grid/event-grid-graph.png)

1. Veritabanına ulaşan ileti sayısını denetlemek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

    ```kusto
    TestTable
    | count
    ```

1. İletilerin içeriğini görmek için test veritabanınızda aşağıdaki sorguyu çalıştırın.

    ```kusto
    TestTable
    ```

    Sonuç kümesi aşağıdakine benzer olmalıdır.

    ![İleti sonuç kümesi](media/ingest-data-event-grid/table-result.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Olay ızgaranızı tekrar kullanmayı planlamamanız durumunda, tahakkuk eden maliyetleri önlemek için **test-hub-rg'yi**temizleyin.

1. Azure portalında, en solda bulunan **Kaynak grupları**’nı ve ardından oluşturduğunuz kaynak grubunu seçin.  

    Soldaki menü daraltılmışsa, genişletmek için ![Genişletme düğmesi](media/ingest-data-event-grid/expand.png) öğesine tıklayın.

   ![Silinecek kaynak grubunu seçin](media/ingest-data-event-grid/delete-resources-select.png)

1. **test-resource-group** altında **Kaynak grubunu sil**'i seçin.

1. Yeni pencerede,*(test-hub-rg)* silmek için kaynak grubunun adını girin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini'nde verileri sorgula](web-query-data.md)
