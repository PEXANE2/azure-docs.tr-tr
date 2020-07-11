---
title: Azure Machine Learning-Machine Learning Studio (klasik)
description: Azure Machine Learning ve Machine Learning Studio (klasik) arasındaki fark nedir?
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 03/25/2020
ms.openlocfilehash: 1d4896a2dd2115e5654baffe394618b30a294d5b
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86231752"
---
# <a name="azure-machine-learning-vs-machine-learning-studio-classic"></a>Azure Machine Learning vs Machine Learning Studio (klasik)

Bu makalede Azure Machine Learning ve Machine Learning Studio (klasik) arasındaki farkı öğrenirsiniz. 

Azure Machine Learning, makine öğrenimi modelleri derlemek ve dağıtmak için Python ve R SDK 'Ları **ve** "sürükle ve bırak" tasarımcısını sağlar. Studio (klasik) yalnızca bağımsız bir sürükle ve bırak deneyimi sunar.

Yeni kullanıcıların en geniş düzey son teknoloji makine öğrenimi araçları için Azure Machine Learning seçmesini öneririz.

## <a name="quick-comparison"></a>Hızlı karşılaştırma

Aşağıdaki tabloda Azure Machine Learning ve Studio arasındaki bazı önemli farklılıklar özetlenmektedir (klasik):

| Özellik | Machine Learning Studio (klasik) | Azure Machine Learning |
|---| --- | --- |
| Sürükle ve bırak arabirimi | Desteklenir | Desteklenen- [Azure Machine Learning Tasarımcısı (Önizleme)](concept-designer.md) <br/>(Kurumsal çalışma alanı gerektirir) | 
| Deneme | Ölçeklenebilir (10 GB eğitim veri limiti) | İşlem hedefi ile ölçeklendirme |
| Eğitim işlem hedefleri | Özel işlem hedefi, yalnızca CPU desteği | Geniş kapsamlı özelleştirilebilir [eğitim işlem hedefleri](concept-compute-target.md#train). GPU ve CPU desteği içerir | 
| Dağıtım işlem hedefleri | Özel Web hizmeti biçimi özelleştirilebilir değil | Geniş kapsamlı özelleştirilebilir [dağıtım işlem hedefleri](concept-compute-target.md#deploy). GPU ve CPU desteği içerir |
| ML işlem hattı | Desteklenmez | İş akışlarını otomatikleştirmek için esnek, modüler işlem [hatları](concept-ml-pipelines.md) oluşturun |
| MLOps | Temel model yönetimi ve dağıtımı | Varlık sürümü oluşturma (model, veri, iş akışları), iş akışı Otomasyonu, CICD araçları ile tümleştirme [ve daha fazlası](concept-model-management-and-deployment.md) |
| Model biçimi | Özel biçim, yalnızca Studio (klasik) | Eğitim işi türüne bağlı olarak desteklenen birden çok biçim |
| Otomatik model eğitimi ve hyperparameter ayarlaması |  Desteklenmez | [SDK ve Visual çalışma alanında desteklenir](concept-automated-ml.md) | 
| Veri drması algılama | Desteklenmez | [SDK ve Visual çalışma alanında desteklenir](how-to-monitor-datasets.md) |


## <a name="migrate-from-machine-learning-studio-classic"></a>Machine Learning Studio geçir (klasik)

Şu anda, Studio (klasik) varlıkları Azure Machine Learning tasarımcısına (Önizleme) geçirmenin bir yolu yoktur. Geçerli Studio (klasik) kullanıcılar makine öğrenimi varlıklarını kullanmaya devam edebilir. Ancak, tüm kullanıcıların tasarımcı kullanarak göz önünde bulundurması önerilir. Bu, **geliştirilmiş iş akışı ile ölçeklenebilirlik** , sürüm denetimi ve kurumsal güvenlik konusunda tanıdık bir sürükle ve bırak deneyimi sağlar.

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

Aşağıdaki kaynaklar Azure Machine Learning kullanmaya başlamanıza yardımcı olabilir. 

- [Azure Machine Learning genel bakış](overview-what-is-azure-ml.md)bölümünü okuyun.

- [Python SDK ile ilk denemenizi](tutorial-1st-experiment-sdk-setup.md)oluşturun.

- Otomatik fiyatları tahmin etmek için [ilk tasarımcı işlem hattınızı oluşturun](tutorial-designer-automobile-price-train-score.md) .

![Azure Machine Learning tasarımcı örneği](media/concept-designer/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Sonraki adımlar

Tasarımcıda Sürükle ve bırak özelliklerine ek olarak, Azure Machine Learning başka araçlar da mevcuttur:  
  + [ML modellerini eğitmek & dağıtmak için Python not defterlerini kullanma](tutorial-1st-experiment-sdk-setup.md)
  + [ML modellerini eğitme & dağıtmak için R Markdown kullanma](tutorial-1st-r-experiment.md) 
  + [ML modellerini eğitmek & dağıtmak için otomatik makine öğrenimi kullanma](tutorial-first-experiment-automated-ml.md)  
  + [Makine öğrenimi CLı kullanarak bir modeli eğitme ve dağıtma](tutorial-train-deploy-model-cli.md)

