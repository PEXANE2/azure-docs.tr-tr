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
ms.date: 02/24/2020
ms.openlocfilehash: 264b169eefde18880f50feae2554aa3ca7037b1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368171"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısı (önizleme) ile modelleri yeniden eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu nasıl yapılsa makalede, bir makine öğrenme modelini yeniden eğitmek için Azure Machine Learning tasarımcısını nasıl kullanacağınızı öğrenirsiniz. Yeniden eğitim için makine öğrenimi iş akışlarını otomatikleştirmek için yayımlanmış ardışık hatlarınasıl kullanacağız öğrenin.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir makine öğrenme modeli eğitin.
> * Bir ardışık parametre oluşturun.
> * Eğitim boru hattınızı yayınlayın.
> * Modelinizi yeniden eğitin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://aka.ms/AMLFree)oluşturun.
* Enterprise SKU ile Azure Machine Learning çalışma alanı.

Bu makalede, tasarımcı bina boru hatları temel bilgiye sahip olduğunu varsayar. Tasarımcıya rehberli bir giriş [için, öğreticiyi](tutorial-designer-automobile-price-train-score.md)tamamlayın. 

### <a name="sample-pipeline"></a>Örnek boru hattı

Bu makalede kullanılan ardışık hatlar Örnek 3 bulunan bir değiştirilmiş [sürümüdür: Gelir tahmini.](how-to-designer-sample-classification-predict-income.md) Kendi verilerinizi kullanarak bir modeli nasıl eğiteceklerini göstermek için örnek veri kümesi yerine [Veri Alma](algorithm-module-reference/import-data.md) modülünü kullanır.

