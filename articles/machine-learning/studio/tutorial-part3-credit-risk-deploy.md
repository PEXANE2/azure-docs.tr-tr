---
title: 'Öğretici 3: Kredi risk modelini dağıtma'
titleSuffix: Azure Machine Learning Studio (classic)
description: Azure Machine Learning Studio (klasik) ' de kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümü oluşturmayı gösteren ayrıntılı bir öğretici. Bu öğretici, üç bölümden oluşan bir öğretici serisinin üçüncü bölümüdür. Bir modelin Web hizmeti olarak nasıl dağıtılacağını gösterir.
keywords: kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi, dağıtma, Web hizmeti
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: 9353e5749906aca1f0c91b622c93ba6028cd3bef
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432190"
---
# <a name="tutorial-3-deploy-credit-risk-model---azure-machine-learning-studio-classic"></a>Öğretici 3: Kredi risk modelini dağıtma-Azure Machine Learning Studio (klasik)

Bu öğreticide, tahmine dayalı bir analiz çözümü geliştirme sürecinde genişletilmiş bir görünüm elde edersiniz. Machine Learning Studio (klasik) içinde basit bir model geliştirirsiniz.  Daha sonra modeli bir Azure Machine Learning Web hizmeti olarak dağıtabilirsiniz.  Bu dağıtılan model yeni verileri kullanarak tahminleri yapabilir. Bu öğretici, **üç bölümden oluşan bir öğretici serisinin üçüncü bölümüdür**.

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi risk değerlendirmesi karmaşık bir sorundur, ancak bu öğreticide bir bit basitleştirilmesi sağlanır. Bunu, Microsoft Azure Machine Learning Studio (klasik) kullanarak tahmine dayalı analiz çözümü oluşturma hakkında bir örnek olarak kullanacaksınız. Bu çözüm için Azure Machine Learning Studio (klasik) ve bir Machine Learning Web hizmeti kullanacaksınız. 

Bu üç bölümden oluşan öğreticide, genel kullanıma açık kredi riski verileriyle karşılaşırsınız.  Daha sonra tahmine dayalı bir model geliştirip eğirsiniz.  Son olarak, modeli bir Web hizmeti olarak dağıtırsınız.

[Öğreticinin bir parçası](tutorial-part1-credit-risk.md)olarak bir Machine Learning Studio (klasik) çalışma alanı oluşturdunuz, verileri karşıya yükledi ve bir deneme oluşturdunuz.

[Öğreticinin ikinci bölümünde](tutorial-part2-credit-risk-train.md), modellerinizi eğitildiniz ve değerlendirdiniz.

Öğreticinin bu bölümünde şunları yapabilirsiniz:

> [!div class="checklist"]
> * Dağıtıma hazırlanma
> * Web hizmetini dağıtma
> * Web hizmetini test edin
> * Web hizmetini yönetme
> * Web hizmetine erişme

## <a name="prerequisites"></a>Ön koşullar

[Öğreticinin ikinci bölümünü](tutorial-part2-credit-risk-train.md)doldurun.

## <a name="prepare-for-deployment"></a>Dağıtıma hazırlanma
Başkalarına bu öğreticide geliştirdiğiniz tahmine dayalı modeli kullanma şansı vermek için, Azure 'da Web hizmeti olarak dağıtabilirsiniz.

Modelimizi eğitmek için bu noktaya kadar deneme yapmış olursunuz. Ancak, dağıtılmış hizmet artık eğitim yapacak - kullanıcının girişi bizim modele dayalı Puanlama tarafından yeni tahminler üretmek gittiği. Biz bu denemeden dönüştürmek için bazı hazırlıklar yapacağız şekilde bir ***eğitim*** için deneme bir ***Tahmine dayalı*** denemeler yapın. 

Dağıtım hazırlığı üç adımlı bir işlemdir:  

1. Modelleri birini kaldırın
1. Oluşturduğunuz Eğitim denemesini tahmine dayalı bir *deneyle* dönüştürün
1. Tahmine dayalı denemeye bir web hizmeti olarak dağıtma

### <a name="remove-one-of-the-models"></a>Modelleri birini kaldırın

