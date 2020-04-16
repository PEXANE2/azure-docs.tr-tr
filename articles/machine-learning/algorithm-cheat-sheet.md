---
title: Makine Öğrenme Algoritması Hile Sayfası - tasarımcı
titleSuffix: Azure Machine Learning
description: Yazdırılabilir Makine Öğrenme Algoritması Hile Sayfası, Azure Machine Learning tasarımcısında tahmine dayalı modeliniz için doğru algoritmayı seçmenize yardımcı olur.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: FrancescaLazzeri
ms.author: lazzeri
ms.date: 03/05/2020
ms.openlocfilehash: accc4766f25f5d58ba2f31c153fe1c5d23413170
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399076"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısı için Makine Öğrenimi Algoritması Hile Sayfası

**Azure Machine Learning Algoritma hile sayfası,** tahmine dayalı bir analitik modeli için tasarımcıdan doğru algoritmayı seçmenize yardımcı olur.

Azure Machine Learning ***sınıflandırma,*** ***tavsiye sistemleri,*** ***kümeleme,*** ***anomali algılama,*** ***gerileme***ve metin ***analizi*** ailelerinden algoritmalardan oluşan geniş bir kütüphaneye sahiptir. Her biri farklı türde makine öğrenimi sorununu ele almak üzere tasarlanmıştır.

Ek kılavuz için [algoritmaları nasıl seçiler](how-to-select-algorithms.md)

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Indirin: Makine Öğrenme Algoritması Hile Sayfası

**Burada hile sayfası indirin: [Makine Öğrenme Algoritması Hile Sayfası (11x17 in.)](https://download.microsoft.com/download/3/5/b/35bb997f-a8c7-485d-8c56-19444dafd757/azure-machine-learning-algorithm-cheat-sheet-nov2019.pdf?WT.mc_id=docs-article-lazzeri)**

![Makine Öğrenme Algoritması Hile Sayfası: Makine Öğrenme algoritması nasıl seçeceğinizi öğrenin.](./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet.svg)

Makine Öğrenimi Algoritması Hile Sayfasını kullanışlı tutmak ve algoritma seçiminde yardım almak için tabloid boyutta indirin ve yazdırın.

## <a name="how-to-use-the-machine-learning-algorithm-cheat-sheet"></a>Makine Öğrenme Algoritması Hile Sayfası nasıl kullanılır

Bu algoritma hile sayfasında sunulan öneriler yaklaşık kurallar-of-thumb vardır. Bazıları bükülebilir, bazıları da açıkça ihlal edilebilir. Bu hile sayfası bir başlangıç noktası önermek için tasarlanmıştır. Verilerinizdeki çeşitli algoritmalar arasında kafa kafaya rekabet yapmaktan korkmayın. Her algoritmanın ilkelerini ve verilerinizi oluşturan sistemin ilkelerini anlamanın hiçbir yerini tutamaz.

Her makine öğrenme algoritması kendi tarzı veya endüktif önyargı vardır. Belirli bir sorun için, birkaç algoritma uygun olabilir ve bir algoritma diğerlerinden daha uygun olabilir. Ama her zaman en uygun olduğunu önceden bilmek mümkün değildir. Bu gibi durumlarda, birkaç algoritma birlikte hile sayfasında listelenir. Uygun bir strateji bir algoritma denemek olacaktır ve sonuçlar henüz tatmin edici değilse, diğerlerini deneyin. 

Azure Machine Learning tasarımcısındaki algoritmalar hakkında daha fazla bilgi edinmek için [Algoritma ve modül başvurusuna](algorithm-module-reference/module-reference.md)gidin.

## <a name="kinds-of-machine-learning"></a>Makine öğrenimi türleri

Makine öğreniminin üç ana kategorisi vardır: *denetimli öğrenme,* *denetimsiz öğrenme*ve *pekiştirme öğrenme*.

### <a name="supervised-learning"></a>Denetimli öğrenme

Denetimli öğrenmede, her veri noktası bir kategori veya ilgi alanı değeriyle etiketlenir veya ilişkilendirilir. Kategorik bir etikete örnek olarak bir görüntü 'kedi' veya 'köpek' olarak atamaktır. Değer etiketine örnek olarak, ikinci el araçla ilişkili satış fiyatı dır. Denetimli öğrenmenin amacı, bu gibi birçok etiketli örneği incelemek ve daha sonra gelecekteki veri noktaları hakkında tahminlerde bulunabilmektir. Örneğin, yeni fotoğrafları doğru hayvanla tanımlamak veya diğer kullanılmış araçlara doğru satış fiyatları atamak. Bu makine öğrenme popüler ve yararlı bir türüdür.

### <a name="unsupervised-learning"></a>Denetimsiz öğrenme

Denetimsiz öğrenmede, veri noktalarının bunlarla ilişkili etiketi yoktur. Bunun yerine, denetimsiz bir öğrenme algoritmasının amacı, verileri bir şekilde düzenlemek veya yapısını açıklamaktır. Denetimsiz öğrenme grupları verileri, K-'nin yaptığı gibi kümelere ayırır veya karmaşık verilere daha basit görünmesi için farklı bakış biçimleri bulur.

### <a name="reinforcement-learning"></a>Pekiştirmeye dayalı öğrenme

Pekiştirme öğrenmede, algoritma her veri noktasına yanıt olarak bir eylem seçer. Bu robotik, zaman içinde bir noktada sensör okumaları kümesi bir veri noktası olduğu ortak bir yaklaşımdır, ve algoritma robotun bir sonraki eylem seçmek gerekir. Ayrıca Nesnelerin İnterneti uygulamaları için de doğal bir uyum. Öğrenme algoritması da kısa bir süre sonra, kararın ne kadar iyi olduğunu gösteren bir ödül sinyali alır. Bu sinyale dayanarak, algoritma en yüksek ödülü elde etmek için stratejisini değiştirir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Algoritmaların nasıl seçilenhakkında](how-to-select-algorithms.md) ek kılavuza bakın

* [Azure Machine Learning ve Azure portalındaki stüdyo hakkında bilgi edinin.](overview-what-is-azure-ml.md)

* [Öğretici: Azure Machine Learning tasarımcısında bir tahmin modeli oluşturun.](tutorial-designer-automobile-price-train-score.md)

* [Makine öğrenimi hakkında derin öğrenme ve makine öğrenimi hakkında bilgi edinin.](concept-deep-learning-vs-machine-learning.md)
