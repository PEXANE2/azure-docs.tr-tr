---
title: Azure purview 'daki verilerinize ilişkin varlık öngörüleri (Önizleme)
description: Bu nasıl yapılır kılavuzunda, verileriniz üzerinde purview Insights varlık raporlama 'nın nasıl görüntüleneceği ve kullanılacağı açıklanmaktadır.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: b9a207ffa14a18a5f4421fd21cebed28290b5ea6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102183089"
---
# <a name="asset-insights-on-your-data-in-azure-purview"></a>Azure purview 'daki verilerinize ilişkin varlık öngörüleri

Bu nasıl yapılır kılavuzunda verilerinize yönelik varlık Insight raporlarının nasıl erişebileceği, görüntüleneceği ve filtreleneceği açıklanmaktadır.

Bu nasıl yapılır kılavuzunda şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Purview hesabınızdan öngörüleri görüntüleyin.
> * Verilerinizin bir kuşbakışı görünümünü alın.
> * Daha fazla varlık sayısı ayrıntısı için detaya gidin.

## <a name="prerequisites"></a>Önkoşullar

Purview Insights 'ı kullanmaya başlamadan önce aşağıdaki adımları tamamladığınızdan emin olun:

* Azure kaynaklarınızı ayarlayın ve hesabı verilerle doldurun.

* Kaynak türü üzerinde bir tarama kurun ve doldurun.

Daha fazla bilgi için bkz. [Azure purview 'ta veri kaynaklarını yönetme (Önizleme)](manage-data-sources.md).

## <a name="use-purview-asset-insights"></a>Purview varlık öngörülerini kullanma

Azure purview ' de kaynak türlerini kaydedebilir ve tarayabilirsiniz. Tarama tamamlandıktan sonra varlık öngörüleri ' nde varlık dağıtımını görüntüleyerek, sınıflandırma ve kaynak kümelerine göre verilerinizin durumunu söylemiş olursunuz. Veri boyutunda herhangi bir değişiklik olup olmadığını da bildirir.

> [!NOTE]
> Kaynak türlerinizi taradıktan sonra, yeni varlıkları yansıtmak için varlık öngörüleri 3-8 saatlere izin verin. Gecikme, dağıtım bölgesindeki veya iş yükünüzün boyutundaki yüksek trafikle kaynaklanıyor olabilir. Daha fazla bilgi için lütfen alan desteği ekibine başvurun.

1. Azure portal Azure purview kaynağına gidin.

1. **Genel bakış** sayfasında, **Başlarken** bölümünde, **purview Studio 'yu aç** kutucuğunu seçin.

   :::image type="content" source="./media/asset-insights/portal-access.png" alt-text="Azure portal Takiview 'ı başlatın":::

1. Takip görünümü **giriş** sayfasında, Öngörüler alanına erişmek için **öngörüleri görüntüle** kutucuğunu seçin  :::image type="icon" source="media/asset-insights/ico-insights.png" border="false"::: .

   :::image type="content" source="./media/asset-insights/view-insights.png" alt-text="Azure portal öngörülerinizi görüntüleyin":::

1. **Öngörüler** :::image type="icon" source="media/asset-insights/ico-insights.png" border="false"::: alanında, Içgörüler **varlık öngörüleri** raporunu görüntülemek için **varlıklar** ' ı seçin.

### <a name="view-asset-insights"></a>Varlık öngörülerini görüntüle

1. Ana **varlık öngörüleri** sayfasında aşağıdaki bölgeler görüntülenir:

2. Kaynak türlerini, sınıflandırılmış varlıkları ve keşfedilen varlıkları göstermek için üst düzey KPI 'ler
 
3. İlk grafik **kaynak türü başına varlıkları** gösterir.

4. Kaynak türüne göre varlık dağıtımınızı görüntüleyin. Kaynak türüne göre varlık dağıtımını görmek için bir sınıflandırma veya bir sınıflandırma kategorisinin tamamını seçin. 
 
5. Daha fazla bilgi için, kaynak türlerinin ve varlık sayılarının tablolu bir biçimini görüntüleyen **daha fazla görüntüle**' yi seçin. Sınıflandırma filtreleri bu sayfaya taşınır.

   :::image type="content" source="./media/asset-insights/highlight-kpis.png" alt-text="Varlık öngörülerine ait KPI 'Ları ve grafiği görüntüleme":::
 
6. Varlık sayımlarını olan en üst klasörleri görmek istediğiniz belirli bir kaynağı seçin. 

   :::image type="content" source="./media/asset-insights/select-data-source.png" alt-text="Kaynak türünü seçin":::
 
7. Yukarıda seçtiğiniz kaynak türü içinde en üst klasöre göre toplam varlıkları seçin.

   :::image type="content" source="./media/asset-insights/file-path.png" alt-text="Dosya yollarını görüntüle":::

8. Klasör içindeki dosyaların listesini görüntüleyin. İçgörü Crumbs kullanarak Öngörüler 'e geri gidin.

   :::image type="content" source="./media/asset-insights/list-page.png" alt-text="Varlık listesini görüntüle":::  

### <a name="file-based-source-types"></a>Dosya tabanlı kaynak türleri
Varlık öngörülerine ait sonraki birkaç grafik dosya tabanlı kaynak türlerinin bir dağıtımını gösterir. **Kaynak türlerindeki dosya türünün boyut eğilimi (GB)** olarak adlandırılan ilk grafik, son 30 gün içinde en üstteki dosya türü boyutunu gösterir. 
 
1. Kaynak içinde dosya türünü görüntülemek için kaynak türünü seçin. 
 
1. Geçerli veri boyutunu, boyutun değiştirilmesini, geçerli varlık sayısını ve varlık sayısı değişikliğini görmek için **daha fazla görüntüle** ' yi seçin.
 
   > [!NOTE]
   > Tarama son 30 gün içinde yalnızca bir kez çalıştırılmışsa veya sınıflandırma ekleme/kaldırma gibi Katalog değişikliği yalnızca bir kez 30 gün içinde gerçekleştiyse, yukarıdaki değişiklik bilgileri boş görünür.

1. Kaynak türü ' ne tıkladığınızda en üstteki varlık sayısını değiştir olan en üstteki klasörlere bakın.

1. Varlık listesini görmek için yolu seçin.

Dosya tabanlı kaynak türlerindeki ikinci grafik, ***kaynak kümesi ile Ilişkilendirilmemiş dosyalardır***. Tüm dosyaların bir kaynak kümesine toplaması beklendiğinde, bu grafik hangi varlıkların toplanmayacağını anlamanıza yardımcı olabilir. Eksik varlıklar, klasördeki yanlış dosya deseninin bir göstergesi olabilir. Dosyalar hakkında daha fazla bilgi görüntülemek için diğer grafiklerle aynı adımları izleyin.

   :::image type="content" source="./media/asset-insights/file-based-assets.png" alt-text="Dosya tabanlı varlıkları görüntüle":::  

## <a name="next-steps"></a>Sonraki adımlar

[Tarama öngörüleri](./scan-insights.md) Ile Azure purview Insight Reports hakkında daha fazla bilgi edinin
