---
title: Günlüğe kaydetme Azure Depolama Analizi
description: Azure depolama 'ya karşı yapılan isteklerle ilgili ayrıntıları günlüğe kaydetme hakkında bilgi edinin.
author: normesta
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.openlocfilehash: 36c6c914c96048825c82a8d1f590a7e805373c08
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854609"
---
# <a name="azure-storage-analytics-logging"></a>Azure depolama Analizi günlüğü

Depolama Analizi, başarılı ve başarısız isteklerle ilgili ayrıntılı bilgileri bir depolama hizmetine kaydeder. Bu bilgiler, bireysel istekleri izlemek ve bir depolama hizmetiyle ilgili sorunları tanılamak için kullanılabilir. İstekler en iyi çaba temelinde günlüğe kaydedilir.

 Depolama Analizi günlüğü, depolama hesabınız için varsayılan olarak etkinleştirilmemiştir. Bunu [Azure Portal](https://portal.azure.com/)etkinleştirebilirsiniz; Ayrıntılar için bkz. [Azure Portal bir depolama hesabını izleme](/azure/storage/storage-monitor-storage-account). Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi programlı bir şekilde etkinleştirebilirsiniz. Her hizmet için Depolama Analizi etkinleştirmek üzere [BLOB hizmeti özelliklerini al](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API), [kuyruk hizmeti özelliklerini al](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties)ve [Tablo hizmeti özelliklerini](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties) Al işlemlerini kullanın.

 Günlük girişleri yalnızca hizmet uç noktasında yapılan istekler varsa oluşturulur. Örneğin, bir depolama hesabının kendi BLOB uç noktasında etkinliği varsa ancak tablo veya sıra uç noktalarında etkinlik varsa, yalnızca blob hizmetiyle ilgili günlükler oluşturulur.

> [!NOTE]
>  Depolama Analizi günlüğe kaydetme şu anda yalnızca blob, kuyruk ve tablo Hizmetleri için kullanılabilir. Ancak, Premium depolama hesabı desteklenmez.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="requests-logged-in-logging"></a>Günlüğe kaydedilen istekler
### <a name="logging-authenticated-requests"></a>Kimliği doğrulanmış istekleri günlüğe kaydetme

 Aşağıdaki kimlik doğrulamalı istek türleri günlüğe kaydedilir:

- Başarılı istekler
- Zaman aşımı, azaltma, ağ, yetkilendirme ve diğer hatalar da dahil olmak üzere başarısız istekler
- Başarısız ve başarılı istekler dahil, paylaşılan erişim Imzası (SAS) veya OAuth kullanan istekler
- Analiz verilerine yönelik istekler

  Günlük oluşturma veya silme gibi Depolama Analizi kendisi tarafından yapılan istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi, [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) konularında belgelenmiştir.

### <a name="logging-anonymous-requests"></a>Anonim istekleri günlüğe kaydetme

 Aşağıdaki anonim istek türleri günlüğe kaydedilir:

- Başarılı istekler
- Sunucu hataları
- İstemci ve sunucu için zaman aşımı hataları
- 304 hata koduyla başarısız olan istek ALıNAMADı (değiştirilmedi)

  Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi, [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) konularında belgelenmiştir.

## <a name="how-logs-are-stored"></a>Günlüklerin nasıl depolandığı

Tüm Günlükler, bir depolama hesabı için depolama Analizi etkinleştirildiğinde otomatik olarak `$logs`oluşturulan adlı kapsayıcıda Blok Bloblarında depolanır. Kapsayıcı, depolama hesabının blob ad alanında bulunur, örneğin: `http://<accountname>.blob.core.windows.net/$logs`. `$logs` Depolama Analizi etkinleştirildikten sonra içerikleri silinebilse de bu kapsayıcı silinemez. Doğrudan kapsayıcıya gitmek için depolama tarama aracınızı kullanıyorsanız, günlüğe kaydetme verilerinizi içeren tüm Blobları görürsünüz.

> [!NOTE]
>  `$logs` Kapsayıcı, liste kapsayıcıları işlemi gibi bir kapsayıcı listeleme işlemi gerçekleştirildiğinde gösterilmez. Doğrudan erişilmesi gerekir. Örneğin, `$logs` kapsayıcıdaki bloblara erişmek için Blobları Listele işlemini kullanabilirsiniz.

İstekler günlüğe kaydedildiğinde Depolama Analizi, ara sonuçları bloklar olarak karşıya yükler. Düzenli aralıklarla, Depolama Analizi bu blokları yapar ve bunları bir blob olarak kullanılabilir hale getirir. Depolama hizmetinin günlük yazıcılarını boşaltdığı sıklık nedeniyle, günlük verilerinin **$logs** kapsayıcısındaki bloblarda görünmesi bir saate kadar sürebilir. Aynı saat içinde oluşturulan Günlükler için yinelenen kayıtlar var olabilir. **RequestId** ve **işlem** numarasını denetleyerek bir kaydın yinelenen olup olmadığını belirleyebilirsiniz.

Her saat için birden çok dosya içeren yüksek miktarda günlük veriniz varsa, blob meta verileri alanlarını inceleyerek günlüğün hangi verileri içerdiğini belirlemek için blob meta verilerini kullanabilirsiniz. Bu Ayrıca, veriler günlük dosyalarına yazılırken bir gecikme olabileceğinden da yararlıdır: blob meta verileri, Blob içeriğinin blob adından daha doğru bir şekilde göstergesidir.

Çoğu depolama gözatma araçları, Blobların meta verilerini görüntülemenizi sağlar; Ayrıca, bu bilgileri PowerShell veya programlı olarak kullanarak da okuyabilirsiniz. Aşağıdaki PowerShell kod parçacığı, bir zaman belirtmek için günlük bloblarının listesini ada göre filtreleme ve yalnızca **yazma** işlemleri içeren günlükleri tanımlamak için meta veriler ile ilgili bir örnektir.  

 ```powershell
 Get-AzureStorageBlob -Container '$logs' |  
 Where-Object {  
     $_.Name -match 'table/2014/05/21/05' -and   
     $_.ICloudBlob.Metadata.LogType -match 'write'  
 } |  
 ForEach-Object {  
     "{0}  {1}  {2}  {3}" –f $_.Name,   
     $_.ICloudBlob.Metadata.StartTime,   
     $_.ICloudBlob.Metadata.EndTime,   
     $_.ICloudBlob.Metadata.LogType  
 }  
 ```  

Blob 'ları program aracılığıyla listeleme hakkında bilgi için bkz. blob [kaynaklarını numaralandırma](https://msdn.microsoft.com/library/azure/hh452233.aspx) ve [BLOB kaynakları Için özellikleri ve meta verileri ayarlama ve alma](https://msdn.microsoft.com/library/azure/dd179404.aspx).  

### <a name="log-naming-conventions"></a>Günlük adlandırma kuralları

 Her günlük aşağıdaki biçimde yazılır:

 `<service-name>/YYYY/MM/DD/hhmm/<counter>.log`

 Aşağıdaki tabloda, günlük adındaki her bir öznitelik açıklanmaktadır:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`<service-name>`|Depolama hizmetinin adı. Örneğin: `blob`, `table`, veya`queue`|
|`YYYY`|Günlük için dört basamaklı yıl. Örneğin, `2011`|
|`MM`|Günlüğün iki basamaklı ayı. Örneğin, `07`|
|`DD`|Günlüğün iki basamaklı günü. Örneğin, `31`|
|`hh`|24 saat UTC biçiminde Günlükler için başlangıç saatini gösteren iki basamaklı saat. Örneğin, `18`|
|`mm`|Günlükler için başlangıç dakikasını gösteren iki basamaklı sayı. **Not:**  Bu değer, geçerli Depolama Analizi sürümünde desteklenmez ve değeri her zaman `00`olur.|
|`<counter>`|Bir saatlik zaman diliminde depolama hizmeti için oluşturulan günlük bloblarının sayısını belirten altı basamaklı sıfır tabanlı bir sayaç. Bu sayaç tarihinde `000000`başlar. Örneğin, `000001`|

 Aşağıda, Yukarıdaki örnekleri birleştiren bir örnek günlük adı verilmiştir:

 `blob/2011/07/31/1800/000001.log`

 Aşağıda, yukarıdaki günlüğe erişmek için kullanılabilecek bir örnek URI verilmiştir:

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 Bir depolama isteği günlüğe kaydedildiğinde, sonuçta elde edilen günlük adı, istenen işlem tamamlandığında saat ile ilişkili olur. Örneğin, 7/31/2011 tarihinde bir GetBlob isteği tamamlanırsa, günlük şu önek ile yazılır:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Günlük meta verileri

 Tüm günlük Blobları, blob 'un içerdiği günlüğe kaydetme verilerini belirlemek için kullanılabilecek meta verilerle depolanır. Aşağıdaki tabloda her meta veri özniteliği açıklanmaktadır:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`LogType`|Günlük okuma, yazma veya silme işlemleriyle ilgili bilgiler içerip içermediğini açıklar. Bu değer, virgülle ayrılmış olarak bir tür ya da üç tane birleşimi içerebilir.<br /><br /> Örnek 1: `write`<br /><br /> Örnek 2: `read,write`<br /><br /> Örnek 3:`read,write,delete`|
|`StartTime`|Günlükteki girdinin en erken saati, biçiminde `YYYY-MM-DDThh:mm:ssZ`. Örneğin, `2011-07-31T18:21:46Z`|
|`EndTime`|Günlükteki bir girişin en son saati, biçiminde `YYYY-MM-DDThh:mm:ssZ`. Örneğin, `2011-07-31T18:22:09Z`|
|`LogVersion`|Günlük biçiminin sürümü.|

 Aşağıdaki liste Yukarıdaki örnekleri kullanarak tüm örnek meta verileri görüntüler:

-   `LogType=write`
-   `StartTime=2011-07-31T18:21:46Z`
-   `EndTime=2011-07-31T18:22:09Z`
-   `LogVersion=1.0`

## <a name="enable-storage-logging"></a>Depolama günlük kaydını etkinleştir

Azure portal, PowerShell ve depolama SDK 'Ları ile depolama günlük kaydını etkinleştirebilirsiniz.

### <a name="enable-storage-logging-using-the-azure-portal"></a>Azure portal kullanarak depolama günlüğünü etkinleştirme  

Azure portal, depolama hesabının **menü dikey**penceresinin **izleme (klasik)** bölümünde erişilebilen depolama günlüğünü denetlemek için **Tanılama ayarları (klasik)** dikey penceresini kullanın.

Günlüğe kaydetmek istediğiniz depolama hizmetlerini ve günlüğe kaydedilen veriler için bekletme süresini (gün cinsinden) belirtebilirsiniz.  

### <a name="enable-storage-logging-using-powershell"></a>PowerShell kullanarak depolama günlüğünü etkinleştirme  

 Geçerli ayarları ve cmdlet **'ı almak Için Get-AzureStorageServiceLoggingProperty cmdlet 'ini kullanarak depolama hesabınızda depolama günlüğü yapılandırmak Için yerel makinenizde PowerShell kullanabilirsiniz Azure PowerShell.** Geçerli ayarları değiştirmek için-AzureStorageServiceLoggingProperty ayarlayın.  

 Depolama günlüğünü denetleyen cmdlet 'ler, günlüğe kaydedilecek istek türleri için virgülle ayrılmış bir liste içeren bir **Loggingoperations** parametresi kullanır. Olası üç istek türü **okuma**, **yazma**ve **silme**. Günlüğe kaydetmeyi devre dışı bırakmak için, **Loggingoperations** parametresi için **none** değerini kullanın.  

 Aşağıdaki komut, bekletme için beş güne ayarlanmış varsayılan depolama hesabınızdaki Kuyruk hizmeti okuma, yazma ve silme istekleri için günlüğe kaydetme yapar:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5  
```  

 Aşağıdaki komut, varsayılan depolama hesabınızda tablo hizmeti için günlüğe kaydetmeyi kapatır:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none  
```  

 Azure PowerShell cmdlet 'lerinin Azure aboneliğinizle çalışacak şekilde nasıl yapılandırılacağı ve kullanılacak varsayılan depolama hesabını nasıl seçkullanılacağı hakkında bilgi için, bkz.: [Azure PowerShell nasıl yüklenir ve yapılandırılır](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

### <a name="enable-storage-logging-programmatically"></a>Depolama günlüğünü programlı olarak etkinleştir  

 Depolama günlüğünü denetlemek için Azure portal veya Azure PowerShell cmdlet 'lerini kullanmanın yanı sıra Azure Storage API 'Lerinden birini de kullanabilirsiniz. Örneğin, bir .NET dili kullanıyorsanız, depolama Istemci kitaplığını kullanabilirsiniz.  

 **Cloudblobclient**, **Cloudqueueclient**ve **cloudtableclient** sınıflarının hepsi, bir **serviceproperties** nesnesi olan **setserviceproperties** ve **SetServicePropertiesAsync** gibi yöntemlere sahiptir parametresinin. Depolama günlüğünü yapılandırmak için **Serviceproperties** nesnesini kullanabilirsiniz. Örneğin, aşağıdaki C# kod parçacığında günlüğe nelerin kaydedildiğini ve kuyruk günlüğe kaydetme için bekletme süresini nasıl değiştirebileceğiniz gösterilmektedir:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

 Depolama günlüğünü yapılandırmak üzere .NET dili kullanma hakkında daha fazla bilgi için bkz. [depolama Istemci kitaplığı başvurusu](https://msdn.microsoft.com/library/azure/dn261237.aspx).  

 REST API kullanarak depolama günlüğünü yapılandırma hakkında genel bilgi için, bkz. [depolama Analizi etkinleştirme ve yapılandırma](https://msdn.microsoft.com/library/azure/hh360996.aspx).  

## <a name="download-storage-logging-log-data"></a>Depolama günlüğü günlük verilerini indirin

 Günlük verilerinizi görüntülemek ve analiz etmek için ilgilendiğiniz günlük verilerini içeren Blobları yerel bir makineye indirmeniz gerekir. Birçok depolama Tarama Aracı, depolama hesabınızdan blob 'ları indirmelerini sağlar; Günlük verilerinizi indirmek için Azure depolama ekibi tarafından sunulan komut satırı Azure kopyalama aracı 'nı (**AzCopy**) de kullanabilirsiniz.  

 İlgilendiğiniz günlük verilerini indirdiğinizden ve aynı günlük verilerinin birden çok kez indirilmesinden kaçınmak için:  

-   Aynı verileri birden çok kez yeniden indirmeyi önlemek için, analiz için zaten indirdiğiniz blob 'ları izlemek üzere günlük verilerini içeren Blobların tarih ve saat adlandırma kuralını kullanın.  

-   Blob 'un, indirme için gereken tam blobu tanımlamak üzere günlük verilerini tuttuğu belirli bir dönemi belirlemek için günlük verilerini içeren bloblarda bulunan meta verileri kullanın.  

> [!NOTE]
>  AzCopy, Azure SDK 'sının bir parçasıdır, ancak her zaman en son sürümü ' den [https://aka.ms/AzCopy](https://aka.ms/AzCopy)indirebilirsiniz. Varsayılan olarak, AzCopy, **C:\Program Files (x86) \Microsoft SDKs\Windows Azure\AzCopy**klasörüne yüklenir ve aracı bir komut Isteminde veya PowerShell penceresinde çalıştırmayı denemeden önce bu klasörü yolunuza eklemelisiniz.  

 Aşağıdaki örnek, 15 Mayıs, 10:00 ve 11 Mayıs 2014 Mayıs 'ta başlayan saat için kuyruk hizmeti günlük verilerini nasıl indirebileceğiniz gösterilmektedir. **/S** parametresi, AzCopy 'in günlük dosyası adlarındaki tarih ve saatlere göre yerel bir klasör yapısı oluşturmasına neden olur; **/v** parametresi AzCopy 'in ayrıntılı çıkış oluşturmasına neden olur; **/y** parametresi, AzCopy 'ın herhangi bir yerel dosyanın üzerine yazılmasına neden olur. **< Yourstorageaccount\>**  değerini depolama hesabınızın adıyla değiştirin ve **\> yourstoragekey <** depolama hesabı anahtarınızla değiştirin.  

```
AzCopy 'http://<yourstorageaccount>.blob.core.windows.net/$logs/queue'  'C:\Logs\Storage' '2014/05/20/09' '2014/05/20/10' '2014/05/20/11' /sourceKey:<yourstoragekey> /S /V /Y  
```  

 AzCopy Ayrıca, indirdiği dosyalardaki son değiştirilme saatini nasıl ayarladığından ve yerel makinenizde zaten var olan dosyalardan daha eski veya daha yeni olan dosyaları indirmeye çalışıp çalışmadığını denetleyen bazı yararlı parametrelere sahiptir. Ayrıca, yeniden başlatılabilir modda da çalıştırabilirsiniz. Tam Ayrıntılar için **AzCopy/?** komutunu çalıştırarak yardımı görüntüleyin komutundaki.  

 Günlük verilerinizi programlama yoluyla nasıl indirecek hakkında bir örnek için bkz [. Microsoft Azure depolama günlüğe kaydetme: Depolama isteklerini](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) izlemek ve sayfada "dumplogs" sözcüğünü aramak için günlükleri kullanma.  

 Günlük verilerinizi indirdiyseniz, dosyalardaki günlük girişlerini görüntüleyebilirsiniz. Bu günlük dosyaları, Microsoft Message Analyzer da dahil olmak üzere çok sayıda günlük okuma aracının ayrıştırabildiği sınırlı bir metin biçimini kullanır (daha fazla bilgi için bkz. kılavuz [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md)). Farklı araçların, günlük dosyalarınızın içeriğini biçimlendirmek, filtrelemek, sıralamak ve ad aramak için farklı özellikleri vardır. Depolama günlüğü günlük dosyası biçimi ve içeriği hakkında daha fazla bilgi için bkz. [depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) ve [depolama Analizi günlüğe kaydedilmiş Işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format)
* [Günlüğe kaydedilen Işlemleri ve durum Iletilerini Depolama Analizi](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Depolama Analizi ölçümleri (klasik)](storage-analytics-metrics.md)
