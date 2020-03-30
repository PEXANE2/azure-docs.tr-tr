---
title: Dağıtım için model hazırlama
titleSuffix: ML Studio (classic) - Azure
description: Machine Learning Studio (klasik) eğitim deneyiminizi tahmine dayalı bir denemeye dönüştürerek eğitimli modelinizi web hizmeti olarak dağıtıma nasıl hazırlayacağınızı.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 03/28/2017
ms.openlocfilehash: 061c340f8c4952d5a0f2a3873f7475e4f733c290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204520"
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da (klasik) dağıtım için modelinizi hazırlama

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning Studio (klasik), bir tahmine dayalı analiz modeli geliştirmek ve azure web hizmeti olarak dağıtarak bu modeli operasyonel hale getirmek için ihtiyacınız olan araçları sağlar.

Bunu yapmak için, modelinizi eğittiğinizi, puanladığınız ve düzenlemeyaptığınız bir *eğitim deneyi* olarak adlandırılan bir deney oluşturmak için Studio 'yı (klasik) kullanırsınız. Tatmin olduktan sonra, eğitim denemenizi kullanıcı verilerini puanlamak üzere yapılandırılmış bir *tahmine dayalı denemeye* dönüştürerek modelinizi dağıtmaya hazır olursunuz.

Bu sürecin bir örneğini [Tutorial 1: Predict credit risk'te](tutorial-part1-credit-risk.md)görebilirsiniz.

Bu makalede, bir eğitim deneyinin nasıl öngörülü bir deneye dönüştürüldüğü ve bu tahmine dayalı deneyin nasıl dağıtılan olduğu hakkında ayrıntılı bilgi yer almaktadır. Bu ayrıntıları anlayarak, dağıtılan modelinizi daha etkili hale getirmek için nasıl yapılandırabileceğinizi öğrenebilirsiniz.



## <a name="overview"></a>Genel Bakış 

Bir eğitim denemesini tahmine dayalı bir deneye dönüştürme işlemi üç adım içerir:

1. Makine öğrenimi algoritma modüllerini eğitimli modelinizle değiştirin.
2. Denemeyi yalnızca puanlama için gerekli olan modüllere kırpın. Bir eğitim deneyi, eğitim için gerekli olan ancak model eğitildikten sonra gerekli olmayan bir dizi modül içerir.
3. Modelinizin web hizmeti kullanıcısından verileri nasıl kabul edeceğini ve hangi verilerin döndürüleceğini tanımlayın.

> [!TIP]
> Eğitim denemenizde, kendi verilerinizi kullanarak modelinizi eğitmek ve puanlamak la ilgileniyorsunuz. Ancak dağıtıldıktan sonra, kullanıcılar modelinize yeni veriler gönderir ve tahmin sonuçlarını döndürecektir. Bu nedenle, eğitim denemenizi dağıtıma hazır hale getirmek için tahmine dayalı bir denemeye dönüştürürken, modelin başkaları tarafından nasıl kullanılacağını aklınızda bulundurun.
> 
> 

## <a name="set-up-web-service-button"></a>Web Hizmeti düğmesini ayarlama
Denemenizi çalıştırdıktan sonra (deneme tuvalinin altındaki **ÇALıŞTıR'ı** tıklatın) **Web Hizmeti Ayarla** düğmesini tıklatın **(Tahmine Dayalı Web Hizmeti** seçeneğini belirleyin). **Web Hizmeti'ni ayarlama,** eğitim denemenizi tahmine dayalı bir denemeye dönüştürmenin üç adımını sizin için gerçekleştirir:

1. Modül paletinin Eğitimli **Modeller** bölümünde (deneme tuvalinin solunda) eğitimli modelinizi kaydeder. Daha sonra makine öğrenme algoritması ve [Tren Modeli][train-model] modülleri kaydedilmiş eğitimli modeli ile değiştirir.
2. Denemenizi analiz eder ve açıkça yalnızca eğitim için kullanılan ve artık ihtiyaç duyulmamış modülleri kaldırır.
3. Denemenizde Varsayılan konumlara _Web hizmeti giriş_ ve _çıktı_ modülleri ekler (bu modüller kullanıcı verilerini kabul eder ve döndürür).

Örneğin, aşağıdaki deneme, örnek sayım verilerini kullanarak iki sınıf artırılmış karar ağacı modelini eğitir:

![Eğitim deneyi](./media/convert-training-experiment-to-scoring-experiment/figure1.png)

Bu deneydeki modüller temelde dört farklı işlevi yerine getirir:

![Modül fonksiyonları](./media/convert-training-experiment-to-scoring-experiment/figure2.png)

Bu eğitim denemesini tahmine dayalı bir denemeye dönüştürdüğünüzde, bu modüllerden bazılarına artık ihtiyaç duyulmamaktadır veya artık farklı bir amaca hizmet ederler:

* **Veriler** - Bu örnek veri kümesindeki veriler puanlama sırasında kullanılmaz - web hizmetinin kullanıcısı puanlanacak verileri sağlar. Ancak, bu veri kümesinden veri türleri gibi meta veriler, eğitilen model tarafından kullanılır. Bu nedenle veri kümesini tahmin edilebilir deneyde tutmanız gerekir, böylece bu meta verileri sağlayabilir.

* **Hazırlık** - Puanlama için gönderilecek kullanıcı verilerine bağlı olarak, bu modüller gelen verileri işlemek için gerekli olabilir veya olmayabilir. **Web Hizmeti Ayarla** düğmesi bunlara dokunmuyor - bunları nasıl işlemek istediğinize karar vermeniz gerekir.
  
    Örneğin, bu örnek veri kümesinde eksik değerler olabilir, bu nedenle bunlarla başa çıkmak için [Bir Temiz Eksik Veri][clean-missing-data] modülü eklenmiştir. Ayrıca, örnek veri kümesi modeli eğitmek için gerekli olmayan sütunlar içerir. Bu nedenle, bu ekstra sütunları veri akışından çıkarmak için [Dataset modülündeki Sütunları Seç][select-columns] eklenmiştir. Web hizmeti üzerinden puanlama için gönderilecek verilerin eksik değerlere sahip olmayacağını biliyorsanız, [Temiz Eksik Veri][clean-missing-data] modüllerini kaldırabilirsiniz. Ancak, [Dataset modülündeki Sütunları Seç,][select-columns] eğitilmiş modelin beklediği veri sütunlarını tanımlamaya yardımcı olduğundan, modülün kalması gerekir.

* **Tren** - Bu modüller modeli eğitmek için kullanılır. **Web Hizmeti Ayarla'yı**tıklattığınızda, bu modüller eğittiğiniz modeli içeren tek bir modülle değiştirilir. Bu yeni modül, modül paletinin **Eğitilmiş Modeller** bölümüne kaydedilir.

* **Puan** - Bu örnekte, [Veri Bölme][split] modülü veri akışını test verilerine ve eğitim verilerine bölmek için kullanılır. Tahmine dayalı deneyde artık eğitim vermiyoruz, bu yüzden [Split Data][split] kaldırılabilir. Benzer şekilde, test verilerinden elde edilen sonuçları karşılaştırmak için ikinci [Puan Modeli][score-model] modülü ve [Model'i Değerlendir][evaluate-model] modülü kullanılır, bu nedenle bu modüller tahmine dayalı deneyde gerekli değildir. Ancak, kalan [Puan Modeli][score-model] modülü, web hizmeti üzerinden bir puan sonucu döndürmek için gereklidir.

**Web Hizmetini Ayarla'yı**tıklattıktan sonra örneğimiz şu şekilde görünür:

![Dönüştürülmüş tahmine dayalı deney](./media/convert-training-experiment-to-scoring-experiment/figure3.png)

Web Hizmetini **Ayarla** tarafından yapılan iş, denemenizi bir web hizmeti olarak dağıtılmak üzere hazırlamak için yeterli olabilir. Ancak, denemenize özgü bazı ek işler yapmak isteyebilirsiniz.

### <a name="adjust-input-and-output-modules"></a>Giriş ve çıkış modüllerini ayarlama
Eğitim denemenizde, bir dizi eğitim verisi kullandınız ve sonra verileri makine öğrenme algoritmasının ihtiyaç duyduğu bir biçimde elde etmek için bazı işlemler yaptınız. Web hizmeti aracılığıyla almayı beklediğiniz verilerin bu işleme ihtiyacı yoksa, bunu atlayabilirsiniz: **Web hizmeti giriş modülüçıktısını** denemenizdeki farklı bir modüle bağlayın. Kullanıcının verileri artık modele bu konumda gelecektir.

Örneğin, varsayılan olarak **Web Hizmetini Ayarla,** **web hizmeti giriş** modülünü yukarıdaki şekilde gösterildiği gibi veri akışınızın en üstüne koyar. Ancak **Web hizmeti girdisini** veri işleme modüllerinin ötesine el ile konumlandırabiliriz:

![Web hizmeti girişini taşıma](./media/convert-training-experiment-to-scoring-experiment/figure4.png)

Web hizmeti aracılığıyla sağlanan giriş verileri artık herhangi bir ön işleme gerek kalmadan doğrudan Puan Modeli modülüne geçecektir.

Benzer şekilde, varsayılan olarak **Web Hizmetini Ayarla,** Web hizmeti çıktı modüllerini veri akışınızın en altına koyar. Bu örnekte, web hizmeti kullanıcıya, tam giriş veri vektörü artı puanlama sonuçlarını içeren [Puan Modeli][score-model] modülünün çıktısını geri verecektir.
Ancak, farklı bir şey döndürmek isterseniz, **Web hizmeti çıktı** modülünden önce ek modüller ekleyebilirsiniz. 

Örneğin, giriş verilerinin tüm vektörünü değil, yalnızca puanlama sonuçlarını döndürmek için, veri kümesi modülüne puanlama sonuçları dışındaki tüm sütunları hariç tutmak için [Sütunları Seç'i][select-columns] ekleyin. Ardından **Web hizmeti çıktı** modülünü [Dataset modülündeki Sütunları Seç'in][select-columns] çıktısına taşıyın. Deney şuna benzer:

![Web hizmeti çıktısını taşıma](./media/convert-training-experiment-to-scoring-experiment/figure5.png)

### <a name="add-or-remove-additional-data-processing-modules"></a>Ek veri işleme modülleri ekleme veya kaldırma
Denemenizde puanlama sırasında gerekli olmayacağını bildiğiniz daha fazla modül varsa, bunlar kaldırılabilir. Örneğin, Web hizmeti **giriş** modüllerini veri işleme modüllerinden sonra bir noktaya taşıdığımız için, [Temiz Eksik Veri][clean-missing-data] modüllerini tahmin edilebilir deneyden kaldırabiliriz.

Tahmine dayalı deneyimiz şuna benziyor:

![Ek modülü kaldırma](./media/convert-training-experiment-to-scoring-experiment/figure6.png)


### <a name="add-optional-web-service-parameters"></a>İsteğe bağlı Web Hizmeti Parametreleri Ekleme
Bazı durumlarda, web hizmetinizin kullanıcısının hizmete erişildiğinde modüllerin davranışını değiştirmesine izin vermek isteyebilirsiniz. *Web Hizmeti Parametreleri* bunu yapmanıza olanak sağlar.

Yaygın bir örnek, dağıtılan web hizmetinin kullanıcısının web hizmetine erişildiğinde farklı bir veri kaynağı belirtebilmeleri için [Bir İçe Aktarma Veri][import-data] modülü ayarlamaktır. Veya farklı bir hedef belirtilen bir [Dışa Aktarma Verileri][export-data] modülü yapılandırma.

Web Hizmeti Parametrelerini tanımlayabilir ve bunları bir veya daha fazla modül parametresiyle ilişkilendirebilir ve bunların gerekli mi yoksa isteğe bağlı mı olduğunu belirtebilirsiniz. Web hizmetinin kullanıcısı, hizmete erişildiğinde bu parametreler için değerler sağlar ve modül eylemleri buna göre değiştirilir.

Web Hizmeti Parametrelerinin ne olduğu ve bunların nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Machine Learning Web Service Parametrelerini kullanma][webserviceparameters]bilgisine bakın.

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Tahmine dayalı denemeyi bir web hizmeti olarak dağıtma
Tahmin etütyeterincehazırlanmış olduğundan, bunu bir Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar modelinize veri gönderebilir ve model tahminlerini döndürecektir.

Dağıtım sürecinin tamamı hakkında daha fazla bilgi için [bkz.][deploy]

[deploy]: deploy-a-machine-learning-web-service.md

<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
