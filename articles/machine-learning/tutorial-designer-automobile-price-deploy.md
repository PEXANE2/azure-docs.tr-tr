---
title: 'Öğretici: tasarımcı ile ML modellerini dağıtma'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında bir tahmine dayalı analiz çözümü oluşturun. Sürükle ve bırak modüllerini kullanarak makine öğrenimi modelini eğitme, Puanlama ve dağıtma.
author: likebupt
ms.author: keli19
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 01/15/2021
ms.custom: designer
ms.openlocfilehash: ec563371ab505113117707f56c31f506f7fdf377
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101659518"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer"></a>Öğretici: tasarımcı ile makine öğrenimi modeli dağıtma


Diğer kullanıcılara bunu kullanma şansı vermek için [öğreticinin birinci](tutorial-designer-automobile-price-train-score.md) kısmında geliştirilen tahmine dayalı modeli dağıtabilirsiniz. Birinci bölümde, modelinizi eğitilolursunuz. Şimdi, kullanıcı girişine göre tahmin oluşturma zamanı. Öğreticinin bu bölümünde şunları yapmanız gerekir:

> [!div class="checklist"]
> * Gerçek zamanlı bir çıkarım işlem hattı oluşturun.
> * Bir ınıri sınırlama kümesi oluşturun.
> * Gerçek zamanlı bitiş noktasını dağıtın.
> * Gerçek zamanlı uç noktayı test edin.

## <a name="prerequisites"></a>Önkoşullar

Tasarımcı 'da makine öğrenimi modelinin nasıl eğeceğinizi ve puanlandıralınacağını öğrenmek için [öğreticiden birinin bir kısmını](tutorial-designer-automobile-price-train-score.md) doldurun.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı bir çıkarım işlem hattı oluşturma

İşlem hattınızı dağıtmak için öncelikle eğitim işlem hattını gerçek zamanlı bir çıkarım ardışık düzenine dönüştürmeniz gerekir. Bu işlem, eğitim modüllerini kaldırır ve istekleri işlemek için Web hizmeti girişleri ve çıkışları ekler.

### <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı bir çıkarım işlem hattı oluşturma

1. İşlem hattı tuvalinin üzerinde, **çıkarım ardışık düzen**  >  **gerçek zamanlı çıkarım işlem hattı**' nı seçin.

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/tutorial2-create-inference-pipeline.png" alt-text="Ardışık düzen oluştur düğmesinin nerede bulunacağını gösteren ekran görüntüsü":::

    İşlem hatlarınız şu şekilde görünmelidir: 

   ![Dağıtım için hazırlandıktan sonra işlem hattının beklenen yapılandırmasını gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/real-time-inference-pipeline.png)

    **Çıkarım işlem hattı oluştur**' u seçtiğinizde birkaç şey meydana gelir:
    
    * Eğitilen model, modül paletinde bir **veri kümesi** modülü olarak depolanır. **Veri kümelerim** altında bulabilirsiniz.
    * Model ve **bölünmüş verileri** **eğitme** gibi eğitim modülleri kaldırılır.
    * Kaydedilen eğitilen model ardışık düzene geri eklenir.
    * **Web hizmeti girişi** ve **Web hizmeti çıkış** modülleri eklendi. Bu modüller, Kullanıcı verilerinin işlem hattına girdiği ve verilerin döndürüldüğü yeri gösterir.

    > [!NOTE]
    > Varsayılan olarak, **Web hizmeti girişi** , tahmine dayalı işlem hattını oluşturmak için kullanılan eğitim verileriyle aynı veri şemasını bekler. Bu senaryoda, şemaya bir değer dahildir. Ancak, fiyat tahmin sırasında bir faktör olarak kullanılmaz.
    >

