---
title: Puan resmi modeli
titleSuffix: Azure Machine Learning
description: Eğitilen bir görüntü modeli kullanarak tahmin oluşturmak için Azure Machine Learning görüntü modeli modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 021572aef673cf88f7744a0a210ef794c739448b
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450720"
---
# <a name="score-image-model"></a>Puan resmi modeli

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Bu modülü, giriş görüntü verilerinde eğitilen bir görüntü modeli kullanarak tahmin oluşturmak için kullanın.

## <a name="how-to-configure-score-image-model"></a>Puan resmi modelini yapılandırma

1. İşlem hattınızı, **Puanlama görüntü modeli** modülünü ekleyin.

2. Eğitilen bir görüntü modeli ve girdi resim verilerini içeren bir veri kümesi ekleyin. 

    Verilerin ImageDirectory türünde olması gerekir. Görüntü dizini alma hakkında daha fazla bilgi için bkz. [görüntü dizini modülüne dönüştürme](convert-to-image-directory.md) . Giriş veri kümesinin şeması da genellikle modeli eğitmek için kullanılan verilerin şemasıyla aynı olmalıdır.

3. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

[Puan görüntü modeli](score-image-model.md)kullanarak bir puan kümesi oluşturduktan sonra, modelin doğruluğunu (performans) değerlendirmek için kullanılan bir dizi ölçüm oluşturmak üzere, bu modülü ve puanlanmış veri kümesini [modeli değerlendirmek](evaluate-model.md)için bağlayabilirsiniz, 

### <a name="publish-scores-as-a-web-service"></a>Puanları Web hizmeti olarak yayımlama

Puanlama kullanımı, bir tahmine dayalı Web hizmetinin parçası olarak çıktıyı döndürmemelidir. Daha fazla bilgi için, Azure Machine Learning tasarımcısında bir işlem hattına göre gerçek zamanlı bir uç noktanın nasıl dağıtılacağı hakkında [Bu öğreticiye](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
