---
title: Azure Machine Learning nedir?
description: Azure Machine Learning genel bakış profesyonel veri bilimcilerinin bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmeye, denemeye ve dağıtmaya yönelik tümleşik, uçtan uca bir veri bilimi çözümüdür.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 08/05/2019
ms.custom: seodec18
ms.openlocfilehash: 0a70b1c9d3ac888f0f77cf0f6e9ca37bc6cd4324
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "70999931"
---
# <a name="what-is-azure-machine-learning"></a>Azure Machine Learning nedir?

Azure Machine Learning, bulut tarafından sağlanan geniş ölçekte makine öğrenimi modellerini eğitmek, dağıtmak, otomatikleştirmek ve yönetmek için kullandığınız bir bulut hizmetidir.

## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Makine öğrenimini kullanarak bilgisayarlar açıkça programlamadan öğreniyor.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken makine öğrenimi, satın aldığınız ürüne göre istediğiniz diğer ürünleri önermenize yardımcı olur. Veya kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="what-is-azure-machine-learning"></a>Azure Machine Learning nedir?

Azure Machine Learning, makine öğrenimi modellerini geliştirmek, test etmek, dağıtmak, yönetmek ve izlemek için kullanabileceğiniz bulut tabanlı bir ortam sağlar. Yerel makinenizde eğitime başlayın ve ardından buluta ölçeklendirin. Hizmet, PyTorch, TensorFlow ve scikit-öğrenme gibi açık kaynaklı teknolojileri tam olarak destekler ve klasik ml 'den derin öğrenme, denetimli ve denetlenmediğini öğrenme açısından her türlü makine öğrenimi için de kullanılabilir.

