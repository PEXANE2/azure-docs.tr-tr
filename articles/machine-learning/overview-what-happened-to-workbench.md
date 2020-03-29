---
title: Workbench'e ne oldu?
titleSuffix: Azure Machine Learning
description: Workbench uygulaması olmadan nelerin mümkün olduğu ve destek zaman çizelgesinin ne olduğu hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 03/05/2020
ms.openlocfilehash: 9b49eb6237346a76903202a118331383c5a8f623
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944248"
---
# <a name="what-happened-to-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench’te neler oldu?

Azure Machine Learning Workbench uygulaması ve diğer bazı erken özellikler, geliştirilmiş bir [mimarinin](concept-azure-machine-learning-architecture.md)önünü açmak için **Eylül 2018** sürümünde küçümsüldü ve değiştirildi.

Deneyiminizi geliştirmek için, sürüm müşteri geri bildirimi tarafından istenilen birçok önemli güncelleştirme içerir. Deneme çalıştıran lardan model dağıtımına kadar temel işlevsellik değişmedi. Ancak artık, makine öğrenimi görevlerinizi ve boru hatlarınızı gerçekleştirmek için sağlam <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">Python SDK,</a>R SDK ve [Azure CLI'yi](reference-azure-machine-learning-cli.md) kullanabilirsiniz.

Azure Machine Learning'in önceki sürümünde oluşturulan yapıtların çoğu kendi yerel veya bulut depolama alanınızda depolanır. Bu yapıtlar hiçbir zaman kaybolmayacaktır.

Bu makalede, azure machine learning workbench ve API'leri ile önceden varolan çalışmanızı nelerin değiştiği ve nasıl etkilediği hakkında bilgi edinin.

>[!Warning]
>Bu makale Azure Machine Learning Studio kullanıcıları için değildir. Workbench (önizleme) uygulamasını yükleyen ve/veya deneme ve model yönetimi önizleme hesapları olan Azure Machine Learning müşterileri içindir.


## <a name="what-changed"></a>Ne değişti?

Azure Machine Learning'in en son sürümü aşağıdaki özellikleri içerir:
+ [Basitleştirilmiş bir Azure kaynakları modeli.](concept-azure-machine-learning-architecture.md)
+ Deneylerinizi ve hesaplama hedeflerinizi yönetmek için yeni bir [portal UI.](how-to-track-experiments.md)
+ Yeni, daha kapsamlı Python <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a>.
+ Makine öğrenimi için yeni genişletilmiş [Azure CLI uzantısı.](reference-azure-machine-learning-cli.md)

[Mimari](concept-azure-machine-learning-architecture.md) kullanım kolaylığı için yeniden tasarlandı. Birden çok Azure kaynağı ve hesabı yerine, size gereken yalnızca bir [Azure Machine Learning Çalışma Alanı](concept-workspace.md)'dır. [Azure portalda](how-to-manage-workspace.md) hemen çalışma alanları oluşturabilirsiniz. Bir çalışma alanı kullanarak, birden çok kullanıcı eğitim ve dağıtım işlem hedeflerini, model denemelerini, Docker görüntülerini, dağıtılmış modelleri ve benzeri leri depolayabilir.

