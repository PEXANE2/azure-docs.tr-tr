---
title: Denetim günlüklerini Yapılandırma-Azure portal-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal MySQL için Azure veritabanı esnek sunucusu 'ndaki denetim günlüklerine nasıl yapılandırılacağı ve erişebileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: 1d0687e6cdcc8208218183cb873f9620a5525d37
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942054"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql---flexible-server-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme-esnek sunucu

> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

MySQL için Azure veritabanı esnek sunucu [Denetim günlükleri](concepts-audit-logs.md) ve tanılama ayarları Azure portal yapılandırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımlarda [esnek sunucu](quickstart-create-server-portal.md)olması gerekir.

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

>[!IMPORTANT]
> Sunucunuzun performansının yoğun bir şekilde etkilenmemesini sağlamak için, yalnızca denetim amaçlarıyla gerekli olay türlerini ve kullanıcıları günlüğe kaydetmek önerilir.

Denetim günlüğünü etkinleştirin ve yapılandırın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Esnek sunucunuzu seçin.

1. Kenar çubuğu 'ndaki **Ayarlar** bölümünde **sunucu parametreleri**' ni seçin.
    <!--:::image type="content" source="./media/howto-configure-audit-logs-portal/server-parameters.png" alt-text="Server parameters":::-->

1. **Audit_log_enabled** parametresini açık olarak güncelleştirin.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-enabled.png" alt-text="Enable audit logs":::-->

1. **Audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-events.png" alt-text="Audit log events":::-->

1. **Audit_log_exclude_users** parametresini güncelleştirerek, günlüğe kaydetme dışında tutulacak MySQL kullanıcılarını ekleyin. Kullanıcıları MySQL Kullanıcı adlarını girerek belirtin.
    <!--:::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png" alt-text="Audit log exclude users":::-->

1. Parametreleri değiştirildikten sonra **Kaydet**' e tıklayabilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .
    <!--:::image type="content" source="./media/howto-configure-audit-logs-portal/save-parameters.png" alt-text="Save":::-->

## <a name="set-up-diagnostics"></a>Tanılamayı ayarla

Denetim günlükleri, günlüklerinizi Azure Izleyici günlüklerine, Event Hubs veya Azure depolama 'ya yönelterek Azure Izleyici tanılama ayarlarıyla tümleştirilir.

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları**' nı seçin.

1. "+ Tanılama ayarı Ekle" seçeneğine tıklayın  <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png" alt-text="Add diagnostic setting":::-->

1. Bir tanılama ayarı adı belirtin.

1. Denetim günlüklerinin hangi hedeflere gönderileceğini belirtin (depolama hesabı, Olay Hub 'ı ve/veya Log Analytics çalışma alanı).

1. Günlük türü olarak **MySqlAuditLogs** öğesini seçin.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png" alt-text="Configure diagnostic setting"::: -->

1. Denetim günlüklerini için veri havuzlarını yapılandırdıktan sonra **Kaydet**' e tıklayabilirsiniz.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png" alt-text="Save diagnostic setting":::-->

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek denetim günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

Denetim günlüklerinizi Azure Izleyici günlüklerine (Log Analytics) aldıysanız, analiz için kullanabileceğiniz bazı [örnek sorgulara](concepts-audit-logs.md#analyze-logs-in-azure-monitor-logs) bakın.  

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin
- [Yavaş sorgu günlükleri](concepts-slow-query-logs.md) hakkında bilgi edinin
<!-- - Learn how to configure audit logs in the [Azure CLI](howto-configure-audit-logs-cli.md)-->