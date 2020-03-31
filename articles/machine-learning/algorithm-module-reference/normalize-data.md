---
title: 'Verileri Normalleştir: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Bir veri kümesini *normalleştirme*yoluyla dönüştürmek için Azure Machine Learning'de Normalize Veri modüllerini nasıl kullanacağınızı öğrenin...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: a740c81aa165e221bca19987c7b3c62da56b0402
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477536"
---
# <a name="normalize-data-module"></a>Veri modüllerini normalleştirin

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

*Normalleştirme*yoluyla bir veri kümesini dönüştürmek için bu modülü kullanın.

Normalleştirme genellikle makine öğrenimi için veri hazırlama nın bir parçası olarak uygulanan bir tekniktir. Normalleştirmenin amacı, değerler aralıklarında farklılıkları bozmadan veya bilgi kaybetmeden, ortak bir ölçek kullanmak için veri kümesindeki sayısal sütunların değerlerini değiştirmektir. Bazı algoritmaların verileri doğru bir şekilde modellemesi için normalleştirme de gereklidir.

Örneğin, giriş veri setinizin 0 ile 1 arasında değişen değerlere sahip bir sütun ve 10.000 ile 100.000 arasında değişen değerlere sahip başka bir sütun içerdiğini varsayalım. Modelleme sırasında değerleri özellikler olarak birleştirmeyi denediğinizde sayıların *ölçeğindeki* büyük fark sorunlara neden olabilir.

*Normalleştirme,* kaynak verilerdeki genel dağılım ve oranları koruyan yeni değerler oluşturarak, değerleri modelde kullanılan tüm sayısal sütunlarda uygulanan bir ölçekte tutarak bu sorunları önler.

Bu modül sayısal verileri dönüştürmek için çeşitli seçenekler sunar:

- Tüm değerleri 0-1 ölçeğine değiştirebilir veya değerleri mutlak değerler yerine yüzdelik sıralar olarak temsil ederek dönüştürebilirsiniz.
- Normalleştirmeyi tek bir sütuna veya aynı veri kümesindeki birden çok sütuna uygulayabilirsiniz.
- Ardışık işlemi yinelemeniz veya aynı normalleştirme adımlarını diğer verilere uygulamanız gerekiyorsa, normalleştirme dönüşümü olarak adımları kaydedebilir ve aynı şemaya sahip diğer veri kümelerine uygulayabilirsiniz.

> [!WARNING]
> Bazı algoritmalar, bir modeli eğitmeden önce verilerin normalleştirilmesini gerektirir. Diğer algoritmalar kendi veri ölçekleme veya normalleştirme gerçekleştirin. Bu nedenle, tahmine dayalı bir model oluşturmak için bir makine öğrenme algoritması seçtiğinizde, eğitim verilerine normalleştirme uygulamadan önce algoritmanın veri gereksinimlerini gözden geçirdiğinizden emin olun.

##  <a name="configure-normalize-data"></a>Verileri Normalleştir'i Yapılandır

Bu modülü kullanarak aynı anda yalnızca bir normalleştirme yöntemi uygulayabilirsiniz. Bu nedenle, seçtiğiniz tüm sütunlara aynı normalleştirme yöntemi uygulanır. Farklı normalleştirme yöntemlerini kullanmak **için, Verileri Normalleştir'in**ikinci bir örneğini kullanın.

1. Veri hattınıza **Normalize Veri** modüllerini ekleyin. Modülü Azure Machine Learning'de, **Veri Dönüşümü**altında Ölçek **ve Azalt** kategorisinde bulabilirsiniz.

2. Tüm sayılardan en az bir sütun içeren bir veri kümesi bağlayın.

