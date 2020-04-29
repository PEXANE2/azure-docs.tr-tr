---
title: 'Bölüm ve örnek: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Bir veri kümesinde örnekleme gerçekleştirmek veya veri kümesinizden bölümler oluşturmak için Azure Machine Learning bölümünde Bölüm ve örnek modülün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 0392a05df485b45b1244f4542ed55af1837dca3d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79477519"
---
# <a name="partition-and-sample-module"></a>Bölüm ve örnek modül

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Bir veri kümesinde örnekleme gerçekleştirmek veya veri kümesinizden bölümler oluşturmak için bölüm ve örnek modülünü kullanın.

Örnekleme, makine öğreniminde önemli bir araçtır çünkü aynı değer oranını koruyarak bir veri kümesinin boyutunu azaltmanızı sağlar. Bu modül makine öğreniminde önemli olan birkaç ilgili görevi destekler: 

- Verilerinizi aynı boyuttaki birden çok alt tabloya bölme. 

  Bölümleri çapraz doğrulama için kullanabilir veya rastgele gruplara servis talepleri atayabilirsiniz.

- Verileri gruplara ayırma ve ardından belirli bir gruptaki verilerle çalışma. 

  Farklı gruplara rastgele durumlar atadıktan sonra, yalnızca bir grupla ilişkili özellikleri değiştirmeniz gerekebilir.

- Aşağıdakine. 

  Verilerin bir yüzdesini çıkarabilir, rastgele örnekleme uygulayabilir ya da veri kümesini dengelemek için kullanılacak bir sütun seçebilir ve değerleri üzerinde kümeleştirilmiş örnekleme yapabilirsiniz.

- Test için daha küçük bir veri kümesi oluşturma. 

  Çok fazla veriniz varsa, işlem hattını ayarlarken yalnızca ilk *n* satırı kullanmak ve ardından modelinizi oluştururken tam veri kümesini kullanmaya geçiş yapmak isteyebilirsiniz. Ayrıca, geliştirmede kullanılmak üzere daha küçük bir veri kümesi oluşturmak için örnekleme kullanabilirsiniz.

## <a name="configure-the-module"></a>Modülü yapılandırma

Bu modül, verilerinizi bölümlere bölmek veya örneklemeye yönelik aşağıdaki yöntemleri destekler. Önce yöntemi seçin ve sonra yöntemin gerektirdiği ek seçenekleri ayarlayın.

- Head
- Örnekleme
- Katlara ata
- Katlama Seç

### <a name="get-top-n-rows-from-a-dataset"></a>Bir veri kümesinden Ilk N satır al

Yalnızca ilk *n* satırı almak için bu modu kullanın. Bu seçenek, bir işlem hattını az sayıda satırda test etmek isterseniz ve verilerin herhangi bir şekilde dengelenmesi veya örneklenmesi gerekmiyorsa yararlıdır.

1. Arabirimde **bölüm ve örnek** modülünü ekleyin ve veri kümesini bağlayın.  

1. **Bölüm veya örnek modu**: Bu seçeneği **Head**olarak ayarlayın.

1. **Seçilecek satır sayısı**: Döndürülecek satır sayısını girin.

   Satır sayısı negatif olmayan bir tamsayı olmalıdır. Seçilen satır sayısı veri kümesindeki satır sayısından büyükse, tüm veri kümesi döndürülür.

1. İşlem hattını gönderme.

Modül yalnızca belirtilen satır sayısını içeren tek bir veri kümesini çıktı. Satırlar her zaman veri kümesinin üstünden okunurdur.

### <a name="create-a-sample-of-data"></a>Veri örneği oluşturma

Bu seçenek, basit rastgele örnekleme veya düzensiz rastgele örneklemeyi destekler. Test için daha küçük bir temsili örnek veri kümesi oluşturmak istiyorsanız yararlıdır.

1. İşlem hattınızı **bölüm ve örnek** modülünü ekleyin ve veri kümesini bağlayın.

