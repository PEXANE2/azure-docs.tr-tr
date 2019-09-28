---
title: Model eğitimi yöntemleri
titleSuffix: Azure Machine Learning
description: Modeli Azure Machine Learning ile eğitebilmeniz için kullanabileceğiniz farklı yöntemler hakkında bilgi edinin. Estimators, Scikit-öğrenme, TensorFlow, keras, PyTorch ve Chainer gibi popüler çerçevelerle çalışmak için kolay bir yol sağlar. Machine Learning işlem hatları, katılımsız çalıştırmaları zamanlamayı, heterojen işlem ortamlarını kullanmayı ve iş akışınızın parçalarını yeniden kullanmayı kolaylaştırır. Ve çalıştırma yapılandırması, eğitim sürecinin üzerinde çalıştığı işlem hedefleri üzerinde ayrıntılı denetim sağlar.
services: machine-learning
ms.service: machine-learning
author: Blackmist
ms.author: larryfr
ms.subservice: core
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: a755fe1607e581cb0a25eb9bd90c2ba223829a46
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350614"
---
# <a name="train-models-with-azure-machine-learning"></a>Azure Machine Learning ile eğitme modelleri

Azure Machine Learning, modellerinizi, otomatik makine öğrenimi ve görsel arabirim gibi düşük kod çözümlerine yönelik olarak SDK kullanarak kod ilk çözümlerinden eğitmek için çeşitli yollar sağlar. Size hangi eğitim yönteminin uygun olduğunu öğrenmek için aşağıdaki listeyi kullanın:

+ [Python IÇIN SDK Azure Machine Learning](#python-sdk): Python SDK, her biri farklı yeteneklere sahip modelleri eğitmek için çeşitli yollar sağlar.

    | Eğitim yöntemi | Açıklama |
    | ----- | ----- |
    | [Yapılandırmayı Çalıştır](#run-configuration) | **Modelleri eğitmek için genel bir yol** , bir eğitim betiği kullanmaktır ve yapılandırma çalıştırmanız. Çalıştırma yapılandırması, modelinizi eğitemak için kullanılan eğitim ortamını yapılandırmak için gereken bilgileri sağlar. Bir çalıştırma yapılandırması, eğitim betiğinizi ve bir işlem hedefini (eğitim ortamı) alabilir ve bir eğitim işi çalıştırabilirsiniz. |
    | [Otomatik makine öğrenimi](#automated-machine-learning) | Otomatikleştirilmiş makine **öğrenimi, kapsamlı veri bilimi veya programlama bilgisi olmadan modelleri eğmenize**olanak tanır. Veri bilimi ve programlama arka planına sahip kişiler için, algoritma seçimini ve hiper parametre ayarlamayı otomatikleştirerek zaman ve kaynak tasarrufu yapmak için bir yol sağlar. Otomatik makine öğrenimi kullanırken çalıştırma yapılandırması tanımlama konusunda endişelenmeniz gerekmez. |
    | [Tahmini](#estimators) | Estimator sınıfları, **popüler makine öğrenimi çerçeveleri temelinde modellerin eğmesini kolaylaştırır**. **Scikit-öğren**, **pytorch**, **TensorFlow**ve **Chainer**için tahmin aracı sınıfları vardır. Zaten adanmış bir tahmin aracı sınıfına sahip olmayan çerçevelerle kullanılabilen genel bir tahmin aracı da vardır. Estimators kullanırken bir çalıştırma yapılandırması tanımlama konusunda endişelenmeniz gerekmez. |
    | [Machine Learning işlem hattı](#machine-learning-pipeline) | İşlem hatları, farklı bir eğitim yöntemi değildir, ancak iş akışının bir parçası olarak eğitim içerebilen **modüler, yeniden kullanılabilir adımları kullanarak bir iş akışı tanımlamanın bir yoludur**. Makine öğrenimi ardışık düzenleri, otomatik makine öğrenimi, estimators kullanımı ve modelleri eğitmek için yapılandırma çalıştırma desteği sağlar. İşlem hatları eğitimlere özel olarak odaklanmadığından, işlem hattı kullanmanın nedenleri diğer eğitim yöntemlerinden daha farklılaştırılmıştır. Genellikle, şu durumlarda bir işlem hattı kullanabilirsiniz:<br>* Uzun süre çalışan eğitim işleri veya veri hazırlama gibi **Katılımsız işlemleri zamanlamak** istiyorsunuz.<br>* Heterojen işlem kaynakları ve depolama konumları genelinde koordine edilen **birden çok adım** kullanın.<br>* Yeniden eğitim veya toplu Puanlama gibi belirli senaryolar için işlem hattını yeniden **kullanılabilir bir şablon** olarak kullanın.<br>iş akışınız için * **izleme ve sürüm veri kaynakları, girişleri ve çıkışları** .<br>* İş akışınız, **belirli adımlarda bağımsız olarak çalışan farklı takımlar tarafından uygulanır**. Adımlar daha sonra iş akışını uygulamak için bir ardışık düzende birleştirilebilir. |

+ **Görsel arabirim**: Azure Machine Learning __görsel arabirimi__ , kavram kanıtı oluşturmak için veya çok az kodlama deneyimi olan kullanıcılar için makine öğrenimine kolay bir giriş noktası sağlar. Bir sürükle ve bırak Web tabanlı kullanıcı arabirimi kullanarak modelleri eğmenize olanak tanır. Python kodunu tasarımın bir parçası olarak kullanabilir veya herhangi bir kod yazmadan modelleri eğitebilirsiniz.

+ **CLI**: Machine Learning CLı, Azure Machine Learning ile yaygın görevlere yönelik komutlar sağlar ve genellikle **komut dosyası oluşturma ve otomatikleştirme işlemleri**için kullanılır. Örneğin, bir eğitim betiği veya işlem hattı oluşturduktan sonra, bir zamanlamaya göre çalışan bir eğitimi başlatmak veya eğitim için kullanılan veri dosyaları güncelleniyorsa, CLı 'yi kullanabilirsiniz. Eğitim modelleri için eğitim işleri gönderen komutları sağlar. Çalışma yapılandırması veya işlem hatları kullanarak işleri gönderebilir.

Bu eğitim yöntemlerinin her biri, eğitim için farklı türlerde işlem kaynakları kullanabilir. Toplu olarak, bu kaynaklara [__işlem hedefleri__](concept-azure-machine-learning-architecture.md#compute-targets)denir. İşlem hedefi bir yerel makine veya Azure Machine Learning Işlem, Azure HDInsight veya uzak bir sanal makine gibi bir bulut kaynağı olabilir.

## <a name="python-sdk"></a>Python SDK'sı

Python için Azure Machine Learning SDK, Azure Machine Learning ile makine öğrenimi iş akışları oluşturmanıza ve çalıştırmanıza olanak tanır. Etkileşimli bir Python oturumundan, Jupyıter not defterlerinden, Visual Studio Code veya başka bir IDE 'den hizmetle etkileşime geçebilirsiniz.

* [Python için Azure Machine Learning SDK nedir?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [SDK 'Yı yükler/güncelleştirir](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning için bir geliştirme ortamı yapılandırma](how-to-configure-environment.md)

### <a name="run-configuration"></a>Çalıştırma yapılandırma

Azure Machine Learning olan bir genel eğitim işi [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py)kullanılarak tanımlanabilir. Çalışma yapılandırması daha sonra, bir işlem hedefinde bir modeli eğitmek için eğitim betikleriyle birlikte kullanılır.

Yerel bilgisayarınız için bir çalıştırma yapılandırması ile başlayabilir ve ardından gerektiğinde bulut tabanlı bir işlem hedefi için birine geçiş yapabilirsiniz. İşlem hedefini değiştirirken yalnızca kullandığınız çalıştırma yapılandırmasını değiştirirsiniz. Bir çalıştırma Ayrıca, eğitim işiyle ilgili olarak girişler, çıktılar ve Günlükler gibi bilgileri de günlüğe kaydeder.

* [Çalışma yapılandırması nedir?](concept-azure-machine-learning-architecture.md#run-configurations)
* [Öğretici: İlk ML modelinizi eğitme @ no__t-0
* [Örnekleri: Eğitim modellerine Jupyter Notebook örnekleri @ no__t-0
* [Nasıl yapılır: Model eğitimi için işlem hedeflerini ayarlama ve kullanma @ no__t-0

### <a name="automated-machine-learning"></a>Otomatik Machine Learning

Yinelemeleri, hiper parametre ayarlarını, fealeştirme ve diğer ayarları tanımlayın. Eğitim sırasında Azure Machine Learning, paralel olarak farklı algoritmalar ve parametreler gerçekleştirmeye çalışır. Eğitim, tanımladığınız çıkış ölçütlerine ulaştıktan sonra duraklar. Estimators kullanırken bir çalıştırma yapılandırması tanımlama konusunda endişelenmeniz gerekmez.

> [!TIP]
> Python SDK 'ya ek olarak, [çalışma alanı giriş sayfası (Önizleme)](https://ml.azure.com)aracılığıyla otomatikleştirilmiş ml 'yi de kullanabilirsiniz.

* [Otomatik makine öğrenimi nedir?](concept-automated-ml.md)
* [Öğretici: Otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturma @ no__t-0
* [Öğretici: Otomatik makine öğrenimi kullanarak taksi Fares @ no__t-0 ' ı tahmin edin
* [Örnekleri: Otomatik makine öğrenimi için Jupyter Notebook örnekleri @ no__t-0
* [Nasıl yapılır: Python 'da otomatik ML denemeleri yapılandırma @ no__t-0
* [Nasıl yapılır: Zaman serisi tahmin modelini oto eğitme @ no__t-0
* [Nasıl yapılır: Azure Machine Learning çalışma alanı giriş sayfası (Önizleme @no__t) ile otomatik makine öğrenimi denemeleri oluşturma, araştırma ve dağıtma-0

### <a name="estimators"></a>Tahmini

Estimators, popüler ML çerçeveleri kullanarak modellerin eğmesini kolaylaştırır. **Scikit-öğren**, **pytorch**, **TensorFlow**veya **Chainer**kullanıyorsanız, eğitim için bir tahmin aracı kullanmayı göz önünde bulundurmanız gerekir. Zaten adanmış bir tahmin aracı sınıfına sahip olmayan çerçevelerle kullanılabilen genel bir tahmin aracı da vardır. Estimators kullanırken bir çalıştırma yapılandırması tanımlama konusunda endişelenmeniz gerekmez.

* [Estimators nedir?](concept-azure-machine-learning-architecture.md#estimators)
* [Öğretici: Veri ve scikit ile görüntü sınıflandırma modellerini eğitme Azure Machine Learning @ no__t-0 kullanarak öğrenin
* [Örnekleri: Jupyter Notebook estimators kullanma örnekleri @ no__t-0
* [Nasıl yapılır: Eğitim bölümünde tahmin oluşturma @ no__t-0

### <a name="machine-learning-pipeline"></a>Machine Learning işlem hattı

Machine Learning işlem hatları, daha önce bahsedilen eğitim yöntemlerini (çalıştırma yapılandırması, estimators ve otomatik makine öğrenimi) kullanabilir. İşlem hatları, bir iş akışı oluşturma hakkında daha fazla bilgi için yalnızca modellerin eğitimine kıyasla daha fazla bilgi alırlar. Bir işlem hattında, otomatik makine öğrenimi, estimators veya çalışma yapılandırması kullanarak bir modeli eğitebilirsiniz.

* [Azure Machine Learning ML işlem hatları nelerdir?](concept-ml-pipelines.md)
* [Azure Machine Learning SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)
* [Öğretici: Batch Puanlama için Azure Machine Learning işlem hatlarını kullanma @ no__t-0
* [Örnekleri: Machine Learning işlem hatları için Jupyter Notebook örnekleri @ no__t-0
* [Örnekleri: Otomatik makine öğrenimi ile işlem hattı @ no__t-0
* [Örnekleri: Estimators ile işlem hattı @ no__t-0

## <a name="visual-interface"></a>Görsel arabirim

Görsel Arabirim (Önizleme), Web tarayıcınızda bir sürükle ve bırak arabirimini kullanarak modelleri eğmenize olanak sağlar.

+ [Görsel arabirim nedir?](ui-concept-visual-interface.md)
+ [Öğreticisi: Otomobil ön fiyatını tahmin edin @ no__t-0
+ [Gerileme: Tahmin fiyatı](how-to-ui-sample-regression-predict-automobile-price-basic.md)
+ [Sınıflandırma: Kredi riskini tahmin etme](how-to-ui-sample-classification-predict-credit-risk-basic.md)
+ [Sınıflandırma: Dalgalanma, uygulama ve BT satışı tahmin etme](how-to-ui-sample-classification-predict-churn.md)

## <a name="cli"></a>CLI

Machine Learning CLı, Azure CLı için bir uzantıdır. Azure Machine Learning ile çalışmak için platformlar arası CLı komutları sağlar. Genellikle, bir makine öğrenimi modeline eğitim gibi görevleri otomatikleştirmek için CLı 'yi kullanırsınız.

* [Azure Machine Learning için CLı uzantısını kullanın](reference-azure-machine-learning-cli.md)
* [Azure üzerinde MLOps](https://github.com/microsoft/MLOps)

## <a name="next-steps"></a>Sonraki adımlar

[Eğitim ortamlarını ayarlamayı](how-to-set-up-training-targets.md)öğrenin.
