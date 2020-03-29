---
title: Otomatik büyütme depolama - Azure CLI - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki Azure CLI'yi kullanarak depolama otomatik büyümesini nasıl yapılandırabileceğiniz açıklanmaktadır.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 8/7/2019
ms.openlocfilehash: b0dc2fbb168d9325439ee18a227f71a3b88ef9c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74767968"
---
# <a name="auto-grow-azure-database-for-postgresql-storage---single-server-using-the-azure-cli"></a>PostgreSQL depolama için Azure Veritabanını otomatik olarak büyüt - Azure CLI'yi kullanarak Tek Sunucu
Bu makalede, iş yükünü etkilemeden büyümek üzere PostgreSQL sunucu depolama için bir Azure Veritabanını nasıl yapılandırabileceğiniz açıklanmaktadır.

Depolama [sınırına ulaşan](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#reaching-the-storage-limit)sunucu, salt okunur olarak ayarlanır. Depolama otomatik büyütme etkinse, 100 GB'dan az özellikli depolama alanına sahip sunucular için, ücretsiz depolama alanı 1 GB'ın altında veya sağlanan depolama nın %10'unun altında olduğu anda sağlanan depolama boyutu 5 GB artırılır. 100 GB'dan fazla depoya sahip sunucular için, ücretsiz depolama alanı sağlanan depolama alanının %5'in altında olduğunda, sağlanan depolama boyutu %5 artırılır. [Burada](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) belirtildiği gibi maksimum depolama limitleri geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [PostgreSQL sunucusu için Bir Azure Veritabanı](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılsa kılavuzu, Azure CLI sürüm 2.0 veya sonrası kullanmanızı gerektirir. Sürümü onaylamak için Azure CLI komut istemine girin. `az --version` Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.

## <a name="enable-postgresql-server-storage-auto-grow"></a>PostgreSQL sunucu depolamaotomatik büyümesini etkinleştirin

Sunucu otomatik olarak depolamayı varolan bir sunucuda aşağıdaki komutla etkinleştirin:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Aşağıdaki komuta sahip yeni bir sunucu oluştururken sunucu otomatik olarak depolama yı etkinleştirin:

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 9.6
```

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-on-metric.md)hakkında bilgi edinin.