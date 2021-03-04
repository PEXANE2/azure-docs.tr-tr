---
title: İçeriği bir bulut klasöründen Eşitle
description: OneDrive veya Dropbox dahil olmak üzere bir bulut klasöründen içerik eşitlemesi aracılığıyla Azure App Service uygulamanızı nasıl dağıtacağınızı öğrenin.
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.topic: article
ms.date: 02/25/2021
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: bfee320c7a8b4cbe8439c376350d1234b393bfb5
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051248"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Bir bulut klasöründen içerik eşitleme Azure App Service
Bu makalede, içeriğinizi Dropbox ve OneDrive 'dan [Azure App Service](./overview.md) eşitlenecek. 

İçerik eşitleme yaklaşımıyla, uygulama kodunuzla ve içeriklerle birlikte çalışarak bir dağıtım öncesi durumunda olduğundan emin olun ve ardından bir düğmeye tıklayarak App Service eşitleyin. 

API 'lerde temeldeki farklılıklar nedeniyle, **OneDrive iş** Şu anda desteklenmiyor.

> [!NOTE]
> Eski dağıtım deneyimi olan Azure portal **Geliştirme Merkezi (klasik)** sayfası mart, 2021 ' de kullanım dışı olacaktır. Bu değişiklik uygulamanızdaki mevcut dağıtım ayarlarını etkilemez ve **Dağıtım Merkezi** sayfasında uygulama dağıtımını yönetmeye devam edebilirsiniz.

## <a name="enable-content-sync-deployment"></a>İçerik eşitleme dağıtımını etkinleştir

1. [Azure Portal](https://portal.azure.com), App Service uygulamanızın yönetim sayfasına gidin.

1. Sol menüden **Dağıtım Merkezi**  >  **ayarları**' na tıklayın. 

1. **Kaynak** bölümünde **OneDrive** veya **Dropbox**' u seçin.

1. **Yetkilendir** ' e tıklayın ve yetkilendirme istemlerini izleyin. 

    ![Azure portal dağıtım merkezinde OneDrive veya Dropbox 'ın nasıl yetkilendirilemez olduğunu gösterir.](media/app-service-deploy-content-sync/choose-source.png)

    Azure hesabınız için yalnızca OneDrive veya Dropbox ile bir kez yetkilendirme yapmanız gerekir. Bir uygulama için farklı bir OneDrive veya Dropbox hesabını yetkilendirmek üzere **hesabı Değiştir**' e tıklayın.

1. **Klasör**' de, eşitlenmek üzere klasörü seçin. Bu klasör, OneDrive veya Dropbox 'ta aşağıdaki belirtilen içerik yolu altında oluşturulur. 
   
    * **OneDrive**: `Apps\Azure Web Apps`
    * **Dropbox**: `Apps\Azure`
    
1. **Kaydet**’e tıklayın.

## <a name="synchronize-content"></a>İçeriği eşitler

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

1. [Azure Portal](https://portal.azure.com), App Service uygulamanızın yönetim sayfasına gidin.

1. Sol menüden **Dağıtım Merkezi** yeniden  >  **Dağıt/Eşitle**' ye tıklayın. 

    ![Bulut klasörünüzün App Service ile nasıl eşitleneceğini gösterir.](media/app-service-deploy-content-sync/synchronize.png)
   
1. Eşitlemeyi onaylamak için **Tamam** ' ı tıklatın.

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Aşağıdaki komutu çalıştırarak ve öğesini değiştirerek bir eşitleme başlatın \<group-name> \<app-name> :

```azurecli-interactive
az webapp deployment source sync –-resource-group <group-name> –-name <app-name>
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Aşağıdaki komutu çalıştırarak ve öğesini değiştirerek bir eşitleme başlatın \<group-name> \<app-name> :

```azurepowershell-interactive
Invoke-AzureRmResourceAction -ResourceGroupName <group-name> -ResourceType Microsoft.Web/sites -ResourceName <app-name> -Action sync -ApiVersion 2019-08-01 -Force –Verbose
```

-----

## <a name="disable-content-sync-deployment"></a>İçerik eşitleme dağıtımını devre dışı bırak

1. [Azure Portal](https://portal.azure.com), App Service uygulamanızın yönetim sayfasına gidin.

1. Sol menüden **Dağıtım Merkezi**  >  **ayarlarının**  >  **bağlantısını kes**' e tıklayın. 

    ![Bulut klasörünüzün Azure portal App Service uygulamanızın bağlantısının nasıl yapılacağını gösterir.](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel git deposundan dağıtma](deploy-local-git.md)