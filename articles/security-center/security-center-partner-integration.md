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
ms.date: 04/19/2020
ms.author: memildin
ms.openlocfilehash: 48869140ba8cd1a9598562b0057b0005d8fcd9c7
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758063"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolay bir şekilde sağlanmasını mümkün kılar. Kötü amaçlı yazılımdan koruma ve güvenlik açığı değerlendirmesi gibi çözümler için, Güvenlik Merkezi aracıyı sanal makinelerinizde sağlayabilir. Güvenlik duvarı cihazları için, Güvenlik Merkezi gereken ağ yapılandırmasının çoğunu halledebilir.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetim**: Müşteriler tüm iş ortağı çözümlerini bir bakışta izlemek için tümleşik sistem durumu olaylarını kullanabilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Şu anda, tümleşik güvenlik çözümleri [Qualys](https://www.qualys.com/public-cloud/#azure) ve [Rapid7](https://www.rapid7.com/products/insightvm/) ve Microsoft Application Gateway Web uygulama güvenlik duvarı tarafından güvenlik açığı değerlendirmesi içerir.

> [!NOTE]
> Güvenlik Merkezi, log analytics aracısını iş ortağı sanal cihazlara yüklemez, çünkü çoğu güvenlik satıcısı harici aracıların cihazlarında çalışmasını yasaklar.

Qualys'in güvenlik açığı tarama araçlarının entegrasyonu hakkında daha fazla bilgi edinmek için, standart katman müşterilerinin kullanabileceği yerleşik bir tarayıcı da dahil olmak üzere bkz: 

- [Sanal makineler için entegre güvenlik açığı tarayıcı.](built-in-vulnerability-assessment.md)
- [İş ortağı güvenlik açığı tarama çözümlerini dağıtma.](partner-vulnerability-assessment.md)

Güvenlik Merkezi, aşağıdakiler için güvenlik açığı çözümlemesi de sunar:

* SQL veritabanları - bkz. [güvenlik açığı değerlendirme panosundaki güvenlik açığı değerlendirme raporlarını keşfedin](security-center-iaas-advanced-data.md#explore-vulnerability-assessment-reports)
* Azure Kapsayıcı Kayıt Defteri resimleri - [Bkz. Güvenlik Merkezi ile Azure Kapsayıcı Kayıt Defteri tümleştirmesi (Önizleme)](azure-container-registry-integration.md)

## <a name="how-security-solutions-are-integrated"></a>Güvenlik çözümlerinin tümleştirilme şekli
Güvenlik Merkezinden dağıtılan Azure güvenlik çözümleri otomatik olarak bağlanır. Şirket içinde veya diğer bulutlarda çalışan bilgisayarlar da dahil olmak üzere diğer güvenlik veri kaynaklarını da bağlayabilirsiniz.

[![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-solutions-page.png)](./media/security-center-partner-integration/security-solutions-page.png#lightbox)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Tümleşik Azure güvenlik çözümlerini ve diğer veri kaynaklarını yönetme

1. Azure [portalından](https://azure.microsoft.com/features/azure-portal/)açık **Güvenlik Merkezi.**

1. Güvenlik Merkezi'nin menüsünden **Güvenlik çözümlerini**seçin.

Güvenlik **çözümleri** sayfasından, tümleşik Azure güvenlik çözümlerinin durumunu görebilir ve temel yönetim görevlerini çalıştırabilirsiniz.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlı çözümler** bölümü, şu anda Güvenlik Merkezi'ne bağlı olan güvenlik çözümlerini içerir. Ayrıca her çözümün sağlık durumunu gösterir.  

![Bağlantılı çözümler](./media/security-center-partner-integration/connected-solutions.png)

İş ortağı çözümünün durumu şu olabilir:

* **Sağlıklı** (yeşil) - hiçbir sağlık sorunları.
* **Sağlıksız** (kırmızı) - acil müdahale gerektiren bir sağlık sorunu var.
* **Raporlamayı durdurdu** (turuncu) - çözüm sağlık durumunu bildirmeyi durdurdu.
* **Rapor edilmedi** (gri) - çözüm henüz bir şey bildirmedi ve hiçbir sağlık verisi yok. Bir çözümün durumu yakın zamanda bağlanmışsa ve hala dağıtılmaya devam ediyorsa bildirilmemiş olabilir.

> [!NOTE]
> Sistem durumu verileri kullanılamıyorsa, Güvenlik Merkezi çözümün raporlama olup olmadığını belirtmek için alınan son olayın tarih ve saatini gösterir. Son 14 gün içinde herhangi bir sistem durumu verisi yoksa ve son 14 gün içinde uyarı alınmazsa, Güvenlik Merkezi çözümün sağlıksız olduğunu veya raporlamadığını gösterir.
>
>

Ek bilgi ve seçenekler için **VIEW'ı** seçin:

   - **Çözüm konsolu** - Bu çözüm için yönetim deneyimini açar.
   - **Link VM** - Bağlantı Uygulamaları sayfasını açar. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
   - **Çözümü sil**
   - **Yapılandır**

   ![İş ortağı çözümü ayrıntısı](./media/security-center-partner-integration/partner-solutions-detail.png)


### <a name="discovered-solutions"></a>Bulunan çözümler

Güvenlik Merkezi, Azure'da çalışan ancak Güvenlik Merkezi'ne bağlı olmayan güvenlik çözümlerini otomatik olarak keşfeder ve çözümleri **Keşfedilen çözümler** bölümünde görüntüler. Bu çözümler, Azure [AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)ve iş ortağı çözümleri gibi Azure çözümlerini içerir.

> [!NOTE]
> Güvenlik Merkezi’nin Standart katmanı, bulunan çözümler özelliği için abonelik düzeyinde gereklidir. Fiyatlandırma katmanları hakkında daha fazla bilgi edinmek için [Fiyatlandırma'ya](security-center-pricing.md) bakın.
>

Güvenlik Merkezi ile tümleştirmek ve güvenlik uyarıları hakkında bilgilendirilmek için bir çözüm altında **CONNECT'i** seçin.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/add-data-sources.png)



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. İlgili bilgiler için aşağıdaki makalelere bakın:

* [Güvenlik uyarılarını ve önerilerini dışa aktar.](continuous-export.md) Azure Sentinel veya başka bir SIEM ile tümleştirmeyi nasıl kuracağız öğrenin.
* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.