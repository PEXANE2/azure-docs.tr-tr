---
title: 'Nasıl yapılır: veri kataloğuna tarama'
description: Bu makalede, varlık türüne göre Azure purview veri kataloğuna gözatmaya yönelik bir genel bakış sunulmaktadır.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/30/2020
ms.openlocfilehash: dd2fd4fbc9d8aecd9821d668dd02bd4b68659cf6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97695050"
---
# <a name="browse-the-azure-purview-data-catalog"></a>Azure purview veri kataloğuna gözatın

Bu makalede veri kaynağı hiyerarşik ad alanı kullanılarak Azure purview veri kataloğunda verilerin nasıl keşfedileceğini açıklanmaktadır.

## <a name="browse-experience"></a>Gözatmaya yönelik deneyim

Bir veri tüketicisi, bir Gezgin Görünümü kullanarak her bir veri kaynağı için tanıdık sıradüzenli ad alanını kullanarak verileri bulabilir. Veri kaynağı kaydedildikten ve tarandıktan sonra veri eşlemesi, veri kaynağının yapısı (hiyerarşik ad alanı) hakkında bilgi ayıklar. Bu bilgiler, veri bulma için gözatma deneyimini oluşturmak için kullanılır.

Örneğin, Azure Data Lake Storage Gen 2 ' de *Boyutlar* adlı bir klasör altında *datedimension* adlı bir veri kümesini kolayca bulabilirsiniz. ADLS Gen 2 depolama hesabına gitmek için ' varlık türünü gözden geçirme ' deneyimini kullanabilirsiniz, sonra belirli *Boyutlar* klasörüne ulaşmak üzere Service > kapsayıcısına > klasör (ler) e gidebilir ve sonra *datedimension* tablosuna bakabilirsiniz.

Her ilgili veri kaynağının her biri için hiyerarşik ad alanı ile yerel bir gözatma deneyimi sağlanır.

## <a name="browse-the-data-catalog-by-asset-type"></a>Varlık türüne göre veri kataloğuna gözatın

1. Giriş sayfasındaki **varlık türüne göre gözden** geçirme ' i seçerek veri varlıklarına gözatabilmeniz gerekir.

    :::image type="content" source="media/how-to-browse-catalog/studio-home-page.png" alt-text="Takip görünümü giriş sayfası" border="true":::

1. **Varlık türlerini inceleyin** sayfasında, kutucuklar veri kaynaklarına göre kategorilere ayrılır. Her veri kaynağındaki varlıkları daha fazla araştırmak için ilgili kutucuğu seçin.

    :::image type="content" source="media/how-to-browse-catalog/browse-asset-types.jpg" alt-text="Varlık türlerine gözatamıyorum sayfası" border="true":::

1. Bir sonraki sayfada, seçtiğiniz veri türü altındaki en üst düzey varlıklar listelenir. İçeriğini daha ayrıntılı incelemek için varlıklardan birini seçin.

    :::image type="content" source="media/how-to-browse-catalog/asset-type-specific-browse.jpg" alt-text="Varlık türüne özgü tarama sayfası. Azure depolama hesabı gösterilen örnek" border="true":::

1. Gezgin Görünümü açılır. Sol panelde varlığı seçerek gözatmayı başlatın. Alt varlıklar sayfanın sağ bölmesinde listelenecektir.

    :::image type="content" source="media/how-to-browse-catalog/explorer-view.jpg" alt-text="Gezgin Görünümü" border="true":::

1. Bir varlığın ayrıntılarını görüntülemek için, en sağdaki adı veya üç nokta düğmesini seçin.

    :::image type="content" source="media/how-to-browse-catalog/view-asset-detail-click-ellipses.jpg" alt-text="Varlık ayrıntıları sayfasını görmek için üç nokta düğmesini seçin" border="true":::

## <a name="view-related-data-assets"></a>İlgili veri varlıklarını görüntüleme

Varlık ayrıntısı sayfasında, diğer ilgili veri varlıklarını da görüntüleyebilirsiniz. Örneğin, *Boyutlar* klasörünü görmek Için *datedimension* ' ın üst klasörüne gidebilir veya hatta belirli bir hiyerarşi altındaki varlıkları görmek için kapsayıcıya gidebilirsiniz.

1. Varlık ayrıntısı sayfasında, diğer ilgili varlıkları görmek için **ilgili** sekmeyi seçin.

    :::image type="content" source="media/how-to-browse-catalog/launch-related-tab.jpg" alt-text="Ilgili sekmeyi Başlat" border="true":::

1. Geçerli varlığın tam hiyerarşisi solda listelenecektir.

    :::image type="content" source="media/how-to-browse-catalog/hierarchical-structure.jpg" alt-text="Hiyerarşik yapı" border="true":::

1. Hiyerarşideki herhangi bir düzeyi seçerek en hızlı varlıkları bu düzeyin altına listeleyin.

    :::image type="content" source="media/how-to-browse-catalog/select-different-hierarchy.jpg" alt-text="Farklı hiyerarşi Seç" border="true":::

## <a name="next-steps"></a>Sonraki adımlar

- [Sözlük terimleri oluşturma, içeri aktarma ve dışarı aktarma](how-to-create-import-export-glossary.md)
- [İş sözlüğü için terim şablonlarını yönetme](how-to-manage-term-templates.md)
