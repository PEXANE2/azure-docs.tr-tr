---
title: İşlem hedefleri nelerdir?
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modelinize eğmesini veya dağıtmayı istediğinizi tanımlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 03/30/2020
ms.openlocfilehash: ed65d69c18f2dbcd53324fe3cc18af8c51c546b2
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780122"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Azure Machine Learning işlem hedefleri nelerdir? 

**İşlem hedefi** , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındırabileceğiniz, belirlenen bir işlem kaynağıdır/ortamıdır. Bu konum, yerel makineniz veya bulut tabanlı bir işlem kaynağıdır. İşlem hedeflerini kullanmak, kodunuzu değiştirmek zorunda kalmadan daha sonra işlem ortamınızı değiştirmenizi kolaylaştırır.  

Tipik bir model geliştirme yaşam döngüsünde şunları yapabilirsiniz:
1. Daha az miktarda veri geliştirme ve deneme ile başlayın. Bu aşamada, işlem hedefi olarak yerel ortamınızı (yerel bilgisayar veya bulut tabanlı VM) öneririz. 
2. Daha büyük verilere kadar ölçeklendirin ve bu [eğitim işlem hedeflerinden](#train)birini kullanarak dağıtılmış eğitim yapın.  
3. Modeliniz hazırlandıktan sonra, bu [dağıtım işlem hedeflerinin](#deploy)birine sahip bir Web barındırma ortamına veya IoT cihazına dağıtın.

İşlem hedeflerinizin kullanacağı işlem kaynakları bir [çalışma alanına](concept-workspace.md)eklenir. Yerel makine dışında işlem kaynakları, çalışma alanının kullanıcıları tarafından paylaşılır.

## <a name="training-compute-targets"></a><a name="train"></a>Eğitim işlem hedefleri

Azure Machine Learning, farklı işlem kaynakları arasında farklı destek içerir.  Ayrıca, çeşitli senaryolar için destek farklılık gösterebilse de kendi işlem kaynağınızı ekleyebilirsiniz.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

[Model eğitimi için bir işlem hedefi ayarlama ve kullanma](how-to-set-up-training-targets.md)hakkında daha fazla bilgi edinin.

## <a name="deployment-targets"></a><a name="deploy"></a>Dağıtım hedefleri

Aşağıdaki işlem kaynakları, model dağıtımınızı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

[Modelinizin bir işlem hedefine dağıtımını nerede ve nasıl dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning işlem (yönetilen)

Yönetilen bir işlem kaynağı Azure Machine Learning tarafından oluşturulur ve yönetilir. Bu işlem makine öğrenimi iş yükleri için iyileştirilmiştir. Azure Machine Learning işlem kümeleri ve [işlem örnekleri](concept-compute-instance.md) yalnızca yönetilen hesaplalardır. Daha sonra ek yönetilen işlem kaynakları eklenebilir.

Şunları yapmak için Azure Machine Learning işlem örnekleri (Önizleme) veya işlem kümeleri oluşturabilirsiniz:
* Azure Machine Learning Studio
* Azure portal
* Python SDK [ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py) ve [amlcompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute(class)?view=azure-ml-py) sınıfları
* [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets)
* Resource Manager şablonu

Ayrıca [, Azure CLI için Machine Learning uzantısını](tutorial-train-deploy-model-cli.md#create-the-compute-target-for-training)kullanarak işlem kümeleri oluşturabilirsiniz.

Bu işlem kaynakları oluşturulduğunda, diğer tür bilgi işlem hedeflerinin aksine, çalışma alanınızın otomatik olarak bir parçasıdır.

### <a name="compute-clusters"></a>İşlem kümeleri

Eğitim için Azure Machine Learning işlem kümelerini ve toplu iş ınuzyı (Önizleme) için kullanabilirsiniz.  Bu işlem kaynağıyla şunları yapabilirsiniz:

* Tek veya çok düğümlü küme
* Her çalıştırma gönderdiğinizde otomatik ölçeklendirme 
* Otomatik küme yönetimi ve iş zamanlaması 
* Hem CPU hem de GPU kaynakları için destek

### <a name="supported-vm-series-and-sizes"></a>Desteklenen VM Serisi ve boyutları

Azure Machine Learning yönetilen bir işlem kaynağı için bir düğüm boyutu seçtiğinizde, Azure 'da kullanılabilir olan VM boyutları arasından seçim yapabilirsiniz. Azure, farklı iş yükleri için Linux ve Windows için bir dizi boyut sunar. Farklı [VM türleri ve boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)hakkında daha fazla bilgi edinmek için buraya bakın.

VM boyutu seçmenin bazı özel durumları ve sınırlamaları vardır:
* Bazı VM serileri Azure Machine Learning desteklenmez.
* Bazı VM dizileri kısıtlıdır. Kısıtlı bir seriyi kullanmak için desteğe başvurun ve seri için bir kota artışı isteyin. Desteğe başvurma hakkında daha fazla bilgi için bkz. [Azure destek seçenekleri](https://azure.microsoft.com/support/options/)

Desteklenen seriler ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın. 

| **Desteklenen VM Serisi**  | **Kısıtlamalar** |
|------------|------------|
| D | Yok |
| Dv2 | Yok |  
| DSv2 | Yok |  
| FSv2 | Yok |  
| M | Onay gerekiyor |
| NC | Yok |    
| NCsv2 | Onay gerekiyor |
| NCsv3 | Onay gerekiyor |  
| Üstündeki | Onay gerekiyor |
| NDv2 | Onay gerekiyor |
| NV | Yok |
| NVv3 | Onay gerekiyor | 


Azure Machine Learning bu VM serisini desteklese de, tüm Azure bölgelerinde bulunmayabilir. Burada kullanılabilir VM serisine bakabilirsiniz: [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen bir işlem hedefi Azure Machine Learning *tarafından yönetilmiyor.* Bu tür bir işlem hedefini Azure Machine Learning dışında oluşturun ve ardından çalışma alanınıza ekleyin. Yönetilmeyen işlem kaynakları, makine öğrenimi iş yüklerinin performansını korumanız veya iyileştirebilmeniz için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Modelinizi eğitme için bir işlem hedefi ayarlama](how-to-set-up-training-targets.md)
* [Modelinizi bir işlem hedefine dağıtın](how-to-deploy-and-where.md)
