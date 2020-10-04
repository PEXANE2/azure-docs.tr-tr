---
title: Sunucuyu yeniden başlatma-Azure CLı-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure CLı kullanarak bir PostgreSQL için Azure veritabanı 'nı (tek sunucu) nasıl yeniden başlatabileceğinizi açıklanmaktadır
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 30770dd7ed71f4cde68c1a616a256bf60b5acaef
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91704766"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı 'nı yeniden başlatma-tek sunucu
Bu konuda, PostgreSQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.
 
Yeniden başlatmayı tamamlaması için gereken süre, PostgreSQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-server-up-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır Kılavuzu, Azure CLı sürüm 2,0 veya üstünü kullanmanızı gerektirir. Sürümü onaylamak için, Azure CLı komut isteminde, girin `az --version` . Yüklemek veya yükseltmek için bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL Için Azure veritabanı 'nda parametrelerin nasıl ayarlanacağı](howto-configure-server-parameters-using-cli.md) hakkında bilgi edinin