3. Normalleştirmek için sayısal sütunları seçmek için Sütun Seçici'yi kullanın. Tek tek sütunları seçmezseniz, varsayılan olarak girişteki **tüm** sayısal tür sütunları dahil edilir ve aynı normalleştirme işlemi seçili tüm sütunlara uygulanır. 

    Normalleştirilmemesi gereken sayısal sütunlar eklerseniz, bu durum garip sonuçlara yol açabilir! Sütunları her zaman dikkatlice kontrol edin.

    Sayısal sütun algılanmamışsa, sütunun veri türünün desteklenen sayısal bir tür olduğunu doğrulamak için sütun meta verilerini denetleyin.

    > [!TIP]
    > Belirli bir türdeki sütunların giriş olarak sağlandığından emin olmak için, **Verileri Normalleştirmeden**önce [Veri Kümesi modülündeki Sütunları Seç'i](./select-columns-in-dataset.md) kullanmayı deneyin.

4. **İşaretlendiğinde sabit sütunlar için 0'ı kullanın**: Sayısal sütuntek bir değişmeyen değer içeriyorsa bu seçeneği seçin. Bu, bu tür sütunların normalleştirme işlemlerinde kullanılmamasını sağlar.

5. Dönüşüm **yöntemi** açılır listesinden, seçili tüm sütunlara uygulamak için tek bir matematiksel işlev seçin. 
  
    - **Zscore**: Tüm değerleri z-skoruna dönüştürür.
    
      Sütundaki değerler aşağıdaki formül kullanılarak dönüştürülür:  
  
      ![z&#45;skorları kullanılarak normalleştirme](media/module/aml-normalization-z-score.png)
  
      Ortalama ve standart sapma her sütun için ayrı ayrı hesaplanır. Nüfus standardı sapması kullanılır.
  
    - **MinMax**: Min-max normalleştirici doğrusal [0,1] aralığına kadar her özelliği yeniden ölçekler.
    
      [0,1] aralığına yeniden ölçekleme, her özelliğin değerlerini en az değerin 0 olması için kaydırArak ve yeni maksimal değere bölünerek (orijinal maksimal ve en küçük değerler arasındaki fark) yapılır.
      
      Sütundaki değerler aşağıdaki formül kullanılarak dönüştürülür:  
  
      ![min&#45;max fonksiyonu kullanılarak normalleştirme](media/module/aml-normalization-minmax.png "AML_normalization-minmax")  
  
    - **Logistic**: Sütundaki değerler aşağıdaki formül kullanılarak dönüştürülür:

      ![lojistik fonksiyona göre normalleştirme formülü](media/module/aml-normalization-logistic.png "AML_normalization-lojistik")  
  
    - **LogNormal**: Bu seçenek tüm değerleri günlük normal ölçeğine dönüştürür.
  
      Sütundaki değerler aşağıdaki formül kullanılarak dönüştürülür:
  
      ![formül günlüğü normal dağılım&#45;](media/module/aml-normalization-lognormal.png "AML_normalization-lognormal")
    
      Burada μ ve σ, her sütun için ayrı ayrı maksimum olasılık tahminleri olarak verilerden ampirik olarak hesaplanan dağıtım parametreleridir.  
  
    - **TanH**: Tüm değerler hiperbolik teğete dönüştürülür.
    
      Sütundaki değerler aşağıdaki formül kullanılarak dönüştürülür:
    
      ![tanh fonksiyonunu kullanarak normalleştirme](media/module/aml-normalization-tanh.png "AML_normalization tanh")

6. Ardışık hattı gönderin veya **Normalize Veri** modülünü çift tıklatın ve **Seçili Çalıştır'ı**seçin. 

## <a name="results"></a>Sonuçlar

**Normalize Veri** modülü iki çıktı üretir:

- Dönüştürülen değerleri görüntülemek için modülü sağ tıklatın ve **Visualize'ı**seçin.

    Varsayılan olarak, değerler yerine dönüştürülür. Dönüştürülen değerleri özgün değerlerle karşılaştırmak istiyorsanız, veri kümelerini yeniden birleştirmek ve sütunları yan yana görüntülemek için [Sütun Ekle](./add-columns.md) modülünü kullanın.

- Aynı normalleştirme yöntemini başka bir veri kümesine uygulayabilmeniz için dönüşümü kaydetmek için modülü seçin ve doğru paneldeki **Çıktılar** sekmesi altında **Veri Kümesi'ni kaydet'i** seçin.

    Daha sonra kaydedilen **dönüşümleri** sol gezinti bölmesinin Dönüşümler grubundan yükleyebilir ve [Dönüşüm Uygula'yı](apply-transformation.md)kullanarak aynı şemaya sahip bir veri kümesine uygulayabilirsiniz.  


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 