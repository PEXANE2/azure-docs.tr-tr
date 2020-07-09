---
title: Erişim denetim günlükleri-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portal MariaDB için Azure veritabanı 'nda denetim günlüklerinin nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 6/24/2020
ms.openlocfilehash: 30ca62305c325943e17ee574873cfbbbaee97f77
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86113046"
---
# <a name="configure-and-access-audit-logs-in-the-azure-portal"></a>Azure portal denetim günlüklerini yapılandırma ve erişme

[MariaDB Için Azure veritabanı denetim günlükleri](concepts-audit-logs.md) ve tanılama ayarları Azure portal yapılandırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:

- [MariaDB sunucusu için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

>[!IMPORTANT]
> Sunucunuzun performansının yoğun bir şekilde etkilenmemesini sağlamak için, yalnızca denetim amaçlarıyla gerekli olay türlerini ve kullanıcıları günlüğe kaydetmek önerilir.

Denetim günlüğünü etkinleştirin ve yapılandırın.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. MariaDB sunucusu için Azure veritabanınızı seçin.

1. Kenar çubuğu 'ndaki **Ayarlar** bölümünde **sunucu parametreleri**' ni seçin.
    ![Sunucu parametreleri](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. **Audit_log_enabled** parametresini açık olarak güncelleştirin.
    ![Denetim günlüklerini etkinleştir](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. **Audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    ![Denetim günlüğü olayları](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. **Audit_log_exclude_users** parametresi güncelleştirilerek günlüğe kaydetme dışında tutulacak MariaDB kullanıcılarını ekleyin. Kullanıcıları, MariaDB kullanıcı adlarını girerek belirtin.
    ![Denetim günlüğü kullanıcıları hariç tut](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Parametreleri değiştirildikten sonra **Kaydet**' e tıklayabilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .
    ![Kaydet](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları**' nı seçin.

1. "+ Tanılama ayarı Ekle" ![ Tanılama ayarı Ekle ' ye tıklayın.](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Bir tanılama ayarı adı belirtin.

1. Denetim günlüklerinin (depolama hesabı, Olay Hub 'ı ve/veya Log Analytics çalışma alanı) gönderileceği veri havuzlarını belirtin.

1. Günlük türü olarak "MySqlAuditLogs" öğesini seçin.
![Tanılama ayarını Yapılandır](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Denetim günlüklerini için veri havuzlarını yapılandırdıktan sonra **Kaydet**' e tıklayabilirsiniz.
![Tanılama ayarını kaydet](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek denetim günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- MariaDB için Azure veritabanı 'nda [Denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin
- [Azure CLI](howto-configure-audit-logs-cli.md) 'de denetim günlüklerini yapılandırma hakkında bilgi edinin