Verileri bulun ve hazırlayın, modelleri eğitme ve test edin ve şu gibi zengin araçları kullanarak dağıtın:
+ Denemeleri 'nizi derlemek ve ardından modeller dağıtmak için sürükleyip n bırakma modüllerini sürüklediğiniz bir [görsel arabirim](ui-tutorial-automobile-price-train-score.md)
+ [Bu örnek Not defterleri](https://aka.ms/aml-notebooks) gibi kendi kodunuzu yazmak Için [SDK 'Ları](https://docs.microsoft.com/azure/machine-learning) kullandığınız [jupyıter Not defterleri](https://jupyter.org)
+ [Visual Studio Code uzantısı](how-to-vscode-tools.md)


> [!VIDEO https://channel9.msdn.com/Events/Connect/Microsoft-Connect--2018/D240/player]

## <a name="what-can-i-do-with-azure-machine-learning"></a>Azure Machine Learning ile ne yapabilirim?

Açık kaynaklı Python paketleriyle <a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">Azure Machine Learning Python SDK</a> 'yı kullanın veya bir Azure Machine Learning çalışma alanı yüksek düzeyde doğru makine öğrenimi ve derin öğrenme modelleri derlemek ve eğmek için [görsel arabirimi (Önizleme)](ui-tutorial-automobile-price-train-score.md) kullanın.

Açık kaynaklı Python paketlerinde kullanılabilen, <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-öğren</a>, <a href="https://www.tensorflow.org" target="_blank">TensorFlow</a>, <a href="https://pytorch.org" target="_blank">Pytorch</a>ve <a href="https://mxnet.io" target="_blank">mxnet</a>gibi birçok makine öğrenimi bileşeni arasından seçim yapabilirsiniz.

Kodu yazarken veya görsel arabirimini kullandığınızda, en iyi çözümü bulmak ve dağıtılan modelleri yönetmek için denemeler yaparken birden çok çalıştırma izleyebilirsiniz.

### <a name="code-first-experience"></a>Kod ilk deneyim

<a href="https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py" target="_blank">Azure Machine Learning Python SDK 'sını</a> kullanarak yerel makinenizde eğitim başlatın ve ardından buluta ölçeklendirin. Azure Machine Learning Işlem ve [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)gibi birçok kullanılabilir [işlem hedefi](how-to-set-up-training-targets.md)ile ve [Gelişmiş hyperparameter ayarlama hizmetleriyle](how-to-tune-hyperparameters.md), bulutun gücünü kullanarak daha iyi modeller oluşturabilirsiniz.

SDK kullanarak [model eğitimi ve ayarlamayı otomatik hale](tutorial-auto-train-models.md) getirebilirsiniz.

### <a name="ui-based-low-code-experience"></a>UI tabanlı, düşük kodlu deneyim

Kod ücretsiz eğitim için şunu deneyin:

+ Kullanımı kolay arabirimde [OTOMATIK ml denemeleri](tutorial-first-experiment-automated-ml.md) oluşturma.
+ [Görsel arabirimde Sürükle-n-Bırak deneme](ui-tutorial-automobile-price-train-score.md).
  ![Azure Machine Learning için görsel arabirim](media/overview-what-is-azure-ml/visual-interface.png)



### <a name="operationalization-mlops"></a>Operationalization (MLOps)

Doğru modele sahip olduğunuzda, bir Web hizmetinde, IoT cihazında veya Power BI aracılığıyla kolayca kullanabilirsiniz. Daha fazla bilgi için, [nasıl dağıtılacağı ve nerede yapılacağı](how-to-deploy-and-where.md)hakkındaki makaleye bakın.

Ardından, [Python için Azure MACHINE LEARNING SDK](https://aka.ms/aml-sdk), [Azure Portal](https://portal.azure.com/) veya [çalışma alanı giriş sayfanız (Önizleme)](https://ml.azure.com)kullanarak dağıtılan modellerinizi yönetebilirsiniz.

Bu modeller tüketilebilir ve [gerçek zamanlı](how-to-consume-web-service.md) veya [zaman uyumsuz](how-to-run-batch-predictions.md) olarak büyük miktarlarda veri alabilir.

Gelişmiş [makine öğrenimi ardışık düzenleri](concept-ml-pipelines.md)sayesinde, dağıtım aracılığıyla veri hazırlama, model eğitimi ve değerlendirmede her bir adımla işbirliği yapabilirsiniz. İşlem hatları şunları yapmanıza olanak sağlar:

* Bulutta uçtan uca makine öğrenimi işlemini otomatikleştirin
* Bileşenleri yeniden kullan ve yalnızca gerektiğinde adımları yeniden çalıştır
* Her adımda farklı işlem kaynakları kullanın
* Toplu Puanlama görevlerini Çalıştır

Azure Machine Learning kullanmaya başlamak için bkz. [sonraki adımlar](#next-steps).

## <a name="how-does-azure-machine-learning-differ-from-studio"></a>Azure Machine Learning Studio 'dan farklı midir?

[Machine Learning Studio](../studio/what-is-ml-studio.md) , kod yazmaya gerek kalmadan makine öğrenimi çözümlerini oluşturabileceğiniz, test ettiğiniz ve dağıtabileceğiniz, birlikte çalışan, sürükle ve bırak görsel çalışma alanıdır. Önceden oluşturulmuş ve önceden yapılandırılmış makine öğrenimi algoritmalarını ve veri işleme modüllerini, ayrıca özel bir işlem platformunu kullanır.

Azure Machine Learning, veri hızlı **bir şekilde** geliştirmek, makine öğrenimi modellerini eğitmek ve dağıtmak Için hem SDK 'lar hem de bir görsel arabirim (Önizleme) sağlar. Bu görsel arabirim (Önizleme) Studio 'ya benzer bir sürükle ve bırak deneyimi sağlar. Ancak, Studio 'nun özel işlem platformunun aksine, görsel arabirim kendi işlem kaynaklarınızı kullanır ve Azure Machine Learning ile tamamen tümleşiktir.

Hızlı bir karşılaştırma aşağıda verilmiştir.

|| Machine Learning Studio | Azure Machine Learning:<br/>Görsel arabirim|
|---| --- | --- |
|| Genel olarak kullanılabilir (GA) | Önizleme aşamasında|
|Arabirim için modüller| Sayısı | İlk popüler modüller kümesi|
|Eğitim işlem hedefleri| Özel işlem hedefi, yalnızca CPU desteği| Azure Machine Learning işlem, GPU veya CPU 'yu destekler.<br/>(SDK 'da desteklenen diğer hesaplar)|
|Dağıtım işlem hedefleri| Özel Web hizmeti biçimi özelleştirilebilir değil | Azure Kubernetes hizmeti & Kurumsal güvenlik seçenekleri. <br/>(SDK 'da desteklenen[diğer hesaplar](how-to-deploy-and-where.md) ) |
|Otomatik model eğitimi ve hyperparameter ayarlaması | Hayır | Henüz görsel arabirimde değil. <br/> (SDK ve Azure portal/Workspace giriş sayfasında desteklenir.) |

Visual Interface 'i (Önizleme) öğreticiyle [deneyin: Görsel arabirim](ui-tutorial-automobile-price-train-score.md)ile otomobil fiyatını tahmin edin.

> [!NOTE]
> Studio 'da oluşturulan modeller Azure Machine Learning tarafından dağıtılamaz veya yönetilemez. Ancak, hizmet görsel arabiriminde oluşturulan ve dağıtılan modeller Azure Machine Learning çalışma alanı aracılığıyla yönetilebilir.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez. Veya ücretli Azure hizmetleri için kullanabileceğiniz her ay [kredileri veren MSDN abone avantajlarını etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için [Machine Learning bir hizmet çalışma alanı oluşturun](how-to-manage-workspace.md) .

- Tam uzunluklu öğreticileri izleyin:
  + [Çalışma alanı oluşturma ve ilk ML modelinizi eğitme](tutorial-1st-experiment-sdk-setup.md)
  + [Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)


- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](/azure/machine-learning/service/concept-ml-pipelines) hakkında bilgi edinin.

- Derinlemesine [Azure Machine Learning mimarisi ve kavramlar](concept-azure-machine-learning-architecture.md) makalesini okuyun.

- Daha fazla bilgi için bkz. [Microsoft 'un diğer makine öğrenimi ürünleri](/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning).
