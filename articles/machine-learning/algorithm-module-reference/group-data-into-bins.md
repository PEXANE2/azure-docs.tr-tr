---
title: Verileri depo gözlerine gruplandırma
titleSuffix: Azure Machine Learning
description: Numaraları gruplandırmak veya sürekli verilerin dağıtımını değiştirmek için verileri depo gözleri ' nde nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: b33aa3d2ab22fc986100d6141dd03d5547ef1862
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853830"
---
# <a name="group-data-into-bins"></a>Verileri depo gözlerine gruplandırma

Bu makalede, sayıları gruplamak veya sürekli verilerin dağıtımını değiştirmek için Azure Machine Learning tasarımcısında (Önizleme) bulunan [veri](group-data-into-bins.md) kümesi modülünün nasıl kullanılacağı açıklanır.

[Verileri depo gözleri halinde gruplandırma](group-data-into-bins.md) modülü, verileri binme için birden çok seçeneği destekler. Bölme kenarlarının nasıl ayarlanacağını ve değerlerin depo gözlerine nasıl alınacağını özelleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:  

+ Küme sınırları olarak kullanılacak bir değer serisini el ile yazın.  
+ *Quantiles*veya yüzdelik dereceleri kullanarak depo gözlerine değerler atayın.  
+ Değerlerin eşit bir şekilde dağıtılmasını zorlamak için zorlayın.  

### <a name="more-about-binning-and-grouping"></a>Atma ve gruplandırma hakkında daha fazla bilgi

Veri *hazırlama veya gruplama* (bazen de *quantilama*olarak adlandırılır), Machine Learning için sayısal veri hazırlarken önemli bir araçtır ve bunlar gibi senaryolarda faydalıdır:

+ Sürekli sayıların bir sütununda, etkili bir şekilde model oluşturmak için çok sayıda benzersiz değer bulunur, bu nedenle değerleri otomatik olarak veya el ile gruplara atayarak, daha küçük bir ayrık aralıklar kümesi oluşturabilirsiniz.

+ Bir sayı sütununu, belirli aralıkları temsil eden kategorik değerlerle değiştirin.

    Örneğin, Kullanıcı demograflarındaki 1-15, 16-22, 23-30 gibi özel aralıklar belirterek bir yaş sütunundaki değerleri gruplandırmak isteyebilirsiniz.

+ Bir veri kümesi, beklenen aralığın dışında kalan çok sayıda değere sahiptir ve bu değerler eğitilen modelde bir üst düzeyde etkisi vardır. Modeldeki farkı azaltmak için, quantiles yöntemini kullanarak verileri Tekdüzen dağıtımına dönüştürebilirsiniz.

    Bu yöntemde, [verileri depo gözleri olarak Gruplandır](group-data-into-bins.md) modülü, her bir sepete yaklaşık olarak aynı sayıda örnek bulunduğundan emin olmak için ideal bölme konumları ve bölme genişlikleri belirler. Ardından, seçtiğiniz normalleştirme yöntemine bağlı olarak, depo gözlerinde yer alan değerler yüzdebirlik değeri olarak dönüştürülür veya bir bin numarası ile eşleştirilir.

### <a name="examples-of-binning"></a>Bina örnekleri

Aşağıdaki diyagramda, **quantiles** yöntemiyle bir önce ve sonra sayısal değerlerin dağılımı gösterilmektedir. Solda ham verilerle karşılaştırıldığında verilerin oluşturulmuş ve birim normal ölçeğe dönüştürüldüğünü fark edersiniz.  

' Bu işlem hattı çalıştırmasının sonuçlarından bir örnek bulunabilir: https://ml.azure.com/visualinterface/authoring/Normal/87270db9-4651-448e-bd28-8ef7428084dc?wsid=%2Fsubscriptions%2Fe9b2ec51-5c94-4fa8-809a-dc1e695e4896%2Fresourcegroups%2Fmodule-ws-rg%2Fworkspaces%2Fmodule-prerelease-119&flight=cm&tid=72f988bf-86f1-41af-91ab-2d7cd011db47&smtendpoint=https%3A%2F%2Fsmt-test1.azureml-test.net '

Verileri gruplamak için pek çok yol olduğundan, tüm özelleştirilebilir, farklı yöntemler ve değerlerle denemeler yapmanızı öneririz. 

## <a name="how-to-configure-group-data-into-bins"></a>Grup verilerini depo gözlerine yapılandırma

1. **Grup verilerini** , tasarımcı 'daki işlem hattınızı (Önizleme) işlem hattı modülüne ekleyin. Bu modülü kategori **veri dönüşümünde**bulabilirsiniz.

2. Sayısal verileri olan veri kümesini bin 'e bağlayın.  Quantidileştirme yalnızca sayısal verileri içeren sütunlara uygulanabilir. 

    Veri kümesi sayısal olmayan sütunlar içeriyorsa, birlikte çalışacak sütunların bir alt kümesini seçmek için [veri kümesinde sütunları seç](select-columns-in-dataset.md) modülünü kullanın.

