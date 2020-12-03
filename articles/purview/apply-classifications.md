---
title: Varlıklara sınıflandırmalar uygulama (Önizleme)
description: Bu belge, varlıklarda sınıflandırmaların nasıl uygulanacağını açıklar.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/19/2020
ms.openlocfilehash: d12a7d52562fe32126e12a844c2d36c14cf01431
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96555474"
---
# <a name="apply-classifications-on-assets-in-azure-purview"></a>Azure purview 'daki varlıklar üzerinde sınıflandırmalar uygulama

Bu makalede, varlıklarda sınıflandırmaların nasıl uygulanacağı açıklanır.

## <a name="introduction"></a>Giriş

Sınıflandırmalar sistem veya özel türler olabilir. Sistem sınıflandırmaları varsayılan olarak purview içinde bulunur. Özel sınıflandırmalar, normal ifade düzenine göre oluşturulabilir. Sınıflandırmalar, otomatik olarak veya el ile varlıklara uygulanabilir.

Bu belgede, verilerinize sınıflandırmaların nasıl uygulanacağı açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- İhtiyaya göre özel sınıflandırmalar oluşturun.
- Veri kaynaklarınızın taramasını ayarlayın.

## <a name="apply-classifications"></a>Sınıflandırmaları Uygula
Azure purview ' de bir dosya, tablo veya sütun varlığı üzerinde sistem veya özel sınıflandırmalar uygulayabilirsiniz. Bu makalede, varlıklarınıza sınıflandırmaların el ile uygulanması için gereken adımlar açıklanır.

### <a name="apply-classification-to-a-file-asset"></a>Bir dosya varlığına sınıflandırma uygulama
Azure purview, belge dosyalarını tarayabilir ve otomatik olarak sınıflandırabilir. Örneğin, *multiple.docx* adlı bir dosyanız varsa ve IÇERIĞINDE ulusal kimlik numarası varsa, Azure purview, sınıflandırma **AB Ulusal kimlik numarasını** dosya varlığının ayrıntı sayfasına ekler.

Bazı senaryolarda, dosya varlığınıza sınıflandırmaları el ile eklemek isteyebilirsiniz. Bir kaynak kümesinde gruplanmış birden çok dosya varsa, kaynak kümesi düzeyinde sınıflandırmalar ekleyin.

Bölüm kaynak kümesine özel veya sistem sınıflandırması eklemek için aşağıdaki adımları izleyin:

1. Bölüme erişin veya sayfayı inceleyin ve varlık ayrıntıları sayfasına gidin.

    :::image type="content" source="./media/apply-classifications/asset-detail-page.png" alt-text="Varlık ayrıntısı sayfasını gösteren ekran görüntüsü.":::

1. **Genel bakış** sekmesinde, mevcut sınıflandırmalar olup olmadığını görmek için **sınıflandırmalar** bölümünü görüntüleyin. **Düzenle**’yi seçin.

1. **Sınıflandırmalar** açılan listesinden ilgilendiğiniz belirli sınıflandırmaları seçin. Örneğin, bir özel sınıflandırma olan bir sistem sınıflandırması ve **Customeraccountıd** olan **kredi kartı numarası**.

    :::image type="content" source="./media/apply-classifications/select-classifications.png" alt-text="Bir varlığa eklemek için sınıflandırmaların nasıl seçileceğini gösteren ekran görüntüsü.":::

1. **Kaydet**’i seçin

1. **Genel bakış** sekmesinde, seçtiğiniz sınıflandırmaların **sınıflandırmalar** bölümünün altında göründüğünü onaylayın.

    :::image type="content" source="./media/apply-classifications/confirm-classifications.png" alt-text="Bir varlığa sınıflandırmaların nasıl eklendiğini gösteren ekran görüntüsü.":::

### <a name="apply-classification-to-a-table-asset"></a>Bir tablo varlığına sınıflandırma uygulama

