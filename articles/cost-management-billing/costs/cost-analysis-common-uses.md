---
title: Azure Maliyet Yönetimi'ndeki yaygın maliyet analizi kullanımları
description: Bu makale, Azure Maliyet Yönetimi'ndeki yaygın maliyet analizi görevlerinden nasıl faydalanabileceğinizi açıklamaktadır.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/21/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: adwise
ms.openlocfilehash: 1d9a498185e76f9771ec98656b1cad914b0f3214
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562590"
---
# <a name="common-cost-analysis-uses"></a>Yaygın maliyet analizi kullanımları

Azure Maliyet Yönetimi kullanıcıları genellikle çoğu kullanıcının sorduğu soruların yanıtlarını bulmak ister. Bu makale, Maliyet Yönetimi'ndeki yaygın maliyet analizi görevlerinden sonuç elde etme konusunda size yol gösterecektir.

## <a name="view-cost-breakdown-by-azure-service"></a>Azure hizmetine göre maliyet dökümünü görüntüleme

Azure hizmetine göre maliyet dökümünü görüntülemek, altyapınızın en yüksek maliyetli bölümlerini daha iyi anlamanıza yardımcı olabilir. Örneğin VM işlem maliyetleri düşük olabilir. Ancak VM'lerden çıkan bilgi miktarı nedeniyle önemli düzeyde ağ maliyetleriyle karşı karşıya olabilirsiniz. Azure hizmetlerinizin temel maliyet noktalarını anlamak, hizmet kullanımında gerekli ayarlamaları yapabilme açısından önemlidir.

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. **Hizmete göre maliyet**'i seçip **Hizmet katmanı** ölçütüne göre gruplandırın.
1. Görünümü **Tablo** olarak değiştirin.

![Azure hizmetine göre dökümü görüntüleme](./media/cost-analysis-common-uses/breakdown-by-service.png)

## <a name="view-cost-breakdown-by-azure-resource"></a>Azure kaynağına göre maliyet dökümünü görüntüleme

Hizmetleriniz, Azure kaynaklarıyla oluşturulur. Kaynaklara göre maliyetleri gözden geçirmek, temel maliyet noktalarını hızlıca tanımlamanıza yardımcı olabilir. Bir hizmette çok pahalı kaynaklar varsa tasarruf sağlamak için değişiklik yapabilirsiniz.

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. **Kaynağa göre maliyet**'i seçin.
1. Görünümü **Tablo** olarak değiştirin.

![Azure kaynağına göre maliyet dökümünü görüntüleme](./media/cost-analysis-common-uses/cost-by-resource.png)

## <a name="view-cost-breakdown-by-selected-dimensions"></a>Seçilen boyutlara göre maliyet dökümünü görüntüleme

Boyutlar, maliyetlerinizi ücretlerinizde gösterilen çeşitli meta verilere göre düzenlemenizi sağlar. Örneğin maliyetlerinizi konuma göre gruplandırabilirsiniz.

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. **Gruplandırma ölçütü** filtresini seçin.  
    ![Gruplandırma ölçütü seçin](./media/cost-analysis-common-uses/group-by.png)
1. İsterseniz görünümü daha sonra kullanmak üzere kaydedebilirsiniz.
1. Daha ayrıntılı verileri görüntülemek için grafiğin altındaki pasta grafiğine tıklayın.  
    ![Seçilen boyutlara göre maliyet dökümünü görüntüleme](./media/cost-analysis-common-uses/drill-down.png)

## <a name="view-costs-per-day-or-by-month"></a>Güne veya aya göre maliyetleri görüntüleme

Günlük ve aylık maliyetleri incelemek, belirli bir haftada veya yılda maliyetlerinizin daha yüksek olduğu dönemi daha iyi anlamanıza yardımcı olabilir. Tatil döneminde daha fazla müşteri trafiği oluyorsa Azure maliyetleriniz de bununla birlikte yükseliyor mu? Cuma, Pazartesi gününden daha mı maliyetli?

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. **Ayrıntı düzeyi**'ni **Aylık** veya **Günlük** olarak ayarlayın.

![Güne göre maliyetleri görüntüleme](./media/cost-analysis-common-uses/daily-granularity.png)

## <a name="view-costs-for-a-specific-tag"></a>Belirli bir etikete göre maliyetleri görüntüleme

Çoğu Azure kullanıcısı, ücretleri daha iyi sınıflandırabilmek için kaynaklarına maliyet merkezi veya geliştirme ortamı (üretim ve test) gibi etiketler ekliyor. Etiketler, maliyet analizinde boyut olarak görünür. Özel etiketleme kategorileriniz hakkında içgörü elde etmek için ilgili boyutu kullanabilirsiniz.

