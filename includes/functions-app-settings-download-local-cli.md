---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: e10de8093bf152b75cc6f262a142ff07c3d5b0d3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72329574"
---
Azure 'da gerekli depolama hesabıyla birlikte bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken depolama hesabınıza bağlanmak için, uygulama ayarlarını yerel. Settings. JSON dosyasına indirmeniz gerekir. 

Projenin kökünden, ayarları yerel. Settings. json ' a indirmek için, önceki makaledeki işlev uygulamanızın adıyla değiştirerek `<APP_NAME>` aşağıdaki Azure Functions Core Tools komutunu çalıştırın:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Azure hesabınızda oturum açmanız gerekebilir.

> [!IMPORTANT]  
> Bu komut, Azure 'daki işlev uygulamanızdan gelen değerlerle mevcut ayarların üzerine yazar.  
>
> Gizli dizi içerdiğinden, Local. Settings. JSON dosyası asla yayımlanmaz ve kaynak denetiminden dışlanmalıdır.  
> 

Depolama hesabı bağlantı dizesi `AzureWebJobsStorage`olan değere ihtiyacınız vardır. Çıkış bağlamasının beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.
