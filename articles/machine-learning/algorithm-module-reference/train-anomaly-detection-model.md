---
title: 'Anomali algılama modeli eğitme: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Eğitimli anomali algılama modeli oluşturmak için anomali algılama modeli modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 78ed2d85729cce94e8dfa579545f558d2cfe4651
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79502977"
---
# <a name="train-anomaly-detection-model"></a>Anomali Algılama Modeli Eğitme

Bu makalede, eğitilen bir algılama modeli oluşturmak için Azure Machine Learning tasarımcısında (Önizleme) **anomali algılama modeli** modülünün nasıl kullanılacağı açıklanır.

Modül, anomali algılama modeli için bir model parametreleri kümesi ve etiketli olmayan bir veri kümesi olarak alır. Eğitim verileri için bir Etiketler kümesiyle birlikte eğitilen bir algılama modeli döndürür.  

Tasarımcıda sunulan anomali algılama algoritmaları hakkında daha fazla bilgi için şu konulara bakın: 

+ [PCA Tabanlı Anomali Algılama](pca-based-anomaly-detection.md)  

## <a name="how-to-configure-train-anomaly-detection-model"></a>Tren anomali algılama modelini yapılandırma 

1.  Tasarımcı 'daki işlem hattınızı **eğitme algılama modeli** modülünü ekleyin. Bu modülü **anomali algılama** kategorisinde bulabilirsiniz.

2. [PCA tabanlı anomali algılama](pca-based-anomaly-detection.md) gibi anomali algılama için tasarlanan modüllerden birini bağlayın

    Diğer model türleri desteklenmez; işlem hattını çalıştırırken şu hatayı alırsınız: tüm modeller aynı Learner türüne sahip olmalıdır.  

3.  Etiket sütununu seçerek ve algoritmaya özgü diğer parametreleri ayarlayarak anomali algılama modülünü yapılandırın.  

4.  **Anomali algılama modelini eğmek**için doğru girişe eğitim veri kümesi ekleyin.  

5.  İşlem hattını gönderme.  

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Modelin parametrelerini görüntülemek için modüle sağ tıklayın ve **Görselleştir**' i seçin. 

+ Tahmin oluşturmak için, [puan modelini](score-model.md) yeni giriş verileriyle kullanın.

+ Eğitilen modelin anlık görüntüsünü kaydetmek için modülünü seçin ve sağ paneldeki **çıktılar + Günlükler** sekmesinde yer alan **veri kümesini kaydet** simgesine tıklayın.   

 
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

Tasarımcı modüllerine özgü hataların listesi için bkz. [Tasarımcı Için özel durumlar ve hata kodları (Önizleme)](designer-error-codes.md) .
'