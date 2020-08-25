---
title: Mevcut verileri Azure Cosmos DB Tablo API'si hesaba geçirin
description: Şirket içi veya bulut verilerini Azure Cosmos DB Azure Tablo API'si hesabına geçirme veya içeri aktarma hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 12/07/2017
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 0023308c74d58b1c94bf13fcb47ffb8aa7ade1d6
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "85557629"
---
# <a name="migrate-your-data-to-azure-cosmos-db-table-api-account"></a>Verilerinizi Azure Cosmos DB Tablo API'si hesabına geçirme

Bu öğreticide, Azure Cosmos DB [tablo API'si](table-introduction.md)kullanım için verileri içeri aktarmaya yönelik yönergeler sağlanmaktadır. Azure Tablo depolama alanında depolanan verileriniz varsa verilerinizi Azure Cosmos DB Tablo API'sine aktarmak için Veri Taşıma Aracı'nı veya AzCopy'yi kullanabilirsiniz. Bir Azure Cosmos DB Tablo API'si (önizleme) hesabında depolanan verileriniz varsa, verilerinizi taşımak için Veri Taşıma Aracı'nı kullanmanız gerekir. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Veri Taşıma Aracı ile veri içeri aktarma
> * AzCopy ile veri içeri aktarma
> * Tablo API’sinden (önizleme) Tablo API’sine geçiş 

## <a name="prerequisites"></a>Ön koşullar

* **Aktarım hızını artırma:** Veri geçişinizin süresi, tek bir kapsayıcı veya bir kapsayıcı kümesi için ayarladığınız aktarım hızı miktarına bağlıdır. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Azure portalda aktarım hızını artırma hakkında daha fazla bilgi için bkz. Azure Cosmos DB’de performans düzeyleri ve fiyatlandırma katmanları.

* **Azure Cosmos DB kaynaklarını oluşturma:** Veri geçişini başlatmadan önce Azure portaldan tüm tablolarınızı oluşturun. Veritabanı düzeyinde aktarım hızına sahip olan bir Azure Cosmos DB hesabına geçiş yapıyorsanız Azure Cosmos DB tablolarını oluştururken bölüm anahtarı girdiğinizden emin olun.

## <a name="data-migration-tool"></a>Veri Taşıma aracı

Komut satırı Azure Cosmos DB Veri Taşıma Aracı (dt.exe), var olan Azure Table depolama verilerinizi bir Tablo API GA hesabına içeri aktarmak veya verileri bir Tablo API (önizleme) hesabından bir Tablo API GA hesabına taşımak için kullanılabilir. Diğer kaynaklar şu anda desteklenmemektedir. Kullanıcı arabirimi temelli Veri Taşıma Aracı (dtui.exe), Tablo API hesapları için şu anda desteklenmemektedir. 

Tablo verilerinin geçişini gerçekleştirmek için aşağıdaki görevleri tamamlayın:

1. Geçiş aracını [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool)'dan indirin.
2. `dt.exe` aracını, senaryonuzun komut satırı bağımsız değişkenlerini kullanarak çalıştırın. `dt.exe` aşağıdaki biçimde bir komut alır:

   ```bash
    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 
   ```

Bu komut için desteklenen seçenekler şunlardır:

* **/Errorlog:** Seçim. Veri aktarımı hatalarının yeniden yönlendirileceği CSV dosyasının adı
* **/Overwritehata günlüğü:** Seçim. Hata günlüğü dosyasının üzerine yaz
* **/ProgressUpdateInterval:** İsteğe bağlı, varsayılan değer 00:00:01 ' dir. Ekran üzerinde veri aktarımı ilerlemesini yenileme zaman aralığı
* **/ErrorDetails:** İsteğe bağlı, varsayılan değer None ' dır. Aşağıdaki hatalar için ayrıntılı hata bilgilerinin görüntüleneceğini belirtir: None, Critical, All
* **/Enablecosmostablelog:** Seçim. Günlüğü bir Cosmos tablo hesabına yönlendirin. Ayarlanırsa,/CosmosTableLogConnectionString sağlanmamışsa bu varsayılan hedef hesap bağlantı dizesine ayarlanır. Bu, aynı anda birden çok DT örneği çalıştırılmakta olduğunda yararlıdır.
* **/Cosmostablelogconnectionstring:** Seçim. Günlüğü uzak Cosmos tablo hesabına yönlendirmek için ConnectionString.

### <a name="command-line-source-settings"></a>Komut satırı kaynak ayarları

Azure Tablo Depolama veya Tablo API önizlemesini geçişin kaynağı olarak tanımlarken aşağıdaki kaynak seçeneklerini kullanın.

* **/s: AzureTable:** Azure Tablo depolamadan verileri okur
* **/S.ConnectionString:** Tablo uç noktası için bağlantı dizesi. Bu, Azure portal alınabilir
* **/S.locationmode:** İsteğe bağlı, varsayılan yalnızca Primarydır. Azure Tablo depolama alanına bağlanılırken kullanılacak konum modunu belirtir: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, Secondarythenprımary
* **/S.Table:** Azure tablosunun adı
* **/S.InternalFields:** İçeri aktarma için RowKey ve PartitionKey gerekli olduğundan tablo geçişi için tümüne ayarlayın.
* **/S.Filter:** Seçim. Uygulanacak filtre dizesi
* **/S.Projection:** Seçim. Seçilecek sütunların listesi

Azure Tablo depolamadan içeri aktarırken kaynak bağlantı dizesini almak için, Azure Portal açın ve **depolama hesapları**  >  **Hesap**  >  **erişim anahtarları**' na tıklayın ve sonra **bağlantı dizesini**kopyalamak için Kopyala düğmesini kullanın.

:::image type="content" source="./media/table-import/storage-table-access-key.png" alt-text="HBase kaynağı seçeneklerinin ekran görüntüsü":::

Azure Cosmos db tablo API'si (Önizleme) hesabından içeri aktarırken kaynak bağlantı dizesini almak için, Azure Portal açın, **Azure Cosmos DB**  >  **hesabı**  >  **bağlantı dizesi** ' ne tıklayın ve Kopyala düğmesini kullanarak **bağlantı dizesini**kopyalayın.

:::image type="content" source="./media/table-import/cosmos-connection-string.png" alt-text="HBase kaynağı seçeneklerinin ekran görüntüsü":::

[Azure Tablo Depolama komutu örneği](#azure-table-storage)

[Azure Cosmos DB Tablo API’si (önizleme) komutu örneği](#table-api-preview)

### <a name="command-line-target-settings"></a>Komut satırı hedefi ayarları

Azure Cosmos DB Tablo API'sini geçiş hedefi olarak tanımlarken aşağıdaki hedef seçeneklerini kullanın.

* **/t: Tabloapitoplu:** Toplu iş içindeki verileri Azure CosmosDB tablosuna yükler
* **/T.exe:** Tablo uç noktası için bağlantı dizesi
* **/T/TableName:** Yazılacak tablonun adını belirtir
* **/T.exe üzerine yaz:** İsteğe bağlı, varsayılan değer false 'dur. Varolan değerlerin üzerine yazılıp yazılmayacağını belirtir
* **/T.exe:** İsteğe bağlı, varsayılan değer 1 ' dir. Verileri havuza reçeteye göre kapatmadan önce arabelleğe girilen giriş baytlarının yaklaşık tahmini tahmini
* **/T/aktarım hızı:** İsteğe bağlı, hizmet varsayılan olarak belirlenir. Tablo için yapılandırılacak aktarım hızını belirtir
* **/T3batchsize:** İsteğe bağlı, varsayılan olarak 2MB ' dir. Toplu iş boyutunu bayt cinsinden belirtin

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Örnek komut: Kaynak Azure Table depolaması

Azure Tablo depolamasından Tablo API'sine içeri aktarmayı gösteren bir komut satırı örneği:

```bash
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Örnek komut: Kaynak Azure Cosmos DB Tablo API (önizleme)

Tablo API önizleme sürümünden Table API GA'ya içeri aktarma için bir komut satırı örneği:

```bash
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="migrate-data-by-using-azcopy"></a>AzCopy'yi kullanarak veri geçirme

Azure Tablo depolamasından Azure Cosmos DB Tablo API'sine veri geçirmek için AzCopy komut satırı yardımcı programı da kullanılabilir. AzCopy'yi kullanmak için önce verilerinizi [Tablo deposundan veri dışarı aktarma](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#export-data-from-table-storage)'da açıklandığı gibi dışarı aktarır, sonra bu verileri [Azure Cosmos DB Tablo API](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#import-data-into-table-storage)'de açıklandığı gibi Azure Cosmos DB'ye içeri aktarırsınız.

Azure Cosmos DB'ye içeri aktarmayı gerçekleştirirken aşağıdaki örneğe bakın. /Dest değerinin core değil cosmosdb kullandığını unutmayın.

Örnek içeri aktarma komutu:

```bash
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>Tablo API’sinden (önizleme) Tablo API’sine geçiş

> [!WARNING]
> Genel olarak kullanılabilen tabloların avantajlarından hemen yararlanmak istiyorsanız, lütfen var olan önizleme tablolarınızı bu bölümde belirtildiği şekilde geçirin. Aksi halde mevcut önizleme müşterileri için önümüzdeki haftalarda otomatik taşımalar gerçekleştireceğiz, ancak otomatik taşınan önizleme tablolarında yeni oluşturulan tablolarda olmayan bazı kısıtlamalar olacak.

Tablo API genel kullanıma (GA) sunulmuştur. Tabloların önizleme ve GA sürümleri arasında gerek bulutta çalışan kodda, gerek istemcide çalışan kodda farklar vardır. Bu nedenle bir önizleme SDK istemcisini bir GA Tablo API hesabıyla karıştırmayı veya bunun aksini denemek önerilmez. Mevcut tablolarını kullanmaya devam etmek isteyen ancak bir üretim ortamında önizleme GA ortamına geçmesi gereken Tablo API önizleme müşterileri otomatik geçişi beklemelidir. Otomatik geçişi beklerseniz, taşınan tablolardaki kısıtlamalar size bildirilir. Geçişten sonra mevcut hesabınızda kısıtlamaları olmayan yeni tablolar oluşturabilirsiniz (yalnızca geçirilen tabloların kısıtlamaları olacaktır).

Tablo API önizleme sürümünden genel olarak kullanılabilen Tablo API'sine geçiş için:

1. Yeni bir Azure Cosmos DB hesabı oluşturun ve API türünü [Veritabanı hesabı oluşturma](create-table-dotnet.md#create-a-database-account)'da açıklandığı gibi Azure Tablosu olarak ayarlayın.

2. İstemcileri [Tablo API SDK'larının](table-sdk-dotnet.md) GA sürümünü kullanacak şekilde değiştirin.

3. Veri Taşıma Aracı'nı kullanarak istemci veritabanını önizleme tablolarından GA tablolarına geçirin. Veri taşıma aracının bu amaçla kullanım yönergeleri [Veri Taşıma Aracı](#data-migration-tool)'nda açıklanmaktadır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Veri Taşıma Aracı ile veri içeri aktarma
> * AzCopy ile veril içeri aktarma
> * Tablo API’sinden (önizleme) Tablo API’sine geçiş

Şimdi sonraki öğreticiye devam edebilir ve Azure Cosmos DB Tablo API’sini kullanarak veri sorgulamayı öğrenebilirsiniz. 

> [!div class="nextstepaction"]
>[Veriler nasıl sorgulanır?](../cosmos-db/tutorial-query-table.md)