3. Binme modunu belirtin. Binme modu diğer parametreleri belirler, bu nedenle ilk olarak **binme modu** seçeneğini seçtiğinizden emin olun. Aşağıdaki tür bir atma desteklenir:

    **Quantiles**: quantile yöntemi, yüzdelik derecelendirmeler temelinde depo gözlerine değerler atar. Quantiles, eşit yükseklik bini olarak da bilinir.

    **Eşit genişlik**: Bu seçenekle, toplam bölme sayısını belirtmeniz gerekir. Veri sütunundaki değerler, her bir bin başlangıç ve bitiş değerleri arasında aynı aralığa sahip olduğu zaman gözlerine yerleştirilir. Sonuç olarak, veriler belirli bir nokta etrafında kümelendirilebilir bazı depo gözleri daha fazla değere sahip olabilir.

    **Özel kenarlar**: her bir bin başlangıç değerlerini belirtebilirsiniz. Kenar değeri her zaman bin alt sınırıdır.  Örneğin, biri 0 ' dan büyük değerler içeren ve biri 0 ' dan küçük veya buna eşit olan değerleri iki bölme halinde gruplandırmak istediğinizi varsayalım. Bu durumda, bin kenarları için, **küme kenarlarının virgülle ayrılmış listesine**0 yazın. Modülün çıktısı 1 ve 2 olur ve her satır değeri için bin dizinini gösterir. Lütfen virgülle ayrılmış değer listesinin, 1, 3, 5, 7 gibi artan bir düzende olması gerektiğini unutmayın.

4. **Bölme sayısı**: **quantiles**ve **eşittir genişlik** ayarı modlarını *kullanıyorsanız, oluşturmak*istediğiniz kaç bin veya ölçü olduğunu belirtmek için bu seçeneği kullanın.

5. **Sütunlarda**yer alan sütunları seçmek Için, sütun seçiciyi kullanarak, depo gözüne istediğiniz değerlere sahip sütunları seçin. Sütunlar sayısal bir veri türü olmalıdır.

    Aynı binkenme kuralı, seçtiğiniz tüm ilgili sütunlara uygulanır. Bu nedenle, bazı sütunları farklı bir yöntem kullanarak Sepetinizde, her bir sütun kümesi için ayrı bir [Grup verileri örneğini depo gözlerinde](group-data-into-bins.md) kullanın.

    > [!WARNING]
    > İzin verilmeyen bir tür olmayan bir sütun seçerseniz, bir çalışma zamanı hatası oluşturulur. Modül, izin verilmeyen bir türdeki herhangi bir sütunu bulur oluşturmaz bir hata döndürür. Bir hata alırsanız, tüm seçili sütunları gözden geçirin. Hata, tüm geçersiz sütunları listelemez.

6. **Çıkış modu**için, quantideðerlendirilmiş değerlerin nasıl çıkışını istediğinizi belirtin.

      + **Append**: oluşturulan değerlerle yeni bir sütun oluşturur ve bunu giriş tablosuna ekler.

      + **InPlace**: özgün değerleri veri kümesindeki yeni değerlerle değiştirir.

      + **Resultonly**: yalnızca sonuç sütunlarını döndürür.

7. **Quantiles** 'in modunu seçerseniz, değerlerin, quantiles 'e sıralamadan önce nasıl normalleştirilme olduğunu belirlemek Için, **quantile normalleştirme** seçeneğini kullanın. Değerlerin normalleştirilmesi değerlerin dönüştürdüğüne, ancak son bölme sayısını etkilemediğini unutmayın.

    Aşağıdaki normalleştirme türleri desteklenir:

    + **Yüzde**: değerler Aralık içinde normalleştirillerdir [0100]

    + **Pquantile**: değerler Aralık içinde normalleştirilir [0, 1]

    + **Quantitaındex**: değerler [1, bölme sayısı] aralığında normalleştirilir

8. **Özel kenarlar** seçeneğini belirlerseniz, **bin kenarları metin kutusunun virgülle ayrılmış listesinde** *küme kenarları* olarak kullanılacak sayıların virgülle ayrılmış bir listesini yazın.  Değerler, depo gözlerini bölen noktayı işaret eder, bu nedenle bir bin kenar değeri yazarsanız iki bölme oluşturulur; iki bin kenar değeri yazarsanız, üç bölme oluşturulacaktır ve bu şekilde devam eder.

    Değerlerin, en küçükten en büyüğe doğru şekilde oluşturulması için sıralanması gerekir.

10. **Sütunları kategorik olarak etiketle**: Bu seçeneği, quantidiklen sütunların kategorik değişkenler olarak işleneceğini belirtmek için seçin.

11. İşlem hattını gönderme.

### <a name="results"></a>Sonuçlar

[Gruplandırma verileri](group-data-into-bins.md) , her bir öğenin belirtilen moda göre oluşturulduğu bir veri kümesini döndürür. 

Aynı zamanda, aynı binme modunu ve parametrelerini kullanarak yeni veri örneklerini bölmeye dönüştürmek için [dönüştürme](apply-transformation.md) modülüne geçirilebilecek bir işlev olan bir **binme dönüştürmesi**de döndürür.  

> [!TIP]
> Eğitim verilerinizde bir binüyi kullanıyorsanız, test ve tahmin için kullandığınız verilerde aynı binkenme yöntemini kullanmanız gerektiğini unutmayın. Bu, binme yöntemi, bin konumları ve bin genişliklerini içerir. 
> 
> Verilerin her zaman aynı binme yöntemi kullanılarak dönüştürülmesini sağlamak için, yararlı veri dönüştürmelerini kaydetmenizi ve ardından [dönüştürme modülünü Uygula](apply-transformation.md) ' yı kullanarak bunları diğer veri kümelerine uygulamanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
