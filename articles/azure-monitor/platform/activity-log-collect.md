---
title: Log Analytics çalışma alanında Azure etkinlik günlüklerini toplayın ve çözümleyin | Microsoft Docs
description: Azure etkinlik günlüğünü Azure Izleyici günlüklerine toplayın ve izleme çözümünü kullanarak Azure etkinlik günlüğünü analiz edin ve tüm Azure aboneliklerinizde arayın.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/30/2019
ms.author: bwren
ms.openlocfilehash: c9fdd0c530ca34305aa3a9197c3bb938d4fb9f1f
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72528707"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplayın ve çözümleyin
[Azure etkinlik günlüğü](activity-logs-overview.md) , Azure aboneliğinizde oluşan abonelik düzeyi olaylar hakkında öngörüler sağlar. Bu makalede, Etkinlik günlüğünün bir Log Analytics çalışma alanında nasıl toplanacağı ve bu verileri çözümlemek için günlük sorguları ve görünümler sağlayan Etkinlik Günlüğü Analizi [izleme çözümünün](../insights/solutions.md)nasıl kullanılacağı açıklanır. 

Etkinlik günlüğünü bir Log Analytics çalışma alanına bağlamak aşağıdaki avantajları sağlar:

- Birden çok Azure aboneliğinden etkinlik günlüğünü analiz için tek bir konuma birleştirin.
- Etkinlik günlüğü girdilerini 90 günden daha uzun bir süre için depolayın.
- Etkinlik günlüğü verilerinin Azure Izleyici tarafından toplanan diğer izleme verileriyle ilişkilendirilmesi.
- Karmaşık analizler gerçekleştirmek ve etkinlik günlüğü girişleriyle ilgili ayrıntılı Öngörüler elde etmek için [günlük sorgularını](../log-query/log-query-overview.md) kullanın.

## <a name="connect-to-log-analytics-workspace"></a>Log Analytics çalışma alanına bağlan
Aynı Azure kiracısındaki birden çok abonelik için etkinlik günlüğüne tek bir çalışma alanı bağlanabilir. Birden çok kiracının genelinde koleksiyon için bkz. [Azure etkinlik günlüklerini farklı Azure Active Directory kiracılardaki abonelikler arasında Log Analytics çalışma alanında toplama](activity-log-collect-tenants.md).

> [!IMPORTANT]
> Microsoft. Operationalınsights ve Microsoft. OperationsManagement kaynak sağlayıcılarının aboneliğiniz için kayıtlı olmadığı durumlarda aşağıdaki yordamla bir hata alabilirsiniz. Bu sağlayıcıları kaydetmek için bkz. [Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/resource-manager-supported-services.md) .

Etkinlik günlüğünü Log Analytics çalışma alanınıza bağlamak için aşağıdaki yordamı kullanın:

1. Azure portal **Log Analytics çalışma alanları** menüsünde, etkinlik günlüğünü toplamak için çalışma alanını seçin.
1. Çalışma alanının menüsündeki **çalışma alanı veri kaynakları** bölümünde **Azure etkinlik günlüğü**' nü seçin.
1. Bağlanmak istediğiniz aboneliğe tıklayın.

    ![Çalışma alanları](media/activity-log-export/workspaces.png)

