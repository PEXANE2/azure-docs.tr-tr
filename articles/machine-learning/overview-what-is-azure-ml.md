---
title: Azure Makine Öğrenimi Nedir
description: Azure Machine Learning'e Genel Bakış - Profesyonel veri bilimcilerin bulut ölçeğinde gelişmiş analitik uygulamaları geliştirmeleri, denemeleri ve dağıtmaları için entegre, uçtan uca veri bilimi çözümü.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 11/04/2019
ms.openlocfilehash: b8dbbb2810277bef20cb3b9b47a63deeea3e0ff9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241615"
---
# <a name="what-is-azure-machine-learning"></a>Azure Machine Learning nedir?

Bu makalede, ML modellerini eğitmek, dağıtmak, otomatikleştirmek, yönetmek ve izlemek için kullanabileceğiniz bulut tabanlı bir ortam olan Azure Machine Learning hakkında bilgi edinebilirsiniz. 

Azure Machine Learning, klasik ml'den derin öğrenmeye, denetimli ve denetimsiz öğrenime kadar her türlü makine öğrenimi için kullanılabilir. İster Python veya R kodu yazın ister [tasarımcı](tutorial-designer-automobile-price-train-score.md)gibi sıfır kodlu/düşük kod seçenekleri yazın, bir Azure Machine Learning Workspace'te son derece doğru makine öğrenimi ve derin öğrenme modelleri oluşturabilir, eğitebilir ve izleyebilirsiniz. 

Yerel makinenizde eğitime başlayın ve ardından buluta ölçeklendirin. 

Hizmet ayrıca PyTorch, TensorFlow ve scikit-learn gibi popüler açık kaynak araçları yla da çalışır.

