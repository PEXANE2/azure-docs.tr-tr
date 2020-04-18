---
title: Tutorial - Blob depolama ile çok kullanılabilir bir uygulama oluşturun
titleSuffix: Azure Storage
description: Uygulama verilerinizi yüksek oranda kullanılabilir hale getirmek için okuma erişimi coğrafi yedekli depolama alanını kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: tamram
ms.reviewer: artek
ms.custom: mvc
ms.subservice: blobs
ms.openlocfilehash: 27f90edf84fd51e5c13bc082cfaba50e26c54780
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606019"
---
# <a name="tutorial-build-a-highly-available-application-with-blob-storage"></a>Öğretici: Blob depolama ile son derece kullanılabilir bir uygulama oluşturun

Bu öğretici, bir dizinin birinci bölümüdür. Bu oyunda, uygulama verilerinizi Azure'da nasıl yüksek oranda kullanılabilir hale getirebilirsiniz.

Bu öğreticiyi tamamladığınızda, [okuma erişimi coğrafi yedekli](../common/storage-redundancy.md) (RA-GRS) depolama hesabından bir blob yükleyen ve alan bir konsol uygulamanız olacaktır.

RA-GRS, hareketleri birincil bölgeden ikincil bir bölgeye kopyalayarak çalışır. Bu çoğaltma işlemi, ikincil bölgedeki verilerin nihai olarak tutarlı olmasını sağlar. Uygulama, hangi uç noktaya bağlanışla bağlanış, hatalar ve kurtarmalar simüle edildikçe uç noktalar arasında otomatik olarak geçiş yapmak için [Devre Kesici](/azure/architecture/patterns/circuit-breaker) deseni kullanır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Bağlantı dizesini ayarlama
> * Konsol uygulamasını çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

# <a name="net"></a>[.NET](#tab/dotnet)

