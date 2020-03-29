---
title: Sunucuyeniden başlatın - Azure CLI - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure CLI'yi kullanarak PostgreSQL - Single Server için bir Azure Veritabanını nasıl yeniden başlatabileceğiniz açıklanmaktadır
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 13b26b545f9e95ce2457e4f8d9cf32da59cd91e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770161"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>PostgreSQL için Azure Veritabanını Yeniden Başlat - Azure CLI'yi kullanarak Tek Sunucu
Bu konu, PostgreSQL sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirirken kısa bir kesintiye neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatılır. Örneğin, hizmet vCores ölçekleme gibi daha önce istenen bir işlem işliyor olabilir.
 
Yeniden başlatmayı tamamlamak için gereken süre PostgreSQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda oluşan etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [PostgreSQL sunucusu için Bir Azure Veritabanı](quickstart-create-server-up-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılsa kılavuzu, Azure CLI sürüm 2.0 veya sonrası kullanmanızı gerektirir. Sürümü onaylamak için Azure CLI komut istemine girin. `az --version` Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.


## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Sunucuyu aşağıdaki komutla yeniden başlatın:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL için Azure Veritabanı'nda parametrelerin nasıl ayarlanması](howto-configure-server-parameters-using-cli.md) hakkında bilgi edinin