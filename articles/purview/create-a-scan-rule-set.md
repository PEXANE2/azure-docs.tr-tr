---
title: Tarama kuralı kümesi oluşturma
description: Kuruluşunuzdaki veri kaynaklarını hızlı bir şekilde taramak için Azure purview ' de bir tarama kural kümesi oluşturun.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 12/02/2020
ms.openlocfilehash: 9662652a6a40285ad382857975ec0dd04b8ba8be
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96555453"
---
# <a name="create-a-scan-rule-set"></a>Tarama kuralı kümesi oluşturma

Azure purview kataloğunda, kuruluşunuzda veri kaynaklarını hızlıca taramanızı sağlamak için tarama kuralı kümeleri oluşturabilirsiniz.

Tarama kuralı kümesi, bir tarama kurallarını bir tarama ile kolayca ilişkilendirebilmeniz için bir dizi tarama kuralını gruplamak için bir kapsayıcıdır. Örneğin, veri kaynağı türlerinizin her biri için varsayılan tarama kuralı kümesi oluşturabilir ve ardından bu tarama kuralı kümelerini şirketinizdeki tüm taramalar için varsayılan olarak kullanabilirsiniz. Ayrıca, doğru izinlere sahip kullanıcıların, iş ihtiyacını temel alan farklı yapılandırmalara sahip başka tarama kuralı kümeleri oluşturmasını da isteyebilirsiniz.

## <a name="steps-to-create-a-scan-rule-set"></a>Tarama kuralı kümesi oluşturma adımları

Bir tarama kuralı kümesi oluşturmak için:

1. Azure purview kataloğunuzda **Yönetim Merkezi**' ni seçin.

1. Sol bölmeden **kural kümelerini Tara** ' yı seçin ve ardından **Yeni**' yi seçin.

1. **Yeni tarama kural kümesi** sayfasından, Katalog tarayıcısının **kaynak türü** açılır listesinden desteklediği veri kaynaklarını seçin. Taramayı planladığınız her veri kaynağı türü için bir tarama kuralı kümesi oluşturabilirsiniz.

1. Tarama kuralınız için bir **ad** ayarlayın. Maksimum uzunluk, boşluk izni olmayan 63 karakterdir. İsteğe bağlı olarak bir **Açıklama** girin. En fazla 256 karakter uzunluğunda olur.

   :::image type="content" source="./media/create-a-scan-rule-set/purview-home-page.png" alt-text="Tarama kuralı kümesi sayfasını gösteren ekran görüntüsü." border="true":::

1. **Devam**’ı seçin.

   **Dosya türlerini seçin** sayfası görüntülenir. Bu sayfadaki dosya türü seçeneklerinin, önceki sayfada seçtiğiniz veri kaynağı türüne göre değişiklik gösterdiğinden emin olun. Tüm dosya türleri varsayılan olarak etkindir.

      :::image type="content" source="./media/create-a-scan-rule-set/select-file-types-page.png" alt-text="Dosya türlerini seç sayfasını gösteren ekran görüntüsü.":::

   Bu sayfadaki **belge dosyası türleri** seçimi, şu Office dosya türlerini dahil etmenize veya dışlayamazsınız:. doc,. docm,. docx,. dot,. odp,. ods,. odt,. PDF,. pot,. PPS,. ppsx,. ppt,. pptm,. pptx,. xlc,. xls,. xlsb,. xlsm,. xlsx ve. xlt.

1. Onay kutusunu seçerek veya temizleyerek bir dosya türü kutucuğunu etkinleştirin veya devre dışı bırakın. Bir Data Lake türü veri kaynağı seçerseniz (örneğin, Azure Data Lake Storage 2. veya Azure Blobu), şema ayıklanıp sınıflandırmasını istediğiniz dosya türlerini etkinleştirin.

