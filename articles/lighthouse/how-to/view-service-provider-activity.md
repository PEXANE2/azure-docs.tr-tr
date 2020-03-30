---
title: Hizmet sağlayıcısı etkinliğini görüntüleme
description: Müşteriler, hizmet sağlayıcıların Azure temsilci kaynak yönetimi aracılığıyla gerçekleştirdiği eylemleri görmek için günlüğe kaydedilmiş etkinliği görüntüleyebilir.
ms.date: 01/15/2020
ms.topic: conceptual
ms.openlocfilehash: a923a57ecc94ac15af207c2b8dc8998708b708d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649645"
---
# <a name="view-service-provider-activity"></a>Hizmet sağlayıcısı etkinliğini görüntüleme

Azure temsilci kaynak yönetimi için abonelikleri devralan müşteriler, gerçekleştirilen tüm eylemleri görmek için [Azure Etkinliği günlük](../../azure-monitor/platform/platform-logs-overview.md) verilerini görüntüleyebilir. Bu, müşterilere hizmet sağlayıcıların Azure temsilci kaynak yönetimi aracılığıyla gerçekleştirdiği işlemlerde tam görünürlük ve müşterinin kendi Azure Etkin Dizin (Azure AD) kiracısı içindeki kullanıcılar tarafından gerçekleştirilen işlemler hakkında tam görünürlük sağlar.

> [!TIP]
> Ayrıca, kapsamlar delegasyonunun yönetici kiracıya denetlenebilmek için yerleşik bir ilke tanımı da sayılyız. Daha fazla bilgi için [ortamınızdaki Denetim heyetlerine](view-manage-service-providers.md#audit-delegations-in-your-environment)bakın.

## <a name="view-activity-log-data"></a>Etkinlik günlüğü verilerini görüntüleme

Azure portalındaki **Monitör** menüsünden [etkinlik günlüğünü görüntüleyebilirsiniz.](../../azure-monitor/platform/activity-log-view.md) Sonuçları belirli bir abonelikle sınırlamak için, belirli bir aboneliği seçmek için filtreleri kullanın. Ayrıca etkinlik günlüğü olaylarını programlı olarak [görüntüleyebilir ve alabilirsiniz.](../../azure-monitor/platform/activity-log-view.md)

> [!NOTE]
> Bir hizmet sağlayıcısının kiracısındaki kullanıcılar, bu abonelik Azure temsilcikaynak yönetimi için yerleşik olduğunda [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü (veya Reader erişimi içeren başka bir yerleşik rol) verildiğinde, müşteri kiracısında devralınan bir aboneliğin etkinlik günlüğü sonuçlarını görüntüleyebilir.

Etkinlik günlüğünde, işlemin gerçekleştirildüğü tarih ve saatle birlikte işlemin adını ve durumunu görürsünüz. Sütun **tarafından başlatılan Olay,** hangi kullanıcının işlemi gerçekleştirdiğini, ister bir hizmet sağlayıcısının kiracısında, Azure temsilci kaynak yönetimi aracılığıyla hareket eden bir kullanıcı olsun, ister müşterinin kendi kiracısında bir kullanıcı olsun gösterir. Kiracı veya kullanıcının bu abonelik için atandığı rol yerine kullanıcının adının gösterildiğini unutmayın.

Günlüğe kaydedilmiş etkinlik, son 90 gün boyunca Azure portalında kullanılabilir. Bu verileri 90 günden daha uzun süre nasıl depolayacağımöğrenmek için Log [Analytics çalışma alanında Azure etkinlik günlüklerini topla ve çözümle](../../azure-monitor/platform/activity-log-collect.md)'ye bakın.

> [!NOTE]
> Hizmet sağlayıcısının kullanıcıları etkinlik günlüğünde görünür, ancak bu kullanıcılar ve rol atamaları **Access Denetimi'nde (IAM)** veya API'ler aracılığıyla rol atama bilgilerini alırken gösterilmez.

## <a name="set-alerts-for-critical-operations"></a>Kritik işlemler için uyarıları ayarlama

Servis sağlayıcılarının (veya kendi kiracınızdaki kullanıcıların) gerçekleştirdiği kritik işlemlerden haberdar olmak için [etkinlik günlüğü uyarıları](../../azure-monitor/platform/activity-log-alerts.md)oluşturmanızı öneririz. Örneğin, bir abonelik için tüm yönetim eylemlerini izlemek veya belirli bir kaynak grubundaki herhangi bir sanal makine silindiğinde bilgilendirilmek isteyebilirsiniz. Uyarılar oluşturduğunuzda, bunlar kullanıcılar tarafından gerçekleştirilen eylemleri müşterinin kendi kiracısının yanı sıra tüm yönetici kiracılarda içerir.

Daha fazla bilgi için [bkz.](../../azure-monitor/platform/alerts-activity-log.md)

## <a name="create-log-queries"></a>Günlük sorguları oluşturma

Günlüğe kaydedilmiş etkinliğinizi çözümlemek veya belirli öğelere odaklanmak için sorgular oluşturabilirsiniz. Örneğin, belki de bir denetim, bir abonelik üzerinde gerçekleştirilen tüm yönetim düzeyindeki eylemleri rapor etmenizi gerektirir. Yalnızca bu eylemlere filtre uygulayacak bir sorgu oluşturabilir ve sonuçları kullanıcıya, tarihe veya başka bir değere göre sıralayabilirsiniz.

Daha fazla bilgi için Azure [Monitor'daki günlük sorgularına genel bakış bölümüne](../../azure-monitor/log-query/log-query-overview.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitör](../../azure-monitor/index.yml)hakkında daha fazla bilgi edinin.
- Azure portalında servis sağlayıcı tekliflerini nasıl [görüntüleyip yöneteceklerini](view-manage-service-providers.md) öğrenin.