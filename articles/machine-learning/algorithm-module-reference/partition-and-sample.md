---
title: 'Bölüm ve Örnek: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Bir veri kümesinde örnekleme gerçekleştirmek veya veri setinizden bölümler oluşturmak için Bölüm ve Örnekleme modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 0392a05df485b45b1244f4542ed55af1837dca3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477519"
---
# <a name="partition-and-sample-module"></a>Bölüm ve Örnek modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Bir veri kümesinde örnekleme gerçekleştirmek veya veri setinizden bölümler oluşturmak için Bölüm ve Örnekleme modüllerini kullanın.

Örnekleme, aynı değer oranını korurken bir veri kümesinin boyutunu küçültmenize olanak sağlar, çünkü makine öğreniminde önemli bir araçtır. Bu modül, makine öğreniminde önemli olan ilgili görevleri destekler: 

- Verilerinizi aynı boyuttaki birden çok alt bölüme bölme. 

  Bölümleri çapraz doğrulama veya servis taleplerini rasgele gruplara atamak için kullanabilirsiniz.

- Verileri gruplara ayırma ve ardından belirli bir gruptan gelen verilerle çalışma. 

  Servis taleplerini farklı gruplara rasgele atadıktan sonra, yalnızca bir grupla ilişkili özellikleri değiştirmeniz gerekebilir.

- Örnekleme. 

  Verilerin bir yüzdesini ayıklayabilir, rasgele örnekleme uygulayabilir veya veri kümesini dengelemek ve değerleri üzerinde katmanlı örnekleme gerçekleştirmek için kullanılacak bir sütun seçebilirsiniz.

- Sınama için daha küçük bir veri kümesi oluşturma. 

  Çok fazla veriniz varsa, ardışık lığı ayarlarken yalnızca ilk *n* satırlarını kullanmak ve ardından modelinizi oluştururken tam veri kümesini kullanmaya geçmek isteyebilirsiniz. Geliştirmede kullanılmak üzere daha küçük bir veri kümesi oluşturmak için örneklemeyi de kullanabilirsiniz.

## <a name="configure-the-module"></a>Modülü yapılandırın

Bu modül, verilerinizi bölümlere bölmek veya örnekleme için aşağıdaki yöntemleri destekler. Önce yöntemi seçin ve sonra yöntemin gerektirdiği ek seçenekler ayarlayın.

- Head
- Örnekleme
- Kıvrımlara atama
- Kat seçin

### <a name="get-top-n-rows-from-a-dataset"></a>Veri kümesinden TOP N satırlarını alma

Yalnızca ilk *n* satırlarını almak için bu modu kullanın. Bu seçenek, bir ardışık hattı az sayıda satırda sınamak istiyorsanız ve verilerin herhangi bir şekilde dengelenmesi veya örneklenmesi gerekmezse yararlıdır.

1. **Arabirimdeki bölüm ve örnek** modülünü ardışık noktanıza ekleyin ve veri kümesini bağlayın.  

1. **Bölüm veya örnek modu**: Bu seçeneği **Head**olarak ayarlayın.

1. **Seçilecek satır sayısı**: Döndürülecek satır sayısını girin.

   Satır sayısı negatif olmayan bir tamsayı olmalıdır. Seçili satır sayısı veri kümesindeki satır sayısından daha büyükse, tüm veri kümesi döndürülür.

1. Boru hattını gönderin.

Modül, yalnızca belirtilen satır sayısını içeren tek bir veri kümesini çıkar. Satırlar her zaman veri kümesinin üst kısmından okunur.

### <a name="create-a-sample-of-data"></a>Veri örneği oluşturma

Bu seçenek basit rasgele örneklemeyi veya katmanlı rasgele örneklemeyi destekler. Sınama için daha küçük bir temsili örnek veri kümesi oluşturmak istiyorsanız yararlıdır.

1. Bölüm **ve Örnek** modülünü ardışık kurulumunuza ekleyin ve veri kümesini bağlayın.

