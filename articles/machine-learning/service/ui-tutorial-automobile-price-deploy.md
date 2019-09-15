---
title: 'Öğretici: Visual Interface ile makine öğrenimi modeli dağıtma'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning görsel arabiriminde tahmine dayalı analiz çözümü oluşturmayı öğrenin. Sürükle ve bırak modüllerini kullanarak makine öğrenimi modelini eğitme, Puanlama ve dağıtma. Bu öğretici, doğrusal regresyon kullanarak otomobil fiyatlarını tahmin eden iki bölümden oluşan bir serinin ikinci bölümüdür.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 07/11/2019
ms.openlocfilehash: 22d5c41e8b815fd99450962cb63d11e9560c787f
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997021"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Öğretici: Visual Interface ile makine öğrenimi modeli dağıtma

Başkalarına [öğreticinin birinci kısmında](ui-tutorial-automobile-price-train-score.md)geliştirilen tahmine dayalı modeli kullanma şansı vermek için bunu bir Azure Web hizmeti olarak dağıtabilirsiniz. Şimdiye kadar, modelinize eğitim yapmaya çalıştık. Şimdi, kullanıcı girişine göre yeni tahmin oluşturma zamanı. Öğreticinin bu bölümünde şunları yapabilirsiniz:

> [!div class="checklist"]
> * Bir modeli dağıtım için hazırlama
> * Bir web hizmetini dağıtma
> * Web hizmetini test etme
> * Bir Web hizmetini yönetme
> * Web hizmetini kullanma

## <a name="prerequisites"></a>Önkoşullar

Bir makine öğrenimi modelini görsel arabirimde eğitme ve puan alma hakkında bilgi edinmek için [öğreticiden birinin bir kısmını](ui-tutorial-automobile-price-train-score.md) doldurun.

## <a name="prepare-for-deployment"></a>Dağıtıma hazırlanma

Denemenizi bir Web hizmeti olarak dağıtmadan önce, ilk olarak *eğitim denemenizi* bir tahmine *dayalı deneyle*dönüştürmeniz gerekir.

