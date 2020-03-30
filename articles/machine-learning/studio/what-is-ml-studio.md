---
title: ML Studio nedir (klasik)
titleSuffix: Azure
description: Azure Machine Learning Studio (klasik), kullanıma hazır algoritma ve modülkitaplığından modelleri hızlı bir şekilde oluşturmak için bir sürükle ve bırak aracıdır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371961"
---
# <a name="what-is-machine-learning-studio-classic"></a>Machine Learning Studio (klasik) nedir?

[!INCLUDE [Designer notice](../../../includes/designer-notice.md)]

Microsoft Azure Machine Learning Studio (klasik), verilerinizde tahmine dayalı analitik çözümleri oluşturmak, sınamak ve dağıtmak için kullanabileceğiniz ortak, sürükle ve bırak aracıdır. Azure Machine Learning Studio (klasik), özel uygulamalar veya Excel gibi BI araçları tarafından kolayca tüketilebilen web hizmetleri olarak modeller yayınlar.

Machine Learning Studio (klasik), veri bilimi, tahmine dayalı analitik, bulut kaynakları ve verilerinizin buluştuğu yerdir.

## <a name="the-machine-learning-studio-classic--interactive-workspace"></a>Machine Learning Studio (klasik) interaktif çalışma alanı

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Tahmine dayalı bir analiz modeli geliştirmek için genellikle bir veya daha fazla kaynaktan gelen verileri kullanır, bu verileri çeşitli veri işleme ve istatistiksel işlevler aracılığıyla dönüştürür ve analiz eder ve bir dizi sonuç oluşturursunuz. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

Azure Machine Learning Studio (klasik), tahmine dayalı bir analiz modeli nde kolayca oluşturmanız, test edilebindirmeniz ve yinelemeniz için etkileşimli, görsel bir çalışma alanı sağlar. ***Veri kümelerini*** ve analiz ***modüllerini*** etkileşimli bir tuvale sürükleyip atarsınız ve bunları machine learning studio 'da (klasik) çalıştırdığınız bir ***deney***oluşturmak için birbirine bağlarsınız. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazır olduğunuzda, ***eğitim denemenizi*** bir ***tahmine dayalı denemeye*** dönüştürebilir ve ardından modelinize başkaları tarafından erişilebilmesi için bunu bir ***web hizmeti*** olarak yayımlayabilirsiniz.

Tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlamak için programlama gerekmez.

![Azure Machine Learning Studio (klasik) diyagramı: Denemeler oluşturun, birçok kaynak için veri okuyun, puanlı veriler yazın, modeller yazın.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-machine-learning-studio-classic-overview-diagram"></a>Machine Learning Studio (klasik) genel bakış diyagramını indirin
Microsoft **Azure Machine Learning Studio (klasik) Yetenekleri Genel Bakış** diyagramını indirin ve Machine Learning Studio'nun (klasik) yeteneklerini üst düzey bir şekilde görüntüleyin. Diyagramı yakınınızda tutmak için tabloid boyutunda (11 x 17 inç) yazdırabilirsiniz.

**Diyagramı buradan indirin: [Microsoft Azure Machine Learning Studio (klasik) Yeteneklere Genel Bakış](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)**
![Microsoft Azure Machine Learning Studio (klasik) Yeteneklerine Genel Bakış](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)


## <a name="components-of-a-studio-classic--experiment"></a>Studio (klasik) deneyinbileşenleri
Bir deneme, tahmine dayalı bir modeli oluşturmak için birlikte bağladığınız analitik modüllere veri sağlayan veri kümelerinden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Denemenin en az bir veri kümesi ve bir modülü vardır
* Veri kümeleri yalnızca modüllere bağlanabilir
* Modüller veri kümelerine veya diğer modüllere bağlanabilir
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir
* Her bir modül için gereken tüm parametreler ayarlanmalıdır

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma](sample-experiments.md).

Deneme oluşturma örneği için bkz. [Azure Machine Learning Studio'da (klasik) basit bir deneme oluştur.](create-experiment.md)

Tahmine dayalı bir analitik çözümü oluşturmanın daha eksiksiz bir çözümü için [bkz.](tutorial-part1-credit-risk.md)

### <a name="datasets"></a>Veri kümeleri
Veri kümesi, modelleme işleminde kullanılabilmek için Machine Learning Studio'ya (klasik) yüklenen verilerdir. Denemeniz için Machine Learning Studio (klasik) ile bir dizi örnek veri kümesi dahildir ve ihtiyacınız olduğu şekilde daha fazla veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuvalin solunda bulunan veri kümeleri listesinden seçim yapabilirsiniz.

Machine Learning Studio'da [(klasik)](use-sample-datasets.md)yer alan örnek veri kümelerinin listesi için bkz.

### <a name="modules"></a>Modules
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Azure Machine Learning Studio (klasik), veri giriş işlevlerinden eğitim, puanlama ve doğrulama işlemlerine kadar çeşitli modüllere sahiptir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF'ye Dönüştürme][convert-to-arff] - Seri hale getirilmiş .NET veri kümesini Öznitelik-İlişki Dosyası Biçimi'ne (ARFF) dönüştürür.
* [Basit İstatistikleri Hesaplama][elementary-statistics] - Ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal Regresyon][linear-regression] - Çevrimiçi bir gradyan düşüşü tabanlı doğrusal regresyon modeli oluşturur.
* [Model Puanlama][score-model] - Eğitilmiş bir sınıflandırma veya regresyon modelini puanlar.

Bir deneme oluştururken, tuvalin solundaki modüller listesinden seçim yapabilirsiniz.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Mevcut büyük makine öğrenimi algoritmaları kitaplığında gezinmek için microsoft [Azure Machine Learning Studio (klasik) için algoritmaları nasıl seçeceğiniz bölümüne](algorithm-choice.md)bakın.

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analitik modeliniz hazır olduğunda, makine öğrenimi stüdyosundan (klasik) web hizmeti olarak dağıtabilirsiniz. Bu işlem hakkında daha fazla bilgi için [bkz.](deploy-a-machine-learning-web-service.md)

## <a name="next-steps"></a>Sonraki adımlar
[Öngörülü](create-experiment.md) analitik ve makine öğreniminin temellerini adım adım hızlı bir başlangıç yaparak ve [numuneler üzerine inşa](sample-experiments.md)ederek öğrenebilirsiniz.

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
