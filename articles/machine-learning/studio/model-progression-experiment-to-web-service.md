---
title: Bir model nasıl web hizmetine dönüşür?
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) modelinizin geliştirme deneyinden bir Web hizmetine nasıl ilerlediklerine genel bakış.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 03/20/2017
ms.openlocfilehash: e6a8fe7771776a92b3c28a188bc352e3bb105313
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217913"
---
# <a name="how-a-machine-learning-studio-classic-model-progresses-from-an-experiment-to-a-web-service"></a>Machine Learning Studio (klasik) modeli bir denemeden Web hizmetine nasıl ilerler?

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
Azure Machine Learning Studio (klasik), tahmine dayalı bir analiz modelini temsil eden bir ***deneme*** geliştirmenize, çalıştırmanıza, test etmenizi ve yinelemenizi sağlayan etkileşimli bir tuval sağlar. Şu nakadar çok çeşitli modüller mevcuttur:

* Denemenize veri girişi
* Verileri işleme
* Makine öğrenme algoritmalarını kullanarak bir model eğitin
* Modeli puanlama
* Sonuçları değerlendirme
* Çıktı son değerleri

Denemenizden memnun kaldıktan sonra, kullanıcıların yeni veriler göndermesi ve sonuçları geri alabilmesi için bunu ***Klasik Azure Machine Learning Web hizmeti*** veya Yeni Azure Machine Learning Web ***hizmeti*** olarak dağıtabilirsiniz.

Bu makalede, Machine Learning modelinizin bir geliştirme deneyinden operasyonel bir Web hizmetine nasıl ilerlediklerine genel bir bakış sayılmı veriyoruz.

> [!NOTE]
> Makine öğrenimi modellerini geliştirmenin ve dağıtmanın başka yolları da vardır, ancak bu makale, Machine Learning Studio'yı (klasik) nasıl kullandığınıza odaklanmıştır. Örneğin, R ile klasik bir tahmine dayalı Web hizmeti oluşturmanın açıklamasını okumak için, [RStudio ve Azure Machine Learning stüdyosunu kullanarak Tahmine Dayalı Web Uygulamalarını Dağıtma& blog](https://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx)gönderisine bakın.
>
>

Azure Machine Learning Studio (klasik), *bir tahmine dayalı analiz modeli*geliştirmenize ve dağıtmanıza yardımcı olmak üzere tasarlanmış olsa da, tahmine dayalı bir analiz modeli içermeyen bir deneme geliştirmek için Studio 'yı (klasik) kullanmak mümkündür. Örneğin, bir deneme yalnızca veri girişi yapabilir, onu işleyebilir ve sonra sonuçları çıktıalabilir. Öngörülü bir analiz denemesi gibi, bu tahmine dayalı olmayan deneyi bir Web hizmeti olarak dağıtabilirsiniz, ancak deneme bir makine öğrenme modelini eğitmediği veya puanlamadığı için bu daha basit bir işlemdir. Bu şekilde Studio (klasik) kullanmak için tipik olmasa da, biz Studio (klasik) nasıl çalıştığını tam bir açıklama verebilir böylece tartışmaya dahil edeceğiz.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Tahmine dayalı bir Web hizmeti geliştirme ve dağıtma
Machine Learning Studio (klasik) kullanarak geliştirip dağıtırken tipik bir çözümün izlediği aşamalar şunlardır:

![Dağıtım akışı](./media/model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Şekil 1 - Tipik bir tahmin analizi modelinin aşamaları*

### <a name="the-training-experiment"></a>Eğitim deneyi
***Eğitim deneyi,*** Machine Learning Studio'da (klasik) Web hizmetinizi geliştirmenin ilk aşamasıdır. Eğitim deneyinin amacı, size bir makine öğrenme modeli geliştirmeniz, test etmeniz, yinelemek ve sonunda eğitmek için bir yer vermektir. En iyi çözümü ararken aynı anda birden fazla modeli bile eğitebilirsiniz, ancak denemeyi bitirdikten sonra tek bir eğitimli model seçecek ve geri kalanını deneyden eleyebilirsiniz. Tahmine dayalı bir analiz denemesi geliştirmeye örnek olarak, bkz. [Azure Machine Learning Studio'da (klasik) kredi riski değerlendirmesi için tahmine dayalı bir analitik çözüm geliştirin.](tutorial-part1-credit-risk.md)

### <a name="the-predictive-experiment"></a>Tahmindeney
Eğitim denemenizde eğitimli bir modele sahip olduktan sonra, **Web Hizmeti Ayarla'yı** tıklatın ve eğitim deneyiminizi ***tahmine dayalı***bir denemeye dönüştürme işlemini başlatmak için Machine Learning Studio'da (klasik) **Tahmine Dayalı Web Hizmeti'ni** seçin. Tahmine dayalı denemenin amacı, sonunda bir Azure Web hizmeti olarak işlevsel hale gelmek amacıyla yeni veriler elde etmek için eğitilmiş modelinizi kullanmaktır.

