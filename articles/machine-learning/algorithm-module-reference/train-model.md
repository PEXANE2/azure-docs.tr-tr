---
title: 'Tren Modeli: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Bir sınıflandırma veya regresyon modeli eğitmek için Azure Machine Learning'de **Tren Modeli** modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 0a9728e05aee27e74054a77e2c9be7dc08968207
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455868"
---
# <a name="train-model-module"></a>Tren Modeli modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Bir sınıflandırma veya regresyon modeli eğitmek için bu modülü kullanın. Eğitim, bir modeli tanımladıktan ve parametrelerini ayarladıktan sonra gerçekleşir ve etiketli verileri gerektirir. Mevcut bir modeli yeni verilerle yeniden eğitmek için **Train Model'i** de kullanabilirsiniz. 

## <a name="how-the-training-process-works"></a>Eğitim süreci nasıl çalışır?

Azure Machine Learning'de, makine öğrenimi modeli oluşturmak ve kullanmak genellikle üç adımlı bir işlemdir. 

1. Belirli bir algoritma türünü seçerek ve parametrelerini veya hiperparametrelerini tanımlayarak bir modeli yapılandırAbilirsiniz. Aşağıdaki model türlerinden birini seçin: 

    + Nöral ağlara, karar ağaçlarına, karar ormanlarına ve diğer algoritmalara dayalı **sınıflandırma** modelleri.
    + Standart doğrusal regresyon içerebilen veya nöral ağlar ve Bayesrereresyon da dahil olmak üzere diğer algoritmaları kullanan **regresyon** modelleri.  

2. Etiketli ve algoritmaile uyumlu veri kümesi sağlayın. Hem verileri hem de modeli **Train Model'e**bağlayın.

    Ne eğitim üretir belirli bir ikili biçimidir, iLearner, bu verilerden öğrenilen istatistiksel desenleri kapsüller. Bu biçimi doğrudan değiştiremez veya okuyamazsınız; ancak, diğer modüller bu eğitilmiş modeli kullanabilirsiniz. 
    
    Modelin özelliklerini de görüntüleyebilirsiniz. Daha fazla bilgi için Sonuçlar bölümüne bakın.

3. Eğitim tamamlandıktan sonra, yeni veriler üzerinde tahminlerde bulunmak [için, puanlama modüllerinden](./score-model.md)biri ile eğitimli modeli kullanın.

## <a name="how-to-use-train-model"></a>Tren Modeli nasıl kullanılır? 
  
1.  Azure Machine Learning'de bir sınıflandırma modeli veya regresyon modeli yapılandırın.
    
2. Tren **Modeli** modüllerini boru hattına ekleyin.  Bu modülü **Machine Learning** kategorisi altında bulabilirsiniz. **Tren'i**genişletin ve **tren modeli** modüllerini boru hattınıza sürükleyin.
  
3.  Sol girişte, eğitimsiz modu takın. Eğitim veri kümesini **Tren Modeli'nin**sağ girişine takın.

    Eğitim veri kümesi bir etiket sütunu içermelidir. Etiketleri olmayan satırlar yoksayılır.
  
4.  **Etiket sütunu**için, modülün sağ panelinde **sütunu düzenledive** modelin eğitim için kullanabileceği sonuçları içeren tek bir sütun seçin.
  
    - Sınıflandırma sorunları için etiket sütunu **kategorik** değerler veya **ayrı değerler** içermelidir. Bazı örnekler evet/hayır derecelendirmesi, hastalık sınıflandırma kodu veya adı veya gelir grubu olabilir.  Kategorik olmayan bir sütun seçerseniz, modül eğitim sırasında bir hata döndürür.
  
    -   Regresyon sorunları için etiket sütununda yanıt değişkenini temsil eden **sayısal** veriler bulunmalıdır. İdeal olarak sayısal veriler sürekli bir ölçeği temsil eder. 
    
    Örnekler bir kredi risk puanı, sabit disk için başarısız olmak için öngörülen süre veya belirli bir gün veya saatte bir çağrı merkezine yapılan tahmini çağrı sayısı olabilir.  Sayısal bir sütun seçmezseniz, bir hata alabilirsiniz.
  
    -   Hangi etiket sütununun kullanılacağını belirtmezseniz, Azure Machine Learning veri kümesinin meta verilerini kullanarak uygun etiket sütunu olduğu çıkarmasını sağlamaya çalışır. Yanlış sütunu seçerse, düzeltmek için sütun seçiciyi kullanın.
  
    > [!TIP] 
    > Sütun Seçici'yi kullanmakta sorun yaşıyorsanız, ipuçları için [Veri Kümesi'ndeki Sütunları Seç](./select-columns-in-dataset.md) makalesine bakın. **Bu kurallar** ve **BY NAME** seçeneklerini kullanmak için bazı yaygın senaryolar ve ipuçları açıklar.
  
5.  Boru hattını gönderin. Çok fazla veriniz varsa, bu işlem biraz zaman alabilir.

## <a name="results"></a><a name="bkmk_results"></a>Sonuç -ları

Model eğitildikten sonra:


+ Modeli diğer ardışık alanlarda kullanmak için modülü seçin ve sağ paneldeki **Çıktılar** sekmesinin altındaki **Kayıt veri kümesi** simgesini seçin. Veri Setleri altında modül paletinde kayıtlı **modellere**erişebilirsiniz.

+ Modeli yeni değerleri tahmin ederken kullanmak için, yeni giriş verileriyle birlikte [Puan Modeli](./score-model.md) modülüne bağlayın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 