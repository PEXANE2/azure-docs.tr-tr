---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/28/2020
ms.author: tamram
ms.openlocfilehash: a9d7f4f77d91abc88ea348e71a3d9c471b26a273
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103622079"
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

#### <a name="connect-to-the-emulator-account-using-the-shortcut"></a>Kısayolunu kullanarak öykünücü hesabına bağlanma

Uygulamanızdan öykünücüye bağlanmanın en kolay yolu, uygulamanızın yapılandırma dosyasında kısayola başvuran bir bağlantı dizesi yapılandırmaktır `UseDevelopmentStorage=true` . Kısayol, her bir Azure depolama hizmeti için hesap adını, hesap anahtarını ve öykünücü uç noktalarını belirten öykünücü için tam bağlantı dizesine eşdeğerdir:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
```

Aşağıdaki .NET kod parçacığı, bir bağlantı dizesi alan bir yöntemden kısayolu nasıl kullanabileceğinizi gösterir. Örneğin, [Blobcontainerclient (dize, dize)](/dotnet/api/azure.storage.blobs.blobcontainerclient.-ctor#Azure_Storage_Blobs_BlobContainerClient__ctor_System_String_System_String_) Oluşturucusu bir bağlantı dizesi alır.

```csharp
BlobContainerClient blobContainerClient = new BlobContainerClient("UseDevelopmentStorage=true", "sample-container");
blobContainerClient.CreateIfNotExists();
```

Kod parçacığında kodu çağırmadan önce öykünücünün çalıştığından emin olun.
