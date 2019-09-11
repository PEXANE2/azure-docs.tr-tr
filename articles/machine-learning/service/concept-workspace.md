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
ms.date: 08/06/2019
ms.openlocfilehash: ef060c083109a7096b62ebb35f2b49bb37a371b8
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860507"
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
    + Azure portal veya çalışma alanı giriş sayfanızdaki [otomatik makine öğrenimi denemeleri (Önizleme)](how-to-create-portal-experiments.md) bölümü (Önizleme).
    + [Görsel arabirim (Önizleme)](ui-concept-visual-interface.md).
+ İşlem [hatları](concept-azure-machine-learning-architecture.md#ml-pipelines) , modelinize eğitim ve yeniden eğitim için yeniden kullanılabilir iş akışlarıdır.
+ Veri [kümeleri](concept-azure-machine-learning-architecture.md#datasets-and-datastores) , model eğitimi ve işlem hattı oluşturma için kullandığınız verilerin yönetimine yardımcı olur.
+ Dağıtmak istediğiniz bir model olduktan sonra, kayıtlı bir model oluşturursunuz.
+ Bir [dağıtım](concept-azure-machine-learning-architecture.md#deployment)oluşturmak için kayıtlı modeli ve bir Puanlama betiğini kullanın.

## <a name="tools-for-workspace-interaction"></a>Çalışma alanı etkileşimi için Araçlar

Aşağıdaki yollarla çalışma alanım ile etkileşim kurabilirsiniz:

+ Web 'de:
    + [Azure portalı](https://portal.azure.com)
    + [Çalışma alanı giriş sayfanız (Önizleme)](https://ml.azure.com)
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

### <a name='create-workspace'></a>Çalışma alanı oluşturma

Çalışma alanı oluşturmanın birden çok yolu vardır.

* Her adımda size yol göstermek için bir nokta ve tıklama arabirimi için [Azure Portal](how-to-manage-workspace.md) kullanın.
* Python betikleri veya Jupiter not defterlerinden anında çalışma alanı oluşturmak için [Python için Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) kullanın
* Şirket güvenlik standartları ile oluşturma işlemini otomatik hale getirmeniz veya özelleştirmeniz gerektiğinde bir [Azure Resource Manager şablonu](how-to-create-workspace-template.md) veya [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md) kullanın.
* Visual Studio Code ' de çalışıyorsanız [vs Code uzantısını](how-to-vscode-tools.md#get-started-with-azure-machine-learning)kullanın.

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
+ [Çalışma Alanı oluşturma](how-to-manage-workspace.md)
+ [Çalışma Alanını Yönetme](how-to-manage-workspace.md)
+ [Öğretici: Model eğitme](tutorial-train-models-with-aml.md)
