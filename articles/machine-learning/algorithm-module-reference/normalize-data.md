---
title: 'Verileri Normalleştir: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir veri kümesini *normalleştirme*yoluyla dönüştürmek için Azure Machine Learning hizmetinde Normalleştir modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 504224ae586e18fc5bf9294b537e730da37a2423
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128560"
---
# <a name="normalize-data-module"></a>Veri modülünü Normalleştir

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bir veri kümesini *normalleştirme*yoluyla dönüştürmek için bu modülü kullanın.

Normalleştirme, makine öğrenimine yönelik veri hazırlama kapsamında genellikle uygulanan bir tekniktir. Normalleştirme hedefi, veri kümesindeki sayısal sütunların değerlerini, değerlerin aralıklarına ilişkin farklılıkları deforme etmeden veya bilgi kaybına gerek kalmadan, ortak bir ölçek kullanacak şekilde değiştirmektir. Ayrıca, bazı algoritmaların verileri doğru şekilde modelmesi için normalleştirme de gereklidir.

Örneğin, giriş veri kümenizin, 0 ile 1 arasında ve 10.000 ile 100.000 arasında değişen değerler içeren bir sütun içerdiğini varsayalım. Sayıları, modelleme sırasında özellikler olarak birleştirmeye çalıştığınızda, sayıların *ölçeğinde* harika bir farklılık sorunlara yol açabilir.

*Normalleştirme* , kaynak verilerdeki genel dağıtım ve oranları koruyan yeni değerler oluşturarak bu sorunları ortadan kaldırarak, değerleri modelde kullanılan tüm sayısal sütunlarda uygulanan bir ölçekte tutar.

Bu modül, sayısal verileri dönüştürmek için çeşitli seçenekler sunar:

- Tüm değerleri bir 0-1 ölçeğinde değiştirebilir veya değerleri mutlak değerler yerine yüzdebirlik dereceleri olarak temsil ederek dönüştürebilirsiniz.
- Tek bir sütuna veya aynı veri kümesindeki birden çok sütuna normalleştirme uygulayabilirsiniz.
- Denemeyi tekrarlamanız veya diğer verilere aynı normalleştirme adımlarını uygulamanız gerekiyorsa, adımları bir normalleştirme dönüştürmesi olarak kaydedebilir ve aynı şemaya sahip diğer veri kümelerine uygulayabilirsiniz.

> [!WARNING]
> Bazı algoritmalar, bir modeli eğitmek için verilerin normalleştirilmesini gerektirir. Diğer algoritmalar kendi veri ölçeklendirmesini veya normalleştirmesini gerçekleştirir. Bu nedenle, bir tahmine dayalı model oluştururken kullanmak üzere bir makine öğrenimi algoritması seçtiğinizde, eğitim verilerine normalleştirme uygulamadan önce algoritmanın veri gereksinimlerini gözden geçirdiğinizden emin olun.

##  <a name="configure-normalize-data"></a>Normalleştir verileri yapılandırma

Bu modülü kullanarak tek seferde yalnızca bir normalleştirme yöntemi uygulayabilirsiniz. Bu nedenle, seçtiğiniz tüm sütunlara aynı normalleştirme yöntemi uygulanır. Farklı normalleştirme yöntemleri kullanmak için, **verileri normalleştirmek**için ikinci bir örnek kullanın.

1. Denemeniz için **Normalveri** modülünü ekleyin. Modülü, **veri dönüştürme**altında, **ölçek ve azalt** kategorisinde Azure Machine Learning bulabilirsiniz.

2. Tüm sayıların en az bir sütununu içeren bir veri kümesini bağlayın.

3. Normalleştirerek sayısal sütunları seçmek için sütun seçiciyi kullanın. Sütunları tek tek seçmezseniz, varsayılan olarak, girişte bulunan **Tüm** sayısal tür sütunları dahil edilir ve tüm seçili sütunlara aynı normalleştirme işlemi uygulanır. 

    Bu, normalleştirilmemelidir sayısal sütunlar dahil ederseniz garip sonuçlara yol açabilir! Sütunları her zaman dikkatle denetleyin.

    Sayısal sütun algılanmazsa, sütunun veri türünün desteklenen bir sayısal tür olduğunu doğrulamak için sütun meta verilerini denetleyin.

    > [!TIP]
    > Belirli bir türdeki sütunların giriş olarak sağlandığından emin olmak için, **verileri normalleştirmek**Için veri [kümesinde sütunları seç](./select-columns-in-dataset.md) modülünü kullanmayı deneyin.

