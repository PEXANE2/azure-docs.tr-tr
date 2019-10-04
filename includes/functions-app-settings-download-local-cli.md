---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 801613f4e3fb1e754856d716c6815895ea5da3aa
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839080"
---
Azure 'da gerekli depolama hesabıyla birlikte bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken depolama hesabınıza bağlanmak için, uygulama ayarlarını yerel. Settings. JSON dosyasına indirmeniz gerekir. Ayarları yerel. Settings. json ' a indirmek için aşağıdaki Azure Functions Core Tools komutunu çalıştırın, bu `<APP_NAME>` ' ı önceki makaledeki işlev uygulamanızın adıyla değiştirin:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Azure hesabınızda oturum açmanız gerekebilir.

> [!IMPORTANT]  
> Gizli dizi içerdiğinden, Local. Settings. JSON dosyası asla yayımlanmaz ve kaynak denetiminden dışlanmalıdır.

Depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage` değerine ihtiyacınız vardır. Çıkış bağlamasının beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.
