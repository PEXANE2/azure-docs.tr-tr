---
title: Silinen uygulamaları geri yükleme
description: Azure Uygulama Hizmeti'nde silinen bir uygulamayı nasıl geri yükleyin öğrenin. Yanlışlıkla silinen bir uygulamanın baş ağrısından kaçının.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: 296c8e2dfe99e3b0aea66f364ac6f6d9b2f60a1a
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272500"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>PowerShell kullanarak silinmiş App Service uygulamasını geri yükleme

Azure Uygulama Hizmeti'nde uygulamanızı yanlışlıkla sildiyseniz, [Az PowerShell modülündeki](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0)komutları kullanarak uygulamayı geri yükleyebilirsiniz.

> [!NOTE]
> Silinen uygulamalar, ilk silinmeden 30 gün sonra sistemden silinir. Bir uygulama temizlendikten sonra kurtarılamaz.
>

## <a name="re-register-app-service-resource-provider"></a>Uygulama Hizmeti kaynak sağlayıcısını yeniden kaydedin
Bazı müşteriler, silinen uygulamaların listesini almanın başarısız olduğu bir sorunla karşılaşabilir. Sorunu gidermek için aşağıdaki komutu çalıştırın:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Silinen uygulamaları listele

Silinen uygulamaların koleksiyonunu almak için `Get-AzDeletedWebApp`.

Silinen belirli bir uygulamayla ilgili ayrıntılar için şunları kullanabilirsiniz:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

Ayrıntılı bilgiler şunları içerir:

- **DeletedSiteId**: Aynı adı taşıyan birden fazla uygulamanın silindiği senaryolar için kullanılan uygulama için benzersiz tanımlayıcı
- **SubscriptionID**: Silinen kaynağı içeren abonelik
- **Konum**: Orijinal uygulamanın konumu
- **ResourceGroupName**: Özgün kaynak grubunun adı
- **Adı**: Orijinal uygulamanın adı.
- **Yuva**: yuvanın adı.
- **Silme Süresi**: Uygulama ne zaman silindi  

## <a name="restore-deleted-app"></a>Silinen uygulamayı geri yükleme

Geri yüklemek istediğiniz uygulama tanımlandıktan sonra, 'yi `Restore-AzDeletedWebApp`kullanarak geri yükleyebilirsiniz.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Dağıtım yuvaları uygulamanızın bir parçası olarak geri yüklenmez. Bir evreleme yuvasını geri yüklemeniz gerekiyorsa `-Slot <slot-name>` bayrağı kullanın.
>

Komut girişleri şunlardır:

- **Kaynak Grubu**: Uygulamanın geri yüklendiği hedef kaynak grubu
- **Adı**: Uygulamanın adı, genel olarak benzersiz olmalıdır.
- **TargetAppServicePlanName**: Uygulamaya bağlı Uygulama Hizmeti planı

Varsayılan `Restore-AzDeletedWebApp` olarak hem uygulama yapılandırmanızı hem de bir içeriği geri yüklenir. Yalnızca içeriği geri yüklemek istiyorsanız, `-RestoreContentOnly` bayrağı bu komut ile kullanırsınız.

> [!NOTE]
> Uygulama bir Uygulama Hizmet Ortamında barındırıldıysa ve ardından silindiyse, yalnızca ilgili Uygulama Hizmeti Ortamı hala varsa geri yüklenebilir.
>

Burada tam komut dosyası referans bulabilirsiniz: [Geri Yükleme-AzDeletedWebApp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
