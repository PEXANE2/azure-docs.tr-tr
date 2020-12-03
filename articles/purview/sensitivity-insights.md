---
title: Azure Blob depolama alanındaki verilerinizde duyarlılık etiketi raporlaması
description: Bu nasıl yapılır kılavuzunda, Azure Blob depolamada verileriniz üzerinde Guview duyarlılık etiketi raporlama 'nın nasıl görüntüleneceği ve kullanılacağı açıklanmaktadır.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/24/2020
ms.openlocfilehash: beaa231eddd57e29578a1d69ee30af05ce7c8d0f
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96554380"
---
# <a name="sensitivity-label-insights-about-your-data-in-azure-purview"></a>Azure purview 'daki verileriniz hakkında duyarlılık etiketi öngörüleri

Bu nasıl yapılır kılavuzunda verilerinize uygulanan duyarlılık etiketleri tarafından sunulan güvenlik öngörülerinin nasıl erişebileceği, görüntüleneceği ve filtreleneceği açıklanmaktadır.

Desteklenen veri kaynakları şunlardır: Azure Blob depolama, Azure Data Lake Storage (ADLS) GEN 1, Azure Data Lake Storage (ADLS) GEN 2, SQL Server, Azure SQL veritabanı, Azure SQL yönetilen örneği

Bu nasıl yapılır kılavuzunda şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> - Azure 'dan purview hesabınızı başlatın.
> - Verilerinize ilişkin duyarlılık etiketleme öngörülerini görüntüleme
> - Verileriniz hakkında daha fazla duyarlılık etiketleme ayrıntısı için detaya gidin

## <a name="prerequisites"></a>Ön koşullar

Purview Insights 'ı kullanmaya başlamadan önce aşağıdaki adımları tamamladığınızdan emin olun:

- Azure kaynaklarınızı ayarlayın ve test verileriyle ilgili hesapları doldurulmuştur

