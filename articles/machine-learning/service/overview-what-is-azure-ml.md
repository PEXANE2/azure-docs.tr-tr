---
title: Nedir?
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning Service 'e genel bakış-profesyonel veri uzmanları için tümleşik, uçtan uca bir veri bilimi çözümü, bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmeye, denemeye ve dağıtmanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 08/05/2019
ms.custom: seodec18
ms.openlocfilehash: 7a9c0f24c7b6175b2289af39f2267d695894857d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855957"
---
# <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti, bulut tarafından sağlanan geniş ölçekte makine öğrenimi modellerini eğitmek, dağıtmak, otomatikleştirmek ve yönetmek için kullandığınız bir bulut hizmetidir.

## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Makine öğrenimini kullanarak bilgisayarlar açıkça programlamadan öğreniyor.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken makine öğrenimi, satın aldığınız ürüne göre istediğiniz diğer ürünleri önermenize yardımcı olur. Veya kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti, makine öğrenimi modellerini geliştirmek, test etmek, dağıtmak, yönetmek ve izlemek için kullanabileceğiniz bulut tabanlı bir ortam sağlar. Yerel makinenizde eğitime başlayın ve ardından buluta ölçeklendirin. Hizmet, PyTorch, TensorFlow ve scikit-öğrenme gibi açık kaynaklı teknolojileri tam olarak destekler ve klasik ml 'den derin öğrenme, denetimli ve denetlenmediğini öğrenme açısından her türlü makine öğrenimi için de kullanılabilir.

