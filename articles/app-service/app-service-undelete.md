---
title: Silinen App Service uygulamalarını geri yükleme-Azure App Service
description: Silinen App Service uygulamasını PowerShell kullanarak geri yüklemeyi öğrenin.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.service: app-service
ms.openlocfilehash: 7b3a21f3cfee806dc94353e0bc6c11e88641ea34
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827533"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>PowerShell kullanarak silinen App Service uygulamasını geri yükleme

Uygulamanızı Azure App Service yanlışlıkla sildikten sonra, [az PowerShell modülündeki](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0)komutları kullanarak geri yükleyebilirsiniz.

## <a name="list-deleted-apps"></a>Silinen uygulamaları listeleme

Silinen uygulamalar koleksiyonunu almak için `Get-AzDeletedWebApp` ' ı kullanabilirsiniz.

Silinen belirli bir uygulamayla ilgili ayrıntılar için şunları kullanabilirsiniz:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app>
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

Geri yüklemek istediğiniz uygulama tanımlandıktan sonra, `Restore-AzDeletedWebApp` kullanarak geri yükleyebilirsiniz.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```

Komut girişleri şunlardır:

- **Kaynak grubu**: uygulamanın geri yükleneceği hedef kaynak grubu
- **Ad**: uygulamanın adı, genel olarak benzersiz olmalıdır.
- **Targetappserviceplanname**: uygulamaya bağlı App Service plan

Varsayılan olarak `Restore-AzDeletedWebApp`, hem uygulama yapılandırmanızı hem de içeriği geri yükler. Yalnızca içeriği geri yüklemek istiyorsanız, bu commandlet ile `-RestoreContentOnly` bayrağını kullanın.

Tam komutunu başvurusunu buradan bulabilirsiniz: [restore-azdeletedwebapp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
