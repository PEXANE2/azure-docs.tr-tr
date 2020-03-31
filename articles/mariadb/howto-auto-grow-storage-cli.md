---
title: Otomatik büyütme depolama - Azure CLI - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB için Azure Veritabanı'ndaki Azure CLI'yi kullanarak otomatik büyütme depolamasını nasıl etkinleştirebileceğiniz açıklanmaktadır.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 4be84c750f6a3ca7a0d48aa2b98d75272c1cbadf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529093"
---
# <a name="auto-grow-azure-database-for-mariadb-storage-using-the-azure-cli"></a>Azure CLI'yi kullanarak MariaDB depolama için Azure Veritabanını otomatik olarak büyütün
Bu makalede, MariaDB sunucu depolama için bir Azure Veritabanını iş yükünü etkilemeden büyüyecek şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

Depolama [sınırına ulaşan](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#reaching-the-storage-limit)sunucu, salt okunur olarak ayarlanır. Depolama otomatik büyütme etkinse, 100 GB'dan az özellikli depolama alanına sahip sunucular için, ücretsiz depolama alanı 1 GB'ın altında veya sağlanan depolama nın %10'unun altında olduğu anda sağlanan depolama boyutu 5 GB artırılır. 100 GB'dan fazla depoya sahip sunucular için, ücretsiz depolama alanı sağlanan depolama alanının %5'in altında olduğunda, sağlanan depolama boyutu %5 artırılır. [Burada](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) belirtildiği gibi maksimum depolama limitleri geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [MariaDB sunucusu için bir Azure Veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılsa kılavuzu, Azure CLI sürüm 2.0 veya sonrası kullanmanızı gerektirir. Sürümü onaylamak için Azure CLI komut istemine girin. `az --version` Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.

## <a name="enable-mariadb-server-storage-auto-grow"></a>MariaDB sunucu depolamaotomatik büyümesini etkinleştirin

Sunucu otomatik olarak depolamayı varolan bir sunucuda aşağıdaki komutla etkinleştirin:

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Aşağıdaki komuta sahip yeni bir sunucu oluştururken sunucu otomatik olarak depolama yı etkinleştirin:

```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.3
```

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-metric.md)hakkında bilgi edinin.