1. **Bölüm veya örnek modu**: **örnekleme**için bu seçeneği ayarlayın.

1. **Örnekleme oranı**: 0 ile 1 arasında bir değer girin. Bu değer, kaynak veri kümesinden çıktı veri kümesine dahil edilecek satırların yüzdesini belirtir.

   Örneğin, özgün veri kümesinin yalnızca yarısını istiyorsanız örnekleme hızının yüzde 50 olması gerektiğini `0.5` belirtmek için girin.

   Giriş veri kümesinin satırları, belirtilen oranına göre karıştırılmış ve çıktı veri kümesine seçmeli olarak yerleştirildi.

1. **Örnekleme Için rastgele çekirdek**: isteğe bağlı olarak, çekirdek değeri olarak kullanılacak bir tamsayı girin.

   Satırların her seferinde aynı şekilde bölüneceğini istiyorsanız bu seçenek önemlidir. Varsayılan değer **0**' dır, yani bir başlangıç çekirdeği sistem saatine göre oluşturulur. Bu değer, ardışık düzeni her çalıştırdığınızda biraz farklı sonuçlara yol açabilir.

1. **Örnekleme için ayrılmış bölme**: veri kümesindeki satırların örnekleme öncesinde bazı anahtar sütunlara göre eşit olarak bölünebilmesi önemliyse bu seçeneği belirleyin.

   **Örnekleme Için Stratification anahtar sütunu**için, veri kümesini bölmek üzere tek bir *konuşmasıyla Strata sütunu* seçin. Veri kümesindeki satırlar şu şekilde bölünür:

   1. Tüm giriş satırları, belirtilen konuşmasıyla Strata sütunundaki değerler tarafından gruplandırılır (alt).

   1. Satırlar her grup içinde karıştırılır.

   1. Her grup, belirtilen oranı karşılamak için, çıktı veri kümesine seçmeli olarak eklenir.


1. İşlem hattını gönderme.

   Bu seçenekle modül, verilerin temsili bir örneklemeyi içeren tek bir veri kümesi çıkarır. Veri kümesinin kalan, örneksiz olmayan kısmı çıkış değil. 

## <a name="split-data-into-partitions"></a>Verileri bölümlere ayırma

Veri kümesini verilerin alt kümelerine bölmek istediğinizde bu seçeneği kullanın. Bu seçenek ayrıca, çapraz doğrulama için özel sayıda katların oluşturulması veya satırları birkaç gruba bölmek istediğinizde yararlı olur.

1. İşlem hattınızı **bölüm ve örnek** modülünü ekleyin ve veri kümesini bağlayın.

1. **Bölüm veya örnek mod**Için, **katlara ata**' yı seçin.

1. **Bölümlemede değiştirme kullan**: örneklenmiş satırın olası yeniden kullanım için satır havuzuna geri dönmesi istiyorsanız bu seçeneği belirleyin. Sonuç olarak, aynı satır birkaç katlara atanabilir.

   Değiştirme (varsayılan seçenek) kullanmıyorsanız, Örneklenmiş satır olası yeniden kullanım için satır havuzuna geri yerleştirmez. Sonuç olarak, her satır yalnızca bir katlamalı atanabilir.

1. **Rastgele bölme**: satırların katlara rastgele atanmasını istiyorsanız bu seçeneği belirleyin.

   Bu seçeneği seçmezseniz, satırlar hepsini bir kez deneme yöntemiyle katlara atanır.

1. **Rastgele çekirdek**: isteğe bağlı olarak, çekirdek değeri olarak kullanılacak bir tamsayı girin. Satırların her seferinde aynı şekilde bölüneceğini istiyorsanız bu seçenek önemlidir. Aksi takdirde, varsayılan **0** değeri rastgele bir başlangıç temel değerinin kullanılacağı anlamına gelir.

