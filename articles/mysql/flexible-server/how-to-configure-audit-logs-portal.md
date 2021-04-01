---
title: Denetim günlüklerini Yapılandırma-Azure portal-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal MySQL için Azure veritabanı esnek sunucusu 'ndaki denetim günlüklerine nasıl yapılandırılacağı ve erişebileceğiniz açıklanır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 9/29/2020
ms.openlocfilehash: ebb980aa257fc09c3d6a407febbf60f2d1a26a4e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94536481"
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

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Esnek sunucunuzu seçin.

1. Kenar çubuğu 'ndaki **Ayarlar** bölümünde **sunucu parametreleri**' ni seçin.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/server-parameters.png" alt-text="Sunucu parametreleri":::

1. **Audit_log_enabled** parametresini açık olarak güncelleştirin.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/audit-log-enabled.png" alt-text="Denetim günlüklerini etkinleştir":::

1. **Audit_log_events** parametresini güncelleştirerek günlüğe kaydedilecek [olay türlerini](concepts-audit-logs.md#configure-audit-logging) seçin.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/audit-log-events.png" alt-text="Denetim günlüğü olayları":::

1. **Audit_log_exclude_users** ve **audit_log_include_users** parametrelerini güncelleştirerek günlük kaydıyla dahil edilecek veya hariç tutulacak MySQL kullanıcılarını ekleyin. Kullanıcıları MySQL Kullanıcı adlarını girerek belirtin.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/audit-log-exclude-users.png" alt-text="Denetim günlüğü kullanıcıları hariç tut":::

1. Parametreleri değiştirildikten sonra **Kaydet**' e tıklayabilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/save-parameters.png" alt-text="Kaydet":::

## <a name="set-up-diagnostics"></a>Tanılamayı ayarla

Denetim günlükleri, günlüklerinizi Azure Izleyici günlüklerine, Event Hubs veya Azure depolama 'ya yönelterek Azure Izleyici tanılama ayarlarıyla tümleştirilir.

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları**' nı seçin.

1. "+ Tanılama ayarı Ekle" :::image type="content" source="./media/how-to-configure-audit-logs-portal/add-diagnostic-setting.png" alt-text="Tanılama ayarı Ekle"::: ' ye tıklayın.

1. Bir tanılama ayarı adı belirtin.

1. Denetim günlüklerinin hangi hedeflere gönderileceğini belirtin (depolama hesabı, Olay Hub 'ı ve/veya Log Analytics çalışma alanı).

1. Günlük türü olarak **MySqlAuditLogs** öğesini seçin.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/configure-diagnostic-setting.png" alt-text="Tanılama ayarını Yapılandır":::

1. Denetim günlüklerini için veri havuzlarını yapılandırdıktan sonra **Kaydet**' e tıklayabilirsiniz.
    :::image type="content" source="./media/how-to-configure-audit-logs-portal/save-diagnostic-setting.png" alt-text="Tanılama ayarını kaydet":::

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek denetim günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

Denetim günlüklerinizi Azure Izleyici günlüklerine (Log Analytics) aldıysanız, analiz için kullanabileceğiniz bazı [örnek sorgulara](concepts-audit-logs.md#analyze-logs-in-azure-monitor-logs) bakın.  

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim günlükleri](concepts-audit-logs.md) hakkında daha fazla bilgi edinin
- [Yavaş sorgu günlükleri](concepts-slow-query-logs.md) hakkında bilgi edinin
<!-- - Learn how to configure audit logs in the [Azure CLI](howto-configure-audit-logs-cli.md)-->