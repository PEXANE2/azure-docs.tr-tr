---
title: 'Öğretici 3: Kredi risk modelini dağıtın'
titleSuffix: Azure Machine Learning Studio (classic)
description: Azure Machine Learning Studio'da (klasik) kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümünü nasıl oluşturacağımı gösteren ayrıntılı bir öğretici. Bu öğretici üç bölümlük öğretici serisinin üçüncü bölümüdür. Bir modelin web hizmeti olarak nasıl dağıtılangerektiğini gösterir.
keywords: kredi riski, tahmine dayalı analitik çözümü, risk değerlendirmesi, dağıtım, web hizmeti
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: 9fb0b59374edf322e5e2221b90e912ee2c665bac
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79204162"
---
# <a name="tutorial-3-deploy-credit-risk-model---azure-machine-learning-studio-classic"></a>Eğitim 3: Kredi risk modelini dağıtın - Azure Machine Learning Studio (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu eğitimde, tahmine dayalı bir analitik çözüm geliştirme sürecine uzun bir göz atabilirsiniz. Machine Learning Studio (klasik) basit bir model geliştirmek.  Ardından modeli Azure Machine Learning web hizmeti olarak dağıtırsınız.  Bu dağıtılan model, yeni verileri kullanarak öngörülerde bulunabilir. Bu öğretici **üç bölümlük öğretici serisinin üçüncü bölümüdür.**

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi risk değerlendirmesi karmaşık bir sorundur, ancak bu öğretici biraz basitleştirir. Microsoft Azure Machine Learning Studio 'yı (klasik) kullanarak nasıl tahmine dayalı bir analiz çözümü oluşturabileceğinize bir örnek olarak kullanırsınız. Bu çözüm için Azure Machine Learning Studio (klasik) ve Machine Learning web hizmetini kullanırsınız. 

Bu üç bölümlü eğitimde, herkese açık kredi riski verileriyle başlarsınız.  Daha sonra geliştirmek ve bir tahmin modeli eğitmek.  Son olarak modeli bir web hizmeti olarak dağıtın.

[Öğreticinin birinci bölümünde,](tutorial-part1-credit-risk.md)bir Machine Learning Studio (klasik) çalışma alanı oluşturdunuz, veri yükledin ve bir deneme oluşturdunuz.

[Öğretici bölüm iki,](tutorial-part2-credit-risk-train.md)sen eğitimli ve modelleri değerlendirildi.

Öğretici bu bölümünde size:

> [!div class="checklist"]
> * Dağıtım için hazırlanma
> * Web hizmetini dağıtma
> * Web hizmetini test etme
> * Web hizmetini yönetme
> * Web hizmetine erişme

## <a name="prerequisites"></a>Ön koşullar

Öğretici tam [bölümü iki](tutorial-part2-credit-risk-train.md).

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma
Başkalarına bu eğitimde geliştirdiğiniz tahmine dayalı modeli kullanma şansı vermek için, bu modeli Azure'da bir web hizmeti olarak dağıtabilirsiniz.

Şu anda modelimizi eğiterek denemeler yapıyorsunuz. Ama dağıtılan hizmet artık eğitim yapmak için gidiyor - bizim modele dayalı kullanıcının giriş puanlayarak yeni tahminler oluşturmak için gidiyor. Bu deneyi ***bir eğitim*** deneyinden ***tahmine dayalı*** bir deneye dönüştürmek için bazı hazırlıklar yapacağız. 

Dağıtıma hazırlık üç adımlı bir işlemdir:  

1. Modellerden birini kaldırma
1. Oluşturduğunuz *eğitim denemesini* *tahmine dayalı* bir denemeye dönüştürün
1. Tahmine dayalı denemeyi bir web hizmeti olarak dağıtma

### <a name="remove-one-of-the-models"></a>Modellerden birini kaldırma

İlk olarak, bu deneyi biraz küçültmeniz gerekiyor. Şu anda denemede iki farklı modelvar, ancak bunu bir web hizmeti olarak dağıtırken yalnızca bir model kullanmak istiyorsunuz.  

Artırılmış ağaç modelinin SVM modelinden daha iyi performans gösterdiğine karar verdiğini varsayalım. Bu yüzden yapılacak ilk [şey, Iki Sınıf Destek Vektör Makinesi][two-class-support-vector-machine] modüllerini ve onu eğitmek için kullanılan modülleri kaldırmaktır. Deneme tuvalinin altındaki **Kaydet'i** tıklatarak önce denemenin bir kopyasını yapmak isteyebilirsiniz.

aşağıdaki modülleri silmeniz gerekir:  

* [İki Sınıflı Destek Vektör Makinesi][two-class-support-vector-machine]
* [Ona][train-model] bağlı Tren Modeli ve [Puan Modeli][score-model] modülleri
* [Verileri Normalleştir][normalize-data] (her ikisi de)
* [Modeli Değerlendirin][evaluate-model] (modelleri değerlendirmeyi bitirdiğimiz için)

Her modülü seçin ve Sil tuşuna basın veya modülü sağ tıklatın ve **Sil'i**seçin. 

![Destek Vektör Makinesi modelini kaldırmak için hangi modüllerin silineceklerini vurgular](./media/tutorial-part3-credit-risk-deploy/publish3a.png)

Bizim model şimdi böyle bir şey bakmak gerekir:

![Destek Vektör Makinesi modeli silindiğinde ortaya çıkan deneme](./media/tutorial-part3-credit-risk-deploy/publish3.png)

Şimdi bu modeli [Iki Sınıf Artırılmış Karar Ağacı'nı][two-class-boosted-decision-tree]kullanarak dağıtmaya hazırız.

### <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini tahmine dayalı bir deneye dönüştürme

Bu modeli dağıtıma hazırlamak için, bu eğitim denemesini tahmine dayalı bir denemeye dönüştürmeniz gerekir. Bu üç adım içerir:

1. Eğittiğiniz modeli kaydedin ve ardından eğitim modüllerimizi değiştirin
1. Yalnızca eğitim için gerekli olan modülleri kaldırmak için denemeyi kırpın
1. Web hizmetinin girişi nerede kabul edeceğini ve çıktıyı nerede oluşturduğunu tanımlayın

bunu el ile yapabilirsiniz, ancak neyse ki deneme tuvalinin altındaki **Web Hizmetini Ayarla'yı** tıklatarak (ve **Tahmine Dayalı Web Hizmeti** seçeneğini seçerek) üç adımda da gerçekleştirilebilir.

> [!TIP]
> Bir eğitim denemesini tahmine dayalı bir denemeye dönüştürdüğünüzde ne olacağı hakkında daha fazla ayrıntı istiyorsanız, [modelinizi Azure Machine Learning Studio'da (klasik) dağıtıma nasıl hazırlayacağınızı](convert-training-experiment-to-scoring-experiment.md)görün.

**Web Hizmeti Ayarla'yı**tıklattığınızda, birkaç şey olur:

* Eğitilen model tek bir **Eğitilmiş Model** modülüne dönüştürülür ve deneme tuvalinin solundaki modül paletinde saklanır **(Eğitimli Modeller**altında bulabilirsiniz)
* Eğitim için kullanılan modüller kaldırılır; Özellikle:
  * [İki Sınıf Artırılmış Karar Ağacı][two-class-boosted-decision-tree]
  * [Model Eğitme][train-model]
  * [Verileri Bölme][split]
  * test verileri için kullanılan ikinci [Execute R Script][execute-r-script] modülü
* Kaydedilen eğitimli model deneye geri eklenir
* **Web hizmeti girişi** ve **Web hizmeti çıktı** modülleri eklenir (bunlar kullanıcı verilerinin modele nerede gireceğini ve web hizmetine erişildiğinde hangi verilerin döndürüleceğini belirler)

> [!NOTE]
> Deneme tuvalinin üst kısmında eklenen sekmeler altında iki bölüm halinde kaydedildiğini görebilirsiniz. Orijinal eğitim deneysekmesi **Eğitim deneyi**altında ve yeni oluşturulan tahmin **deneyi Tahmin deneyi**altındadır. Tahmine dayalı deney, web hizmeti olarak dağıtacağınız denemedir.

Bu deneme ile ek bir adım atmanız gerekir.
verilere ağırlıklandırma işlevi sağlamak için iki [Execute R Script][execute-r-script] modülü eklediniz. Bu sadece eğitim ve test için gerekli bir hile, böylece son modelde bu modülleri alabilir.
Machine Learning Studio (klasik) [Split][split] modülü kaldırıldığında bir [Execute R Script][execute-r-script] modülü kaldırıldı. Şimdi diğer kaldırmak ve [Doğrudan Score Model][score-model] [Metadata Düzenleyicisi][metadata-editor] bağlayabilirsiniz.    

Deneyimiz şimdi şöyle görünmelidir:  

![Eğitimli model puanlama](./media/tutorial-part3-credit-risk-deploy/publish4.png)


> [!NOTE]
> UCI Alman Kredi Kartı Veri Kümesi'ni neden öngörüctahmini deneyde bıraktığınızı merak ediyor olabilirsiniz. Hizmet, orijinal veri kümesini değil, kullanıcının verilerini puanlayacak, peki neden orijinal veri kümesini modelde bırakalım?
> 
> Hizmetin orijinal kredi kartı verilerine ihtiyacı olmadığı doğrudur. Ancak, kaç sütun olduğu ve hangi sütunların sayısal olduğu gibi bilgileri içeren bu veriler için şema gerekir. Bu şema bilgileri kullanıcının verilerini yorumlamak için gereklidir. bu bileşenleri, puanlama modülünün hizmet çalışırken veri kümesi şemasına sahip olması için bağlı bırakırsınız. Veriler kullanılmaz, sadece şema kullanılır.  
> 
>Unutulmaması gereken önemli bir şey, orijinal veri seti etiketi içeriyorsa, o zaman web girişi beklenen şema da etiket ile bir sütun bekleyecek olmasıdır! Bunun bir yolu, web girişi ve eğitim veri kümesini ortak bir modüle bağlamadan önce etiketi ve eğitim veri kümesinde bulunan diğer verileri kaldırmaktır, ancak web girişlerinde yer almayacaktır. 
> 

Denemeyi son bir kez çalıştırın **(Çalıştır'ı**tıklatın .) Modelin hala çalıştığını doğrulamak istiyorsanız, [Puan Modeli][score-model] modülünün çıktısını tıklatın ve **Sonuçları Görüntüle'yi**seçin. Orijinal verilerin, kredi risk değeri ("Puanlı Etiketler") ve puanlama olasılık değeri ("Puanlı Olasılıklar") ile birlikte görüntülendiğini görebilirsiniz. 

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Denemeyi Klasik web hizmeti olarak veya Azure Kaynak Yöneticisi'ni temel alan Yeni bir web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtın
Denememizden türetilen Klasik bir web hizmetini dağıtmak için tuvalin altındaki **Web Hizmetini Dağıt'ı** tıklatın ve **Web Hizmeti Dağıt'ı [Klasik]** seçeneğini belirleyin. Machine Learning Studio (klasik) denemeyi bir web hizmeti olarak dağıtır ve sizi bu web hizmeti için panoya götürür. Bu sayfadan denemeye **(En** son görüntü veya **Görüntüle)** dönebilir ve web hizmetinin basit bir testini çalıştırabilirsiniz (aşağıdaki **web hizmetini test edin).** Web hizmetine erişebilen uygulamalar oluşturmak için burada da bilgiler vardır (bu öğreticinin bir sonraki adımında bu konuda daha fazla).

![Web hizmeti panosu](./media/tutorial-part3-credit-risk-deploy/publish6.png)


**CONFIGURATION** sekmesini tıklatarak hizmeti yapılandırabilirsiniz. Burada hizmet adını değiştirebilir (varsayılan olarak deneme adı verilir) ve bir açıklama verebilirsiniz. Ayrıca giriş ve çıktı verileri için daha kolay etiketler verebilirsiniz.  

![Web hizmetini yapılandırma](./media/tutorial-part3-credit-risk-deploy/publish5.png)


### <a name="deploy-as-a-new-web-service"></a>Yeni bir web hizmeti olarak dağıtın

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için, web hizmetini dağıttığınız abonelikte yeterli izinlere sahip olmalısınız. Daha fazla bilgi için Azure [Machine Learning Web Services portalını kullanarak bir web hizmetini yönet'e](manage-new-webservice.md)bakın. 

Denememizden türetilen yeni bir web hizmetini dağıtmak için:

1. Tuvalin altındaki **Web Hizmetini Dağıt'ı** tıklatın ve **Web Hizmetini Dağıt'ı [Yeni]** seçeneğini belirleyin. Machine Learning Studio (klasik) sizi Azure Machine Learning web hizmetlerine AktarAn **Dağıt Madeney** sayfası.

1. Web hizmeti için bir ad girin. 

1. **Fiyat Planı**için, varolan bir fiyatlandırma planı seçebilir veya "Yeni oluştur" seçeneğini belirleyebilir ve yeni plana bir ad verip aylık plan seçeneğini seçebilirsiniz. Plan, varsayılan bölgenizin planlarını varsayılan olarak katmanlar ve web hizmetiniz bu bölgeye dağıtılır.

1. **Dağıt'ı**tıklatın.

Birkaç dakika sonra, web hizmetinizin **Quickstart** sayfası açılır.

**Yapılı** sekmesini tıklatarak hizmeti kışlayabilir. Burada hizmet başlığını değiştirebilir ve bir açıklama verebilirsiniz. 

Web hizmetini test etmek için **Test** sekmesini tıklatın (aşağıdaki **web hizmetini test edin).** Web hizmetine erişebilen uygulamalar oluşturma hakkında bilgi **için, Tüket sekmesini** tıklatın (bu öğreticideki bir sonraki adım daha ayrıntılı olarak gidecektir).

> [!TIP]
> Web hizmetini dağıttıktan sonra güncelleyebilirsiniz. Örneğin, modelinizi değiştirmek istiyorsanız, eğitim denemesini düzenlemeniz, model parametrelerini değiştirebilirsiniz ve **Web Hizmeti**Dağıt'ı tıklatarak **Web Hizmetini [Klasik]** dağıtveya Web Hizmetini **Dağıt [Yeni]** seçeneğini tıklatabilirsiniz. Denemeyi yeniden dağıttığınızda, artık güncelleştirilmiş modelinizi kullanarak web hizmetinin yerini alır.  
> 
> 

## <a name="test-the-web-service"></a>Web hizmetini test etme

Web hizmetine erişildiğinde, kullanıcının **verileri,** [Puan Modeli][score-model] modülüne geçirildiği ve puanlandığı Web hizmeti giriş modülü üzerinden girer. Tahmine dayalı denemeyi ayarlama şekliniz, model verileri özgün kredi riski veri kümesiyle aynı biçimde bekler.
Sonuçlar, **Web hizmeti çıktı** modülü aracılığıyla web hizmetinden kullanıcıya döndürülür.

> [!TIP]
> Tahmine dayalı denemeyi yapılandırma şekliniz, [Puan Modeli][score-model] modülünden elde edilen tüm sonuçlar döndürülür. Bu, tüm giriş verilerini, kredi risk değerini ve puanlama olasılığını içerir. Ancak isterseniz farklı bir şey döndürebilirsiniz - örneğin, sadece kredi risk değerini iade edebilirsiniz. Bunu yapmak için, web hizmetinin geri dönmesini istemediğiniz sütunları ortadan kaldırmak için [Puan Modeli][score-model] ve **Web hizmeti çıktısı** arasına Sütun [Seç][select-columns] modülü ekleyin. 
> 
> 

Klasik web hizmetini **Machine Learning Studio'da (klasik)** veya **Azure Machine Learning Web Services** portalında test edebilirsiniz.
Yeni bir web hizmetini yalnızca **Machine Learning Web Services** portalında test edebilirsiniz.

> [!TIP]
> Azure Machine Learning Web Services portalında sınama yaparken, portalın İstek-Yanıt hizmetini test etmek için kullanabileceğiniz örnek veriler oluşturmasını sağlayabilirsiniz. **Yapılandırılan** sayfada, **Örnek Veri Etkinliği**için "Evet" seçeneğini belirleyin. **Test** sayfasında İstek-Yanıt sekmesini açtığınızda, portal orijinal kredi riski veri kümesinden alınan örnek verileri doldurur.

### <a name="test-a-classic-web-service"></a>Klasik web hizmetini test edin

Klasik web hizmetini Machine Learning Studio'da (klasik) veya Machine Learning Web Services portalında test edebilirsiniz. 

#### <a name="test-in-machine-learning-studio-classic"></a>Machine Learning Studio Testi (klasik)

1. Web hizmetinin **PANO** sayfasında Varsayılan **Bitiş Noktası'nın**altındaki **Test** düğmesini tıklatın. Bir iletişim kutusu açılır ve hizmetin giriş verilerini ister. Bunlar, özgün kredi riski veri kümesinde görünen sütunların aynıdır.  

1. Bir veri kümesi girin ve ardından **Tamam'ı**tıklatın. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Machine Learning Web Services portalında test

1. Web hizmetinin **PANO** sayfasında, **Varsayılan Bitiş Noktası**altındaki Test **önizleme** bağlantısını tıklatın. Web hizmeti bitiş noktası için Azure Machine Learning Web Services portalındaki test sayfası açılır ve hizmetiçin giriş verilerini ister. Bunlar, özgün kredi riski veri kümesinde görünen sütunların aynıdır.

2. **Test İstek-Yanıt'ı**tıklatın. 

### <a name="test-a-new-web-service"></a>Yeni bir web hizmetini test edin

Yeni bir web hizmetini yalnızca Machine Learning Web Services portalında test edebilirsiniz.

1. Azure [Machine Learning Web Services](https://services.azureml.net/quickstart) portalında sayfanın üst kısmındatest'i tıklatın. **Test** **Test** sayfası açılır ve hizmet için veri girişi yapabilirsiniz. Görüntülenen giriş alanları, özgün kredi riski veri kümesinde görünen sütunlara karşılık gelir. 

1. Bir veri kümesi girin ve ardından **Test İstek-Yanıt'ı**tıklatın.

Test sonuçları, çıktı sütunundaki sayfanın sağ tarafında görüntülenir. 


## <a name="manage-the-web-service"></a>Web hizmetini yönetme

İster Klasik ister Yeni olsun, web hizmetinizi dağıttıktan sonra [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portalından yönetebilirsiniz.

Web hizmetinizin performansını izlemek için:

1. Microsoft Azure [Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalında oturum açın
1. **Web hizmetlerini** tıklatın
1. Web hizmetinizi tıklatın
1. **Pano'yu** tıklatın

## <a name="access-the-web-service"></a>Web hizmetine erişme

Bu öğreticinin önceki adımında, kredi riski tahmin modelinizi kullanan bir web hizmeti dağıttınız. Artık kullanıcılar bu verilere veri gönderebilir ve sonuç alabilirler. 

Web hizmeti, REST API'lerini kullanarak verileri iki şekilde alabilen ve döndürebilen bir Azure web hizmetidir:  

* **İstek/Yanıt** - Kullanıcı bir HTTP protokolü kullanarak hizmete bir veya daha fazla kredi verisi satırı gönderir ve hizmet bir veya daha fazla sonuç kümesiyle yanıt verir.
* **Toplu Yürütme** - Kullanıcı bir veya daha fazla kredi veri satırını Azure blob'unda depolar ve sonra blob konumunu hizmete gönderir. Hizmet, giriş blob'undaki tüm veri satırlarını puanlar, sonuçları başka bir lekede depolar ve bu kapsayıcının URL'sini döndürür.  

Web hizmetine erişim ve tüketme hakkında daha fazla bilgi için [bkz.](/azure/machine-learning/studio/consume-web-services)



## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şu adımları tamamladınız:

> [!div class="checklist"]
> * Dağıtım için hazırlanma
> * Web hizmetini dağıtma
> * Web hizmetini test etme
> * Web hizmetini yönetme
> * Web hizmetine erişme

Ayrıca, R, C# ve Python programlama dillerinde sizin için sağlanan başlangıç kodunu kullanarak web hizmetine erişmek için özel bir uygulama geliştirebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Machine Learning Web hizmetini tüketin](consume-web-services.md)

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
