---
title: Azure Analiz Hizmetleri modelleri için eşiz yenileme | Microsoft Dokümanlar
description: Model verilerinin eşzamanlı yenilenmesini kodlamak için Azure Analiz Hizmetleri REST API'nin nasıl kullanılacağını açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6457f062a40e60a491220fcf977585e8b07445b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273712"
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>REST API ile zaman uyumsuz yenileme

REST çağrılarını destekleyen herhangi bir programlama dilini kullanarak, Azure Çözümleme Hizmetleri eşzamanlı modellerinizde eşzamanlı veri yenileme işlemleri gerçekleştirebilirsiniz. Bu, sorgu ölçeklendirmesi için salt okunur yinelemelerin eşitleştirilmesini içerir. 

Veri yenileme işlemleri, veri hacmi, bölümleri kullanarak optimizasyon düzeyi, vb dahil olmak üzere bir dizi faktöre bağlı olarak biraz zaman alabilir. Bu işlemler geleneksel olarak [TOM](https://docs.microsoft.com/analysis-services/tom/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (Tabular Object Model), [PowerShell](https://docs.microsoft.com/analysis-services/powershell/analysis-services-powershell-reference) cmdlets veya [TMSL](https://docs.microsoft.com/analysis-services/tmsl/tabular-model-scripting-language-tmsl-reference) (Tabular Model Scripting Language) kullanma gibi varolan yöntemlerle çağrılmıştır. Ancak, bu yöntemler genellikle güvenilmez, uzun süreli HTTP bağlantıları gerektirebilir.

Azure Çözümleme Hizmetleri için REST API'sı veri yenileme işlemlerinin eş zamanlı olarak gerçekleştirilmesini sağlar. REST API'yi kullanarak, istemci uygulamalarından uzun süreli HTTP bağlantıları gerekmez. Otomatik yeniden denemeler ve toplu işlemeler gibi güvenilirlik için başka yerleşik özellikler de vardır.

## <a name="base-url"></a>Temel URL

Temel URL şu biçimi izler:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Örneğin, Batı ABD Azure bölgesinde bulunan `myserver`, adlı sunucuda AdventureWorks adlı bir model düşünün. Sunucu adı:

```
asazure://westus.asazure.windows.net/myserver 
```

Bu sunucu adının temel URL'si:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

Temel URL kullanılarak, kaynaklar ve işlemler aşağıdaki parametrelere göre eklenebilir: 

![Async yenileme](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- **S** ile biten her şey bir koleksiyondur.
- **()** ile biten her şey bir fonksiyondur.
- Başka bir şey bir kaynak / nesnedir.

Örneğin, Yenileme ler koleksiyonundaki POST fiilini bir yenileme işlemi gerçekleştirmek için kullanabilirsiniz:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Kimlik doğrulaması

Tüm aramalar, Yetkilendirme üstbilgisinde geçerli bir Azure Etkin Dizini (OAuth 2) belirteciyle doğrulanmalıdır ve aşağıdaki gereksinimleri karşılamalıdır:

- Belirteç, kullanıcı belirteci veya uygulama hizmeti ilkesi olmalıdır.
- Belirteç, doğru hedef kitleyi `https://*.asazure.windows.net`.
- Kullanıcı veya uygulama, istenen aramayı yapmak için sunucu veya modelde yeterli izinlere sahip olmalıdır. İzin düzeyi, modeldeki veya sunucudaki yönetici grubundaki rollere göre belirlenir.

    > [!IMPORTANT]
    > Şu anda, **sunucu yöneticisi** rol izinleri gereklidir.

## <a name="post-refreshes"></a>POST /yenileme

Yenileme işlemi gerçekleştirmek için, koleksiyona yeni bir yenileme öğesi eklemek için /refreshes koleksiyonundaki POST fiilini kullanın. Yanıttaki Konum üstbilgisi, yenileme kimliğini içerir. İstemci uygulaması bağlantısını kesebilir ve eşsenkron olduğu için gerekirse durumu daha sonra denetleyebilir.

Bir model için aynı anda yalnızca bir yenileme işlemi kabul edilir. Geçerli bir çalışan yenileme işlemi varsa ve başka bir işlem gönderilirse, 409 Çakışma HTTP durum kodu döndürülür.

Vücut aşağıdakigibi olabilir:

```
{
    "Type": "Full",
    "CommitMode": "transactional",
    "MaxParallelism": 2,
    "RetryCount": 2,
    "Objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer"
        },
        {
            "table": "DimDate"
        }
    ]
}
```

### <a name="parameters"></a>Parametreler

Parametrelerin belirtilmesi gerekli değildir. Varsayılan uygulanır.

| Adı             | Tür  | Açıklama  |Varsayılan  |
|------------------|-------|--------------|---------|
| `Type`           | Sabit Listesi  | Gerçekleştirecek işlem türü. Türler TMSL [yenileme komut](https://docs.microsoft.com/analysis-services/tmsl/refresh-command-tmsl) türleri ile hizalanır: tam, net Değerler, hesaplamak, dataOnly, otomatik ve parçayı ayırın. Ekle türü desteklenmez.      |   otomatik      |
| `CommitMode`     | Sabit Listesi  | Nesnelerin toplu olarak mı yoksa tamamlandığında mı işlendiğini belirler. Modlar şunlardır: varsayılan, işlemsel, partialBatch.  |  Işlem       |
| `MaxParallelism` | int   | Bu değer, işleme komutlarının paralel olarak çalıştırılabilmek için üzerinde bulunan en fazla iş parçacığı sayısını belirler. Bu değer, TMSL [Sequence komutunda](https://docs.microsoft.com/analysis-services/tmsl/sequence-command-tmsl) veya diğer yöntemler kullanılarak ayarlanabilen MaxParallelism özelliğiyle hizalanmıştır.       | 10        |
| `RetryCount`     | int   | Başarısız olmadan önce işlemin kaç kez yeniden deneceğini gösterir.      |     0    |
| `Objects`        | Dizi | İşlenecek bir dizi nesne. Her nesne içerir: "tablo" tüm tablo veya "tablo" ve "bölüm" bir bölüm işlerken işlerken. Nesne belirtilmemişse, tüm model yenilenir. |   Tüm modeli işleme      |

CommitMode partialBatch eşittir. Saatler alabilecek büyük veri kümelerinin ilk yükünü yüklerken kullanılır. Yenileme işlemi bir veya daha fazla toplu işlemi başarıyla işledikten sonra başarısız olursa, başarılı bir şekilde işlenen toplu işler kararlı kalır (başarılı bir şekilde işlenen toplu işlemleri geri almaz).

> [!NOTE]
> Yazma sırasında, toplu iş boyutu MaxParallelism değeridir, ancak bu değer değişebilir.

### <a name="status-values"></a>Durum değerleri

|Durum değeri  |Açıklama  |
|---------|---------|
|`notStarted`    |   Operasyon henüz başlamadı.      |
|`inProgress`     |   Operasyon devam ediyor.      |
|`timedOut`     |    Kullanıcının belirtilen zaman anına göre zaman doldu.     |
|`cancelled`     |   Kullanıcı veya sistem tarafından iptal edilen işlem.      |
|`failed`     |   İşlem başarısız oldu.      |
|`succeeded`      |   Operasyon başarılı oldu.      |

## <a name="get-refreshesrefreshid"></a>GET /refreshes/\<refreshId>

Yenileme işleminin durumunu kontrol etmek için, yenileme kimliğindeki GET fiilini kullanın. İşte yanıt gövdesinin bir örneği. İşlem devam ediyorsa, `inProgress` durum döndürülür.

```
{
    "startTime": "2017-12-07T02:06:57.1838734Z",
    "endTime": "2017-12-07T02:07:00.4929675Z",
    "type": "full",
    "status": "succeeded",
    "currentRefreshType": "full",
    "objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer",
            "status": "succeeded"
        },
        {
            "table": "DimDate",
            "partition": "DimDate",
            "status": "succeeded"
        }
    ]
}
```

## <a name="get-refreshes"></a>GET /yenileme

Bir model için geçmiş yenileme işlemlerinin listesini almak için /refreshes koleksiyonundaki GET fiilini kullanın. İşte yanıt gövdesinin bir örneği. 

> [!NOTE]
> Yazma sırasında, son 30 günlük yenileme işlemleri depolanır ve döndürülür, ancak bu sayı değişebilir.

```
[
    {
        "refreshId": "1344a272-7893-4afa-a4b3-3fb87222fdac",
        "startTime": "2017-12-09T01:58:04.76",
        "endTime": "2017-12-09T01:58:12.607",
        "status": "succeeded"
    },
    {
        "refreshId": "474fc5a0-3d69-4c5d-adb4-8a846fa5580b",
        "startTime": "2017-12-07T02:05:48.32",
        "endTime": "2017-12-07T02:05:54.913",
        "status": "succeeded"
    }
]
```

## <a name="delete-refreshesrefreshid"></a>DELETE /refreshes/\<refreshId>

Devam eden bir yenileme işlemini iptal etmek için, yenileme kimliğindeki DELETE fiilini kullanın.

## <a name="post-sync"></a>POST /senkronizasyon

Yenileme işlemleri gerçekleştirildikten sonra, sorgu ölçeklendirmesi için yeni verileri yinelemelerle eşitlemek gerekebilir. Bir model için eşitleme işlemi gerçekleştirmek için /sync işlevinde POST fiilini kullanın. Yanıttaki Konum üstbilgisi eşitleme işlemi kimliğini içerir.

## <a name="get-sync-status"></a>GET /sync durumu

Eşitleme işleminin durumunu denetlemek için, işlem kimliğini parametre olarak geçen GET fiilini kullanın. Yanıt gövdesinin bir örneği aşağıda verilmiştir:

```
{
    "operationId": "cd5e16c6-6d4e-4347-86a0-762bdf5b4875",
    "database": "AdventureWorks2",
    "UpdatedAt": "2017-12-09T02:44:26.18",
    "StartedAt": "2017-12-09T02:44:20.743",
    "syncstate": 2,
    "details": null
}
```

Değerler `syncstate`için :

- 0: Çoğaltma. Veritabanı dosyaları hedef klasöre çoğaltılır.
- 1: Rehydrating. Veritabanı salt okunur sunucu örneğinde (ler) yeniden sulandırılıyor.
- 2: Tamamlandı. Eşitleme işlemi başarıyla tamamlandı.
- 3: Başarısız oldu. Eşitleme işlemi başarısız oldu.
- 4: Sonuçlandırışuyor. Eşitleme işlemi tamamlandı, ancak temizleme adımları gerçekleştiriyor.

## <a name="code-sample"></a>Kod örneği

Burada başlamak için bir C # kodu örneği, [GitHub RestApiSample](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample)' s.

### <a name="to-use-the-code-sample"></a>Kod örneğini kullanmak için

1.  Kopyaya veya repo'yu indirin. RestApiSample çözümlerini açın.
2.  Hat **istemcisini bulun. BaseAddress = ...** ve [temel URL'nizi](#base-url)sağlayın.

Kod örneği [hizmet temel](#service-principal) kimlik doğrulaması kullanır.

### <a name="service-principal"></a>Hizmet sorumlusu

Bkz. [Hizmet yöneticisi oluştur - Azure portalı](../active-directory/develop/howto-create-service-principal-portal.md) ve Bir hizmet yöneticisinin nasıl ayarlanıp Azure AS'de gerekli izinleri atayacağım hakkında daha fazla bilgi için sunucu yöneticisi [rolüne bir hizmet yöneticisi](analysis-services-addservprinc-admins.md) ekleyin. Adımları tamamladıktan sonra aşağıdaki ek adımları tamamlayın:

1.  Kod örneğinde, **string yetkilisi = ... ,** kuruluşunuzun kiracı kimliğiyle **değiştirin.**
2.  Yorum/yorum suz böylece ClientCredential sınıf güven nesnesi anlık olarak kullanılır. \<App ID> \<ve App Key> değerlerine güvenli bir şekilde erişilmesini sağlayın veya hizmet ilkeleri için sertifika tabanlı kimlik doğrulamasını kullanın.
3.  Örnek uygulamayı çalıştırın.


## <a name="see-also"></a>Ayrıca bkz.

[Örnekleri](analysis-services-samples.md)   
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)   