İlk olarak, bu denemeyi biraz daha az kırpabilmeniz gerekir. Şu anda denemenin iki farklı modeli vardır, ancak bunu bir Web hizmeti olarak dağıtırken yalnızca bir model kullanmak istersiniz.  

Daha önce, artırılmış ağaç modelinin SVM modelinden daha iyi gerçekleştirildiğinden emin olalım. Yani ilk yapmanız gereken [Iki sınıf destek vektör makinesi][two-class-support-vector-machine] modülünü ve bunu eğitmek için kullanılan modülleri kaldırdık. Tıklayarak denemenin bir kopyasını önce yapmak isteyebileceğiniz **Kaydet** deneme tuvalinin altındaki.

aşağıdaki modülleri silmeniz gerekir:  

* [İki sınıflı destek vektör makinesi][two-class-support-vector-machine]
* Modele bağlı model ve [puan modeli][score-model] modüllerini [eğitme][train-model]
* [Verileri Normalleştir][normalize-data] (her ikisi de)
* [Modeli değerlendir][evaluate-model] (modellerin değerlendirilmesi tamamlandığımızda)

Her modülü seçin ve Delete tuşuna basın veya modül sağ tıklayıp **Sil**. 

![Destek vektör makinesi modelini kaldırmak için hangi modüllerin silineceğini vurgular](./media/tutorial-part3-credit-risk-deploy/publish3a.png)

Modelimiz şöyle görünmelidir:

![Destek vektör makine modeli silindiğinde ortaya çıkan deneme](./media/tutorial-part3-credit-risk-deploy/publish3.png)

Şimdi, bu modeli [Iki sınıf önceden artırılmış karar ağacını][two-class-boosted-decision-tree]kullanarak dağıtmaya hazırız.

### <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini öngörücü bir denemeye dönüştürme

Bu modeli dağıtıma hazırlamak için, bu eğitim denemesini bir tahmine dayalı deneyle dönüştürmeniz gerekir. Bu üç adımdan oluşur:

1. Eğitilen modeli kaydedin ve ardından eğitim modüllerimizi değiştirin
1. Eğitim için yalnızca gerekli olan modülleri kaldırmak için denemeyi Kırp
1. Burada, web hizmeti giriş kabul eder ve burada Bu çıktıyı oluşturur tanımlayın

Bunu el ile yapabilirsiniz ancak neyse ki, deneme tuvalinin alt kısmındaki **Web hizmeti ayarla** öğesine tıklanarak (tahmine **dayalı Web hizmeti** seçeneği seçilerek) tüm üç adımı gerçekleştirebilirsiniz.

> [!TIP]
> Bir eğitim denemesini tahmine dayalı bir deneyle dönüştürdüğünüzde ne olacağı hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Machine Learning Studio (klasik) dağıtım için modelinizi hazırlama](convert-training-experiment-to-scoring-experiment.md).

Tıkladığınızda **Web hizmetinin ayarı**, olacaklar:

* Tek bir eğitim modeli dönüştürülür **eğitilen Model** modülü ve deneme tuvaline solundaki modül paletindeki saklı (altında bulabilirsiniz **eğitilen modelleri**)
* Eğitim için kullanılan modülleri kaldırılır; özellikle:
  * [İki sınıf önceden artırılmış karar ağacı][two-class-boosted-decision-tree]
  * [Modeli eğitme][train-model]
  * [Verileri Böl][split]
  * test verileri için kullanılan ikinci [Execute R betik][execute-r-script] modülü
* Kaydedilmiş eğitilen modeli yeniden denemenin eklenir
* **Web hizmeti giriş** ve **Web hizmeti çıkış** modülleri eklendi (bunlar burada kullanıcının veri modeli girer ve web hizmeti erişim sağlandığında hangi verilerin döndürülen tanımlar)

> [!NOTE]
> Denemeyi deneme tuvalinin üst kısmında eklenen sekmeleri altında iki parça kaydedildiğini görebilirsiniz. Özgün eğitim denemesini sekmesi altında olduğunu **eğitim denemesini**, ve yeni oluşturulan Tahmine dayalı denemeye altındadır **Tahmine dayalı denemeye**. Tahmine dayalı deneme, Web hizmeti olarak dağıtacağınız bir deneyimdir.

Bu deneyle ek bir adım gerçekleştirmeniz gerekir.
verilere bir ağırlık işlevi sağlamak için iki [Execute R betik][execute-r-script] modülü eklediniz. Bu, yalnızca eğitim ve test için gerekli olan bir eliydi ve bu modülleri son modelde alabilirsiniz.
Machine Learning Studio (klasik) [bölme][split] modülünü kaldıran bir [Execute R betik][execute-r-script] modülünü kaldırdı. Artık diğer ve bağlantı [meta verileri düzenleyicisini][metadata-editor] doğrudan [model Puanlama][score-model]' ne gönderebilirsiniz.    

Bizim deneme gibi görünmelidir:  

![Eğitim modeli Puanlama](./media/tutorial-part3-credit-risk-deploy/publish4.png)


> [!NOTE]
> Tahmine dayalı denemenize ait UCı Almanya kredi kartı veri veri kümesini neden solınızdan merak ediyor olabilirsiniz. Hizmet değil özgün veri kümesinden kullanıcı verilerini puanlamak için neden özgün veri kümesinden modelde geliştirmediğinden geçiyor?
> 
> Hizmet asıl kredi kartı verileriyle gerektirmeyeceği geçerlidir. Ancak, şema vardır, kaç sütun gibi bilgileri içerir ve hangi sütunların sayısal bu veriler için gerekir. Bu şema bilgileri, kullanıcının verileri yorumlamak gereklidir. hizmet çalışırken Puanlama modülünün veri kümesi şemasına sahip olması için bu bileşenleri bağlı bırakmalısınız. Verileri kullanılmaz, yalnızca şema.  
> 
>Dikkat edilecek önemli bir unsur, özgün veri etiketi içeriyorsa, ardından web girdisinden beklenen şema ayrıca etiketli bir sütun beklediğiniz olan! Etiket ve diğer verileri, eğitim kümesinde olan ancak web girişlerinde web giriş ve eğitim veri kümesi, ortak bir modüle bağlanmadan önce olmaz, bu kullanacağınızı kaldırmaktır. 
> 

Deneme süresini bir kez çalıştırın ( **Çalıştır**' a tıklayın.) Modelin hala çalıştığını doğrulamak istiyorsanız, [puan modeli][score-model] modülünün çıktısına tıklayın ve **sonuçları görüntüle**' yi seçin. Özgün veriler, kredi riski değeri ("Puanlanmış etiketler") ve puanlama olasılık değeri ("Puanlanmış olasılıklar".) ile birlikte görüntülendiğini görebilirsiniz. 

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Denemeyi ya da bir Klasik web hizmeti olarak veya Azure Resource Manager'a bağlı yeni bir web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtma
Bizim denemeden türetilmiş bir Klasik web hizmetini dağıtmak için **Web hizmeti Dağıt** seçin ve tuval aşağıda **Web hizmeti dağıtma [Klasik]** . Machine Learning Studio (klasik), denemeyi bir Web hizmeti olarak dağıtır ve sizi o Web hizmeti panosuna götürür. Bu sayfadan denemeye geri dönebilir (**anlık görüntü** veya **en sonuncuyu**görüntüleme) ve Web hizmetinin basit bir testini çalıştırabilirsiniz (bkz. **Test Web hizmeti** ). Burada Web hizmetine erişebilen uygulamalar (Bu öğreticinin sonraki adımında daha fazla) oluşturmak için buradaki bilgiler de vardır.

![Web hizmet Panosu](./media/tutorial-part3-credit-risk-deploy/publish6.png)


**Yapılandırma** sekmesine tıklayarak hizmeti yapılandırabilirsiniz. Burada, hizmet adını değiştirebilirsiniz (varsayılan olarak deneme adı verilir) ve buna bir açıklama verin. Daha fazla kolay etiketler için girdi ve çıktı verilerini de tanıyabilirsiniz.  

![Web hizmetini yapılandır](./media/tutorial-part3-credit-risk-deploy/publish5.png)


### <a name="deploy-as-a-new-web-service"></a>Yeni bir web hizmeti dağıtma

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için web hizmetini dağıttığınız aboneliğe yeterli izinleri olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri portalını kullanarak bir web hizmetini yönetme](manage-new-webservice.md). 

Yeni bir web hizmetini dağıtmak için sunduğumuz denemeden türetilmiş:

1. Tıklayın **Web hizmeti Dağıt** seçin ve tuval aşağıda **Web hizmeti dağıtma [Yeni]** . Machine Learning Studio (klasik) sizi Azure Machine Learning Web Hizmetleri **dağıtım** denemesi sayfasına aktarır.

1. Web hizmeti için bir ad girin. 

1. İçin **fiyat planı**, bir devrenin fiyatlandırma planını seçin veya "Yeni Oluştur" seçebilir ve yeni plan bir ad verin ve aylık plan seçeneğini belirleyin. Hizmetinizi varsayılan bölgeden ve web hizmetiniz için planlar plan katmanları varsayılan bu bölgeye dağıtılır.

1. Tıklayın **dağıtma**.

Birkaç dakika sonra **hızlı** web hizmetiniz için sayfası açılır.

**Yapılandır** sekmesine tıklayarak hizmeti yapılandırabilirsiniz. Burada hizmet başlığını değiştirebilir ve bir açıklama verebilirsiniz. 

Web hizmetini test etmek için **Test** sekme (bkz **web hizmetini Test** aşağıda). Web hizmetine erişebilen uygulamalar oluşturma hakkında daha fazla bilgi **için, kullanma sekmesine tıklayın** (bu öğreticideki bir sonraki adım daha fazla ayrıntıya gidecektir).

> [!TIP]
> Bunu dağıttıktan sonra web hizmeti güncelleştirebilirsiniz. Örneğin, eğitim denemesini düzenleyebilir, sonra da, modelinizde değiştirmek istiyorsanız, model parametrelerinin ince ve tıklayın **Web hizmeti Dağıt**u seçerek **Web hizmeti dağıtma [Klasik]** veya **[Yeni] Web hizmetini dağıtma**. Denemeyi yeniden dağıttığınızda, artık güncelleştirilmiş model kullanarak web hizmeti değiştirir.  
> 
> 

## <a name="test-the-web-service"></a>Web hizmetini test edin

Web hizmetine erişildiğinde, kullanıcının verileri, [puan modeli modülüne ve puanına][score-model] geçirildiği **Web hizmeti giriş** modülüne girer. Tahmine dayalı denemeyi ayarlama yönteminiz, model, verileri orijinal kredi riski veri kümesiyle aynı biçimde bekler.
Sonuçları kullanıcıya web hizmeti aracılığıyla döndürülen **Web hizmeti çıkış** modülü.

> [!TIP]
> Tahmine dayalı denemenize sahip olduğunuz şekilde, [puan modeli][score-model] modülündeki tüm sonuçlar döndürülür. Bu, tüm giriş verilerini ayrıca kredi riski değeri ve puanlama olasılık içerir. Ancak isterseniz farklı bir şey döndürebilir; Örneğin, yalnızca kredi riski değer döndürebilir. Bunu yapmak için, Web hizmetinin döndürmesini istemediğiniz sütunları ortadan kaldırmak için [puan modeli][score-model] ve **Web hizmeti çıkışı** arasına bir [sütun Seç][select-columns] modülü ekleyin. 
> 
> 

Klasik bir Web hizmetini **Machine Learning Studio (klasik)** veya **Azure Machine Learning Web Hizmetleri** portalında test edebilirsiniz.
Yeni bir web test edebilirsiniz yalnızca hizmet **Machine Learning Web Hizmetleri** portalı.

> [!TIP]
> Azure Machine Learning Web Hizmetleri portalında test ederken, istek-yanıt hizmeti test etmek için kullanabileceğiniz örnek veriler oluşturma portalı olabilir. Üzerinde **yapılandırma** sayfasında, "Evet" seçeneğini **örnek veriler etkin?** . Üzerinde istek-yanıt sekmesini açtığınızda **Test** sayfasında, portal özgün kredi riski veri kümesinden alınan örnek veriler doldurur.

### <a name="test-a-classic-web-service"></a>Klasik web hizmetini test edin

Klasik bir Web hizmetini Machine Learning Studio (klasik) veya Machine Learning Web Hizmetleri portalında test edebilirsiniz. 

#### <a name="test-in-machine-learning-studio-classic"></a>Machine Learning Studio 'de test (klasik)

1. Üzerinde **PANO** sayfasında web hizmeti, **Test** düğmesini **varsayılan uç nokta**. Bir iletişim kutusu açılır ve hizmet için bir giriş verisi sorar. Özgün kredi riski kümesinde görünen aynı sütunları şunlardır.  

1. Bir veri kümesini girin ve ardından **Tamam**. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Machine Learning Web Hizmetleri portalında test etme

1. Üzerinde **PANO** sayfasında web hizmeti, **Test Önizleme** altında bağlantı **varsayılan uç nokta**. Azure Machine Learning Web Hizmetleri portalında web hizmeti uç noktası için test sayfası açılır ve hizmet için bir giriş verisi ister. Özgün kredi riski kümesinde görünen aynı sütunları şunlardır.

2. Tıklayın **Test istek-yanıt**. 

### <a name="test-a-new-web-service"></a>Yeni bir web hizmetini test edin

Yalnızca Machine Learning Web Hizmetleri portalında yeni bir web hizmetini test edebilirsiniz.

1. İçinde [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portal'ı tıklatın **Test** sayfanın üstünde. **Test** sayfası açılır ve hizmet için veri girebilirsiniz. Görüntülenen giriş alanları, özgün kredi riski kümesinde görünen sütunlara karşılık gelir. 

1. Bir veri kümesini girin ve ardından **Test istek-yanıt**.

Test sonuçlarını, çıkış sütununun sayfanın sağ tarafında görüntülenir. 


## <a name="manage-the-web-service"></a>Web hizmetini yönetme

Web hizmetinizi Klasik veya yeni dağıttıktan sonra buradan yönetebilirsiniz [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı.

Web hizmetinizin performansını izlemek için:

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı
1. Tıklayın **Web Hizmetleri**
1. Web hizmeti
1. Tıklayın **Panosu**

## <a name="access-the-web-service"></a>Web hizmetine erişme

Bu öğreticinin önceki adımında, kredi riski tahmin modelini kullanan bir Web hizmeti dağıttınız. Artık kullanıcıların veri göndermek ve sonuçları almak kullanabilirsiniz. 

Web hizmeti, alabilir ve dönüş verileri iki yöntemden biriyle REST API'lerini kullanarak bir Azure web hizmetidir:  

* **İstek/yanıt** - kullanıcı bir HTTP protokolünü kullanarak bir veya daha fazla kredi veri satırlarını hizmetine gönderir ve hizmet yanıt veren bir veya daha fazla sonuç kümeleri.
* **Toplu yürütme** - kullanıcı depolar veya daha fazla satır kredi verileri bir Azure blob ve sonra blob konum hizmetine gönderir. Hizmet giriş blob veri satırı puanlar, başka bir blobun sonuçlarını depolar ve bu kapsayıcı URL'sini döndürür.  

Klasik web hizmetine erişmek için hızlı ve en kolay yollarından biri sayesinde [Azure ML istek-yanıt hizmeti Web uygulaması](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) veya [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonunu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Bu web uygulaması şablonları, web hizmetinizin girdi verilerini ve hangi döndüreceği bildiği bir özel web uygulaması oluşturabilirsiniz. Tek yapmak için ihtiyacınız olan web hizmeti ve veri erişim sağlamak ve şablon geri kalanını yapar.

Web uygulaması şablonları kullanma hakkında daha fazla bilgi için bkz. [web uygulaması şablonu ile bir Azure Machine Learning Web hizmetini kullanma](/azure/machine-learning/studio/consume-web-services).



## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu adımları tamamladınız:

> [!div class="checklist"]
> * Dağıtıma hazırlanma
> * Web hizmetini dağıtma
> * Web hizmetini test edin
> * Web hizmetini yönetme
> * Web hizmetine erişme

Ayrıca, R, sağlanan başlangıç kodunu kullanarak web hizmetine erişmek için özel bir uygulama geliştirebilirsiniz C#, ve Python programlama dilleri arasındadır.

> [!div class="nextstepaction"]
> [Azure Machine Learning Web hizmeti kullanma](consume-web-services.md)

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
