---
title: Azure Machine Learning tasarımcısını kullanarak modelleri yeniden eğitin (önizleme)
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında (önizleme) yayımlanmış ardışık hatlar ile modelleri nasıl yeniden eğitabileceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: keli19
author: likebupt
ms.date: 04/06/2020
ms.openlocfilehash: 721e5414fc4753cd5d58a17fc7ed51ea99868778
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810368"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısı (önizleme) ile modelleri yeniden eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu nasıl yapılsa makalede, bir makine öğrenme modelini yeniden eğitmek için Azure Machine Learning tasarımcısını nasıl kullanacağınızı öğrenirsiniz. İş akışınızı otomatikleştirmek ve modelinizi yeni veriler üzerinde eğitmek için parametreleri ayarlamak için yayımlanmış ardışık hatlar kullanırsınız. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir makine öğrenme modeli eğitin.
> * Bir ardışık parametre oluşturun.
> * Eğitim boru hattınızı yayınlayın.
> * Modelinizi yeni parametrelerle yeniden eğitin.

## <a name="prerequisites"></a>Ön koşullar

* Enterprise SKU ile Azure Machine Learning çalışma alanı.
* Tasarımcının erişebileceği bir veri kümesi. Bu aşağıdakilerden biri olabilir:
   * Azure Machine Learning kayıtlı veri kümesi
    
     **-veya-**
   * Azure Machine Learning veri deposunda depolanan bir veri dosyası.
   
Tasarımcıyı kullanarak veri erişimi hakkında bilgi [için](how-to-designer-import-data.md)bkz.

Bu makalede, tasarımcı da boru hattı oluşturma temel bilgiye sahip olduğunu varsayar. Rehberli bir giriş [için, öğreticiyi](tutorial-designer-automobile-price-train-score.md)tamamlayın. 

### <a name="sample-pipeline"></a>Örnek boru hattı

