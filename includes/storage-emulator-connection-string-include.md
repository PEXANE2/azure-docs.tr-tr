---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 8c577db3e9f2bff9e86c3a7c37274630f90dd680
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188575"
---
Depolama öykünücüsü, paylaşılan anahtar kimlik doğrulaması için tek bir sabit hesabı ve iyi bilinen bir kimlik doğrulama anahtarını destekler. Bu hesap ve anahtar, depolama öykünücüsüyle kullanılmasına izin verilen tek paylaşılan anahtar kimlik bilgileridir. Bunlar:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Depolama öykünücüsü tarafından desteklenen kimlik doğrulama anahtarı yalnızca istemci kimlik doğrulama kodunuzun işlevlerini test etmek için tasarlanmıştır. Herhangi bir güvenlik amacına sahip değildir. Depolama öykünücüsüyle üretim depolama hesabınızı ve anahtarınızı kullanamazsınız. Üretim verileriyle birlikte geliştirme hesabı kullanmamalısınız.
> 
> Depolama öykünücüsü yalnızca HTTP üzerinden bağlantıyı destekler. Ancak HTTPS, bir üretim Azure depolama hesabındaki kaynaklara erişmek için önerilen protokoldür.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Bir kısayol kullanarak öykünücü hesabına bağlanma
Uygulamanızdan depolama öykünücüye bağlanmanın en kolay yolu, uygulamanızın yapılandırma dosyasında kısayola `UseDevelopmentStorage=true`başvuran bir bağlantı dizesi yapılandırmaktır. Bir *app. config* dosyasında depolama öykünücüsüne yönelik bir bağlantı dizesi örneği aşağıda verilmiştir: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>İyi bilinen hesap adını ve anahtarını kullanarak öykünücü hesabına bağlanın
Öykünücü hesap adına ve anahtarına başvuran bir bağlantı dizesi oluşturmak için, bağlantı dizesindeki öykünücüsünde kullanmak istediğiniz her bir hizmet için uç noktaları belirtmeniz gerekir. Bu, bağlantı dizesinin, bir üretim depolama hesabı için olandan farklı olan öykünücü uç noktalarına başvurması için gereklidir. Örneğin, bağlantı dizeniz değeri şöyle görünür:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Bu değer yukarıda gösterilen kısayolla aynıdır `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Bir HTTP proxy 'si belirtin
Ayrıca, hizmetinizi depolama öykünücüsüyle test ederken kullanılacak bir HTTP proxy de belirtebilirsiniz. Bu, depolama hizmetlerine karşı işlemlerden hata ayıklarken HTTP isteklerini ve yanıtlarını gözlemlemek için yararlı olabilir. Bir proxy belirtmek için, bağlantı dizesine `DevelopmentStorageProxyUri` seçeneği ekleyin ve değerini proxy URI 'si olarak ayarlayın. Örneğin, depolama öykünücüsünü işaret eden ve bir HTTP proxy yapılandıran bağlantı dizesi aşağıda verilmiştir:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

