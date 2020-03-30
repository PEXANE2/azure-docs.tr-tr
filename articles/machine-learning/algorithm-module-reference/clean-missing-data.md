---
title: 'Eksik Verileri Temizle: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Eksik değerleri kaldırmak, değiştirmek veya çıkarmak için Azure Machine Learning'de Temiz Eksik Veri modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 14c3bc968da9d398fbc14eda74378047cf28277b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477706"
---
# <a name="clean-missing-data-module"></a>Eksik Veri modüllerini temizle

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Eksik değerleri kaldırmak, değiştirmek veya çıkarmak için bu modülü kullanın. 

Veri bilim adamları genellikle eksik değerler için verileri denetler ve ardından verileri düzeltmek veya yeni değerler eklemek için çeşitli işlemler gerçekleştirir. Bu tür temizleme işlemlerinin amacı, bir modeli eğitirken ortaya çıkabilecek eksik verilerin neden olduğu sorunları önlemektir. 

Bu modül, eksik değerleri "temizlemek" için birden çok türde işlemi destekler:

+ Eksik değerleri bir yer tutucu, ortalama veya başka bir değerle değiştirme
+ Eksik değerlere sahip satır ve sütunları tamamen kaldırma
+ İstatistiksel yöntemlere dayalı değerler inferring


Bu modülü kullanmak kaynak veri kümenizi değiştirmez. Bunun yerine, çalışma alanınızda sonraki iş akışında kullanabileceğiniz yeni bir veri kümesi oluşturur. Yeniden kullanmak için yeni, temizlenmiş veri kümesini de kaydedebilirsiniz.

Bu modül aynı zamanda eksik değerleri temizlemek için kullanılan dönüşümün bir tanımını da çıkarmaktadır. Bu dönüşümü, [Dönüşüm Uygula](./apply-transformation.md) modüllerini kullanarak aynı şema olan diğer veri kümelerinde yeniden kullanabilirsiniz.  

## <a name="how-to-use-clean-missing-data"></a>Temiz Eksik Verileri Nasıl Kullanılır?

Bu modül bir temizleme işlemi tanımlamanızı sağlar. Temizleme işlemini daha sonra yeni verilere uygulayabilmek için de kaydedebilirsiniz. Temizleme işleminin nasıl oluşturulup kaydedilene ilgili aşağıdaki bölümlere bakın: 
 
