---
title: Sunucuyu yeniden başlatma-Azure CLı-MySQL için Azure veritabanı
description: Bu makalede, Azure CLı kullanarak MySQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 146c531805ff22544797c44c8df32b0e5d1e83b0
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74774232"
---
# <a name="restart-azure-database-for-mysql-server-using-the-azure-cli"></a>Azure CLı kullanarak MySQL için Azure veritabanı sunucusunu yeniden başlatma
Bu konuda, MySQL sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklanmaktadır. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmayı tamamlaması için gereken süre MySQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-server-up-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır Kılavuzu, Azure CLı sürüm 2,0 veya üstünü kullanmanızı gerektirir. Sürümü onaylamak için, Azure CLı komut isteminde `az --version`girin. Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurecli-interactive
az mysql server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

[MySQL Için Azure veritabanı 'nda parametreleri ayarlama](howto-configure-server-parameters-using-cli.md) hakkında bilgi edinin