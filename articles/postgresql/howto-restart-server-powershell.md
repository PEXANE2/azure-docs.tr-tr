---
title: Sunucuyu yeniden Başlat-Azure PowerShell-PostgreSQL için Azure veritabanı
description: Bu makalede, PowerShell kullanarak PostgreSQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: a46437ae14ae644fb9d63d76a64075cc3ba2e25e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84739969"
---
# <a name="restart-azure-database-for-postgresql-server-using-powershell"></a>PowerShell kullanarak PostgreSQL için Azure veritabanı sunucusunu yeniden başlatma

Bu konuda, PostgreSQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklar. İşlem sırasında kısa bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmayı tamamlaması için gereken süre, PostgreSQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

- Yerel olarak yüklenen veya tarayıcıda [Azure Cloud Shell](https://shell.azure.com/) [az PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps)
- [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az. PostgreSql PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.PostgreSql -AllowPrerelease` .
> Az. PostgreSql PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

PowerShell 'i yerel olarak kullanmayı seçerseniz, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurepowershell-interactive
Restart-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [PowerShell kullanarak PostgreSQL için Azure veritabanı sunucusu oluşturma](quickstart-create-postgresql-server-database-using-azure-powershell.md)