1. **Bölüm veya örneklem modu**: Bu seçeneği **Örnekleme**olarak ayarlayın.

1. **Örnekleme hızı**: 0 ile 1 arasında bir değer girin. bu değer, çıktı veri kümesine dahil edilmesi gereken kaynak veri kümesinden satır yüzdesini belirtir.

   Örneğin, özgün veri kümesinin yalnızca yarısını istiyorsanız, örnekleme oranının yüzde 50 olması gerektiğini belirtmek için girin. `0.5`

   Giriş veri kümesinin satırları, belirtilen orana göre çıktı veri kümesine karıştırılır ve seçici olarak yerleştirilir.

1. **Örnekleme için rastgele tohum**: İsteğe bağlı olarak, tohum değeri olarak kullanmak üzere bir tamsayı girin.

   Satırların her seferinde aynı şekilde bölünmesini istiyorsanız, bu seçenek önemlidir. Varsayılan değer **0'dır,** yani sistem saatine göre bir başlangıç tohumu oluşturulur. Bu değer, ardışık hattı her çalıştırdığınızda biraz farklı sonuçlara yol açabilir.

1. **Örnekleme için katmanlı bölme**: Veri kümesindeki satırların örneklemeden önce bazı anahtar sütunlar tarafından eşit olarak bölünmesi önemliyse bu seçeneği seçin.

   **Örnekleme için Tabakalaşma anahtar sütunu için,** veri kümesini bölerken kullanmak üzere tek bir *tabaka sütunu* seçin. Veri kümesindeki satırlar aşağıdaki gibi bölünür:

   1. Tüm giriş satırları, belirtilen tabaka sütunundaki değerlere göre gruplandırılır (tabakalaşmış).

   1. Satırlar her grup içinde karıştırılır.

   1. Her grup, belirtilen oranı karşılamak için çıktı veri kümesine seçici olarak eklenir.


1. Boru hattını gönderin.

   Bu seçenekle, modül verilerin temsili örneklemesini içeren tek bir veri kümesi ni verir. Veri kümesinin kalan, örneklenmemiş kısmı çıktı değildir. 

## <a name="split-data-into-partitions"></a>Verileri bölümlere bölme

Veri kümesini verilerin alt kümelerine bölmek istediğinizde bu seçeneği kullanın. Bu seçenek, çapraz doğrulama için özel bir kıvrım sayısı oluşturmak veya satırları birkaç gruba bölmek istediğinizde de yararlıdır.

1. Bölüm **ve Örnek** modülünü ardışık kurulumunuza ekleyin ve veri kümesini bağlayın.

1. **Bölüm veya örnek modu** **için, Katlara Atay'ı**seçin.

1. **Bölümlemede değiştirme yi kullanın**: Örneklenmiş satırın olası yeniden kullanım için satır havuzuna geri konmasını istiyorsanız bu seçeneği seçin. Sonuç olarak, aynı satır birkaç kıvrıma atanabilir.

   Değiştirme (varsayılan seçenek) kullanmıyorsanız, örneklenmiş satır olası yeniden kullanım için satır havuzuna geri konmaz. Sonuç olarak, her satır yalnızca bir kat atanır.

1. **Randomize bölme**: Satırların kıvrımlara rasgele atanmasını istiyorsanız bu seçeneği seçin.

   Bu seçeneği seçmezseniz, satırlar round-robin yöntemi yle katlanır atanır.

1. **Rastgele tohum**: İsteğe bağlı olarak, tohum değeri olarak kullanmak için bir insa yer. Satırların her seferinde aynı şekilde bölünmesini istiyorsanız, bu seçenek önemlidir. Aksi takdirde, **0** varsayılan değeri rasgele bir başlangıç tohumu kullanılacağı anlamına gelir.