* **Azure geliştirme** iş yüküyle Visual [Studio 2019'u](https://www.visualstudio.com/downloads/) yükleyin.

  ![Azure geliştirme (Web ve Bulut altında)](media/storage-create-geo-redundant-storage/workloads.png)

# <a name="python"></a>[Python](#tab/python)

* [Python'u](https://www.python.org/downloads/) Yükle
* [Python için Azure Depolama SDK'sını](https://github.com/Azure/azure-storage-python) indirin ve yükleyin

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

* [Node.js](https://nodejs.org)yükleyin.

---

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Depolama hesabı, Azure Depolama veri nesnelerinizin depolanması ve erişimi için benzersiz bir ad alanı sağlar.

Okuma erişimli coğrafi olarak yedekli depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesini seçin.
2. **Yeni** sayfadan **Depolama'yı** seçin.
3. **Depolama**hesabı seçin **- blob, dosya, tablo, Özellikli** altında kuyruk .
4. Aşağıdaki bilgileri kullanarak depolama hesabı formunu alttaki resimde gösterildiği gibi doldurun ve **Oluştur**’u seçin:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Adı** | mystorageaccount | Depolama hesabınız için benzersiz bir değer |
   | **Dağıtım modeli** | Resource Manager  | Resource Manager en son özellikleri içerir.|
   | **Hesap türü** | StorageV2 | Hesap türleri hakkında ayrıntılı bilgi almak için bkz. [depolama hesabı türleri](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Performans** | Standart | Standart, örnek senaryo için yeterli olacaktır. |
   | **Çoğaltma**| Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) | Örneğin çalışması için bunun seçilmesi gereklidir. |
   |**Abonelik** | aboneliğiniz |Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions). |
   |**Kaynak Grubu** | myResourceGroup |Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). |
   |**Konum** | Doğu ABD | Konum seçin. |

![depolama hesabı oluşturma](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Örneği indirme

# <a name="net"></a>[.NET](#tab/dotnet)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje bir konsol uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="python"></a>[Python](#tab/python)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje temel bir Python uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-node-v10-ha-ra-grs) ve dosyanın zip'ini açın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje temel bir Düğüm.js uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-node-v10-ha-ra-grs
```

---

## <a name="configure-the-sample"></a>Örneği yapılandırma

# <a name="net"></a>[.NET](#tab/dotnet)

Bu uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Bu bağlantı dizesini uygulamayı çalıştıran yerel makine üzerindeki bir ortam değişkeninde depolayabilirsiniz. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. Bağlantı dizenizi \<\> gerçek bağlantı dizenizle değiştirerek işletim sisteminizi temel alan aşağıdaki komutlardan birini çalıştırın. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Windows'da, kullandığınız **Komut İstemi** veya kabuğunu yeniden yükleyene kadar ortam değişkeni kullanılamaz.

### <a name="linux"></a>Linux

```
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```powershell
setx storageconnectionstring "<yourconnectionstring>"
```

# <a name="python"></a>[Python](#tab/python)

Uygulamada, depolama hesabı kimlik bilgilerinizi sağlamanız gerekir. Bu bilgileri uygulamayı çalıştıran yerel makinede ortam değişkenlerinde depolayabilirsiniz. Ortam değişkenlerini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. **Depo hesap adını** ve **Anahtar** değerlerini aşağıdaki komutlara yapıştırarak hesap\> \<adınızı ve\> hesap anahtar yer \<tutucularınızı değiştirin. Bu komut, ortam değişkenlerini yerel makineye kaydeder. Windows'da, kullandığınız **Komut İstemi** veya kabuğunu yeniden yükleyene kadar ortam değişkeni kullanılamaz.

### <a name="linux"></a>Linux

```
export accountname=<youraccountname>
export accountkey=<youraccountkey>
```

### <a name="windows"></a>Windows

```powershell
setx accountname "<youraccountname>"
setx accountkey "<youraccountkey>"
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Bu örneği çalıştırmak `.env.example` için, depolama hesabı kimlik bilgilerinizi dosyaya eklemeniz ve sonra da ''ya `.env`yeniden adlandırmanız gerekir.

```
AZURE_STORAGE_ACCOUNT_NAME=<replace with your storage account name>
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=<replace with your storage account access key>
```

Bu bilgileri, depolama hesabınızda gezinerek ve **Ayarlar** bölümünde **Erişim tuşlarını** seçerek Azure portalında bulabilirsiniz.

Gerekli bağımlılıkları yükleyin. Bunu yapmak için bir komut istemi açın, örnek `npm install`klasöre gidin ve sonra girin.

---

## <a name="run-the-console-application"></a>Konsol uygulamasını çalıştırma

# <a name="net"></a>[.NET](#tab/dotnet)

Visual Studio'da, uygulamayı hata ayıklamaya başlamak için **F5** tuşuna basın veya **Başlat'ı** seçin. Visual studio, yapılandırılırsa eksik NuGet paketlerini otomatik olarak geri yükler, daha fazla bilgi edinmek için [paket geri yükleme ve yeniden yükleme paketlerini](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) ziyaret edin.

Bir konsol penceresi açılır ve uygulama çalışmaya başlar. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma bir **P** veya **S**ile temsil edilir. **P** birincil bitiş noktasını, **S'nin** ise ikincil bitiş noktasını temsil ettiği yer.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync) yöntemini kullanarak depolama hesabından bir resim indirmek için `Program.cs` dosyasındaki `RunCircuitBreakerAsync` görevi kullanılmaktadır. İndirmeden önce bir [OperationContext](/dotnet/api/microsoft.azure.cosmos.table.operationcontext) tanımlanır. İşlem bağlamı, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="python"></a>[Python](#tab/python)

Uygulamayı bir terminalde veya komut isteminde çalıştırmak için **circuitbreaker.py** dizinine gidip `python circuitbreaker.py` komutunu girin. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, resmin ikincil RA-GRS uç noktasında çoğaltıldığını denetler. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma bir **P** veya **S**ile temsil edilir. **P** birincil bitiş noktasını, **S'nin** ise ikincil bitiş noktasını temsil ettiği yer.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [get_blob_to_path](https://docs.microsoft.com/python/api/azure-storage-blob/azure.storage.blob.baseblobservice.baseblobservice?view=azure-python-previous#get-blob-to-path-container-name--blob-name--file-path--open-mode--wb---snapshot-none--start-range-none--end-range-none--validate-content-false--progress-callback-none--max-connections-2--lease-id-none--if-modified-since-none--if-unmodified-since-none--if-match-none--if-none-match-none--timeout-none-) yöntemini kullanarak depolama hesabından bir resim indirmek için `circuitbreaker.py` dosyasındaki `run_circuit_breaker` yöntemi kullanılmaktadır.

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesine ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve isteği yeniden denemeden önce kaç saniye bekleneceğini belirtir. Birincile yapılan istek başarısız olursa aynı isteğin ikincile yeniden denenmesini istiyorsanız **retry\_to\_secondary** değerini true olarak ayarlayın. Örnek uygulamada depolama nesnesinin `retry_callback` işlevinde özel bir yeniden deneme ilkesi tanımlanmıştır.

İndirmeden önce, Hizmet nesnesi [retry_callback](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.storageclient.storageclient?view=azure-python) ve [response_callback](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.storageclient.storageclient?view=azure-python) işlevi tanımlanır. Bu işlevler, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Örneği çalıştırmak için bir komut istemi açın, örnek klasöre gidin ve sonra girin. `node index.js`

Örnek Blob depolama hesabınızda bir kapsayıcı oluşturur, **HelloWorld.png'yi** kapsayıcıya yükler, ardından kapsayıcının ve resmin ikincil bölgeye çoğaltılıp çoğaltılmadığını tekrar tekrar denetler. Çoğaltmadan sonra, d **veya** **Q** (ardından ENTER) girmek veya çıkmak ister. Çıktınız aşağıdaki örneğe benzer olmalıdır:

```
Created container successfully: newcontainer1550799840726
Uploaded blob: HelloWorld.png
Checking to see if container and blob have replicated to secondary region.
[0] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[1] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
...
[31] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[32] Container found, but blob has not replicated to secondary region yet.
...
[67] Container found, but blob has not replicated to secondary region yet.
[68] Blob has replicated to secondary region.
Ready for blob download. Enter (D) to download or (Q) to quit, followed by ENTER.
> D
Attempting to download blob...
Blob downloaded from primary endpoint.
> Q
Exiting...
Deleted container newcontainer1550799840726
```

---

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

### <a name="net"></a>[.NET](#tab/dotnet)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`OperationContextRetrying` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) değeri `SecondaryOnly` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>İstek tamamlandı olay işleyicisi

`OperationContextRequestCompleted` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) değerini yeniden `PrimaryThenSecondary` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

### <a name="python"></a>[Python](#tab/python)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`retry_callback` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.models.locationmode?view=azure-python) değeri `SECONDARY` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write(
        "\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>İstek tamamlandı olay işleyicisi

`response_callback` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.models.locationmode?view=azure-python) değerini yeniden `PRIMARY` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

### <a name="nodejs"></a>[Node.js](#tab/nodejs)

Node.js V10 SDK ile geri arama işleyicileri gereksizdir. Bunun yerine, örnek yeniden deneme seçenekleri ve ikincil bir bitiş noktası ile yapılandırılan bir ardışık yapılandırır. Bu, birincil ardışık işlem deverinize ulaşamazsa uygulamanın otomatik olarak ikincil ardışık ardışık boru hattına geçmesini sağlar.

```javascript
const accountName = process.env.AZURE_STORAGE_ACCOUNT_NAME;
const storageAccessKey = process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY;
const sharedKeyCredential = new SharedKeyCredential(accountName, storageAccessKey);

const primaryAccountURL = `https://${accountName}.blob.core.windows.net`;
const secondaryAccountURL = `https://${accountName}-secondary.blob.core.windows.net`;

const pipeline = StorageURL.newPipeline(sharedKeyCredential, {
  retryOptions: {
    maxTries: 3,
    tryTimeoutInMs: 10000,
    retryDelayInMs: 500,
    maxRetryDelayInMs: 1000,
    secondaryHost: secondaryAccountURL
  }
});
```

---

## <a name="next-steps"></a>Sonraki adımlar

Serinin birinci bölümünde, RA-GRS depolama hesapları ile yüksek kullanılabilir bir uygulama yapma hakkında öğrendim.

Bir hata simülasyonu yapıp uygulamanızı ikincil RA-GRS uç noktasını kullanmaya zorlamayı öğrenmek için serinin ikinci bölümüne geçin.

> [!div class="nextstepaction"]
> [Birincil bölgeden okumada bir hata simüle](storage-simulate-failure-ragrs-account-app.md)
