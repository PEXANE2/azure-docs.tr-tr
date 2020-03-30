---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: 1c2196f1f834002b76dbea555b54a5162655ec1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77205687"
---
## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Local.settings.json dosyası, yerel geliştirme araçları tarafından kullanılan uygulama ayarlarını, bağlantı dizelerini ve ayarları saklar. Local.settings.json dosyasındaki ayarlar yalnızca projeleri yerel olarak çalıştırırken kullanılır. Yerel ayarlar dosyası şu yapıya sahiptir:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

Projeleri yerel olarak çalıştırdığınızda bu ayarlar desteklenir:

| Ayar      | Açıklama                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Bu ayar `true`ayarlandığında, tüm değerler yerel bir makine anahtarıyla şifrelenir. `func settings` Komutlarla kullanılır. Varsayılan değer. `false` |
| **`Values`** | Bir proje yerel olarak çalışırken kullanılan uygulama ayarları ve bağlantı dizeleri dizisi. Bu anahtar değeri (string-string) çiftleri, Azure'daki işlev uygulamanızdaki uygulama ayarlarına karşılık gelir. [`AzureWebJobsStorage`] Birçok tetikleyici ve bağlama, [Blob depolama tetikleyicisi](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#configuration)gibi `Connection` bir bağlantı dize uygulaması ayarı anlamına gelen bir özelliğe sahiptir. Bu özellikler için `Values` dizide tanımlanan bir uygulama ayarı gerekir. <br/>[`AzureWebJobsStorage`]HTTP dışındaki tetikleyiciler için gerekli bir uygulama ayarıdır. <br/>Sürüm 2.x ve daha yüksek Fonksiyonlar`FUNCTIONS_WORKER_RUNTIME`çalışma süresi Core Tools tarafından projeniz için oluşturulan [ ] ayarı gerektirir. <br/> Azure depolama [emülatörü](../articles/storage/common/storage-use-emulator.md) yerel olarak yüklendiğinde [`AzureWebJobsStorage`] ve `UseDevelopmentStorage=true`"Core Tools emülatör"u kullanır. Emülatör geliştirme sırasında yararlıdır, ancak dağıtımdan önce gerçek bir depolama bağlantısıyla sınamalısınız.<br/> Değerler, JSON nesneleri veya dizileri değil, dizeleri olmalıdır. Adları ayarlama, iki nokta`:`üst üste (`__`) veya çift altı çizili ( ) içeremez. Bu karakterler çalışma süresine göre ayrılmıştır.  |
| **`Host`** | Bu bölümdeki ayarlar, projeleri yerel olarak çalıştırdığınızda İşcüller ana bilgisayar işlemini özelleştirin. Bu ayarlar, Azure'da proje çalıştırdığınızda da geçerli olan ana bilgisayar.json ayarlarından ayrıdır. |
| **`LocalHttpPort`** | Yerel İşlevler ana bilgisayar`func host start` (ve) `func run`çalıştırırken kullanılan varsayılan bağlantı noktasını ayarlar. `--port` Komut satırı seçeneği bu ayarda önceliklidir. |
| **`CORS`** | [Orijinler arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)için izin verilen kökenleri tanımlar. Kökenleri boşluk olmadan virgülden ayrılmış bir liste olarak verilir. Joker karakter değeri\*( ) desteklenir, bu da herhangi bir kaynaktan gelen isteklere izin verir. |
| **`CORSCredentials`** |  `true`Ayarlandığında, istekleri `withCredentials` sağlar. |
| **`ConnectionStrings`** | Bir koleksiyon. Bu koleksiyonu işlev bağlamalarınız tarafından kullanılan bağlantı dizeleri için kullanmayın. Bu koleksiyon [yalnızca, Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx)gibi bir yapılandırma `ConnectionStrings` dosyasının bölümünden bağlantı dizeleri alan çerçeveler tarafından kullanılır. Bu nesnedeki bağlantı dizeleri [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)sağlayıcı türü ile ortama eklenir. Bu koleksiyondaki öğeler diğer uygulama ayarlarıyla birlikte Azure'da yayınlanmaz. Bu değerleri işlev uygulama ayarlarınızın koleksiyonuna `Connection strings` açıkça eklemeniz gerekir. İşlev kodunuzda [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) bir kod oluşturuyorsanız, bağlantı dize değerini diğer bağlantılarınızla birlikte portaldaki **Uygulama Ayarları'nda** depolamanız gerekir. |

['AzureWebJobsStorage']: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
