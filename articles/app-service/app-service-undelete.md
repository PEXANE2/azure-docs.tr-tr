---
title: Silinen uygulamaları geri yükleme
description: Azure App Service silinen bir uygulamayı nasıl geri yükleyeceğinizi öğrenin. Yanlışlıkla silinen bir uygulamanın zahmetli kullanmaktan kaçının.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: 94a5fd3ae1de0708f0e5550547f7ab2fc9f881cb
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86146233"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>PowerShell kullanarak silinmiş App Service uygulamasını geri yükleme

Uygulamanızı Azure App Service yanlışlıkla sildikten sonra, [az PowerShell modülündeki](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0)komutları kullanarak geri yükleyebilirsiniz.

> [!NOTE]
> Silinen uygulamalar, ilk silinmeden 30 gün sonra sistemden temizlenir. Bir uygulama temizlenmeden sonra kurtarılamaz.
>

> [!NOTE]
> Geri alma işlevselliği, tüketim planı için desteklenmez.
>

## <a name="re-register-app-service-resource-provider"></a>App Service kaynak sağlayıcısını yeniden Kaydet
Bazı müşteriler, silinen uygulamaların listesini almanın başarısız olduğu bir sorun boyunca gelebilir. Sorunu çözmek için aşağıdaki komutu çalıştırın:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Silinen uygulamaları listeleme

Silinen uygulamalar koleksiyonunu almak için kullanabilirsiniz `Get-AzDeletedWebApp` .

Silinen belirli bir uygulamayla ilgili ayrıntılar için şunları kullanabilirsiniz:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

Ayrıntılı bilgiler şunları içerir:

- **Deletedsiteıd**: aynı ada sahip birden çok uygulama silindiği senaryolar için kullanılan, uygulamanın benzersiz tanımlayıcısı
- **SubscriptionID**: silinen kaynağı içeren abonelik
- **Konum**: özgün uygulamanın konumu
- **Resourcegroupname**: özgün kaynak grubunun adı
- **Ad**: özgün uygulamanın adı.
- **Yuva**: yuvanın adı.
- **Silme zamanı**: uygulamanın ne zaman silindiği  

## <a name="restore-deleted-app"></a>Silinen uygulamayı geri yükle
>[!NOTE]
> `Restore-AzDeletedWebApp`işlev uygulamaları için desteklenmez.

Geri yüklemek istediğiniz uygulama tanımlandıktan sonra öğesini kullanarak geri yükleyebilirsiniz `Restore-AzDeletedWebApp` .

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Dağıtım yuvaları, uygulamanızın bir parçası olarak geri yüklenmez. Bir hazırlama yuvasını geri yüklemeniz gerekiyorsa `-Slot <slot-name>` bayrağını kullanın.
>

Komut girişleri şunlardır:

- **Kaynak grubu**: uygulamanın geri yükleneceği hedef kaynak grubu
- **Ad**: uygulamanın adı, genel olarak benzersiz olmalıdır.
- **Targetappserviceplanname**: uygulamaya bağlı App Service plan

Varsayılan olarak `Restore-AzDeletedWebApp` , hem uygulama yapılandırmanızı hem de tüm içerikleri geri yükler. Yalnızca içeriği geri yüklemek istiyorsanız `-RestoreContentOnly` Bu komutu kullanın.

> [!NOTE]
> Uygulama üzerinde barındırılıyorsa ve sonra bir App Service Ortamı silinirse, bu, yalnızca karşılık gelen App Service Ortamı hala mevcutsa geri yüklenebilir.
>

Tam komutunu başvurusunu buradan bulabilirsiniz: [restore-azdeletedwebapp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