Bu makalede kullanılan ardışık işlem, [Örnek 3: Gelir tahmininin](samples-designer.md#classification-samples)değiştirilmiş bir sürümüdür. Ardışık iş, kendi verilerinizi kullanarak modelleri nasıl eğiteceklerini göstermek için örnek veri kümesi yerine [Veri Alma](algorithm-module-reference/import-data.md) modülünü kullanır.

![İçe Aktarma Verisi modüllerini vurgulayan bir kutuyla değiştirilmiş örnek ardışık ardışık gösteriyi gösteren ekran görüntüsü](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="create-a-pipeline-parameter"></a>Bir boru hattı parametresi oluşturma

Değişkenleri çalışma zamanında dinamik olarak ayarlamak için ardışık hatlar parametreleri oluşturun. Bu örnekte, modelinizi farklı veriler üzerinde yeniden eğitebilmeniz için eğitim veri yolunu sabit bir değerden bir parametreye değiştirirsiniz.

1. Veri **Alma** modülünü seçin.

    > [!NOTE]
    > Bu örnek, kayıtlı bir veri deposundaki verilere erişmek için Veri Alma modüllerini kullanır. Ancak, alternatif veri erişim desenleri kullanıyorsanız benzer adımları izleyebilirsiniz.

1. Modül ayrıntı bölmesinde, tuvalin sağında veri kaynağınızı seçin.

1. Verilerinize giden yolu girin. Ayrıca, dosya ağacınıza göz atmak için **Gözat yolunu** da seçebilirsiniz. 

1. **Fare** yolu alanını ve görünen **Yol** alanının üzerindeki elipsleri seçin.

    ![Bir ardışık parametrenin nasıl oluşturulurulur gösteren ekran görüntüsü](media/how-to-retrain-designer/add-pipeline-parameter.png)

1. **Ardışık etki hattına ekle'yi**seçin.

1. Bir parametre adı ve varsayılan değer sağlayın.

   > [!NOTE]
   > Boru hattı taslağınızın başlığının yanındaki **Ayarlar** dişli simgesini seçerek boru hattı parametrelerinizi inceleyebilir ve düzenlemeyapabilirsiniz. 

1. **Kaydet'i**seçin.

1. Boru hattı çalıştırmasını gönderin.

## <a name="find-a-trained-model"></a>Eğitimli bir model bulun

Tasarımcı, eğitilmiş modeller de dahil olmak üzere tüm boru hattı çıktısını varsayılan çalışma alanı depolama hesabına kaydeder. Ayrıca, eğitimli modellere doğrudan tasarımcıdan erişebilirsiniz:

1. Boru hattının çalışmasını bekleyin.
1. Tren **Modeli** modülünü seçin.
1. Modül ayrıntıları bölmesinde, tuvalin sağında, **Çıktılar + günlükleri**seçin.
1. Modelinizi çalıştırma günlükleriyle birlikte **diğer çıktılarda** bulabilirsiniz.
1. Alternatif olarak, **görünüm çıktısimgesini** seçin. Buradan, doğrudan veri deponuza gitmek için iletişim kutusundaki yönergeyi takip edebilirsiniz. 

![Eğitilmiş modelin nasıl indirilir olduğunu gösteren ekran görüntüsü](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="publish-a-training-pipeline"></a>Eğitim boru hattı yayımlama

Gelecekte ardışık hatlarınızı kolayca yeniden kullanmak için bir ardışık nokta bitiş noktasına bir ardışık hat lar yayımlayın. Bir ardışık nokta bitiş noktası, gelecekte ardışık alanı çağırmak için bir REST bitiş noktası oluşturur. Bu örnekte, ardışık nokta uç noktanız, farklı veriler üzerinde bir modeli yeniden eğitmek için ardışık hattınızı yeniden kullanmanıza olanak tanır.

1. Tasarımcı tuvalinin üzerinde **Yayımla'yı** seçin.
1. Bir ardışık lık bitiş noktası seçin veya oluşturun.

   > [!NOTE]
   > Birden çok ardışık hattı tek bir bitiş noktasına yayımlayabilirsiniz. Belirli bir bitiş noktasındaki her ardışık ardışık noktaya, ardışık hatsonu noktasını çağırdığınızda belirtebileceğiniz bir sürüm numarası verilir.

1. **Yayımla**’yı seçin.

## <a name="retrain-your-model"></a>Modelinizi yeniden eğitin

Artık yayınlanmış bir eğitim hattınız olduğuna göre, modelinizi yeni veriler üzerinde yeniden eğitmek için bunu kullanabilirsiniz. Stüdyo çalışma alanından veya programlı bir şekilde bir ardışık bitiş noktasından çalıştırmagönderebilirsiniz.

### <a name="submit-runs-by-using-the-designer"></a>Tasarımcıyı kullanarak çalıştırma gönder

Tasarımcıdan parametrelendirilmiş ardışık ardışık nokta sonu çalışması göndermek için aşağıdaki adımları kullanın:

1. Stüdyo çalışma alanınızdaki **Uç Noktalar** sayfasına gidin.
1. Pipeline **uç noktaları** sekmesini seçin. Ardından, ardışık nokta bitiş noktanızı seçin.
1. **Yayımlanmış ardışık hatlar** sekmesini seçin. Ardından, çalıştırmak istediğiniz ardışık hatlar hattını seçin.
1. **Gönder**’i seçin.
1. Kurulum iletişim kutusunda, çalıştırmanın parametreleri değerlerini belirtebilirsiniz. Bu örnekte, ABD dışı bir veri kümesi kullanarak modelinizi eğitmek için veri yolunu güncelleştirin.

![Tasarımcıda parametrelendirilmiş bir ardışık etki hattı nın nasıl ayarlanış yapılacağını gösteren ekran görüntüsü](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>Kodu kullanarak çalıştırma gönder

Yayınlanan bir ardışık boru hattının REST bitiş noktasını genel bakış panelinde bulabilirsiniz. Bitiş noktasını arayarak, yayımlanmış ardışık hattı yeniden eğitebilirsiniz.

REST araması yapmak için OAuth 2.0 taşıyıcı tipi kimlik doğrulama üstbilgisine ihtiyacınız vardır. Çalışma alanınızda kimlik doğrulamayı ayarlama ve parametreli REST araması yapma hakkında bilgi için toplu [puanlama için bir Azure Machine Learning ardışık hattı oluşturun'a](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, tasarımcıyı kullanarak parametreli bir eğitim ardışık nokta oluşturmayı öğrendiniz.

Öngörülerde bulunmak için bir modeli nasıl dağıtabileceğinize dair tam bir gözden geçirme için, bir regresyon modelini eğitmek ve dağıtmak için [tasarımcı öğreticisine](tutorial-designer-automobile-price-train-score.md) bakın.
