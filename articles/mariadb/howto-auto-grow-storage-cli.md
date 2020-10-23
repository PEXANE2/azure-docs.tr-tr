---
title: Depolamayı otomatik Büyüt-Azure CLı-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda Azure CLı kullanarak otomatik büyüme depolamayı nasıl etkinleştirebileceğinizi açıklanmaktadır.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: how-to
ms.date: 3/18/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: e46539e76eec73554f6e57ea09a373b79520def9
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92426082"
---
# <a name="auto-grow-azure-database-for-mariadb-storage-using-the-azure-cli"></a>Azure CLı kullanarak MariaDB depolama için Azure veritabanı 'nı otomatik büyütme
Bu makalede, bir MariaDB sunucu depolaması için Azure veritabanı 'nı, iş yükünü etkilemeden büyümeden nasıl yapılandırabileceğiniz açıklanmaktadır.

[Depolama sınırına ulaşan](concepts-pricing-tiers.md#reaching-the-storage-limit)sunucu, salt okunurdur olarak ayarlanmıştır. Depolama otomatik büyüme, 100 GB 'tan az kullanılabilir depolama alanı olan sunucular için etkinleştirildiyse, boş depolama alanı sağlanan depolamanın en fazla 1 GB veya %10 ' u altındaysa, sağlanan depolama boyutu 5 GB artar. 100 GB 'tan fazla kullanılabilir depolama alanı olan sunucularda, boş depolama alanı sağlanan depolama boyutunun %5 ' inden az olduğunda sağlanan depolama boyutu %5 oranında artar. [Burada](concepts-pricing-tiers.md#storage) belirtilen en fazla depolama sınırı geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MariaDB sunucusu Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır Kılavuzu, Azure CLı sürüm 2,0 veya üstünü kullanmanızı gerektirir. Sürümü onaylamak için, Azure CLı komut isteminde, girin `az --version` . Yüklemek veya yükseltmek için bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="enable-mariadb-server-storage-auto-grow"></a>MariaDB Server Storage otomatik büyümesini etkinleştir

Sunucu, var olan bir sunucuda depolamayı otomatik olarak büyüyerek aşağıdaki komutla etkinleştirin:

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Aşağıdaki komutla yeni bir sunucu oluştururken sunucu otomatik büyüyerek depolamayı etkinleştirin:

```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.3
```

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-metric.md)hakkında bilgi edinin.