![İçe Aktarma Verisi modüllerini vurgulayan bir kutuyla değiştirilmiş örnek ardışık ardışık gösteriyi gösteren ekran görüntüsü](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>Bir makine öğrenme modeli eğitin

Bir modeli yeniden eğitmek için, bir başlangıç modeli gerekir. Bu bölümde, tasarımcıyı kullanarak bir modeli nasıl eğiteceklerini ve kaydedilen modele nasıl erişinersiniz.

1. Veri **Alma** modülünü seçin.
1. Özellikler bölmesine bir veri kaynağı belirtin.

   ![Alma Verisi modülünün örnek yapılandırmasını gösteren ekran görüntüsü](./media/how-to-retrain-designer/import-data-settings.png)

   Bu örnekte, veriler bir [Azure veri deposunda](how-to-access-data.md)depolanır. Zaten bir veri mağazanız yoksa, **Yeni veri deposu'nü**seçerek şimdi bir tane oluşturabilirsiniz.

1. Verilerinize giden yolu belirtin. Ayrıca veri mağazanıza göz atmak için **Gözat yolunu** da seçebilirsiniz. 
1. Tuvalin üst kısmında **Çalıştır'ı** seçin.
    
   > [!NOTE]
   > Bu ardışık işlem için varsayılan bir işlem ayarladıysanız, ardışık işlem otomatik olarak çalışır. Aksi takdirde, şimdi bir tane ayarlamak için ayarlar bölmesindeki istemleri izleyebilirsiniz.

### <a name="find-your-trained-model"></a>Eğitimli modelinizi bulun

Tasarımcı, eğitimli modeller de dahil olmak üzere tüm boru hattı çıktılarını varsayılan depolama hesabına kaydeder. Ayrıca, eğitimli modele doğrudan tasarımcıdan erişebilirsiniz:

1. Boru hattının çalışmasını bekleyin.
1. Tren **Modeli** modülünü seçin.
1. Ayarlar bölmesinde **Çıktılar+günlükleri'ni**seçin.
1. Görünüm **çıktısimgesini** seçin ve eğitimli modeli bulmak için açılan penceredeki yönergeyi izleyin.

![Eğitilmiş modelin nasıl indirilir olduğunu gösteren ekran görüntüsü](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="create-a-pipeline-parameter"></a>Bir boru hattı parametresi oluşturma

Çalışma zamanında değişkenleri dinamik olarak ayarlamak için boru hattı parametrelerini ekleyin. Bu ardışık işlem için, yeni bir veri kümesi üzerinde modelinizi yeniden eğitmek için eğitim veri yolu için bir parametre ekleyin.

1. Veri **Alma** modülünü seçin.
1. Ayarlar bölmesinde, **Yol** alanının üzerindeki elipsleri seçin.
1. **Ardışık etki hattına ekle'yi**seçin.
1. Bir parametre adı ve varsayılan değer sağlayın.

   > [!NOTE]
   > Boru hattı taslağınızın başlığının yanındaki **Ayarlar** dişli simgesini seçerek boru hattı parametrelerinizi inceleyebilir ve düzenlemeyapabilirsiniz. 

![Bir ardışık parametrenin nasıl oluşturulurulur gösteren ekran görüntüsü](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>Eğitim boru hattı yayımlama

Bir ardışık hatlar yayımladığında, bir ardışık nokta bitiş noktası oluşturur. Boru hattı uç noktaları, tekrarlanabilirlik ve otomasyon için ardışık hatlarınızı yeniden kullanmanıza ve yönetmenize izin sağlar. Bu örnekte, yeniden eğitim için ardışık hattınızı kurdunuz.

1. Tasarımcı tuvalinin üzerinde **Yayımla'yı** seçin.
1. Bir ardışık lık bitiş noktası seçin veya oluşturun.

   > [!NOTE]
   > Birden çok ardışık hattı tek bir bitiş noktasına yayımlayabilirsiniz. Bitiş noktasındaki her ardışık noktaya, ardışık nokta bitiş noktasını çağırdığınızda belirtebileceğiniz bir sürüm numarası verilir.

1. **Yayımla**’yı seçin.

## <a name="retrain-your-model"></a>Modelinizi yeniden eğitin

Artık yayınlanmış bir eğitim ardışık hattınız olduğuna göre, yeni veriler kullanarak modelinizi yeniden eğitmek için kullanabilirsiniz. Azure portalından bir ardışık bitiş noktasından çalıştırma gönderebilir veya bunları programlı olarak gönderebilirsiniz.

### <a name="submit-runs-by-using-the-designer"></a>Tasarımcıyı kullanarak çalıştırma gönder

Tasarımcıdan bir ardışık nokta bitiş noktası çalıştırın: aşağıdaki adımları kullanın:

1. **Uç Noktalar** sayfasına gidin.
1. Pipeline **uç noktaları** sekmesini seçin.
1. Ardışık nokta bitiş noktanızı seçin.
1. **Yayımlanmış ardışık hatlar** sekmesini seçin.
1. Çalıştırmak istediğiniz ardışık alanı seçin.
1. **Gönder**’i seçin.
1. Kurulum iletişim kutusunda, giriş veri yolu değeri için yeni bir değer belirtebilirsiniz. Bu değer, yeni veri kümenize işaret ediyor.

![Tasarımcıda parametrelendirilmiş bir ardışık etki hattı nın nasıl ayarlanış yapılacağını gösteren ekran görüntüsü](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>Kodu kullanarak çalıştırma gönder

Yayınlanan bir ardışık boru hattının REST bitiş noktasını genel bakış panelinde bulabilirsiniz. Bitiş noktasını arayarak, yayımlanmış ardışık hattı yeniden eğitebilirsiniz.

REST araması yapmak için OAuth 2.0 taşıyıcı tipi kimlik doğrulama üstbilgisine ihtiyacınız vardır. Çalışma alanınızda kimlik doğrulamayı ayarlama ve parametreli REST araması yapma hakkında bilgi için toplu [puanlama için bir Azure Machine Learning ardışık hattı oluşturun'a](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bir regresyon modelini eğitmek ve dağıtmak için [tasarımcı öğreticisini](tutorial-designer-automobile-price-train-score.md) izleyin.