1. Abonelikteki etkinlik günlüğünü seçili çalışma alanına bağlamak için **Bağlan** ' a tıklayın. Abonelik zaten başka bir çalışma alanına bağlıysa bağlantıyı kesmek için önce **bağlantıyı kes** ' e tıklayın.

    ![Çalışma alanlarını bağlama](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Log Analytics çalışma alanında Çözümle
Bir etkinlik günlüğünü Log Analytics çalışma alanına bağladığınızda, girdiler çalışma alanına, bir [günlük sorgusuyla](../log-query/log-query-overview.md)alabileceğiniz **AzureActivity** adlı bir tabloya yazılır. Bu tablonun yapısı, [günlük girişi kategorisine](activity-logs-overview.md#categories-in-the-activity-log)bağlı olarak değişir. Her kategorinin açıklaması için bkz. [Azure etkinlik günlüğü olay şeması](activity-log-schema.md) .

## <a name="activity-logs-analytics-monitoring-solution"></a>Etkinlik günlükleri analitik izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki etkinlik günlüğü kayıtlarını çözümlemek için birden çok günlük sorgusu ve görünümü içerir.

### <a name="install-the-solution"></a>Çözümü yükler
**Etkinlik günlüğü Analizi** çözümünü yüklemek için [bir izleme çözümü yüklerken](../insights/solutions.md#install-a-monitoring-solution) yordamı kullanın. Ek yapılandırma gerekmez.

### <a name="use-the-solution"></a>Çözümü kullanma
**İzleme çözümlerine Azure Portal izleme menüsünden erişilir** . **Genel bakış** sayfasını çözüm kutucukları ile açmak için **Öngörüler** bölümünde **daha fazla** ' yı seçin. **Azure etkinlik günlükleri** kutucuğu, çalışma alanınızdaki **AzureActivity** kayıt sayısının sayısını görüntüler.

![Azure etkinlik günlükleri kutucuğu](media/collect-activity-logs/azure-activity-logs-tile.png)


Azure **etkinlik günlükleri görünümünü açmak** için Azure etkinlik **günlükleri** kutucuğuna tıklayın. Görünüm, aşağıdaki tablodaki görselleştirme parçalarını içerir. Her bölümde, belirtilen zaman aralığı için parçaların ölçütlerine uyan 10 ' a kadar öğe listelenir. Bölümün en altında bulunan **Tümünü görüntüle ' ye** tıklayarak eşleşen tüm kayıtları döndüren bir günlük sorgusu çalıştırabilirsiniz.

![Azure etkinlik günlükleri panosu](media/collect-activity-logs/activity-log-dash.png)

| Görselleştirme bölümü | Açıklama |
| --- | --- |
| Azure etkinlik günlüğü girdileri | Seçtiğiniz tarih aralığı için en üstteki Azure etkinlik günlüğü girişi kayıt toplamlarını gösteren bir çubuk grafiği gösterir ve ilk 10 etkinlik çağıranlarının bir listesini gösterir. @No__t_0 için bir günlük araması çalıştırmak için çubuk grafiğine tıklayın. Bu öğe için tüm etkinlik günlüğü girdilerini döndüren bir günlük araması çalıştırmak için bir arayan öğesine tıklayın. |
| Duruma göre etkinlik günlükleri | Seçili tarih aralığı için Azure etkinlik günlüğü durumu ve ilk on durum kayıtlarının listesi için bir halka grafiği gösterir. @No__t_0 günlük sorgusu çalıştırmak için grafiğe tıklayın. Bu durum kaydı için tüm etkinlik günlüğü girdilerini döndüren bir günlük araması çalıştırmak için bir durum öğesine tıklayın. |
| Kaynağa göre etkinlik günlükleri | Etkinlik günlüklerinin bulunduğu toplam kaynak sayısını gösterir ve her bir kaynak için kayıt sayısı olan ilk on kaynağı listeler. Çözüm için kullanılabilen tüm Azure kaynaklarını gösteren `AzureActivity | summarize AggregatedValue = count() by Resource` için bir günlük araması çalıştırmak için Toplam alanına tıklayın. Kaynak için tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için kaynağa tıklayın. |
| Kaynak sağlayıcısına göre etkinlik günlükleri | Etkinlik günlükleri üreten ve ilk on olan kaynak sağlayıcılarının toplam sayısını gösterir. Tüm Azure Kaynak sağlayıcılarını gösteren `AzureActivity | summarize AggregatedValue = count() by ResourceProvider` bir günlük sorgusu çalıştırmak için Toplam alanına tıklayın. Sağlayıcıya yönelik tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için bir kaynak sağlayıcısına tıklayın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik günlüğü](activity-logs-overview.md)hakkında daha fazla bilgi edinin.
- [Azure izleyici veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- Etkinlik Günlüğliğinizden ayrıntılı bilgileri görüntülemek için [günlük sorgularını](../log-query/log-query-overview.md) kullanın.
