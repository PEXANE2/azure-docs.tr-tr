---
title: Erişim denetim günlükleri-Azure CLı-MariaDB için Azure veritabanı
description: Bu makalede, Azure CLı 'dan MariaDB için Azure veritabanı 'nda denetim günlüklerinin nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/18/2020
ms.openlocfilehash: 670fd7f34b75ae05726053decdab7c15cdcdecc8
ms.sourcegitcommit: 51718f41d36192b9722e278237617f01da1b9b4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85100840"
---
# <a name="configure-and-access-audit-logs-in-the-azure-cli"></a>Azure CLı 'de denetim günlüklerini yapılandırma ve erişme

[MariaDB Için Azure veritabanı denetim günlüklerini](concepts-audit-logs.md) Azure CLI 'dan yapılandırabilirsiniz.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği Şu anda önizleme aşamasındadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:

- [MariaDB sunucusu için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır Kılavuzu, Azure CLı sürüm 2,0 veya üstünü kullanmanızı gerektirir. Sürümü onaylamak için, Azure CLı komut isteminde, girin `az --version` . Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek]( /cli/azure/install-azure-cli).

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

>[!NOTE]
> Sunucunuzun performansının yoğun bir şekilde etkilenmemesini sağlamak için, yalnızca denetim amaçlarıyla gerekli olay türlerini ve kullanıcıları günlüğe kaydetmek önerilir.

Aşağıdaki adımları kullanarak denetim günlüğünü etkinleştirin ve yapılandırın: 

1. **Audit_logs_enabled** PARAMETRESINI "on" olarak ayarlayarak denetim günlüklerini açın. 
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_enabled --resource-group myresourcegroup --server mydemoserver --value ON
    ```

1. **Audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_events --resource-group myresourcegroup --server mydemoserver --value "ADMIN,CONNECTION"
    ```

1. **Audit_log_exclude_users** parametresi güncelleştirilerek günlüğe kaydetme dışında tutulacak MariaDB kullanıcılarını ekleyin. Kullanıcıları, MariaDB kullanıcı adlarını girerek belirtin.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_exclude_users --resource-group myresourcegroup --server mydemoserver --value "azure_superuser"
    ```

1. **Audit_log_include_users** parametresini güncelleştirerek günlüğe kaydetmek için dahil edilecek herhangi bir özel MariaDB kullanıcısı ekleyin. Kullanıcıları, MariaDB kullanıcı adlarını girerek belirtin.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_include_users --resource-group myresourcegroup --server mydemoserver --value "sampleuser"
    ```

## <a name="next-steps"></a>Sonraki adımlar

- MariaDB için Azure veritabanı 'nda [Denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin
- [Azure Portal](howto-configure-audit-logs-portal.md) denetim günlüklerini yapılandırma hakkında bilgi edinin