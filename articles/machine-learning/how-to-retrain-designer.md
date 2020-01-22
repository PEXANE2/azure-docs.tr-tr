---
title: Azure Machine Learning tasarımcısını kullanarak modelleri yeniden eğitme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında yayımlanmış işlem hatlarına sahip modelleri yeniden eğitme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: peterlu
author: peterclu
ms.date: 12/15/2019
ms.openlocfilehash: d1382da739fd8ca56d4cc53c2c302331bdfbf1c3
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311878"
---
# <a name="retrain-models-with-azure-machine-learning-designer"></a>Azure Machine Learning Designer ile modelleri yeniden eğitme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu nasıl yapılır, bir makine öğrenimi modelini yeniden eğitme Azure Machine Learning tasarımcısını nasıl kullanacağınızı öğrenirsiniz. Makine öğrenimi iş akışlarını yeniden eğitimlere otomatik hale getirmek için yayınlanmış işlem hatlarını nasıl kullanacağınızı öğrenin.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Makine öğrenimi modelini eğitme.
> * İşlem hattı parametresi oluşturun.
> * Eğitim işlem hattınızı yayımlayın.
> * Modelinize yeniden eğitme.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree)oluşturun.

* Kurumsal SKU 'SU olan bir Azure Machine Learning çalışma alanı.

Bu nasıl yapılır, tasarımcıda işlem hattı oluşturmak için temel bilgiye sahip olduğunuzu varsayar. Tasarımcıya yönelik kılavuzlu bir giriş için [öğreticiyi](tutorial-designer-automobile-price-train-score.md)doldurun. 

### <a name="sample-pipeline"></a>Örnek işlem hattı

Bu makalede kullanılan işlem hattı, [örnek 3 gelir tahmininden](how-to-designer-sample-classification-predict-income.md)birinin değiştirilmiş bir sürümüdür. Kendi verilerinizi kullanarak bir modeli nasıl eğitekullanacağınızı göstermek için örnek veri kümesi yerine [veri alma](algorithm-module-reference/import-data.md) modülünü kullanır.

