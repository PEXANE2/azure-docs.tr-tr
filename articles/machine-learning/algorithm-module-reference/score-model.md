---
title: 'Puan Modeli: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Eğitimli bir sınıflandırma veya regresyon modeli kullanarak öngörüler oluşturmak için Azure Machine Learning'de Puan Modeli modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 56d8cad05a42da8de680ade487dddee9a97aab3a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80364178"
---
# <a name="score-model-module"></a>Score Model (Model Puanlama) modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Eğitimli bir sınıflandırma veya regresyon modeli kullanarak öngörüler oluşturmak için bu modülü kullanın.

## <a name="how-to-use"></a>Nasıl kullanılır

1. Puan **Modeli** modüllerini boru hattınıza ekleyin.

2. Eğitimli bir model ve yeni giriş verileri içeren bir veri kümesi takın. 

    Veriler, kullanmakta olduğunuz eğitilen model türüyle uyumlu bir biçimde olmalıdır. Giriş veri kümesinin şeması da genellikle modeli eğitmek için kullanılan verilerin şema eşleşmelidir.

3. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

[Puan Modelini](./score-model.md)kullanarak bir puan kümesi oluşturduktan sonra:

+ Modelin doğruluğunu (performansını) değerlendirmek için kullanılan bir ölçüm kümesi oluşturmak için, puanlanan veri kümesini [Model'i değerlendirmek](./evaluate-model.md)için bağlayabilirsiniz, 
+ Modüle sağ tıklayın ve sonuçların bir örneğini görmek için **Visualize'ı** seçin.
<!-- + To Save the results to a dataset. -->

Puan veya öngörülen değer, modele ve giriş verilerinize bağlı olarak birçok farklı biçimde olabilir:

- Sınıflandırma modelleri [için, Puan Modeli](./score-model.md) sınıf için öngörülen değerin yanı sıra öngörülen değerin olasılığını çıkarır.
- Regresyon modelleri [için, Score Model](./score-model.md) yalnızca öngörülen sayısal değeri oluşturur.


## <a name="publish-scores-as-a-web-service"></a>Puanları web hizmeti olarak yayımlama

Puanlamanın yaygın kullanımı, çıktıyı tahmine dayalı bir web hizmetinin parçası olarak döndürmektir. Daha fazla bilgi için, Azure Machine Learning tasarımcısındaki bir ardışık yapıya dayalı gerçek zamanlı bir bitiş noktasının nasıl dağıtılanabildiğini anlatan [bu öğreticiye](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 