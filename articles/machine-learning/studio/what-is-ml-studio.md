---
title: Nedir?
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio, bir dizi algoritmaların ve modüllerden kullanıma hazırlama kitaplığından hızlı bir şekilde model oluşturmaya yönelik bir sürükle ve bırak aracıdır.
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/20/2019
ms.openlocfilehash: ce1e5f349f55074b53cf447126c411a7a1cd3394
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68516921"
---
# <a name="what-is-azure-machine-learning-studio"></a>Azure Machine Learning Studio nedir?
Microsoft Azure Machine Learning Studio, verilerinizde tahmine dayalı analiz çözümleri oluşturma, test etme ve dağıtma amacıyla kullanabileceğiniz bir işbirliğine dayalı sürükle ve bırak aracıdır. Machine Learning Studio, modelleri özel uygulamalar veya Excel gibi BI araçları tarafından kolayca kullanılabilen web hizmetleri olarak yayımlar.

Machine Learning Studio, verilerinizin veri bilimi, tahmine dayalı analiz ve bulut kaynakları ile buluştuğu yerdir.


## <a name="the-machine-learning-studio-interactive-workspace"></a>Machine Learning Studio etkileşimli çalışma alanı
Tahmine dayalı bir analiz modeli geliştirmek için genellikle bir veya daha fazla kaynaktaki verileri kullanır, çeşitli veri işleme ve istatistiksel işlevler aracılığıyla bu verileri dönüştürür ve analiz eder ve bir sonuç kümesi oluşturur. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

**Azure Machine Learning Studio**, tahmine dayalı bir analiz modelini kolayca oluşturma, test etme ve yineleme amacıyla etkileşimli ve görsel bir çalışma alanı sunar. ***Veri kümelerini*** ve analiz ***modüllerini*** etkileşimli bir tuvale sürükleyip bırakır ve bunları birbirine bağlayarak Machine Learning Studio'da çalıştıracağınız bir ***deneme*** oluşturursunuz. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazır olduğunuzda, ***eğitim denemenizi*** bir ***tahmine dayalı denemeye*** dönüştürebilir ve ardından modelinize başkaları tarafından erişilebilmesi için bunu bir ***web hizmeti*** olarak yayımlayabilirsiniz.

Programlama gerekmez; tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlamanız yeterlidir.