Azure purview veri kaynaklarınızı taradığında, otomatik olarak sınıflandırmalar tablo varlıklarına atanmaz. Tablo varlığınızın bir sınıflandırmasını istiyorsanız, el ile eklemeniz gerekir.

Bir tablo varlığına sınıflandırma eklemek için:

1. İlgilendiğiniz bir tablo varlığını bulun. Örneğin, **Müşteri** tablosu.

1. Tabloya hiçbir sınıflandırma atanmadığını doğrulayın. **Düzenle** 'yi seçin

    :::image type="content" source="./media/apply-classifications/select-edit-from-table-asset.png" alt-text="Bir tablo varlığının sınıflandırmalarının nasıl görüntüleneceğini ve düzenleneceğini gösteren ekran görüntüsü.":::

1. **Sınıflandırmalar** açılan listesinden bir veya daha fazla sınıflandırma seçin. Bu örnek, **CustomerInfo** adlı özel bir sınıflandırma kullanır, ancak bu adım için herhangi bir sınıflandırmaları seçebilirsiniz.

    :::image type="content" source="./media/apply-classifications/select-classifications-in-table.png" alt-text="Bir tablo varlığına eklemek için sınıflandırmaların nasıl seçileceğini gösteren ekran görüntüsü.":::

1. Sınıflandırmaları kaydetmek için **Kaydet** ' i seçin.

1. **Genel bakış** sayfasında, Azure purview ' in yeni sınıflandırmalarınızı eklediğini doğrulayın.

    :::image type="content" source="./media/apply-classifications/verify-classifications-added-to-table.png" alt-text="Sınıflandırmaların tablo varlığına eklendiğini gösteren ekran görüntüsü.":::

### <a name="add-classification-to-a-column-asset"></a>Bir sütun varlığına sınıflandırma ekleme

Azure purview tüm sütun varlıklarına otomatik olarak tarar ve sınıflandırmalar ekler. Ancak, Sınıflandırmayı değiştirmek istiyorsanız, bunu sütun düzeyinde yapabilirsiniz.

Bir sütuna sınıflandırma eklemek için:

1. Sütun varlığını bulun ve seçin ve ardından **genel bakış** sekmesinden **Düzenle** ' yi seçin.

1. **Şema** sekmesini seçin

    :::image type="content" source="./media/apply-classifications/edit-column-schema.png" alt-text="Bir sütunun şemasının nasıl düzenleneceğini gösteren ekran görüntüsü.":::

1. İlgilendiğiniz sütunları belirleyin ve **Sınıflandırma Ekle**' yi seçin. Bu örnek, **PasswordHash** sütununa **ortak bir parola** sınıflandırması ekler.

    :::image type="content" source="./media/apply-classifications/add-classification-to-column.png" alt-text="Bir sütuna nasıl sınıflandırma ekleneceğini gösteren ekran görüntüsü.":::

1. **Kaydet**’i seçin

1. **Şema** sekmesini seçin ve sınıflandırmanın sütuna eklendiğini onaylayın.

    :::image type="content" source="./media/apply-classifications/confirm-classification-added.png" alt-text="Bir sınıflandırmanın sütun şemasına eklendiğini nasıl doğrulayagösteren ekran görüntüsü.":::

## <a name="impact-of-rescanning-on-existing-classifications"></a>Mevcut sınıflandırmalara yeniden tarama etkisi

Sınıflandırmalar, verilerinize yönelik örnek küme denetimine göre ve bunu ayarla Regex düzenine karşı eşleşen ilk kez uygulanır. Yeniden tarama sırasında, yeni sınıflandırmalar uygulanacaksa, sütun üzerinde ek sınıflandırmalar alır. Mevcut sınıflandırmalar sütunda kalır ve el ile kaldırılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Özel sınıflandırma oluşturmayı öğrenmek için bkz. [özel sınıflandırma oluşturma](create-a-custom-classification-and-classification-rule.md).