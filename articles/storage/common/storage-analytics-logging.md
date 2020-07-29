---
title: Azure Depolama Analizi günlüğü
description: Azure depolama 'ya karşı yapılan isteklerle ilgili ayrıntıları günlüğe kaydetme hakkında bilgi edinin.
author: normesta
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 07/23/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring
ms.openlocfilehash: 061c7f6a45b8667b7fd03d62bee67c695bec5e68
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87276797"
---
# <a name="azure-storage-analytics-logging"></a>Azure depolama Analizi günlüğü

Depolama Analizi, bir depolama cihazına gönderilen başarılı ve başarısız isteklerle ilgili ayrıntılı bilgileri günlüğe kaydeder. Bu bilgileri kullanarak istekleri ayrı ayrı izleyebilir ve depolama hizmetiyle ilgili sorunları tanılayabilirsiniz. İstekler en iyi çaba temelinde günlüğe kaydedilir.

 Depolama Analizi günlüğe kaydetme özelliği depolama hesabınızda varsayılan olarak etkin değildir. Bunu [Azure Portal](https://portal.azure.com/)etkinleştirebilirsiniz; Ayrıntılar için bkz. [Azure Portal bir depolama hesabını izleme](/azure/storage/storage-monitor-storage-account). Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi programlı bir şekilde etkinleştirebilirsiniz. Her hizmet için Depolama Analizi etkinleştirmek üzere [BLOB hizmeti özelliklerini al](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API), [kuyruk hizmeti özelliklerini al](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties)ve [Tablo hizmeti özelliklerini](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties) Al işlemlerini kullanın.

 Günlük girişleri yalnızca hizmet uç noktasında yapılan istekler varsa oluşturulur. Örneğin, bir depolama hesabının kendi BLOB uç noktasında etkinliği varsa ancak tablo veya sıra uç noktalarında etkinlik varsa, yalnızca blob hizmetiyle ilgili günlükler oluşturulur.

> [!NOTE]
>  Depolama Analizi günlüğe kaydetme özelliği şu anda yalnızca Blob, Kuyruk ve Tablo hizmetleri için kullanılabilir. Depolama Analizi günlüğe kaydetme, Premium performanslı [blok Blobstorage](../blobs/storage-blob-create-account-block-blob.md) hesapları için de kullanılabilir. Ancak, Premium performansa sahip genel amaçlı v2 hesapları için kullanılamaz.

## <a name="requests-logged-in-logging"></a>Günlüğe kaydedilen istekler
### <a name="logging-authenticated-requests"></a>Kimliği doğrulanmış istekleri günlüğe kaydetme

 Aşağıdaki türden kimliği doğrulanmış istekler kaydedilir:

- Başarılı istekler
- Zaman aşımı, azaltma, ağ, yetkilendirme hatalarını ve diğer hataları içeren başarısız istekler
- Başarısız ve başarılı istekler de dahil olmak üzere Paylaşılan Erişim İmzası (SAS) veya OAuth kullanan istekler
- Analiz verisi istekleri

  Günlük oluşturma veya silme gibi Depolama Analizi kendisi tarafından yapılan istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi, [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) konularında belgelenmiştir.

### <a name="logging-anonymous-requests"></a>Anonim istekleri günlüğe kaydetme

 Aşağıdaki anonim istek türleri günlüğe kaydedilir:

- Başarılı istekler
- Sunucu hataları
- İstemci ve sunucu için zaman aşımı hataları
- 304 hata koduna sahip başarısız GET istekleri (Değiştirilmemiş)

  Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi, [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) konularında belgelenmiştir.

## <a name="how-logs-are-stored"></a>Günlüklerin nasıl depolandığı

Tüm Günlükler `$logs` , bir depolama hesabı için depolama Analizi etkinleştirildiğinde otomatik olarak oluşturulan adlı kapsayıcıda Blok Bloblarında depolanır. `$logs`Kapsayıcı, depolama hesabının blob ad alanında bulunur, örneğin: `http://<accountname>.blob.core.windows.net/$logs` . Depolama Analizi etkinleştirildikten sonra içerikleri silinebilse de bu kapsayıcı silinemez. Doğrudan kapsayıcıya gitmek için depolama tarama aracınızı kullanıyorsanız, günlüğe kaydetme verilerinizi içeren tüm Blobları görürsünüz.

> [!NOTE]
>  Kapsayıcı, `$logs` liste kapsayıcıları işlemi gibi bir kapsayıcı listeleme işlemi gerçekleştirildiğinde gösterilmez. Doğrudan erişilmesi gerekir. Örneğin, kapsayıcıdaki bloblara erişmek için Blobları Listele işlemini kullanabilirsiniz `$logs` .

İstekler günlüğe kaydedildiğinde Depolama Analizi, ara sonuçları bloklar olarak karşıya yükler. Düzenli aralıklarla, Depolama Analizi bu blokları yapar ve bunları bir blob olarak kullanılabilir hale getirir. Depolama hizmetinin günlük yazıcılarını boşaltdığı sıklık nedeniyle, günlük verilerinin **$logs** kapsayıcısındaki bloblarda görünmesi bir saate kadar sürebilir. Aynı saat içinde oluşturulan Günlükler için yinelenen kayıtlar var olabilir. **RequestId** ve **işlem** numarasını denetleyerek bir kaydın yinelenen olup olmadığını belirleyebilirsiniz.

Her saat için birden çok dosya içeren yüksek miktarda günlük veriniz varsa, blob meta verileri alanlarını inceleyerek günlüğün hangi verileri içerdiğini belirlemek için blob meta verilerini kullanabilirsiniz. Bu Ayrıca, veriler günlük dosyalarına yazılırken bir gecikme olabileceğinden da yararlıdır: blob meta verileri, Blob içeriğinin blob adından daha doğru bir şekilde göstergesidir.

Çoğu depolama gözatma araçları, Blobların meta verilerini görüntülemenizi sağlar; Ayrıca, bu bilgileri PowerShell veya programlı olarak kullanarak da okuyabilirsiniz. Aşağıdaki PowerShell kod parçacığı, bir zaman belirtmek için günlük bloblarının listesini ada göre filtreleme ve yalnızca **yazma** işlemleri içeren günlükleri tanımlamak için meta veriler ile ilgili bir örnektir.  

 ```powershell
 Get-AzStorageBlob -Container '$logs' |  
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
|`<service-name>`|Depolama hizmetinin adı. Örneğin: `blob` , `table` , veya`queue`|
|`YYYY`|Günlük için dört basamaklı yıl. Örnek: `2011`|
|`MM`|Günlüğün iki basamaklı ayı. Örnek: `07`|
|`DD`|Günlüğün iki basamaklı günü. Örnek: `31`|
|`hh`|24 saat UTC biçiminde Günlükler için başlangıç saatini gösteren iki basamaklı saat. Örnek: `18`|
|`mm`|Günlükler için başlangıç dakikasını gösteren iki basamaklı sayı. **Note:**  Bu değer, geçerli Depolama Analizi sürümünde desteklenmez ve değeri her zaman olur `00` .|
|`<counter>`|Bir saatlik zaman diliminde depolama hizmeti için oluşturulan günlük bloblarının sayısını belirten altı basamaklı sıfır tabanlı bir sayaç. Bu sayaç tarihinde başlar `000000` . Örnek: `000001`|

 Aşağıda, Yukarıdaki örnekleri birleştiren bir örnek günlük adı verilmiştir:

 `blob/2011/07/31/1800/000001.log`

 Aşağıda, yukarıdaki günlüğe erişmek için kullanılabilecek bir örnek URI verilmiştir:

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 Bir depolama isteği günlüğe kaydedildiğinde, sonuçta elde edilen günlük adı, istenen işlem tamamlandığında saat ile ilişkili olur. Örneğin, 7/31/2011 tarihinde bir GetBlob isteği tamamlanırsa, günlük şu önek ile yazılır:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Günlük meta verileri

 Tüm günlük Blobları, blob 'un içerdiği günlüğe kaydetme verilerini belirlemek için kullanılabilecek meta verilerle depolanır. Aşağıdaki tabloda her meta veri özniteliği açıklanmaktadır:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`LogType`|Günlük okuma, yazma veya silme işlemleriyle ilgili bilgiler içerip içermediğini açıklar. Bu değer, virgülle ayrılmış olarak bir tür ya da üç tane birleşimi içerebilir.<br /><br /> Örnek 1:`write`<br /><br /> Örnek 2:`read,write`<br /><br /> Örnek 3:`read,write,delete`|
|`StartTime`|Günlükteki girdinin en erken saati, biçiminde `YYYY-MM-DDThh:mm:ssZ` . Örnek: `2011-07-31T18:21:46Z`|
|`EndTime`|Günlükteki bir girişin en son saati, biçiminde `YYYY-MM-DDThh:mm:ssZ` . Örnek: `2011-07-31T18:22:09Z`|
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

 Geçerli ayarları almak için **Get-AzStorageServiceLoggingProperty** cmdlet 'ini ve geçerli ayarları değiştirmek için **set-AzStorageServiceLoggingProperty** cmdlet 'ini Azure PowerShell kullanarak depolama hesabınızda depolama günlüğü yapılandırmak için yerel makinenizde PowerShell kullanabilirsiniz.  

 Depolama günlüğünü denetleyen cmdlet 'ler, günlüğe kaydedilecek istek türleri için virgülle ayrılmış bir liste içeren bir **Loggingoperations** parametresi kullanır. Olası üç istek türü **okuma**, **yazma**ve **silme**. Günlüğe kaydetmeyi devre dışı bırakmak için, **Loggingoperations** parametresi için **none** değerini kullanın.  

 Aşağıdaki komut, bekletme için beş güne ayarlanmış varsayılan depolama hesabınızdaki Kuyruk hizmeti okuma, yazma ve silme istekleri için günlüğe kaydetme yapar:  

```powershell
Set-AzStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5  
```  

 Aşağıdaki komut, varsayılan depolama hesabınızda tablo hizmeti için günlüğe kaydetmeyi kapatır:  

```powershell
Set-AzStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none  
```  

 Azure PowerShell cmdlet 'lerinin Azure aboneliğinizle çalışacak şekilde nasıl yapılandırılacağı ve kullanılacak varsayılan depolama hesabını nasıl seçeceksiniz hakkında bilgi için bkz.: [Azure PowerShell nasıl yüklenir ve yapılandırılır](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

### <a name="enable-storage-logging-programmatically"></a>Depolama günlüğünü programlı olarak etkinleştir  

 Depolama günlüğünü denetlemek için Azure portal veya Azure PowerShell cmdlet 'lerini kullanmanın yanı sıra Azure Storage API 'Lerinden birini de kullanabilirsiniz. Örneğin, bir .NET dili kullanıyorsanız, depolama Istemci kitaplığını kullanabilirsiniz.  

# <a name="net-v12-sdk"></a>[\.NET V12 SDK](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/Monitoring.cs" id="snippet_EnableDiagnosticLogs":::

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

---


 Depolama günlüğünü yapılandırmak üzere .NET dili kullanma hakkında daha fazla bilgi için bkz. [depolama Istemci kitaplığı başvurusu](https://msdn.microsoft.com/library/azure/dn261237.aspx).  

 REST API kullanarak depolama günlüğünü yapılandırma hakkında genel bilgi için, bkz. [depolama Analizi etkinleştirme ve yapılandırma](https://msdn.microsoft.com/library/azure/hh360996.aspx).  

## <a name="download-storage-logging-log-data"></a>Depolama günlüğü günlük verilerini indirin

 Günlük verilerinizi görüntülemek ve analiz etmek için ilgilendiğiniz günlük verilerini içeren Blobları yerel bir makineye indirmeniz gerekir. Birçok depolama Tarama Aracı, depolama hesabınızdan blob 'ları indirmelerini sağlar; Günlük verilerinizi indirmek için Azure depolama ekibi tarafından sunulan komut satırı Azure kopyalama aracı [AzCopy](storage-use-azcopy-v10.md) ' i de kullanabilirsiniz.  
 
>[!NOTE]
> `$logs`Kapsayıcı Event Grid tümleştirmemişse, günlük dosyaları yazıldığında bildirim almazsınız. 

 İlgilendiğiniz günlük verilerini indirdiğinizden ve aynı günlük verilerinin birden çok kez indirilmesinden kaçınmak için:  

-   Aynı verileri birden çok kez yeniden indirmeyi önlemek için, analiz için zaten indirdiğiniz blob 'ları izlemek üzere günlük verilerini içeren Blobların tarih ve saat adlandırma kuralını kullanın.  

-   Blob 'un, indirme için gereken tam blobu tanımlamak üzere günlük verilerini tuttuğu belirli bir dönemi belirlemek için günlük verilerini içeren bloblarda bulunan meta verileri kullanın.  

AzCopy kullanmaya başlamak için bkz. [AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md) 

Aşağıdaki örnek, 15 Mayıs, 10:00 ve 11 Mayıs 2014 Mayıs 'ta başlayan saat için kuyruk hizmeti günlük verilerini nasıl indirebileceğiniz gösterilmektedir.

```
azcopy copy 'https://mystorageaccount.blob.core.windows.net/$logs/queue' 'C:\Logs\Storage' --include-path '2014/05/20/09;2014/05/20/10;2014/05/20/11' --recursive
```

Belirli dosyaları indirme hakkında daha fazla bilgi edinmek için bkz. [belirli dosyaları indirme](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-blobs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#download-specific-files).

Günlük verilerinizi indirdiyseniz, dosyalardaki günlük girişlerini görüntüleyebilirsiniz. Bu günlük dosyaları, Microsoft Message Analyzer da dahil olmak üzere çok sayıda günlük okuma aracının ayrıştırabildiği sınırlı bir metin biçimini kullanır (daha fazla bilgi için bkz. kılavuz [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md)). Farklı araçların, günlük dosyalarınızın içeriğini biçimlendirmek, filtrelemek, sıralamak ve ad aramak için farklı özellikleri vardır. Depolama günlüğü günlük dosyası biçimi ve içeriği hakkında daha fazla bilgi için bkz. [depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) ve [depolama Analizi günlüğe kaydedilmiş Işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format)
* [Günlüğe kaydedilen Işlemleri ve durum Iletilerini Depolama Analizi](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Depolama Analizi ölçümleri (klasik)](storage-analytics-metrics.md)