> [!VIDEO https://channel9.msdn.com/Events/Connect/Microsoft-Connect--2018/D240/player]

> [!Tip]
> **Ücretsiz deneme!**  Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin. Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.


## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Bilgisayar, makine öğrenimini kullanarak açıkça programlanmadan öğrenirler.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken, makine öğrenimi satın aldığınız ürünlere bağlı olarak isteyebileceğiniz diğer ürünleri önermenize yardımcı olur. Veya kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="machine-learning-tools-to-fit-each-task"></a>Her göreve uyacak makine öğrenimi araçları 

Azure Machine Learning, geliştiricilerin ve veri bilimcilerin makine öğrenimi iş akışları için ihtiyaç duydukları tüm araçları sağlar:
+ [Azure Machine Learning tasarımcısı](tutorial-designer-automobile-price-train-score.md) (önizleme): denemelerinizi oluşturmak ve ardışık hatlar dağıtmak için sürükle-n-bırak modülleri.

+ Jupyter dizüstü bilgisayarlar: makine öğreniminiz için <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">SDK</a> for Python örneklerimizden yararlanmak için [örnek not defterlerimizi](https://aka.ms/aml-notebooks) kullanın veya kendi dizüstü bilgisayarlarınızı oluşturun. 

+ Kendi kodunuzu yazmak için R <a href="https://azure.github.io/azureml-sdk-for-r/reference/index.html" target="_blank">için SDK'yı</a> kullandığınız r komut dosyaları veya dizüstü bilgisayarlar veya tasarımcıdaki R modüllerini kullanın.

+ [Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md)

+ [Makine öğrenimi CLI](reference-azure-machine-learning-cli.md)

+ PyTorch, TensorFlow ve scikit-learn ve daha birçok açık kaynak çerçeveleri

Hatta ölçümleri izlemek ve uçlardan [uca iş akışı ardışık işler oluşturmak](https://www.kubeflow.org/docs/azure/)için modelleri veya Kubeflow dağıtmak için [MLflow](how-to-use-mlflow.md) kullanabilirsiniz.

## <a name="build-ml-models-in-python-or-r"></a>Python veya R'de ML modelleri oluşturun

Azure Machine Learning <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">Python SDK</a> veya R <a href="https://azure.github.io/azureml-sdk-for-r/reference/index.html" target="_blank">SDK'yı</a>kullanarak yerel makinenizde eğitime başlayın. Daha sonra, buluta ölçeklendirebilirsiniz. 

Azure Machine Learning Compute ve [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)gibi birçok kullanılabilir [bilgi işlem hedefi](how-to-set-up-training-targets.md)ve gelişmiş [hiperparametre aparatlama hizmetleriyle](how-to-tune-hyperparameters.md)bulutun gücünü kullanarak daha hızlı daha iyi modeller oluşturabilirsiniz.

Ayrıca SDK'yı kullanarak [model eğitimini ve atolmayı otomatikleştirebilirsiniz.](tutorial-auto-train-models.md)

## <a name="build-ml-models-with-no-code-tools"></a>Kod suz araçlarla ML modelleri oluşturun

Kodsuz veya düşük kodlu eğitim ve dağıtım için şunları deneyin:

+ **Azure Machine Learning tasarımcısı (önizleme)**

  Herhangi bir kod yazmadan verileri hazırlamak, eğitmek, test etmek, dağıtmak, yönetmek ve izlemek için tasarımcıyı kullanın. Programlama gerektirmez, modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlarsınız. [Tasarımcı öğretici](tutorial-designer-automobile-price-train-score.md)deneyin.

  [Azure Machine Learning tasarımcıgenel bakış makalesinde](concept-designer.md)daha fazla bilgi edinin. 

  ![Azure Machine Learning tasarımcı örneği](./media/overview-what-is-azure-ml/designer-drag-and-drop.gif)

+ **Otomatik makine öğrenme ui**

  Kullanımı kolay arabirimde [otomatik ML denemeleri](tutorial-first-experiment-automated-ml.md) oluşturmayı öğrenin. 

  [![Azure Machine Learning stüdyo navigasyon bölmesi](./media/overview-what-is-azure-ml/azure-machine-learning-automated-ml-ui.jpg)](./media/overview-what-is-azure-ml/azure-machine-learning-automated-ml-ui.jpg)

## <a name="mlops-deploy--lifecycle-management"></a>MLOps: & yaşam döngüsü yönetimi dağıtmak
Doğru modele sahip olduğunuzda, bir web hizmetinde, bir IoT aygıtında veya Power BI'den kolayca kullanabilirsiniz. Daha fazla bilgi için, [nasıl dağıtılanın ve nerede dağıtılanıncaya ilişkin makaleye](how-to-deploy-and-where.md)bakın.

Daha sonra [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk)kullanarak dağıtılan modelleri yönetebilirsiniz , Azure Machine Learning [stüdyo](https://ml.azure.com), veya [makine cli öğrenme](reference-azure-machine-learning-cli.md).

Bu modeller gerçek [zamanlı](how-to-consume-web-service.md) olarak veya büyük miktarlarda veri üzerinde eşzamanlı olarak tüketilebilir ve öngörüler [döndürebilir.](how-to-use-parallel-run-step.md)

Ve gelişmiş [makine öğrenimi boru hatları](concept-ml-pipelines.md)ile, dağıtım yoluyla veri hazırlama, model eğitimi ve değerlendirme her adımda işbirliği yapabilirsiniz. Boru hatları şunları sağlar:

* Bulutta uçdan uca makine öğrenimi işlemini otomatikleştirin
* Bileşenleri yeniden kullanın ve yalnızca gerektiğinde adımları yeniden çalıştırın
* Her adımda farklı işlem kaynakları kullanma
* Toplu puanlama görevlerini çalıştırma

Makine öğrenimi iş akışınızı otomatikleştirmek için komut dosyalarını kullanmak istiyorsanız, [cli öğrenen makine,](reference-azure-machine-learning-cli.md) bir eğitim çalışması gönderme veya bir model dağıtma gibi yaygın görevleri gerçekleştiren komut satırı araçları sağlar.

Azure Machine Learning'i kullanmaya başlamak için [Sonraki adımlara](#next-steps)bakın.

## <a name="integration-with-other-services"></a>Diğer hizmetlerle entegrasyon

Azure Machine Learning, Azure platformundaki diğer hizmetlerle çalışır ve Git ve MLFlow gibi açık kaynak araçlarıyla tümleşir.

+ __Azure Kubernetes Hizmeti,__ __Azure Kapsayıcı Örnekleri,__ __Azure Databricks,__ __Azure Veri Gölü Analitiği__ve __Azure HDInsight__gibi hesaplama hedefleri. Bilgi işlem hedefleri hakkında daha fazla bilgi için [bkz.](concept-compute-target.md)
+ __Azure Olay Tablosu__. Daha fazla bilgi için bkz. Azure [Machine Learning etkinliklerini tüket.](concept-event-grid-integration.md)
+ __Azure Monitör__. Daha fazla bilgi için bkz: [Azure Machine Learning'i İzleme.](monitor-azure-machine-learning.md)
+ Azure Depolama __hesapları,__ __Azure Veri Gölü Depolama,__ __Azure SQL Veritabanı,__ __PostgreSQL için Azure Veritabanı__ve Azure Açık Veri Kümeleri gibi veri __depoları.__ Daha fazla bilgi için Azure [depolama hizmetlerinde Access verilerine](how-to-access-data.md) bakın ve [Azure Açık Veri Kümeleri ile veri kümeleri oluşturun.](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets)
+ __Azure Sanal Ağlar__. Daha fazla bilgi için bkz: [Sanal ağdaki Güvenli deneme ve çıkarım.](how-to-enable-virtual-network.md)
+ __Azure Ardışık Hatları__. Daha fazla bilgi için [Train ve dağıtma makinesi öğrenme modelleri](/azure/devops/pipelines/targets/azure-machine-learning)ne bürünme bilgindir.
+ __Git deposu günlükleri.__ Daha fazla bilgi için [Git tümleştirmesi'ne](concept-train-model-git-integration.md)bakın.
+ __MLFlow__. Daha fazla bilgi için [ölçümleri izlemek ve modelleri dağıtmak için MLflow'a](how-to-use-mlflow.md) bakın 
+ __Kubeflow.__ Daha fazla bilgi için [uçtan uca iş akışı ardışık hatlar oluşturmaya](https://www.kubeflow.org/docs/azure/)bakın.

### <a name="secure-communications"></a>Güvenli iletişim

Azure Depolama hesabınız, bilgi işlem hedefleriniz ve diğer kaynaklarınız, modelleri eğitmek ve çıkarım gerçekleştirmek için sanal ağ içinde güvenli bir şekilde kullanılabilir. Daha fazla bilgi için bkz: [Sanal ağdaki Güvenli deneme ve çıkarım.](how-to-enable-virtual-network.md)

## <a name="basic--enterprise-editions"></a><a name="sku"></a>Temel & Enterprise sürümleri

Azure Machine Learning, makine öğrenimi gereksinimlerinize göre hazırlanmış iki sürüm sunar:
+ Temel (genellikle kullanılabilir)
+ Kurumsal (önizleme)

Bu sürümler, hangi makine öğrenim araçlarının geliştiriciler ve veri bilimciler tarafından çalışma alanından kullanılabillerini belirler.   

Temel çalışma alanları, Azure Machine Learning'i kullanmaya devam edebilmenize ve yalnızca makine öğrenimi işlemi sırasında tüketilen Azure kaynaklarıiçin ödeme yapmaya olanak tanır. Kurumsal sürüm çalışma alanları yalnızca azure tüketimi için ücretlendirilir ve sürüm önizlemededir. Azure Machine Learning sürümünde nelerin mevcut olduğu hakkında daha fazla bilgi edinin [& fiyatlandırma sayfasına genel bakış.](https://azure.microsoft.com/pricing/details/machine-learning/) 

Bir çalışma alanı oluşturduğunuzda sürümü atarsınız. Ayrıca, önceden varolan çalışma alanları sizin için Temel sürümüne dönüştürüldü. Temel sürüm, Ekim 2019 itibariyle genel olarak mevcut olan tüm özellikleri içerir. Enterprise edition özellikleri kullanılarak oluşturulmuş bu çalışma alanlarındaki tüm denemeler, siz Enterprise'a yükseltilene kadar salt okunur durumda olmaya devam edecektir. [Temel çalışma alanını Enterprise sürümüne](how-to-manage-workspace.md#upgrade)nasıl yükselteceklerini öğrenin. 

Müşteriler bu süre içinde bilgi işlem ve diğer Azure kaynaklarında oluşan maliyetlerden sorumludur.

## <a name="next-steps"></a>Sonraki adımlar

- Tercih ettiğiniz yöntemle ilk denemenizi oluşturun:
  + [ML modellerini eğitmek & dağıtmak için Python dizüstü bilgisayarları kullanın](tutorial-1st-experiment-sdk-setup.md)
  + [ML modellerini eğitmek & dağıtmak için R Markdown'ı kullanın](tutorial-1st-r-experiment.md) 
  + [ML modellerini eğitmek & dağıtmak için otomatik makine öğrenimini kullanın](tutorial-first-experiment-automated-ml.md) 
  + [Eğitmek & dağıtmak için tasarımcının sürükleme & bırakma özelliklerini kullanın](tutorial-designer-automobile-price-train-score.md) 
  + [Bir modeli eğitmek ve dağıtmak için CLI'yi öğrenen makineyi kullanın](tutorial-train-deploy-model-cli.md)

- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](concept-ml-pipelines.md) hakkında bilgi edinin.

- Ayrıntılı [Azure Machine Learning mimarisi ve kavramlar](concept-azure-machine-learning-architecture.md) makalesini okuyun.
