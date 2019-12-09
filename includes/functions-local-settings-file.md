---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: c505909599b6b69719de1cb9224db52d2f524b2b
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74935935"
---
## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Local. Settings. JSON dosyası, uygulama ayarlarını, bağlantı dizelerini ve yerel geliştirme araçları tarafından kullanılan ayarları depolar. Local. Settings. JSON dosyasındaki ayarlar yalnızca projeleri yerel olarak çalıştırırken kullanılır. Yerel ayarlar dosyası bu yapıya sahiptir:

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
| **`IsEncrypted`** | Bu ayar `true`olarak ayarlandığında, tüm değerler yerel makine anahtarıyla şifrelenir. `func settings` komutlarıyla birlikte kullanılır. Varsayılan değer `false`. |
| **`Values`** | Bir proje yerel olarak çalışırken kullanılan uygulama ayarları ve bağlantı dizeleri dizisi. Bu anahtar-değer (dize-dize) çiftleri, [`AzureWebJobsStorage`]gibi Azure 'daki işlev uygulamanızda uygulama ayarlarına karşılık gelir. Birçok tetikleyici ve bağlamanın, [BLOB depolama tetikleyicisi](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---configuration)için `Connection` gibi bir bağlantı dizesi uygulama ayarına başvuran bir özelliği vardır. Bu özellikler için `Values` dizisinde tanımlanmış bir uygulama ayarı gereklidir. <br/>[`AzureWebJobsStorage`] , http dışındaki Tetikleyiciler için gerekli bir uygulama ayarıdır. <br/>Işlevler çalışma zamanının 2. x ve üzeri sürümleri için, temel araçlar tarafından projeniz için oluşturulan [`FUNCTIONS_WORKER_RUNTIME`] ayarı gereklidir. <br/> [Azure Storage öykünücüsü](../articles/storage/common/storage-use-emulator.md) yerel olarak yüklüyse ve [`AzureWebJobsStorage`] `UseDevelopmentStorage=true`ayarlarsanız, çekirdek araçlar öykünücüyü kullanır. Öykünücü geliştirme sırasında faydalıdır, ancak dağıtımdan önce gerçek bir depolama bağlantısı ile test etmeniz gerekir.<br/> Değerler dize olmalıdır ve JSON nesneleri veya dizileri olmamalıdır. Ayar adları iki nokta (`:`) veya çift alt çizgi (`__`) içeremez. Bu karakterler çalışma zamanı tarafından ayrılmıştır.  |
| **`Host`** | Bu bölümdeki ayarlar, projeleri yerel olarak çalıştırdığınızda Işlevler ana bilgisayar işlemini özelleştirir. Bu ayarlar, Azure 'da projeler çalıştırdığınızda da uygulanan Host. JSON ayarlarından ayrıdır. |
| **`LocalHttpPort`** | Yerel Işlevler Konağı (`func host start` ve `func run`) çalıştırılırken kullanılan varsayılan bağlantı noktasını ayarlar. `--port` komut satırı seçeneği bu ayarın üzerine gelir. |
| **`CORS`** | , [Çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)için izin verilen kaynakları tanımlar. Kaynaklar, boşluk olmadan virgülle ayrılmış bir liste olarak sağlanır. Joker karakter değeri (\*) desteklenir ve bu, herhangi bir kaynaktan gelen isteklere izin verir. |
| **`CORSCredentials`** |  `true`olarak ayarlandığında `withCredentials` isteklere izin verir. |
| **`ConnectionStrings`** | Bir koleksiyon. İşlev bağlamalarınız tarafından kullanılan bağlantı dizeleri için bu koleksiyonu kullanmayın. Bu koleksiyon, genellikle [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx)gibi bir yapılandırma dosyasının `ConnectionStrings` bölümünden bağlantı dizelerini alan çerçeveler tarafından kullanılır. Bu nesnedeki bağlantı dizeleri [System. Data. SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)sağlayıcı türüyle ortama eklenir. Bu koleksiyondaki öğeler, diğer uygulama ayarlarıyla Azure 'da yayımlanmaz. Bu değerleri, işlev uygulaması ayarlarınızın `Connection strings` koleksiyonuna açıkça eklemeniz gerekir. İşlev kodunuzda bir [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) oluşturuyorsanız, bağlantı dizesi değerini portaldaki **uygulama ayarları** ' nda diğer bağlantılarınız ile depolamanız gerekir. |

[AzureWebJobsStorage]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