![Azure Machine Learning Studio diyagramı: Denemeleri oluşturun, çok sayıda kaynak için verileri okuyun, puanlanmış verileri yazın, yazma modellerini yazın.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-machine-learning-studio-overview-diagram"></a>Machine Learning Studio'ya genel bakış diyagramını indirme
**Microsoft Azure Machine Learning Studio İşlevlerine Genel Bakış** diyagramını indirin ve Machine Learning Studio işlevlerine üst düzey bir genel bakış elde edin. Diyagramı yakınınızda tutmak için tabloid boyutunda (11 x 17 inç) yazdırabilirsiniz.

**Diyagramı buraya indirin: [Microsoft Azure Machine Learning Studio özelliklerine genel bakış](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)** 
MicrosoftAzureMachineLearningStudioözelliklerinegenel![bakış](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)

## <a name="get-started-with-machine-learning-studio"></a>Machine Learning Studio ile çalışmaya başlama
Machine Learning Studio,] ilk kez girdiğinizde (https://studio.azureml.net) **giriş** sayfasını görürsünüz. Buradan belgeleri, videoları, web seminerlerini görüntüleyebilir ve diğer değerli kaynakları bulabilirsiniz.

Sol üst menüye tıkladığınızda ![Menü](./media/what-is-ml-studio/menu.png) birkaç seçenek göreceksiniz.
### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
Burada iki seçenek vardır: Başladığınız sayfa olan **Ana Sayfa** ve **Studio**.

**Studio**'ya tıkladığınızda **Azure Machine Learning Studio** sayfası açılır. Öncelikle Microsoft hesabınızı veya iş ya da okul hesabınızı kullanarak oturum açmanız istenir. Oturum açtıktan sonra, solda şu sekmeleri görürsünüz:

* **PROJELER** - Tek bir projeyi temsil eden denemeler, veri kümeleri, not defterleri ve diğer kaynakların koleksiyonu
* **DENEMELER** - Oluşturup çalıştırdığınız veya taslak olarak kaydettiğiniz denemeler
* **WEB HİZMETLERİ** - Denemelerinizden dağıttığınız web hizmetleri
* **NOT DEFTERLERİ** - Oluşturduğunuz Jupyter not defterleri
* **VERİ KÜMELERİ** - Studio'ya yüklediğiniz veri kümeleri
* **EĞİTİLMİŞ MODELLER** - Denemelerde eğittiğiniz ve Studio'da kaydettiğiniz modeller
* **AYARLAR** - Hesabınızı ve kaynaklarınızı yapılandırmak için kullanabileceğiniz ayarlar koleksiyonu.

### <a name="gallery"></a>Galeri
**Galeri** sekmesine tıkladığınızda **[Azure AI Gallery](https://gallery.azure.ai/)** açılır. Galeri, bir veri bilimcileri ve geliştiricileri topluluğunun Cortana Intelligence Suite bileşenleri kullanılarak oluşturduğu çözümleri paylaştığı yerdir.

Galeri hakkında daha fazla bilgi için bkz. [Azure AI Gallery'de çözüm paylaşma ve keşfetme](gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Deneme bileşenleri
Bir deneme, tahmine dayalı bir modeli oluşturmak için birlikte bağladığınız analitik modüllere veri sağlayan veri kümelerinden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Denemenin en az bir veri kümesi ve bir modülü vardır
* Veri kümeleri yalnızca modüllere bağlanabilir
* Modüller veri kümelerine veya diğer modüllere bağlanabilir
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir
* Her bir modül için gereken tüm parametreler ayarlanmalıdır

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma](sample-experiments.md).

Basit bir deneme oluşturma örneği için bkz. [Azure Machine Learning Studio'da basit bir deneme oluşturma](create-experiment.md).

Tahmine dayalı analiz çözümü oluşturmaya yönelik daha kapsamlı bir anlatım için bkz. [Azure Machine Learning Studio ile tahmine dayalı bir çözüm geliştirme](tutorial-part1-credit-risk.md).

### <a name="datasets"></a>Veri kümeleri
Bir veri kümesi, model oluşturma işleminde kullanılabilmesi için Machine Learning Studio'ya yüklenen verilerdir. Machine Learning Studio'da deneme yapabileceğiniz birçok örnek veri kümesi bulunur ve ihtiyaç duyarsanız daha çok veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuvalin solunda kullanılabilen veri kümeleri listesinden seçim yapabilirsiniz.

Machine Learning Studio'ya dahil olan örnek veri kümelerinin listesi için bkz. [Azure Machine Learning Studio'daki örnek veri kümelerini kullanma](use-sample-datasets.md).

### <a name="modules"></a>Modüller
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Machine Learning Studio, veri alım işlevlerinden eğitim, puanlama ve doğrulama işlemlerine kadar değişiklik gösteren birçok modüle sahiptir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF 'e Dönüştür][convert-to-arff] -.net seri hale getirilmiş bir veri kümesini öznitelik Ilişkisi dosya biçimine dönüştürür (arff).
* [Temel Istatistikleri hesaplama][elementary-statistics] -ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal regresyon][linear-regression] -çevrimiçi bir gradyan tabanlı doğrusal regresyon modeli oluşturur.
* [Puan modeli][score-model] -eğitilmiş bir sınıflandırma veya regresyon modeli puanları.

Bir deneme oluştururken, tuvalin solundaki kullanılabilir modül listesinden seçebilirsiniz.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Kullanılabilir makine öğrenimi algoritmalarının büyük kitaplığındaki bazı yardım için bkz. [Microsoft Azure Machine Learning Studio algoritmaları seçme](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analiz modeliniz hazır olduktan sonra, bunu doğrudan Machine Learning Studio'dan bir web hizmeti olarak dağıtabilirsiniz. Bu işlem hakkında daha ayrıntılı bilgi için bkz. [Bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

<a name="compare"></a>
## <a name="how-is-machine-learning-studio-different-from-azure-machine-learning-service"></a>Azure Machine Learning hizmetinden farklı Machine Learning Studio?

[Azure Machine Learning hizmet](../service/overview-what-is-azure-ml.md) , veri hızla geliştirmek, makine öğrenimi modellerini eğitmek ve dağıtmak Için **hem SDK 'lar** hem de bir görsel arabirim (Önizleme) sağlar. Bu görsel arabirim (Önizleme) Studio 'ya benzer bir sürükle ve bırak deneyimi sağlar. Ancak, Studio 'nun özel işlem platformunun aksine, görsel arabirim kendi işlem kaynaklarınızı kullanır ve Azure Machine Learning hizmeti ile tamamen tümleşiktir.

Hızlı bir karşılaştırma aşağıda verilmiştir.

|| Machine Learning Studio | Azure Machine Learning hizmeti:<br/>Görsel arabirim|
|---| --- | --- |
|| Genel olarak kullanılabilir (GA) | Önizleme aşamasında|
|Arabirim için modüller| Sayısı | İlk popüler modüller kümesi|
|Eğitim işlem hedefleri| Özel işlem hedefi, yalnızca CPU desteği| Azure Machine Learning işlem, GPU veya CPU 'yu destekler.<br/>(SDK 'da desteklenen diğer hesaplar)|
|Dağıtım işlem hedefleri| Özel Web hizmeti biçimi özelleştirilebilir değil | Azure Kubernetes hizmeti & Kurumsal güvenlik seçenekleri. <br/>(SDK 'da desteklenen[diğer hesaplar](../service/how-to-deploy-and-where.md) ) |
|Otomatik model eğitimi ve hyperparameter ayarlaması | Hayır | Henüz görsel arabirimde değil. <br/> (SDK 'da desteklenir ve Azure portal.) | 

Hızlı başlangıç ile [görsel arabirimi (Önizleme) deneyin: Kodu yazmadan verileri hazırlayın ve görselleştirin](../service/ui-quickstart-run-experiment.md)

> [!NOTE]
> Studio 'da oluşturulan modeller Azure Machine Learning hizmeti tarafından dağıtılamaz veya yönetilemez. Ancak, Service Visual arabiriminde oluşturulan ve dağıtılan modeller Azure Machine Learning hizmet çalışma alanı aracılığıyla yönetilebilir.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı analiz ve makine öğrenimine ilişkin temel bilgileri, [adım adım hızlı başlangıç](create-experiment.md) ve [örnek oluşturma](sample-experiments.md)aracılığıyla öğrenebilirsiniz.

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