- [Azure purview 'daki varlıklara genişletilmiş Microsoft 365 duyarlılık etiketleri](create-sensitivity-label.md)ve verilerinize uygulamak istediğiniz etiketleri oluşturma veya seçme.

- Her veri kaynağındaki test verilerini ayarlama ve bir taramayı tamamlanan

Daha fazla bilgi için bkz. [Azure purview 'ta veri kaynaklarını yönetme (Önizleme)](manage-data-sources.md) ve [verilerinizi otomatik olarak etiketleme](create-sensitivity-label.md).

## <a name="use-purview-sensitivity-labeling-insights"></a>Purview duyarlılığı etiketleme öngörülerini kullanma

Purview 'da, sınıflandırmalar konu etiketlerine benzerdir ve tarama sırasında verileriniz içinde bulunan belirli bir türün verilerini işaretlemek ve tanımlamak için kullanılır.

Duyarlılık etiketleri, belirli verilerin kuruluşunuzda ne kadar hassas olduğunu size sağlar. Örneğin, belirli bir proje adı kuruluşunuzda çok gizli olabilir, ancak aynı terim diğer kuruluşlara gizli değildir. 

Sınıflandırmalar doğrudan eşleştirirken (sosyal güvenlik numarası bir **sosyal güvenlik numarası** sınıflandırmasına sahiptir), bir veya daha fazla sınıflandırma ve senaryo birlikte bulunduğunda duyarlık etiketleri uygulanır. 

Takip, Microsoft 365 olarak gizli bilgi türleri olarak da bilinen aynı sınıflandırmaları kullanır. Bu, mevcut duyarlılık etiketlerinizi Azure purview varlıklarınız genelinde genişletmenizi sağlar.

**Duyarlılık etiketleme öngörülerini görüntülemek için:**

1. **Azure purview** giriş sayfasına gidin.

1. **Genel bakış** sayfasında, **Başlarken** bölümünde, **purview hesabını Başlat** kutucuğunu seçin.

1. Takip görünümü ' nde, **Insights** :::image type="icon" source="media/insights/ico-insights.png" border="false"::: **Öngörüler** alanına erişmek için soldaki Öngörüler menü öğesini seçin.

1. **Öngörüler** :::image type="icon" source="media/insights/ico-insights.png" border="false"::: alanında **duyarlık etiketleri** ' ni seçerek purview **duyarlılık etiketleme öngörüleri** raporunu görüntüleyin.

    > [!NOTE]
    > Bu rapor boşsa, duyarlılık etiketlerinizi Azure purview 'a genişletmeyebilirsiniz. Daha fazla bilgi için bkz. [Azure purview 'da verilerinizi otomatik olarak etiketleme](create-sensitivity-label.md).

   :::image type="content" source="media/insights/sensitivity-labeling-insights-small.png" alt-text="Duyarlılık etiketleme öngörüleri" lightbox="media/insights/sensitivity-labeling-insights.png":::

   Ana **duyarlılık etiketleme öngörüleri** sayfasında aşağıdaki bölgeler görüntülenir:

   |Alan  |Açıklama  |
   |---------|---------|
   |**Duyarlılık etiketleriyle kaynaklara genel bakış**     |Şunları sağlayan kutucukları görüntüler: <br>-Verilerinizde bulunan aboneliklerin sayısı. <br>-Verilerinize uygulanan benzersiz duyarlılık etiketi sayısı <br>-Duyarlılık etiketleri uygulanmış kaynak sayısı <br>-Duyarlılık etiketleri uygulanmış olarak bulunan dosya ve tablo sayısı|
   |**Etiketli verileri olan en popüler kaynaklar (son 30 gün)**     | Duyarlılık etiketi uygulanmış kaynak sayısının son 30 günü boyunca eğilimi gösterir.       |
   |**Kaynaklar arasında uygulanan üst Etiketler**     |Tüm purview veri kaynaklarınızın tamamında uygulanan en üst etiketleri gösterir. |
   |**Dosyalara uygulanan üst Etiketler**     |Verilerinizde bulunan dosyalara uygulanan en büyük duyarlılık etiketlerini gösterir.          |
   |**Tablolara uygulanan üst Etiketler**     | Verilerinize ait veritabanı tablolarına uygulanan en büyük duyarlılık etiketlerini gösterir. |   
   |  **Etiketleme etkinliği**  |  Her biri seçili zaman çerçevesinde etiketlendirilmiş dosya veya tablo sayısını gösteren dosyalar ve tablolar için ayrı grafikler görüntüler. <br>**Varsayılan**: 30 gün<br>Görüntülenecek farklı bir zaman çerçevesini seçmek için grafiklerin üzerindeki **zaman** filtresini seçin.    |
   |    |    |
## <a name="sensitivity-labeling-insights-drilldown"></a>Duyarlılık etiketleme öngörüleri ayrıntıya gitme

Aşağıdaki **duyarlık grafiklerini etiketleyerek** , daha fazla ayrıntı için detaya gitmek üzere **daha fazla bilgi görüntüle** bağlantısını seçin:

- **Kaynaklar arasında uygulanan üst Etiketler**
- **Dosyalara uygulanan üst Etiketler**
- **Tablolara uygulanan üst Etiketler**
- **Etiketlendirilmiş verileri > etiketleme**

Örnek:

:::image type="content" source="media/insights/sensitivity-label-drilldown-small.png" alt-text="Duyarlılık etiketi detaya gitme" lightbox="media/insights/sensitivity-label-drilldown.png":::

Daha fazla bilgi edinmek için aşağıdakilerden birini yapın:

|Seçenek  |Açıklama  |
|---------|---------|
|**Verilerinizi filtreleme**     |  Etiket adı, abonelik adı veya kaynak türü dahil olmak üzere gösterilen verileri filtrelemek için kılavuzun üzerindeki filtreleri kullanın. <br><br>Tam etiket adından emin değilseniz, adın bir kısmını veya tamamını **filtre ölçütü anahtar** kutusuna girebilirsiniz.       |
|**Kılavuzu sıralama** |Kılavuzu bu sütuna göre sıralamak için bir sütun üst bilgisi seçin. | 
|**Sütunları düzenle**     |  Kılavuzunuzda daha fazla veya daha az sütun görüntülemek için **Sütunları Düzenle** :::image type="icon" source="media/insights/ico-columns.png" border="false"::: ' yi seçin ve ardından siparişi görüntülemek veya değiştirmek istediğiniz sütunları seçin.    <br><br>Kılavuzu bu sütuna göre sıralamak için bir sütun üst bilgisi seçin.   |
|**Daha fazla ayrıntıya gitme**     | Belirli bir etikete gitmek için **duyarlık etiketi** sütunundaki bir adı seçerek **etiketi kaynak raporuna göre** görüntüleyin. <br><br>Bu rapor, kaynak adı, kaynak türü, abonelik KIMLIĞI ve sınıflandırılan dosya ve tablo sayıları dahil olmak üzere seçili etiket için verileri görüntüler.      |
|**Varlıklara gözatamıyorum**     |  Belirli bir etiket veya kaynakla bulunan varlıklara gözatmak için, görüntülemekte olduğunuz rapora bağlı olarak bir veya daha fazla etiket veya kaynak seçin ve ardından filtrelerin üzerindeki **varlıklara göz at** ' ı seçin :::image type="icon" source="media/insights/ico-browse-assets.png" border="false"::: . <br><br>Arama sonuçları, seçili filtre için bulunan etiketlenmiş varlıkların tümünü görüntüler.  Daha fazla bilgi için bkz. [Azure purview veri kataloğunu arama](how-to-search-catalog.md).       |
| | |
## <a name="sensitivity-label-integration-with-microsoft-365-compliance"></a>Microsoft 365 uyumlulukla duyarlılık etiketi tümleştirmesi

Microsoft 365 sunulan [Microsoft Information Protection](/microsoft-365/compliance/information-protection) ile yakın tümleştirme, purview 'ın, verileri parçalara göre görünürlüğünüzü genişletmenin ve etiketlemesini ve etiketlemenize yönelik doğrudan yolları mümkün hale getirecek anlamına gelir.

Microsoft 365 duyarlılık etiketlerinizi Azure purview 'daki varlıklarınıza genişletmek için, Microsoft 365 Uyumluluk Merkezi 'nde Azure purview için Information Protection etkin bir şekilde açmanız gerekir.

Daha fazla bilgi için bkz. [Azure purview 'da verilerinizi otomatik olarak etiketleme](create-sensitivity-label.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure purview Insight Reports hakkında daha fazla bilgi edinin
> [!div class="nextstepaction"]
> [Sözlük öngörüleri](glossary-insights.md)

> [!div class="nextstepaction"]
> [Öngörüleri Tara](scan-insights.md)

> [!div class="nextstepaction"]
> [Sınıflandırma öngörüleri](./classification-insights.md)

> [!div class="nextstepaction"]
> [Dosya Uzantısı öngörüleri](file-extension-insights.md)
