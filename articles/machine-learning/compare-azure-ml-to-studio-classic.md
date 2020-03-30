---
title: Azure Machine Learning vs. Machine Learning Studio (klasik)
description: Azure Machine Learning ve Machine Learning Studio (klasik) arasındaki fark nedir?
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 03/25/2020
ms.openlocfilehash: 5577a9847ff405397c553028a6dfdf2df80d03fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371832"
---
# <a name="azure-machine-learning-vs-machine-learning-studio-classic"></a>Azure Machine Learning vs Machine Learning Studio (klasik)

Bu makalede, Azure Machine Learning ve Machine Learning Studio (klasik) arasındaki farkı öğrenebilirsiniz. 

Azure Machine Learning, Python ve R SDK'ları **ve** makine öğrenimi modelleri oluşturmak ve dağıtmak için "sürükle ve bırak" tasarımcısını sağlar. Studio (klasik) sadece bağımsız bir sürükle-ve-bırak deneyimi sunuyor.

Yeni kullanıcıların en geniş yelpazedeki en ileri makine öğrenimi araçları için Azure Machine Learning'i seçmelerini öneririz.

## <a name="quick-comparison"></a>Hızlı karşılaştırma

Aşağıdaki tablo, Azure Machine Learning ve Studio (klasik) arasındaki bazı temel farkları özetleyilmiştir:

| | Machine Learning Studio (klasik) | Azure Machine Learning |
|---| --- | --- |
| Sürükle ve bırak arabirimi | Destekleniyor | Desteklenen - [Azure Machine Learning tasarımcısı (önizleme)](concept-designer.md) | 
| Deneme | Ölçeklenebilir (10 GB eğitim veri sınırı) | İşlem hedefiyle ölçeklendirin |
| Eğitim hesaplama hedefleri | Özel işlem hedefi, yalnızca CPU desteği | Özelleştirilebilir eğitim [hesaplama hedefleri](concept-compute-target.md#train)geniş bir yelpazede. GPU ve CPU desteği içerir | 
| Dağıtım işlem hedefleri | Özel web hizmet biçimi, özelleştirilebilir değil | Çok çeşitli özelleştirilebilir [dağıtım işlem hedefleri.](concept-compute-target.md#deploy) GPU ve CPU desteği içerir |
| ML Boru Hattı | Desteklenmiyor | İş [akışlarını](concept-ml-pipelines.md) otomatikleştirmek için esnek, modüler boru hatları oluşturun |
| MISPler | Temel model yönetimi ve dağıtım | Varlık sürümü (model, veri, iş akışları), iş akışı otomasyonu, CICD takımlama ile entegrasyon [ve daha fazlası](concept-model-management-and-deployment.md) |
| Model biçimi | Tescilli format, Studio (klasik) sadece | Eğitim iş türüne bağlı olarak birden çok desteklenen biçim |
| Otomatik model eğitimi ve hiperparametre |  Desteklenmiyor | [SDK ve görsel çalışma alanında desteklendi](concept-automated-ml.md) | 
| Veri kayması algılama | Desteklenmiyor | [SDK ve görsel çalışma alanında desteklendi](how-to-monitor-datasets.md) |


## <a name="migrate-from-machine-learning-studio-classic"></a>Machine Learning Studio'dan göç edin (klasik)

Şu anda, Studio (klasik) varlıklarını Azure Machine Learning tasarımcısına (önizleme) geçirmenin bir yolu yoktur. Geçerli Studio (klasik) kullanıcıları makine öğrenimi varlıklarını kullanmaya devam edebilirler. Ancak, tüm kullanıcıları, geliştirilmiş iş akışı **artı** ölçeklenebilirlik, sürüm denetimi ve kurumsal güvenlik ile tanıdık bir sürükle ve bırak deneyimi sağlayan tasarımcıyı kullanmayı düşünmeye teşvik ediyoruz.

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning ile başlayın

Aşağıdaki kaynaklar Azure Machine Learning'e başlamanıza yardımcı olabilir. 

- Azure [Machine Learning genel görünümünü](overview-what-is-azure-ml.md)okuyun.

- Python [SDK ile ilk denemenizi](tutorial-1st-experiment-sdk-setup.md)oluşturun.

- Otomobil fiyatlarını tahmin etmek için [ilk tasarımcı boru hattınızı oluşturun.](tutorial-designer-automobile-price-train-score.md)

![Azure Machine Learning tasarımcı örneği](media/concept-designer/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning, tasarımcıdaki sürükle ve bırak özelliklerine ek olarak, kullanılabilen başka araçlarda da vardır:  
  + [ML modellerini eğitmek & dağıtmak için Python dizüstü bilgisayarları kullanın](tutorial-1st-experiment-sdk-setup.md)
  + [ML modellerini eğitmek & dağıtmak için R Markdown'ı kullanın](tutorial-1st-r-experiment.md) 
  + [ML modellerini eğitmek & dağıtmak için otomatik makine öğrenimini kullanın](tutorial-first-experiment-automated-ml.md)  
  + [Bir modeli eğitmek ve dağıtmak için CLI'yi öğrenen makineyi kullanın](tutorial-train-deploy-model-cli.md)