Geçerli sürümde yeni geliştirilmiş CLI ve SDK istemcileri olmasına rağmen, masaüstü tezgah uygulamasının kendisi kullanımdan kaldırılmıştır. Denemeler Azure Machine [Learning stüdyosundaki çalışma alanı panosunda](how-to-track-experiments.md#view-the-experiment-in-the-web-portal)yönetilebilir. Deneme geçmişinizi almak, çalışma alanınıza bağlı işlem hedeflerini yönetmek, modellerinizi ve Docker görüntülerinizi yönetmek, hatta web hizmetlerini dağıtmak için panoyu kullanın.

<a name="timeline"></a>

## <a name="support-timeline"></a>Destek zaman çizelgesi

9 Ocak 2019'da Machine Learning Workbench, Azure Machine Learning Experimentation ve Model Management hesapları ve bunların ilişkili SDK ve CLI hesapları için destek sona erdi.

Tüm son yetenekleri bu <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a>kullanarak kullanılabilir , [CLI](reference-azure-machine-learning-cli.md), ve [portal](how-to-manage-workspace.md).

## <a name="what-about-run-histories"></a>Çalıştırma geçmişleri ne olacak?

Eski çalıştırma geçmişlerine artık erişilemez, en son sürümde çalışanlarınızı nasıl görebilirsiniz.

Run geçmişleri artık **deneyler**olarak adlandırılır. SDK, CLI veya Azure Machine Learning stüdyosunu kullanarak modelinizin denemelerini toplayabilir ve bunları keşfedebilirsiniz.

Portalın çalışma alanı panosu yalnızca Microsoft Edge, Chrome ve Firefox tarayıcılarında desteklenir:

[![Online portal](./media/overview-what-happened-to-workbench/image001.png)](./media/overview-what-happened-to-workbench/image001.png#lightbox)

Yeni CLI ve SDK'yı kullanarak modellerinizi eğitmeye ve çalışma geçmişlerini izlemeye başlayın. Öğretici ile nasıl yapılacağını [öğrenebilirsiniz: Azure Machine Learning ile modelleri eğitin.](tutorial-train-models-with-aml.md)

## <a name="will-projects-persist"></a>Projeler kalacak mı?

Hiçbir kodu veya çalışmayı kaybetmeyeceksiniz. Eski sürümde projeler yerel dizini olan bulut varlıklarıydı. En son sürümde, yerel bir config dosyası kullanarak Azure Machine Learning çalışma alanına yerel dizinler eklersiniz. En [son mimarinin diyagramına](concept-azure-machine-learning-architecture.md)bakın.

Proje içeriğinin çoğu zaten yerel makinenizdeydi. Bu nedenle, çalışma alanınıza bağlanmak için bu dizinde bir config dosyası oluşturmanız ve kodunuzda gönderme yapmanız gerekir. Dosyalarınızı ve komut dosyalarınızı içeren yerel dizini kullanmaya devam etmek için, ['deneme.gönder'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) Python `az ml project attach` komutunda veya CLI komutunu kullanarak dizinin adını belirtin.  Örnek:
```python
run = exp.submit(source_directory=script_folder,
                 script='train.py', run_config=run_config_system_managed)
```

Başlamak için [bir çalışma alanı oluşturun.](how-to-manage-workspace.md)

## <a name="what-about-my-registered-models-and-images"></a>Peki ya kayıtlı modellerim ve resimlerim?

Eski model kayıt defterinizde kaydettiğiniz modellerin, kullanmaya devam etmek istiyorsanız yeni çalışma alanınıza geçirilmeniz gerekir. Modellerinizi geçirmek için modelleri indirin ve yeni çalışma alanınızda yeniden kaydedin.

Eski resim kayıt defterinizde oluşturduğunuz görüntüler doğrudan yeni çalışma alanına geçirilemez. Çoğu durumda, model bir görüntü oluşturmak zorunda kalmadan dağıtılabilir. Gerekirse, yeni çalışma alanında model için bir görüntü oluşturabilirsiniz. Daha fazla bilgi için bkz: [Makine öğrenimi modellerini yönet, kaydedin, dağıtın ve izleyin.](concept-model-management-and-deployment.md)

## <a name="what-about-deployed-web-services"></a>Dağıtılan web hizmetlerine ne oldu?

Eski CLI desteği sona erdiği için, modelleri artık yeniden dağıtamaz veya model yönetimi hesabınızla ilk olarak dağıttığınız web hizmetlerini yönetemezsiniz. Ancak, Azure Kapsayıcı Hizmeti (ACS) hala desteklendikçe bu web hizmetleri çalışmaya devam eder.

En son sürümde, modeller Web hizmetleri olarak Azure Kapsayıcı Örnekleri (ACI) veya Azure Kubernetes Service (AKS) kümelerine dağıtılır. FPGA'lara ve Azure IoT Edge'e de dağıtabilirsiniz.

Bu makalelerde daha fazla bilgi edinin:
+ [Modelleri nerede ve nasıl dağıtılır](how-to-deploy-and-where.md)
+ [Öğretici: Azure Machine Learning ile modelleri dağıtın](tutorial-deploy-models-with-aml.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning için en son mimari](concept-azure-machine-learning-architecture.md)hakkında bilgi edinin.

Hizmete genel bir bakış için [Azure Machine Learning nedir' okuyun.](overview-what-is-azure-ml.md)

Tercih ettiğiniz yöntemle ilk denemenizi oluşturun:
  + [Python not defterlerini kullanma](tutorial-1st-experiment-sdk-setup.md)
  + [R İşaretleme özelliğini kullanma](tutorial-1st-r-experiment.md) 
  + [Otomatik makine öğrenimini kullanma](tutorial-designer-automobile-price-train-score.md) 
  + [Tasarımcının sürükle & bırakma özelliklerini kullanma](tutorial-first-experiment-automated-ml.md) 
  + [CLI'nin ML uzantısını kullanma](tutorial-train-deploy-model-cli.md)