Etiket desteği, etiket kaynağa uygulandıktan *sonra* bildirilen kullanımlar için geçerlidir. Etiketler, maliyet toplamları için geriye dönük olarak uygulanmaz.

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. Etiketiniz için **Gruplandırma ölçütü** seçin.

![Belirli bir etikete göre maliyetleri görüntüleme](./media/cost-analysis-common-uses/tag.png)

## <a name="download-your-usage-details"></a>Kullanım verilerinizi indirme

CSV biçimindeki kullanım ayrıntıları dosyanız, bir faturaya dahil edilen tüm ücretlerle ilgili bir döküm sunar. Raporunuzu faturanızla karşılaştırabilir ve daha fazla bilgi sahibi olabilirsiniz. Faturanızdaki her bir ücret, kullanım raporunuzdaki ilgili ücret dökümlerine karşılık gelir.

1. Azure portalında bir ödeme hesabı veya abonelik için **Kullanım ve Ücretler** sekmesine gidin. Örneğin: **Maliyet Yönetimi + Faturalandırma** > **Faturalandırma** > **Kullanım + ücretler**.
1. İndirmek istediğiniz satır öğesini seçin ve ardından indirme simgesine tıklayın.  
    ![Kullanımı ve ücretleri indirme](./media/cost-analysis-common-uses/download1.png)
1.  İndirilecek kullanım dosyasını seçin.  
    ![İndirilecek kullanım dosyasını seçin](./media/cost-analysis-common-uses/download2.png)

## <a name="view-monthly-ea-cost-breakdown"></a>Aylık EA maliyet dökümünü görüntüleme

EA kaydınız, kuruluşun tamamına ait maliyetlerin tahakkuk ettiği yerdir. Maliyetlerin zaman içinde nasıl tahakkuk ettiğini ve faturalandığını anlamak, maliyetlerin doğru şekilde yönetilmesini sağlamak için uygun proje katılımcılarıyla etkileşim kurmanıza yardımcı olur.

Maliyetler yalnızca etkin kaydınız için gösterilir. Bir kaydı (etkin olmayan) yeni bir kayda (etkin) aktardıysanız, önceki kayda yönelik maliyetler Maliyet Yönetimi’nde gösterilmez.


1. Azure portalında **Maliyet Yönetimi + Faturalandırma** > **Genel Bakış**'a gidin.
1. Geçerli aya ait **Döküm**'e tıklayın ve parasal taahhüdünüzün nasıl tükendiğine bakın.  
    ![EA maliyetlerine genel bakış - döküm özeti](./media/cost-analysis-common-uses/breakdown1.png)
1.  **Kullanım ve Ücretler** sekmesine tıklayıp seçili zaman aralığı için bir önceki aynı dökümünü görüntüleyin.  
    ![Kullanım ve ücretler sekmesi](./media/cost-analysis-common-uses/breakdown2.png)

## <a name="view-enrollment-monthly-cost-by-term"></a>Döneme göre aylık kayıt maliyetini görüntüleme

Belirli bir döneme ait maliyet eğilimlerini ve fatura edilen tutarları anlamak için kaydınızın aylık maliyetlerini grafik üzerinde görüntüleyin.

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. Kaydınızı seçin ve kayıt dönemini belirtin.
1. Ayrıntı düzeyini ay olarak ayarlayın ve **Sütun (yığılmış)** görünümünü seçin.

Daha ayrıntılı bir analiz gerçekleştirmek için verilerinizde gruplandırma ve filtreleme gerçekleştirebilirsiniz.

![Döneme göre aylık kayıt maliyeti](./media/cost-analysis-common-uses/enrollment-term1.png)

## <a name="view-ea-enrollment-accumulated-costs"></a>EA kaydı birikmiş maliyetlerini görüntüleme

Kuruluşunuzun belirli bir döneme ait genel harcamalarını anlamak için zaman içindeki net birikmiş ücretleri görüntüleyebilirsiniz.

1. Azure portalında kapsamınıza ait maliyet analizine gidin. Örneğin: **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**.
1. Kaydınızı seçin ve geçerli birikmiş maliyetlerinizi görüntüleyin.

![Kayıt birikmiş maliyetleri](./media/cost-analysis-common-uses/cost-analysis-enrollment.png)

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet Yönetimi için ilk hızlı başlangıcı önceden tamamlamadıysanız, [Maliyetleri analiz etmeye başlama](quick-acm-cost-analysis.md) bölümünden bilgi edinin.
- Bkz. [Azure Maliyet Yönetimi belgeleri](../index.yml).
