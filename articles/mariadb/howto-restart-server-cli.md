---
title: Sunucuyı yeniden başlat - Azure CLI - MariaDB için Azure Veritabanı
description: Bu makalede, Azure CLI'yi kullanarak MariaDB sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: f5572611b99245fd62b4e0a9d73e6ed728e42f5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530657"
---
# <a name="restart-azure-database-for-mariadb-server-using-the-azure-cli"></a>Azure CLI'yi kullanarak MariaDB sunucusu için Azure Veritabanını yeniden başlatın
Bu konu, MariaDB sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirirken kısa bir kesintiye neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatılır. Örneğin, hizmet vCores ölçekleme gibi daha önce istenen bir işlem işliyor olabilir.

Yeniden başlatmayı tamamlamak için gereken süre MariaDB kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda oluşan etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [MariaDB sunucusu için bir Azure Veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılsa kılavuzu, Azure CLI sürüm 2.0 veya sonrası kullanmanızı gerektirir. Sürümü onaylamak için Azure CLI komut istemine girin. `az --version` Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.


## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Sunucuyu aşağıdaki komutla yeniden başlatın:

```azurecli-interactive
az mariadb server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

[MariaDB için Azure Veritabanı'nda parametrelerin nasıl ayarlanır](howto-configure-server-parameters-cli.md) hakkında bilgi edinin