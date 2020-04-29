---
title: Depolamayı otomatik Büyüt-Azure CLı-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda Azure CLı kullanarak otomatik büyüme depolamayı nasıl etkinleştirebileceğinizi açıklanmaktadır.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 4be84c750f6a3ca7a0d48aa2b98d75272c1cbadf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79529093"
---
# <a name="auto-grow-azure-database-for-mariadb-storage-using-the-azure-cli"></a>Azure CLı kullanarak MariaDB depolama için Azure veritabanı 'nı otomatik büyütme
Bu makalede, bir MariaDB sunucu depolaması için Azure veritabanı 'nı, iş yükünü etkilemeden büyümeden nasıl yapılandırabileceğiniz açıklanmaktadır.

[Depolama sınırına ulaşan](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#reaching-the-storage-limit)sunucu, salt okunurdur olarak ayarlanmıştır. Depolama otomatik büyüme, 100 GB 'tan az kullanılabilir depolama alanı olan sunucular için etkinleştirildiyse, boş depolama alanı sağlanan depolamanın en fazla 1 GB veya %10 ' u altındaysa, sağlanan depolama boyutu 5 GB artar. 100 GB 'tan fazla kullanılabilir depolama alanı olan sunucularda, boş depolama alanı sağlanan depolama boyutunun %5 ' inden az olduğunda sağlanan depolama boyutu %5 oranında artar. [Burada](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) belirtilen en fazla depolama sınırı geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MariaDB sunucusu Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır Kılavuzu, Azure CLı sürüm 2,0 veya üstünü kullanmanızı gerektirir. Sürümü onaylamak için, Azure CLı komut isteminde, girin `az --version`. Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek]( /cli/azure/install-azure-cli).

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