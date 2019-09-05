---
title: Machine Learning çalışma ekranına ne oldu?
titleSuffix: Azure Machine Learning service
description: Machine Learning çalışma ekranı uygulamasına ne olduğunu, Azure Machine Learning hizmette nelerin değiştirildiğini ve destek zaman çizelgesinin ne olduğunu öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 22f9ff8c493e850152e1fd74c8787e2cde507685
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308845"
---
# <a name="what-happened-to-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench’te neler oldu?

Azure Machine Learning Workbench uygulaması ve diğer bazı erken Özellikler kullanım dışı bırakılmıştır ve geliştirilmiş bir [mimarinin](concept-azure-machine-learning-architecture.md)yolu olması için Eylül 2018 sürümünde değiştirilmiştir.

Deneyim, deneyiminizi geliştirmek için müşteri geri bildirimi tarafından istenen birçok önemli güncelleştirme içerir. Deneme çalıştırmalarının temel işlevleri model dağıtımına göre değişmemiştir. Ancak şimdi, Machine Learning görevlerinizi ve işlem hatlarını gerçekleştirmek için sağlam <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a> ve [Azure CLI](reference-azure-machine-learning-cli.md) kullanabilirsiniz.

Azure Machine Learning hizmetinin önceki sürümünde oluşturulan yapıtların çoğu kendi yerel veya bulut depolama alanına depolanır. Bu yapıtlar hiçbir zaman kaybolmayacaktır.

Bu makalede, nelerin değiştiğini ve Azure Machine Learning Workbench ve API 'Leri ile önceden var olan çalışmanızı nasıl etkilediğini öğreneceksiniz.

>[!Warning]
>Bu makalede, Azure Machine Learning Studio kullanıcılar için değil. Workbench (Önizleme) uygulamasının yüklü olması ve/veya deneme ve model Yönetimi Önizleme hesapları Azure Machine Learning hizmeti müşteriler için var.


## <a name="what-changed"></a>Ne değişti?

Azure Machine Learning hizmetinin en son sürümü aşağıdaki özellikleri içerir:
+ [Basitleştirilmiş bir Azure kaynakları modeli](concept-azure-machine-learning-architecture.md).
+ Denemeleri ve işlem hedeflerinizi yönetmek için [Yeni bir Portal Kullanıcı arabirimi](how-to-track-experiments.md) .
+ Yeni, daha kapsamlı bir Python <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a>.
+ Machine Learning için yeni genişletilmiş [Azure CLI uzantısı](reference-azure-machine-learning-cli.md) .

[Mimari](concept-azure-machine-learning-architecture.md) kullanım kolaylığı için yeniden tasarlanmıştır. Birden çok Azure kaynağı ve hesabı yerine, size gereken yalnızca bir [Azure Machine Learning hizmeti Çalışma Alanı](concept-workspace.md)'dır. [Azure portalda](how-to-manage-workspace.md) hemen çalışma alanları oluşturabilirsiniz. Bir çalışma alanı kullanarak, birden çok Kullanıcı eğitim ve dağıtım işlem hedeflerini, model denemeleri, Docker görüntülerini, dağıtılan modelleri ve benzerlerini depolayabilirler.

Geçerli sürümde geliştirilmiş yeni CLı ve SDK istemcileri olsa da masaüstü çalışma ekranı uygulamasının kendisi kullanımdan kaldırılmıştır. Denemeleri, [Azure Portal çalışma alanı panosunda](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal)yönetilebilir. Deneme geçmişinizi almak, çalışma alanınıza bağlı işlem hedeflerini yönetmek, modellerinizi ve Docker görüntülerinizi yönetmek, hatta web hizmetlerini dağıtmak için panoyu kullanın.

<a name="timeline"></a>

## <a name="support-timeline"></a>Destek zaman çizelgesi

