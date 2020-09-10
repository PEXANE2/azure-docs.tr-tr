---
title: Azure Machine Learning nedir?
description: Azure Machine Learning genel bakış profesyonel veri bilimcilerinin bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmeye, denemeye ve dağıtmaya yönelik tümleşik, uçtan uca bir veri bilimi çözümüdür.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 11/04/2019
ms.custom: devx-track-python
ms.openlocfilehash: d5e1e8c0ced03191a530598bf2dca308dc4fab03
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651042"
---
# <a name="what-is-azure-machine-learning"></a>Azure Machine Learning nedir?

Bu makalede, ML modellerini eğmek, dağıtmak, otomatikleştirmek, yönetmek ve izlemek için kullanabileceğiniz bulut tabanlı bir ortam olan Azure Machine Learning hakkında bilgi edineceksiniz. 

Azure Machine Learning, klasik ml 'den derin öğrenme, denetimli ve denetimli öğrenime kadar her türlü makine öğrenimi için kullanılabilir. Bu SDK ile Python veya R kodu yazmayı veya [Studio](#build-ml-models-in-the-studio)'da kod içermeyen/düşük kod seçenekleriyle çalışmayı tercih etmeksizin, bir Azure Machine Learning çalışma alanı makine öğrenimi ve derin öğrenme modelleri oluşturabilir, eğitebilir ve izleyebilirsiniz. 

Yerel makinenizde eğitime başlayın ve ardından buluta ölçeklendirin. 

Hizmet Ayrıca, bilinen derin öğrenme ve pekiştirmeye dayalı açık kaynaklı araçlar ile PyTorch, TensorFlow, scikit-öğren ve Ray. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/Microsoft-Connect--2018/D240/player]

> [!Tip]
> **Ücretsiz deneme!**  Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin. Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.


## <a name="what-is-machine-learning"></a>Makine öğrenmesi nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Makine öğrenimini kullanarak bilgisayarlar açıkça programlamadan öğreniyor.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken makine öğrenimi, satın aldığınız ürüne göre istediğiniz diğer ürünleri önermenize yardımcı olur. Ya da kredi kartınızdan çekim yapıldığında makine öğrenmesi işlemi bir işlem veritabanıyla karşılaştırır ve dolandırıcılığın tespit edilmesine yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="machine-learning-tools-to-fit-each-task"></a>Her göreve uyacak makine öğrenimi araçları 

Azure Machine Learning, aşağıdakiler dahil olmak üzere makine öğrenimi iş akışları için gereken tüm araç geliştiricileri ve veri bilimcilerini sağlar:
+ [Azure Machine Learning Tasarımcısı](tutorial-designer-automobile-price-train-score.md) (Önizleme): denemeleri 'nizi derlemek ve sonra işlem hatları dağıtmak için sürükleyip n-Bırak modüllerini sürükleyin.

+ Jupyter Not defterleri: makinenizde öğrendiğimiz <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">Python örnekleri Için SDK</a> 'ümüzden yararlanmak üzere [örnek Not defterlerimizi](https://github.com/Azure/MachineLearningNotebooks) kullanın veya kendi not defterlerinizi oluşturun. 

+ Kendi kodunuzu yazmak için <a href="https://azure.github.io/azureml-sdk-for-r/reference/index.html" target="_blank">r Için SDK 'yı</a> kullandığınız r betikleri veya Not defterleri veya tasarımcıda r modüllerini kullanın.