4. **İşaretlendiğinde sabit sütunlar için 0 kullanın**:  Herhangi bir sayısal sütun, tek bir değişmeyen değer içerdiğinde bu seçeneği belirleyin. Bu, bu tür sütunların normalleştirme işlemlerinde kullanılmamasını sağlar.

5. **Dönüştürme yöntemi** açılan listesinden, seçili tüm sütunlara uygulamak için tek bir matematik işlevi seçin. 
  
    - **Zscore**: Tüm değerleri z puanına dönüştürür.
    
      Sütunundaki değerler aşağıdaki formül kullanılarak dönüştürülür:  
  
      ![z&#45;puanlarını kullanarak normalleştirme](media/module/aml-normalization-z-score.png)
  
      Ortalama ve standart sapma her bir sütun için ayrı olarak hesaplanır. Popülasyon standart sapması kullanılır.
  
    - **MinMax**: Minimum-Max normalleştirici her özelliği [0, 1] aralığına göre yeniden ölçeklendirin.
    
      [0, 1] aralığına yeniden boyutlandırılması, en az değer 0 olacak şekilde her bir özelliğin değerlerini değiştirerek ve sonra yeni maxhayvan değeri (özgün maxhayvan ve en düşük değerler arasındaki fark) ile bölünerek yapılır.
      
      Sütunundaki değerler aşağıdaki formül kullanılarak dönüştürülür:  
  
      ![Min&#45;Max işlevi kullanılarak normalleştirme](media/module/aml-normalization-minmax.png "AML_normalization-MinMax")  
  
    - **Lojistik**: Sütunundaki değerler aşağıdaki formül kullanılarak dönüştürülür:

      ![lojistik işlevi tarafından normalleştirilmesi için formül](media/module/aml-normalization-logistic.png "AML_normalization-lojistik")  
  
    - **LogNormal**: Bu seçenek tüm değerleri bir lognormal ölçeğe dönüştürür.
  
      Sütunundaki değerler aşağıdaki formül kullanılarak dönüştürülür:
  
      ![Formül günlüğü&#45;normal dağıtım](media/module/aml-normalization-lognormal.png "AML_normalization-lognormal")
    
      Burada μ ve σ, her bir sütun için, verilerden en yüksek olasılık tahminleri olarak hesaplanan, dağıtım parametreleridir.  
  
    - **Tanh**: Tüm değerler hiperbolik tanjanta dönüştürülür.
    
      Sütunundaki değerler aşağıdaki formül kullanılarak dönüştürülür:
    
      ![tanh işlevini kullanarak normalleştirme](media/module/aml-normalization-tanh.png "AML_normalization-tanh")

6. Denemeyi çalıştırın veya **Normalleştir veri** modülünü çift tıklayın ve **Seçileni Çalıştır**' ı seçin. 

## <a name="results"></a>Sonuçlar

**Normalize veri** modülü iki çıkış üretir:

- Dönüştürülen değerleri görüntülemek için modüle sağ tıklayın, **dönüştürülmüş veri kümesi**' ni seçin ve **Görselleştir**' e tıklayın.

    Varsayılan olarak, değerler yerinde dönüştürülür. Dönüştürülmüş değerleri özgün değerlerle karşılaştırmak istiyorsanız, veri kümelerini yeniden birleştirmek ve sütunları yan yana görüntülemek için [sütun Ekle](./add-columns.md) modülünü kullanın.

- Benzer bir veri kümesine aynı normalleştirme yöntemini uygulayabilmeniz için dönüştürmeyi kaydetmek üzere, modüle sağ tıklayın, **dönüştürme işlevi**' ni seçin ve **dönüştürme olarak kaydet**' e tıklayın.

    Ardından, sol gezinti bölmesinin **dönüşümler** grubundan kaydedilen dönüştürmeleri yükleyebilir ve [./Apply dönüşümünü](apply-transformation.md)kullanarak aynı şemaya sahip bir veri kümesine uygulayabilirsiniz.  


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 