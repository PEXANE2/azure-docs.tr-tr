---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 8c577db3e9f2bff9e86c3a7c37274630f90dd680
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188575"
---
Depolama emülatörü, paylaşılan anahtar kimlik doğrulaması için tek bir sabit hesabı ve tanınmış bir kimlik doğrulama anahtarını destekler. Bu hesap ve anahtar, depolama emülatörüyle birlikte kullanılmasına izin verilen tek Paylaşılan Anahtar kimlik bilgileridir. Bunlar:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Depolama emülatörü tarafından desteklenen kimlik doğrulama anahtarı yalnızca istemci kimlik doğrulama kodunuzu işlevselliğini sınamak için tasarlanmıştır. Herhangi bir güvenlik amacına hizmet etmez. Üretim depolama hesabınızı ve anahtarınızı depolama emülatörüyle kullanamazsınız. Geliştirme hesabını üretim verileriyle birlikte kullanmamalısınız.
> 
> Depolama emülatörü yalnızca HTTP üzerinden bağlantıyı destekler. Ancak HTTPS, bir üretim Azure depolama hesabındaki kaynaklara erişmek için önerilen protokoldür.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Kısayol kullanarak emülatör hesabına bağlanma
Uygulamanızdan depolama emülatörüne bağlanmanın en kolay yolu, uygulamanızın yapılandırma dosyasında kısayola `UseDevelopmentStorage=true`başvuran bir bağlantı dizesini yapılandırmaktır. Aşağıda, bir *app.config* dosyasındaki depolama emülatörüne bağlantı dizesinin bir örneği verilmiştir: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Tanınmış hesap adını ve anahtarını kullanarak emülatör hesabına bağlanın
Emülatör hesap adı ve anahtarına başvuran bir bağlantı dizesi oluşturmak için, bağlantı dizesindeki emülatörden kullanmak istediğiniz hizmetlerin her biri için uç noktaları belirtmeniz gerekir. Bu, bağlantı dizesi bir üretim depolama hesabı için farklı emülatör uç noktaları, başvuruolacak şekilde gereklidir. Örneğin, bağlantı dizenizin değeri aşağıdaki gibi görünür:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Bu değer, yukarıda gösterilen kısayol `UseDevelopmentStorage=true`ile aynıdır.

#### <a name="specify-an-http-proxy"></a>BIR HTTP proxy belirtin
Ayrıca, hizmetinizi depolama emülatörüne karşı test ederken kullanmak üzere bir HTTP proxy'si de belirtebilirsiniz. Bu, depolama hizmetlerine karşı işlemleri hata ayıklarken HTTP isteklerini ve yanıtlarını gözlemlemek için yararlı olabilir. Proxy belirtmek için bağlantı `DevelopmentStorageProxyUri` dizesine seçeneği ekleyin ve değerini proxy URI'ye ayarlayın. Örneğin, burada depolama emülatörü işaret eden ve bir HTTP proxy yapılandırAn bir bağlantı dizesi:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

