---
title: Azure Veri Fabrikası ile Azure Veri Gezgini entegrasyonu
description: Bu konuda, kopyalama, arama ve komut etkinliklerini kullanmak için Azure Veri Gezgini'ni Azure Veri Fabrikası'na entegre edin
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tomersh26
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: bb08cf4db45a378b35a8245eadd56a2ab3e48bab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76293632"
---
# <a name="integrate-azure-data-explorer-with-azure-data-factory"></a>Azure Veri Gezgini'ni Azure Veri Fabrikasıile tümleştir

[Azure Veri Fabrikası](/azure/data-factory/) (ADF), farklı veri depolarını tümleştirmenize ve veriler üzerinde etkinlikler gerçekleştirmenize olanak tanıyan bulut tabanlı bir veri tümleştirme hizmetidir. ADF, veri hareketi ve veri dönüşümlerini düzenlemek ve otomatikleştirmek için veri tabanlı iş akışları oluşturmanıza olanak tanır. Azure Veri Gezgini, Azure Veri Fabrikası'ndaki [desteklenen veri depolarından](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) biridir. 

## <a name="azure-data-factory-activities-for-azure-data-explorer"></a>Azure Veri Gezgini için Azure Veri Fabrikası etkinlikleri

Azure Veri Fabrikası ile çeşitli tümleştirmeler Azure Veri Gezgini kullanıcıları için kullanılabilir:

### <a name="copy-activity"></a>Kopyalama etkinliği
 
