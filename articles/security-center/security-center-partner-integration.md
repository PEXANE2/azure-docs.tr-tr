---
title: Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri | Microsoft Docs
description: Azure Güvenlik Merkezi'nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: 9d52526f87f20a855c045f4e565793ac29abf04a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70101006"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

> [!NOTE]
> Güvenlik çözümlerinin bir alt kümesi 31 Temmuz 2019 tarihinde kullanımdan kaldırılmıştır. Daha fazla bilgi ve diğer hizmetler için bkz. [Güvenlik Merkezi özelliklerini devre dışı bırakma (2019 Temmuz)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolaylaştırılabilir sağlamasını sağlar. Güvenlik Merkezi, kötü amaçlı yazılımdan koruma ve güvenlik açığı değerlendirmesi gibi çözümler için gerekli aracıyı sanal makinelerinize sağlayabilir. Güvenlik duvarı cihazları için ise Güvenlik Merkezi gerekli ağ yapılandırmasının çoğunluğunu gerçekleştirebilir.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetimi**: Müşteriler, tümleşik sistem durumu olaylarını kullanarak bir bakışta tüm iş ortağı çözümlerini izleyebilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Şu anda, tümleşik güvenlik çözümleri [Qualys](https://www.qualys.com/public-cloud/#azure) ve [Rapid7](https://www.rapid7.com/products/insightvm/) ve Microsoft Application Gateway Web uygulaması güvenlik duvarı tarafından güvenlik açığı değerlendirmesi içerir.

> [!NOTE]
> Güvenlik cihazı satıcılarının çoğu dış aracıların cihazlarında çalışmasını engellediğinden Güvenlik Merkezi, Microsoft Monitoring Agent uygulamasını iş ortağı sanal cihazlarına yüklemez.
>
>

## <a name="how-security-solutions-are-integrated"></a>Güvenlik çözümlerinin tümleştirilme şekli
Güvenlik Merkezinden dağıtılan Azure güvenlik çözümleri otomatik olarak bağlanır. Ayrıca, şirket içinde veya diğer bulutlarda çalışan bilgisayarlar da dahil olmak üzere diğer güvenlik veri kaynaklarını da bağlayabilirsiniz.

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Tümleşik Azure güvenlik çözümlerini ve diğer veri kaynaklarını yönetme

1. [Azure Portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

3. Güvenlik Merkezi menüsünde **Güvenlik çözümleri**’ni seçin.

   ![Güvenlik Merkezine Genel Bakış](./media/security-center-partner-integration/overview.png)

**Güvenlik çözümleri** altında tümleşik Azure güvenlik çözümlerinin sistem durumu hakkındaki bilgileri görüntüleyebilir ve temel yönetim görevlerini gerçekleştirebilirsiniz.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlantılı çözümler** bölümünde Güvenlik Merkezi'ne bağlı olan güvenlik çözümleri ve her çözümün sistem durumu hakkında bilgiler yer alır.  

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

İş ortağı çözümünün durumu şu olabilir:

* Sağlıklı (yeşil)-sistem durumu sorunu yok.
* Sağlıksız (kırmızı) - sistem durumunda hemen ilgilenilmesi gereken bir sorun var.
* Sağlık sorunları (turuncu)-çözüm, sistem durumunu raporlamayı durdurdu.
* Bildirilmedi (gri)-çözüm henüz bir şey bildirmediyse, bir çözümün durumu yakın zamanda bağlandıysa ve hala dağıtıyor ya da bir sistem durumu verisi kullanılabilir değilse rapor edilmemiş olabilir.

> [!NOTE]
> Sağlık durumu verileri yoksa, Güvenlik Merkezi, çözümün raporlama yapılıp yapılmayacağını göstermek için alınan son olayın tarih ve saatini gösterir. Bir sistem durumu verisi yoksa ve son 14 gün içinde hiçbir uyarı alınmıyorsa, Güvenlik Merkezi, çözümün sağlıksız olduğunu veya bildirilmediğini belirtir.
>
>

1. Ek bilgi ve seçenekler için **Görünüm** ' ü seçin ve şunları içerir:

   - **Çözüm konsolu**. Bu çözüm için yönetim deneyimini açar.
   - **VM 'Yi bağlayın**. Uygulamaları bağla dikey penceresini açar. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
   - **Çözümü silin**.
   - Öğesini **yapılandırın**.

   ![İş ortağı çözümü ayrıntısı](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Keşfedilen çözümler

Güvenlik Merkezi, Azure’da çalışmasına karşın Güvenlik Merkezi’ne bağlı olmayan güvenlik çözümlerini otomatik olarak bulur ve çözümleri **Bulunan çözümler** bölümünde gösterir. Buna [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) gibi Azure çözümlerinin yanı sıra iş ortağı çözümleri dahildir.

> [!NOTE]
> Güvenlik Merkezi’nin Standart katmanı, bulunan çözümler özelliği için abonelik düzeyinde gereklidir. Güvenlik fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

Güvenlik Merkezi ile tümleştirmek ve güvenlik uyarılarını bildirim olarak almak üzere bir çözümün altında **BAĞLAN** öğesini seçin.

![Keşfedilen çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

### <a name="add-data-sources"></a>Veri kaynakları ekleyin

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Verileri SıEM 'ye aktarma

Azure Güvenlik Merkezi tarafından üretilen işlenen olaylar, Azure Izleyici aracılığıyla kullanılabilen günlük türlerinden biri olan Azure [etkinlik günlüğünde](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)yayımlanır. Azure Izleyici, izleme verilerinizin herhangi birini SıEM aracında yönlendirmek için birleştirilmiş bir işlem hattı sunar. Bu, daha sonra bir iş ortağı aracına çekilecek bir olay hub 'ına veri akışı yaparak yapılır.

Bu kanal Azure ortamınızdan izleme verilerine erişim elde etmek için [Azure izleme tek](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) işlem hattını kullanır. Bu, verileri tüketmek için kolayca Sıems ve izleme araçlarını ayarlamanıza olanak sağlar.

Sonraki bölümlerde, verileri bir olay hub 'ına akışa almak üzere nasıl yapılandırabileceğiniz açıklanır. Adımlarda Azure aboneliğinizde zaten Azure Güvenlik Merkezi 'nin yapılandırıldığı varsayılır.

Yüksek düzey genel bakış

![Üst düzey genel bakış](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>SıEM 'ye sunulan Azure Güvenlik verileri nedir?

Bu sürümde [güvenlik uyarılarını](../security-center/security-center-managing-and-responding-alerts.md) kullanıma sunduk. Yaklaşan sürümlerde, veri kümesini güvenlik önerileri ile Zenginleştireceğiz.

### <a name="how-to-setup-the-pipeline"></a>İşlem hattını ayarlama

#### <a name="create-an-event-hub"></a>Event Hub'ı oluşturma

Başlamadan önce [bir Event Hubs ad alanı oluşturmanız](../event-hubs/event-hubs-create.md)gerekir. Bu ad alanı ve Olay Hub 'ı tüm izleme verilerinizin hedefdir.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Azure etkinlik günlüğünü Event Hubs akışa almak

Lütfen [Event Hubs için aşağıdaki makale akışı etkinlik günlüğüne](../azure-monitor/platform/activity-logs-stream-event-hubs.md) bakın

#### <a name="install-a-partner-siem-connector"></a>İş ortağı SıEM bağlayıcısını yükler 

İzleme verilerinizi Azure Izleyici ile bir olay hub 'ına yönlendirme, iş ortağı SıEM ve izleme araçlarıyla kolayca tümleştirmenize olanak sağlar.

[Desteklenen Sıems](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub) listesini görmek için aşağıdaki bağlantıya başvurun

### <a name="example-for-querying-data"></a>Veri sorgulama örneği 

Uyarı verilerini çekmek için kullanabileceğiniz birkaç splunk sorgusu aşağıda verilmiştir:

| **Sorgunun açıklaması** | **Sorgu** |
|----|----|
| Tüm Uyarılar| index = ana Microsoft. güvenlik/konumlar/uyarılar|
| İşlem sayısını adlarıyla özetleme| Index = Main SourceType = "Amal: güvenlik" \| tablo OperationName \| istatistik sayısı/OperationName|
| Uyarı bilgilerini al: Saat, ad, durum, KIMLIK ve abonelik | index = ana Microsoft. Security/Locations/Alerts \| tablosu \_saati, Properties. eventName, eyalet, Properties. operationId, am_subscriptionId |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