+ [Eksik değerleri değiştirmek için](#replace-missing-values)
  
+ [Yeni verilere temizleme dönüşümü uygulamak için](#apply-a-saved-cleaning-operation-to-new-data)
 
> [!IMPORTANT]
> Eksik değerleri işlemek için kullandığınız temizleme yöntemi sonuçlarınızı önemli ölçüde etkileyebilir. Farklı yöntemler denemenizi öneririz. Hem belirli bir yöntemin kullanımının gerekçesini hem de sonuçların kalitesini göz önünde bulundurun.

### <a name="replace-missing-values"></a>Eksik değerleri değiştirme  

[Temiz Eksik Veri](./clean-missing-data.md) modülünü bir veri kümesine her uyguladığınızda, seçtiğiniz tüm sütunlara aynı temizleme işlemi uygulanır. Bu nedenle, farklı yöntemleri kullanarak farklı sütunları temizlemeniz gerekiyorsa, modülün ayrı örneklerini kullanın.

1.  Eksik [Verileri Temizle](./clean-missing-data.md) modülünü ardışık kurulumuza ekleyin ve eksik değerlere sahip veri kümesini bağlayın.  
  
2.  **Sütunların temizlenmesi için,** değiştirmek istediğiniz eksik değerleri içeren sütunları seçin. Birden çok sütun seçebilirsiniz, ancak tüm seçili sütunlarda aynı değiştirme yöntemini kullanmanız gerekir. Bu nedenle, genellikle dize sütunlarını ve sayısal sütunları ayrı ayrı temizlemeniz gerekir.

    Örneğin, tüm sayısal sütunlarda eksik değerleri denetlemek için:

    1. Eksik **Verileri Temizle** modülünü seçin ve modülün sağ panelindeki **Edit sütununa** tıklayın.

    3. **Ekle**için, açılan listeden **Sütun türlerini** seçin ve ardından **Sayısal'ı**seçin. 
  
    Seçtiğiniz herhangi bir temizleme veya değiştirme yöntemi, seçimdeki **tüm** sütunlar için geçerli olmalıdır. Herhangi bir sütundaki veriler belirtilen işlemle uyumsuzsa, modül bir hata döndürür ve ardışık işlemi durdurur.
  
3.  **Minimum eksik değer oranı**için, yapılacak işlem için gereken minimum eksik değer sayısını belirtin.  
  
    Bu seçeneği, veri kümesinde temizleme işleminin hangi koşullarda gerçekleştirildirileni tanımlamak için **Maksimum eksik değer oranıyla** birlikte kullanırsınız. Çok fazla veya çok az satır eksik değerler varsa, işlem gerçekleştirilemez. 
  
    Girdiğiniz sayı, eksik değerlerin sütundaki tüm değerlere **oranını** gösterir. Varsayılan olarak, **Minimum eksik değer oranı** özelliği 0 olarak ayarlanır. Bu, eksik değerlerin yalnızca bir eksik değer olsa bile temizlendiğini gösterir. 

    > [!WARNING]
    > Belirtilen işlemin uygulanabilmesi için bu koşulun her sütun tarafından karşılanması gerekir. Örneğin, üç sütun seçtiğinizi ve ardından eksik değerlerin en az oranını 0,2 'ye (%20) ayarladığınız, ancak yalnızca bir sütunun gerçekte %20 eksik değere sahip olduğunu varsayalım. Bu durumda, temizleme işlemi yalnızca %20'den fazla eksik değeri olan sütuna uygulanır. Bu nedenle, diğer sütunlar değişmeden olacaktır.
    > 
    > Eksik değerlerin değiştirilip değiştirilmediği konusunda herhangi bir şüpheniz varsa, **eksik değer göstergesi sütunu oluştur**seçeneğini seçin. Her sütunun minimum ve maksimum aralıklar için belirtilen ölçütleri karşılayıp karşılmadığını belirtmek için veri kümesine bir sütun eklenir.  
  
4. **Maksimum eksik değer oranı**için, gerçekleştirilecek işlem için bulunabilecek en fazla eksik değer sayısını belirtin.   
  
    Örneğin, yalnızca satırların %30'u veya daha azı eksik değerler içeriyorsa eksik değer değiştirme gerçekleştirmek isteyebilirsiniz, ancak satırların %30'undan fazlası eksik değerlere sahipse değerleri olduğu gibi bırakın.  
  
    Sayıyı, eksik değerlerin sütundaki tüm değerlere oranı olarak tanımlarsınız. Varsayılan olarak, **Maksimum eksik değer oranı** 1 olarak ayarlanır. Bu, sütundaki değerlerin %100'ü eksik olsa bile eksik değerlerin temizlenmitir.  
  
   
  
5. **Temizleme Modu**için, eksik değerleri değiştirmek veya kaldırmak için aşağıdaki seçeneklerden birini seçin:  
  
  
    + **Özel ikame değeri**: Tüm eksik değerler için geçerli olan bir yer tutucu değeri (0 veya NA gibi) belirtmek için bu seçeneği kullanın. Değiştirme olarak belirttiğiniz değer, sütunun veri türüyle uyumlu olmalıdır.
  
    + **Ortalama ile değiştir**: Sütun ortalamasını hesaplar ve sütundaki her eksik değer için ortalamayı yedek değer olarak kullanır.  
  
        Yalnızca Tamsayı, Çift veya Boolean veri türlerine sahip sütunlar için geçerlidir.  
  
    + **Medyan ile değiştir**: Sütun ortanca değerini hesaplar ve sütundaki eksik değerin yerine ortanca değeri kullanır.  
  
        Yalnızca Tamsayı veya Çift veri türleri olan sütunlar için geçerlidir. 
  
    + **Modla Değiştir**: Sütunun modunu hesaplar ve sütundaki her eksik değer için değiştirme değeri olarak modu kullanır.  
  
        Tamsayı, Çift, Boolean veya Kategorik veri türleri olan sütunlar için geçerlidir. 
  
    + **Satırın tamamını kaldır**: Veri kümesinde bir veya daha fazla eksik değeri olan satırları tamamen kaldırır. Eksik değer rasgele eksik olarak kabul edilebilir, bu yararlıdır.  
  
    + **Sütunun tamamını kaldır**: Veri kümesinde bir veya daha fazla eksik değeri olan sütunları tamamen kaldırır.  
  
    
  
6. Seçeneği seçtiyseniz **Değiştirme değeri** kullanılabilir, **Özel ikame değeri**. Sütundaki tüm eksik değerler için değiştirme değeri olarak kullanmak üzere yeni bir değer yazın.  
  
    Bu seçeneği yalnızca Tamsayı, Çift, Boolean veya String olan sütunlarda kullanabileceğinizi unutmayın.
  
7. **Eksik değer göstergesi sütunu oluşturma**: Sütundaki değerlerin eksik değer temizleme ölçütlerini karşılayıp karşılayıp karşılmadığını gösteren bazı göstergeler oluşturmak istiyorsanız bu seçeneği seçin. Bu seçenek, özellikle yeni bir temizleme işlemi ayarlarken ve tasarladığı gibi çalıştığından emin olmak istediğinizde kullanışlıdır.
  
8. Boru hattını gönderin.

### <a name="results"></a>Sonuçlar

Modül iki çıktı döndürür:  

-   **Temizlenmiş veri kümesi**: Bu seçeneği seçtiyseniz, bir gösterge sütunuyla birlikte, belirtilen eksik değerlerle birlikte, seçili sütunlardan oluşan bir veri kümesidir.  

    Temizleme için seçilmemiş sütunlar da "geçirilir".  
  
-  **Temizleme dönüşümü**: Çalışma alanınızda kaydedilebilen ve daha sonra yeni verilere uygulanabilen temizleme için kullanılan veri dönüşümü.

### <a name="apply-a-saved-cleaning-operation-to-new-data"></a>Kaydedilen temizleme işlemini yeni verilere uygulama  

Temizleme işlemlerini sık sık tekrarlamanız gerekiyorsa, aynı veri kümesiyle yeniden kullanmak için veri temizleme tarifinizi bir *dönüştürme*olarak kaydetmenizi öneririz. Temizleme dönüşümlerini kaydetmek, sık sık yeniden içe aktarmanız ve ardından aynı şema olan verileri temizlemeniz gerekiyorsa özellikle yararlıdır.  
      
1.  Dönüşüm [Uygula](./apply-transformation.md) modüllerini ardınıza ekleyin.  
  
2.  Temizlemek istediğiniz veri kümesini ekleyin ve veri kümesini sağ giriş bağlantı noktasına bağlayın.  
  
3.  Tasarımcının sol bölmesinde **Transforms** grubunu genişletin. Kaydedilen dönüşümü bulun ve boru hattına sürükleyin.  

4.  Kaydedilen dönüşümü [Dönüşüm Uygula'nın](./apply-transformation.md)sol giriş bağlantı noktasına bağlayın. 

    Kaydedilen dönüşüm uyguladığınız zaman, dönüşümün uygulandığı sütunları seçemezsiniz. Bunun nedeni, dönüşümün zaten tanımlanmış olması ve özgün işlemde belirtilen sütunlara otomatik olarak uygulanmış olmasıdır.

    Ancak, sayısal sütunlar bir alt kümesi üzerinde bir dönüşüm oluşturduğunuzu varsayalım. Eksik değerler yalnızca eşleşen sayısal sütunlarda değiştirilmelerinden, bu dönüşümü hata yükseltmeden karışık sütun türlerinin bir veri kümesine uygulayabilirsiniz.

6.  Boru hattını gönderin.  

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 