---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: e10de8093bf152b75cc6f262a142ff07c3d5b0d3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "72329574"
---
Azure 'da gerekli depolama hesabıyla birlikte bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken depolama hesabınıza bağlanmak için, uygulama ayarlarını dosyaya local.settings.jsindirmeniz gerekir. 

Projenin kökünden, `<APP_NAME>` önceki makaledeki işlev uygulamanızın adıyla değiştirerek, local.settings.jsüzerine ayarları indirmek için aşağıdaki Azure Functions Core Tools komutunu çalıştırın:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Azure hesabınızda oturum açmanız gerekebilir.

> [!IMPORTANT]  
> Bu komut, Azure 'daki işlev uygulamanızdan gelen değerlerle mevcut ayarların üzerine yazar.  
>
> Gizli dizi içerdiğinden, dosyadaki local.settings.jshiçbir şekilde yayımlanmaz ve kaynak denetiminden dışlanmalıdır.  
> 

`AzureWebJobsStorage`Depolama hesabı bağlantı dizesi olan değere ihtiyacınız vardır. Çıkış bağlamasının beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.
