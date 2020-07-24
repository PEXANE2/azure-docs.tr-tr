---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/17/2020
ms.author: tamram
ms.openlocfilehash: 85e7cb86217340e77a6f597a357c3de1f91fb8d0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87070545"
---
Azurite, paylaşılan anahtar kimlik doğrulaması için tek bir sabit hesabı ve iyi bilinen bir kimlik doğrulama anahtarını destekler. Bu hesap ve anahtar, Azurıite ile kullanım için izin verilen tek paylaşılan anahtar kimlik bilgileridir. Bunlar:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Azurite tarafından desteklenen kimlik doğrulama anahtarı yalnızca istemci kimlik doğrulama kodunuzun işlevlerini test etmek üzere tasarlanmıştır. Herhangi bir güvenlik amacına sahip değildir. Üretim depolama hesabınızı ve anahtarınızı Azurite ile kullanamazsınız. Üretim verileriyle birlikte geliştirme hesabı kullanmamalısınız.
> 
> Azurite yalnızca HTTP üzerinden bağlantıyı destekler. Ancak HTTPS, bir üretim Azure depolama hesabındaki kaynaklara erişmek için önerilen protokoldür.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Bir kısayol kullanarak öykünücü hesabına bağlanma
Uygulamanızdan Azurite 'ya bağlanmanın en kolay yolu, uygulamanızın yapılandırma dosyasında kısayola başvuran bir bağlantı dizesi yapılandırmaktır `UseDevelopmentStorage=true` . Bir *app.config* dosyasında Azurıite için bir bağlantı dizesi örneği aşağıda verilmiştir: 

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
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Bu değer yukarıda gösterilen kısayolla aynıdır `UseDevelopmentStorage=true` .
