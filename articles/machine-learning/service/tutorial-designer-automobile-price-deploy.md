---
title: 'Öğretici: tasarımcı ile makine öğrenimi modeli dağıtma'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında bir tahmine dayalı analiz çözümü oluşturmayı öğrenin (Önizleme). Sürükle ve bırak modüllerini kullanarak makine öğrenimi modelini eğitme, Puanlama ve dağıtma.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 69fba508eac4b778dcd72371fd1471625ecb8c1a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73501606"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer-preview"></a>Öğretici: tasarımcı ile makine öğrenimi modeli dağıtma (Önizleme)
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Başkalarına [öğreticinin birinci kısmında](tutorial-designer-automobile-price-train-score.md)geliştirilen tahmine dayalı modeli kullanma şansı vermek için bunu gerçek zamanlı bir uç nokta olarak dağıtabilirsiniz. 1\. Bölüm 'de, modelinizi eğitilolursunuz. Şimdi, kullanıcı girişine göre yeni tahmin oluşturma zamanı. Öğreticinin bu bölümünde şunları yapabilirsiniz:

> [!div class="checklist"]
> * Gerçek zamanlı uç nokta dağıtma
> * Bir ınıri yalıtma kümesi oluşturma
> * Gerçek zamanlı uç noktayı test etme

## <a name="prerequisites"></a>Önkoşullar

Tasarımcı 'da makine öğrenimi modelinin nasıl eğeceğinizi ve puanlandıralınacağını öğrenmek için [öğreticiden birinin bir kısmını](tutorial-designer-automobile-price-train-score.md) doldurun.

## <a name="deploy-a-real-time-endpoint"></a>Gerçek zamanlı uç nokta dağıtma

İşlem hattınızı dağıtmak için şunları yapmanız gerekir:

1. Eğitim işlem hattını, eğitim modüllerini kaldıran ve ınsekmek istekleri için giriş ve çıkışları ekleyen gerçek zamanlı bir çıkarım işlem hattına dönüştürün.
1. Çıkarım işlem hattını dağıtın.

### <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı bir çıkarım işlem hattı oluşturma

1. Ardışık düzen tuvalinin üst kısmında, **gerçek zamanlı çıkarım işlem hattı** > **çıkarım işlem hattı oluştur** ' u seçin.

    **Çıkarım işlem hattı oluştur**' u seçtiğinizde birkaç şey meydana gelir:
    
    * Eğitilen model, modül paletinde bir **veri kümesi** modülü olarak depolanır. **Veri kümelerim**altında bulabilirsiniz.
    * Eğitim için kullanılan model ve **bölünmüş verileri** **eğitme** gibi modüller kaldırılır.
    * Kaydedilen eğitilen model ardışık düzene geri eklenir.
    * **Web hizmeti girişi** ve **Web hizmeti çıkış** modülleri eklendi. Bu modüller Kullanıcı verilerinin modeli nereye giremeyeceğini ve verilerin döndürüldüğü yeri belirler.

    > [!Note]
    > **Eğitim işlem hattı** , ardışık düzen tuvalinin en üstündeki yeni sekmenin altına kaydedilir. Ayrıca, tasarımcıda yayımlanmış bir işlem hattı olarak bulunabilir.
    >

    İşlem hatlarınız şu şekilde görünmelidir:  

   ![Dağıtım için hazırlandıktan sonra işlem hattının beklenen yapılandırmasını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/real-time-inference-pipeline.png)

1. **Çalıştır** ' ı seçin ve aynı işlem hedefini kullanın ve Bölüm 1 ' de kullanmayı deneyin.

1. **Puan modeli** modülünü seçin.

1. Özellikler bölmesinde, modelin hala çalıştığını doğrulamak için **çıktılar** > **Görselleştir** ' i seçin. Orijinal verilerin tahmini fiyat ("puanlanmış Etiketler") ile birlikte görüntülendiğini görebilirsiniz.

1. **Dağıt**'ı seçin.

### <a name="create-an-inferencing-cluster"></a>Bir ınıri yalıtma kümesi oluşturma

Görüntülenen iletişim kutusunda, modelinizi dağıtmak için çalışma alanınızdaki mevcut Azure Kubernetes hizmeti (AKS) kümelerinden seçim yapabilirsiniz. AKS kümeniz yoksa, bir tane oluşturmak için aşağıdaki adımları kullanın.

1. **İşlem** sayfasına gitmek için Iletişim kutusunda **işlem** ' ı seçin.

1. Gezinti şeridinde, **çıkarım kümeleri** >  **+ Yeni**' yi seçin.

    ![Yeni çıkarım kümesi bölmesine nasıl gidebileceğiniz gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/new-inference-cluster.png)

1. Çıkarım kümesi bölmesinde yeni bir Kubernetes hizmeti yapılandırın.

1. **İşlem adı**için "aks-COMPUTE" yazın.
    
1. Yakında kullanılabilir bir **bölge**seçin.

1. **Oluştur**'u seçin.

    > [!Note]
    > Yeni bir AKS hizmeti oluşturmak yaklaşık 15 dakika sürer. **Çıkarım kümeleri** sayfasında sağlama durumunu kontrol edebilirsiniz
    >

### <a name="deploy-the-real-time-endpoint"></a>Gerçek zamanlı uç noktayı dağıtma

AKS hizmetiniz sağlamayı tamamladıktan sonra, dağıtımı tamamlamaya yönelik gerçek zamanlı ıntıma ardışık düzenine geri dönün.

1. Tuvalin üzerinde **Dağıt** ' ı seçin.

1. **Yeni gerçek zamanlı uç noktayı dağıt**' ı seçin. 

1. Oluşturduğunuz AKS kümesini seçin.

1. **Dağıt**'ı seçin.

    ![Yeni bir gerçek zamanlı uç noktanın nasıl ayarlanacağını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/setup-endpoint.png)

    Dağıtım tamamlandığında tuvalin üzerindeki bir başarı bildirimi görüntülenir, bu işlem birkaç dakika sürebilir.

## <a name="test-the-real-time-endpoint"></a>Gerçek zamanlı uç noktayı test etme

Soldaki çalışma alanı gezinti bölmesindeki **uç noktalar** sayfasına giderek gerçek zamanlı uç noktanızı test edebilirsiniz.

1. **Uç noktalar** sayfasında, dağıttığınız uç noktayı seçin.

    ![Son oluşturulan bitiş noktası vurgulanmış şekilde gerçek zamanlı uç noktalar sekmesini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/endpoints.png)

1. **Test**' i seçin.

1. Test verilerini girin veya oto dolgulu örnek verileri kullanın ve **Test**' i seçin.

    Test isteği uç noktaya gönderilir ve sonuçlar sayfada gösterilir. Giriş verileri için bir fiyat değeri oluşturulsa da tahmin değeri oluşturmak için kullanılmaz.

    ![Fiyat vurgulanmış olarak gerçek zamanlı uç noktanın puanlanmış etiketle nasıl test leneceğini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/test-endpoint.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, tasarımcıda makine öğrenimi modeli oluşturma, dağıtma ve kullanma konusunda önemli adımları öğrendiniz. Diğer sorun türlerini çözümlemek için tasarımcıyı nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için diğer örnek işlem hatlarımıza bakın.

> [!div class="nextstepaction"]
> [Kredi riski sınıflandırma örneği](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
