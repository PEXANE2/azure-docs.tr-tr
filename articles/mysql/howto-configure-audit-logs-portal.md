---
title: Erişim denetim günlükleri - Azure portalı - MySQL için Azure Veritabanı
description: Bu makalede, Azure portalından MySQL için Azure Veritabanı'ndaki denetim günlüklerinin nasıl yapılandırılabildiğini ve bunlara erişilenler açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 188ef3a1b9777c37f8557a69e19887638a973611
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062538"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql-in-the-azure-portal"></a>Azure portalında MySQL için Azure Veritabanı için denetim günlüklerini yapılandırma ve erişim

Azure portalından [MySQL denetim günlükleri](concepts-audit-logs.md) ve tanılama ayarları için Azure Veritabanını yapılandırabilirsiniz.

> [!IMPORTANT]
> Denetim günlüğü işlevi şu anda önizlemede.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:

- [MySQL sunucusu için Azure Veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Denetim günlüğe kaydetme

Denetim günlüğe kaydetmeyi etkinleştirin ve yapılandırın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. MySQL sunucusu için Azure Veritabanınızı seçin.

1. Kenar çubuğundaki **Ayarlar** bölümünde **Sunucu parametrelerini**seçin.
    ![Sunucu parametreleri](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. **audit_log_enabled** parametresini ON'a güncelleştirin.
    ![Denetim günlüklerini etkinleştirme](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. **audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    ![Denetim günlüğü olayları](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. **audit_log_exclude_users** parametresini güncelleyerek günlük dışı bırakılacak herhangi bir MySQL kullanıcısını ekleyin. MySQL kullanıcı adlarını vererek kullanıcıları belirtin.
    ![Denetim günlüğü kullanıcıları hariç tut](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Parametreleri değiştirdikten sonra **Kaydet'i**tıklatabilirsiniz. Ya da değişikliklerinizi **atabilirsiniz.**
    ![Kaydet](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğundaki **İzleme** bölümünde **Tanılama ayarlarını**seçin.

1. "+ Tanı ayarını ![ekle" üzerine tıklayın Tanılama ayarı ekle](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Tanılama ayarı adı sağlayın.

1. Denetim günlüklerini (depolama hesabı, etkinlik merkezi ve/veya Log Analytics çalışma alanı) gönderecek verileri belirtin.

1. Günlük türü olarak "MySqlAuditLogs"u seçin.
![Tanılama ayarını yapılandırma](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Denetim günlüklerini denetlemek için veri lavabolarını yapılandırdıktan sonra **Kaydet'i**tıklatabilirsiniz.
![Tanı ayarını kaydet](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Yapılattığınız veri lavabolarında bunları keşfederek denetim günlüklerine erişin. Günlüklerin görünmesi 10 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- MySQL için Azure Veritabanı'ndaki [denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin.