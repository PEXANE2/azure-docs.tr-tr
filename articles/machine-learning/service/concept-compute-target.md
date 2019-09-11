---
title: 'İşlem hedefleri: modellerin eğmesi ve dağıtılması'
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile modelinizi eğmesini veya dağıtmayı istediğinizi belirleyin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/10/2019
ms.openlocfilehash: b2e4c9160ccad66b04dd30979f0568da68836d76
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860596"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde işlem hedefleri nelerdir? 

**İşlem hedefi** , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındırabileceğiniz, belirlenen bir işlem kaynağıdır/ortamıdır. Bu konum, yerel makineniz veya bulut tabanlı bir işlem kaynağıdır. İşlem hedeflerini kullanmak, kodunuzu değiştirmek zorunda kalmadan daha sonra işlem ortamınızı değiştirmenizi kolaylaştırır.  

Tipik bir model geliştirme yaşam döngüsünde şunları yapabilirsiniz:
1. Daha az miktarda veri geliştirme ve deneme ile başlayın. Bu aşamada, işlem hedefi olarak yerel ortamınızı (yerel bilgisayar veya bulut tabanlı VM) öneririz. 
2. Daha büyük verilere kadar ölçeklendirin ve bu [eğitim işlem hedeflerinden](#train)birini kullanarak dağıtılmış eğitim yapın.  
3. Modeliniz hazırlandıktan sonra, bu [dağıtım işlem hedeflerinin](#deploy)birine sahip bir Web barındırma ortamına veya IoT cihazına dağıtın.

İşlem hedeflerinizin kullanacağı işlem kaynakları bir [çalışma alanına](concept-workspace.md)eklenir. Yerel makine dışında işlem kaynakları, çalışma alanının kullanıcıları tarafından paylaşılır.

## <a name="train"></a>Eğitim işlem hedefleri

Azure Machine Learning hizmeti farklı işlem kaynakları üzerinde farklı destek içerir.  Ayrıca, çeşitli senaryolar için destek farklılık gösterebilse de kendi işlem kaynağınızı ekleyebilirsiniz.

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]

[Model eğitimi için bir işlem hedefi ayarlama ve kullanma](how-to-set-up-training-targets.md)hakkında daha fazla bilgi edinin.

## <a name="deploy"></a>Dağıtım hedefleri

Aşağıdaki işlem kaynakları, model dağıtımınızı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

[Modelinizin bir işlem hedefine dağıtımını nerede ve nasıl dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning işlem (yönetilen)

Yönetilen bir işlem kaynağı Azure Machine Learning hizmeti tarafından oluşturulur ve yönetilir. Bu işlem makine öğrenimi iş yükleri için iyileştirilmiştir. Azure Machine Learning Işlem, 30 Mayıs 2019 itibariyle yalnızca yönetilen işlem olur. Daha sonra ek yönetilen işlem kaynakları eklenebilir.

Eğitim için Azure Machine Learning Işlem ve toplu iş kullanımı (Önizleme) için kullanabilirsiniz.  Bu işlem kaynağıyla şunları yapabilirsiniz:

* Tek veya çok düğümlü küme
* Her çalıştırma gönderdiğinizde otomatik ölçekler 
* Otomatik küme yönetimi ve iş zamanlaması 
* Hem CPU hem de GPU kaynakları için destek

Azure portal veya [çalışma alanı giriş sayfanızda (Önizleme)](https://ml.azure.com), SDK Ile veya clı Ile Azure Machine Learning işlem örnekleri oluşturabilirsiniz. Oluşturulduğunda, diğer tür bilgi işlem hedeflerinin aksine, çalışma alanınızın otomatik olarak bir parçası olur.

## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen bir işlem hedefi Azure Machine Learning hizmeti tarafından *yönetilmez* . Bu tür bir işlem hedefini Azure Machine Learning dışında oluşturun ve ardından çalışma alanınıza ekleyin. Yönetilmeyen işlem kaynakları, makine öğrenimi iş yüklerinin performansını korumanız veya iyileştirebilmeniz için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Modelinizi eğitme için bir işlem hedefi ayarlama](how-to-set-up-training-targets.md)
* [Modelinizi bir işlem hedefine dağıtın](how-to-deploy-and-where.md)