---
title: Bulut klasöründeki içeriği eşitleme
description: OneDrive veya Dropbox gibi bulut klasöründen içerik eşitleme yoluyla uygulamanızı Azure Uygulama Hizmeti'ne nasıl dağıtılacağize gideceğinizi öğrenin.
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.topic: article
ms.date: 12/03/2018
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 9f18eef607a5f655aecc0b6ebe1c85a239b34192
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77482981"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>İçeriği bulut klasöründen Azure Uygulama Hizmeti'ne senkronize etme
Bu makalede, içeriğinizin Dropbox ve OneDrive'dan [Azure Uygulama Hizmeti](https://go.microsoft.com/fwlink/?LinkId=529714) ile nasıl senkronize edilebilmektedir. 

İsteğe bağlı içerik eşitleme dağıtımı, App Service [Kudu dağıtım motoru](https://github.com/projectkudu/kudu/wiki)tarafından desteklenmektedir. Uygulama kodunuz ve içeriğinizle belirlenmiş bir bulut klasöründe çalışabilir ve ardından bir düğmeyi tıklatarak Uygulama Hizmeti'ne senkronize edebilirsiniz. İçerik eşitleme Kudu build sunucusu kullanır. 

## <a name="enable-content-sync-deployment"></a>İçerik eşitleme dağıtımını etkinleştirme

İçerik eşitlemesini etkinleştirmek için [Azure portalındaki](https://portal.azure.com)Uygulama Hizmeti uygulama sayfanıza gidin.

Sol menüde **Dağıtım Merkezi** > **OneDrive** veya **Dropbox** > **Yetkilendirme'yi**tıklatın. Yetkilendirme istemlerini izleyin. 

![](media/app-service-deploy-content-sync/choose-source.png)

OneDrive veya Dropbox ile yalnızca bir kez yetkilendirmeniz gerekir. Zaten yetkiliyseniz, **Devam et'i**tıklatın. Hesabı **Değiştir'i**tıklatarak yetkili OneDrive veya Dropbox hesabını değiştirebilirsiniz.

![](media/app-service-deploy-content-sync/continue.png)

**Yapılandırma** sayfasında, eşitlemek istediğiniz klasörü seçin. Bu klasör, OneDrive veya Dropbox'ta aşağıdaki belirtilen içerik yolu altında oluşturulur. 
   
* **OneDrive**:`Apps\Azure Web Apps`
* **Dropbox**:`Apps\Azure`

Bittiğinde, **Devam et'i**tıklatın.

**Özet** sayfasında, seçeneklerinizi doğrulayın ve **Bitir'i**tıklatın.

## <a name="synchronize-content"></a>İçeriği senkronize etme

Bulut klasörünüzdeki içeriği Uygulama Hizmeti ile senkronize etmek istediğinizde, **Dağıtım Merkezi** sayfasına geri dön ve **Eşitle'yi**tıklatın.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > API'lerde temel deki farklılıklar nedeniyle, **OneDrive for Business** şu anda desteklenmez. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>İçerik eşitleme dağıtımını devre dışı

İçerik eşitlemeyi devre dışı kalmak için [Azure portalındaki](https://portal.azure.com)Uygulama Hizmeti uygulama sayfanıza gidin.

Sol menüde **Dağıtım Merkezi** > **Bağlantısını Kes'i**tıklatın.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel Git repo'sundan dağıt](deploy-local-git.md)
