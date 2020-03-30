---
title: Azure Depolama analiz ölçümleri (Klasik)
description: Azure Depolama'da ölçümleri nasıl kullanacağınızı öğrenin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 897ae1fa474de8726ed0caa1def162a00e142dbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268411"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Azure Depolama analiz ölçümleri (Klasik)

Depolama Analitiği, bir depolama hizmetine gelen isteklerle ilgili toplu işlem istatistikleri ve kapasite verilerini içeren ölçümler ilerleyebilir. Hareketler hem API işlem düzeyinde hem de depolama hizmeti düzeyinde raporlanır ve kapasite depolama hizmeti düzeyinde bildirilir. Ölçüm verileri, depolama hizmeti kullanımını çözümlemek, depolama hizmetine karşı yapılan isteklerle ilgili sorunları tanılamak ve bir hizmeti kullanan uygulamaların performansını artırmak için kullanılabilir.  

 Depolama Analizi ölçümleri varsayılan olarak yeni depolama hesapları için etkinleştirilir. Azure [portalında](https://portal.azure.com/)ölçümleri yapılandırabilirsiniz; ayrıntılar için Azure [portalındaki bir depolama hesabını izleyin'e](/azure/storage/storage-monitor-storage-account)bakın. Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi'ni programlı olarak etkinleştirebilirsiniz. Her hizmet için Depolama Analizi'ni etkinleştirmek için Hizmet Özelliklerini Ayarla işlemlerini kullanın.  

> [!NOTE]
> Depolama Analizi ölçümleri Blob, Queue, Tablo ve Dosya hizmetleri için kullanılabilir.
> Depolama Analizi ölçümleri artık Klasik ölçümlerdir. Microsoft, Depolama Analizi ölçümleri yerine [Azure Monitör'de Depolama Ölçümleri](storage-metrics-in-azure-monitor.md) kullanmanızı önerir.

## <a name="transaction-metrics"></a>İşlem ölçümleri  
 Sağlam bir veri kümesi, her depolama hizmeti ve giriş/çıkış, kullanılabilirlik, hatalar ve kategorilere ayrılarak istek yüzdeleri dahil olmak üzere istenen API işlemi için saatlik veya dakika aralıklarla kaydedilir. [Depolama Analitiği Ölçümleri Tablosu Şeması'nda](/rest/api/storageservices/storage-analytics-metrics-table-schema) işlem ayrıntılarının tam listesini görebilirsiniz.  

 İşlem verileri iki düzeyde kaydedilir : hizmet düzeyi ve API işlem düzeyi. Hizmet düzeyinde, istenen tüm API işlemlerini özetleyen istatistikler, hizmete hiçbir istek yapılmamış olsa bile her saat başı bir tablo varlığına yazılır. API işlem düzeyinde, istatistikler yalnızca işlem bu saat içinde istenirse bir varlığa yazılır.  

 Örneğin, Blob hizmetinizde bir **GetBlob** işlemi gerçekleştirirseniz, Depolama Analizi Ölçümleri isteği günlüğe kaydeder ve hem Blob hizmeti hem de **GetBlob** işlemi için toplanan verilere ekler. Ancak, saat içinde **GetBlob** işlemi istenmezse, bu işlem için *$MetricsTransactionsBlob* bir varlık yazılmayacaktır.  

 İşlem ölçümleri, Depolama Analytics'in kendisi tarafından yapılan hem kullanıcı istekleri hem de istekleri için kaydedilir. Örneğin, Depolama Analitiği'nin günlükleri ve tablo varlıkları yazma istekleri kaydedilir.

## <a name="capacity-metrics"></a>Kapasite ölçümleri  

> [!NOTE]
>  Şu anda kapasite ölçümleri yalnızca Blob hizmeti için kullanılabilir.

 Kapasite verileri, bir depolama hesabının Blob hizmeti için günlük olarak kaydedilir ve iki tablo varlığı yazılır. Bir varlık kullanıcı verileri için istatistikler sağlarken, `$logs` diğeri Depolama Analitiği tarafından kullanılan blob kapsayıcısı hakkında istatistikler sağlar. *$MetricsCapacityBlob* tablosu aşağıdaki istatistikleri içerir:  

- **Kapasite**: Depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı, baytlar halinde.  
- **Konteyner Sayısı**: Depolama hesabının Blob hizmetindeki blob kaplarının sayısı.  
- **ObjectCount**: Depolama hesabının Blob hizmetinde işlenen ve işlenmemiş blok veya sayfa lekelerinin sayısı.  

  Kapasite ölçümleri hakkında daha fazla bilgi için Depolama [Analizi Ölçümleri Tablosu Şeması'na](/rest/api/storageservices/storage-analytics-metrics-table-schema)bakın.  

## <a name="how-metrics-are-stored"></a>Ölçümler nasıl depolanır?  

 Depolama hizmetlerinin her biri için tüm ölçüm verileri, bu hizmet için ayrılmış üç tabloda depolanır: hareket bilgileri için bir tablo, dakika hareket bilgileri için bir tablo ve kapasite bilgileri için başka bir tablo. İşlem ve dakika işlem bilgileri istek ve yanıt verilerinden, kapasite bilgileri ise depolama kullanım verilerinden oluşur. Saat ölçümleri, dakika ölçümleri ve bir depolama hesabının Blob hizmeti için kapasite aşağıdaki tabloda açıklandığı gibi adlandırılmış tablolarda erişilebilir.  

|Ölçümler Düzeyi|Tablo Adları|Sürümler için desteklendi|  
|-------------------|-----------------|----------------------------|  
|Saatlik ölçümler, birincil konum|- $MetricsTransactionsBlob<br />- $MetricsTransactionsTable<br />- $MetricsTransactionsQueue|Yalnızca 2013-08-15 önceki sürümler. Bu adlar hala desteklenirken, aşağıda listelenen tabloları kullanmaya geçmeniz önerilir.|  
|Saatlik ölçümler, birincil konum|- $MetricsHourPrimaryTransactionsBlob<br />- $MetricsHourPrimaryTransactionsTable<br />- $MetricsHourPrimaryTransactionsQueue<br />- $MetricsHourPrimaryTransactionsFile|Tüm versiyonları. Dosya hizmeti ölçümleri için destek yalnızca sürüm 2015-04-05 ve sonraki sürümlerde kullanılabilir.|  
|Dakika ölçümleri, birincil konum|- $MetricsMinutePrimaryTransactionsBlob<br />- $MetricsMinutePrimaryTransactionsTable<br />- $MetricsMinutePrimaryTransactionsQueue<br />- $MetricsMinutePrimaryTransactionsFile|Tüm versiyonları. Dosya hizmeti ölçümleri için destek yalnızca sürüm 2015-04-05 ve sonraki sürümlerde kullanılabilir.|  
|Saatlik ölçümler, ikincil konum|- $MetricsHourSecondaryTransactionsBlob<br />- $MetricsHourSecondaryTransactionsTable<br />- $MetricsHourSecondaryTransactionsQueue|Tüm versiyonları. Okuma-erişim coğrafi çoğaltma etkinleştirilmelidir.|  
|Dakika ölçümleri, ikincil konum|- $MetricsMinuteSecondaryTransactionsBlob<br />- $MetricsMinuteSecondaryTransactionsTable<br />- $MetricsMinuteSecondaryTransactionsQueue|Tüm versiyonları. Okuma-erişim coğrafi çoğaltma etkinleştirilmelidir.|  
|Kapasite (yalnızca Blob hizmeti)|$MetricsCapacityBlob|Tüm versiyonları.|  

 Bu tablolar, depolama hizmeti bitiş noktası için Depolama Analitiği etkinleştirildiğinde otomatik olarak oluşturulur. Bunlara, örneğin depolama hesabının ad alanı üzerinden `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`erişilirler: . Ölçüm tabloları bir giriş işleminde görünmez ve doğrudan tablo adı üzerinden erişilmelidir.  

## <a name="enable-metrics-using-the-azure-portal"></a>Azure portalını kullanarak ölçümleri etkinleştirme
[Azure portalında](https://portal.azure.com)ölçümleri etkinleştirmek için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin.
1. **Menü** bölmesinden **Tanılama ayarlarını (klasik)** seçin.
1. **Durum'un** **A'ya**ayarlandığından emin olun.
1. İzlemek istediğiniz hizmetlerin ölçümlerini seçin.
1. Ölçümleri ve günlük verilerini ne kadar süreyle saklayıp kaydedin.
1. **Kaydet'i**seçin.

[Azure portalı](https://portal.azure.com) şu anda depolama hesabınızda dakika ölçümleri yapılandırmanıza izin vermiyor; PowerShell'i kullanarak veya programlı olarak dakika ölçümlerini etkinleştirmelisiniz.

## <a name="enable-storage-metrics-using-powershell"></a>PowerShell'i kullanarak Depolama ölçümlerini etkinleştirme  
Geçerli ayarları almak için Azure PowerShell cmdlet **Get-AzStorageServiceMetricsProperty'i** ve geçerli ayarları değiştirmek için cmdlet **Set-AzStorageServiceMetricsProperty'i** kullanarak depolama hesabınızdaki Depolama Ölçümlerini yapılandırmak için yerel makinenizde PowerShell'i kullanabilirsiniz.  

Depolama Ölçümlerini kontrol eden cmdletler aşağıdaki parametreleri kullanır:  

* **ServiceType**, olası değeri **Blob**, **Sıra**, **Tablo**ve **Dosya**vardır.
* **MetricsType**, olası değerler **Saat** ve **Dakika**vardır.  
* **MetricsLevel**, olası değerler şunlardır:
* **Yok**: İzlemeyi kapatır.
* **Hizmet**: Blob, queue, tablo ve dosya hizmetleri için toplanan giriş/çıkış, kullanılabilirlik, gecikme ve başarı yüzdeleri gibi ölçümleri toplar.
* **ServiceAndApi**: Hizmet ölçümlerine ek olarak, Azure Depolama hizmeti API'sindeki her depolama işlemi için aynı ölçüm kümesini toplar.

Örneğin, aşağıdaki komut, depolama hesabınızdaki blob hizmeti için dakika ölçümlerini beş güne ayarlanmış bekletme süresiyle değiştirir: 

> [!NOTE]
> Bu komut, `Connect-AzAccount` komutu kullanarak Azure aboneliğinizde oturum açtığınızı varsayar.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"

Set-AzStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5 -Context $storageAccount.Context
```  

* `<resource-group-name>` Yer tutucu değerini kaynak grubunuzun adı ile değiştirin.
        
* `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.



Aşağıdaki komut, varsayılan depolama hesabınızdaki blob hizmeti için geçerli saatlik ölçüm düzeyini ve bekletme günlerini alır:  

```powershell
Get-AzStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob -Context $storagecontext.Context
```  

Azure PowerShell cmdlets'ini Azure aboneliğinizle çalışacak şekilde nasıl yapılandırılabildiğini ve kullanılacak varsayılan depolama hesabını nasıl seçip seçileceksiniz hakkında bilgi için bkz: [Azure PowerShell'i nasıl yükleyip yapılandırın.](https://azure.microsoft.com/documentation/articles/install-configure-powershell/)  

## <a name="enable-storage-metrics-programmatically"></a>Depolama ölçümlerini programlı olarak etkinleştirme  
Depolama Ölçümlerini denetlemek için Azure portalını veya Azure PowerShell cmdlets'i kullanmanın yanı sıra, Azure Depolama API'lerinden birini de kullanabilirsiniz. Örneğin, bir .NET dili kullanıyorsanız Depolama İstemci Kitaplığı'nı kullanabilirsiniz.  

**CloudBlobClient**, **CloudQueueClient**, **CloudTableClient**ve **CloudFileClient** sınıfları, Bir **ServiceProperties** nesnesini parametre olarak alan **SetServiceProperties** ve **SetServicePropertiesAsync** gibi yöntemlere sahiptir. Depolama Ölçümlerini yapılandırmak için **ServiceProperties** nesnesini kullanabilirsiniz. Örneğin, aşağıdaki C# snippet saatlik sıra ölçümleri için ölçümler düzeyi ve bekletme günlerini nasıl değiştireceğinigösterir:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Depolama Ölçümlerini yapılandırmak için .NET dili kullanma hakkında daha fazla bilgi [için .NET için Depolama İstemci Kitaplığı'na](https://msdn.microsoft.com/library/azure/mt347887.aspx)bakın.  

REST API'sini kullanarak Depolama Ölçümlerini yapılandırma hakkında genel bilgi [için](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics)bkz.  

##  <a name="viewing-storage-metrics"></a>Depolama ölçümlerini görüntüleme  
Depolama hesabınızı izlemek için Depolama Analizi ölçümlerini yapılandırdıktan sonra, Depolama Analizi ölçümleri depolama hesabınızda iyi bilinen tablolar kümesinde kaydeder. Grafikleri [Azure portalında](https://portal.azure.com)saatlik ölçümleri görüntülemek üzere yapılandırabilirsiniz:

1. [Azure portalındaki](https://portal.azure.com)depolama hesabınıza gidin.
1. Ölçümleri görüntülemek istediğiniz hizmet için **Menü** bıçağında **Ölçümleri (klasik)** seçin.
1. Yapılandırmak istediğiniz grafiği tıklatın.
1. **Grafik'i Edit** bıçağında, **Zaman Aralığı,** **Grafik türü**ve grafikte görüntülenmesini istediğiniz ölçümleri seçin.

Azure portalındaki Depolama hesabınızın menü bıçağının **İzleme (klasik)** bölümünde, belirli bir metrik belirli bir değere ulaştığında sizi bilgilendirmek için e-posta uyarıları göndermek gibi [Uyarı kurallarını](#metrics-alerts)yapılandırabilirsiniz.

Ölçümleri uzun süreli depolama için indirmek veya yerel olarak analiz etmek istiyorsanız, tabloları okumak için bir araç kullanmanız veya bazı kodlar yazmanız gerekir. Analiz için dakika ölçümlerini indirmeniz gerekir. Depolama hesabınızdaki tüm tabloları listelerseniz tablolar görünmez, ancak bunlara doğrudan adıyla erişebilirsiniz. Depolama alanına göz atma araçlarının çoğu bu tabloların farkındadır ve bunları doğrudan görüntülemenize olanak tanır (kullanılabilir araçların listesi için [Azure Depolama İstemci Araçları'na](/azure/storage/storage-explorers) bakın).

||||  
|-|-|-|  
|**Ölçümler**|**Tablo adları**|**Notlar**|  
|Saatlik ölçümler|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|2013-08-15 önceki sürümlerde bu tablolar şu şekilde biliniyordu:<br /><br /> $MetricsTransactionsBlob<br /><br /> $MetricsTransactionsTable<br /><br /> $MetricsTransactionsQueue<br /><br /> Dosya hizmeti için ölçümler 2015-04-05 sürümünden başlayarak kullanılabilir.|  
|Dakika ölçümleri|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|Yalnızca PowerShell kullanılarak veya programlı olarak etkinleştirilebilir.<br /><br /> Dosya hizmeti için ölçümler 2015-04-05 sürümünden başlayarak kullanılabilir.|  
|Kapasite|$MetricsCapacityBlob|Blob hizmeti sadece.|  

Bu tablolar için şemaların tüm ayrıntılarını [Depolama Analizi Ölçümleri Tablosu Şeması'nda](/rest/api/storageservices/storage-analytics-metrics-table-schema)bulabilirsiniz. Aşağıdaki örnek satırlar yalnızca kullanılabilir sütunların bir alt kümesini gösterir, ancak Depolama Ölçümleri'nin bu ölçümleri kaydetme biçiminin bazı önemli özelliklerini göstermektedir:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**PartitionKey**|**RowKey**|**Zaman damgası**|**TotalRequests**|**Toplam Faturalı İstekler**|**TotalIngress**|**TotalEgress**|**Kullanılabilir -lik**|**AverageE2ELatency**|**AverageServerLatency**|**Yüzde Başarı**|  
|20140522T100|kullanıcı; Tüm|2014-05-22T11:01:16.7650250Z|7|7|4003|46801|100|104.4286|6.857143|100|  
|20140522T100|kullanıcı; Sorgu Kuruluşları|2014-05-22T11:01:16.7640250Z|5|5|2694|45951|100|143.8|7.8|100|  
|20140522T100|kullanıcı; SorguVarlık|2014-05-22T11:01:16.7650250Z|1|1|538|633|100|3|3|100|  
|20140522T100|kullanıcı; UpdateEntity|2014-05-22T11:01:16.7650250Z|1|1|771|217|100|9|6|100|  

Bu örnekte dakika ölçümleri verileri, bölüm anahtarı dakika çözünürlükte zaman kullanır. Satır anahtarı satırda depolanan bilgi türünü tanımlar ve bu iki bilgi parçasından oluşur, erişim türü ve istek türü:  

-   Erişim türü, **kullanıcının** depolama **user** hizmetindeki tüm kullanıcı isteklerini ifade ettiği kullanıcı veya **sistemdir**ve **sistem** Depolama Analitiği tarafından yapılan istekleri ifade eder.  

-   İstek türü, özet satırı olduğu veya **QueryEntity** veya **UpdateEntity**gibi belirli API'yi tanımladığı durumlarda **tümüdür.**  

Yukarıdaki örnek veriler tek bir dakika (11:00'dan itibaren) için tüm kayıtları gösterir, bu nedenle **Sorgu Varlık** isteklerinin sayısı artı QueryEntity isteklerinin sayısı artı **UpdateEntity** isteklerinin sayısı yediye kadar eklenir, bu da kullanıcıda gösterilen **toplamdır:Tüm** satır. **UpdateEntity** Benzer şekilde, kullanıcı üzerinde ortalama uç-uç gecikme 104.4286 elde **edebilirsiniz: Tüm** satır hesaplayarak ((143.8 * 5) + 3 + 9)/7.  

## <a name="metrics-alerts"></a>Ölçümler uyarıları
Depolama hizmetlerinizin davranışındaki önemli değişikliklerden otomatik olarak haberdar olmak için [Azure portalında](https://portal.azure.com) uyarılar ayarlamayı düşünmelisiniz. Bu ölçüm verilerini sınırlı bir biçimde indirmek için bir depolama gezgini aracı kullanıyorsanız, verileri çözümlemek için Microsoft Excel'i kullanabilirsiniz. Kullanılabilir depolama gezgini araçlarının listesi için [Azure Depolama İstemci Araçları'na](/azure/storage/storage-explorers) bakın. Uyarıları, Depolama hesabı menü bıçağında **İzleme (klasik)** altında erişilebilen **Alert (klasik)** bıçakta yapılandırabilirsiniz.

> [!IMPORTANT]
> Bir depolama olayı ile ilgili saatlik veya dakika ölçümleri verilerinin kaydedilmesi arasında bir gecikme olabilir. Dakika ölçümleri söz konusu olduğunda, birkaç dakikalık veriler aynı anda yazılabilir. Bu, önceki dakikalardan gelen hareketlerin geçerli dakika için hareketiçine biraraya olmasına neden olabilir. Bu durumda, uyarı hizmeti yapılandırılan uyarı aralığı için kullanılabilir tüm ölçümlere sahip olmayabilir ve bu da uyarıların beklenmeyen şekilde ateş edilmesine neden olabilir.
>

## <a name="accessing-metrics-data-programmatically"></a>Ölçümlerverilerine programlı olarak erişme  
Aşağıdaki liste, dakika ölçümlerine çeşitli dakikalar için erişen örnek C# kodunu gösterir ve sonuçları bir konsol Penceresinde görüntüler. Kod örneği, depolama daki ölçüm tablolarına erişimi kolaylaştıran **CloudAnalyticsClient** sınıfını içeren Azure Depolama İstemci Kitaplığı sürüm 4.x veya sonraki sürümlerini kullanır.  

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)  
{  
 // Convert the dates to the format used in the PartitionKey  
 var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  
 var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  

 var services = Enum.GetValues(typeof(StorageService));  
 foreach (StorageService service in services)  
 {  
     Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);  
     var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);  
     var t = analyticsClient.GetMinuteMetricsTable(service);  
     var opContext = new OperationContext();  
     var query =  
             from entity in metricsQuery  
             // Note, you can't filter using the entity properties Time, AccessType, or TransactionType  
             // because they are calculated fields in the MetricsEntity class.  
             // The PartitionKey identifies the DataTime of the metrics.  
             where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0   
             select entity;  

     // Filter on "user" transactions after fetching the metrics from Table Storage.  
     // (StartsWith is not supported using LINQ with Azure Table storage)  
     var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));  
     var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();  
     Console.WriteLine(resultString);  
 }  
}  

private static string MetricsString(MetricsEntity entity, OperationContext opContext)  
{  
 var entityProperties = entity.WriteEntity(opContext);  
 var entityString =  
         string.Format("Time: {0}, ", entity.Time) +  
         string.Format("AccessType: {0}, ", entity.AccessType) +  
         string.Format("TransactionType: {0}, ", entity.TransactionType) +  
         string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));  
 return entityString;  
}  
```  

## <a name="billing-on-storage-metrics"></a>Depolama ölçümlerinde faturalandırma  
Ölçümler için tablo varlıkları oluşturmak için yazma istekleri, tüm Azure Depolama işlemleri için geçerli olan standart fiyatlarla ücretlendirilir.  

Bir istemci tarafından ölçüm verilerinin okuma ve silme istekleri de standart fiyatlarla faturalandırılabilir. Bir veri saklama ilkesi yapılandırıldıysanız, Azure Depolama eski metrik verilerini sildiğinde ücretlendirilmezsiniz. Ancak, analitik verileri silerseniz, hesabınızsilme işlemleri için ücretlendirilir.  

Ölçüm tabloları tarafından kullanılan kapasite de faturalandırılabilir. Ölçümler verilerini depolamak için kullanılan kapasite miktarını tahmin etmek için aşağıdakileri kullanabilirsiniz:  

-   Her saat bir hizmet her hizmette her API kullanıyorsa, hem hizmet hem de API düzeyinde özeti etkinleştirdiyseniz, her saat yaklaşık 148 KB veri ölçüm hareketi tablolarında depolanır.  
-   Her saat içinde bir hizmet hizmetteki her API'yi kullanıyorsa, yalnızca hizmet düzeyi özetini etkinleştirdiyseniz, her saat yaklaşık 12KB veri ölçüm hareketi tablolarında depolanır.  
-   Blobs için kapasite tablosu, günlükleri için tercih etmiş seniz, her gün iki satır eklenir. Bu, her gün bu tablonun boyutunun yaklaşık 300 bayt kadar arttığı anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Hesabı Nasıl İzlenir?](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Depolama Analizi Ölçümleri Tablo Şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Depolama Analitiği Günlüğe Kaydedilmiş İşlemler ve Durum Mesajları](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Depolama Analizi Günlüğü](storage-analytics-logging.md)