9 Ocak 'ta, Machine Learning çalışma ekranı, Azure Machine Learning Denemesi ve Model Yönetimi hesapları için 2019 desteği ve bunlarla ilişkili SDK ve CLı sona erdi.

En son tüm yetenekler, bu <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a>, [CLI](reference-azure-machine-learning-cli.md)ve [Portal](how-to-manage-workspace.md)kullanılarak kullanılabilir.

## <a name="what-about-run-histories"></a>Çalıştırma geçmişleri ne olacak?

Daha eski çalışma geçmişlerini artık erişilebilir değildir ve en son sürümde çalıştırmalarını nasıl görebilirsiniz.

Çalışma geçmişi artık **denemeleri**olarak adlandırılır. Modelinizin denemeleri toplayabilir ve SDK, CLı veya Azure portal kullanarak bunları keşfedebilirsiniz.

Portalın çalışma alanı panosu yalnızca Microsoft Edge, Chrome ve Firefox tarayıcılarında desteklenir:

[![Çevrimiçi portal](./media/overview-what-happened-to-workbench/image001.png)](./media/overview-what-happened-to-workbench/image001.png#lightbox)

Modellerinizde eğitim ve yeni CLI ve SDK'sını kullanarak çalıştırma geçmişleri izleme başlatın. Şunları öğrenebilirsiniz nasıl [öğretici: Azure Machine Learning hizmeti ile modellerinin eğitilmesi](tutorial-train-models-with-aml.md).

## <a name="can-i-still-prep-data"></a>Verileri yine hazırlayabilir miyim?

Önceden var olan veri hazırlama dosyalarınız, artık Machine Learning bir çalışma ekranına sahip olmadığınızdan en son sürüme taşınabilir değildir. Ancak yine de modelleme için herhangi bir boyut verisi kümesi hazırlayacaksınız.

Her boyuttaki veri kümeleri sayesinde, Python kodu yazarak verileri modellemeye başlamadan önce hızlı bir şekilde hazırlamak üzere [Azure Machine Learning için veri hazırlığı paketini](https://aka.ms/data-prep-sdk) kullanabilirsiniz.

## <a name="will-projects-persist"></a>Projeler kalacak mı?

Hiçbir kodu veya çalışmayı kaybetmeyeceksiniz. Eski sürümde projeler yerel dizini olan bulut varlıklarıydı. En son sürümde, yerel bir yapılandırma dosyası kullanarak Azure Machine Learning hizmeti çalışma alanına yerel dizinler iliştirerek. [En son mimarinin diyagramını](concept-azure-machine-learning-architecture.md)görüntüleyin.

Proje içeriğinin büyük bölümü zaten yerel makinenizde. Bu nedenle, çalışma alanınıza bağlanmak için bu dizinde bir yapılandırma dosyası oluşturmanız ve kodunuzda buna başvurmanız yeterlidir. Dosyalarınızı ve betikleri içeren yerel dizini kullanmaya devam etmek için, [' denemeler. Gönder '](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) Python komutunda veya `az ml project attach` CLI komutunu kullanarak dizinin adını belirtin.  Örneğin:
```python
run = exp.submit(source_directory=script_folder,
                 script='train.py', run_config=run_config_system_managed)
```

Başlamak için [bir çalışma alanı oluşturun](how-to-manage-workspace.md) .

## <a name="what-about-my-registered-models-and-images"></a>Kayıtlı modellerim ve görüntülerim hakkında ne olacak?

Eski model kayıt defterinizde kayıtlı modelleri, bunları kullanmaya devam etmek istiyorsanız yeni çalışma alanınıza geçirilmelidir. Modellerinizi geçirmek için modelleri indirin ve yeni çalışma alanınıza yeniden kaydedin.

Eski görüntü kayıt defterinizde oluşturduğunuz görüntüler yeni çalışma alanına doğrudan geçirilemez. Çoğu durumda, model, görüntü oluşturmaya gerek kalmadan dağıtılabilir. Gerekirse, yeni çalışma alanında model için bir görüntü oluşturabilirsiniz. Daha fazla bilgi için bkz. [makine öğrenimi modellerini yönetme, kaydetme, dağıtma ve izleme](concept-model-management-and-deployment.md).

## <a name="what-about-deployed-web-services"></a>Dağıtılan web hizmetlerine ne oldu?

Artık eski CLı desteği sona ermiştir, artık modelleri yeniden dağıtırsınız veya ilk olarak Model Yönetimi hesabınızla dağıttığınız Web hizmetlerini yönetebilirsiniz. Ancak, bu web hizmetleri için Azure Container Service (ACS) hala destekleniyor sürece çalışmaya devam eder.

En son sürümde modelleri web hizmetleri için Azure Container Instances'a (ACI) veya Azure Kubernetes Service (AKS) kümesi olarak dağıtılır. Ayrıca, FPGAs ve Azure IoT Edge için de dağıtabilirsiniz.

Bu makalelerde daha fazla bilgi edinin:
+ [Modellerin nasıl ve nasıl dağıtılacağı](how-to-deploy-and-where.md)
+ [Öğretici: Azure Machine Learning hizmeti ile modelleri dağıtma](tutorial-deploy-models-with-aml.md)

## <a name="what-about-the-old-sdk-and-cli"></a>Eski SDK ve CLı hakkında ne olacak?

Önceki [zaman çizelgesine](#timeline)bakın. Yeni denemeleri ve modellerinizi en son SDK veya CLı ile oluşturmaya başlayabilmeniz önerilir.

Yeni Python SDK 'sını en son sürümde kullanarak, herhangi bir Python ortamında Azure Machine Learning hizmetiyle etkileşime geçebilirsiniz. En son <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a>’yı yüklemeyi öğrenin. Ayrıca, Azure Cloud Shell dahil olmak üzere herhangi bir komut satırı ortamında hizmetle etkileşimde bulunmak `az ml` için, güncelleştirilmiş [Azure Machine Learning CLI uzantısını](reference-azure-machine-learning-cli.md) zengin komutlarla birlikte kullanabilirsiniz.

## <a name="what-about-visual-studio-code-tools-for-ai"></a>Ne Visual Studio Code Tools for AI?

Bu en son sürümde uzantı, Visual Studio Code için Azure Machine Learning olarak yeniden adlandırıldı ve genişletilmişti ve önceki yeni özelliklerle çalışacak şekilde geliştirildi.

[![Visual Studio Code için Azure Machine Learning](./media/overview-what-happened-to-workbench/vscode.png)](./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Etki alanı paketlerine ne oldu?

Bilgisayar görme, metin analizi ve tahmin için etki alanı paketleri Azure Machine Learning en son sürümüyle kullanılamaz. Ancak, en son Azure Machine Learning Python <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a>ile bilgisayar görme, metin ve tahmin modelleri oluşturup eğitmeye devam edebilirsiniz. Görüntü işleme, metin analizi ve tahmin paketleri kullanılarak oluşturulan önceden var olan modelleri nasıl geçirebileceğinizi öğrenmek için, iletişim kurun [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning hizmeti için en son mimari](concept-azure-machine-learning-architecture.md)hakkında bilgi edinin.

Hizmete genel bakış için [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)makalesini okuyun.

[Ortamı ve çalışma alanını ayarlamak](tutorial-1st-experiment-sdk-setup.md) ve [İlk modelinize eğmek](tutorial-1st-experiment-sdk-train.md) için iki bölümden oluşan öğreticide ilk denemenizi oluşturun

Bu iş akışı hakkında daha ayrıntılı bir deneyim için, Azure Machine Learning hizmeti ile modellerle dağıtım ve dağıtım için ayrıntılı adımlar içeren [tam uzunluklu öğreticiyi](tutorial-train-models-with-aml.md) izleyin.
