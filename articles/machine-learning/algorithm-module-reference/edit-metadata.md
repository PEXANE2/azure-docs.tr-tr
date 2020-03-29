---
title: 'Meta verileri edit: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Bir veri kümesindeki sütunlarla ilişkili meta verileri değiştirmek için Azure Machine Learning'deki Meta Verileri Edit modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 9853a3decc8d145fee58d1da526926e224ee2030
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064254"
---
# <a name="edit-metadata-module"></a>Meta veri modüllerini edit

Bu makalede, Azure Machine Learning tasarımcısına (önizleme) dahil olan bir modül açıklanmaktadır.

Veri kümesindeki sütunlarla ilişkili meta verileri değiştirmek için Meta Verileri Edit modülünü kullanın. Meta veri seti edin den sonra veri kümesinin değeri ve veri türü değişecektir.

Tipik meta veri değişiklikleri şunları içerebilir:
  
+ Boolean veya sayısal sütunları kategorik değerler olarak ele alma.
  
+ Hangi sütunun **sınıf** etiketini içerdiğini veya kategorilere ayırmak veya tahmin etmek istediğiniz değerleri içerdiğini gösterir.
  
+ Sütunları özellik olarak işaretleme.
  
+ Tarih/saat değerlerini sayısal değerlerle veya tam tersi olarak değiştirme.
  
+ Sütunları yeniden adlandırma.
  
 Genellikle akış aşağı modülü gereksinimlerini karşılamak için sütun tanımını değiştirmeniz gerektiğinde Meta Verileri Düzenle'yi kullanın. Örneğin, bazı modüller yalnızca belirli veri türleri ile çalışır veya `IsFeature` `IsCategorical`sütunlarda bayraklar gerektirir, örneğin.  
  
 Gerekli işlemi yaptıktan sonra, meta verileri özgün durumuna sıfırlayabilirsiniz.
  
## <a name="configure-edit-metadata"></a>Meta verileri yapılandır
  
1. Azure Machine Learning tasarımcısında, Meta Verileri Edit modülünü ardışık kurulumuza ekleyin ve güncelleştirmek istediğiniz veri kümesini bağlayın. Modülü **Veri Dönüşümü** kategorisinde bulabilirsiniz.
  
1. Modülün sağ panelindeki **Sütunu Edit'i** tıklatın ve çalışmak için sütun veya sütun kümesini seçin. Sütunları ada veya dizinolarak tek tek seçebilir veya türüne göre bir sütun grubu seçebilirsiniz.  
  
1. Seçili sütunlara farklı bir veri türü atamanız gerekiyorsa **Veri türü** seçeneğini seçin. Belirli işlemler için veri türünü değiştirmeniz gerekebilir. Örneğin, kaynak veri kümenizde metin olarak işlenen sayılar varsa, bunları matematik işlemlerini kullanmadan önce sayısal bir veri türüne değiştirmeniz gerekir.

    + Desteklenen veri türleri **String,** **Integer**, **Double**, **Boolean**ve **DateTime'dır.**

    + Birden çok sütun seçerseniz, meta veri değişikliklerini *tüm* seçili sütunlara uygulamanız gerekir. Örneğin, iki veya üç sayısal sütun seçtiğinizi varsayalım. Bunların tümünü bir dize veri türüne değiştirebilir ve tek bir işlemde yeniden adlandırabilirsiniz. Ancak, bir sütunu dize veri türüne, başka bir sütunu da float'tan tamsayıya değiştiremezsiniz.
  
    + Yeni bir veri türü belirtmezseniz, sütun meta verileri değişmez.

    + Meta Verileri Edit işlemini yaptıktan sonra sütun türü ve değerleri değişecektir. Sütun veri türünü sıfırlamak için Meta Verilerini Edit'i kullanarak istediğiniz zaman özgün veri türünü kurtarabilirsiniz.  

    > [!NOTE]
    > Herhangi bir numara türünü **DateTime** türüyle değiştirirseniz, **DateTime Format** alanını boş bırakın. Şu anda hedef veri biçimini belirtmek mümkün değildir.  