1. **Gönder**' i seçin ve aynı işlem hedefini kullanın ve birinci bölümde kullandığınız deneyin.

    İlk çalıştırıldıysanız, işlem hattının çalışmasının tamamlanması 20 dakikaya kadar sürebilir. Varsayılan işlem ayarlarının minimum düğüm boyutu 0 ' dır ve bu, tasarımcının boşta kaldıktan sonra kaynakları ayırması gerektiği anlamına gelir. İşlem kaynakları zaten ayrıldığından tekrarlanan işlem hattı çalıştırmaları daha az zaman alır. Ayrıca tasarımcı, verimliliği artırmak için her modül için önbelleğe alınmış sonuçları kullanır.

1. **Dağıt**'ı seçin.

## <a name="create-an-inferencing-cluster"></a>Bir ınıri yalıtma kümesi oluşturma

Görüntülenen iletişim kutusunda, modelinizi dağıtmak için mevcut herhangi bir Azure Kubernetes hizmeti (AKS) kümesi arasından seçim yapabilirsiniz. AKS kümeniz yoksa, bir tane oluşturmak için aşağıdaki adımları kullanın.

1. **İşlem** sayfasına git görüntülenen Iletişim kutusunda **işlem** ' i seçin.

1. Gezinti şeridinde, **çıkarım kümeleri**  >  **+ Yeni**' yi seçin.

    ![Yeni çıkarım kümesi bölmesine nasıl alınacağını gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/new-inference-cluster.png)
   
1. Çıkarım kümesi bölmesinde yeni bir Kubernetes hizmeti yapılandırın.

1. **İşlem adı** için *aks-COMPUTE* girin.
    
1. **Bölge** için kullanılabilen bir yakın bölge seçin.

1. **Oluştur**’u seçin.

    > [!NOTE]
    > Yeni bir AKS hizmeti oluşturmak yaklaşık 15 dakika sürer. **Çıkarım kümeleri** sayfasında sağlama durumunu kontrol edebilirsiniz.
    >

## <a name="deploy-the-real-time-endpoint"></a>Gerçek zamanlı uç noktayı dağıtma

AKS hizmetiniz sağlamayı tamamladıktan sonra, dağıtımı tamamlamaya yönelik gerçek zamanlı ıntıma ardışık düzenine geri dönün.

1. Tuvalin üzerinde **Dağıt** ' ı seçin.

1. **Yeni gerçek zamanlı uç noktayı dağıt**' ı seçin. 

1. Oluşturduğunuz AKS kümesini seçin.

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/setup-endpoint.png" alt-text="Yeni bir gerçek zamanlı uç noktanın nasıl ayarlanacağını gösteren ekran görüntüsü":::

    Ayrıca, gerçek zamanlı uç noktanıza yönelik **Gelişmiş** ayarları değiştirebilirsiniz.
    
    |Gelişmiş ayar|Açıklama|
    |---|---|
    |Application Insights tanılamayı ve veri toplamayı etkinleştir| Azure Application Insights dağıtılan uç noktalardan veri toplamak üzere etkinleştirilip etkinleştirilmeyeceğini belirtir. </br> Varsayılan olarak: false |
    |Puanlama zaman aşımı| Web hizmetine yönelik Puanlama çağrılarına zorlamak için milisaniye cinsinden zaman aşımı.</br>Varsayılan olarak: 60000|
    |Otomatik ölçeklendirme etkin|   Web hizmeti için otomatik ölçeklendirmenin etkinleştirilip etkinleştirilmeyeceğini belirtir.</br>Varsayılan olarak: true|
    |Minimum çoğaltmalar| Bu Web hizmetini otomatik ölçeklendirirken kullanılacak kapsayıcı sayısı alt sınırı.</br>Varsayılan olarak: 1|
    |En fazla çoğaltma| Bu Web hizmetini otomatik ölçeklendirirken kullanılacak kapsayıcı sayısı üst sınırı.</br> Varsayılan olarak: 10|
    |Hedef kullanım|Otomatik Scaler 'nın bu Web hizmeti için bakımını denemesi gereken hedef kullanım (100 ' dan fazla).</br> Varsayılan olarak: 70|
    |Yenileme süresi|Otomatik gizleme bu Web hizmetini ölçeklendirmek için ne sıklıkta (saniye cinsinden) çalışır.</br> Varsayılan olarak: 1|
    |CPU Reserve kapasitesi|Bu Web hizmeti için ayrılacak CPU çekirdeklerinin sayısı.</br> Varsayılan olarak: 0,1|
    |Bellek ayırma kapasitesi|Bu Web hizmeti için ayrılacak bellek miktarı (GB cinsinden).</br> Varsayılan olarak: 0,5|
        

1. **Dağıt**'ı seçin. 

    Dağıtım bittikten sonra tuvalin üzerindeki başarı bildirimi görüntülenir. İşlem birkaç dakika sürebilir.

> [!TIP]
> Ayrıca, gerçek zamanlı uç nokta ayarı kutusunda **işlem türü** Için **Azure Container Instance** ' ı seçerseniz **Azure Container Instance** 'a (aci) dağıtabilirsiniz.
> Azure Container Instance, test veya geliştirme için kullanılır. 48 GB 'den az RAM gerektiren düşük ölçekli CPU tabanlı iş yükleri için ACI 'yi kullanın.

## <a name="test-the-real-time-endpoint"></a>Gerçek zamanlı uç noktayı test etme

Dağıtım bittikten sonra, **uç noktalar** sayfasına giderek gerçek zamanlı uç noktanızı görüntüleyebilirsiniz.

1. **Uç noktalar** sayfasında, dağıttığınız uç noktayı seçin.

    **Ayrıntılar** SEKMESINDE Rest URI, Swagger tanımı, durum ve Etiketler gibi daha fazla bilgi görebilirsiniz.

    Kullanım sekmesinde **,** örnek tüketim kodu, güvenlik anahtarları bulabilir ve kimlik doğrulama yöntemlerini ayarlayabilirsiniz.

    **Dağıtım günlükleri** sekmesinde, gerçek zamanlı uç noktanızın ayrıntılı dağıtım günlüklerini bulabilirsiniz.

1. Uç noktanızı test etmek için **Test** sekmesine gidin. Buradan, test verileri girebilir ve **Test** bitiş noktanıza ait çıktıyı Doğrula ' yı seçebilirsiniz.

Web hizmetinizi kullanma hakkında daha fazla bilgi için bkz. Web hizmeti [olarak dağıtılan bir modeli](how-to-consume-web-service.md) kullanma

## <a name="limitations"></a>Sınırlamalar

Eğitim işlem hattınızda bazı değişiklikler yaparsanız, eğitim işlem hattını yeniden göndermeniz, çıkarım işlem hattını **güncelleştirmeniz** ve çıkarım ardışık düzenini tekrar çalıştırmanız gerekir.

Yalnızca eğitilen modellerin, çıkarım işlem hattında güncelleştirileceğini, ancak veri dönüştürme güncellenmediğini unutmayın.

Güncel dönüştürmeyi çıkarım ardışık düzeninde kullanmak için, dönüştürme modülünün dönüştürme çıkışını DataSet olarak kaydetmeniz gerekir.

![Dönüştürme veri kümesinin nasıl kaydedileceği gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/register-transformation-dataset.png)

Ardından, çıkarım işlem hattındaki **TD** modülünü kayıtlı veri kümesiyle el ile değiştirin.

![Dönüştürme modülünün nasıl değiştirileceğini gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/replace-td-module.png)

Ardından, çıkarım işlem hattını güncelleştirilmiş model ve dönüşümle gönderebilir ve dağıtabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, tasarımcıda makine öğrenimi modeli oluşturma, dağıtma ve kullanma bölümündeki temel adımları öğrendiniz. Tasarımcıyı nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın:

+ [Tasarımcı örnekleri](samples-designer.md): diğer sorun türlerini çözümlemek için tasarımcıyı nasıl kullanacağınızı öğrenin.
+ [Azure Machine Learning Studio 'yu bir Azure sanal ağında kullanın](how-to-enable-studio-virtual-network.md).
