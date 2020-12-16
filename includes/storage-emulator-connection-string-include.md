---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/17/2020
ms.author: tamram
ms.openlocfilehash: 37fba0101365e425110c2943264c8c0e8c511329
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97582634"
---
Öykünücü, paylaşılan anahtar kimlik doğrulaması için tek bir sabit hesabı ve iyi bilinen bir kimlik doğrulama anahtarını destekler. Bu hesap ve anahtar, öykünücü ile kullanım için izin verilen tek paylaşılan anahtar kimlik bilgileridir. Bunlar:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Öykünücü tarafından desteklenen kimlik doğrulama anahtarı yalnızca istemci kimlik doğrulama kodunuzun işlevlerini test etmek üzere tasarlanmıştır. Herhangi bir güvenlik amacına sahip değildir. Üretim depolama hesabınızı ve anahtarınızı öykünücü ile kullanamazsınız. Üretim verileriyle birlikte geliştirme hesabı kullanmamalısınız.
> 
> Öykünücü yalnızca HTTP üzerinden bağlantıyı destekler. Ancak HTTPS, bir üretim Azure depolama hesabındaki kaynaklara erişmek için önerilen protokoldür.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Bir kısayol kullanarak öykünücü hesabına bağlanma
Uygulamanızdan öykünücüye bağlanmanın en kolay yolu, uygulamanızın yapılandırma dosyasında kısayola başvuran bir bağlantı dizesi yapılandırmaktır `UseDevelopmentStorage=true` . Bir *app.config* dosyadaki öykünücüye yönelik bağlantı dizesi örneği aşağıda verilmiştir: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

, Bağlantı dizesinde kullanmak istediğiniz öykünücü hizmetlerinin her biri için hesap adını, hesap anahtarını ve uç noktalarını tam olarak belirtmeye eşdeğerdir. Bu, bağlantı dizesinin, bir üretim depolama hesabı için olandan farklı olan öykünücü uç noktalarına başvurması için gereklidir. Örneğin, bağlantı dizeniz değeri şöyle görünür:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```
