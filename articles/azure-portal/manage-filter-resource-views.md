---
title: Azure Kaynak bilgilerini görüntüleme ve filtreleme
description: Azure kaynaklarınızı daha iyi anlamak için bilgileri filtreleyin ve farklı görünümler kullanın.
author: mgblythe
ms.service: azure-portal
ms.topic: how-to
ms.author: mblythe
ms.date: 09/11/2020
ms.openlocfilehash: f6185e1c4042110f446337c1b2c2904fefdae7a9
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92280245"
---
# <a name="view-and-filter-azure-resource-information"></a>Azure Kaynak bilgilerini görüntüleme ve filtreleme

Azure portal, Azure aboneliklerinizdeki kaynaklarla ilgili ayrıntılı bilgilere gözatmanızı sağlar. Bu makalede, kaynaklarınızı daha iyi anlamak için bilgilerin nasıl filtreleneceği ve farklı görünümlerin nasıl kullanılacağı gösterilmektedir.

Makale, aşağıdaki ekran görüntüsünde gösterilen **tüm kaynaklar** ekranına odaklanır. Sanal makineler gibi ayrı kaynak türlerinin ekranları, bir VM başlatma ve durdurma gibi farklı seçeneklere sahiptir.

:::image type="content" source="media/manage-filter-resource-views/all-resources.png" alt-text="Tüm kaynakların Azure portal görünümü":::

## <a name="filter-resources"></a>Kaynakları filtrele

Kaynaklarınızın bir alt kümesine odaklanmak için filtreleri kullanarak **tüm kaynakları** keşfetmeye başlayın. Aşağıdaki ekran görüntüsünde, bir abonelikte altı kaynak grubundan birini seçerek kaynak gruplarında filtreleme gösterilmektedir.

:::image type="content" source="media/manage-filter-resource-views/filter-resource-group.png" alt-text="Tüm kaynakların Azure portal görünümü" içeren kaynakların kapsamına alınır.

:::image type="content" source="media/manage-filter-resource-views/filter-simplewinvm.png" alt-text="Tüm kaynakların Azure portal görünümü":::

Bir görünüme eklenen sütunları değiştirmek için **Yönet görünüm** ' ü ve **Sütunları Düzenle**' yi seçin.

:::image type="content" source="media/manage-filter-resource-views/edit-columns.png" alt-text="Tüm kaynakların Azure portal görünümü":::

## <a name="save-use-and-delete-views"></a>Görünümleri kaydetme, kullanma ve silme

Seçtiğiniz filtreleri ve sütunları içeren görünümleri kaydedebilirsiniz. Bir görünümü kaydetmek ve kullanmak için:

1. **Yönet görünüm** ' ü ve ardından **Görünümü Kaydet**' i seçin

1. Görünüm için bir ad girin ve **Tamam**' ı seçin. Kaydedilen görünüm artık **görünümü Yönet** menüsünde görüntülenir.

    :::image type="content" source="media/manage-filter-resource-views/simple-view.png" alt-text="Tüm kaynakların Azure portal görünümü" bölmesindeki kaydedilen görünümlerde** görünümü seçin ve **Sil** simgesini ![ görünümü Sil simgesini seçin ](media/manage-filter-resource-views/icon-delete.png) .

## <a name="summarize-resources-with-visuals"></a>Görsellerle kaynakları özetleme

Şimdiye kadar baktığı Görünümler _liste görünümlerdik_, ancak görselleri içeren _Özet görünümler_ de vardır. Bu görünümleri, görünümleri listeleyebilir gibi kaydedebilir ve kullanabilirsiniz. Filtreler iki tür görünüm arasında kalır. Aşağıda gösterilen **konum** görünümü gibi standart görünümlerin yanı sıra, Azure Storage **durum** görünümü gibi belirli hizmetlerle ilgili görünümler de vardır.

:::image type="content" source="media/manage-filter-resource-views/summary-map.png" alt-text="Tüm kaynakların Azure portal görünümü":::

Bir Özet görünümünü kaydetmek ve kullanmak için:

1. Görünüm menüsünden **Özet Görünüm**' ü seçin.

    :::image type="content" source="media/manage-filter-resource-views/menu-summary-view.png" alt-text="Tüm kaynakların Azure portal görünümü":::

1. Özet görünümü, **konum** ve **tür**dahil farklı özniteliklere göre özetlemenize olanak sağlar. Bir **özetleme** seçeneği ve uygun bir görsel seçin. Aşağıdaki ekran görüntüsünde, **çubuk grafik** görselinle **tür Özeti** gösterilmektedir.

    :::image type="content" source="media/manage-filter-resource-views/type-summary-bar-chart.png" alt-text="Tüm kaynakların Azure portal görünümü":::

1. Bu görünümü liste görünümüyle yaptığınız gibi kaydetmek için **yönet görünümü** ' nü ve **Kaydet** ' i seçin.

1. Özet görünümünde, **tür Özeti**altında grafikte bir çubuk seçin. Çubuğun seçilmesi, bir kaynak türüne göre filtrelenmiş bir liste sağlar.

    :::image type="content" source="media/manage-filter-resource-views/all-resources-filtered-type.png" alt-text="Tüm kaynakların Azure portal görünümü":::

## <a name="run-queries-in-azure-resource-graph"></a>Azure Kaynak grafiğinde sorguları çalıştırma

Azure Kaynak Grafiği, bir abonelik kümesi genelinde ölçekte sorgu oluşturma özelliği ile verimli ve performanslı kaynak araştırması sağlar. Azure portal **tüm kaynaklar** ekranı, geçerli filtrelenmiş görünüm kapsamındaki kaynak grafik sorgusunu açmak için bir bağlantı içerir.

Kaynak Grafiği sorgusu çalıştırmak için:

1. **Sorgu aç**' ı seçin.

    :::image type="content" source="media/manage-filter-resource-views/open-query.png" alt-text="Tüm kaynakların Azure portal görünümü":::

1. **Azure Kaynak Grafiği Gezgini**' nde sonuçları görmek Için **Sorguyu Çalıştır** ' ı seçin.

    :::image type="content" source="media/manage-filter-resource-views/run-query.png" alt-text="Tüm kaynakların Azure portal görünümü":::

    Daha fazla bilgi için bkz. [Azure Kaynak Grafiği gezginini kullanarak Ilk kaynak grafik sorgunuzu çalıştırma](../governance/resource-graph/first-query-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure portal’a genel bakış](azure-portal-overview.md)

[Azure portalında pano oluşturma ve paylaşma](azure-portal-dashboards.md)
