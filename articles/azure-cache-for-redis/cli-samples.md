---
title: Azure CLı ile Redsıs için Azure önbelleğini yönetme
description: 'Redsıs için Azure önbelleğini yönetmeye yönelik Azure CLı örnekleri: önbellek oluşturma, önbelleği silme, önbellek ayrıntılarını alma, ana bilgisayar, bağlantı noktaları ve anahtarlar, bir Web uygulamasını bağlama.'
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/14/2017
ms.openlocfilehash: 345fd0272e0cfe9b4d7329f004e628ea7820c2d9
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87421249"
---
# <a name="manage-azure-cache-for-redis-with-azure-cli"></a>Azure CLı ile Redsıs için Azure önbelleğini yönetme

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| Önbellek oluşturma | Description |
| ------------ | ----------- |
| [Bir önbellek oluşturma](./scripts/create-cache.md) | Redsıs için bir kaynak grubu ve temel katman Azure önbelleği oluşturur. |
| [Kümeleme ile Premium önbellek oluşturma](./scripts/create-premium-cache-cluster.md) | Kümelendirmeyi etkin bir kaynak grubu ve Premium katman önbelleği oluşturur.|
| [Önbellek ayrıntılarını al](./scripts/show-cache.md) | Sağlama durumu da dahil olmak üzere Redsıs örneği için Azure önbelleğinin ayrıntılarını alır. |
| [Ana bilgisayar adı, bağlantı noktaları ve anahtarlar alın](./scripts/cache-keys-ports.md) | Redsıs örneği için bir Azure önbelleği için ana bilgisayar adı, bağlantı noktaları ve anahtarlar alır. |
|**Web uygulaması artı önbelleği**| **Açıklama**|
| [Redsıs için bir Web uygulamasını Azure önbelleğine bağlama](./../app-service/scripts/cli-connect-to-redis.md) | Redsıs için bir Azure Web uygulaması ve Azure önbelleği oluşturur, ardından uygulama ayarlarına redsıs bağlantı ayrıntılarını ekler. |
|**Önbelleği sil**| **Açıklama** |
| [Önbellek silme](./scripts/delete-cache.md) | Redsıs örneği için bir Azure önbelleğini siler  |

Azure CLı hakkında daha fazla bilgi için bkz. [Azure CLI 'Yı yükleyip](https://docs.microsoft.com/cli/azure/install-azure-cli) [Azure CLI kullanmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
