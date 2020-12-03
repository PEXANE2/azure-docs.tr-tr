---
title: "Öğretici: Azure purview 'da kaynak kümelerini, ayrıntıları, şemaları ve sınıflandırmaları araştırma (Önizleme)"
description: Bu öğreticide kaynak kümelerinin, varlık ayrıntılarının, şemaların ve sınıflandırmaların nasıl kullanılacağı açıklanmaktadır.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: tutorial
ms.date: 12/01/2020
ms.openlocfilehash: c39ed746630c646b0ce305a9535ae6d610f5161c
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96555799"
---
# <a name="tutorial-explore-resource-sets-details-schemas-and-classifications-in-azure-purview-preview"></a>Öğretici: Azure purview 'da kaynak kümelerini, ayrıntıları, şemaları ve sınıflandırmaları araştırma (Önizleme)

> [!IMPORTANT]
> Azure takip görünümü şu anda ÖNIZLEME aşamasındadır. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) , Beta, önizleme veya başka bir şekilde genel kullanıma sunulmayan Azure özelliklerine uygulanan ek yasal koşulları içerir.

Bu öğreticide, Katalog: kaynak kümeleri, varlık ayrıntıları, şemalar ve sınıflandırmalar için anahtar bileşenleri keşfedebilirsiniz.

Bu öğretici, Azure purview kullanarak bir veri yönetiminde veri yönetimini yönetme temellerini öğrentiğinizde *beş bölümlü bir öğretici serisinin 4. parçasıdır* . Bu öğretici, [3. Bölüm: Azure 'daki varlıklara (Önizleme) gözatıp kökenini görüntüleyin](tutorial-browse-and-view-lineage.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Kaynak kümelerini görüntüleyin.
> * Varlık ayrıntılarını görüntüleyin.
> * Şemayı düzenleyin ve sınıflandırmalar ekleyin.

## <a name="prerequisites"></a>Ön koşullar

* Tüm [öğretici: Azure purview (Önizleme) içindeki varlıklara gözatıp kökenini görüntüleyin](tutorial-browse-and-view-lineage.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="view-resource-sets"></a>Kaynak kümelerini görüntüle

Kaynak kümesi, katalogdaki çok sayıda fiziksel nesneyi temsil eden katalogdaki tek bir nesnedir. Nesneler genellikle ortak bir şemayı ve çoğu durumda bir adlandırma kuralını veya klasör yapısını paylaşır. Örneğin, tarih biçimi *yyyy/aa/gg* şeklindedir. Kaynak kümeleri hakkında daha fazla bilgi için bkz. [kaynak kümelerini anlama](concept-resource-sets.md).

1. Azure portal Azure purview kaynağına gidin ve **purview Studio 'Yu aç**' ı seçin. Otomatik olarak purview Studio 'nun giriş sayfasına yönlendirilirsiniz.

2. **Varlıkları ara** kutusuna **contoso_staging_positivecashflow_ n** girin ve sonra arama sonuçlarından **Contoso_Staging_PositiveCashFlow_ {n}. csv** ' yi seçin.

## <a name="view-asset-details"></a>Varlık ayrıntılarını görüntüle

1. **Genel bakış** sekmesi, **QualifiedName** gibi **açıklamayı**, **Sözlük koşullarını** ve **özellikleri** görüntüler.

   :::image type="content" source="./media/tutorial-schemas-and-classifications/overview-tab.png" alt-text="Kaynak kümesi varlık sayfasının genel bakış sekmesini gösteren ekran görüntüsü.":::

1. **Özellikler** altında aşağıdaki iki alanı aklınızda yapın:

   * **partitionCount**: Bu kaynak kümesiyle ilişkili fiziksel dosya sayısını gösterir.
   * **Schemacount**: Bu kaynak kümesi içinde bulunan şemanın değişim sayısını belirtir.

   [Bu öğretici serisinin 1. kısmında](tutorial-scan-data.md)taramayı tamamladıktan sonra Azure purview bu özellikleri 24 saat içinde doldurur.

1. **Uzmanlar** ve **sahipler** değerlerini gözden geçirmek için **kişiler** sekmesini seçin.

## <a name="edit-the-schema-and-add-classifications"></a>Şemayı düzenleyin ve sınıflandırmalar ekleyin

1. **Şema** sekmesini seçin. Sütun adlarını ve bunlarla ilişkili sınıflandırmaları gözlemleyin. Taramanın özellikleri otomatik olarak doldurduğuna dikkat edin.

   :::image type="content" source="./media/tutorial-schemas-and-classifications/schema-tab.png" alt-text="Şema sekmesini gösteren ekran görüntüsü.":::

1. Varlığı düzenlemek için sol üstteki **Düzenle** düğmesini seçin. Ardından **şema** sekmesini seçin.

1. Bir sütun için **Açıklama** ekleyin veya sütunu daha kolay bir adla yeniden adlandırın.

1. Küme sınıflandırması olmayan bir sütun adı için **sütun düzeyi sınıflandırma** açılan listesini seçerek varlık düzeyinde bir sınıflandırma ekleyin.

   :::image type="content" source="./media/tutorial-schemas-and-classifications/edit-schema.png" alt-text="Şema düzenleme sayfası":::

1. Yaptığınız değişikliklerle işiniz bittiğinde **Kaydet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
>
> * Kaynak kümelerini görüntüleyin.
> * Varlık ayrıntılarını görüntüleyin.
> * Şemayı düzenleyin ve sınıflandırmalar ekleyin.

Sözlük hakkında bilgi edinmek ve varlıklar için yeni koşulları tanımlama ve içeri aktarma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sözlük terimleri oluşturma ve içeri aktarma](tutorial-import-create-glossary-terms.md)