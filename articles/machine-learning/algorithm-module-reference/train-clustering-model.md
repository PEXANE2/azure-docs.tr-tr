---
title: 'Tren Kümeleme Modeli: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Kümeleme modellerini eğitmek için Azure Machine Learning'de Tren Kümeleme Modeli modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: c29baf3cdda998a2ab78c84f3311b84d37086bcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477434"
---
# <a name="train-clustering-model"></a>Kümeleme Modeli Eğitme

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Kümeleme modelini eğitmek için bu modülü kullanın.

Modül, [K-Means Kümeleme](k-means-clustering.md) modüllerini kullanarak zaten yapılandırdığınız eğitimsiz bir kümeleme modelini alır ve modeli etiketli veya etiketlenmemiş veri kümesi kullanarak eğitir. Modül, hem tahmin için kullanabileceğiniz eğitilmiş bir model hem de eğitim verilerindeki her servis talebi için küme atamaları kümesi oluşturur.

> [!NOTE]
> Bir kümeleme modeli, makine öğrenme modellerinin eğitimi için genel modül olan [Tren Modeli](train-model.md) modülü kullanılarak eğitilemez. Tren [Modeli](train-model.md) sadece denetimli öğrenme algoritmaları ile çalışır olmasıdır. K-araçları ve diğer kümeleme algoritmaları denetimsiz öğrenmesağlar, bu da algoritmanın etiketlenmemiş verilerden öğrenebileceği anlamına gelir.  
  
## <a name="how-to-use-train-clustering-model"></a>Tren Kümeleme Modeli nasıl kullanılır?  

1.  Tasarımcıda boru hattınıza **Tren Kümeleme Modeli** modüllerini ekleyin. Modülü **Makine Öğrenimi Modülleri**altında, **Tren** kategorisinde bulabilirsiniz.  
  
2. [K-Means Kümeleme](k-means-clustering.md) modülünün veya uyumlu bir kümeleme modeli oluşturan başka bir özel modülü ekleyin ve kümeleme modelinin parametrelerini ayarlayın.  
    
3.  **Tren Kümeleme Modeli'nin**sağ girişine bir eğitim veri kümesi takın.
  
5.  **Sütun**Kümesi'nde, yapı kümelerinde kullanmak üzere veri kümesinden sütunları seçin. İyi özellikler oluşturan sütunları seçtiğinizden emin olun: örneğin, özgün değerlere sahip diğer disvediğer sütunları veya aynı değerlere sahip sütunları kullanmaktan kaçının.

    Bir etiket varsa, bunu özellik olarak kullanabilir veya dışarıda bırakabilirsiniz.  
  
6. Yeni küme etiketiyle birlikte eğitim verilerini çıkarmak istiyorsanız, yalnızca **ek ineve onayını veya yalnızca sonucu onayını alın**seçeneğini belirleyin.

    Bu seçeneği n için seçerseniz, yalnızca küme atamaları çıktıdır. 

7. Ardışık hattı gönderin veya **Tren Kümeleme Modeli** modülünü tıklatın ve **Seçili Çalıştır'ı**seçin.  
  
### <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek **için, Tren modeli** modülünün sağ panelindeki **Çıktılar** sekmesini seçin. Modeli yeniden kullanılabilir bir modül olarak kaydetmek için **Kayıt veri kümesi** simgesini seçin.

+ Modelden puan lar oluşturmak [için, Kümelere Veri Atay'ı](assign-data-to-clusters.md)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 