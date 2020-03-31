---
title: Tasarımcı ile ML modelleri oluşturun
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısını oluşturan terimler, kavramlar ve iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: peterlu
author: peterclu
ms.date: 11/12/2019
ms.openlocfilehash: 78a6e7fa8d030185f537136a3a2124d8bc59d808
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79037622"
---
# <a name="what-is-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısı (önizleme) nedir? 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Azure Machine Learning tasarımcısı, makine öğrenimi modelleri oluşturmak için [veri kümelerini](#datasets) ve [modülleri](#module) etkileşimli bir tuvale görsel olarak bağlamanızı sağlar. Tasarımcıyla nasıl başlayın, [bkz.](tutorial-designer-automobile-price-train-score.md)

![Azure Machine Learning tasarımcı örneği](./media/concept-designer/designer-drag-and-drop.gif)

Tasarımcı, Azure Machine Learning [çalışma alanınızı](concept-workspace.md) aşağıdakiler gibi paylaşılan kaynakları düzenlemek için kullanır:

+ [İşlem hatları](#pipeline)
+ [Veri kümeleri](#datasets)
+ [Kaynak hesaplama](#compute)
+ [Kayıtlı modeller](concept-azure-machine-learning-architecture.md#models)
+ [Yayınlanan boru hatları](#publish)
+ [Gerçek zamanlı uç noktalar](#deploy)

## <a name="model-training-and-deployment"></a>Model eğitimi ve dağıtım

Tasarımcı, makine öğrenimi modellerini oluşturmanız, test etmek ve dağıtmak için size görsel bir tuval sağlar. Tasarımcı ile şunları yapabilirsiniz:

+ [Veri kümelerini](#datasets) ve [modülleri](#module) tuvale sürükleyip bırakın.
+ Bir [boru hattı taslağı](#pipeline-draft)oluşturmak için modülleri birbirine bağlayın.
+ Azure Machine Learning çalışma alanınızdaki bilgi işlem kaynaklarını kullanarak bir [ardışık işlem çalışması](#pipeline-run) gönderin.
+ Eğitim **ardışık hatlarınızı** **çıkarım boru hatlarına**dönüştürün.
+ Farklı parametreler ve veri kümeleri içeren yeni ardışık hatlar çalışır' göndermek için ardışık hatlarınızı bir REST **ardışık nokta** yla [yayımlayın.](#publish)
    + Parametreleri ve veri kümelerini değiştirirken birden çok modeli eğitmek için tek bir ardışık hattı yeniden kullanmak için bir **eğitim ardışık hattı** yayımlayın.
    + Önceden eğitilmiş bir model kullanarak yeni veriler üzerinde öngörülerde bulunmak için toplu **çıkarım ardışık bir** işlem yayımlayın.
+ Yeni veriler le ilgili öngörülerde bulunmak için **gerçek zamanlı çıkarım ardışık hattını** gerçek zamanlı bir bitiş noktasına [dağıtın.](#deploy)

![Tasarımcıda eğitim, toplu çıkarım ve gerçek zamanlı çıkarım için iş akışı diyagramı](./media/concept-designer/designer-workflow-diagram.png)

## <a name="pipeline"></a>İşlem hattı

[Bir ardışık işlem,](concept-azure-machine-learning-architecture.md#ml-pipelines) birbirine bağlandığınız veri kümeleri ve analitik modüllerden oluşur. Boru hatlarının birçok faydası vardır: tek bir modeli veya birden çok modeli eğiten bir boru hattı yapabilirsiniz. Öngörüleri gerçek zamanlı veya toplu olarak yapan bir ardışık hatlar oluşturabilir veya yalnızca verileri temizleyen bir ardışık hatlar oluşturabilirsiniz. Boru hatları, çalışmanızı yeniden kullanmanıza ve projelerinizi düzenlemenize izin sağlar.

### <a name="pipeline-draft"></a>Boru hattı taslağı

Tasarımcıda bir ardışık hattı nı olarak, ilerlemeniz bir **boru hattı taslağı**olarak kaydedilir. Modülleri ekleyerek veya kaldırarak, bilgi işlem hedeflerini yapılandırarak, parametreler oluşturarak ve benzeri nedenlerle istediğiniz noktada bir boru hattı taslağını düzenleme yapabilirsiniz.

Geçerli bir ardışık hat:

* Veri kümeleri yalnızca modüllere bağlanabilir.
* Modüller yalnızca veri kümelerine veya diğer modüllere bağlanabilir.
* Modüller için tüm giriş bağlantı noktaları nın veri akışına bir bağlantısı olmalıdır.
* Her modül için gerekli tüm parametreler ayarlanmalıdır.

Boru hattı taslağınızı çalıştırmaya hazır olduğunuzda, bir boru hattı çalıştırın.

### <a name="pipeline-run"></a>Boru hattı çalıştır

Bir ardışık ardışık yapıyı her çalıştırdığınızda, ardışık yapının yapılandırması ve sonuçları çalışma alanınızda **bir ardışık iş bölümü olarak**depolanır. Sorun giderme veya denetim amacıyla denetlemek için herhangi bir ardışık hat lar çalışmasına geri dönebilirsiniz. Yeni **bir** boru hattı taslağı oluşturmak için bir boru hattı çalıştırın.

Ardışık hatlar çalışır, çalışma geçmişini düzenlemek için [denemeler](concept-azure-machine-learning-architecture.md#experiments) halinde gruplanır. Denemeyi her ardışık hat lar için ayarlayabilirsiniz. 

## <a name="datasets"></a>Veri kümeleri

Makine öğrenimi veri seti, verilerinize erişmenizi ve verilerinizle çalışmayı kolaylaştırır. Denemeniz için tasarımcıya çok sayıda örnek veri kümesi dahildir. İhtiyacınız [register](how-to-create-register-datasets.md) olduğu anda daha fazla veri kümesi kaydedebilirsiniz.

## <a name="module"></a>Modül

Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Tasarımcının veri giriş işlevlerinden eğitim, puanlama ve doğrulama işlemlerine kadar çeşitli modülleri vardır.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki Özellikler bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz. Tasarımcıdaki tek tek modüller için bilgi işlem kaynaklarını ayarlayabilirsiniz. 

![Modül özellikleri](./media/concept-designer/properties.png)

Mevcut makine öğrenimi algoritmaları kitaplığında gezinmek için [algoritma & modül referans özetine](algorithm-module-reference/module-reference.md) bakın

## <a name="compute-resources"></a><a name="compute"></a>Kaynak hesaplama

Ardışık hattınızı çalıştırmak ve dağıtılmış modellerinizi gerçek zamanlı uç noktalar veya boru hattı uç noktaları (toplu iş çıkarımı için) olarak barındırmak için çalışma alanınızdaki bilgi işlem kaynaklarını kullanın. Desteklenen işlem hedefleri şunlardır:

| İşlem hedefi | Eğitim | Dağıtım |
| ---- |:----:|:----:|
| Azure Machine Learning bilgi işlem | ✓ | |
| Azure Kubernetes Service | | ✓ |

İşlem hedefleri [Azure Machine Learning çalışma alanınıza](concept-workspace.md)eklenir. Azure [Machine Learning Studio'da (klasik)](https://ml.azure.com)iş alanınızda bilgi işlem hedeflerinizi yönetirsiniz.

## <a name="deploy"></a>Dağıtma

Gerçek zamanlı çıkarsama gerçekleştirmek için, bir ardışık ardışık hattı **gerçek zamanlı bitiş noktası**olarak dağıtmanız gerekir. Gerçek zamanlı bitiş noktası, harici bir uygulama ile puanlama modeliniz arasında bir arabirim oluşturur. Gerçek zamanlı bitiş noktasına yapılan arama, tahmin sonuçlarını uygulamaya gerçek zamanlı olarak döndürür. Gerçek zamanlı bitiş noktasına çağrı yapmak için, bitiş noktasını dağıttığınızda oluşturulan API anahtarını geçersiniz. Bitiş noktası REST, web programlama projeleri için popüler bir mimari seçim dayanmaktadır.

Gerçek zamanlı uç noktaların bir Azure Kubernetes Hizmet kümesine dağıtılması gerekir.

Modelinizi nasıl dağıtacağımıöğrenmek için [Bkz. Öğretici: Tasarımcıyla bir makine öğrenme modeli dağıtın.](tutorial-designer-automobile-price-deploy.md)

## <a name="publish"></a>Yayımlama

Bir **ardışık ardışık nokta bitiş noktasına bir ardışık hatlar**yayımlayabilirsiniz. Gerçek zamanlı bitiş noktasına benzer şekilde, bir ardışık nokta bitiş noktası, REST çağrılarını kullanarak harici uygulamalardan yeni ardışık ardışık ardışık çalışır göndermenize olanak tanır. Ancak, bir ardışık nokta bitiş noktasını kullanarak gerçek zamanlı olarak veri gönderemez veya alamazsınız.

Yayınlanan boru hatları esnektir, modelleri eğitmek veya yeniden eğitmek, [toplu çıkarTma yapmak,](how-to-run-batch-predictions-designer.md)yeni verileri işlemek ve çok daha fazlası için kullanılabilir. Birden çok ardışık ardışık ardışık ardışık noktayı tek bir ardışık nokta için yayımlayabilir ve hangi ardışık nokta sürümünün çalıştırılacak olduğunu belirtebilirsiniz.

Yayımlanmış bir ardışık işlem hattı, her modül için boru hattı taslağında tanımladığınız bilgi işlem kaynaklarında çalışır.

Tasarımcı, SDK ile aynı [PublishedPipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py) nesnesini oluşturur.


## <a name="moving-from-the-visual-interface-to-the-designer"></a>Görsel arabirimden tasarımcıya geçme

Görsel arabirim (önizleme) güncellendi ve artık Azure Machine Learning tasarımcısı (önizleme). Tasarımcı, Azure Machine Learning'in diğer özellikleriyle tam olarak bütünleşen boru hattı tabanlı bir arka uç kullanmak üzere yeniden yeniden tasarlandı. 

Bu güncelleştirmelerin bir sonucu olarak, görsel arabirimin bazı kavramları ve terimleri değiştirildi veya yeniden adlandırıldı. En önemli kavramsal değişiklikler için aşağıdaki tabloya bakın. 

| Tasarımcıda konsept | Görsel arabirimde daha önce |
| ---- |:----:|
| Boru hattı taslağı | Deneme |
| Gerçek zamanlı bitiş noktası | Web hizmeti |

### <a name="migrating-to-the-designer"></a>Tasarımcıya geçiş

Varolan görsel arabirim denemelerini ve web hizmetlerini tasarımcıdaki ardışık hatlara ve gerçek zamanlı uç noktalara dönüştürebilirsiniz. Görsel arabirim varlıklarınızı geçirmek için aşağıdaki adımları kullanın:

[!INCLUDE [migrate from the visual interface](../../includes/aml-vi-designer-migration.md)]


## <a name="next-steps"></a>Sonraki adımlar

* Öğretici ile tahmine dayalı analitik ve makine öğreniminin temellerini [öğrenin: Tasarımcıyla otomobil fiyatını tahmin edin](tutorial-designer-automobile-price-train-score.md)
* Varolan tasarımcı [örneklerini](samples-designer.md) gereksinimlerinize uyarlamak için nasıl değiştirip değiştirebilirsiniz.

