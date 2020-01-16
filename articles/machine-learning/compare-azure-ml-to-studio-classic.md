---
title: Azure Machine Learning-Machine Learning Studio (klasik)
description: Azure Machine Learning Machine Learning Studio 'den farklı (klasik)
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 10/29/2019
ms.openlocfilehash: a3122a3ae1687369b87d193efc693b3b7c659aac
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75974129"
---
# <a name="how-azure-machine-learning-differs-from-machine-learning-studio-classic"></a>Azure Machine Learning Machine Learning Studio 'den farklı (klasik)

Bu makalede, Azure Machine Learning özellikleri, özellikleri ve arabirimi Machine Learning Studio (klasik) ile karşılaştırılır. 

## <a name="about-machine-learning-studio-classic"></a>Machine Learning Studio hakkında (klasik)
[Machine Learning Studio (klasik)](studio/what-is-ml-studio.md) , kod yazmaya gerek kalmadan makine öğrenimi çözümlerini oluşturabileceğiniz, test ettiğiniz ve dağıtabileceğiniz, birlikte çalışan, sürükle ve bırak görsel çalışma alanıdır. Önceden oluşturulmuş ve önceden yapılandırılmış makine öğrenimi algoritmalarını ve veri işleme modüllerini, ayrıca özel bir işlem platformunu kullanır.

## <a name="about-azure-machine-learning"></a>Azure Machine Learning hakkında

[Azure Machine Learning](overview-what-is-azure-ml.md) , her ikisi de tasarımcı (Önizleme) **ve** ÇEŞITLI SDK 'lar ve CLI adlı bir Web arabirimi sağlayarak verileri hızlı bir şekilde hazırlayın, eğitim ve dağıtım, makine öğrenimi modellerini dağıtır. Azure Machine Learning, otomatik makine öğrenimi ve işlem hattı desteği gibi ölçek, birden çok Framework desteği, gelişmiş ML özellikleri de alır.

Azure Machine Learning tasarımcı, Studio 'ya (klasik) benzer bir sürükle ve bırak deneyimi sağlar. Ancak, Studio 'nun özel işlem platformunun (klasik) aksine, tasarımcı kendi işlem kaynaklarınızı kullanır, ölçeklenebilir ve Azure Machine Learning tamamen tümleşiktir.  

> [!TIP]
> Şu anda Machine Learning Studio (klasik) kullanan veya değerlendiren müşterilerin, sürükle ve bırak __ml modülleri ve__ ölçeklenebilirlik, sürüm denetimi ve kurumsal güvenlik sağlayan [Azure Machine Learning tasarımcı](https://docs.microsoft.com/azure/machine-learning/concept-designer) 'yı (Önizleme) denemesini sağlamak önerilir.

## <a name="comparison-azure-machine-learning-vs-machine-learning-studio-classic"></a>Karşılaştırma: Azure Machine Learning ile Machine Learning Studio (klasik)

Hızlı bir karşılaştırma aşağıda verilmiştir.

||  Azure Machine Learning Tasarımcısı|Studio (klasik) |
|---| --- | --- |
||Tasarımcı önizlemededir, Azure Machine Learning GA 'dir|Genel olarak kullanılabilir (GA) | 
|Sürükle bırak arabirimi| Evet | Evet|
|Deneme| İşlem hedefi ile ölçeklendirme|Ölçek (10 GB eğitim veri limiti) | 
|Arabirim için modüller| [Birçok popüler modül](algorithm-module-reference/module-reference.md) | Sayısı |
|Eğitim işlem hedefleri| AML Işlem (GPU/CPU)|Özel işlem hedefi, yalnızca CPU|
|Inlekrime işlem hedefleri| Gerçek zamanlı çıkarım için Azure Kubernetes hizmeti <br/>Toplu çıkarım için AML Işlem|Özel Web hizmeti biçimi özelleştirilebilir değil | 
|ML işlem hattı| İşlem hattı yazma <br/> Yayınlanan işlem hattı <br/> Ardışık düzen uç noktası <br/> [ML işlem hattı hakkında daha fazla bilgi](concept-ml-pipelines.md)|Desteklenmiyor | 
|ML Ops| Yapılandırılabilir dağıtım, model ve işlem hattı sürümü oluşturma|Temel model yönetimi ve dağıtımı | 
|Model| Standart biçim, çeşitli eğitim işine bağlıdır|Tescilli, taşınabilir olmayan biçim.| 
|Otomatik model eğitimi|Henüz tasarımcıda değildir, ancak arabirim ve SDK 'lar aracılığıyla mümkün değildir.| Hayır | 

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

Aşağıdaki kaynaklar Azure Machine Learning kullanmaya başlamanıza yardımcı olabilir

- [Azure Machine Learning genel bakış](tutorial-first-experiment-automated-ml.md) bölümünü okuyun 

- Otomatik fiyatları tahmin etmek için [ilk tasarımcı işlem hattınızı oluşturun](tutorial-designer-automobile-price-train-score.md) .

![Azure Machine Learning tasarımcı örneği](media/concept-designer/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Sonraki adımlar

Tasarımcıda Sürükle ve bırak özelliklerine ek olarak, Azure Machine Learning başka araçlar da mevcuttur:  
  + [ML modellerini eğitmek & dağıtmak için Python not defterlerini kullanma](tutorial-1st-experiment-sdk-setup.md)
  + [ML modellerini eğitme & dağıtmak için R Markdown kullanma](tutorial-1st-r-experiment.md) 
  + [ML modellerini eğitmek & dağıtmak için otomatik makine öğrenimi kullanma](tutorial-designer-automobile-price-train-score.md) 
  + [Makine öğrenimi CLı kullanarak bir modeli eğitme ve dağıtma](tutorial-train-deploy-model-cli.md)