+ + [Birçok model Çözüm Hızlandırıcısı](https://aka.ms/many-models) (önizleme) Azure Machine Learning oluşturur ve yüzlerce veya hatta binlerce makine öğrenimi modelini eğitmenize, çalıştırmanıza ve yönetmenize olanak sağlar.

+ [Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md)

+ [Machine learning CLI](reference-azure-machine-learning-cli.md)

+ PyTorch, TensorFlow ve scikit-öğrenme gibi açık kaynaklı çerçeveler ve çok daha fazlası

+ [Pekiştirmeye dayalı](how-to-use-reinforcement-learning.md) with Ray Rlrllib

Hatta, [ölçümleri izlemek Için Mlflow, modelleri](how-to-use-mlflow.md) veya Kubeflow kullanarak [uçtan uca iş akışı işlem hatları](https://www.kubeflow.org/docs/azure/)oluşturabilirsiniz.

## <a name="build-ml-models-in-python-or-r"></a>Python veya R 'de ML modelleri derleme

<a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">Python SDK</a> veya <a href="https://azure.github.io/azureml-sdk-for-r/reference/index.html" target="_blank">R SDK</a>Azure Machine Learning kullanarak yerel makinenizde eğitim başlatın. Daha sonra, buluta ölçeklendirebilirsiniz. 

Azure Machine Learning Işlem ve [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)gibi birçok kullanılabilir [işlem hedefi](how-to-create-attach-compute-sdk.md)ile ve [Gelişmiş hyperparameter ayarlama hizmetleriyle](how-to-tune-hyperparameters.md), bulutun gücünü kullanarak daha iyi modeller oluşturabilirsiniz.

SDK kullanarak [model eğitimi ve ayarlamayı otomatik hale](tutorial-auto-train-models.md) getirebilirsiniz.

## <a name="build-ml-models-in-the-studio"></a>Studio 'da ML modelleri derleme

[Azure Machine Learning Studio](https://studio.azureml.net) , model eğitimi, dağıtımı ve varlık yönetimi için düşük kod ve kod içermeyen seçenekler için Azure Machine Learning bir Web portalıdır. Studio, sorunsuz bir deneyim için Azure Machine Learning SDK ile tümleşir. Daha fazla bilgi için bkz. [Azure Machine Learning Studio nedir?](overview-what-is-machine-learning-studio.md).

+ **Azure Machine Learning Tasarımcısı (Önizleme)**

  [Tasarımcı](concept-designer.md) kullanarak makine öğrenimi modellerini herhangi bir kod yazmadan eğitme ve dağıtma. Başlamak için [Tasarımcı öğreticisini](tutorial-designer-automobile-price-train-score.md) deneyin. 

  ![Azure Machine Learning Tasarımcısı 'nın sürükle ve bırak arabiriminin animasyonlu gif 'i](media/concept-designer/designer-drag-and-drop.gif)

+ **Denemeleri izleme**

  Studio 'da [veri bilimi denemeleri nasıl izleneceğini ve görselleştirileyeceğinizi](tutorial-first-experiment-automated-ml.md) öğrenin. 

    ![Azure Machine Learning Studio 'da çalıştırma ayrıntıları](media/how-to-track-experiments/experimentation-tab.gif)


+ **Ve çok daha fazlası...**

  [Ml.Azure.com](https://studio.azureml.net)adresinde Azure Machine Learning Studio adresini ziyaret edin. 


## <a name="mlops-deploy--lifecycle-management"></a>MLOps: & yaşam döngüsü yönetimini dağıtma
Doğru modele sahip olduğunuzda, bir Web hizmetinde, IoT cihazında veya Power BI aracılığıyla kolayca kullanabilirsiniz. Daha fazla bilgi için, [nasıl dağıtılacağı ve nerede yapılacağı](how-to-deploy-and-where.md)hakkındaki makaleye bakın.

Daha sonra Python, [Azure Machine Learning Studio](https://ml.azure.com)veya [Machine Learning CLI](reference-azure-machine-learning-cli.md) [için Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py&preserve-view=true)kullanarak dağıtılan modellerinizi yönetebilirsiniz.

Bu modeller tüketilebilir ve [gerçek zamanlı](how-to-consume-web-service.md) veya [zaman uyumsuz](how-to-use-parallel-run-step.md) olarak büyük miktarlarda veri alabilir.

Gelişmiş [makine öğrenimi ardışık düzenleri](concept-ml-pipelines.md)sayesinde, dağıtım aracılığıyla veri hazırlama, model eğitimi ve değerlendirmede her bir adımla işbirliği yapabilirsiniz. İşlem hatları şunları yapmanıza olanak sağlar:

* Bulutta uçtan uca makine öğrenimi işlemini otomatikleştirin
* Bileşenleri yeniden kullan ve yalnızca gerektiğinde adımları yeniden çalıştır
* Her adımda farklı işlem kaynakları kullanın
* Toplu Puanlama görevlerini Çalıştır

Machine Learning iş akışınızı otomatik hale getirmek için betikleri kullanmak istiyorsanız, [Machine LEARNING CLI](reference-azure-machine-learning-cli.md) , bir eğitim gönderme veya model dağıtma gibi genel görevleri gerçekleştiren komut satırı araçları sağlar.

Azure Machine Learning kullanmaya başlamak için bkz. [sonraki adımlar](#next-steps).

## <a name="integration-with-other-services"></a>Diğer hizmetlerle tümleştirme

Azure Machine Learning Azure platformunda diğer hizmetlerle çalışır ve ayrıca git ve MLFlow gibi açık kaynak araçlarla tümleştirilir.

+ __Azure Kubernetes hizmeti__, __Azure Container Instances__, __Azure Databricks__, __Azure Data Lake Analytics__ve __Azure HDInsight__gibi işlem hedefleri. İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefleri nelerdir?](concept-compute-target.md).
+ __Azure Event Grid__. Daha fazla bilgi için bkz. [Azure Machine Learning olayları](concept-event-grid-integration.md)kullanma.
+ __Azure izleyici__. Daha fazla bilgi için bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).
+ __Azure depolama hesapları__, __Azure Data Lake Storage__, __Azure SQL veritabanı__, __PostgreSQL Için Azure veritabanı__ve __Azure açık veri kümeleri__gibi veri depoları. Daha fazla bilgi için bkz. Azure [Storage hizmetlerindeki verilere erişme](how-to-access-data.md) ve [Azure açık veri kümeleri Ile veri kümeleri oluşturma](how-to-create-register-datasets.md).
+ __Azure sanal ağları__. Daha fazla bilgi için bkz. [sanal ağdaki güvenli deneme ve çıkarım](how-to-enable-virtual-network.md).
+ __Azure Pipelines__. Daha fazla bilgi için bkz. [makine öğrenimi modellerini eğitme ve dağıtma](/azure/devops/pipelines/targets/azure-machine-learning).
+ __Git deposu günlükleri__. Daha fazla bilgi için bkz. [Git tümleştirmesi](concept-train-model-git-integration.md).
+ __Mlflow__. Daha fazla bilgi için bkz. [ölçümleri izlemek ve modelleri dağıtmak Için Mlflow](how-to-use-mlflow.md) 
+ __Kubeflow__. Daha fazla bilgi için bkz. [uçtan uca iş akışı işlem hatları oluşturma](https://www.kubeflow.org/docs/azure/).

### <a name="secure-communications"></a>Güvenli iletişim

Azure depolama hesabınız, işlem hedefleri ve diğer kaynaklarınız, modelleri eğitmek ve çıkarım gerçekleştirmek için bir sanal ağ içinde güvenli bir şekilde kullanılabilir. Daha fazla bilgi için bkz. [sanal ağdaki güvenli deneme ve çıkarım](how-to-enable-virtual-network.md).

## <a name="basic--enterprise-editions"></a><a name="sku"></a>Temel & Enterprise sürümleri

Azure Machine Learning, Machine Learning gereksinimleriniz için tasarlanmış iki sürüm sunar:
+ **Temel**: bir kod ilk deneyimiyle bulut ölçeğinde açık kaynaklı geliştirme için harika.

+ **Enterprise**: tüm temel ve bir Web arabirimi (Studio) + güvenli ve tüm beceri düzeyleri IÇIN kapsamlı ml yaşam döngüsü yönetimi.

Bu sürümler hakkında daha fazla bilgi edinmek ve hangi özellikleri kilitleyeceğinizi öğrenmek için, ["Azure Machine Learning sürümleri" makalesini](concept-editions.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- Tercih ettiğiniz yöntemle ilk denemenizi oluşturun:
  + [ML modellerini eğitmek & dağıtmak için Python not defterlerini kullanma](tutorial-1st-experiment-sdk-setup.md)
  + [ML modellerini eğitme & dağıtmak için R Markdown kullanma](tutorial-1st-r-experiment.md) 
  + [ML modellerini eğitmek & dağıtmak için otomatik makine öğrenimi kullanma](tutorial-first-experiment-automated-ml.md) 
  + [& dağıtmayı eğitme tasarımcı & bırakma yeteneklerini kullanın](tutorial-designer-automobile-price-train-score.md) 
  + [Makine öğrenimi CLı kullanarak bir modeli eğitme ve dağıtma](tutorial-train-deploy-model-cli.md)

- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](concept-ml-pipelines.md) hakkında bilgi edinin.

- Derinlemesine [Azure Machine Learning mimarisi ve kavramlar](concept-azure-machine-learning-architecture.md) makalesini okuyun.