1. **Bölümleyici yöntemini belirtin**: Bu seçenekleri kullanarak verilerin her bölüme nasıl tahsis edilebilmesini istediğinizi belirtin:

   - **Eşit bölüm**: Her bölüme eşit sayıda satır yerleştirmek için bu seçeneği kullanın. Çıktı bölümsayısını belirtmek için, kutuya eşit **olarak bölmek için kıvrım sayısını belirtin'e** bir tam sayı girin.

   - **Özelleştirilmiş oranlarda bölüm**: Her bölümün boyutunu virgülle ayrılmış bir liste olarak belirtmek için bu seçeneği kullanın.

     Örneğin, üç bölüm oluşturmak istediğinizi varsayalım. İlk bölüm verilerin yüzde 50'sini içerecektir. Kalan iki bölüm, verilerin yüzde 25'ini içerir. **Virgül kutusuyla ayrılmış oranlar listesine** şu sayıları girin: **.5, .25, 0,25**.

     Tüm bölüm boyutlarının toplamı tam olarak 1'e kadar eklenmelidir.

     *1'den az*birsayı yada sayılar girerseniz, kalan satırları tutmak için fazladan bir bölüm oluşturulur. Örneğin, **.2** ve **.3**değerlerini girerseniz, tüm satırların kalan yüzde 50'sini tutmak için üçüncü bir bölüm oluşturulur.
     
     *1'den fazla*toplayan sayılar girerseniz, ardışık hattı çalıştırdığınızda bir hata yükselir.

1. **Tabakalı bölme**: Satırların bölündüğünde tabakalaşmış olmasını istiyorsanız bu seçeneği seçin ve sonra _katman sütununu_seçin.

1. Boru hattını gönderin.

   Bu seçenekle, modül birden çok veri kümesi nden çıkar. Veri kümeleri belirttiğiniz kurallara göre bölümlere ayrılmıştır.

### <a name="use-data-from-a-predefined-partition"></a>Önceden tanımlanmış bir bölümdeki verileri kullanma  

Bir veri kümesini birden çok bölüme böldüyseniz ve şimdi daha fazla analiz veya işleme için her bölümü sırayla yüklemek istediğinizde bu seçeneği kullanın.

1. Bölme **ve Örnek** modüllerini boru hattına ekleyin.

1. Modülü, **Önceki Bölüm ve Örnek**örneğinin çıktısına bağlayın. Bu örnek, bazı sayıda bölüm oluşturmak **için Katlara Atama** seçeneğini kullanmış olmalıdır.

1. **Bölüm veya örnek modu**: **Seç Kat'ı**seçin.

1. **Hangi katTan örnek alıneceğini belirtin**: Dizinini girerek kullanılacak bir bölüm seçin. Bölüm dizinleri 1 tabanlıdır. Örneğin, veri kümesini üç bölüme bölerseniz, bölümlerin indeksleri 1, 2 ve 3 olurdu.

   Geçersiz bir dizin değeri girerseniz, bir tasarım zamanı hatası yükseltilir: "Hata 0018: Veri kümesi geçersiz veri içerir."

   Veri kümesini kıvrımlara göre gruplandırmanın yanı sıra, veri kümesini iki gruba ayırabilirsiniz: hedef kıvrım ve diğer her şey. Bunu yapmak için, tek bir kıvrım dizini girin ve sonra belirtilen kat veri hariç her şeyi almak için **seçili kat tamamlayıcıseçin.**

1. Birden çok bölümle çalışıyorsanız, her bölümü işlemek için **Bölüm ve Örnek** modülünün daha fazla örneğini eklemeniz gerekir.

   Örneğin, ikinci satırdaki **Bölüm ve Örnek** modülü **Kıvrımlara Atamak**üzere, üçüncü satırdaki modül ise **Kat Seç'e**ayarlanır.   

   ![Bölüm ve örnek](./media/module/partition-and-sample.png)

1. Boru hattını gönderin.

   Bu seçenekle, modül yalnızca bu kata atanan satırları içeren tek bir veri kümesinden çıkar.

> [!NOTE]
>  Kıvrım tanımlamalarını doğrudan görüntüleyebilirsiniz. Sadece meta verilerde bulunurlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 