1. Deneme tuvalinin en altında, tahmine **dayalı denemeler oluştur**' u seçin.

    ![Tahmine dayalı bir deneye eğitim denemesinin otomatik dönüştürülmesini gösteren animasyonlu GIF](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

    Tahmine **dayalı deneme oluştur**' u seçtiğinizde birkaç şey meydana gelir:
    
    * Eğitilen model, modül paletinde **eğitilen bir model** modülü olarak depolanır. **Eğitilen modeller**altında bulabilirsiniz.
    * Eğitim için kullanılan modülleri kaldırılır; özellikle:
      * Model Eğitme
      * Verileri Bölme
      * Modeli Değerlendirme
    * Kaydedilen eğitilen model denemeye geri eklenir.
    * **Web hizmeti girişi** ve **Web hizmeti çıkış** modülleri eklendi. Bu modüller, Kullanıcı verilerinin modeli nereye giremeyeceğini ve verilerin döndürüldüğü yeri belirler.

    **Eğitim** denemesi hala deneme tuvalinin en üstündeki yeni sekmelerin altına kaydedilir.

1. Denemeyi çalıştırmak için **Run (Çalıştır)** düğmesine basın.

1. **Puan modeli** modülünün çıkışını seçin ve modelin hala çalıştığını doğrulamak Için **sonuçları görüntüle** ' yi seçin. Orijinal verilerin görüntülendiğini ve tahmin edilen fiyat ("puanlanmış Etiketler") ile birlikte görebilirsiniz.

Denemeniz şu şekilde görünmelidir:  

![Dağıtım için hazırlandıktan sonra deneme sürümünün beklenen yapılandırmasını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma

1. Tuvalin altında **Web Hizmeti Dağıt** ' ı seçin.

1. Web hizmetinizi çalıştırmak istediğiniz **Işlem hedefini** seçin.

    Şu anda, görsel arabirim yalnızca Azure Kubernetes Service (AKS) işlem hedeflerine dağıtımı destekler. Machine Learning hizmeti çalışma alanınızda kullanılabilir AKS işlem hedefleri arasından seçim yapabilir veya görüntülenen iletişim kutusunda bulunan adımları kullanarak yeni bir AKS ortamı yapılandırabilirsiniz.

    ![Yeni bir işlem hedefi için olası bir yapılandırmayı gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. **Web hizmetini dağıt**' ı seçin. Dağıtım tamamlandığında aşağıdaki bildirimi görürsünüz. Dağıtım birkaç dakika sürebilir.

    ![Başarılı bir dağıtımın onay iletisini gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>Web hizmetini test edin

**Web Hizmetleri** sekmesine giderek, Visual Interface Web hizmetlerinizi test edebilir ve yönetebilirsiniz.

1. Web hizmeti bölümüne gidin. Ad öğreticisiyle dağıttığınız Web hizmeti 'ni görürsünüz **-otomobil Tahmini fiyatını tahmin edin [tahmine dayalı exp]** .

     ![Son oluşturulan Web hizmeti vurgulanmış Web hizmeti sekmesini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Ek ayrıntıları görüntülemek için Web hizmeti adını seçin.

1. **Test**' i seçin.

    [![Web hizmeti testi sayfasını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)](./media/ui-tutorial-automobile-price-deploy/web-service-test.png#lightbox)

1. Test verilerini girin veya oto dolgulu örnek verileri kullanın ve **Test**' i seçin.

    Test isteği Web hizmetine gönderilir ve sonuçlar sayfada gösterilir. Giriş verileri için bir fiyat değeri oluşturulsa da tahmin değeri oluşturmak için kullanılmaz.

## <a name="consume-the-web-service"></a>Web hizmetini kullanma

Kullanıcılar artık Azure Web hizmetinize API istekleri gönderebilir ve yeni otomobil fiyatlarını tahmin etmek için sonuçlar alabilir.

**İstek/yanıt** -Kullanıcı, bir http protokolünü kullanarak bir veya daha fazla otomobil verilerini hizmete bir veya daha fazla satır gönderir. Hizmet bir veya daha fazla sonuç kümesiyle yanıt verir.

Örnek REST çağrılarını, Web hizmeti ayrıntıları sayfasının **tüketme** sekmesinde bulabilirsiniz.

   ![Kullanıcıların tüketme sekmesinde bulabileceği örnek bir REST çağrısını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Daha fazla API ayrıntısı bulmak için **API belge** sekmesine gidin.

## <a name="manage-models-and-deployments"></a>Modelleri ve dağıtımları yönetme

Görsel arabirimde oluşturduğunuz modeller ve Web hizmeti dağıtımları da Azure Machine Learning çalışma alanından yönetilebilir.

1. Çalışma alanınızı [Azure Portal](https://portal.azure.com/)açın.  

1. Çalışma alanınızda **modeller**' ı seçin. Sonra oluşturduğunuz denemeyi seçin.

    ![Azure portal 'de denemeleri 'e nasıl gidebileceğiniz gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/portal-models.png)

    Bu sayfada, modelle ilgili ek ayrıntılar görürsünüz.

1. **Dağıtımları**seçin, modeli kullanan tüm Web hizmetlerini listeler. Web hizmeti adı ' nı seçin, Web hizmeti ayrıntısı sayfasına gider. Bu sayfada, Web hizmeti hakkında daha ayrıntılı bilgi edinebilirsiniz.

    [![Ekran görüntüsü ayrıntılı çalıştırma raporu](./media/ui-tutorial-automobile-price-deploy/deployment-details.png)](./media/ui-tutorial-automobile-price-deploy/deployment-details.png#lightbox)

Ayrıca, bu modelleri ve dağıtımları [çalışma alanı giriş sayfanızın (Önizleme)](https://ml.azure.com) **modeller** ve **uç noktalar** bölümlerinde bulabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Visual arabiriminde makine öğrenimi modeli oluşturma, dağıtma ve kullanma konusunda temel adımları öğrendiniz. Diğer sorun türlerini çözümlemek için görsel arabirimi nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için bkz. diğer örnek denemeleri.

> [!div class="nextstepaction"]
> [Kredi riski sınıflandırma örneği](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
