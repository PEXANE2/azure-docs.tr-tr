---
title: 'Tren Anomali Algılama Modeli: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Eğitimli bir anomali algılama modeli oluşturmak için Tren Anomali Algılama Modeli modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 78ed2d85729cce94e8dfa579545f558d2cfe4651
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502977"
---
# <a name="train-anomaly-detection-model"></a>Tren Anomali Algılama Modeli

Bu makalede, eğitilmiş bir anormallik algılama modeli oluşturmak için Azure Machine Learning tasarımcısındaki (önizleme) **Tren Anomali Algılama Modeli** modülünün nasıl kullanılacağı açıklanmaktadır.

Modül, anomali algılama modeli ve etiketsiz veri kümesi için bir dizi model parametresini girdi olarak alır. Eğitim verileri için bir dizi etiketle birlikte eğitimli bir anomali algılama modelini döndürür.  

Tasarımcıda sağlanan anomali algılama algoritmaları hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

+ [PCA Tabanlı Anomali Algılama](pca-based-anomaly-detection.md)  

## <a name="how-to-configure-train-anomaly-detection-model"></a>Tren Anomali Algılama Modeli nasıl yapılandırılır? 

1.  Tren **Anomali Algılama Modeli** modüllerini tasarımcıda boru hattınıza ekleyin. Bu modülü **Anomali Algılama** kategorisinde bulabilirsiniz.

2. [PCA Tabanlı Anomali Algılama](pca-based-anomaly-detection.md) gibi anomali tespiti için tasarlanmış modüllerden birini bağlayın

    Diğer model türleri desteklenmez; ardışık hattı çalıştıran hata alırsınız: Tüm modeller aynı öğrenci türüne sahip olmalıdır.  

3.  Etiket sütununu seçerek ve algoritmaya özgü diğer parametreleri ayarlayarak anomali algılama modüllerini yapılandırın.  

4.  **Tren Anomali Algılama Modeli'nin**sağ girişine bir eğitim veri seti takın.  

5.  Boru hattını gönderin.  

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Modelin parametrelerini görüntülemek için modülü sağ tıklatın ve **Visualize'ı**seçin. 

+ Öngörüler oluşturmak için Yeni giriş verileriyle [Puan Modeli'ni](score-model.md) kullanın.

+ Eğitilen modelin anlık görüntüsünü kaydetmek için modülü seçin ve sağ paneldeki **Çıktılar+günlükler** sekmesinin altındaki **Kayıt veri kümesi** simgesine tıklayın.   

 
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 

Tasarımcı modüllerine özgü hataların listesi [için tasarımcının özel durumları ve hata kodlarına bakın (önizleme).](designer-error-codes.md)
'