1. Seçili sütunlarda değerlerin kategori olarak ele alınması gerektiğini belirtmek için **Kategorik** seçeneğini seçin.

    Örneğin, 0, 1 ve 2 sayılarını içeren bir sütununuz olabilir, ancak sayıların aslında "Sigara içen", "Sigara içmeyen" ve "Bilinmeyen" anlamına geldiğini bilin. Bu durumda, sütunu kategorik olarak işaretleyerek, değerlerin sayısal hesaplamalarda değil, yalnızca verileri gruplandırmak için kullanıldığından emin olun.
  
1. Azure Machine Learning'in bir modeldeki verileri kullanma şeklini değiştirmek istiyorsanız **Alanlar** seçeneğini kullanın.

    + **Özellik**: Yalnızca özellik sütunlarında çalışan modüllerde bir sütunu özellik olarak işaretlemek için bu seçeneği kullanın. Varsayılan olarak, tüm sütunlar başlangıçta özellik olarak kabul edilir.  
  
    + **Etiket**: Öngörülebilir öznitelik veya hedef değişken olarak da bilinen etiketi işaretlemek için bu seçeneği kullanın. Birçok modül, veri kümesinde tam olarak bir etiket sütununun bulunmasını gerektirir.

        Çoğu durumda, Azure Machine Learning bir sütunun sınıf etiketi içerdiği ne varsa çıkarılabilir. Bu meta verileri ayarlayarak, sütunun doğru tanımlandığından emin olabilirsiniz. Bu seçeneği ayarlamak veri değerlerini değiştirmez. Yalnızca bazı makine öğrenme algoritmalarının verileri işleme şeklini değiştirir.
  
    > [!TIP]
    > Bu kategorilere uymayan verileriniz var mı? Örneğin, veri kümeniz değişken ler olarak yararlı olmayan benzersiz tanımlayıcılar gibi değerler içerebilir. Bazen bu tür iD'ler bir modelde kullanıldığında sorunlara neden olabilir.
    >
    > Neyse ki, Azure Machine Learning tüm verilerinizi saklar, böylece bu tür sütunları veri kümesinden silmek zorunda kalmamanız gerekir. Bazı özel sütun lar üzerinde işlem gerçekleştirmeniz gerektiğinde, [Dataset modülündeki Sütunları Seç'i](select-columns-in-dataset.md) kullanarak diğer tüm sütunları geçici olarak kaldırmanız gerekir. Daha sonra [Sütun Ekle](add-columns.md) modülünü kullanarak sütunları veri kümesine geri birleştirebilirsiniz.  
  
1. Önceki seçimleri temizlemek ve meta verileri varsayılan değerlere geri yüklemek için aşağıdaki seçenekleri kullanın.  
  
    + **Özelliği temizle**: Özellik bayrağını kaldırmak için bu seçeneği kullanın.  
  
         Tüm sütunlar başlangıçta özellik olarak kabul edilir. Matematiksel işlemler gerçekleştiren modüller için, sayısal sütunların değişken olarak ele alınmasını önlemek için bu seçeneği kullanmanız gerekebilir.
  
    + **Etiketi temizle**: **Etiket** meta verilerini belirtilen sütundan kaldırmak için bu seçeneği kullanın.  
  
    + **Skoru temizleme**: **Puan** meta verilerini belirtilen sütundan kaldırmak için bu seçeneği kullanın.  
  
         Şu anda bir sütunu Azure Machine Learning'de açıkça puan olarak işaretleyemezsiniz. Ancak, bazı işlemler bir sütunun dahili olarak puan olarak işaretlenmeyle sonuçlanır. Ayrıca, özel bir R modülü çıktı puan değerleri olabilir.

1. **Yeni sütun adları**için, seçili sütun veya sütunların yeni adını girin.  
  
    + Sütun adları yalnızca UTF-8 kodlaması tarafından desteklenen karakterleri kullanabilir. Tamamen boşluklardan oluşan boş dizeler, nulls veya adlara izin verilmez.  
  
    + Birden çok sütunu yeniden adlandırmak için, sütun dizinleri sırasına göre adları virgülle ayrılmış bir liste olarak girin.  
  
    + Seçili tüm sütunların adı yeniden adlandırılmalıdır. Sütunları atlayamaz veya atlayamaz.  
  
1. Boru hattını gönderin.  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın.
