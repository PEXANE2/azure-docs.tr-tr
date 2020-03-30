---
title: Log Analytics çalışma alanında Azure etkinlik günlüğü toplama
description: Azure Monitör Günlüklerinde Azure Etkinlik Günlüğü'nün toplanmasını ve tüm Azure aboneliklerinizde Azure etkinlik günlüğünü analiz etmek ve arama yapmak için izleme çözümünü kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/30/2019
ms.openlocfilehash: 407bff10e2480c5210d3057bcccd6c60e591c165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055316"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Azure Monitor'da Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve analiz

> [!WARNING]
> Artık, kaynak günlüklerini nasıl topladığınıza benzer bir tanıa ayarını kullanarak Günlük Analizi çalışma alanında Etkinlik günlüğü toplayabilirsiniz. Azure [Monitörü'nde Günlük Analizi çalışma alanında Azure etkinlik günlüklerini topla ve analiz edin.](diagnostic-settings-legacy.md)

[Azure Etkinlik Günlüğü,](platform-logs-overview.md) Azure aboneliğinizde gerçekleşen abonelik düzeyindeki olaylara ilişkin öngörüler sağlar. Bu makalede, Bir Günlük Analizi çalışma alanında Etkinlik Günlüğü'nün nasıl toplandığı ve bu verileri çözümlemek için günlük sorguları ve görünümleri sağlayan Activity Log Analytics [izleme çözümünü](../insights/solutions.md)nasıl kullanacağı açıklanmaktadır. 

Etkinlik Günlüğü'nün Bir Günlük Analizi çalışma alanına bağlanması aşağıdaki avantajları sağlar:

- Etkinlik Günlüğü'nün birden çok Azure aboneliğinden analiz için tek bir konumda birleştirilmesi.
- Etkinlik Günlüğü girişlerini 90 günden uzun süre saklayın.
- Etkinlik Günlüğü verilerini Azure Monitor tarafından toplanan diğer izleme verileriyle ilişkilendirin.
- Karmaşık çözümleme gerçekleştirmek ve Etkinlik Günlüğü girişleri hakkında derin bilgiler edinmek için [günlük sorgularını](../log-query/log-query-overview.md) kullanın.

## <a name="connect-to-log-analytics-workspace"></a>Log Analytics çalışma alanına bağlanın
Aynı Azure kiracısında birden çok abonelik için tek bir çalışma alanı Etkinlik Günlüğü'ne bağlanabilir. Birden çok kiracı arasında tahsilat için [bkz.](activity-log-collect-tenants.md)

> [!IMPORTANT]
> Microsoft.OperationalInsights ve Microsoft.OperationsManagement kaynak sağlayıcıları aboneliğiniz için kayıtlı değilse, aşağıdaki yordamda bir hata alabilirsiniz. Bu sağlayıcıları kaydetmek için [Azure kaynak sağlayıcılarına ve türlerine](../../azure-resource-manager/management/resource-providers-and-types.md) bakın.

Etkinlik Günlüğü'nün Günlük Analizi çalışma alanınıza bağlanması için aşağıdaki yordamı kullanın:

1. Azure portalındaki **Günlük Analizi çalışma alanları** menüsünden Etkinlik Günlüğü'nü toplamak için çalışma alanını seçin.
1. Çalışma **alanı** menüsünün Çalışma Alanı Veri Kaynakları bölümünde Azure **Etkinliği günlüğünü**seçin.
1. Bağlanmak istediğiniz aboneliği tıklatın.

    ![Çalışma Alanları](media/activity-log-export/workspaces.png)

1. Abonelikteki Etkinlik günlüğünü seçili çalışma alanına bağlamak için **Bağlan'ı** tıklatın. Abonelik zaten başka bir çalışma alanına bağlıysa, bağlantısını kesmek için önce **Bağlantıyı Kes'i** tıklatın.

    ![Çalışma Alanlarını Bağla](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Log Analytics çalışma alanında analiz edin
Bir Etkinlik Günlüğü'nün Günlük Analizi çalışma alanına bağlandığınızda, girişler çalışma alanına bir [günlük sorgusuyla](../log-query/log-query-overview.md)alabileceğiniz **AzureActivity** adlı bir tabloya yazılır. Bu tablonun yapısı günlük girişi [kategorisine](activity-log-view.md#categories-in-the-activity-log)bağlı olarak değişir. Her kategorinin açıklaması için [Azure Etkinlik Günlüğü etkinlik şemasına](activity-log-schema.md) bakın.

## <a name="activity-logs-analytics-monitoring-solution"></a>Aktivite Günlükleri Analiz izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki Etkinlik Günlüğü kayıtlarını analiz etmek için birden çok günlük sorgusu ve görünüm içerir.

### <a name="install-the-solution"></a>Çözümü yükleyin
**Etkinlik Günlüğü Analizi** çözümünü yüklemek için bir izleme çözümü [yükleyin](../insights/solutions.md#install-a-monitoring-solution) yordamını kullanın. Ek yapılandırma gerekmez.

### <a name="use-the-solution"></a>Çözümü kullanın
İzleme çözümlerine Azure portalındaki **Monitör** menüsünden erişilir. Çözüm kutucuklarıyla **Genel Bakış** sayfasını açmak için **Öngörüler** bölümünde **Daha Fazla'yı** seçin. **Azure Etkinlik Günlükleri döşemesi,** çalışma alanınızdaki **Azure Etkinliği** kayıtlarının sayısını görüntüler.

![Azure Etkinlik Günlükleri döşeme](media/collect-activity-logs/azure-activity-logs-tile.png)


Azure **Etkinlik Günlükleri** görünümünü açmak için Azure Etkinlik **Günlükleri** döşemesini tıklatın. Görünüm aşağıdaki tabloda görselleştirme bölümlerini içerir. Her parça, belirtilen zaman aralığı için bu parçaların ölçütleriyle eşleşen en fazla 10 öğeyi listeler. Bölümün altındaki **tümlerini gör'e** tıklayarak eşleşen tüm kayıtları döndüren bir günlük sorgusu çalıştırabilirsiniz.

![Azure Etkinlik Günlükleri panosu](media/collect-activity-logs/activity-log-dash.png)

| Görselleştirme bölümü | Açıklama |
| --- | --- |
| Azure Etkinlik Günlüğü Girişleri | Seçtiğiniz tarih aralığıiçin en üst Azure Etkinliği Günlüğü giriş toplamlarının çubuk grafiğini gösterir ve en iyi 10 etkinlik arayanların listesini gösterir. Bir günlük aramasını çalıştırmak için `AzureActivity`çubuk grafiği tıklatın. Bu öğeiçin tüm Etkinlik Günlüğü girişlerini döndüren bir günlük araması çalıştırmak için bir arayan öğesini tıklatın. |
| Duruma Göre Etkinlik Günlükleri | Seçili tarih aralığı için Azure Etkinlik Günlüğü durumu için bir halka grafiği ve en iyi on durum kaydının listesini gösterir. Bir günlük sorgusunu çalıştırmak `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`için grafiği tıklatın. Bu durum kaydı için tüm Etkinlik Günlüğü girişlerini döndüren bir günlük araması çalıştırmak için bir durum öğesini tıklatın. |
| Kaynağa Göre Etkinlik Günlükleri | Etkinlik Günlükleri ile toplam kaynak sayısını gösterir ve her kaynak için kayıt sayıları ile ilk on kaynağı listeler. Çözümde kullanılabilen tüm Azure `AzureActivity | summarize AggregatedValue = count() by Resource`kaynaklarını gösteren bir günlük araması çalıştırmak için toplam alanı tıklatın. Bu kaynak için tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için bir kaynak tıklatın. |
| Kaynak Sağlayıcıya Göre Etkinlik Günlükleri | Etkinlik Günlükleri üreten toplam kaynak sağlayıcısı sayısını gösterir ve ilk on'u listeler. Tüm Azure kaynak sağlayıcılarını gösteren `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`bir günlük sorgusu çalıştırmak için toplam alanı tıklatın. Sağlayıcının tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için bir kaynak sağlayıcısını tıklatın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik Günlüğü](platform-logs-overview.md)hakkında daha fazla bilgi edinin.
- Azure Monitör [veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- Etkinlik Günlüğünüzdeki ayrıntılı bilgileri görüntülemek için [günlük sorgularını](../log-query/log-query-overview.md) kullanın.
