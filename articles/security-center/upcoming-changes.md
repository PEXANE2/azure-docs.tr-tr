---
title: Azure Güvenlik Merkezi 'ne gelen önemli değişiklikler
description: Azure Güvenlik Merkezi 'Nde, planlamanız gerekebilecek ve planlamanız gerekebilen değişiklikler
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2020
ms.author: memildin
ms.openlocfilehash: df863372cbf7abfb6fee145b7d13bb00d8deb074
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94380184"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

### <a name="system-updates-should-be-installed-on-your-machines-recommendation-getting-sub-recommendations"></a>"Alt öneriler alma" makinelerinizde sistem güncelleştirmeleri yüklenmelidir "önerisi

#### <a name="summary"></a>Özet

| Görünüş | Ayrıntılar |
|---------|---------|
|Duyuru tarihi | 9 Kasım 2020  |
|Bu değişikliğin tarihi  |  Orta seviye Kasım 2020 |
|Etki     | Bu öneriye ait geçerli sürümden değiştirme sırasında, güvenli puanınız değişebilir. |
|  |  |

**Makine Önerinizde sistem güncelleştirmelerinin** gelişmiş bir sürümünü yayınlıyoruz. Yeni sürüm, sistem güncelleştirmelerini Uygula güvenlik denetimindeki geçerli sürümün *yerini* alacak ve aşağıdaki geliştirmeleri getirir:

- Eksik her güncelleştirme için alt öneriler
- Azure portal Azure Güvenlik Merkezi sayfalarında yeniden tasarlanan bir deneyim
- Azure Kaynak Grafının önerisi için zenginleştirilmiş veriler

#### <a name="transition-period"></a>Geçiş süresi

36 saat (yaklaşık) geçiş süresi olacaktır. Olası kesintiyi en aza indirmek için, güncelleştirmeyi bir hafta sonu ile gerçekleşecek şekilde zamanlıyoruz. Geçiş sırasında güvenli puanlarınız etkilenebilir.

#### <a name="redesigned-portal-experience"></a>Yeniden tasarlanan Portal deneyimi

**Makinelerinizde sistem güncelleştirmeleri** için öneri ayrıntıları sayfası, aşağıda gösterildiği gibi bulguları listesini içerir. Tek bir bulma seçtiğinizde, Ayrıntılar bölmesi düzeltme bilgilerinin bağlantısını ve etkilenen kaynakların listesini açar.

:::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="Güncelleştirilmiş öneri için Portal deneyimindeki alt önerilerden birini açma":::


#### <a name="richer-data-from-azure-resource-graph"></a>Azure Kaynak grafiğinden daha zengin veriler

Azure Kaynak Grafiği, verimli kaynak araştırması sağlamak üzere tasarlanan Azure hizmetidir. Ortamınızı etkin bir şekilde yönetebilmeniz için, belirli bir abonelik kümesi genelinde ölçeği sorgulamak için bağımsız değişkeni kullanabilirsiniz. 

Azure Güvenlik Merkezi için bağımsız değişken ve [kusto sorgu dili (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) kullanarak çok çeşitli güvenlik sonrası verileri sorgulayabilirsiniz.

Geçerli **sistem güncelleştirmeleri sürümünü makinenizde yüklü olmalıdır**. Bu, bağımsız değişken tarafından kullanılabilen tek bilgi, önerinin bir makinede düzeltilme gereksinimleridir. Güncelleştirilmiş sürüm yayınlandığında, aşağıdaki sorgu makineye göre gruplandırılan tüm eksik sistem güncelleştirmelerini döndürür.

```kusto
securityresources
| where type =~ "microsoft.security/assessments/subassessments"
| where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
| where properties.status.code == "Unhealthy"
```

## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).