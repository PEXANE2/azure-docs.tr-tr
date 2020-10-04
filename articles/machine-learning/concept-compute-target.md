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
ms.date: 09/29/2020
ms.openlocfilehash: ca23bb49a3592dcc139bcc04875f3867018e158d
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91707752"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Azure Machine Learning işlem hedefleri nelerdir? 

**İşlem hedefi** , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındırabileceğiniz, belirlenen bir işlem kaynağıdır/ortamıdır. Bu konum, yerel makineniz veya bulut tabanlı bir işlem kaynağıdır. İşlem hedeflerini kullanmak, kodunuzu değiştirmek zorunda kalmadan daha sonra işlem ortamınızı değiştirmenizi kolaylaştırır.  

Tipik bir model geliştirme yaşam döngüsünde şunları yapabilirsiniz:
1. Daha az miktarda veri geliştirme ve deneme ile başlayın. Bu aşamada, işlem hedefi olarak yerel ortamınızı (yerel bilgisayar veya bulut tabanlı VM) öneririz. 
2. Daha büyük verilere kadar ölçeklendirin ve bu [eğitim işlem hedeflerinden](#train)birini kullanarak dağıtılmış eğitim yapın.  
3. Modeliniz hazırlandıktan sonra, bu [dağıtım işlem hedeflerinin](#deploy)birine sahip bir Web barındırma ortamına veya IoT cihazına dağıtın.

İşlem hedeflerinizin kullanacağı işlem kaynakları bir [çalışma alanına](concept-workspace.md)eklenir. Yerel makine dışında işlem kaynakları, çalışma alanının kullanıcıları tarafından paylaşılır.

## <a name="training-compute-targets"></a><a name="train"></a> Eğitim işlem hedefleri
Azure Machine Learning, farklı işlem hedefleri arasında değişen desteğe sahiptir. Tipik bir model geliştirme yaşam döngüsü, düşük miktarda veri üzerinde dev/deneme ile başlar. Bu aşamada, yerel bir ortam kullanmanızı öneririz. Örneğin, Yerel bilgisayarınız veya bulut tabanlı bir VM. Daha büyük veri kümelerinde eğitime göre ölçeklendirirken veya dağıtılmış eğitim yaparken, her çalıştırma gönderdiğinizde otomatik olarak ölçeklenen tek veya çok düğümlü bir küme oluşturmak için Azure Machine Learning Işlem kullanmanızı öneririz. Ayrıca, çeşitli senaryolar için destek aşağıda ayrıntılı şekilde değişiklik gösterebilse de kendi işlem kaynağınızı ekleyebilirsiniz:

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

[Bir eğitim çalıştırmasının bir işlem hedefine nasıl gönderileceği](how-to-set-up-training-targets.md)hakkında daha fazla bilgi edinin.

## <a name="compute-targets-for-inference"></a><a name="deploy"></a> Çıkarımı için işlem hedefleri

Aşağıdaki işlem kaynakları, model dağıtımınızı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

Çıkarımı gerçekleştirirken, Azure Machine Learning modeli ve onu kullanmak için gereken ilişkili kaynakları barındıran bir Docker kapsayıcısı oluşturur. Bu kapsayıcı daha sonra aşağıdaki dağıtım senaryolarından birinde kullanılır:

* Gerçek zamanlı çıkarım için kullanılan bir __Web hizmeti__ olarak. Web hizmeti dağıtımları aşağıdaki işlem hedeflerinden birini kullanır:

    * [Yerel bilgisayar](how-to-attach-compute-targets.md#local)
    * [Azure Machine Learning işlem örneği](how-to-create-manage-compute-instance.md)
    * [Azure Container Instances](how-to-attach-compute-targets.md#aci)
    * [Azure Kubernetes Services](how-to-create-attach-kubernetes.md)
    * Azure Işlevleri (Önizleme). Azure Işlevlerine dağıtım, yalnızca Docker kapsayıcısını oluşturmak için Azure Machine Learning kullanır. Buradan Azure Işlevleri kullanılarak dağıtılır. Daha fazla bilgi için bkz. [Azure işlevlerine makine öğrenimi modeli dağıtma (Önizleme)](how-to-deploy-functions.md).

* Düzenli aralıklarla veri toplu işlemleri işlemek için kullanılan bir __yığın çıkarım__ uç noktası olarak. Batch Inna, [Azure Machine Learning işlem kümesi](how-to-create-attach-compute-cluster.md)kullanır.

* Bir __IoT cihazına__ (Önizleme). IoT cihazına dağıtım, Docker kapsayıcısını oluşturmak için yalnızca Azure Machine Learning bağımlıdır. Buradan, Azure IoT Edge kullanılarak dağıtılır. Daha fazla bilgi için bkz. [IoT Edge modülü olarak dağıtma (Önizleme)](/azure/iot-edge/tutorial-deploy-machine-learning).

[Modelinizin bir işlem hedefine dağıtımını nerede ve nasıl dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning işlem (yönetilen)

Yönetilen bir işlem kaynağı Azure Machine Learning tarafından oluşturulur ve yönetilir. Bu işlem makine öğrenimi iş yükleri için iyileştirilmiştir. Azure Machine Learning işlem kümeleri ve [işlem örnekleri](concept-compute-instance.md) yalnızca yönetilen hesaplalardır. 

Azure Machine Learning işlem örnekleri veya işlem kümeleri oluşturabilirsiniz:
* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md)
* Python SDK 'Sı ve CLı:
    * [İşlem örneği](how-to-create-manage-compute-instance.md)
    * [İşlem kümesi](how-to-create-attach-compute-cluster.md)
* [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets) (Önizleme)
* Kaynak Yöneticisi şablonu. Örnek bir şablon için [create Azure Machine Learning işlem şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-amlcompute)bakın.
* [Azure CLI için](reference-azure-machine-learning-cli.md#resource-management)Machine Learning uzantısı.  

Bu işlem kaynakları oluşturulduğunda, diğer tür bilgi işlem hedeflerinin aksine, çalışma alanınızın otomatik olarak bir parçasıdır.


|Özellik  |İşlem kümesi  |İşlem örneği  |
|---------|---------|---------|
|Tek veya çok düğümlü küme     |    **&check;**       |         |
|Her çalıştırma gönderdiğinizde otomatik ölçekler     |     **&check;**      |         |
|Otomatik küme yönetimi ve iş zamanlaması     |   **&check;**        |     **&check;**      |
|Hem CPU hem de GPU kaynakları için destek     |  **&check;**         |    **&check;**       |


> [!NOTE]
> Bir işlem kümesi boşta olduğunda, otomatik olarak 0 düğüme ölçeklenirken, kullanımda olmadığında ödeme yapmayın.  Ancak, bir işlem *örneği*her zaman açık olur ve otomatik ölçeklendirme yapmaz.  Ek maliyetten kaçınmak için, bunu kullanmadığınız durumlarda [işlem örneğini durdurmanız](how-to-create-manage-compute-instance.md#manage) gerekir. 

### <a name="supported-vm-series-and-sizes"></a>Desteklenen VM Serisi ve boyutları

Azure Machine Learning yönetilen bir işlem kaynağı için bir düğüm boyutu seçtiğinizde, Azure 'da kullanılabilir olan VM boyutları arasından seçim yapabilirsiniz. Azure, farklı iş yükleri için Linux ve Windows için bir dizi boyut sunar. Farklı [VM türleri ve boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)hakkında daha fazla bilgi edinmek için buraya bakın.

VM boyutu seçmenin bazı özel durumları ve sınırlamaları vardır:
* Bazı VM serileri Azure Machine Learning desteklenmez.
* Bazı VM dizileri kısıtlıdır. Kısıtlı bir seriyi kullanmak için desteğe başvurun ve seri için bir kota artışı isteyin. Desteğe başvurma hakkında daha fazla bilgi için bkz. [Azure destek seçenekleri](https://azure.microsoft.com/support/options/)

Desteklenen seriler ve kısıtlamalar hakkında daha fazla bilgi edinmek için aşağıdaki tabloya bakın. 

| **Desteklenen VM Serisi**  | **Kısıtlamalar** |
|------------|------------|
| D | Hiçbiri |
| Dv2 | Hiçbiri |  
| Dv3 | Hiçbiri|
| DSv2 | Hiçbiri | 
| DSv3 | Hiçbiri|
| FSv2 | Hiçbiri | 
| HBv2 | Onay gerekiyor |  
| HCS | Onay gerekiyor |  
| M | Onay gerekiyor |
| NC | Hiçbiri |    
| NCsv2 | Onay gerekiyor |
| NCsv3 | Onay gerekiyor |  
| Üstündeki | Onay gerekiyor |
| NDv2 | Onay gerekiyor |
| NV | Hiçbiri |
| NVv3 | Onay gerekiyor | 


Azure Machine Learning bu VM serisini desteklese de, tüm Azure bölgelerinde bulunmayabilir. Burada kullanılabilir VM serisine bakabilirsiniz: [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

### <a name="compute-isolation"></a>İşlem yalıtımı

Azure Machine Learning Işlem, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar. Yalıtılmış sanal makine boyutları, diğer müşterilerin iş yüklerinden, Toplantı uyumluluk ve mevzuat gereksinimlerini kapsayan nedenlerle yüksek derecede yalıtım gerektiren iş yükleri için idealdir. Yalıtılmış bir boyut kullanılması, sanal makinenizin o belirli sunucu örneğinde çalışan tek bir tane olmasını garanti eder.

Geçerli yalıtılmış sanal makine teklifleri şunları içerir:
* Standard_M128ms
* Standard_F72s_v2
* Standard_NC24s_v3
* Standard_NC24rs_v3 *

*RDMA özellikli

[Azure genel bulutu 'Nda yalıtım](https://docs.microsoft.com/azure/security/fundamentals/isolation-choices)hakkında daha fazla bilgi edinmek için buraya bakın.

## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen bir işlem hedefi Azure Machine Learning *tarafından yönetilmiyor.* Bu tür bir işlem hedefini Azure Machine Learning dışında oluşturun ve ardından çalışma alanınıza ekleyin. Yönetilmeyen işlem kaynakları, makine öğrenimi iş yüklerinin performansını korumanız veya iyileştirebilmeniz için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Modelinizi eğitme bir işlem hedefi kullanın](how-to-set-up-training-targets.md)
* [Modelinizi bir işlem hedefine dağıtın](how-to-deploy-and-where.md)
