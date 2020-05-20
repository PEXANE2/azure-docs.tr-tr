---
title: Azure Depolama Analizi ölçümleri (klasik)
description: Azure depolama 'da Depolama Analizi ölçümlerini nasıl kullanacağınızı öğrenin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.custom: monitoring
ms.openlocfilehash: 5613453667e3bb278f4da22ebed4502def70235b
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83675909"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Azure Depolama Analizi ölçümleri (klasik)

Azure depolama, bir depolama hizmetine yapılan isteklerle ilgili toplanan işlem istatistiklerini ve kapasite verilerini içeren ölçümleri depolamak için Depolama Analizi çözümünü kullanır. İşlemler API işlem düzeyinde ve depolama hizmeti düzeyinde raporlanır. Kapasite, depolama hizmeti düzeyinde raporlanır. Ölçüm verileri şu şekilde kullanılabilir:
- Depolama hizmeti kullanımını çözümleyin.
- Depolama hizmetinde yapılan isteklerle ilgili sorunları tanılayın.
- Hizmet kullanan uygulamaların performansını geliştirir.

 Depolama Analizi ölçümler, varsayılan olarak yeni depolama hesapları için etkinleştirilmiştir. [Azure Portal](https://portal.azure.com/)ölçümleri yapılandırabilirsiniz. Daha fazla bilgi için, [Azure Portal depolama hesabını izleme](/azure/storage/storage-monitor-storage-account)bölümüne bakın. Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi programlı bir şekilde etkinleştirebilirsiniz. Her hizmet için Depolama Analizi etkinleştirmek üzere hizmet özelliklerini ayarlama işlemlerini kullanın.  

> [!NOTE]
> Depolama Analizi ölçümler Azure Blob depolama, Azure kuyruk depolama, Azure Tablo depolama ve Azure dosyaları için kullanılabilir.
> Depolama Analizi ölçümler artık klasik ölçümlerdir. Depolama ölçümlerini Depolama Analizi ölçümleri yerine [Azure izleyici 'de](monitor-storage.md) kullanmanızı öneririz.

## <a name="transaction-metrics"></a>İşlem ölçümleri  
 Güçlü bir veri kümesi, her depolama hizmeti için saatlik veya dakika aralıklarında, giriş ve çıkış, kullanılabilirlik, hata ve kategorilere ayrılan istek yüzdelerini içeren istenen API işlemi için kaydedilir. İşlem ayrıntılarının tüm listesi için bkz. [ölçüm tablosu şeması depolama Analizi](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

 İşlem verileri, hizmet düzeyinde ve API işlem düzeyinde kaydedilir. Hizmet düzeyinde, tüm istenen API işlemlerini özetleyen istatistikler, hizmette hiçbir istek yapılmasa bile, her saat bir tablo varlığına yazılır. API işlem düzeyinde, yalnızca işlem bu saat içinde isteniyorsa, bir varlığa istatistikler yazılır.  

 Örneğin, blob hizmetinizde bir **GetBlob** işlemi gerçekleştirirseniz depolama Analizi ölçümleri isteği günlüğe kaydeder ve BLOB hizmeti ve **GetBlob** işlemi için toplanan verilere ekler. Saat boyunca bir **GetBlob** işlemi istenmemişse, bu işlem için *$MetricsTransactionsBlob* bir varlık yazılmaz.  

 İşlem ölçümleri Kullanıcı istekleri ve Depolama Analizi tarafından yapılan istekler için kaydedilir. Örneğin, Günlükler ve tablo varlıkları yazmak için Depolama Analizi tarafından yapılan istekler kaydedilir.

## <a name="capacity-metrics"></a>Kapasite ölçümleri  

> [!NOTE]
>  Şu anda kapasite ölçümleri yalnızca blob hizmeti için kullanılabilir.

 Bir depolama hesabının blob hizmeti için kapasite verileri günlük olarak kaydedilir ve iki tablo varlığı yazılır. Bir varlık Kullanıcı verileri için istatistikler sağlar ve diğeri `$logs` depolama Analizi tarafından kullanılan blob kapsayıcısı hakkında istatistikler sağlar. *$MetricsCapacityBlob* tablosu aşağıdaki istatistikleri içerir:  

- **Kapasite**: depolama hesabının blob hizmeti tarafından bayt cinsinden kullanılan depolama miktarı.  
- **Containercount**: depolama hesabının blob hizmetindeki blob kapsayıcıları sayısı.  
- **ObjectCount**: depolama hesabının blob hizmetindeki işlenen ve kaydedilmemiş blok veya sayfa Blobları sayısı.  

  Kapasite ölçümleri hakkında daha fazla bilgi için bkz. [ölçüm tablosu şeması depolama Analizi](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

## <a name="how-metrics-are-stored"></a>Ölçümler nasıl depolanır  

 Depolama hizmetlerinin her biri için tüm ölçüm verileri, bu hizmet için ayrılmış üç tabloda depolanır. Bir tablo, işlem bilgileri için, bir tablo dakikalık işlem bilgileri için ve başka bir tablo ise kapasite bilgilerine yöneliktir. İşlem ve dakikalık işlem bilgileri, istek ve yanıt verilerinden oluşur. Kapasite bilgileri, depolama kullanım verilerinden oluşur. Aşağıdaki tabloda açıklandığı gibi adlı tablolarda, bir depolama hesabının blob hizmetine yönelik saat ölçümleri, dakika ölçümleri ve kapasiteye erişilir.  

|Ölçüm düzeyi|Tablo adları|Sürümler için desteklenir|  
|-------------------|-----------------|----------------------------|  
|Saatlik ölçümler, birincil konum|-$MetricsTransactionsBlob<br />-$MetricsTransactionsTable<br />-$MetricsTransactionsQueue|Yalnızca 15 Ağustos 2013 ' den önceki sürümler. Bu adlar hala desteklenirken, izleyen tabloları kullanarak geçiş yapmanızı öneririz.|  
|Saatlik ölçümler, birincil konum|-$MetricsHourPrimaryTransactionsBlob<br />-$MetricsHourPrimaryTransactionsTable<br />-$MetricsHourPrimaryTransactionsQueue<br />-$MetricsHourPrimaryTransactionsFile|Tüm sürümler. Dosya hizmeti ölçümleri desteği yalnızca 5 Nisan 2015 ve üzeri sürümlerde kullanılabilir.|  
|Dakikalık ölçümler, birincil konum|-$MetricsMinutePrimaryTransactionsBlob<br />-$MetricsMinutePrimaryTransactionsTable<br />-$MetricsMinutePrimaryTransactionsQueue<br />-$MetricsMinutePrimaryTransactionsFile|Tüm sürümler. Dosya hizmeti ölçümleri desteği yalnızca 5 Nisan 2015 ve üzeri sürümlerde kullanılabilir.|  
|Saatlik ölçümler, ikincil konum|-$MetricsHourSecondaryTransactionsBlob<br />-$MetricsHourSecondaryTransactionsTable<br />-$MetricsHourSecondaryTransactionsQueue|Tüm sürümler. Okuma Erişimli Coğrafi olarak yedekli çoğaltmanın etkinleştirilmesi gerekir.|  
|Dakikalık ölçümler, ikincil konum|-$MetricsMinuteSecondaryTransactionsBlob<br />-$MetricsMinuteSecondaryTransactionsTable<br />-$MetricsMinuteSecondaryTransactionsQueue|Tüm sürümler. Okuma Erişimli Coğrafi olarak yedekli çoğaltmanın etkinleştirilmesi gerekir.|  
|Kapasite (yalnızca blob hizmeti)|$MetricsCapacityBlob|Tüm sürümler.|  

 Bu tablolar, bir depolama hizmeti uç noktası için Depolama Analizi etkinleştirildiğinde otomatik olarak oluşturulur. Bunlar, depolama hesabının ad alanı aracılığıyla erişilir. Örneğin, `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")` . Ölçüm tabloları bir listeleme işleminde görünmez ve doğrudan tablo adı ile erişilmelidir.

## <a name="enable-metrics-by-using-the-azure-portal"></a>Azure portal kullanarak ölçümleri etkinleştirin
[Azure Portal](https://portal.azure.com)ölçümleri etkinleştirmek için şu adımları izleyin:

1. Depolama hesabınıza gidin.
1. Menü bölmesinde **Tanılama Ayarları ' nı (klasik)** seçin.
1. **Durumun** **Açık**olarak ayarlandığından emin olun.
1. İzlemek istediğiniz hizmetler için ölçümleri seçin.
1. Ölçüm ve günlük verilerinin ne kadar süreyle saklanacağını göstermek için bir bekletme ilkesi belirtin.
1. **Kaydet**’i seçin.

[Azure Portal](https://portal.azure.com) Şu anda depolama hesabınızda dakika ölçümlerini yapılandırmanıza izin vermez. PowerShell veya program aracılığıyla dakika ölçümlerini etkinleştirmeniz gerekir.

## <a name="enable-storage-metrics-by-using-powershell"></a>PowerShell kullanarak depolama ölçümlerini etkinleştirme  
Geçerli ayarları almak için **Get-AzStorageServiceMetricsProperty** Azure PowerShell cmdlet 'ini kullanarak Depolama hesabınızdaki depolama ölçümlerini yapılandırmak için yerel makinenizde PowerShell kullanabilirsiniz. Geçerli ayarları değiştirmek için **set-AzStorageServiceMetricsProperty** cmdlet 'ini kullanın.  

Depolama ölçümlerini denetleyen cmdlet 'ler şu parametreleri kullanır:  

* **ServiceType**: olası değerler **BLOB**, **kuyruk**, **tablo**ve **dosyadır**.
* **Metricstype**: olası değerler **saat** ve **dakikadır**.  
* **Metricslevel**: olası değerler şunlardır:
   * **Hiçbiri**: izlemeyi kapatır.
   * **Hizmet**: blob, kuyruk, tablo ve dosya hizmetleri için toplanan giriş ve çıkış, kullanılabilirlik, gecikme süresi ve başarı yüzdeleri gibi ölçümleri toplar.
   * **ServiceAndApi**: hizmet ölçümlerine ek olarak, Azure depolama hizmeti API 'sindeki her depolama işlemi için aynı ölçüm kümesini toplar.

Örneğin, aşağıdaki komut, Depolama hesabınızdaki blob hizmeti için dakika ölçümlerinde, bekletme dönemi beş güne ayarlanmış olarak geçer: 

> [!NOTE]
> Bu komut, komutunu kullanarak Azure aboneliğinizde oturum açmış olduğunuzu varsayar `Connect-AzAccount` .

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"

Set-AzStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5 -Context $storageAccount.Context
```  

* `<resource-group-name>`Yer tutucu değerini kaynak grubunuzun adıyla değiştirin.      
* `<storage-account-name>`Yer tutucu değerini depolama hesabınızın adıyla değiştirin.



Aşağıdaki komut, varsayılan depolama hesabınızdaki blob hizmeti için geçerli saatlik ölçüm düzeyini ve bekletme günlerini alır:  

```powershell
Get-AzStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob -Context $storagecontext.Context
```  

Azure PowerShell cmdlet 'lerinin Azure aboneliğinizle çalışacak şekilde nasıl yapılandırılacağı ve kullanılacak varsayılan depolama hesabının nasıl ayarlanacağı hakkında bilgi için, bkz. [Azure PowerShell 'Yi yükleyip yapılandırma](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

## <a name="enable-storage-metrics-programmatically"></a>Depolama ölçümlerini programlı olarak etkinleştir  
Depolama ölçümlerini denetlemek için Azure portal veya Azure PowerShell cmdlet 'lerini kullanmanın yanı sıra Azure Storage API 'Lerinden birini de kullanabilirsiniz. Örneğin, bir .NET dili kullanıyorsanız Azure Storage istemci kitaplığını kullanabilirsiniz.  

**Cloudblobclient**, **cloudqueueclient**, **Cloudtableclient**ve **Cloudfileclient** sınıflarının hepsi, bir **serviceproperties** nesnesini parametre olarak alan **setserviceproperties** ve **SetServicePropertiesAsync** gibi yöntemlere sahiptir. Depolama ölçümlerini yapılandırmak için **Serviceproperties** nesnesini kullanabilirsiniz. Örneğin, aşağıdaki C# kod parçacığında saatlik sıra ölçümleri için ölçüm düzeyinin ve bekletme günlerinin nasıl değiştirileceği gösterilmektedir:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Depolama ölçümlerini yapılandırmak için .NET dili kullanma hakkında daha fazla bilgi için bkz. [.net Için Azure Storage istemci kitaplıkları](https://msdn.microsoft.com/library/azure/mt347887.aspx).  

REST API kullanarak depolama ölçümlerini yapılandırma hakkında genel bilgi için bkz. [depolama Analizi etkinleştirme ve yapılandırma](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics).  

##  <a name="view-storage-metrics"></a>Depolama ölçümlerini görüntüleme  
Depolama hesabınızı izlemek için Depolama Analizi ölçümleri yapılandırdıktan sonra, Depolama Analizi ölçümleri Depolama hesabınızdaki bir dizi iyi bilinen tabloya kaydeder. [Azure Portal](https://portal.azure.com)saatlik ölçümleri görüntülemek için grafikleri yapılandırabilirsiniz:

1. [Azure Portal](https://portal.azure.com)depolama hesabınıza gidin.
1. Ölçümlerini görüntülemek istediğiniz hizmetin menü bölmesinde **ölçümler (klasik)** öğesini seçin.
1. Yapılandırmak istediğiniz grafiği seçin.
1. **Grafik Düzenle** bölmesinde **zaman aralığını**, **grafik türünü**ve grafikte görüntülenmesini istediğiniz ölçümleri seçin.

Azure portal depolama hesabınızın menü bölmesinin **izleme (klasik)** bölümünde [Uyarı kurallarını](#metrics-alerts)yapılandırabilirsiniz. Örneğin, belirli bir ölçüm belirli bir değere ulaştığında size bildirimde bulunmak için e-posta uyarıları gönderebilirsiniz.

Daha uzun vadeli depolama için ölçümleri indirmek veya yerel olarak analiz etmek istiyorsanız, bir araç kullanmanız veya tabloları okumak için kod yazmanız gerekir. Analiz için dakika ölçümlerini indirmeniz gerekir. Depolama hesabınızdaki tüm tabloları listeleyerek tablolar görünmez, ancak bunlara doğrudan ad ile erişebilirsiniz. Birçok depolama tarama aracı bu tabloları algılar ve bunları doğrudan görüntülemenizi sağlar. Kullanılabilir araçların listesi için bkz. [Azure Storage istemci araçları](/azure/storage/storage-explorers).

||||  
|-|-|-|  
|**Metrics** (Ölçümler)|**Tablo adları**|**Notlar**|  
|Saatlik ölçümler|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|15 Ağustos 2013 ' den önceki sürümlerde bu tablolar şu şekilde bilinmektedir:<br /><br /> $MetricsTransactionsBlob<br /><br /> $MetricsTransactionsTable<br /><br /> $MetricsTransactionsQueue<br /><br /> Dosya hizmeti ölçümleri 5 Nisan 2015 sürümünden itibaren kullanılabilir.|  
|Dakikalık ölçümler|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|Yalnızca PowerShell veya programlı programlama kullanılarak etkinleştirilebilir.<br /><br /> Dosya hizmeti ölçümleri 5 Nisan 2015 sürümünden itibaren kullanılabilir.|  
|Kapasite|$MetricsCapacityBlob|Yalnızca blob hizmeti.|  

Bu tablolara yönelik şemaların tam ayrıntıları için bkz. [ölçüm tablosu şeması depolama Analizi](/rest/api/storageservices/storage-analytics-metrics-table-schema). Aşağıdaki örnek satırlar yalnızca kullanılabilir sütunların bir alt kümesini gösterir, ancak depolama ölçümlerinin bu ölçümleri kaydetme yolunun bazı önemli özelliklerini gösterir:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**PartitionKey**|**RowKey**|**İlişkin**|**TotalRequests**|**TotalBillableRequests**|**TotalIngress**|**TotalEgress**|**Kullanılabilirlik**|**AverageE2ELatency**|**AverageServerLatency**|**PercentSuccess**|  
|20140522T1100|kullanıcısını Bütün|2014-05-22T11:01:16.7650250 Z|7|7|4003|46801|100|104,4286|6,857143|100|  
|20140522T1100|kullanıcısını Queryenlikler|2014-05-22T11:01:16.7640250 Z|5|5|2694|45951|100|143,8|7,8|100|  
|20140522T1100|kullanıcısını QueryEntity|2014-05-22T11:01:16.7650250 Z|1|1|538|633|100|3|3|100|  
|20140522T1100|kullanıcısını UpdateEntity|2014-05-22T11:01:16.7650250 Z|1|1|771|217|100|9|6|100|  

Bu dakikalık ölçüm verileri örneğinde, bölüm anahtarı dakika çözünürlüğündeki zamanı kullanır. Satır anahtarı, satırda depolanan bilgi türünü tanımlar. Bilgiler, erişim türünden ve istek türünden oluşur:  

-   Erişim türü **Kullanıcı veya** **sistemdir**; burada **Kullanıcı** , depolama hizmetine yönelik tüm Kullanıcı isteklerini ifade eder ve **sistem** , depolama Analizi tarafından yapılan isteklere başvurur.  
-   İstek türü **Tümü**, bu durumda bir Özet satırdır veya **Queryentity** veya **updateentity**gibi belirli API 'yi tanımlar.  

Bu örnek veri, tek bir dakikalık (11 ' den başlayarak) tüm kayıtları gösterir. bu nedenle, **Queryentities** isteklerinin sayısı artı **Queryentity** Isteklerinin sayısı artı **updateentity** isteklerinin sayısı yedi ' a kadar bir değer ekler. Bu toplam, **Kullanıcı: ALL** satırında gösterilir. Benzer şekilde, Kullanıcı üzerindeki ortalama uçtan uca gecikme 104,4286 ' u türetebilirsiniz **:** ((143,8 * 5) + 3 + 9)/7.  

## <a name="metrics-alerts"></a>Ölçüm uyarıları
[Azure Portal](https://portal.azure.com) uyarı ayarlamayı düşünün, böylece depolama hizmetlerinizin davranışındaki önemli değişiklikler konusunda otomatik olarak bilgilendirilirsiniz. Bu ölçüm verilerini ayrılmış bir biçimde indirmek için bir Depolama Gezgini aracı kullanırsanız, verileri çözümlemek için Microsoft Excel 'i kullanabilirsiniz. Kullanılabilir Depolama Gezgini araçlarının bir listesi için bkz. [Azure Storage istemci araçları](/azure/storage/storage-explorers). **Uyarı (klasik)** bölmesinde, depolama hesabı menü bölmesinde **izleme (klasik)** bölümünde erişilebilen uyarıları yapılandırabilirsiniz.

> [!IMPORTANT]
> Bir depolama olayı ile buna karşılık gelen saatlik veya dakikalık ölçüm verileri kaydedilirken bir gecikme olabilir. Dakikalık ölçümler söz konusu olduğunda, birkaç dakikalık veriler aynı anda yazılabilir. Bu sorun, geçerli dakikalık işlem için daha önceki dakikadan elde edilen işlemlere neden olabilir. Bu sorun oluştuğunda, uyarı hizmeti yapılandırılan uyarı aralığı için kullanılabilir tüm ölçüm verilerine sahip olmayabilir, bu da uyarıların beklenmedik şekilde tetiklerine neden olabilir.
>

## <a name="access-metrics-data-programmatically"></a>Ölçüm verilerine programlama yoluyla erişin  
Aşağıdaki listede, bir dizi dakika için dakikalık ölçümlere erişen örnek C# kodu gösterilmektedir ve sonuçlar bir konsol penceresinde görüntülenir. Kod örneği, depolamada ölçüm tablolarına erişimi kolaylaştıran **CloudAnalyticsClient** sınıfını Içeren Azure Storage istemci kitaplığı sürüm 4. x veya üstünü kullanır.  

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)  
{  
 // Convert the dates to the format used in the PartitionKey.  
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

     // Filter on "user" transactions after fetching the metrics from Azure Table storage.  
     // (StartsWith is not supported using LINQ with Azure Table storage.)  
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
Ölçümler için tablo varlıkları oluşturmak için yazma istekleri, tüm Azure depolama işlemleri için geçerli olan standart ücretler üzerinden ücretlendirilir.  

Bir istemciye ait ölçüm verilerinin okuma ve silme istekleri de standart ücretler üzerinden faturalandırılabilir. Bir veri saklama ilkesi yapılandırdıysanız, Azure depolama eski ölçüm verilerini sildiğinde ücretlendirilmez. Analiz verilerini silerseniz, hesabınız silme işlemleri için ücretlendirilir.  

Ölçüm tabloları tarafından kullanılan kapasite da faturalanabilir. Ölçüm verilerini depolamak için kullanılan kapasite miktarını tahmin etmek için aşağıdaki bilgileri kullanın:  

-   Her saat bir hizmet her bir hizmette her API 'yi kullanıyorsa, hizmet düzeyi ve API düzeyi bir Özet etkinleştirdiyseniz ölçüm işlem tablolarında yaklaşık 148 KB veri depolanır.  
-   Her saat içinde bir hizmet, hizmette her API 'yi kullanıyorsa, yalnızca bir hizmet düzeyi Özeti etkinleştirdiyseniz ölçüm işlem tablolarında yaklaşık 12 KB 'lık veri depolanır.  
-   Blobların kapasite tablosunda Günlükler için tercih ettiğiniz her gün eklenen iki satır vardır. Bu senaryo, bu tablonun boyutunun yaklaşık 300 bayta kadar arttığı anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama hesabı izleme](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Depolama Analizi ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Günlüğe kaydedilen işlemleri ve durum iletilerini Depolama Analizi](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Günlüğe kaydetme Depolama Analizi](storage-analytics-logging.md)
