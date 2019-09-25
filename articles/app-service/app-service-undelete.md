---
title: Silinen App Service uygulamalarını geri yükleme-Azure App Service
description: Silinen App Service uygulamasını PowerShell kullanarak geri yüklemeyi öğrenin.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.service: app-service
ms.openlocfilehash: a766f7ed7a82874df8ef7506226de0d6f38a5847
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219545"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>PowerShell kullanarak silinen App Service uygulamasını geri yükleme

Uygulamanızı Azure App Service yanlışlıkla sildikten sonra, [az PowerShell modülündeki](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0)komutları kullanarak geri yükleyebilirsiniz.

## <a name="list-deleted-apps"></a>Silinen uygulamaları listeleme

Silinen uygulamalar koleksiyonunu almak için kullanabilirsiniz `Get-AzDeletedWebApp`.

Silinen belirli bir uygulamayla ilgili ayrıntılar için şunları kullanabilirsiniz:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app>
```

Ayrıntılı bilgiler şunları içerir:

- **Silinsiteıd**: Uygulamanın benzersiz tanımlayıcısı, aynı ada sahip birden fazla uygulamanın silindiği senaryolar için kullanılır
- **Abonelik kimliği**: Silinen kaynağı içeren abonelik
- **Konum**: Orijinal uygulamanın konumu
- **Resourcegroupname**: Özgün kaynak grubunun adı
- **Ad**: Orijinal uygulamanın adı.
- **Yuva**: yuvanın adı.
- **Silme zamanı**: Uygulamanın ne zaman silindiği  

## <a name="restore-deleted-app"></a>Silinen uygulamayı geri yükle

Geri yüklemek istediğiniz uygulama tanımlandıktan sonra öğesini kullanarak `Restore-AzDeletedWebApp`geri yükleyebilirsiniz.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```

Komut girişleri şunlardır:

- **Kaynak grubu**: Uygulamanın geri yükleneceği hedef kaynak grubu
- **Ad**: Uygulamanın adı, genel olarak benzersiz olmalıdır.
- **Targetappserviceplanname**: Uygulamayla bağlantılı App Service planı

Varsayılan `Restore-AzDeletedWebApp` olarak, hem uygulama yapılandırmanızı hem de içeriği geri yükler. Yalnızca içeriği geri yüklemek istiyorsanız bu komutu kullanın `-RestoreContentOnly` .
