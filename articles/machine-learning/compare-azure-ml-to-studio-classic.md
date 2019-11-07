---
title: Azure Machine Learning-ML Studio (klasik)
description: Azure Machine Learning ML Studio 'den farklı (klasik)
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 10/29/2019
ms.openlocfilehash: dae708a417a1bf8ca4b118f218e353a28e5675c1
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684965"
---
# <a name="how-azure-machine-learning-differs-from-ml-studio-classic"></a>Azure Machine Learning ML Studio 'den farklı (klasik)

Bu makalede, Azure Machine Learning özellikleri, özellikleri ve arabirimi ML Studio (klasik) ile karşılaştırılır. 

## <a name="about-ml-studio-classic"></a>ML Studio hakkında (klasik)
[Ml Studio (klasik)](/studio/what-is-ml-studio.md) , kod yazmaya gerek kalmadan makine öğrenimi çözümlerini oluşturabileceğiniz, test ettiğiniz ve dağıtabileceğiniz, birlikte çalışan, sürükle ve bırak görsel çalışma alanıdır. Önceden oluşturulmuş ve önceden yapılandırılmış makine öğrenimi algoritmalarını ve veri işleme modüllerini, ayrıca özel bir işlem platformunu kullanır.

## <a name="about-azure-machine-learning"></a>Azure Machine Learning hakkında

[Azure Machine Learning](/service/overview-what-is-azure-ml.md) , her ikisi de tasarımcı (Önizleme) **ve** ÇEŞITLI SDK 'lar ve CLI adlı bir Web arabirimi sağlayarak verileri hızlı bir şekilde hazırlayın, eğitim ve dağıtım, makine öğrenimi modellerini dağıtır. Azure Machine Learning, otomatik makine öğrenimi ve işlem hattı desteği gibi ölçek, birden çok Framework desteği, gelişmiş ML özellikleri de alır.

Azure Machine Learning tasarımcı, Studio 'ya (klasik) benzer bir sürükle ve bırak deneyimi sağlar. Ancak, Studio 'nun özel işlem platformunun (klasik) aksine, tasarımcı kendi işlem kaynaklarınızı kullanır, ölçeklenebilir ve Azure Machine Learning tamamen tümleşiktir.  

> [!TIP]
> Şu anda Machine Learning Studio (klasik) kullanan veya değerlendiren müşterilerin, sürükle ve bırak __ml modülleri ve__ ölçeklenebilirlik, sürüm denetimi ve kurumsal güvenlik sağlayan [Azure Machine Learning tasarımcı](https://docs.microsoft.com/azure/machine-learning/service/ui-concept-visual-interface) 'yı (Önizleme) denemesini sağlamak önerilir.

## <a name="comparison-azure-machine-learning-vs-ml-studio-classic"></a>Karşılaştırma: Azure Machine Learning ile ML Studio (klasik)

Hızlı bir karşılaştırma aşağıda verilmiştir.

||  Azure Machine Learning Tasarımcısı|Studio (klasik) |
|---| --- | --- |
||Tasarımcı önizleme aşamasındadır, Azure Machine Learning GA 'dir|Genel olarak kullanılabilir (GA) | 
|Sürükle ve bırak arabirimi| Evet | Evet|
|Deneyin| İşlem hedefi ile ölçeklendirme|Ölçek (10 GB eğitim veri limiti) | 
|Arabirim için modüller|  Birçok popüler modül|Sayısı |
|Eğitim işlem hedefleri| AML Işlem (GPU/CPU)|Özel işlem hedefi, yalnızca CPU|
|Inlekrime işlem hedefleri| Gerçek zamanlı çıkarım için Azure Kubernetes hizmeti <br/>Toplu çıkarım için AML Işlem|Özel Web hizmeti biçimi özelleştirilebilir değil | 
|ML işlem hattı| İşlem hattı yazma <br/> Yayınlanan işlem hattı <br/> Ardışık düzen uç noktası <br/> [ML işlem hattı hakkında daha fazla bilgi](service/concept-ml-pipelines.md)|Desteklenmiyor | 
|ML Ops| Yapılandırılabilir dağıtım, model ve işlem hattı sürümü oluşturma|Temel model yönetimi ve dağıtımı | 
|Model| Standart biçim, çeşitli eğitim işine bağlıdır|Tescilli, taşınabilir olmayan biçim.| 
|Otomatik model eğitimi|Henüz tasarımcıda değildir, ancak arabirim ve SDK 'lar aracılığıyla mümkün değildir.| Hayır | 

## <a name="get-started-with-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcı ile çalışmaya başlama

Aşağıdaki kaynaklar Azure Machine Learning kullanmaya başlamanıza yardımcı olabilir

- [Azure Machine Learning genel bakış](service/tutorial-first-experiment-automated-ml.md) bölümünü okuyun 

- Otomatik fiyatları tahmin etmek için [ilk tasarımcı işlem hattınızı oluşturun](service/tutorial-designer-automobile-price-train-score.md) .


![Azure Machine Learning tasarımcı örneği](service/media/concept-ml-pipelines/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Sonraki adımlar

Tasarımcıda Sürükle-n-Bırak özelliklerine ek olarak Azure Machine Learning başka araçlar da mevcuttur:  
  + [ML modellerini eğitmek & dağıtmak için Python not defterlerini kullanma](./service/tutorial-1st-experiment-sdk-setup.md)
  + [ML modellerini eğitme & dağıtmak için R Markdown kullanma](./service/tutorial-1st-r-experiment.md) 
  + [ML modellerini eğitmek & dağıtmak için otomatik makine öğrenimi kullanma](./service/tutorial-designer-automobile-price-train-score.md) 
  + [Makine öğrenimi CLı kullanarak bir modeli eğitme ve dağıtma](./service/tutorial-train-deploy-model-cli.md)

