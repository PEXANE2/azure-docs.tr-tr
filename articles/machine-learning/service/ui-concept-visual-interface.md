---
title: Görsel arabirim
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için görsel arabirimi (Önizleme) oluşturan hüküm, kavramlar ve iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/15/2019
ms.openlocfilehash: f560887a48ce4754b26a54ef0e18093c5577af34
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71128817"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning"></a>Azure Machine Learning için görsel arabirim nedir? 

Azure Machine Learning için görsel arabirim (Önizleme), kod yazmadan, verileri dağıtmanıza, eğitmenize, test etmenize, dağıtmanıza, yönetmenize ve izlemenize olanak sağlar.

Programlama gerekmez, modelinizi oluşturmak için [veri kümelerini](#dataset) ve [modülleri](#module) görsel olarak bağlamanız gerekir.

Görsel arabirim, Azure Machine Learning [çalışma alanınızı](concept-workspace.md) şu amaçlarla kullanır:

+ [Deneme](#experiment) sürümü yapılarını çalışma alanına yazın.
+ [Veri kümelerine](#dataset)erişin.
+ Denemeyi çalıştırmak için çalışma alanındaki [işlem kaynaklarını](#compute) kullanın. 
+ [Modelleri](concept-azure-machine-learning-architecture.md#models)kaydedin.
+ Modelleri çalışma alanındaki işlem kaynaklarında Web Hizmetleri olarak [dağıtın](#deployment) .

![Görsel arabirime genel bakış](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>İş akışı

Görsel arabirim, bir modeli hızlıca oluşturmak, test etmek ve yinelemek için size etkileşimli ve görsel bir tuval sağlar. 

+ [Modülleri](#module) tuval üzerine sürükleyip bırakın.
+ Bir [deneme](#experiment)oluşturmak için modülleri birbirine bağlayın.
+ Machine Learning hizmeti çalışma alanının işlem kaynağını kullanarak denemeyi çalıştırın.
+ Denemeyi düzenleyerek ve yeniden çalıştırarak model tasarımınızda yineleme yapın.
+ Hazırsanız, **eğitim denemenizi** bir tahmine **dayalı deneyiye**dönüştürün.
+ Tahmine dayalı denemeyi bir Web hizmeti olarak [dağıtın](#deployment) , böylece modelinize başkaları tarafından erişilebilirler.

## <a name="experiment"></a>Deneme

Sıfırdan bir deneme oluşturun veya var olan bir örnek deneyni şablon olarak kullanın.  Her deneme çalıştırdığınızda yapıtlar çalışma alanınızda depolanır.

Bir deneme, bir model oluşturmak için birlikte bağlandığınız veri kümeleri ve analitik modüllerden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Veri kümeleri yalnızca modüllere bağlı olabilir.
* Modüller, veri kümelerine ya da diğer modüllere bağlı olabilir.
* Modüller için tüm giriş bağlantı noktalarında veri akışına bir bağlantı bulunmalıdır.
* Her modül için gerekli tüm parametrelerin ayarlanması gerekir.


Görsel arabirime nasıl başlacağınızı öğrenmek için bkz [. Öğretici: Görsel arabirim](ui-tutorial-automobile-price-train-score.md)ile otomobil fiyatını tahmin edin.

## <a name="dataset"></a>Veri kümesi

Veri kümesi, modelleme işleminde kullanılmak üzere görsel arabirime yüklenmiş olan bir veri kümesidir. ' İ denemek için bir dizi örnek veri kümesi bulunur ve ihtiyacınız olduğunda daha fazla veri kümesi yükleyebilirsiniz.

## <a name="module"></a>Modül

Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Görsel arabirimde, veri giriş işlevlerinden eğitim, Puanlama ve doğrulama işlemlerine kadar birçok modül bulunur.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki Özellikler bölmesinde görüntülenir. Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

![Modül özellikleri](media/ui-concept-visual-interface/properties.png)

Kullanılabilir makine öğrenimi algoritmalarının kitaplığı ile ilgili bazı yardım için bkz. [algoritma & modül başvurusuna genel bakış](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a>İşlem kaynakları

Denemenizi çalıştırmak veya dağıtılan modellerinizi Web Hizmetleri olarak barındırmak için çalışma alanınızdan işlem kaynaklarını kullanın. Desteklenen işlem hedefleri şunlardır:


| Hedef işlem | Eğitim | Dağıtım |
| ---- |:----:|:----:|
| Azure Machine Learning işlem | ✓ | |
| Azure Kubernetes Service | | ✓ |

İşlem hedefleri Machine Learning [çalışma alanınıza](concept-workspace.md)iliştirilir. İşlem hedeflerinizi [Azure Portal](https://portal.azure.com) çalışma alanınızda veya [çalışma alanınızın giriş sayfasında (Önizleme)](https://ml.azure.com)yönetirsiniz.

## <a name="deployment"></a>Dağıtım

Tahmine dayalı analiz modeliniz hazırlandıktan sonra, bunu görsel arabirimden bir Web hizmeti olarak dağıtabilirsiniz.

Web Hizmetleri, bir uygulama ile Puanlama modeliniz arasında bir arabirim sağlar. Bir dış uygulama, Puanlama modeliyle gerçek zamanlı olarak iletişim kurabilir. Bir Web hizmetine yapılan çağrı, bir dış uygulamaya tahmin sonuçları döndürür. Bir web hizmetine çağrı yapmak için web hizmeti dağıtılırken oluşturulan bir API anahtarını geçirirsiniz. Web hizmeti, Web programlama projeleri için popüler bir mimari seçimi olan REST 'i temel alır.

Modelinizi dağıtmayı öğrenmek için bkz [. Öğretici: Visual Interface](ui-tutorial-automobile-price-deploy.md)ile bir makine öğrenimi modeli dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

* Tahmine dayalı analiz ve makine öğrenimi [hakkında öğreticiyi öğrenin: Görsel arabirim ile otomobil fiyatını tahmin edin](ui-tutorial-automobile-price-train-score.md)
* Örneklerden birini kullanın ve gereksinimlerinize göre değiştirin:
    * [Örnek 1-gerileme: Tahmin fiyatı](how-to-ui-sample-regression-predict-automobile-price-basic.md)
    * [Örnek 2-gerileme: Fiyat ve karşılaştırma algoritmalarını tahmin etme](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Örnek 3-sınıflandırma: Kredi riskini tahmin etme](how-to-ui-sample-classification-predict-credit-risk-basic.md)
    * [Örnek 4-sınıflandırma: Kredi riskini tahmin etme (maliyet duyarlı)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Örnek 5-sınıflandırma: Dalgalanma, uygulama ve BT satışı tahmin etme](how-to-ui-sample-classification-predict-churn.md)