Verileri bulun ve hazırlayın, modelleri eğitme ve test edin ve şu gibi zengin araçları kullanarak dağıtın:
+ Denemeleri 'nizi derlemek ve ardından modeller dağıtmak için sürükleyip n bırakma modüllerini sürüklediğiniz bir [görsel arabirim](ui-tutorial-automobile-price-train-score.md)
+ [Bu örnek Not defterleri](https://aka.ms/aml-notebooks) gibi kendi kodunuzu yazmak Için [SDK 'Ları](https://docs.microsoft.com/azure/machine-learning/service/#reference) kullandığınız [jupyıter Not defterleri](https://jupyter.org)
+ [Visual Studio Code uzantısı](how-to-vscode-tools.md)

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile neler yapabilirim?

Açık kaynaklı Python paketleriyle <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Python SDK 'sını</a> kullanın veya bir Azure Machine Learning hizmetinde yüksek düzeyde doğru makine öğrenimi ve derin öğrenme modelleri derlemek ve eğmek için [görsel arabirimi (Önizleme)](ui-tutorial-automobile-price-train-score.md) kullanın Alanında.

Açık kaynaklı Python paketlerinde kullanılabilen, <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-öğren</a>, <a href="https://www.tensorflow.org" target="_blank">TensorFlow</a>, <a href="https://pytorch.org" target="_blank">Pytorch</a>ve <a href="https://mxnet.io" target="_blank">mxnet</a>gibi birçok makine öğrenimi bileşeni arasından seçim yapabilirsiniz.

Kodu yazarken veya görsel arabirimini kullandığınızda, en iyi çözümü bulmak ve dağıtılan modelleri yönetmek için denemeler yaparken birden çok çalıştırma izleyebilirsiniz.

### <a name="code-first-experience"></a>Kod ilk deneyim

<a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Python SDK 'sını</a> kullanarak yerel makinenizde eğitim başlatın ve ardından buluta ölçeklendirin. Azure Machine Learning Işlem ve [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)gibi birçok kullanılabilir [işlem hedefi](how-to-set-up-training-targets.md)ile ve [Gelişmiş hyperparameter ayarlama hizmetleriyle](how-to-tune-hyperparameters.md), bulutun gücünü kullanarak daha iyi modeller oluşturabilirsiniz.

SDK kullanarak [model eğitimi ve ayarlamayı otomatik hale](tutorial-auto-train-models.md) getirebilirsiniz.

### <a name="code-free--low-code-experience"></a>Kod-ücretsiz/düşük kod deneyimi

Kod ücretsiz eğitim için şunu deneyin:

+ Sürükleme-n bırakma deneme ve dağıtım için görsel arabirim

    ![Azure Machine Learning hizmeti için görsel arabirim](media/overview-what-is-azure-ml/visual-interface.png)

+ Otomatikml denemeleri için Azure portal seçeneği

### <a name="operationalization-mlops"></a>Operationalization (MLOps)

Doğru modele sahip olduğunuzda, bir Web hizmetinde, IoT cihazında veya Power BI aracılığıyla kolayca kullanabilirsiniz. Daha fazla bilgi için, [nasıl dağıtılacağı ve nerede yapılacağı](how-to-deploy-and-where.md)hakkındaki makaleye bakın.

Ardından, [Python için Azure MACHINE LEARNING SDK](https://aka.ms/aml-sdk) veya [Azure Portal](https://portal.azure.com/)kullanarak dağıtılan modellerinizi yönetebilirsiniz.

Bu modeller tüketilebilir ve [gerçek zamanlı](how-to-consume-web-service.md) veya [zaman uyumsuz](how-to-run-batch-predictions.md) olarak büyük miktarlarda veri alabilir.

Gelişmiş [makine öğrenimi ardışık düzenleri](concept-ml-pipelines.md)sayesinde, dağıtım aracılığıyla veri hazırlama, model eğitimi ve değerlendirmede her bir adımla işbirliği yapabilirsiniz. İşlem hatları şunları yapmanıza olanak sağlar:

* bulutta uçtan uca makine öğrenimi işlemini otomatikleştirin
* bileşenleri yeniden kullanın ve yalnızca gerektiğinde adımları yeniden çalıştırın
* her adımda farklı işlem kaynakları kullanın
* toplu Puanlama görevlerini Çalıştır

Azure Machine Learning hizmetini kullanmaya başlamak için bkz. [sonraki adımlar](#next-steps).

## <a name="how-does-azure-machine-learning-service-differ-from-studio"></a>Azure Machine Learning hizmeti Studio 'dan farklı midir?

[Machine Learning Studio](../studio/what-is-ml-studio.md) , kod yazmaya gerek kalmadan makine öğrenimi çözümlerini oluşturabileceğiniz, test ettiğiniz ve dağıtabileceğiniz, birlikte çalışan, sürükle ve bırak görsel çalışma alanıdır. Önceden oluşturulmuş ve önceden yapılandırılmış makine öğrenimi algoritmalarını ve veri işleme modüllerini, ayrıca özel bir işlem platformunu kullanır.

Azure Machine Learning hizmet, veri hızla geliştirmek, makine öğrenimi modellerini eğitmek ve dağıtmak Için hem SDK 'lar hem de bir görsel arabirim (Önizleme) sağlar. Bu görsel arabirim (Önizleme) Studio 'ya benzer bir sürükle ve bırak deneyimi sağlar. Ancak, Studio 'nun özel işlem platformunun aksine, görsel arabirim kendi işlem kaynaklarınızı kullanır ve Azure Machine Learning hizmeti ile tamamen tümleşiktir.

Hızlı bir karşılaştırma aşağıda verilmiştir.

|| Machine Learning Studio | Azure Machine Learning hizmeti:<br/>Görsel arabirim|
|---| --- | --- |
|| Genel olarak kullanılabilir (GA) | Önizleme aşamasında|
|Arabirim için modüller| Sayısı | İlk popüler modüller kümesi|
|Eğitim işlem hedefleri| Özel işlem hedefi, yalnızca CPU desteği| Azure Machine Learning işlem, GPU veya CPU 'yu destekler.<br/>(SDK 'da desteklenen diğer hesaplar)|
|Dağıtım işlem hedefleri| Özel Web hizmeti biçimi özelleştirilebilir değil | Azure Kubernetes hizmeti & Kurumsal güvenlik seçenekleri. <br/>(SDK 'da desteklenen[diğer hesaplar](how-to-deploy-and-where.md) ) |
|Otomatik model eğitimi ve hyperparameter ayarlaması | Hayır | Henüz görsel arabirimde değil. <br/> (SDK 'da desteklenir ve Azure portal.) |

Visual Interface 'i (Önizleme) öğreticiyle [deneyin: Görsel arabirim](ui-tutorial-automobile-price-train-score.md)ile otomobil fiyatını tahmin edin.

> [!NOTE]
> Studio 'da oluşturulan modeller Azure Machine Learning hizmeti tarafından dağıtılamaz veya yönetilemez. Ancak, Service Visual arabiriminde oluşturulan ve dağıtılan modeller Azure Machine Learning hizmet çalışma alanı aracılığıyla yönetilebilir.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez. Veya ücretli Azure hizmetleri için kullanabileceğiniz her ay kredileri veren [MSDN abone avantajlarını etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için [Machine Learning bir hizmet çalışma alanı oluşturun](how-to-manage-workspace.md) .

- Tam uzunluklu öğreticileri izleyin:
  + [Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)
  + [Regresyon modelini otomatik olarak eğitme için verileri hazırlayın ve otomatik makine öğrenimi kullanın](tutorial-data-prep.md)

- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](/azure/machine-learning/service/concept-ml-pipelines) hakkında bilgi edinin.

- Ayrıntılı okuma [Azure Machine Learning hizmeti mimarisi ve kavramları](concept-azure-machine-learning-architecture.md) makalesi.

- Daha fazla bilgi için bkz. [Microsoft 'un diğer makine öğrenimi ürünleri](/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning).