Azure Veri Fabrikası Kopyalama etkinliği, veri depoları arasında veri aktarmak için kullanılır. Azure Veri Gezgini, verilerin Azure Veri Gezgini'nden desteklenen herhangi bir veri deposuna ve desteklenen herhangi bir veri deposundan Azure Veri Gezgini'ne kopyalandığı bir lavabo olarak desteklenir. Daha fazla bilgi için Azure [Veri Fabrikası'nı kullanarak Azure Veri Gezgini'ne veya Azure Veri Gezgini'nden kopya verilerine](/azure/data-factory/connector-azure-data-explorer)bakın. ve Azure [Veri Fabrikası'ndan Azure Veri Gezgini'ne yük verilerini](data-factory-load-data.md)ayrıntılı bir şekilde görmek için.
Azure Veri Gezgini, veriler Azure içinde kopyalandığında kullanılan Azure IR (Tümleştirme Çalışma Süresi) tarafından desteklenir ve verileri şirket içinde veya Azure Sanal Ağı gibi erişim denetimine sahip bir ağa şirket içinde veya ağda kopyalarken kendi kendine barındırılan IR tarafından desteklenir. Daha fazla bilgi [için hangi IR'yi kullanacağına](/azure/data-factory/concepts-integration-runtime#determining-which-ir-to-use) bakın
 
> [!TIP]
> Kopyalama etkinliğini kullanırken ve bir **Bağlantılı Hizmet** veya **Dataset**oluştururken, eski veri deposu **Kusto'yu**değil, veri deposu Azure Veri Gezgini'ni **(Kusto)** seçin.  

### <a name="lookup-activity"></a>Arama etkinliği
 
Arama etkinliği, Azure Veri Gezgini'ndeki sorguları yürütmek için kullanılır. Sorgunun sonucu, Arama etkinliğinin çıktısı olarak döndürülür ve [ADF Arama belgelerinde](/azure/data-factory/control-flow-lookup-activity#use-the-lookup-activity-result-in-a-subsequent-activity)açıklandığı gibi ardışık ardışık ardışık taki bir sonraki etkinlikte kullanılabilir.  
5.000 satır ve 2 MB'lık yanıt boyutu sınırına ek olarak, etkinlik aynı zamanda 1 saatlik bir sorgu zaman önceliği sınırına da sahiptir.

### <a name="command-activity"></a>Komut etkinliği

Komut etkinliği, Azure Veri Gezgini [denetim komutlarının](/azure/kusto/concepts/#control-commands)yürütülmesine izin verir. Sorguların aksine, denetim komutları verileri veya meta verileri değiştirebilir. Bazı denetim komutları, Azure Veri Gezgini gibi `.ingest`komutları kullanarak Verileri `.set-or-append`Azure Veri Gezgini'ne yutmayı veya Azure Veri `.export`Gezgini'nden harici veri depolarına kopyalamayı hedefliyor.
Komut etkinliğinin ayrıntılı bir şekilde gözden geçirimi için Azure [Veri Gezgini denetim komutlarını çalıştırmak için Azure Veri Fabrikası komut etkinliğini kullanın'](data-factory-command-activity.md)a bakın.  Verileri kopyalamak için bir denetim komutu kullanmak, bazen Kopyalama etkinliğinden daha hızlı ve daha ucuz bir seçenek olabilir. Komut etkinliğinin Kopyala etkinliğine karşı ne zaman kullanılacağını belirlemek için, [verileri kopyalarken Kopyala ve Komut etkinlikleri arasında seçim](#select-between-copy-and-azure-data-explorer-command-activities-when-copy-data)e bakın.

### <a name="copy-in-bulk-from-a-database-template"></a>Veritabanı şablonundan toplu olarak kopyalama

[Azure Veri Fabrikası şablonu kullanılarak veritabanından Azure Veri Gezgini'ne toplu](data-factory-template.md) olarak Kopyala, önceden tanımlanmış bir Azure Veri Fabrikası ardışık hattıdır. Şablon, daha hızlı veri kopyalama için veritabanı veya tablo başına birçok ardışık iş aktarım oluşturmak için kullanılır. 

### <a name="mapping-data-flows"></a>Veri akışlarını eşleme 

[Azure Veri Fabrikası haritalama veri akışları,](/azure/data-factory/concepts-data-flow-overview) veri mühendislerinin kod yazmadan grafikveri dönüştürme mantığı geliştirmelerine olanak tanıyan görsel olarak tasarlanmış veri dönüşümleridir. Bir veri akışı oluşturmak ve Azure Veri Gezgini'ne veri almak için aşağıdaki yöntemi kullanın:

1. [Eşleme veri akışını](/azure/data-factory/data-flow-create)oluşturun.
1. [Verileri Azure Blob'a dışa aktarın.](/azure/data-factory/data-flow-sink) 
1. Verileri Azure Veri [Gezgini'ne](/azure/data-explorer/ingest-data-event-grid) almak için Olay Ağıt veya [ADF kopyalama etkinliğini](/azure/data-explorer/data-factory-load-data) tanımlayın.

## <a name="select-between-copy-and-azure-data-explorer-command-activities-when-copy-data"></a>Verileri kopyalarken Kopyala ve Azure Veri Gezgini Komutu etkinlikleri arasında seçim 

Bu bölüm, veri kopyalama gereksinimleriniz için doğru etkinliği seçmenize yardımcı olacaktır.

Azure Veri Gezgini'nden veya Azure Veri Gezgini'nden veri kopyalanırken, Azure Veri Fabrikası'nda iki kullanılabilir seçenek vardır:
* Etkinliği kopyalayın.
* Azure Veri Gezgini'nde veri aktarımı yapan denetim komutlarından birini yürüten Azure Veri Gezgini Komutu etkinliği. 

### <a name="copy-data-from-azure-data-explorer"></a>Azure Veri Gezgini'nden veri kopyalama
  
Kopyalama etkinliğini veya komutu [`.export`](/azure/kusto/management/data-export/) kullanarak Azure Veri Gezgini'nden verileri kopyalayabilirsiniz. Komut `.export` bir sorgu yürütür ve sonra sorgu sonuçlarını dışa dışa böler. 

Azure Veri Gezgini'nden veri kopyalama `.export` küçlükünün kopyala etkinliği ve komutunun karşılaştırılması için aşağıdaki tabloya bakın.

| | Kopyalama etkinliği | .export komutu |
|---|---|---|
| **Akış açıklaması** | ADF, Kusto'da bir sorgu yürütür, sonucu işler ve hedef veri deposuna gönderir. <br>(**ADX > ADF > lavabo veri deposu**) | ADF, `.export` komutu çalıştıran ve verileri doğrudan hedef veri deposuna gönderen Azure Veri Gezgini'ne bir denetim komutu gönderir. <br>(**ADX > lavabo veri deposu**) |
| **Desteklenen hedef veri depoları** | Çok çeşitli [desteklenen veri depoları](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) | ADLSv2, Azure Blob, SQL Veritabanı |
| **Performans** | Merkezi | <ul><li>Dağıtılmış (varsayılan), aynı anda birden çok düğümden veri dışa aktarma</li><li>Daha hızlı ve COGS verimli.</li></ul> |
| **Sunucu sınırları** | [Sorgu sınırları](/azure/kusto/concepts/querylimits) genişletilebilir/devre dışı bırakılır. Varsayılan olarak, ADF sorguları şunları içerir: <ul><li>500.000 kayıt veya 64 MB boyut sınırı.</li><li>10 dakikalık zaman sınırı.</li><li>`noTruncation`yanlış olarak ayarlanır.</li></ul> | Varsayılan olarak, sorgu sınırlarını genişletir veya devre dışı kılabilir: <ul><li>Boyut sınırları devre dışı bırakılır.</li><li>Sunucu zaman dilimi 1 saate uzatılır.</li><li>`MaxMemoryConsumptionPerIterator`ve `MaxMemoryConsumptionPerQueryPerNode` maksimum (5 GB, TotalPhysicalMemory/2) uzatılır.</li></ul>

> [!TIP]
> Kopyalama hedefiniz `.export` komut tarafından desteklenen veri depolarından biriyse ve Copy etkinlik özelliklerinden hiçbiri gereksinimleriniz `.export` için çok önemli değilse, komutu seçin.

### <a name="copying-data-to-azure-data-explorer"></a>Azure Veri Gezgini'ne veri kopyalama

Verileri Azure Veri Gezgini'ne kopyalayabilir, kopyalama etkinliği veya [sorgudan](/azure/kusto/management/data-ingestion/ingest-from-query) yutma `.set` `.replace)`(`.set-or-append`, , `.set-or-replace`,`.ingest`, ve [depolamadan yutma](/azure/kusto/management/data-ingestion/ingest-from-storage) ( ) gibi yutma komutlarını kullanarak. 

Kopyalama etkinliğinin karşılaştırılması ve verileri Azure Veri Gezgini'ne kopyalamak için yutma komutları için aşağıdaki tabloya bakın.

| | Kopyalama etkinliği | Sorgudan alma<br> `.set-or-append` / `.set-or-replace` / `.set` / `.replace` | Depolamadan yutma <br> `.ingest` |
|---|---|---|---|
| **Akış açıklaması** | ADF verileri kaynak veri deposundan alır, bir tabular biçime dönüştürür ve gerekli şema eşleme değişikliklerini yapar. ADF daha sonra verileri Azure blobs'a yükler, parçalara böler ve sonra damlaları ADX tablosuna yutmak için indirir. <br> (**Kaynak veri deposu > ADF > Azure lekeleri > ADX**) | Bu komutlar bir sorgu `.show` yutabilir veya bir komut uyguluyor ve sorgu sonuçlarını tabloya **(ADX > ADX)** alabilir. | Bu komut, bir veya daha fazla bulut depolama yapılarından verileri "çekerek" verileri tabloya sindirir. |
| **Desteklenen kaynak veri depoları** |  [çeşitli seçenekler](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) | ADLS Gen 2, Azure Blob, SQL (sql_request eklentisini kullanarak), Cosmos (cosmosdb_sql_request eklentisini kullanarak) ve HTTP veya Python API'leri sağlayan diğer veri depoları. | Filesystem, Azure Blob Depolama, ADLS Gen 1, ADLS Gen 2 |
| **Performans** | Yutmalar sıraya alınır ve yönetilir, bu da küçük boyutlu alımlar sağlar ve yük dengeleme, yeniden denemeler ve hata işleme sağlayarak yüksek kullanılabilirlik sağlar. | <ul><li>Bu komutlar yüksek hacimli veri alma için tasarlanmadı.</li><li>Beklendiği gibi ve daha ucuz çalışır. Ancak üretim senaryoları ve trafik hızları ve veri boyutları büyük olduğunda Kopyalama etkinliğini kullanın.</li></ul> |
| **Sunucu Sınırları** | <ul><li>Boyut sınırı yok.</li><li>Maksimum zaman sınırı: Yutulan blob başına 1 saat. |<ul><li>Sorgu bölümünde yalnızca bir boyut sınırı vardır ve bu sınır `noTruncation=true`belirterek atlanabilir.</li><li>Maksimum zaman sınırı: 1 saat.</li></ul> | <ul><li>Boyut sınırı yok.</li><li>Maksimum zaman sınırı: 1 saat.</li></ul>|

> [!TIP]
> * ADF'den Azure Veri Gezgini'ne `ingest from query` veri kopyalarken komutları kullanın.  
> * Büyük veri kümeleri (>1GB) için Kopyalama etkinliğini kullanın.  

## <a name="required-permissions"></a>Gerekli izinler

Aşağıdaki tabloda, Azure Veri Fabrikası ile tümleştirmede çeşitli adımlar için gerekli izinler listelanmaktadır.

| Adım | İşlem | Minimum izin düzeyi | Notlar |
|---|---|---|---|
| **Bağlantılı Hizmet Oluşturma** | Veritabanı gezintisi | *veritabanı görüntüleyici* <br>ADF kullanan oturum açmış kullanıcı veritabanı meta verilerini okumaya yetkili olmalıdır. | Kullanıcı veritabanı adını el ile sağlayabilir. |
| | Bağlantıyı Sına | *veritabanı monitörü* veya *tablo yutucu* <br>Hizmet sorumlusu veritabanı düzeyi komutlarını `.show` veya tablo düzeyi alımını yürütmeye yetkili olmalıdır. | <ul><li>TestConnection veritabanına değil kümeye olan bağlantıyı doğrular. Veritabanı yok olsa bile başarılı olabilir.</li><li>Tablo yöneticisi izinleri yeterli değildir.</li></ul>|
| **Veri Kümesi Oluşturma** | Tablo gezintisi | *veritabanı monitörü* <br>ADF kullanarak oturum açmış kullanıcı, veritabanı düzeyi `.show` komutları yürütmek için yetkili olmalıdır. | Kullanıcı tablo adını el ile sağlayabilir.|
| **Veri Kümesi** veya **Kopyalama Etkinliği** Oluşturma | Verileri önizleme | *veritabanı görüntüleyici* <br>Hizmet sorumlusu veritabanı meta verilerini okuma yetkisine verilmelidir. | | 
|   | İthalat şeması | *veritabanı görüntüleyici* <br>Hizmet sorumlusu veritabanı meta verilerini okuma yetkisine verilmelidir. | ADX bir tabular-to-tabular kopya kaynağı olduğunda, ADF otomatik olarak şema alacak, kullanıcı açıkça şema almamış olsa bile. |
| **Lavabo olarak ADX** | Ad sütun eşleme oluşturma | *veritabanı monitörü* <br>Hizmet sorumlusu veritabanı düzeyi `.show` komutlarını yürütmeye yetkili olmalıdır. | <ul><li>Tüm zorunlu işlemler *tablo yutucu*ile çalışacaktır.</li><li> Bazı isteğe bağlı işlemler başarısız olabilir.</li></ul> |
|   | <ul><li>Tabloda CSV eşleme oluşturma</li><li>Haritayı bırak</li></ul>| *tablo yutucu* veya *veritabanı yöneticisi* <br>Servis müdürü, bir tabloda değişiklik yapmaya yetkili olmalıdır. | |
|   | Veriyi çekme | *tablo yutucu* veya *veritabanı yöneticisi* <br>Servis müdürü, bir tabloda değişiklik yapmaya yetkili olmalıdır. | | 
| **Kaynak olarak ADX** | Sorguyı yürüt | *veritabanı görüntüleyici* <br>Hizmet sorumlusu veritabanı meta verilerini okuma yetkisine verilmelidir. | |
| **Kusto komutu** | | Her komutun izin düzeyine göre. |

## <a name="performance"></a>Performans 

Azure Veri Gezgini kaynaksa ve sorgu içeren Arama, kopyalama veya komut etkinliğini kullanıyorsanız, performans bilgileri için [sorgu en iyi uygulamaları](/azure/kusto/query/best-practices) ve kopyalama etkinliği için [ADF belgelerine](/azure/data-factory/copy-activity-performance)bakın.
  
Bu bölüm, Azure Veri Gezgini'nin lavabo olduğu kopyalama etkinliğinin kullanımını ele alır. Azure Veri Gezgini lavabosu için tahmini veri veri girişi 11-13 MBps'dir. Aşağıdaki tabloda Azure Veri Gezgini lavabonun performansını etkileyen parametreler ayrıntılı olarak anlatılır.

| Parametre | Notlar |
|---|---|
| **Bileşenlercoğrafi yakınlık** | Tüm bileşenleri aynı bölgeye yerleştirin:<ul><li>kaynak ve lavabo veri depoları.</li><li>ADF tümleştirme çalışma zamanı.</li><li>ADX kümeniz.</li></ul>En azından tümleştirme çalışma sürenizin ADX kümenizle aynı bölgede olduğundan emin olun. |
| **DIUs sayısı** | ADF tarafından kullanılan her 4 DIUs için 1 VM. <br>DIUs'ları artırmak, yalnızca kaynağınız birden çok dosyaiçeren dosya tabanlı bir depoysa yardımcı olur. Her VM daha sonra paralel olarak farklı bir dosya işleyecektir. Bu nedenle, tek bir büyük dosyakopyalama birden çok küçük dosyaları kopyalamadaha yüksek bir gecikme alanı olacaktır.|
|**ADX kümenizin miktarı ve SKU'su** | Yüksek sayıda ADX düğümü yutma işlemini hızlandırır.|
| **Parallelism** | Bir veritabanından çok büyük miktarda veri kopyalamak için verilerinizi bölümleyin ve ardından her bölümü paralel olarak kopyalayan veya [Veritabanından Azure Veri Gezgini Şablonuna Toplu Kopya'yı](data-factory-template.md)kullanan bir ForEach döngüsü kullanın. Not: **Settings** > Kopyalama etkinliğindeki**Paralellik** Ayarları Derecesi ADX ile ilgili değildir. |
| **Veri işleme karmaşıklığı** | Gecikme sonu kaynak dosya biçimine, sütun eşlemeye ve sıkıştırmaya göre değişir.|
| **Entegrasyon çalışma sürenizi çalıştıran VM** | <ul><li>Azure kopyaiçin ADF VM'ler ve makine SDO'ları değiştirilemez.</li><li> Azure kopyasına yönelik ön hazırlık için, kendi barındırılan IR'nizi barındıran VM'nin yeterince güçlü olduğunu belirleyin.</li></ul>|

## <a name="tips-and-common-pitfalls"></a>İpuçları ve yaygın tuzaklar

### <a name="monitor-activity-progress"></a>Etkinlik ilerlemesini izleme

* Etkinlik ilerlemesini izlerken, *Veri okuma* özelliği ikili dosya boyutuna göre hesaplanırken, *yazılan veriler* veriler seri dışı hale getirilip dekompresyondan arındırıldıktan sonra bellek boyutuna göre hesaplanırken, *Veri yazılı* özelliği Veri *okuma* özelliğinden çok daha büyük olabilir.

* Etkinlik ilerlemesini izlerken, verilerin Azure Veri Gezgini lavabosuna yazıldığını görebilirsiniz. Azure Veri Gezgini tablosunu sorgularken, verilerin gelmediğini görürsünüz. Bunun nedeni, Azure Veri Gezgini'ne kopyalama yaparken iki aşama olmasıdır. 
    * İlk aşama kaynak verileri okur, 900 MB'lık parçalara böler ve her bir öbek bir Azure Blob'una yükler. İlk aşama ADF etkinlik ilerleme görünümü tarafından görülür. 
    * Tüm veriler Azure Blobs'a yüklendikten sonra ikinci aşama başlar. Azure Veri Gezgini motor düğümleri lekeleri indirin ve verileri lavabo tablosuna sindirir. Veriler daha sonra Azure Veri Gezgini tablonuzda görülür.

### <a name="failure-to-ingest-csv-files-due-to-improper-escaping"></a>Uygun olmayan kaçış nedeniyle CSV dosyalarının alınmaması

Azure Veri Gezgini, CSV dosyalarının [RFC 4180](https://www.ietf.org/rfc/rfc4180.txt)ile hizalanmasını bekler.
Bu bekliyor:
* Kaçan karakterler içeren alanlar (" ve yeni satırlar gibi) boşluk olmadan **"** karakterle başlayıp bitmelidir. Alan *içindeki* tüm **"** karakterler çift **"** karakter (**""** kullanılarak kaçılır. Örneğin, _"Merhaba, ""Dünya""_ geçerli bir CSV dosyasıdır ve tek bir kaydı olan ve _Hello, "World"_ içeriğine sahip tek bir sütuna veya alana sahip bir Dosyadır.
* Dosyadaki tüm kayıtlar aynı sayıda sütun ve alana sahip olmalıdır.

Azure Veri Fabrikası, ters eğik çizgi (kaçış) karakterine izin verir. Azure Veri Fabrikası'nı kullanarak ters eğik çizgi karakterli bir CSV dosyası oluşturursanız, dosyanın Azure Veri Gezgini'ne aktarılması başarısız olur.

#### <a name="example"></a>Örnek

Aşağıdaki metin değerleri: Merhaba, "Dünya"<br/>
ABC DEF<br/>
"ABC\D"EF<br/>
"ABC DEF<br/>

Uygun bir CSV dosyasında aşağıdaki gibi görünmelidir: "Merhaba, ""Dünya""<br/>
"ABC DEF"<br/>
"""ABC DEF"<br/>
"""ABC\D""EF"<br/>

Varsayılan kaçış karakterini (ters eğik çizgi) kullanarak, aşağıdaki CSV Azure Veri \"\"Gezgini ile çalışmaz: "Hello, World "<br/>
"ABC DEF"<br/>
"\"ABC DEF"<br/>
"\"ABC\D\"EF"<br/>

### <a name="nested-json-objects"></a>İç içe JSON nesneleri

Bir JSON dosyasını Azure Veri Gezgini'ne kopyalarken şunları unutmayın:
* Diziler desteklenmez.
* JSON yapınız nesne veri türleri içeriyorsa, Azure Veri Fabrikası nesnenin alt öğelerini düzleştirmek ve her alt öğeyi Azure Veri Gezgini tablonuzdaki farklı bir sütunla eşlemeye çalışır. Nesne öğenin tamamının Azure Veri Gezgini'nde tek bir sütuna eşlenebilmesini istiyorsanız:
    * Tüm JSON satırını Azure Veri Gezgini'nde tek bir dinamik sütuna sindirin.
    * Azure Veri Fabrikası'nın JSON düzenleyicisini kullanarak boru hattı tanımını el ile düzenleme. **Haritalamada**
       * Her alt öğe için oluşturulan birden çok eşlemeyi kaldırın ve nesne yazınızı tablo sütununuzla eşleyen tek bir eşleme ekleyin.
       * Kapanış kare sveslesonra, ardından bir virgül ekleyin:<br/>
       `"mapComplexValuesToString": true`.

### <a name="specify-additionalproperties-when-copying-to-azure-data-explorer"></a>Azure Veri Gezgini'ne kopyalarken Ek Özellikler belirtin

> [!NOTE]
> Bu özellik şu anda JSON yükünü el ile düzenleyerek kullanılabilir. 

Kopyalama etkinliğinin "lavabo" bölümünün altına aşağıdaki gibi tek bir satır ekleyin:

```json
"sink": {
    "type": "AzureDataExplorerSink",
    "additionalProperties": "{\"tags\":\"[\\\"drop-by:account_FiscalYearID_2020\\\"]\"}"
},
```

Değerden kaçmak zor olabilir. Başvuru olarak aşağıdaki kod parçacıklarını kullanın:

```csharp
static void Main(string[] args)
{
       Dictionary<string, string> additionalProperties = new Dictionary<string, string>();
       additionalProperties.Add("ignoreFirstRecord", "false");
       additionalProperties.Add("csvMappingReference", "Table1_mapping_1");
       IEnumerable<string> ingestIfNotExists = new List<string> { "Part0001" };
       additionalProperties.Add("ingestIfNotExists", JsonConvert.SerializeObject(ingestIfNotExists));
       IEnumerable<string> tags = new List<string> { "ingest-by:Part0001", "ingest-by:IngestedByTest" };
       additionalProperties.Add("tags", JsonConvert.SerializeObject(tags));
       var additionalPropertiesForPayload = JsonConvert.SerializeObject(additionalProperties);
       Console.WriteLine(additionalPropertiesForPayload);
       Console.ReadLine();
}
```

Yazdırılan değer:

```json
{"ignoreFirstRecord":"false","csvMappingReference":"Table1_mapping_1","ingestIfNotExists":"[\"Part0001\"]","tags":"[\"ingest-by:Part0001\",\"ingest-by:IngestedByTest\"]"}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gezgini'ne](data-factory-load-data.md)kopyalamayı öğrenin.
* [Veritabanından Azure Veri Gezgini'ne toplu kopya için Azure Veri Fabrikası şablonu](data-factory-template.md)kullanma hakkında bilgi edinin.
* [Azure Veri Gezgini denetim komutlarını çalıştırmak için Azure Veri Fabrikası komut etkinliğini kullanma](data-factory-command-activity.md)hakkında bilgi edinin.
* Veri sorgulama için [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) hakkında bilgi edinin.



