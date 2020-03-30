---
title: Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri | Microsoft Docs
description: Azure Güvenlik Merkezi'nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: 23a00c766dbb38853c57c91e7f59ec364390c44b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245388"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

> [!NOTE]
> Güvenlik çözümlerinin bir alt kümesi 31 Temmuz 2019'da kullanımdan kaldırıldı. Daha fazla bilgi ve alternatif hizmetler [için, Güvenlik Merkezi'nin Emeklilik özellikleri (Temmuz 2019)](security-center-features-retirement-july2019.md#menu_solutions)bölümüne bakın.

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolay bir şekilde sağlanmasını mümkün kılar. Kötü amaçlı yazılımdan koruma ve güvenlik açığı değerlendirmesi gibi çözümler için, Güvenlik Merkezi aracıyı sanal makinelerinizde sağlayabilir. Güvenlik duvarı cihazları için, Güvenlik Merkezi gereken ağ yapılandırmasının çoğunu halledebilir.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetim**: Müşteriler tüm iş ortağı çözümlerini bir bakışta izlemek için tümleşik sistem durumu olaylarını kullanabilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Şu anda, tümleşik güvenlik çözümleri [Qualys](https://www.qualys.com/public-cloud/#azure) ve [Rapid7](https://www.rapid7.com/products/insightvm/) ve Microsoft Application Gateway Web uygulama güvenlik duvarı tarafından güvenlik açığı değerlendirmesi içerir.

> [!NOTE]
> Güvenlik Merkezi, microsoft izleme aracısını iş ortağı sanal cihazlara yüklemez, çünkü çoğu güvenlik satıcısı harici aracıların cihazlarında çalışmasını yasaklar.
>
>

## <a name="how-security-solutions-are-integrated"></a>Güvenlik çözümlerinin tümleştirilme şekli
Güvenlik Merkezinden dağıtılan Azure güvenlik çözümleri otomatik olarak bağlanır. Şirket içinde veya diğer bulutlarda çalışan bilgisayarlar da dahil olmak üzere diğer güvenlik veri kaynaklarını da bağlayabilirsiniz.

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Tümleşik Azure güvenlik çözümlerini ve diğer veri kaynaklarını yönetme

1. [Azure portalında](https://azure.microsoft.com/features/azure-portal/)oturum açın.

2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

3. Güvenlik Merkezi menüsünde **Güvenlik çözümleri**’ni seçin.

   ![Güvenlik Merkezine Genel Bakış](./media/security-center-partner-integration/overview.png)

**Güvenlik çözümlerinde,** tümleşik Azure güvenlik çözümlerinin durumunu görebilir ve temel yönetim görevlerini çalıştırabilirsiniz.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlı çözümler** bölümü, şu anda Güvenlik Merkezi'ne bağlı olan güvenlik çözümlerini içerir. Ayrıca her çözümün sağlık durumunu gösterir.  

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

İş ortağı çözümünün durumu şu olabilir:

* Sağlıklı (yeşil) - hiçbir sağlık sorunları.
* Sağlıksız (kırmızı) - acil müdahale gerektiren bir sağlık sorunu var.
* Sağlık sorunları (turuncu) - çözüm sağlık raporlama durdu.
* Rapor edilmedi (gri) - çözüm henüz bir şey bildirmedi ve hiçbir sağlık verisi yok. Bir çözümün durumu yakın zamanda bağlanmışsa ve hala dağıtılmaya devam ediyorsa bildirilmemiş olabilir.

> [!NOTE]
> Sistem durumu verileri kullanılamıyorsa, Güvenlik Merkezi çözümün raporlama olup olmadığını belirtmek için alınan son olayın tarih ve saatini gösterir. Son 14 gün içinde herhangi bir sistem durumu verisi yoksa ve son 14 gün içinde uyarı alınmazsa, Güvenlik Merkezi çözümün sağlıksız olduğunu veya raporlamadığını gösterir.
>
>

1. Ek bilgi ve seçenekler için **VIEW'ı** seçin:

   - **Çözüm konsolu**. Bu çözüm için yönetim deneyimini açar.
   - **Bağlantı VM**. Bağlantı Uygulamaları sayfasını açar. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
   - **Çözümü sil.**
   - **Yapılandır.**

   ![İş ortağı çözümü ayrıntısı](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Bulunan çözümler

Güvenlik Merkezi, Azure'da çalışan ancak Güvenlik Merkezi'ne bağlı olmayan güvenlik çözümlerini otomatik olarak keşfeder ve çözümleri **Keşfedilen çözümler** bölümünde görüntüler. Bu çözümler, Azure [AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)ve iş ortağı çözümleri gibi Azure çözümlerini içerir.

> [!NOTE]
> Güvenlik Merkezi’nin Standart katmanı, bulunan çözümler özelliği için abonelik düzeyinde gereklidir. Fiyatlandırma katmanları hakkında daha fazla bilgi edinmek için [Fiyatlandırma'ya](security-center-pricing.md) bakın.
>
>

Güvenlik Merkezi ile tümleştirmek ve güvenlik uyarıları hakkında bilgilendirilmek için bir çözüm altında **CONNECT'i** seçin.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Bir SIEM'e veri verme

> [!NOTE]
> Bir SIEM'e veri aktarmak için daha basit bir yöntemin ayrıntıları (şu anda önizlemede) için, dışa aktarma [güvenlik uyarıları ve önerileri (Önizleme)](continuous-export.md)bölümüne bakın. Yeni yöntem, Etkinlik Günlüğü'nün ara aracı olarak kullanılmasına izin vermez ve Güvenlik Merkezi'nden Olay Hub'larına (ve ardından SIEM'nize) doğrudan dışa aktarmaya izin verir ve güvenlik önerilerinin dışa aktarımını da destekler.


SIEM'lerinizi veya diğer izleme araçlarınızı Azure Güvenlik Merkezi etkinliklerini alacak şekilde yapılandırabilirsiniz.

Azure Güvenlik Merkezi'nden tüm etkinlikler Azure Monitor'un Azure [Etkinliği günlüğüne](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)yayınlanır. Azure Monitor, verileri bir Olay Hub'ına aktarmak için [birleştirilmiş bir ardışık düzen hattı](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) kullanır ve bu denetim aracınıza çekilebilir.

Sonraki bölümlerde, bir olay hub'ına aktak edilecek verileri nasıl yapılandırabileceğiniz açıklanır. Adımlar, Azure aboneliğinizde Azure Güvenlik Merkezi'ni zaten yapılandırdığınızı varsayar.

### <a name="high-level-overview"></a>Yüksek düzey genel bakış

![Üst Düzey genel bakış](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>SIEM'e maruz kalan Azure güvenlik verileri nedir?

Bu sürümde, güvenlik uyarılarını ortaya [çıkarıyoruz.](../security-center/security-center-managing-and-responding-alerts.md) Gelecek sürümlerde, veri kümesini güvenlik önerileri yle zenginleştireceğiz.

### <a name="how-to-set-up-the-pipeline"></a>Boru hattı nasıl ayarlanır?

#### <a name="create-an-event-hub"></a>Olay Hub'ı oluşturma

Başlamadan önce, tüm izleme verilerinizin hedefi olan [Olay Hub'ları ad alanı oluşturun.](../event-hubs/event-hubs-create.md)

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Azure Etkinlik Günlüğünü Etkinlik Hub'larına aktarın

[Olay Hub'larına](../azure-monitor/platform/activity-logs-stream-event-hubs.md)aşağıdaki makale akışı etkinlik günlüğüne bakın.

#### <a name="install-a-partner-siem-connector"></a>İş ortağı siem konektörü yükleme 

İzleme verilerinizi Azure Monitor ile bir Etkinlik Hub'ına yönlendirmeniz, iş ortağınıza ait SIEM ve izleme araçlarıyla kolayca entegre etmenizi sağlar.

[Desteklenen SIEM'lerin](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#partner-tools-with-azure-monitor-integration)listesi için aşağıdaki makaleye bakın.

### <a name="example-for-querying-data"></a>Veri Sorgulama örneği 

Uyarı verilerini çekmek için kullanabileceğiniz bazı Splunk sorguları şunlardır:

| **Sorgunun Açıklaması** | **Sorgu** |
|----|----|
| All Alerts| index=main Microsoft.Security/locations/alerts|
| Operasyon sayısını adlarına göre özetle| index=ana kaynak türü="amal:security" \| \| tablo operasyonuName istatistikleri operationName göre saymak|
| Uyarılar hakkında bilgi alın: Zaman, Ad, Eyalet, Kimlik ve Abonelik | index=main \| Microsoft.Security/locations/alerts \_table time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.