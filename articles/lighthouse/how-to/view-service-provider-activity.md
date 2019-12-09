---
title: Hizmet sağlayıcı etkinliğini görüntüle
description: Müşteriler, Azure tarafından atanan kaynak yönetimi aracılığıyla hizmet sağlayıcıları tarafından gerçekleştirilen eylemleri görmek için günlüğe kaydedilen etkinliği görüntüleyebilir.
ms.date: 12/6/2019
ms.topic: conceptual
ms.openlocfilehash: 69517e942aa9f82be16fa3d0e7d6f9252de44d4c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932690"
---
# <a name="view-service-provider-activity"></a>Hizmet sağlayıcı etkinliğini görüntüle

Azure 'un Temsilcili kaynak yönetimi için abonelik temsilcileri olan müşteriler, gerçekleştirilen tüm eylemleri görmek için [Azure etkinlik günlüğü verilerini görüntüleyebilir](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview) . Bu, müşterilere hizmet sağlayıcılarının Azure tarafından atanan kaynak yönetimi aracılığıyla yaptığı ve müşterinin kendi Azure Active Directory (Azure AD) kiracısındaki kullanıcılar tarafından gerçekleştirilen işlemlere tam görünürlük sağlar.

## <a name="view-activity-log-data"></a>Etkinlik günlüğü verilerini görüntüle

[Etkinlik günlüğünü](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview#view-the-activity-log) Azure Portal **Monitor** menüsünden görüntüleyebilirsiniz. Sonuçları belirli bir abonelikle sınırlamak için belirli bir abonelik seçmek üzere filtreleri kullanabilirsiniz. Ayrıca, [etkinlik günlüğü olaylarını programlı bir şekilde görüntüleyebilir ve alabilirsiniz](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) .

> [!NOTE]
> Bir hizmet sağlayıcısının kiracısındaki kullanıcılar, bu abonelik Azure tarafından sağlanan kaynak yönetimi için eklendi olduğunda, [okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) rolü (veya okuyucu erişimi içeren başka bir yerleşik rol) verildiyse, bir müşteri kiracısında bir temsilci abonelik için etkinlik günlüğü sonuçlarını görüntüleyebilir.

Etkinlik günlüğünde işlemin adını ve durumunu, gerçekleştirildiği tarih ve saat ile birlikte görürsünüz. Sütunu **tarafından başlatılan olay** , bir hizmet sağlayıcının kiracısında Azure tarafından atanan kaynak yönetimi veya müşterinin kendi kiracısındaki bir kullanıcı olup olmadığını gösteren, hangi kullanıcının işlemi gerçekleştirdiği anlamına gelir. Kullanıcı adının, kiracı veya kullanıcının bu abonelik için atandığı rol yerine gösterildiğini unutmayın.

Günlüğe kaydedilen etkinlik, son 90 gün için Azure portal kullanılabilir. Bu verileri 90 günden daha uzun bir süre için nasıl depolayacağınızı öğrenmek için bkz. Azure [izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect)

## <a name="set-alerts-for-critical-operations"></a>Kritik işlemler için uyarı ayarlama

Hizmet sağlayıcılarının (veya kendi kiracınızdaki kullanıcıların) gerçekleştirdiği kritik işlemlerden haberdar olmak için, [etkinlik günlüğü uyarıları](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-alerts)oluşturmanızı öneririz. Örneğin, bir abonelik için tüm yönetim eylemlerini izlemek veya belirli bir kaynak grubundaki herhangi bir sanal makine silindiğinde bildirim almak isteyebilirsiniz. Uyarılar oluşturduğunuzda, kullanıcılar tarafından müşterinin kendi kiracısındaki ve tüm kiracıların yönetiminde gerçekleştirilecek eylemler dahil edilir.

Daha fazla bilgi için bkz. [etkinlik günlüğü uyarıları oluşturma ve yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

## <a name="create-log-queries"></a>Günlük sorguları oluştur

Günlüğe kaydedilen etkinliğinizi çözümlemek veya belirli öğelere odaklanmak için sorgular oluşturabilirsiniz. Örneğin, bir denetim, bir abonelikte gerçekleştirilen tüm yönetim düzeyi eylemleri raporlamak için gerekli olabilir. Yalnızca bu eylemleri filtrelemek ve sonuçları Kullanıcı, tarih veya başka bir değere göre sıralamak için bir sorgu oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgularına genel bakış](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/)hakkında daha fazla bilgi edinin.
- Azure portal [hizmet sağlayıcısı tekliflerini görüntülemeyi ve yönetmeyi](view-manage-service-providers.md) öğrenin.