![Veri alma modülünü vurgulama kutusu ile değiştirilen örnek işlem hattını gösteren ekran görüntüsü](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>Makine öğrenimi modelini eğitme

Bir modeli yeniden eğitmek için bir başlangıç modeline ihtiyacınız vardır. Bu bölümde, bir modeli eğitme ve tasarımcı kullanarak kaydedilmiş modele erişme hakkında bilgi edineceksiniz.

1. **Veri Içeri aktarma** modülünü seçin.
1. Özellikler bölmesinde bir veri kaynağı belirtin.

    ![Veri Içeri aktarma modülünün örnek yapılandırmasını gösteren ekran görüntüsü](./media/how-to-retrain-designer/import-data-settings.png)

    Bu örnekte, veriler bir [Azure veri deposunda](how-to-access-data.md)depolanır. Henüz bir veri deposu yoksa, **Yeni veri deposu**' nu seçerek şimdi bir tane oluşturabilirsiniz.

1. Verilerinizin yolunu belirtin. Veri deposundan görsel olarak gezinmek için, **yolu araştır** ' ı da seçebilirsiniz. 

1. Tuvalin en üstünde **Çalıştır** ' ı seçin.
    
    > [!NOTE]
    > Bu ardışık düzen taslağı için zaten varsayılan bir işlem ayarladıysanız, işlem hattı otomatik olarak çalışır. Aksi takdirde, şimdi ayarlanmış bir şekilde ayarlanan ayarlar bölmesindeki istemler ' i izleyebilirsiniz.

### <a name="locate-your-trained-model"></a>Eğitilen modelinizi bulun

Tasarımcı, eğitilen modeller dahil olmak üzere tüm işlem hattı çıkışlarını varsayılan depolama hesabına kaydeder. Eğitilen modele doğrudan tasarımcıda de erişebilirsiniz:

1. İşlem hattının çalışmayı bitirmesini bekleyin.

1. **Model eğitme** modülünü seçin.

1. Ayarlar bölmesinde, **çıktılar**' i seçin.

1. Modeli indirmek için **Trained_model** seçin.

![Eğitilen modelin nasıl indirileceği gösteren ekran görüntüsü](./media/how-to-retrain-designer/download-model.png)

## <a name="create-a-pipeline-parameter"></a>İşlem hattı parametresi oluşturma

Çalışma zamanında değişkenleri dinamik olarak ayarlamak için işlem hattı parametreleri ekleyin. Bu işlem hattı için, modelinizi yeni bir veri kümesi üzerinde yeniden eğitebilmeniz için eğitim verileri yolu için bir parametre ekleyin.

1. **Veri Içeri aktarma** modülünü seçin.
1. Ayarlar bölmesinde, **yol** alanının üzerindeki üç nokta simgesini seçin.
1. **Ardışık düzen parametresine Ekle**' yi seçin.
1. Bir parametre adı ve varsayılan değer sağlayın.

    > [!NOTE]
    > İşlem hattı taslağınız başlığının yanındaki **Ayarlar dişli simgesini** seçerek işlem hattı parametrelerinizi inceleyebilir ve düzenleyebilirsiniz. 

![İşlem hattı parametresinin nasıl oluşturulacağını gösteren ekran görüntüsü](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>Eğitim işlem hattı yayımlama

Bir işlem hattını yayımladığınızda, bir ardışık düzen uç noktası oluşturur. İşlem hattı uç noktaları, yinelenebilirlik ve otomasyon için işlem hatlarınızı yeniden kullanmanıza ve yönetmenize olanak sağlar Bu örnekte, yeniden eğitim için işlem hattınızı kurun.

1. Tasarımcı tuvalinin üzerinde **Yayımla** ' yı seçin.
1. Yeni bir ardışık düzen uç noktası seçin veya oluşturun.

    > [!NOTE]
    > Tek bir uç noktada birden çok işlem hattı yayımlayabilirsiniz. Uç noktadaki her bir işlem hattı, işlem hattı uç noktasını çağırdığınızda belirtebileceğiniz bir sürüm numarası olarak verilir.

1. **Yayımla**’yı seçin.

## <a name="retrain-your-model"></a>Modelinizi yeniden eğitme

Artık yayımlanmış bir eğitim işlem hattına sahip olduğunuza göre, yeni verileri kullanarak modelinize yeniden eğitebilmeniz için bunu kullanabilirsiniz. Çalışan bir ardışık düzen uç noktasından, portaldan veya program aracılığıyla bir işlem hattı gönderebilirsiniz.

### <a name="submit-runs-with-the-designer"></a>Tasarımcılarla çalıştırmaları gönderme

Tasarımcıdan bir işlem hattı uç noktası göndermek için aşağıdaki adımları kullanın:

1. **Uç noktalar** sayfasına gidin.

1. **Ardışık düzen uç noktaları** sekmesini seçin.

1. Ardışık düzen uç noktanızı seçin.

1. Yayınlanan işlem **hatları** sekmesini seçin.

1. Çalıştırmak istediğiniz işlem hattını seçin.

1. **Çalıştır**'ı seçin.

1. Kurulum iletişim kutusunda yeni veri kümenizi işaret eden yeni bir giriş veri yolu değeri belirtebilirsiniz.

![Tasarımcıda parametreli bir işlem hattı çalıştırmasının nasıl ayarlanacağını gösteren ekran görüntüsü](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-with-code"></a>Çalışmaları kodla gönder

Geliştirme ortamınıza bağlı olarak REST uç nokta program aracılığıyla erişiminizin birden çok yolu vardır. İşlem hattınızı **kullanma sekmesinde Parametreler** içeren işlem hattı çalıştırmalarını nasıl gönderirsiniz gösteren kod örnekleri bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir regresyon modeli eğitme ve dağıtma için tasarımcı [öğreticisini](tutorial-designer-automobile-price-train-score.md) izleyin.
