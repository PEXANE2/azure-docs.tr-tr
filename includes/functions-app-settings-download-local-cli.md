---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: e10de8093bf152b75cc6f262a142ff07c3d5b0d3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "72329574"
---
Azure'da gerekli Depolama hesabıyla birlikte bir işlev uygulaması zaten oluşturdunuz. Bu hesabın bağlantı dizesi Azure'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken Depolama hesabınıza bağlanmak için uygulama ayarlarını local.settings.json dosyasına indirmeniz gerekir. 

Projenin kökünden, ayarları local.settings.json'a indirmek için aşağıdaki Azure İşleme `<APP_NAME>` Temel Araçları komutunu çalıştırın ve önceki makalede işlev uygulamanızın adını değiştirin:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Azure hesabınızda oturum açmanız gerekebilir.

> [!IMPORTANT]  
> Bu komut, Azure'daki işlev uygulamanızdan alınan değerlerle varolan ayarların üzerine yazar.  
>
> Sırlar içerdiğinden, local.settings.json dosyası asla yayınlanmaz ve kaynak denetiminden çıkarılmalıdır.  
> 

Depolama hesabı `AzureWebJobsStorage`bağlantı dizesi olan değere ihtiyacınız var. Çıktı bağlamanın beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanırsınız.
