---
title: ML Studio nedir (klasik)
titleSuffix: Azure
description: Azure Machine Learning Studio (klasik), bir dizi algoritmaların ve modüllerden kullanıma hazırlama kitaplığından hızlı bir şekilde model oluşturmaya yönelik bir sürükle ve bırak aracıdır.
services: machine-learning
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.topic: overview
ms.date: 03/24/2020
ms.openlocfilehash: c1772ceb514e46542129759711f2d45db39abf82
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80371961"
---
# <a name="what-is-machine-learning-studio-classic"></a>Machine Learning Studio (klasik) nedir?

[!INCLUDE [Designer notice](../../../includes/designer-notice.md)]

Microsoft Azure Machine Learning Studio (klasik), verilerinizde tahmine dayalı analiz çözümleri oluşturmak, test etmek ve dağıtmak için kullanabileceğiniz işbirliğine dayalı, sürükle ve bırak aracıdır. Azure Machine Learning Studio (klasik) modelleri, Excel gibi özel uygulamalar veya BI araçları tarafından kolayca tüketilen Web Hizmetleri olarak yayımlar.

Machine Learning Studio (klasik) veri bilimi, tahmine dayalı analiz, bulut kaynakları ve verilerinizin karşılabileceği yerdir.

## <a name="the-machine-learning-studio-classic--interactive-workspace"></a>Machine Learning Studio (klasik) etkileşimli çalışma alanı

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Tahmine dayalı bir analiz modeli geliştirmek için genellikle bir veya daha fazla kaynaktaki verileri kullanır, çeşitli veri işleme ve istatistiksel işlevler aracılığıyla bu verileri dönüştürür ve analiz eder ve bir sonuç kümesi oluşturur. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

Azure Machine Learning Studio (klasik), tahmine dayalı bir analiz modelini kolayca oluşturmak, test etmek ve yinelemek için size etkileşimli ve görsel bir çalışma alanı sağlar. ***Veri kümelerini*** ve analiz ***modüllerini*** etkileşimli bir tuvale sürükleyip bırakın ve Machine Learning Studio (klasik) ' de çalıştırdığınız bir ***deneme***oluşturmak için bunları birbirine bağlayarak. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazır olduğunuzda, ***eğitim denemenizi*** bir ***tahmine dayalı denemeye*** dönüştürebilir ve ardından modelinize başkaları tarafından erişilebilmesi için bunu bir ***web hizmeti*** olarak yayımlayabilirsiniz.

Programlama gerekmez, tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlayın.

![Azure Machine Learning Studio (klasik) diyagram: denemeleri oluşturma, çok sayıda kaynak için veri okuma, puanlanmış veriler yazma, yazma modelleri.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-machine-learning-studio-classic-overview-diagram"></a>Machine Learning Studio (klasik) genel bakış diyagramını indirin
**Microsoft Azure Machine Learning Studio (klasik) özelliklerine genel bakış** diyagramını indirin ve Machine Learning Studio (klasik) özelliklerine ilişkin üst düzey bir görünüm alın. Diyagramı yakınınızda tutmak için tabloid boyutunda (11 x 17 inç) yazdırabilirsiniz.

**Diyagramı buraya indirin: [Microsoft Azure Machine Learning Studio (klasik) yeteneklere genel bakış](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)**
![Microsoft Azure Machine Learning Studio (klasik) özelliklerine genel bakış](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)


## <a name="components-of-a-studio-classic--experiment"></a>Studio (klasik) deneme bileşenleri
Bir deneme, tahmine dayalı bir modeli oluşturmak için birlikte bağladığınız analitik modüllere veri sağlayan veri kümelerinden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Denemenin en az bir veri kümesi ve bir modülü vardır
* Veri kümeleri yalnızca modüllere bağlanabilir
* Modüller veri kümelerine veya diğer modüllere bağlanabilir
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir
* Her bir modül için gereken tüm parametreler ayarlanmalıdır

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma](sample-experiments.md).

Deneme oluşturma örneği için bkz. [Azure Machine Learning Studio basit bir deneme oluşturma (klasik)](create-experiment.md).

Tahmine dayalı analiz çözümü oluşturmaya yönelik daha kapsamlı bir anlatım için bkz. [Azure Machine Learning Studio (klasik) ile tahmine dayalı bir çözüm geliştirme](tutorial-part1-credit-risk.md).

### <a name="datasets"></a>Veri kümeleri
Veri kümesi, modelleme sürecinde kullanılabilmesi için Machine Learning Studio (klasik) karşıya yüklenmiş olan bir veri kümesidir. Deneme için Machine Learning Studio (klasik) bir dizi örnek veri kümesi bulunur ve ihtiyacınız olduğunda daha fazla veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuvalin solunda kullanılabilen veri kümeleri listesinden seçim yapabilirsiniz.

Machine Learning Studio (klasik) eklenen örnek veri kümelerinin bir listesi için bkz. [Azure Machine Learning Studio (klasik) örnek veri kümelerini kullanma](use-sample-datasets.md).

### <a name="modules"></a>Modüller
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Azure Machine Learning Studio (klasik), veri giriş işlevlerinden eğitim, Puanlama ve doğrulama işlemlerine kadar birçok modül içerir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF'ye Dönüştürme][convert-to-arff] - Seri hale getirilmiş .NET veri kümesini Öznitelik-İlişki Dosyası Biçimi'ne (ARFF) dönüştürür.
* [Basit İstatistikleri Hesaplama][elementary-statistics] - Ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal Regresyon][linear-regression] - Çevrimiçi bir gradyan düşüşü tabanlı doğrusal regresyon modeli oluşturur.
* [Model Puanlama][score-model] - Eğitilmiş bir sınıflandırma veya regresyon modelini puanlar.

Bir deneme oluştururken, tuvalin solunda bulunan modüllerin listesinden seçim yapabilirsiniz.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Kullanılabilir makine öğrenimi algoritmalarının büyük kitaplığındaki bazı yardım için bkz. [Microsoft Azure Machine Learning Studio algoritmaları seçme (klasik)](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analiz modeliniz hazırlandıktan sonra, bunu bir Web hizmeti olarak Machine Learning Studio (klasik) ile dağıtabilirsiniz. Bu işlemle ilgili daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti dağıtma](deploy-a-machine-learning-web-service.md).

## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı analiz ve makine öğrenimine ilişkin temel bilgileri, [adım adım hızlı başlangıç](create-experiment.md) ve [örnek oluşturma](sample-experiments.md)aracılığıyla öğrenebilirsiniz.

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
