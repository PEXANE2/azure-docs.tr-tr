---
title: 'Öğretici: Tasarımcı ile bir makine öğrenme modeli dağıtmak'
titleSuffix: Azure Machine Learning
description: Bu öğretici, Azure Machine Learning tasarımcısında (önizleme) nasıl tahmine dayalı bir analitik çözüm oluşturabileceğinizi gösterir. Sürükle ve bırak modüllerini kullanarak bir makine öğrenme modelini eğitin, puanlandırın ve dağıtın.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: aeccd604a4dae3b2ca83f5b599d9a6857acee34a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79037188"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer-preview"></a>Öğretici: Tasarımcı ile bir makine öğrenme modeli dağıtmak (önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Öğreticinin [birinci bölümünde](tutorial-designer-automobile-price-train-score.md) geliştirilen tahmine dayalı modeli başkalarına kullanma şansı vermek için dağıtabilirsiniz. Birinci bölümde, modelini eğittin. Şimdi, kullanıcı girişine dayalı yeni öngörüler oluşturma nın zamanı. Öğreticinin bu bölümünde, şunları yapacaksınız:

> [!div class="checklist"]
> * Gerçek zamanlı çıkarım ardışık bir iş tonu oluşturun.
> * Çıkarımlı bir küme oluşturun.
> * Gerçek zamanlı bitiş noktasını dağıtın.
> * Gerçek zamanlı bitiş noktasını test edin.

## <a name="prerequisites"></a>Ön koşullar

Tasarımcıda bir makine öğrenme modelinin nasıl eğitilip puan alınabildiğini öğrenmek için [öğreticinin tam bir bölümünü](tutorial-designer-automobile-price-train-score.md) tamamlayın.

## <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı çıkarım ardışık hattı oluşturma

Ardışık sisteminizi dağıtmak için önce eğitim ardışık hattını gerçek zamanlı çıkarım boru hattına dönüştürmeniz gerekir. Bu işlem eğitim modüllerini kaldırır ve istekleri işlemek için web hizmeti giriş ve çıktıları ekler.

### <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı çıkarım ardışık hattı oluşturma

1. Boru hattı tuvalinin üzerinde, **Çıkarım Oluştur ardışık ardışık ardışık ardışık** > **ardışık ardışık**

    ![Oluşturma boru hattı düğmesini nerede bulacağını gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/tutorial2-create-inference-pipeline.png)

    Boru hattınız şimdi şu na benzemelidir: 

   ![Dağıtıma hazırlandıktan sonra ardışık yapının beklenen yapılandırmasını gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/real-time-inference-pipeline.png)

    **Çıkarım oluştur ardışık hattını**seçtiğinizde, birkaç şey olur:
    
    * Eğitilen model, modül paletinde **Bir Dataset** modülü olarak depolanır. **Benim Datasets**altında bulabilirsiniz.
    * **Tren Modeli** ve **Split Data** gibi eğitim modülleri kaldırılır.
    * Kurtarılan eğitimli model boru hattına geri eklenir.
    * **Web Hizmet Girişi** ve **Web Hizmeti Çıktı** modülleri eklenir. Bu modüller, kullanıcı verilerinin ardışık ardışık noktaya nereye girdiğini ve verilerin nerede döndürüldüğü gösterir.

    > [!NOTE]
    > Varsayılan olarak, **Web Hizmeti Girişi,** tahmine dayalı ardışık ardışık ardışık ardışık ardışık oluşturmak için kullanılan eğitim verileriyle aynı veri şemasını bekler. Bu senaryoda, fiyat şema dahil edilir. Ancak, fiyat tahmin sırasında bir faktör olarak kullanılmaz.
    >

1. **Gönder'i**seçin ve birinci bölümde kullandığınız aynı işlem hedefini ve denemeyi kullanın.

1. **Dağıt**'ı seçin.

## <a name="create-an-inferencing-cluster"></a>Çıkarımlı bir küme oluşturma

Görünen iletişim kutusunda, modelinizi dağıtmak için varolan Azure Kubernetes Hizmeti (AKS) kümelerinden birini seçebilirsiniz. AKS kümeniz yoksa, oluşturmak için aşağıdaki adımları kullanın.

1. **İşlem** sayfasına gitmek için görünen iletişim kutusunda **İşlem'i** seçin.

1. Gezinti şeridinde **Çıkarım Kümeleri** > **+ Yeni'yi**seçin.

    ![Yeni çıkarım küme bölmesine nasıl ulaşılabildiğini gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/new-inference-cluster.png)

1. Çıkarım küme bölmesinde, yeni bir Kubernetes Hizmeti yapılandırın.

1. **İşlem adı**için *aks-compute* girin.
    
1. **Bölge**için kullanılabilen yakındaki bir bölge seçin.

1. **Oluştur'u**seçin.

    > [!NOTE]
    > Yeni bir AKS hizmeti oluşturmak yaklaşık 15 dakika sürer. **Çıkarım Kümeleri** sayfasındaki sağlama durumunu denetleyebilirsiniz.
    >

## <a name="deploy-the-real-time-endpoint"></a>Gerçek zamanlı bitiş noktasını dağıtma

AKS hizmetiniz sağlamayı tamamladıktan sonra, dağıtımı tamamlamak için gerçek zamanlı çıkarıcı ardışık düzene geri dönün.

1. Tuvalin üzerine **Dağıt'ı** seçin.

1. **Yeni gerçek zamanlı bitiş noktası dağıt'ı**seçin. 

1. Oluşturduğunuz AKS kümesini seçin.

1. **Dağıt**'ı seçin.

    ![Yeni bir gerçek zamanlı bitiş noktasının nasıl ayarlanolduğunu gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/setup-endpoint.png)

    Dağıtım bittikten sonra tuvalin üzerinde bir başarı bildirimi görüntülenir. Birkaç dakika sürebilir.

## <a name="test-the-real-time-endpoint"></a>Gerçek zamanlı bitiş noktasını test edin

Dağıtım bittikten sonra, **Uç Noktalar** sayfasına giderek gerçek zamanlı bitiş noktanızı test edebilirsiniz.

1. Bitiş **Noktaları** sayfasında, dağıttığınız bitiş noktasını seçin.

    ![Son oluşturulan uç nokta vurgulanmış gerçek zamanlı uç nokta sekmesini gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/endpoints.png)

1. **Test**'i seçin.

1. Test verilerini el ile giriş yapabilir veya otomatik doldurulmuş örnek verileri kullanabilir ve **Test'i**seçebilirsiniz.

    Portal bitiş noktasına bir test isteği gönderir ve sonuçları gösterir. Giriş verileri için bir fiyat değeri oluşturulsa da, tahmin değerini oluşturmak için kullanılmaz.

    ![Vurgulanan fiyat için puanlanan etiketle gerçek zamanlı bitiş noktasının nasıl test edilebildiğini gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/test-endpoint.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, tasarımcıda bir makine öğrenme modeli oluşturma, dağıtma ve tüketme yle ilgili temel adımları öğrendiniz. Tasarımcıyı diğer sorun türlerini çözmek için nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için diğer örnek ardışık alan hatlarımıza bakın.

> [!div class="nextstepaction"]
> [Tasarımcı örnekleri](samples-designer.md)
