---
title: Erişim denetim günlükleri-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal MySQL için Azure veritabanı 'nda denetim günlüklerinin nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: ff1a6c63b6eb99acdef955806a138e3e22b8902a
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74773719"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql-in-the-azure-portal"></a>Azure portal MySQL için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme

[MySQL Için Azure veritabanı denetim günlükleri](concepts-audit-logs.md) ve tanılama ayarları Azure portal yapılandırabilirsiniz.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği Şu anda önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:

- [MySQL için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

Denetim günlüğünü etkinleştirin ve yapılandırın.

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

1. MySQL için Azure veritabanı sunucunuzu seçin.

1. Kenar çubuğu 'ndaki **Ayarlar** bölümünde **sunucu parametreleri**' ni seçin.
    ![Sunucu parametreleri](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. **Audit_log_enabled** parametresini açık olarak güncelleştirin.
    Denetim günlüklerini etkinleştirmek ![](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. **Audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    Denetim günlüğü olaylarını ![](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. **Audit_log_exclude_users** parametresini güncelleştirerek, günlüğe kaydetme dışında tutulacak MySQL kullanıcılarını ekleyin. Kullanıcıları MySQL Kullanıcı adlarını girerek belirtin.
    ![denetim günlüğü kullanıcıları hariç tut](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Parametreleri değiştirildikten sonra **Kaydet**' e tıklayabilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .
    ![](./media/howto-configure-audit-logs-portal/save-parameters.png) Kaydet

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları**' nı seçin.

1. "+ Tanılama ayarı Ekle" ![tanılama ayarı Ekle ' ye tıklayın](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Bir tanılama ayarı adı belirtin.

1. Denetim günlüklerinin (depolama hesabı, Olay Hub 'ı ve/veya Log Analytics çalışma alanı) gönderileceği veri havuzlarını belirtin.

1. Günlük türü olarak "MySqlAuditLogs" öğesini seçin.
Tanılama ayarını yapılandırma ![](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Denetim günlüklerini için veri havuzlarını yapılandırdıktan sonra **Kaydet**' e tıklayabilirsiniz.
Tanılama ayarını ![](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png) Kaydet

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek denetim günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- MySQL için Azure veritabanı 'nda [Denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin.