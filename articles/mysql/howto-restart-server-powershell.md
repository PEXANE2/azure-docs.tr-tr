---
title: Sunucuyu yeniden Başlat-Azure PowerShell-MySQL için Azure veritabanı
description: Bu makalede PowerShell kullanarak MySQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 4/28/2020
ms.openlocfilehash: faefdfb93257521c9c687045c1467c5d2d0f84ab
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86107946"
---
# <a name="restart-azure-database-for-mysql-server-using-powershell"></a>PowerShell kullanarak MySQL için Azure veritabanı sunucusunu yeniden başlatma

Bu konuda, MySQL sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklanmaktadır. İşlem sırasında kısa bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmayı tamamlaması için gereken süre, MySQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](/powershell/azure/install-az-ps)
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. MySql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.MySql -AllowPrerelease` .
> Az. MySql PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurepowershell-interactive
Restart-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak MySQL için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-powershell.md)
