---
title: İşlem hedefleri nelerdir
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile modelinizi nerede eğitmek veya dağıtmak istediğinizi tanımlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 03/30/2020
ms.openlocfilehash: f9ca75943eaec2ae018b54145d872fc09294035e
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398173"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Azure Machine Learning'de bilgi işlem hedefleri nelerdir? 

**Bilgi işlem hedefi,** eğitim komut dosyanızı çalıştırdığınız veya hizmet dağıtımınızı barındırdığınız belirlenmiş bir bilgi işlem kaynağıdır/ortamıdır. Bu konum yerel makineniz veya bulut tabanlı bir bilgi işlem kaynağınız olabilir. İşlem hedeflerini kullanmak, kodunuzu değiştirmek zorunda kalmadan daha sonra işlem ortamınızı değiştirmenizi kolaylaştırır.  

Tipik bir model geliştirme yaşam döngüsünde şunları düşünebilirsiniz:
1. Az miktarda veri geliştirerek ve deneyyaparak başlayın. Bu aşamada, bilgi işlem hedefiniz olarak yerel ortamınızı (yerel bilgisayar veya bulut tabanlı VM) öneririz. 
2. Daha büyük verilere ölçeklendirin veya bu [eğitim bilgi işlem hedeflerinden](#train)birini kullanarak dağıtılmış eğitim yapın.  
3. Modeliniz hazır olduğunda, bu [dağıtım işlem hedeflerinden](#deploy)birine sahip bir web barındırma ortamına veya IoT aygıtına dağıtın.

İşlem hedefleriniz için kullandığınız işlem kaynakları bir [çalışma alanına](concept-workspace.md)eklenir. Yerel makine dışındaki hesaplama kaynakları çalışma alanının kullanıcıları tarafından paylaşılır.

## <a name="training-compute-targets"></a><a name="train"></a>Eğitim hesaplama hedefleri

Azure Machine Learning,farklı bilgi işlem kaynakları arasında farklı desteklere sahiptir.  Çeşitli senaryolar için destek değişebilir olsa da, kendi bilgi işlem kaynağınızı da ekleyebilirsiniz.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

[Model eğitimi için bir bilgi işlem hedefi oluşturma ve kullanma](how-to-set-up-training-targets.md)hakkında daha fazla bilgi edinin.

## <a name="deployment-targets"></a><a name="deploy"></a>Dağıtım hedefleri

Aşağıdaki işlem kaynakları, model dağıtımınızı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

[Modelinizi bir bilgi işlem hedefine nerede ve nasıl dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning bilgi işlem (yönetilen)

Yönetilen bir bilgi işlem kaynağı Oluşturulur ve Azure Machine Learning tarafından yönetilir. Bu bilgi işlem, makine öğrenimi iş yükleri için optimize edilebistir. Azure Machine Learning bilgi işlem kümeleri ve [bilgi işlem örnekleri](concept-compute-instance.md) yalnızca yönetilen hesaplamalardır. Gelecekte ek yönetilen bilgi işlem kaynakları eklenebilir.

Azure Machine Learning bilgi işlem örnekleri (önizleme) veya bilgi işlem kümeleri oluşturabilirsiniz:
* Azure Machine Learning Studio
* Azure portal
* Python SDK [ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py) ve [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute(class)?view=azure-ml-py) sınıfları
* [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets)
* Resource Manager şablonu

[Azure CLI için makine öğrenimi uzantısını](tutorial-train-deploy-model-cli.md#create-the-compute-target-for-training)kullanarak bilgi işlem kümeleri de oluşturabilirsiniz.

Bu hesaplama kaynakları oluşturulduğunda, diğer tür hesaplama hedeflerinin aksine otomatik olarak çalışma alanınızın bir parçasıdır.

### <a name="compute-clusters"></a>İşlem kümeleri

Eğitim ve toplu çıkarım (önizleme) için Azure Machine Learning bilgi işlem kümelerini kullanabilirsiniz.  Bu bilgi işlem kaynağı ile, var:

* Tek veya çok düğümlü küme
* Her çalıştırma gönderdiğinde otomatik ölçekler 
* Otomatik küme yönetimi ve iş planlaması 
* Hem CPU hem de GPU kaynakları için destek



## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen bir işlem hedefi Azure Machine Learning tarafından *yönetilmez.* Azure Machine Learning dışında bu tür bir bilgi işlem hedefi oluşturun ve çalışma alanınıza takın. Yönetilmeyen bilgi işlem kaynakları, makine öğrenimi iş yüklerinin performansını korumanız veya artırmanız için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Modelinizi eğitmek için bir işlem hedefi ayarlama](how-to-set-up-training-targets.md)
* [Modelinizi bir işlem hedefine dağıtma](how-to-deploy-and-where.md)