1. **Bölümleyici yöntemini belirtin**: şu seçenekleri kullanarak verilerin her bölüme nasıl yapılacağını nasıl istediğinizi belirtin:

   - **Bölüm eşit**: her bölüme eşit sayıda satır yerleştirmek için bu seçeneği kullanın. Çıkış bölümlerinin sayısını belirtmek için, **eşit olarak bölünecek kadar olan katların sayısını belirtin** kutusuna bir tamsayı girin.

   - **Özelleştirilmiş oranlar Içeren bölüm**: Bu seçeneği, her bölümün boyutunu virgülle ayrılmış bir liste olarak belirtmek için kullanın.

     Örneğin, üç bölüm oluşturmak istediğinizi varsayalım. İlk bölüm, verilerin yüzde 50 'ini içerir. Kalan iki bölüm, verilerin yüzde 25 ' i içerir. **Virgülle ayrılmış oranlar listesinde** şu sayıları girin: **5,. 25,. 25**.

     Tüm bölüm boyutlarının toplamı tam olarak 1 ' i içermelidir.

     *1 ' den az*olan sayı girerseniz, kalan satırları tutmak için ek bir bölüm oluşturulur. Örneğin, **.2** ve **.3**değerlerini girerseniz, tüm satırların yüzde 50 ' unu tutmak üzere üçüncü bir bölüm oluşturulur.
     
     *1 ' den fazla*ekleyen sayılar girerseniz, işlem hattını çalıştırdığınızda bir hata oluşur.

1. Dikey **bölme**: satırların bölünmesi sırasında dikey olmasını istiyorsanız bu seçeneği belirleyin ve ardından _konuşmasıyla Strata sütununu_seçin.

1. İşlem hattını gönderme.

   Bu seçenekle, modül birden çok veri kümesi çıktısı verir. Veri kümeleri, belirttiğiniz kurallara göre bölümlendirilir.

### <a name="use-data-from-a-predefined-partition"></a>Önceden tanımlanmış bir bölümden verileri kullanma  

Bir veri kümesini birden çok bölüme ayırdığınızda ve şimdi daha fazla analiz veya işleme için her bir bölümü açmak istiyorsanız bu seçeneği kullanın.

1. **Bölüm ve örnek** modülünü işlem hattına ekleyin.

1. Modülü önceki **bölüm ve örnek**örneğinin çıkışına bağlayın. Bu örnek, birkaç bölüm oluşturmak için **katlara ata** seçeneğini kullanmış olmalıdır.

1. **Bölüm veya örnek modu**: **katlama**Seç ' i seçin.

1. **Hangi katlamayı örnekleneceğini belirtin**: dizinini girerek kullanılacak bir bölüm seçin. Bölüm dizinleri 1 tabanlıdır. Örneğin, veri kümesini üç parçaya ayrılmışsa, bölümler 1, 2 ve 3 ' e sahip olur.

   Geçersiz bir dizin değeri girerseniz, bir tasarım zamanı hatası oluşur: "hata 0018: veri kümesi geçersiz veri içeriyor."

   Veri kümesini katlara göre gruplandırmanın yanı sıra, veri kümesini iki grup halinde ayırabilirsiniz: hedef katlama ve diğer her şey. Bunu yapmak için, tek bir katın dizinini girin ve ardından **Seçilen katlamayı** , belirtilen katdaki verileri almak için Seçileni Seç seçeneğini belirleyin.

1. Birden çok bölümle çalışıyorsanız, her bölümü işlemek için **bölüm ve örnek** modülün daha fazla örneğini eklemeniz gerekir.

   Örneğin, ikinci satırdaki **bölüm ve örnek** modül, **katlara atanacak**şekilde ayarlanır ve üçüncü satırdaki modül **katlama Seç**olarak ayarlanır.   

   ![Bölüm ve örnek](./media/module/partition-and-sample.png)

1. İşlem hattını gönderme.

   Bu seçenekle, modül yalnızca bu katlama için atanan satırları içeren tek bir veri kümesi çıkarır.

> [!NOTE]
>  Katlama gösterimlerini doğrudan görüntüleyemezsiniz. Yalnızca meta verilerde bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 