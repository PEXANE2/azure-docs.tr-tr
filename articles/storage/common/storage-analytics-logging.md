---
title: Azure Depolama Analizi günlüğü
description: Azure Depolama'ya karşı yapılan istekler hakkında ayrıntıları nasıl günlüğe kaydedebilirsiniz öğrenin.
author: normesta
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.openlocfilehash: 5b94a97f1286e1273300014e4eef140be412436b
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637176"
---
# <a name="azure-storage-analytics-logging"></a>Azure Depolama analizi günlüğü

Depolama Analizi, bir depolama cihazına gönderilen başarılı ve başarısız isteklerle ilgili ayrıntılı bilgileri günlüğe kaydeder. Bu bilgileri kullanarak istekleri ayrı ayrı izleyebilir ve depolama hizmetiyle ilgili sorunları tanılayabilirsiniz. İstekler en iyi çaba temelinde günlüğe kaydedilir.

 Depolama Analizi günlüğe kaydetme özelliği depolama hesabınızda varsayılan olarak etkin değildir. [Azure portalında](https://portal.azure.com/)etkinleştirebilirsiniz; ayrıntılar için Azure [portalındaki bir depolama hesabını izleyin'e](/azure/storage/storage-monitor-storage-account)bakın. Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi'ni programlı olarak etkinleştirebilirsiniz. Her hizmet için Depolama Analizi'ni etkinleştirmek için [Blob Hizmet Özelliklerini Al,](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API) [Sıra Hizmeti Özelliklerini Al](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties)ve Tablo Hizmeti Özellikleri işlemlerini [alın'](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties) ı kullanın.

 Günlük girişleri yalnızca hizmet bitiş noktasına karşı yapılan istekler varsa oluşturulur. Örneğin, bir depolama hesabının Blob bitiş noktasında etkinliği varsa, ancak Tablo veya Sıra bitiş noktalarında yoksa, yalnızca Blob hizmetiyle ilgili günlükler oluşturulur.

> [!NOTE]
>  Depolama Analizi günlüğe kaydetme özelliği şu anda yalnızca Blob, Kuyruk ve Tablo hizmetleri için kullanılabilir. Ancak premium depolama hesapları desteklenmez.

## <a name="requests-logged-in-logging"></a>Günlüğe kaydedilen istekler
### <a name="logging-authenticated-requests"></a>Kimlik doğrulama isteklerini günlüğe kaydetme

 Aşağıdaki türden kimliği doğrulanmış istekler kaydedilir:

- Başarılı istekler
- Zaman aşımı, azaltma, ağ, yetkilendirme hatalarını ve diğer hataları içeren başarısız istekler
- Başarısız ve başarılı istekler de dahil olmak üzere Paylaşılan Erişim İmzası (SAS) veya OAuth kullanan istekler
- Analiz verisi istekleri

  Günlük oluşturma veya silme gibi Depolama Analitiği kendisi tarafından yapılan istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi Depolama [Analitiği Oturum Açmış İşlemler ve Durum İletileri ve](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) [Depolama Analizi Günlük Biçimi](/rest/api/storageservices/storage-analytics-log-format) konularında belgelenmiştir.

### <a name="logging-anonymous-requests"></a>Anonim istekleri günlüğe kaydetme

 Aşağıdaki anonim istek türleri günlüğe kaydedilir:

- Başarılı istekler
- Sunucu hataları
- Hem istemci hem de sunucu için zaman ara hataları
- 304 hata koduna sahip başarısız GET istekleri (Değiştirilmemiş)

  Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi Depolama [Analitiği Oturum Açmış İşlemler ve Durum İletileri ve](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) [Depolama Analizi Günlük Biçimi](/rest/api/storageservices/storage-analytics-log-format) konularında belgelenmiştir.

## <a name="how-logs-are-stored"></a>Günlükler nasıl depolanır?

Tüm günlükler, depolama hesabı için Depolama Analitiği etkinleştirildiğinde otomatik olarak oluşturulan `$logs`blok bloblarında depolanır. Kapsayıcı, `$logs` örneğin depolama hesabının blob ad alanında yer `http://<accountname>.blob.core.windows.net/$logs`alır: . Depolama Analitiği etkinleştirildikten sonra bu kapsayıcı silinemez, ancak içeriği silinebilir. Depolama alanına göz atma aracınızı doğrudan kapsayıcıya gitmek için kullanırsanız, günlük verilerinizi içeren tüm lekeleri görürsünüz.

> [!NOTE]
>  `$logs` Kapsayıcı, Liste Kapsayıcıları işlemi gibi bir kapsayıcı listeleme işlemi gerçekleştirildiğinde görüntülenmez. Doğrudan erişilmeli. Örneğin, kapsayıcıdaki lekelere erişmek için Liste Blobs işlemini `$logs` kullanabilirsiniz.

İstekler günlüğe kaydedilirken, Depolama Analizi ara sonuçları blok olarak yükler. Depolama Analizi periyodik olarak bu blokları işleyecek ve bunları bir damla olarak kullanıma sunacak. Günlük **verilerinin $logs** kapsayıcıdaki lekelerde görünmesi bir saat kadar sürebilir, çünkü depolama hizmetinin günlük yazarlarını temizleme sıklığı. Yinelenen kayıtlar, aynı saatte oluşturulan günlükler için kullanılabilir. **RequestId** ve **İşlem** numarasını işaretleyerek kaydın yinelenen olup olmadığını belirleyebilirsiniz.

Her saat için birden fazla dosya içeren yüksek hacimli günlük verileriniz varsa, blob meta veri alanlarını inceleyerek günlüğün hangi verileri içerdiğini belirlemek için blob meta verilerini kullanabilirsiniz. Bu da yararlıdır, çünkü veri günlük dosyalarına yazılırken bazen bir gecikme olabilir: blob meta verileri blob adından daha blob içeriğinin daha doğru bir göstergesiverir.

Çoğu depolama tarama aracı, lekelerin meta verilerini görüntülemenizi sağlar; ayrıca powershell veya programlı kullanarak bu bilgileri okuyabilirsiniz. Aşağıdaki PowerShell parçacığı, bir saat belirtmek için günlük lekeleri listesini ada göre filtreleme ve yalnızca **yazma** işlemleri içeren günlükleri tanımlamak için meta verilerle filtreleme örneğidir.  

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

Blob'ları programlı olarak listeleme hakkında bilgi için [blob kaynaklarını ayarlama](https://msdn.microsoft.com/library/azure/hh452233.aspx) ve [Blob Kaynakları için Özellikleri ve Meta Verileri Ayarlama ve Alma bölümüne](https://msdn.microsoft.com/library/azure/dd179404.aspx)bakın.  

### <a name="log-naming-conventions"></a>Günlük adlandırma kuralları

 Her günlük aşağıdaki biçimde yazılacaktır:

 `<service-name>/YYYY/MM/DD/hhmm/<counter>.log`

 Aşağıdaki tablogünlük adındaki her özniteliği açıklar:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`<service-name>`|Depolama hizmetinin adı. Örneğin: `blob`, `table`, veya`queue`|
|`YYYY`|Günlük için dört basamaklı yıl. Örneğin, `2011`|
|`MM`|Günlük için iki basamaklı ay. Örneğin, `07`|
|`DD`|Günlük için iki basamaklı gün. Örneğin, `31`|
|`hh`|Günlüklerin başlangıç saatini gösteren iki basamaklı saat, 24 saat UTC biçiminde. Örneğin, `18`|
|`mm`|Günlüklerin başlangıç dakikasını gösteren iki basamaklı sayı. **Not:**  Bu değer Depolama Analitiği'nin geçerli sürümünde desteklenmez `00`ve değeri her zaman .|
|`<counter>`|Depolama hizmeti için bir saatlik süre içinde oluşturulan günlük blobsayısını gösteren altı basamaklı sıfır tabanlı sayaç. Bu sayaç `000000`şöyle başlıyor. Örneğin, `000001`|

 Yukarıdaki örnekleri birleştiren tam bir örnek günlük adı aşağıda verilmiştir:

 `blob/2011/07/31/1800/000001.log`

 Yukarıdaki günlük erişmek için kullanılabilecek bir örnek URI aşağıda veda edebilirsiniz:

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 Bir depolama isteği günlüğe kaydedildiğinde, ortaya çıkan günlük adı istenen işlemin tamamlandığı saatle ilişkilidir. Örneğin, Bir GetBlob isteği 31/7/2011 tarihinde saat 18:30'da tamamlanırsa, günlük aşağıdaki önek ile yazılır:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Günlük meta verileri

 Tüm günlük lekeleri, blob'un hangi günlük verilerini içerdiğini belirlemek için kullanılabilecek meta verilerle depolanır. Aşağıdaki tabloda her meta veri özniteliği açıklanır:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`LogType`|Günlüğün okuma, yazma veya silme işlemleriyle ilgili bilgiler ihya edip etmediğini açıklar. Bu değer, virgülle ayrılmış bir tür veya üçünün birleşimini içerebilir.<br /><br /> Örnek 1:`write`<br /><br /> Örnek 2:`read,write`<br /><br /> Örnek 3:`read,write,delete`|
|`StartTime`|Günlükteki bir girişin en erken zamanı, `YYYY-MM-DDThh:mm:ssZ`. Örneğin, `2011-07-31T18:21:46Z`|
|`EndTime`|Günlükteki bir girişin son saati, `YYYY-MM-DDThh:mm:ssZ`. Örneğin, `2011-07-31T18:22:09Z`|
|`LogVersion`|Günlük biçiminin sürümü.|

 Aşağıdaki liste, yukarıdaki örnekleri kullanarak tam örnek meta verileri görüntüler:

-   `LogType=write`
-   `StartTime=2011-07-31T18:21:46Z`
-   `EndTime=2011-07-31T18:22:09Z`
-   `LogVersion=1.0`

## <a name="enable-storage-logging"></a>Depolama günlüğe kaydetmeyi etkinleştir

Azure portalı, PowerShell ve Depolama SDK'ları ile Depolama günlüğe kaydetmeyi etkinleştirebilirsiniz.

### <a name="enable-storage-logging-using-the-azure-portal"></a>Azure portalını kullanarak Depolama günlüğe kaydetmeyi etkinleştirme  

Azure portalında, depolama hesabının **Menü bıçağının** **İzleme (klasik)** bölümünden erişilebilen Depolama Günlüğü'nü denetlemek için **Tanılama ayarları (klasik)** bıçak kullanın.

Günlüğe kaydetmek istediğiniz depolama hizmetlerini ve günlüğe kaydedilmiş verilerin bekletme süresini (gün içinde) belirtebilirsiniz.  

### <a name="enable-storage-logging-using-powershell"></a>PowerShell kullanarak Depolama günlüğü etkinleştirme  

 Geçerli ayarları almak için Azure PowerShell cmdlet **Get-AzureStorageServiceLoggingProperty'i** ve geçerli ayarları değiştirmek için cmdlet **Set-AzureStorageServiceLoggingProperty'i** kullanarak depolama hesabınızda Depolama Günlüğü yapılandırmak için yerel makinenizde PowerShell'i kullanabilirsiniz.  

 Depolama Günlüğü'ne yönelik işlemleri denetleyen cmdletler, günlük olacak istek türlerinin virgülle ayrılmış listesini içeren bir dize olan **Bir Günlük İşlemleri** parametresi kullanır. Üç olası istek türleri **okunur,** **yazar**ve **silinir.** Günlüğe kaydetmeyi kapatmak **için, LoggingOperations** parametresi için **hiçbir** değer kullanın.  

 Aşağıdaki komut, varsayılan depolama hesabınızdaki Sıra hizmetinde okuma, yazma ve silme isteklerini günlüğe kaydetmeyi ve beş güne kadar bekletme yi sağlar:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5  
```  

 Aşağıdaki komut, varsayılan depolama hesabınızdaki tablo hizmeti için günlüğe kaydetmeyi kapatır:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none  
```  

 Azure PowerShell cmdlets'ini Azure aboneliğinizle çalışacak şekilde nasıl yapılandırılabildiğini ve kullanılacak varsayılan depolama hesabını nasıl seçip seçileceksiniz hakkında bilgi için bkz: [Azure PowerShell'i nasıl yükleyip yapılandırın.](https://azure.microsoft.com/documentation/articles/install-configure-powershell/)  

### <a name="enable-storage-logging-programmatically"></a>Depolama günlüğe kaydetmeyi programlı olarak etkinleştirme  

 Depolama Günlüğü'nün denetimini sağlamak için Azure portalını veya Azure PowerShell cmdlets'i kullanmanın yanı sıra, Azure Depolama API'lerinden birini de kullanabilirsiniz. Örneğin, bir .NET dili kullanıyorsanız Depolama İstemci Kitaplığı'nı kullanabilirsiniz.  

 **CloudBlobClient**, **CloudQueueClient**ve **CloudTableClient** sınıfları, Bir **ServiceProperties** nesnesini parametre olarak alan **SetServiceProperties** ve **SetServicePropertiesAsync** gibi yöntemlere sahiptir. Depolama Günlüğü yapılandırmak için **ServiceProperties** nesnesini kullanabilirsiniz. Örneğin, aşağıdaki C# snippet, günlüğe kaydedilenleri ve sıra günlüğe kaydetme süresini nasıl değiştireceğini gösterir:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

 Depolama Günlüğü'ne yapılandırmak için .NET dili kullanma hakkında daha fazla bilgi için Bkz. [Depolama İstemci Kitaplığı Başvurusu.](https://msdn.microsoft.com/library/azure/dn261237.aspx)  

 REST API'sini kullanarak Depolama Günlüğü'nün yapılandırılması hakkında genel bilgi [için](https://msdn.microsoft.com/library/azure/hh360996.aspx)bkz.  

## <a name="download-storage-logging-log-data"></a>Depolama günlük günlüğü verilerini indirin

 Günlük verilerinizi görüntülemek ve çözümlemek için, ilgilendiğiniz günlük verilerini içeren lekeleri yerel bir makineye indirmeniz gerekir. Depolama alanı tarama araçlarının çoğu, depolama hesabınızdan blob indirmenize olanak tanır; günlük verilerinizi indirmek için Azure Depolama ekibini komut satırı azure kopyalama aracı [azcopy](storage-use-azcopy-v10.md) olarak da kullanabilirsiniz.  
 
>[!NOTE]
> Kapsayıcı `$logs` Olay Izgara ile tümleşik olmadığından, günlük dosyaları yazıldığında bildirim almazsınız. 

 İlgilendiğiniz günlük verilerini indirdiğinizden emin olmak ve aynı günlük verilerini birden fazla kez indirmeyi önlemek için:  

-   Aynı verileri birden fazla kez yeniden indirmeyi önlemek için analiz için indirdiğiniz lekeleri izlemek için günlük verileri içeren lekeler için tarih ve saat adlandırma kuralını kullanın.  

-   İndirmeniz gereken tam blob'u tanımlamak için, blob'un günlük verilerini tuttuğu belirli dönemi belirlemek için günlük verilerini içeren lekelerdeki meta verileri kullanın.  

AzCopy ile başlamak için [bkz: AzCopy ile başlayın](storage-use-azcopy-v10.md) 

Aşağıdaki örnek, 20 Mayıs 2014 tarihinde 09:00, 10 ve 11:00'de başlayan saatler için kuyruk hizmetinin günlük verilerini nasıl indirebileceğinizi gösterir.

```
azcopy copy 'https://mystorageaccount.blob.core.windows.net/$logs/queue' 'C:\Logs\Storage' --include-path '2014/05/20/09;2014/05/20/10;2014/05/20/11' --recursive
```

Belirli dosyaları nasıl karşıdan yükleyin hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-blobs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#download-specific-files)

Günlük verilerinizi indirdiğinizde, dosyalardaki günlük girişlerini görüntüleyebilirsiniz. Bu günlük dosyaları, Microsoft İleti Çözümleyicisi de dahil olmak üzere birçok günlük okuma aracının ayrıştınabileceği sınırlı bir metin biçimi kullanır (daha fazla bilgi için microsoft azure [depolama kılavuzunu izleme, tanılama ve Sorun Giderme](storage-monitoring-diagnosing-troubleshooting.md)kılavuzuna bakın). Farklı araçlar, günlük dosyalarınızın içeriğini biçimlendirme, filtreleme, sıralama, reklam arama için farklı olanaklara sahiptir. Depolama Günlüğü günlük dosyası biçimi ve içeriği hakkında daha fazla bilgi için, [Depolama Analizi Günlük Biçimi](/rest/api/storageservices/storage-analytics-log-format) ve Depolama Analizi Oturum Açmış İşlemleri ve Durum [Mesajları'na](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Analitiği Günlük Biçimi](/rest/api/storageservices/storage-analytics-log-format)
* [Depolama Analitiği Günlüğe Kaydedilmiş İşlemler ve Durum Mesajları](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Depolama Analizi Ölçümleri (klasik)](storage-analytics-metrics.md)
