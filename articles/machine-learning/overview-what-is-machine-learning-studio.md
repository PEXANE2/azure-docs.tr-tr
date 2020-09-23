---
title: Azure Machine Learning stüdyo nedir?
description: Azure Machine Learning Studio, Azure Machine Learning çalışma alanları için bir Web portalıdır. Studio, kapsamlı bir veri bilimi platformu oluşturmak için kod içermeyen ve kod-ilk deneyimleri birleştirir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: peterclu
ms.author: peterlu
ms.date: 08/24/2020
ms.openlocfilehash: 40a4fbd956b12d469247cb178007d0259cbeac75
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90902930"
---
# <a name="what-is-azure-machine-learning-studio"></a>Azure Machine Learning stüdyo nedir?

Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md)veri bilimi geliştiricileri için Web portalı olan Azure Machine Learning Studio hakkında bilgi edineceksiniz. Studio, kapsamlı bir veri bilimi platformu için kod içermez ve kod ilk deneyimlerini birleştirir.

Bu makalede şunları öğreneceksiniz:
>[!div class="checklist"]
> - Studio 'da [makine öğrenimi projelerini yazma](#author-machine-learning-projects) .
> - Studio 'da [varlıkları ve kaynakları yönetme](#manage-assets-and-resources) .
> - [Azure Machine Learning Studio ile ml Studio arasındaki farklar (klasik)](#ml-studio-classic-vs-azure-machine-learning-studio).


## <a name="author-machine-learning-projects"></a>Machine Learning projelerini yaz

Studio, tür projesine ve kullanıcı deneyiminin düzeyine bağlı olarak birden çok yazma deneyimi sunar.

+ **Not Defterleri**

  Doğrudan Studio 'da tümleştirilmiş olan yönetilen [Jupyter Notebook sunucularında](how-to-run-jupyter-notebooks.md) kendi kodunuzu yazın ve çalıştırın. 

+ **Azure Machine Learning tasarımcısı**

  Tasarımcı kullanarak makine öğrenimi modellerini herhangi bir kod yazmadan eğitme ve dağıtma. ML işlem hatları oluşturmak için veri kümelerini ve modülleri sürükleyip bırakın. [Tasarımcı öğreticisini](tutorial-designer-automobile-price-train-score.md)deneyin.

    ![Azure Machine Learning tasarımcı örneği](media/concept-designer/designer-drag-and-drop.gif)

+ **Otomatik makine öğrenme Kullanıcı arabirimi**

  Kullanımı kolay bir arabirimle [OTOMATIK ml denemeleri](tutorial-first-experiment-automated-ml.md) oluşturma hakkında bilgi edinin. 

  [![Azure Machine Learning Studio gezinti bölmesi](./media/overview-what-is-azure-ml/azure-machine-learning-automated-ml-ui.jpg)](./media/overview-what-is-azure-ml/azure-machine-learning-automated-ml-ui.jpg)

+ **Veri etiketleme**

    Veri etiketleme projelerini etkin bir şekilde koordine etmek için [Azure Machine Learning veri etiketleme](how-to-create-labeling-projects.md) kullanın.

## <a name="manage-assets-and-resources"></a>Varlıkları ve kaynakları yönetme

Makine öğrenimi varlıklarınızı doğrudan tarayıcınızda yönetin. Varlıklar, sorunsuz bir deneyim için SDK ile Studio arasındaki aynı çalışma alanında paylaşılır. Yönetmek için Studio 'yu kullanın:

- Modeller
- Veri kümeleri
- Veri depoları
- İşlem kaynakları
- Notebooks
- Denemeler
- Günlükleri Çalıştır
- İşlem hatları 
- Ardışık düzen uç noktaları

Deneyimli bir geliştirici olsanız bile, Studio çalışma alanı kaynaklarını nasıl yönetebileceğinizi kolaylaştırabilir.

## <a name="ml-studio-classic-vs-azure-machine-learning-studio"></a>ML Studio (klasik) vs Azure Machine Learning Studio

2015 ' de yayınlanan **ml Studio (klasik)** ilk sürükle ve bırak Machine Learning kurucumız vardı. Yalnızca bir görsel deneyim sunan tek başına bir hizmettir. Studio (klasik) Azure Machine Learning ile birlikte çalışmıyor.

**Azure Machine Learning** , kapsamlı bir veri bilimi platformu sunan ayrı ve modernlanmış bir hizmettir. Hem kod hem de düşük kod deneyimlerini destekler.

**Azure Machine Learning Studio** , proje yazma ve varlık yönetimi için düşük kod ve kod içermeyen seçenekler *içeren Azure Machine Learning bir* Web portalıdır. 

Yeni kullanıcıların, en son veri bilimi araçları aralığı için ML Studio (klasik) yerine **Azure Machine Learning**seçmesini öneririz.

### <a name="feature-comparison"></a>Özellik karşılaştırması

Aşağıdaki tablo ML Studio (klasik) ve Azure Machine Learning arasındaki önemli farklılıkları özetler.

| Öne çıkan özelliği | ML Studio (klasik) | Azure Machine Learning |
|---| --- | --- |
| Sürükle ve bırak arabirimi | Klasik deneyim | Güncelleştirilmiş deneyim- [Azure Machine Learning Tasarımcısı](concept-designer.md)| 
| Kod SDK 'Ları | Desteklenmeyen | [Azure Machine Learning Python](https://docs.microsoft.com/python/api/overview/azure/ml/) ve [R](tutorial-1st-r-experiment.md) SDK 'leriyle tamamen tümleşik |
| Deneme | Ölçeklenebilir (10 GB eğitim veri limiti) | İşlem hedefi ile ölçeklendirme |
| Eğitim işlem hedefleri | Özel işlem hedefi, yalnızca CPU desteği | Geniş kapsamlı özelleştirilebilir [eğitim işlem hedefleri](concept-compute-target.md#train). GPU ve CPU desteği içerir | 
| Dağıtım işlem hedefleri | Özel Web hizmeti biçimi özelleştirilebilir değil | Geniş kapsamlı özelleştirilebilir [dağıtım işlem hedefleri](concept-compute-target.md#deploy). GPU ve CPU desteği içerir |
| ML işlem hattı | Desteklenmez | İş akışlarını otomatikleştirmek için esnek, modüler işlem [hatları](concept-ml-pipelines.md) oluşturun |
| MLOps | Temel model yönetimi ve dağıtımı | Varlık sürümü oluşturma (model, veri, iş akışları), iş akışı Otomasyonu, CICD araçları ile tümleştirme [ve daha fazlası](concept-model-management-and-deployment.md) |
| Model biçimi | Özel biçim, yalnızca Studio (klasik) | Eğitim işi türüne bağlı olarak desteklenen birden çok biçim |
| Otomatik model eğitimi ve hyperparameter ayarlaması |  Desteklenmez | [Desteklenir](concept-automated-ml.md). Code-First ve Code No Options. | 
| Veri drması algılama | Desteklenmez | [Desteklenir](how-to-monitor-datasets.md) |
| Veri etiketleme projeleri | Desteklenmez | [Desteklenir](how-to-create-labeling-projects.md) |


## <a name="next-steps"></a>Sonraki adımlar

[Studio 'yu](https://ml.azure.com)ziyaret edin veya bu öğreticilerle farklı yazma seçeneklerini keşfedebilirsiniz:  
  + [& dağıtım modellerini eğitme için Python not defterlerini kullanma](tutorial-1st-experiment-sdk-setup.md)
  + [& dağıtım modellerini eğitme için otomatik makine öğrenimi kullanma](tutorial-first-experiment-automated-ml.md)  
  + [& modellerini eğitmek için tasarımcıyı kullanma](tutorial-designer-automobile-price-train-score.md)

