---
title: İçeriği bir bulut klasöründen Eşitle
description: OneDrive veya Dropbox dahil olmak üzere bir bulut klasöründen içerik eşitlemesi aracılığıyla Azure App Service uygulamanızı nasıl dağıtacağınızı öğrenin.
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.topic: article
ms.date: 12/03/2018
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 9f18eef607a5f655aecc0b6ebe1c85a239b34192
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77482981"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Bir bulut klasöründen içerik eşitleme Azure App Service
Bu makalede, içeriğinizi Dropbox ve OneDrive 'dan [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) eşitlenecek. 

İsteğe bağlı içerik eşitleme dağıtımı App Service [kudu dağıtım altyapısı](https://github.com/projectkudu/kudu/wiki)tarafından desteklenir. Belirlenen bir bulut klasöründe uygulama kodunuzla ve içeriklerle çalışabilir ve ardından bir düğmeye tıklayarak App Service eşitleyebilirsiniz. İçerik eşitleme, kudu derleme sunucusunu kullanır. 

## <a name="enable-content-sync-deployment"></a>İçerik eşitleme dağıtımını etkinleştir

İçerik eşitlemesini etkinleştirmek için [Azure portal](https://portal.azure.com)App Service uygulama sayfanıza gidin.

Sol taraftaki menüde, **dağıtım merkezi** > **OneDrive** veya **Dropbox** > **Yetkilendir**' e tıklayın. Yetkilendirme istemlerini izleyin. 

![](media/app-service-deploy-content-sync/choose-source.png)

Yalnızca OneDrive veya Dropbox ile bir kez yetkilendirme yapmanız gerekir. Zaten yetkiniz varsa **devam**' a tıklamanız yeterlidir. **Hesabı Değiştir**' i tıklatarak yetkili OneDrive veya Dropbox hesabını değiştirebilirsiniz.

![](media/app-service-deploy-content-sync/continue.png)

**Yapılandır** sayfasında, eşitlenmesini istediğiniz klasörü seçin. Bu klasör, OneDrive veya Dropbox 'ta aşağıdaki belirtilen içerik yolu altında oluşturulur. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

İşiniz bittiğinde **devam**' a tıklayın.

**Özet** sayfasında, seçeneklerinizi doğrulayıp **son**' a tıklayın.

## <a name="synchronize-content"></a>İçeriği eşitler

Bulut klasörünüzdeki içerikleri App Service eşitlemek istediğinizde, **Dağıtım Merkezi** sayfasına dönün ve **Eşitle**' ye tıklayın.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > API 'lerde temeldeki farklılıklar nedeniyle, **OneDrive iş** Şu anda desteklenmiyor. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>İçerik eşitleme dağıtımını devre dışı bırak

İçerik eşitlemesini devre dışı bırakmak için [Azure portal](https://portal.azure.com)App Service uygulama sayfanıza gidin.

Sol taraftaki menüden **dağıtım merkezi** > **bağlantısını kes**' e tıklayın.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel git deposundan dağıtma](deploy-local-git.md)
