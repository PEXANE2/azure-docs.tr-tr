---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: 1c2196f1f834002b76dbea555b54a5162655ec1c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77205687"
---
## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Dosya local.settings.js, uygulama ayarlarını, bağlantı dizelerini ve yerel geliştirme araçları tarafından kullanılan ayarları depolar. local.settings.jsdosyadaki ayarlar yalnızca projeleri yerel olarak çalıştırırken kullanılır. Yerel ayarlar dosyası bu yapıya sahiptir:

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
| **`IsEncrypted`** | Bu ayar olarak ayarlandığında `true` , tüm değerler yerel makine anahtarıyla şifrelenir. Komutlarıyla birlikte kullanılır `func settings` . Varsayılan değer `false` . |
| **`Values`** | Bir proje yerel olarak çalışırken kullanılan uygulama ayarları ve bağlantı dizeleri dizisi. Bu anahtar-değer (dize-dize) çiftleri, gibi Azure 'daki işlev uygulamanızda uygulama ayarlarına karşılık gelir [`AzureWebJobsStorage`] . Birçok tetikleyici ve bağlamanın, `Connection` [BLOB depolama tetikleyicisi](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#configuration)gibi bir bağlantı dizesi uygulama ayarına başvuran bir özelliği vardır. Bu özellikler için dizide tanımlanmış bir uygulama ayarı gereklidir `Values` . <br/>[`AzureWebJobsStorage`], HTTP dışındaki Tetikleyiciler için gerekli bir uygulama ayarıdır. <br/>Işlevler çalışma zamanının 2. x ve üzeri sürümleri `FUNCTIONS_WORKER_RUNTIME` için, temel araçlar tarafından projeniz için oluşturulan [] ayarı gereklidir. <br/> [Azure Storage öykünücüsü](../articles/storage/common/storage-use-emulator.md) yerel olarak yüklüyse ve [`AzureWebJobsStorage`] olarak ayarlarsanız `UseDevelopmentStorage=true` , çekirdek araçlar öykünücüyü kullanır. Öykünücü geliştirme sırasında faydalıdır, ancak dağıtımdan önce gerçek bir depolama bağlantısı ile test etmeniz gerekir.<br/> Değerler dize olmalıdır ve JSON nesneleri veya dizileri olmamalıdır. Ayar adları iki nokta ( `:` ) veya çift alt çizgi () içeremez `__` . Bu karakterler çalışma zamanı tarafından ayrılmıştır.  |
| **`Host`** | Bu bölümdeki ayarlar, projeleri yerel olarak çalıştırdığınızda Işlevler ana bilgisayar işlemini özelleştirir. Bu ayarlar, Azure 'da projeleri çalıştırdığınızda da uygulanan ayarları host.jsfarklıdır. |
| **`LocalHttpPort`** | Yerel Işlevler Konağı (ve) çalıştırılırken kullanılan varsayılan bağlantı noktasını ayarlar `func host start` `func run` . `--port`Komut satırı seçeneği bu ayarın üzerine gelir. |
| **`CORS`** | , [Çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)için izin verilen kaynakları tanımlar. Kaynaklar, boşluk olmadan virgülle ayrılmış bir liste olarak sağlanır. () Joker değeri \* desteklenir ve bu, herhangi bir kaynaktan gelen isteklere izin verir. |
| **`CORSCredentials`** |  Olarak ayarlandığında `true` , isteklere izin verir `withCredentials` . |
| **`ConnectionStrings`** | Bir koleksiyon. İşlev bağlamalarınız tarafından kullanılan bağlantı dizeleri için bu koleksiyonu kullanmayın. Bu koleksiyon, genellikle `ConnectionStrings` [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx)gibi bir yapılandırma dosyasının bölümünden bağlantı dizelerini alan çerçeveler tarafından kullanılır. Bu nesnedeki bağlantı dizeleri [System. Data. SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)sağlayıcı türüyle ortama eklenir. Bu koleksiyondaki öğeler, diğer uygulama ayarlarıyla Azure 'da yayımlanmaz. Bu değerleri, `Connection strings` işlev uygulaması ayarlarınızın koleksiyonuna açıkça eklemeniz gerekir. [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx)İşlev kodunuzda bir oluşturuyorsanız, bağlantı dizesi değerini portaldaki **uygulama ayarları** ' nda diğer bağlantılarınız ile depolamanız gerekir. |

[AzureWebJobsStorage]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
