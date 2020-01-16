---
title: Hizmet sağlayıcısı etkinliğini görüntüleme
description: Müşteriler, Azure tarafından atanan kaynak yönetimi aracılığıyla hizmet sağlayıcıları tarafından gerçekleştirilen eylemleri görmek için günlüğe kaydedilen etkinliği görüntüleyebilir.
ms.date: 01/15/2020
ms.topic: conceptual
ms.openlocfilehash: de149bddb6917a63d91b1890c0430f64465cb40c
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76046101"
---
# <a name="view-service-provider-activity"></a>Hizmet sağlayıcısı etkinliğini görüntüleme

Azure 'un Temsilcili kaynak yönetimi için abonelik temsilcileri olan müşteriler, gerçekleştirilen tüm eylemleri görmek için [Azure etkinlik günlüğü verilerini görüntüleyebilir](../../azure-monitor/platform/platform-logs-overview.md) . Bu, müşterilere hizmet sağlayıcılarının Azure tarafından atanan kaynak yönetimi aracılığıyla yaptığı ve müşterinin kendi Azure Active Directory (Azure AD) kiracısındaki kullanıcılar tarafından gerçekleştirilen işlemlere tam görünürlük sağlar.

## <a name="view-activity-log-data"></a>Etkinlik günlüğü verilerini görüntüle

[Etkinlik günlüğünü](../../azure-monitor/platform/activity-log-view.md) Azure Portal **Monitor** menüsünden görüntüleyebilirsiniz. Sonuçları belirli bir abonelikle sınırlamak için belirli bir abonelik seçmek üzere filtreleri kullanabilirsiniz. Ayrıca, [etkinlik günlüğü olaylarını programlı bir şekilde görüntüleyebilir ve alabilirsiniz](../../azure-monitor/platform/activity-log-view.md) .

> [!NOTE]
> Bir hizmet sağlayıcısının kiracısındaki kullanıcılar, bu abonelik Azure tarafından sağlanan kaynak yönetimi için eklendi olduğunda, [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü (veya okuyucu erişimi içeren başka bir yerleşik rol) verildiyse, bir müşteri kiracısında bir temsilci abonelik için etkinlik günlüğü sonuçlarını görüntüleyebilir.

Etkinlik günlüğünde işlemin adını ve durumunu, gerçekleştirildiği tarih ve saat ile birlikte görürsünüz. Sütunu **tarafından başlatılan olay** , bir hizmet sağlayıcının kiracısında Azure tarafından atanan kaynak yönetimi veya müşterinin kendi kiracısındaki bir kullanıcı olup olmadığını gösteren, hangi kullanıcının işlemi gerçekleştirdiği anlamına gelir. Kullanıcı adının, kiracı veya kullanıcının bu abonelik için atandığı rol yerine gösterildiğini unutmayın.

Günlüğe kaydedilen etkinlik, son 90 gün için Azure portal kullanılabilir. Bu verileri 90 günden daha uzun bir süre için nasıl depolayacağınızı öğrenmek için bkz. Azure [izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme](../../azure-monitor/platform/activity-log-collect.md)

> [!NOTE]
> Hizmet sağlayıcısından kullanıcılar etkinlik günlüğünde görünür, ancak bu kullanıcılar ve rol atamaları **Access Control (IAM)** veya API 'ler aracılığıyla rol atama bilgileri alınırken gösterilmez.

## <a name="set-alerts-for-critical-operations"></a>Kritik işlemler için uyarı ayarlama

Hizmet sağlayıcılarının (veya kendi kiracınızdaki kullanıcıların) gerçekleştirdiği kritik işlemlerden haberdar olmak için, [etkinlik günlüğü uyarıları](../../azure-monitor/platform/activity-log-alerts.md)oluşturmanızı öneririz. Örneğin, bir abonelik için tüm yönetim eylemlerini izlemek veya belirli bir kaynak grubundaki herhangi bir sanal makine silindiğinde bildirim almak isteyebilirsiniz. Uyarılar oluşturduğunuzda, kullanıcılar tarafından müşterinin kendi kiracısındaki ve tüm kiracıların yönetiminde gerçekleştirilecek eylemler dahil edilir.

Daha fazla bilgi için bkz. [etkinlik günlüğü uyarıları oluşturma ve yönetme](../../azure-monitor/platform/alerts-activity-log.md).

## <a name="create-log-queries"></a>Günlük sorguları oluştur

Günlüğe kaydedilen etkinliğinizi çözümlemek veya belirli öğelere odaklanmak için sorgular oluşturabilirsiniz. Örneğin, bir denetim, bir abonelikte gerçekleştirilen tüm yönetim düzeyi eylemleri raporlamak için gerekli olabilir. Yalnızca bu eylemleri filtrelemek ve sonuçları Kullanıcı, tarih veya başka bir değere göre sıralamak için bir sorgu oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgularına genel bakış](../../azure-monitor/log-query/log-query-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici](../../azure-monitor/index.yml)hakkında daha fazla bilgi edinin.
- Azure portal [hizmet sağlayıcısı tekliflerini görüntülemeyi ve yönetmeyi](view-manage-service-providers.md) öğrenin.