Bu dönüştürme sizin için aşağıdaki adımlarla yapılır:

* Eğitim için kullanılan modül kümesini tek bir modüle dönüştürün ve eğitimli bir model olarak kaydedin
* Puanlama ile ilgili olmayan gereksiz modülleri ortadan kaldırın
* Nihai Web hizmetinin kullanacağı giriş ve çıktı bağlantı noktaları ekleme

Öngörüdenemenizi Web hizmeti olarak dağıtmaya hazır hale getirmek için yapmak istediğiniz daha fazla değişiklik olabilir. Örneğin, Web hizmetinin yalnızca bir sonuç alt kümesini çıkarmasını istiyorsanız, çıktı bağlantı noktasından önce bir filtreleme modülü ekleyebilirsiniz.

Bu dönüştürme işleminde, eğitim denemesi atılmaz. İşlem tamamlandığında, Studio'da iki sekmeniz vardır (klasik): biri eğitim deneyi için, diğeri de tahmine dayalı deney için. Bu şekilde, Web hizmetinizi dağıtmadan ve tahmine dayalı denemeyi yeniden oluşturmadan önce eğitim deneyinde değişiklik yapabilirsiniz. Veya başka bir deneme satırı başlatmak için eğitim deneyinin bir kopyasını kaydedebilirsiniz.

> [!NOTE]
> **Predictive Web Service'i** tıklattığınızda, eğitim denemenizi tahmine dayalı bir denemeye dönüştürmek için otomatik bir işlem başlatirsınız ve bu çoğu durumda iyi çalışır. Eğitim deneyiniz karmaşıksa (örneğin, birlikte katıldığınız eğitim için birden çok yolunuz varsa), bu dönüşümü el ile yapmayı tercih edebilirsiniz. Daha fazla bilgi [için, modelinizi Azure Machine Learning Studio'da (klasik) dağıtıma nasıl hazırlayacağınızı](convert-training-experiment-to-scoring-experiment.md)öğrenin.
>
>

### <a name="the-web-service"></a>Web hizmeti
Öngörüdenemenizin hazır olduğundan emin olduktan sonra, hizmetinizi Azure Kaynak Yöneticisi'ni temel alan bir Klasik Web hizmeti veya Yeni Web hizmeti olarak dağıtabilirsiniz. Modelinizi Klasik Machine Learning Web *hizmeti*olarak dağıtarak işlevsel hale getirmek için Web Hizmetini **Dağıt'ı** tıklatın ve **Web Hizmetini Dağıt'ı [Klasik]** seçin. *Yeni Makine Öğrenme Web hizmeti*olarak dağıtmak için Web Hizmetini **Dağıt'ı** tıklatın ve **Web Hizmetini Dağıt'ı [Yeni]** seçin. Kullanıcılar artık Web hizmeti REST API'sini kullanarak modelinize veri gönderebilir ve sonuçları geri alabilir. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Tipik olmayan durum: tahmine dayalı olmayan bir Web hizmeti oluşturma
Denemeniz bir tahmine dayalı analiz modeli eğitmiyorsa, hem bir eğitim deneyi hem de puanlama deneyi oluşturmanız gerekmez - sadece bir deneme vardır ve bunu bir Web hizmeti olarak dağıtabilirsiniz. Machine Learning Studio (klasik), kullandığınız modülleri analiz ederek denemenizin tahmine dayalı bir model içerip içermediğini algılar.

Denemenizi doğruladıktan ve bundan memnun olduktan sonra:

1. **Web Hizmetini Ayarla'yı** tıklatın ve **Yeniden Eğitim Web Hizmeti'ni** seçin - giriş ve çıkış düğümleri otomatik olarak eklenir
2. **Çalıştır'ı** tıklatın
3. **Web Hizmeti Dağıt'ı** tıklatın ve dağıtmak istediğiniz ortama bağlı olarak **Web Hizmeti [Klasik] dağıt'ı** veya **Web Hizmetini [Yeni] dağıt'ı** seçin.

Web hizmetiniz artık dağıtıldı ve web hizmetine tahmine dayalı bir Web hizmeti gibi erişebilir ve yönetebilirsiniz.

## <a name="updating-your-web-service"></a>Web hizmetinizi güncelleme
Denemenizi bir Web hizmeti olarak dağıttığınıza göre, güncelleştirmeniz gerekirse ne olur?

Bu, güncelleştirmek için gerekenlere bağlıdır:

**Giriş veya çıktıyı değiştirmek veya Web hizmetinin verileri nasıl işledeceğini değiştirmek istiyorsunuz**

