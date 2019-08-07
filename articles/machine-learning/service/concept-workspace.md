---
title: Çalışma alanı nedir?
titleSuffix: Azure Machine Learning service
description: Çalışma alanı Azure Machine Learning hizmeti için en üst düzey kaynaktır. Günlükler, ölçümler, çıktılar ve betiklerinizin bir anlık görüntüsü dahil olmak üzere tüm eğitim çalıştırmaları geçmişini tutar. Hangi eğitim çalışmasının en iyi modeli ürettiğini öğrenmek için bu bilgileri kullanırsınız
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: fc3f88e627e0ce19195f6df947d4f11f5f8a73ae
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68772747"
---
# <a name="what-is-an-azure-machine-learning-service-workspace"></a>Azure Machine Learning hizmet çalışma alanı nedir?

Çalışma alanı, Azure Machine Learning hizmeti için en üst düzey kaynaktır ve Azure Machine Learning hizmeti kullandığınızda oluşturduğunuz tüm yapıtlarla birlikte çalışmak üzere merkezi bir yer sağlar.  Çalışma alanı; Günlükler, ölçümler, çıktılar ve betiklerinizin bir anlık görüntüsü dahil olmak üzere tüm eğitim çalıştırmaları geçmişini tutar. Hangi eğitim çalıştırmasının en iyi modeli ürettiğini öğrenmek için bu bilgileri kullanın.  

İstediğiniz bir modele sahip olduktan sonra, çalışma alanına kayıt yaptırmış olursunuz. Daha sonra Azure Container Instances, Azure Kubernetes hizmetine dağıtmak için kayıtlı model ve Puanlama betiklerini ve REST tabanlı HTTP uç noktası olarak bir alan ile programlanabilir kapı dizisine (FPGA) dağıtım yapmak için kullanılır. Modeli bir Azure IoT Edge cihazına modül olarak da dağıtabilirsiniz.

## <a name="taxonomy"></a>Sınıflandır 

Çalışma alanının bir sınıflandırması aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı sınıflandırma](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Diyagramda bir çalışma alanının aşağıdaki bileşenleri gösterilmektedir:

+ Bir çalışma alanı, Azure Machine Learning çalıştırmak için gereken Python ortamıyla yapılandırılmış bulut kaynakları ve [Not defteri VM 'leri](tutorial-1st-experiment-sdk-setup.md)içerebilir.
+ [Kullanıcı rolleri](how-to-assign-roles.md) , çalışma alanınızı diğer kullanıcılar, takımlar veya projelerle paylaşmanızı sağlar.
+ [İşlem hedefleri](concept-azure-machine-learning-architecture.md#compute-targets) , denemeleri hesabınızı çalıştırmak için kullanılır.
+ Çalışma alanını oluşturduğunuzda [ilişkili kaynaklar](#resources) da sizin için oluşturulur.
+ [Denemeleri](concept-azure-machine-learning-architecture.md#experiments) , modellerinizi oluşturmak için kullandığınız eğitim çalışmalardır.  İle denemeleri oluşturabilir ve çalıştırabilirsiniz
    + [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
    + Azure portal [Otomatik Machine Learning denemeleri (Önizleme)](how-to-create-portal-experiments.md) bölümü.
    + [Görsel arabirim (Önizleme)](ui-concept-visual-interface.md).
+ İşlem [hatları](concept-azure-machine-learning-architecture.md#ml-pipelines) , modelinize eğitim ve yeniden eğitim için yeniden kullanılabilir iş akışlarıdır.
+ Veri [kümeleri](concept-azure-machine-learning-architecture.md#datasets-and-datastores) , model eğitimi ve işlem hattı oluşturma için kullandığınız verilerin yönetimine yardımcı olur.
+ Dağıtmak istediğiniz bir model olduktan sonra, kayıtlı bir model oluşturursunuz.
+ Bir [dağıtım](concept-azure-machine-learning-architecture.md#deployment)oluşturmak için kayıtlı modeli ve bir Puanlama betiğini kullanın.

## <a name="tools-for-workspace-interaction"></a>Çalışma alanı etkileşimi için Araçlar

Aşağıdaki yollarla çalışma alanım ile etkileşim kurabilirsiniz:

+ Web 'de:
    + [Azure portalı](https://portal.azure.com)
    + [Görsel arabirim (Önizleme)](ui-concept-visual-interface.md)
+ Azure Machine Learning [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) kullanan Python 'da
+ Azure Machine Learning [CLI uzantısını](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli) kullanarak komut satırında

## <a name="machine-learning-with-a-workspace"></a>Çalışma alanıyla makine öğrenimi

Machine Learning görevleri çalışma alanınıza yapıları okur ve/veya yazar. 

+ Model yazma denemesi çalışma alanı için bir deneme çalıştırın.
+ Model yazma eğitim sonuçlarını çalışma alanına eğitme için otomatik ML kullanın.
+ Çalışma alanına bir model kaydettirin.
+ Model dağıtma-bir dağıtım oluşturmak için kayıtlı modeli kullanır.
+ Yeniden kullanılabilir iş akışları oluşturun ve çalıştırın.
+ Denemeleri, işlem hatları, modeller, dağıtımlar gibi makine öğrenimi yapılarını görüntüleyin.
+ Modelleri izleyin ve izleyin.

## <a name="workspace-management"></a>Çalışma alanı yönetimi

Ayrıca, aşağıdaki çalışma alanı yönetim görevlerini gerçekleştirebilirsiniz:

| Çalışma alanı yönetim görevi   | Portal              | SDK        | CLI        |
|---------------------------|------------------|------------|------------|
| Çalışma alanı oluşturma        | **&check;**     | **&check;** | **&check;** |
| İşlem kaynaklarını oluşturma ve yönetme    | **&check;**   | **&check;** |  **&check;**   |
| Çalışma alanı erişimini yönetme    | **&check;**   | |  **&check;**    |
| Not defteri VM 'si oluşturma | **&check;**   | |     |

[Çalışma alanı oluşturarak](setup-create-workspace.md)hizmeti kullanmaya başlayın.

## <a name="resources"></a>İlişkili kaynaklar

Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı tarafından kullanılan bazı Azure kaynakları otomatik olarak oluşturur:

+ [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Eğitim sırasında ve bir modeli dağıtırken kullandığınız Docker kapsayıcılarını kaydeder. Maliyetleri en aza indirmek için, dağıtım görüntüleri oluşturuluncaya kadar ACR **yavaş yüklenir** .
+ [Azure depolama hesabı](https://azure.microsoft.com/services/storage/): , Çalışma alanı için varsayılan veri deposu olarak kullanılır.  Not defteri sanal makineleriyle kullanılan Jupyter Not defterleri burada da depolanır.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): Modelleriniz hakkında izleme bilgilerini depolar.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): İşlem hedefleri ve çalışma alanı için gereken diğer hassas bilgiler tarafından kullanılan gizli dizileri depolar.

> [!NOTE]
> Yeni sürümler oluşturmaya ek olarak, var olan Azure hizmetlerini de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmetine başlamak için bkz.:

+ [Azure Machine Learning hizmete genel bakış](overview-what-is-azure-ml.md)
+ [Çalışma Alanı oluşturma](setup-create-workspace.md)
+ [Çalışma Alanını Yönetme](how-to-manage-workspace.md)
+ [Öğretici: Model eğitme](tutorial-train-models-with-aml.md)
