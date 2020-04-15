---
title: Erişim denetim günlükleri - Azure CLI - MySQL için Azure Veritabanı
description: Bu makalede, Azure CLI'den MySQL için Azure Veritabanı'ndaki denetim günlüklerinin nasıl yapılandırılabildiğini ve bunlara erişilenler açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/13/2020
ms.openlocfilehash: d532e1990586d80d675a8ccb247c0c9f7908bb6f
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81384174"
---
# <a name="configure-and-access-audit-logs-in-the-azure-cli"></a>Azure CLI'deki denetim günlüklerini yapılandırma ve erişim

[MySQL denetim günlükleri için Azure Veritabanını](concepts-audit-logs.md) Azure CLI'den yapılandırabilirsiniz.

> [!IMPORTANT]
> Denetim günlüğü işlevi şu anda önizlemede.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:

- [MySQL sunucusu için Azure Veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılsa kılavuzu, Azure CLI sürüm 2.0 veya sonrası kullanmanızı gerektirir. Sürümü onaylamak için Azure CLI komut istemine girin. `az --version` Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.

## <a name="configure-audit-logging"></a>Denetim günlüğe kaydetme

Denetim günlüğe kaydetmeyi aşağıdaki adımları kullanarak etkinleştirin ve yapılandırın:

1. **audit_logs_enabled** parametresini "AGİ" olarak ayarlayarak denetim günlüklerini açın. 
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_enabled --resource-group myresourcegroup --server mydemoserver --value ON
    ```

1. **audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_events --resource-group myresourcegroup --server mydemoserver --value "ADMIN,CONNECTION"
    ```

1. **audit_log_exclude_users** parametresini güncelleyerek günlük dışı bırakılacak herhangi bir MySQL kullanıcısını ekleyin. MySQL kullanıcı adlarını vererek kullanıcıları belirtin.
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_exclude_users --resource-group myresourcegroup --server mydemoserver --value "azure_superuser"
    ```

1. **audit_log_include_users** parametresini güncelleyerek günlüğe kaydedilecek belirli MySQL kullanıcılarını ekleyin. MySQL kullanıcı adlarını vererek kullanıcıları belirtin.
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_include_users --resource-group myresourcegroup --server mydemoserver --value "sampleuser"
    ```

## <a name="next-steps"></a>Sonraki adımlar
- MySQL için Azure Veritabanı'ndaki [denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin
- [Azure portalında](howto-configure-audit-logs-portal.md) denetim günlüklerini nasıl yapılandırılamayı öğrenin