---
title: Azure Blob Depolama 'da özet akışı değiştirme (Önizleme) | Microsoft Dokümanlar
description: Azure Blob Depolama'da özet akışı günlüklerini değiştirme ve bunları nasıl kullanacağınız hakkında bilgi edinin.
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: ac111b06d578a0e9af8581ef2e8caeccfc4a291e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536896"
---
# <a name="change-feed-support-in-azure-blob-storage-preview"></a>Azure Blob Depolama'da özet akışı desteğini değiştirme (Önizleme)

Değişiklik akışının amacı, depolama hesabınızdaki blob'larda ve blob meta verilerinde meydana gelen tüm değişikliklerin işlem günlüklerini sağlamaktır. Değişiklik akışı, bu değişikliklerin **sıralı,** **garantili,** **dayanıklı,** **değişmez,** **salt okunur** günlük sağlar. İstemci uygulamaları bu günlükleri akış tasnus veya toplu iş modunda herhangi bir zamanda okuyabilir. Değişiklik akışı, Blob Depolama hesabınızda meydana gelen olayları düşük bir maliyetle işleyen verimli ve ölçeklenebilir çözümler oluşturmanıza olanak tanır.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

Değişiklik akışı, standart [blob fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/blobs/) maliyetiyle depolama hesabınızdaki özel bir kapta [blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) olarak depolanır. Bu dosyaların bekletme süresini gereksinimlerinize göre denetleyebilirsiniz (Geçerli sürümün [koşullarına](#conditions) bakın). Değişiklik [olayları, Apache Avro](https://avro.apache.org/docs/1.8.2/spec.html) biçim belirtiminde kayıtlar olarak değişiklik akışına eklenir: satır içi şema ile zengin veri yapıları sağlayan kompakt, hızlı, ikili biçim. Bu biçim Hadoop ekosistemi, Akış Analizi ve Azure Veri Fabrikası'nda yaygın olarak kullanılır.

Bu günlükleri eşit olarak, artımlı veya tam olarak işleyebilirsiniz. Herhangi bir sayıda istemci uygulaması, değişim akışını paralel olarak ve kendi hızlarında bağımsız olarak okuyabilir. [Apache Drill](https://drill.apache.org/docs/querying-avro-files/) veya [Apache Spark](https://spark.apache.org/docs/latest/sql-data-sources-avro.html) gibi analytics uygulamaları, günlükleri doğrudan Avro dosyaları olarak tüketebilir ve bu da bunları düşük maliyetle, yüksek bant genişliğiyle ve özel bir uygulama yazmak zorunda kalmadan işlemenizi sağlar.

Özet akışı desteğini değiştir, verileri değişen nesnelere göre işleyen senaryolar için uygundur. Örneğin, uygulamalar şunları yapabilir:

  - İkincil dizini güncelleştirin, önbellek, arama motoru veya diğer içerik yönetimi senaryolarıyla eşitleyin.
  
  - Akışlı bir şekilde veya toplu modda nesnelerinizde meydana gelen değişikliklere dayalı iş analitiği öngörülerini ve ölçümlerini ayıklayın.
  
  - Kurumsal veri yönetimi için güvenlik, uyumluluk veya istihbarat için, belirli bir süre boyunca nesnelerinizdeki değişiklikleri depolayın, denetleyin ve analiz edin.

  - Olağanüstü durum yönetimi veya uyumluluk için hesabınızdaki nesne durumunu yedeklemek, yansıtmak veya çoğaltmak için çözümler oluşturun.

  - Olayları değiştirmek için tepki veren veya oluşturulan veya değiştirilen nesneyi temel alan yürütmeleri zamanlayan bağlı uygulama ardışık lıkları oluşturun.

> [!NOTE]
> Değişiklik akışı, bir blob'da meydana gelen değişikliklerin dayanıklı, sıralı günlük modelini sağlar. Değişiklikler, değişiklik akışı günlüğünde birkaç dakika içinde yazılır ve kullanıma sunulmuştur. Uygulamanız olaylara bundan çok daha hızlı tepki verecekse, bunun yerine [Blob Depolama olaylarını](storage-blob-event-overview.md) kullanmayı düşünün. [Blob Depolama Etkinlikleri,](storage-blob-event-overview.md) Azure İşlevlerinizin veya uygulamalarınızın bir blob'da meydana gelen değişikliklere hızlı bir şekilde tepki göstermesini sağlayan gerçek zamanlı tek seferlik etkinlikler sağlar. 

## <a name="enable-and-disable-the-change-feed"></a>Değişiklik akışını etkinleştirme ve devre dışı

Değişiklikleri yakalamaya ve kaydetmeye başlamak için depolama hesabınızdaki değişiklik akışını etkinleştirmeniz gerekir. Değişiklikleri yakalamayı durdurmak için değişiklik akışını devre dışı bırakın. Portal veya Powershell'de Azure Kaynak Yöneticisi şablonlarını kullanarak değişiklikleri etkinleştirebilir ve devre dışı kullanabilirsiniz.

Değişiklik akışını etkinleştirdiğinizde aklınızda bulundurmanız gereken birkaç şey aşağıda verebilirsiniz.

- Her depolama hesabında blob hizmeti için yalnızca bir değişiklik akışı vardır ve **$blobchangefeed** kapsayıcıda saklanır.

- Oluşturma, Güncelleştirme ve Sil değişiklikleri yalnızca blob hizmet düzeyinde yakalanır.

- Değişiklik akışı, hesapta oluşan kullanılabilir tüm olaylar için *yapılan tüm* değişiklikleri yakalar. İstemci uygulamaları, olay türlerini gerektiği gibi filtreleyebilir. (Geçerli sürümün [koşullarına](#conditions) bakın).

- Yalnızca GPv2 ve Blob depolama hesapları Akış Değiştir'i etkinleştirebilir. Premium BlockBlobStorage hesapları ve hiyerarşik ad alanı etkin hesapları şu anda desteklenmez. GPv1 depolama hesapları desteklenmez, ancak kesinti olmadan GPv2'ye yükseltilebilir, daha fazla bilgi için [GPv2 depolama hesabına yükseltme](../common/storage-account-upgrade.md) konusuna bakın.

> [!IMPORTANT]
> Değişiklik akışı genel önizlemede dir ve **westcentralus** ve **westus2** bölgelerinde kullanılabilir. Bu makalenin [koşullar](#conditions) bölümüne bakın. Önizlemeye kaydolmak için bu makalenin [abonelik bölümünüzü kaydedin](#register) bölümüne bakın. Depolama hesaplarınızda değişiklik akışını etkinleştirebilmeniz için aboneliğinizi kaydetmeniz gerekir.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak depolama hesabınızdaki yayını değiştirme'yi etkinleştirin:

1. Azure [portalında](https://portal.azure.com/)depolama hesabınızı seçin. 

2. **Blob Hizmeti**altında **Veri Koruma** seçeneğine gidin.

3. **Blob değiştirme akışı** altında **Etkin'i** tıklatın

4. Veri Koruması ayarlarınızı onaylamak için **Kaydet** düğmesini seçin

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak beslemeyi değiştirmeyi etkinleştirin:

1. En son PowershellGet'i yükleyin.

   ```powershell
   Install-Module PowerShellGet –Repository PSGallery –Force
   ```

2. Powershell konsolu kapatın ve yeniden açın.

3. **Az.Storage** önizleme modüllerini yükleyin.

   ```powershell
   Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.8.1-preview –AllowPrerelease –AllowClobber –Force
   ```

4. `Connect-AzAccount` Komutla Azure aboneliğinizde oturum açın ve kimlik doğrulaması için ekrandaki yönergeleri izleyin.

   ```powershell
   Connect-AzAccount
   ```

5. Depolama hesabınız için akış değiştirmeyi etkinleştirin.

   ```powershell
   Update-AzStorageBlobServiceProperty -EnableChangeFeed $true
   ```

### <a name="template"></a>[Şablon](#tab/template)
Azure portalı üzerinden mevcut depolama hesabınızdaki akış değiştirme'yi etkinleştirmek için Azure Kaynak Yöneticisi şablonu kullanın:

1. Azure portalında **kaynak oluştur'u**seçin.

2. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.

3. **[Özel bir şablon dağıt'ı](https://portal.azure.com/#create/Microsoft.Template)** seçin ve ardından **düzenleyicide kendi şablonunuzu oluşturun'u**seçin.

4. Şablon düzenleyicisinde, aşağıdaki json'a yapıştırın. Yer `<accountName>` tutucuyu depolama hesabınızın adı ile değiştirin.

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "variables": {},
       "resources": [{
           "type": "Microsoft.Storage/storageAccounts/blobServices",
           "apiVersion": "2019-04-01",
           "name": "<accountName>/default",
           "properties": {
               "changeFeed": {
                   "enabled": true
               }
           } 
        }]
   }
   ```
    
5. **Kaydet** düğmesini seçin, hesabın kaynak grubunu belirtin ve ardından şablonu dağıtmak ve değişiklik akışını etkinleştirmek için **Satın Alma** düğmesini seçin.

---

## <a name="consume-the-change-feed"></a>Değişiklik akışını tüketin

Değişiklik akışı birkaç meta veri ve günlük dosyaları üretir. Bu dosyalar depolama hesabının **$blobchangefeed** kapsayıcısında bulunur. 

> [!NOTE]
> Geçerli sürümde, **$blobchangefeed** kapsayıcısı Azure Depolama Gezgini'nde veya Azure portalında görünmez. Şu anda ListContainers API'yi aradiğinizde $blobchangefeed kapsayıcıyı göremezsiniz ancak lekeleri görmek için doğrudan kapsayıcının üzerinde ListBlobs API'sini arayabilirsiniz.

İstemci uygulamalarınız, besleme işlemcisi SDK ile birlikte sağlanan blob change feed işlemci kitaplığını kullanarak değişiklik akışını tüketebilir. 

Bkz. [Azure Blob Depolama'daki akış günlüklerini değiştir.](storage-blob-change-feed-how-to.md)

## <a name="understand-change-feed-organization"></a>Değişiklik besleme kuruluşunu anlama

<a id="segment-index"></a>

### <a name="segments"></a>Kesimler

Değişiklik akışı, **saatlik** *bölümlerhalinde* düzenlenen ancak birkaç dakikada bir eklenen ve güncelleştirilen değişikliklerin günlüğüdür. Bu segmentler yalnızca o saatte meydana gelen blob değişiklik olayları olduğunda oluşturulur. Bu, istemci uygulamanızın tüm günlükte arama yapmak zorunda kalmadan belirli zaman aralıklarında gerçekleşen değişiklikleri tüketmesini sağlar. Daha fazla bilgi için [Teknik Özellikler'e](#specifications)bakın.

Değişiklik akışının kullanılabilir bir saatlik kesimi, bu kesimiçin değişiklik akışı dosyalarına giden yolları belirten bir bildirim dosyasında açıklanır. Sanal dizinin `$blobchangefeed/idx/segments/` listesi, zamana göre sıralanan bu segmentleri gösterir. Segmentin yolu, segmentin temsil ettiği saatlik zaman aralığının başlangıcını açıklar. Bu listeyi, ilginizi çeken günlüklerin kesimlerini filtrelemek için kullanabilirsiniz.

```text
Name                                                                    Blob Type    Blob Tier      Length  Content Type    
----------------------------------------------------------------------  -----------  -----------  --------  ----------------
$blobchangefeed/idx/segments/1601/01/01/0000/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/22/1810/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/22/1910/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/23/0110/meta.json                  BlockBlob                      584  application/json
```

> [!NOTE]
> Değişiklik `$blobchangefeed/idx/segments/1601/01/01/0000/meta.json` akışını etkinleştirdiğinizde otomatik olarak oluşturulur. Bu dosyayı güvenle yoksayabilirsiniz. Her zaman boş bir başlatma dosyasıdır. 

Segment bildirimi dosyası`meta.json`( ) `chunkFilePaths` özellikteki bu kesim için değişiklik akışı dosyalarının yolunu gösterir. Aşağıda, segment bildirimi dosyasının bir örneği verilmiştir.

```json
{
    "version": 0,
    "begin": "2019-02-22T18:10:00.000Z",
    "intervalSecs": 3600,
    "status": "Finalized",
    "config": {
        "version": 0,
        "configVersionEtag": "0x8d698f0fba563db",
        "numShards": 2,
        "recordsFormat": "avro",
        "formatSchemaVersion": 1,
        "shardDistFnVersion": 1
    },
    "chunkFilePaths": [
        "$blobchangefeed/log/00/2019/02/22/1810/",
        "$blobchangefeed/log/01/2019/02/22/1810/"
    ],
    "storageDiagnostics": {
        "version": 0,
        "lastModifiedTime": "2019-02-22T18:11:01.187Z",
        "data": {
            "aid": "55e507bf-8006-0000-00d9-ca346706b70c"
        }
    }
}
```

> [!NOTE]
> Kapsayıcı, `$blobchangefeed` yalnızca hesabınızdaki değişiklik akışı özelliğini etkinleştirdikten sonra görüntülenir. Kapsayıcıdaki lekeleri listeleyebilmeniz için değişiklik akışını etkinleştirdikten sonra birkaç dakika beklemeniz gerekir. 

<a id="log-files"></a>

### <a name="change-event-records"></a>Olay kayıtlarını değiştirme

Değişiklik akışı dosyaları bir dizi değişiklik olayı kaydı içerir. Her değişiklik olay kaydı, tek bir blob'a yapılan bir değişikliğe karşılık gelir. Kayıtlar seri hale getirilir ve [Apache Avro](https://avro.apache.org/docs/1.8.2/spec.html) format belirtimi kullanılarak dosyaya yazılır. Kayıtlar Avro dosya biçimi belirtimi kullanılarak okunabilir. Bu biçimde dosyaları işlemek için kullanılabilir birkaç kitaplık vardır.

Değişiklik akışı dosyaları `$blobchangefeed/log/` [ek blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs)olarak sanal dizinde saklanır. Her yolun altındaki ilk değişiklik `00000` akışı dosyasının dosya `00000.avro`adı olacaktır (Örneğin). Bu yola eklenen sonraki her günlük dosyasının adı 1 artacaktır `00001.avro`(Örneğin: ).

Json'a dönüştürülen değişiklik akışı dosyasından değişiklik olay kaydına bir örnek verilmiştir.

```json
{
     "schemaVersion": 1,
     "topic": "/subscriptions/dd40261b-437d-43d0-86cf-ef222b78fd15/resourceGroups/sadodd/providers/Microsoft.Storage/storageAccounts/mytestaccount",
     "subject": "/blobServices/default/containers/mytestcontainer/blobs/mytestblob",
     "eventType": "BlobCreated",
     "eventTime": "2019-02-22T18:12:01.079Z",
     "id": "55e5531f-8006-0000-00da-ca3467000000",
     "data": {
         "api": "PutBlob",
         "clientRequestId": "edf598f4-e501-4750-a3ba-9752bb22df39",
         "requestId": "00000000-0000-0000-0000-000000000000",
         "etag": "0x8D698F13DCB47F6",
         "contentType": "application/octet-stream",
         "contentLength": 128,
         "blobType": "BlockBlob",
         "url": "",
         "sequencer": "000000000000000100000000000000060000000000006d8a",
         "storageDiagnostics": {
             "bid": "11cda41c-13d8-49c9-b7b6-bc55c41b3e75",
             "seq": "(6,5614,28042,28038)",
             "sid": "591651bd-8eb3-c864-1001-fcd187be3efd"
         }
  }
}
```

Her özelliğin açıklaması [için, Blob Depolama için Azure Olay Izgara olay şemasına](https://docs.microsoft.com/azure/event-grid/event-schema-blob-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#event-properties)bakın.

> [!NOTE]
> Bir kesimiçin değişiklik akışı dosyaları, bir kesim oluşturulduktan sonra hemen görünmez. Gecikme süresi, değişiklik akışından birkaç dakika sonra gerçekleşen değişiklik akışının yayımlama gecikmesi normal aralığı içindedir.

<a id="specifications"></a>

## <a name="specifications"></a>Belirtimler

- Değişiklik olayları kayıtları yalnızca değişiklik akışına eklenir. Bu kayıtlar ekledikten sonra değişmez dir ve kayıt konumu kararlıdır. İstemci uygulamaları, değişiklik akışının okuma konumunda kendi denetim noktalarını koruyabilir.

- Değişiklik olay kayıtları, değişikliğin birkaç dakika sıcağısı içinde eklenir. İstemci uygulamaları, akış erişimi için eklenen kayıtları başka bir zamanda toplu olarak tüketmeyi seçebilir.

- Değişiklik olay kayıtları **blob başına**değişiklik sırasına göre sıralanır. Blobs'taki değişiklik sırası Azure Blob Depolama'da tanımlanmamıştır. Önceki segmentteki tüm değişiklikler sonraki segmentlerde herhangi bir değişiklikden önceyapılır.

- Değişiklik olay kayıtları [Apache Avro 1.8.2](https://avro.apache.org/docs/1.8.2/spec.html) biçim belirtimi kullanılarak günlük dosyasına seri hale getirilir.

- Olay kayıtlarını, `eventType` iç sistem `Control` kayıtlarının iç sistem kayıtları olduğu ve hesabınızdaki nesnelere yapılan bir değişikliği yansıtmadığı yerlerde değiştirin. Bu kayıtları güvenle yoksayabilirsiniz.

- Özellik çantasındaki `storageDiagnonstics` değerler yalnızca dahili kullanım içindir ve uygulamanız tarafından kullanılmak üzere tasarlanmaz. Uygulamalarınızın bu verilere bir sözleşme bağımlılığı olmamalıdır. Bu özellikleri güvenle yoksayabilirsiniz.

- Segment tarafından temsil edilen süre **yaklaşık** 15 dakikalık sınırlar ile. Bu nedenle, tüm kayıtların belirli bir süre içinde tüketilmesini sağlamak için, ardışık önceki ve sonraki saat segmentini tüketin.

- Her kesim, yayımlama `chunkFilePaths` veri akışını yönetmek için günlük akışının iç bölümlemesi nedeniyle farklı sayıda olabilir. Her `chunkFilePath` birinde günlük dosyaları birbirini dışlayan lekeler içerdiği garanti edilir ve yineleme sırasında blob başına değişiklik sırasını ihlal etmeden paralel olarak tüketilebilir ve işlenebilir.

- Segmentler durum olarak `Publishing` başlar. Kayıtların bölüme eklemi tamamlandıktan sonra, `Finalized`bu. `$blobchangefeed/meta/Segments.json` Dosyadaki `LastConsumable` özelliğin tarihinden sonra tarihli herhangi bir segmentteki dosyaları günlüğe kaydedin, başvurunuz tarafından tüketilmemelidir. Dosyadaki `LastConsumable` `$blobchangefeed/meta/Segments.json` özelliğin bir örneği aşağıda verilmiştir:

```json
{
    "version": 0,
    "lastConsumable": "2019-02-23T01:10:00.000Z",
    "storageDiagnostics": {
        "version": 0,
        "lastModifiedTime": "2019-02-23T02:24:00.556Z",
        "data": {
            "aid": "55e551e3-8006-0000-00da-ca346706bfe4",
            "lfz": "2019-02-22T19:10:00.000Z"
        }
    }
}

```

<a id="register"></a>

## <a name="register-your-subscription-preview"></a>Aboneliğinizi kaydedin (Önizleme)

Değişiklik akışı yalnızca genel önizlemede olduğundan, özelliği kullanmak için aboneliğinizi kaydetmeniz gerekir.

### <a name="register-by-using-powershell"></a>PowerShell'i kullanarak kaydolun

PowerShell konsolunda şu komutları çalıştırın:

```powershell
Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```
   
### <a name="register-by-using-azure-cli"></a>Azure CLI'yi kullanarak kaydolun

Azure Bulut Su Bulutu'nda şu komutları çalıştırın:

```azurecli
az feature register --namespace Microsoft.Storage --name Changefeed
az provider register --namespace 'Microsoft.Storage'
```

<a id="conditions"></a>

## <a name="conditions-and-known-issues-preview"></a>Koşullar ve bilinen sorunlar (Önizleme)

Bu bölümde, değişiklik akışının geçerli genel önizlemesinde bilinen sorunlar ve koşullar açıklanmaktadır. 
- Önizleme için, westcentralus veya westus2 bölgelerindeki depolama hesabınız için değişiklik akışını etkinleştirmeden önce [aboneliğinizi kaydetmeniz](#register) gerekir. 
- Değişiklik akışı yalnızca oluşturma, güncelleştirme, silme ve kopyalama işlemlerini yakalar. Meta veri güncelleştirmeleri şu anda önizlemede yakalanan değil.
- Herhangi bir değişiklik için olay kayıtlarını değiştir, değişiklik akışınızda birden fazla kez görünebilir.
- Üzerlerinde zamana dayalı bekletme ilkesi ayarlayarak değişiklik akışı günlüğü dosyalarının kullanım ömrünü henüz yönetemezsiniz ve lekeleri silemezsiniz 
- Günlük `url` dosyasının özelliği şu anda her zaman boş.
- segments.json dosyasının `LastConsumable` özelliği, değişiklik akışının tamamlediği ilk kesimi listelemiyor. Bu sorun yalnızca ilk kesim sonuçlandıktan sonra oluşur. İlk saatten sonraki tüm sonraki bölümler `LastConsumable` mülkte doğru bir şekilde yakalanır.
- ListContainers API'yi aradiğinizde şu anda **$blobchangefeed** kapsayıcıyı göremezsiniz ve kapsayıcı Azure portalında veya Depolama Gezgini'nde görünmüyor
- Daha önce bir [hesap başarısızmı](../common/storage-disaster-recovery-guidance.md) başlatan depolama hesaplarının günlük dosyasının görünmemesiyle ilgili sorunlar olabilir. Gelecekteki herhangi bir hesap başarısızları önizleme sırasında günlük dosyasını da etkileyebilir.

## <a name="faq"></a>SSS

### <a name="what-is-the-difference-between-change-feed-and-storage-analytics-logging"></a>Değişim akışı ile Depolama Analizi günlüğü arasındaki fark nedir?
Analytics günlüklerinde tüm işlemlerde başarılı ve başarısız isteklerle tüm okuma, yazma, listeleme ve silme işlemlerinin kayıtları vardır. Analytics günlükleri en iyi çabadır ve sipariş garantisi yoktur.

Değişiklik akışı, bozuk para oluşturma, değişiklik ve silme gibi başarılı mutasyonların veya hesabınızdaki değişikliklerin işlem günlüğü sağlayan bir çözümdür. Değişiklik akışı, blob başına başarılı değişiklikler sırasına göre kaydedilecek ve görüntülenecek tüm olayları garanti eder, böylece çok sayıda okuma işleminden veya başarısız isteklerden gelen gürültüyü filtrelemeniz gerekmez. Değişiklik akışı temelde belirli garantiler gerektiren uygulama geliştirme için tasarlanmış ve optimize edilebilmektedir.

### <a name="should-i-use-change-feed-or-storage-events"></a>Yayını değiştir veya Depolama olaylarını kullanmalı mıyım?
Akış değiştir ve [Blob depolama olayları](storage-blob-event-overview.md) aynı teslim güvenilirliği garantisi ile aynı bilgileri sağladığından, temel fark olay kayıtlarının gecikmesi, siparişi ve depolanması olduğu için her iki özellikten de yararlanabilirsiniz. Değişiklik akışı, değişiklikleri birkaç dakika içinde günlük kayıtları yayımlar ve aynı zamanda blob başına değişiklik işlemleri sırasını garanti eder. Depolama olayları gerçek zamanlı olarak itilir ve sipariş verilmeyebilir. Değişiklik akışı olayları, depolama hesabınızda, kendi tanımlı bekletme niz ile salt okunur kararlı günlükler olarak saklanırken, depolama olayları bunları açıkça depolamadığınız sürece olay işleyicisi tarafından tüketilecek geçicidir. Akış değiştir ile, herhangi bir sayıda uygulamanız blob API'lerini veya SDK'larını kullanarak günlükleri kendi kolaylıklarıyla tüketebilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Bir .NET istemci uygulaması kullanarak değişiklik akışını nasıl okuyeceğiniz hakkında bir örneğe bakın. Bkz. [Azure Blob Depolama'daki akış günlüklerini değiştir.](storage-blob-change-feed-how-to.md)
- Olaylara gerçek zamanlı olarak nasıl tepki vereceğinizi öğrenin. [Bkz. Blob Depolama olaylarına tepki](storage-blob-event-overview.md) verme
- Tüm istekler için hem başarılı hem de başarısız işlemler için ayrıntılı günlük bilgileri hakkında daha fazla bilgi edinin. Bkz. [Azure Depolama analitiği günlüğe kaydetme](../common/storage-analytics-logging.md)