Modeli değiştirmiyorsanız, ancak Web hizmetinin verileri işleme şeklini değiştiriyorsanız, tahmine dayalı denemeyi düzenlemeve **Ardından Web Hizmeti Dağıt'ı** tıklatıp Web **Hizmetini [Klasik]** dağıt'ı veya **Web Hizmetini yeniden dağıt'ı [Yeni]** seçeneğini belirleyebilirsiniz. Web hizmeti durduruldu, güncelleştirilmiş tahmin etütleri dağıtılır ve Web hizmeti yeniden başlatılır.

Aşağıda bir örnek verilmiştir: Tahmine dayalı denemenizin tüm giriş verisatırını öngörülen sonuçla döndürür. Web hizmetinin sonucu döndürmesini istediğinize karar verebilirsiniz. Böylece, sonuç dışındaki sütunları hariç tutmak için, çıkış bağlantı noktasından hemen önce, tahmine dayalı deneye bir **Proje Sütunları** modülü ekleyebilirsiniz. **Web Hizmeti Dağıt'ı** tıklattığınızda ve **Web Hizmetini [Klasik] Dağıt'ı** veya **Web Hizmetini Yeniden Dağıt'ı [Yeni]** olarak seçtiğinizde, Web hizmeti güncelleştirilir.

**Modeli yeni verilerle yeniden eğitmek istiyorsunuz**

Makine öğrenimi modelinizi korumak, ancak yeni verilerle yeniden eğitmek istiyorsanız, iki seçeneğiniz vardır:

1. **Web hizmeti çalışırken modeli yeniden eğitin** - Tahmine dayalı Web hizmeti çalışırken modelinizi yeniden eğitmek istiyorsanız, bunu ***yeniden eğitim denemesi***yapmak için eğitim deneyinde birkaç değişiklik yaparak yapabilirsiniz, o zaman yeniden eğitim ** *web* hizmeti**olarak dağıtabilirsiniz. Bunun nasıl yapılacağını anlatan talimatlar için, [Yeniden Eğitme Makinesi Öğrenme modellerine programlı olarak](/azure/machine-learning/studio/retrain-machine-learning-model)bakın.
2. **Orijinal eğitim deneyine geri dön ve modelinizi geliştirmek için farklı eğitim verilerini kullanın** - Tahmine dayalı deneyiniz Web hizmetiyle bağlantılıdır, ancak eğitim denemesi doğrudan bu şekilde bağlantılı değildir. Özgün eğitim denemesini değiştirir ve **Web Hizmeti Ayarla'yı**tıklattığınızda, dağıtıldığında *yeni* bir Web hizmeti oluşturacak *yeni* bir tahmin denemesi oluşturur. Sadece orijinal Web hizmetini güncellemez.

   Eğitim denemesini değiştirmeniz gerekiyorsa, onu açın ve kopya yapmak için **Kaydet'i** tıklatın. Bu, orijinal eğitim denemesini, tahmine dayalı denemeyi ve Web hizmetini sağlam bırakır. Artık değişikliklerinizle birlikte yeni bir Web hizmeti oluşturabilirsiniz. Yeni Web hizmetini dağıttıktan sonra önceki Web hizmetini durdurup durdurmamaya veya yeni hizmetin yanında çalışmaya devam etmeye karar verebilirsiniz.

**Farklı bir model yetiştirmek istiyorsun.**

Özgün tahmin denemenizde (farklı bir makine öğrenme algoritması seçmek, farklı bir eğitim yöntemi denemek, vb.) gibi değişiklikler yapmak istiyorsanız, modelinizi yeniden eğitmek için yukarıda açıklanan ikinci yordamı izlemeniz gerekir: eğitim denemenizi açın, bir kopyasını oluşturmak için **As'ı** kaydet'i tıklatın ve ardından modelinizi geliştirme, tahmine dayalı deneme oluşturma ve web hizmetini dağıtma nın yeni yolunu başlatmanız gerekir. Bu, orijinali ile ilgisi olmayan yeni bir Web hizmeti oluşturur - hangisinin veya her ikisinin çalışmaya devam edeceğine karar verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ve deneme süreci hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* denemeyi dönüştürme - [Modelinizi Azure Machine Learning Studio'da dağıtıma hazırlama (klasik)](convert-training-experiment-to-scoring-experiment.md)
* Web hizmetini dağıtma - [Azure Machine Learning web hizmetini dağıtma](deploy-a-machine-learning-web-service.md)
* modeli yeniden eğitmek - [Retrain Machine Learning modelleri programlı](/azure/machine-learning/studio/retrain-machine-learning-model)

Tüm işlem örnekleri için bkz:

* [Makine öğrenimi eğitimi: Azure Machine Learning Studio'da ilk denemenizi oluşturun (klasik)](create-experiment.md)
* [Walkthrough: Azure Machine Learning'de kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümü geliştirin](tutorial-part1-credit-risk.md)