1. Belirli veri kaynağı türleri için [özel bir dosya türü de oluşturabilirsiniz](#create-a-custom-file-type).

1. **Devam**’ı seçin.

   **Sınıflandırma kurallarını seçin** sayfası görüntülenir. Bu sayfa seçilen **sistem kurallarını** ve **özel kuralları** ve seçilen sınıflandırma kurallarının toplam sayısını görüntüler. Varsayılan olarak, tüm **sistem kuralları** onay kutuları seçilidir

1. Dahil etmek veya hariç tutmak istediğiniz kurallar için, kategoriye göre genel olarak **sistem kuralları** sınıflandırma kuralı onay kutularını seçebilir veya temizleyebilirsiniz.

   :::image type="content" source="./media/create-a-scan-rule-set/select-classification-rules.png" alt-text="Sınıflandırma kurallarını Seç sayfasını gösteren ekran görüntüsü.":::

1. Kategori düğümünü genişletebilir ve tek tek onay kutularını seçebilir veya temizleyebilirsiniz. Örneğin, **Arjantin. DNı Number** için kural yüksek yanlış pozitif sonuçlar içeriyorsa, bu onay kutusunun işaretini kaldırabilirsiniz.

   :::image type="content" source="./media/create-a-scan-rule-set/select-system-rules.png" alt-text="Sistem kurallarının nasıl seçileceğini gösteren ekran görüntüsü.":::

1. Tarama kuralı kümesini oluşturmayı bitirmeden **Oluştur** ' u seçin.

### <a name="create-a-custom-file-type"></a>Özel dosya türü oluşturma

Azure purview, bir tarama kural kümesinde özel bir uzantı eklemeyi ve özel bir sütun sınırlayıcısı tanımlamayı destekler.

Özel bir dosya türü oluşturmak için:

1. [Tarama kuralı kümesi oluşturma](#steps-to-create-a-scan-rule-set) veya var olan bir tarama kuralı kümesini düzenleme adımları için adım 1 – 5 ' i izleyin.

1. **Dosya türlerini seçin** sayfasında yeni **dosya türü** ' nü seçerek yeni bir özel dosya türü oluşturun.

   :::image type="content" source="./media/create-a-scan-rule-set/select-new-file-type.png" alt-text="Dosya türlerini seçin sayfasından yeni dosya türünün nasıl seçileceğini gösteren ekran görüntüsü.":::

1. Bir **Dosya Uzantısı** ve isteğe bağlı bir **Açıklama** girin.

   :::image type="content" source="./media/create-a-scan-rule-set/new-custom-file-type-page.png" alt-text="Yeni özel dosya türü sayfasını gösteren ekran görüntüsü." border="true":::

1. Dosyanızdaki dosya içeriği türünü belirtmek için, **içindeki dosya içerikleri** için aşağıdaki seçimlerden birini yapın:

   - **Özel sınırlayıcı** ' i seçin ve kendi **özel sınırlayıcıyı** girin (yalnızca tek karakter).

   - Sistem **dosyası türü ' nü** seçin ve sistem dosyası **türü** aşağı açılan listesinden bir sistem dosyası türü (örneğin, XML) seçin.

1. Özel dosyayı kaydetmek için **Oluştur** ' u seçin.

   Sistem **dosya türlerini seç** sayfasına döner ve yeni özel dosya türünü yeni bir kutucuk olarak ekler.

   :::image type="content" source="./media/create-a-scan-rule-set/new-custom-file-type-tile.png" alt-text="Dosya türlerini seçin sayfasında yeni özel dosya türü kutucuğunu gösteren ekran görüntüsü.":::

1. Değiştirmek veya silmek istiyorsanız yeni dosya türü kutucuğunda **Düzenle** ' yi seçin.

1. Tarama kuralı kümesini yapılandırmayı tamamlaymak için **devam** ' ı seçin.

## <a name="system-scan-rule-sets"></a>Sistem tarama kural kümeleri

Sistem tarama kural kümeleri, her Azure purview kataloğu için otomatik olarak oluşturulan Microsoft tanımlı tarama kuralı kümeleridir. Her sistem tarama kuralı kümesi, belirli bir veri kaynağı türüyle ilişkilendirilir. Bir tarama oluşturduğunuzda, bir sistem tarama kuralı kümesiyle ilişkilendirebilirsiniz. Microsoft bu sistem kuralı kümelerine her güncelleştirme yaptığında, bunları kataloğunuzda güncelleştirebilir ve güncelleştirmeyi tüm ilişkili taramalara uygulayabilirsiniz.

1. Sistem tarama kuralı kümelerinin listesini görüntülemek için **Yönetim Merkezi** 'nde **kural kümelerini Tara** ' yı seçin ve **sistem** sekmesini seçin.

   :::image type="content" source="./media/create-a-scan-rule-set/system-scan-rule-sets.jpg" alt-text="Sistem tarama kuralı kümelerinin listesini gösteren ekran görüntüsü." lightbox="./media/create-a-scan-rule-set/system-scan-rule-sets.jpg":::

1. Her sistem taraması kural kümesinin bir **adı**, **kaynak türü** ve **sürümü** vardır. **Sürüm** sütununda ayarlanmış bir tarama kuralı sürüm numarasını seçerseniz, geçerli sürüm ve önceki sürümlerle ilişkili kuralları (varsa) görürsünüz.

   :::image type="content" source="./media/create-a-scan-rule-set/system-scan-rule-set-page.jpg" alt-text="Bir sistem tarama kuralı kümesi sayfasını gösteren ekran görüntüsü." lightbox="./media/create-a-scan-rule-set/system-scan-rule-set-page.jpg":::

1. Bir sistem tarama kuralı kümesi için bir güncelleştirme varsa, **Sürüm** sütununda **Güncelleştir** ' i seçebilirsiniz. Sistem tarama kuralı sayfasında, **Yeni bir sürüm Seç** açılan listesinden bir sürümden seçim yapın. Sayfa, yeni sürüm ve geçerli sürümle ilişkili sistem sınıflandırma kurallarının bir listesini sağlar.

   :::image type="content" source="./media/create-a-scan-rule-set/system-scan-rule-set-version.jpg" alt-text="Bir sistem tarama kuralı kümesinin sürümünün nasıl değiştirileceğini gösteren ekran görüntüsü." lightbox="./media/create-a-scan-rule-set/system-scan-rule-set-version.jpg":::

### <a name="associate-a-scan-with-a-system-scan-rule-set"></a>Bir taramayı sistem tarama kural kümesiyle ilişkilendirme

[Bir tarama oluşturduğunuzda](tutorial-scan-data.md#scan-data-into-the-catalog), bunu bir sistem tarama kuralı kümesiyle ilişkilendirmeyi aşağıda gösterildiği gibi seçebilirsiniz:

1. **Tarama kuralı kümesi seçin** sayfasında, sistem tarama kuralı kümesini seçin.

   :::image type="content" source="./media/create-a-scan-rule-set/set-system-scan-rule-set.jpg" alt-text="Tarama için bir sistem tarama kuralı kümesinin nasıl seçileceğini gösteren ekran görüntüsü." lightbox="./media/create-a-scan-rule-set/set-system-scan-rule-set.jpg":::

1. **Devam**' ı seçin ve ardından **Kaydet ve Çalıştır**' ı seçin.
