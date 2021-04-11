---
title: Çok yönlü işlem hatları derlemek için tasarımcıda işlem hattı parametrelerini kullanma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında işlem hattı parametrelerini kullanma.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: keli19
author: likebupt
ms.date: 03/19/2021
ms.topic: conceptual
ms.custom: how-to, designer
ms.openlocfilehash: 09eabffb0e01ee6c5ea6b541378773a7d60397a3
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106080624"
---
# <a name="use-pipeline-parameters-in-the-designer-to-build-versatile-pipelines"></a>Çok yönlü işlem hatları derlemek için tasarımcıda işlem hattı parametrelerini kullanma

Tasarımcıda esnek işlem hatları oluşturmak için işlem hattı parametreleri kullanın. İşlem hattı parametreleri, işlem hattı mantığını kapsüllemek ve varlıkları yeniden kullanmak için çalışma zamanında dinamik değerleri ayarlamanıza olanak sağlar

İşlem hattı parametreleri özellikle bir işlem hattı çalıştırma, [yeniden eğitim modeli](how-to-retrain-designer.md)veya [toplu](how-to-run-batch-predictions-designer.md)tahmine dayalı olarak yeniden gönderilirken faydalıdır.

Bu makalede, aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * İşlem hattı parametreleri oluşturma
> * İşlem hattı parametrelerini silin ve yönetin
> * İşlem hattı parametrelerini ayarlarken işlem hattı çalıştırmalarını tetikleyin

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

* Tasarımcıya yönelik kılavuzlu bir giriş için [Tasarımcı öğreticisini](tutorial-designer-automobile-price-train-score.md)doldurun. 

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-pipeline-parameter"></a>Ardışık düzen parametresi oluştur

Tasarımcıda bir ardışık düzen parametresi oluşturmanın üç yolu vardır:
- Ayarlar panelinde bir işlem hattı parametresi oluşturun ve bunu bir modüle bağlayın.
- Bir modül parametresini bir işlem hattı parametresine yükseltin.
- Veri kümesini bir işlem hattı parametresine yükseltme

> [!NOTE]
> İşlem hattı parametreleri yalnızca, ve gibi temel veri türlerini destekler `int` `float` `string` .

### <a name="option-1-create-a-pipeline-parameter-in-the-settings-panel"></a>Seçenek 1: Ayarlar panelinde bir işlem hattı parametresi oluşturma

Bu bölümde, Ayarlar panelinde bir işlem hattı parametresi oluşturursunuz.

Bu örnekte, bir işlem hattının eksik verileri **Temizleme eksik veri** modülünü kullanarak nasıl doldurulacağını tanımlayan bir işlem hattı parametresi oluşturursunuz.

1. İşlem hattı taslağlarınızın adının yanındaki **dişli simgesini** seçerek **Ayarlar** panelini açın.

1. İşlem **hattı parametreleri** bölümünde, **+** simgesini seçin.

1.  Parametre için bir ad ve varsayılan değer girin. 

    Örneğin, `replace-missing-value` parametre adı ve `0` varsayılan değer olarak girin.

   ![İşlem hattı parametresinin nasıl oluşturulacağını gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/create-pipeline-parameter.png)


Bir işlem hattı parametresi oluşturduktan sonra, dinamik olarak ayarlamak istediğiniz [Modül parametresine iliştirmeniz](#attach-module-parameter-to-pipeline-parameter) gerekir.

### <a name="option-2-promote-a-module-parameter"></a>Seçenek 2: bir modül parametresini yükseltme

Modül değeri için bir ardışık düzen parametresi oluşturmanın en basit yolu bir modül parametresini yükseltedir. Bir modül parametresini bir işlem hattı parametresine yükseltmek için aşağıdaki adımları kullanın:

1. Bir işlem hattı parametresi eklemek istediğiniz modülü seçin.
1. Modül ayrıntısı bölmesinde, belirtmek istediğiniz parametre üzerinde Mouse.
1. Görüntülenen üç nokta (**...**) simgesini seçin.
1. **Ardışık düzen parametresine Ekle**' yi seçin.

    ![Modül parametresinin işlem hattına nasıl yükseltileceğini gösteren ekran görüntüsü parametre1](media/how-to-use-pipeline-parameter/promote-module-para-to-pipeline-para.png)

1. Bir parametre adı ve varsayılan değer girin.
1. **Kaydet**’i seçin

Artık bu işlem hattını gönderdiğinizde, bu parametre için yeni değerler belirtebilirsiniz.

### <a name="option-3-promote-a-dataset-to-a-pipeline-parameter"></a>Seçenek 3: bir veri kümesini bir işlem hattı parametresine yükseltme

İşlem hattınızı değişken veri kümeleriyle göndermek istiyorsanız, veri kümenizi bir ardışık düzen parametresine yükseltmeniz gerekir:

1. Bir işlem hattı parametresine açmak istediğiniz veri kümesini seçin.

1. Veri kümesinin ayrıntı panelinde, işlem **hattı parametresi olarak ayarla**' yı işaretleyin.

   ![Veri kümesinin ardışık düzen parametresi olarak nasıl ayarlanacağını gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/set-dataset-as-pipeline-parameter.png)

Artık işlem hattını bir sonraki çalıştırışınızda işlem hattı parametresini kullanarak farklı bir veri kümesi belirtebilirsiniz.

## <a name="attach-module-parameter-to-pipeline-parameter"></a>İşlem hattı parametresine modül parametresi iliştirme 

Bu bölümde, işlem hattı parametresine modül parametresi eklemeyi öğreneceksiniz.

İşlem hattı çalıştırmasını tetiklerken değeri bir kez değiştirmek istiyorsanız, yinelenen modüllerin aynı modül parametrelerini aynı işlem hattı parametresine ekleyebilirsiniz.

Aşağıdaki örnek yinelenen **Temizleme eksik veri** modülünü içerir. Her **Temizleme eksik veri** modülü Için, **değiştirme değerini** işlem hattı parametresine iliştirin **değiştirme-eksik-değer**:

1. **Eksik verileri temizle** modülünü seçin.

1. Sağ taraftaki modül ayrıntısı bölmesinde, **Temizleme modunu** "özel değiştirme değeri" olarak ayarlayın.

1. **Değiştirme değeri** alanının üzerinde Mouse.

1. Görüntülenen üç nokta (**...**) simgesini seçin.

1. İşlem hattı parametresini seçin `replace-missing-value` .

   ![İşlem hattı parametresinin nasıl iliştirigösterdiğini gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/attach-replace-value-to-pipeline-parameter.png)

**Değiştirme değeri** alanını işlem hattı parametreye başarıyla eklediniz. Modüllerindeki **değiştirme değeri** eylem yapılamayan değildir.

 ![İşlem hattı parametresine iliştirdikten sonra eylem yapılamayan gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/non-actionable-module-parameter.png)


## <a name="update-and-delete-pipeline-parameters"></a>İşlem hattı parametrelerini güncelleştirme ve silme

Bu bölümde, işlem hattı parametrelerini güncelleştirmeyi ve silmeyi öğreneceksiniz.

### <a name="update-pipeline-parameters"></a>İşlem hattı parametrelerini güncelleştirme

Modül ardışık düzen parametresini güncelleştirmek için aşağıdaki adımları kullanın:

1. Tuvalin en üstünde dişli simgesini seçin.
1. İşlem **hattı parametreleri** bölümünde, tüm işlem hattı parametreleriniz için adı ve varsayılan değeri görüntüleyebilir ve güncelleştirebilirsiniz.

### <a name="delete-a-dataset-pipeline-parameter"></a>Veri kümesi ardışık düzen parametresini silme

Bir veri kümesi ardışık düzen parametresini ayırmak için aşağıdaki adımları kullanın:

1. Veri kümesi modülünü seçin.
1. İşlem **hattı parametresi olarak ayarla** seçeneğinin işaretini kaldırın.


### <a name="delete-module-pipeline-parameters"></a>Modül ardışık düzen parametrelerini Sil

Modül ardışık düzen parametresini silmek için aşağıdaki adımları kullanın:

1. Tuvalin en üstünde dişli simgesini seçin.

1. İşlem hattı parametresinin yanındaki üç nokta (**...**) simgesini seçin.

    Bu görünüm, ardışık düzen parametresinin iliştirildiği modülleri gösterir. Bir işlem hattı parametresini silmek için önce herhangi bir modül parametrelerinden ayırmanız gerekir.

    ![Bir modüle uygulanan geçerli işlem hattı parametresini gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/current-pipeline-parameter.png)

1. Tuvalde, işlem hattı parametresinin hala bağlı olduğu bir modül seçin.
1. Sağdaki modül özellikleri bölmesinde, işlem hattı parametresinin eklendiği alanı bulun.
1. Eklenmiş alanın üzerine Mouse. Ardından görüntülenen üç nokta (**...**) simgesini seçin.
1. İşlem **hattı parametresinden ayır 'ı** seçin

    ![İşlem hattı parametrelerinden ayırmayı gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/detach-from-pipeline-parameter.png)

1. Tüm alanlardan ardışık düzen parametresini ayırana kadar önceki adımları yineleyin.
1. İşlem hattı parametresinin yanındaki üç nokta (**...**) simgesini seçin.
1. Ardışık düzen parametresini silmek için **Parametreyi sil** ' i seçin.

    ![İşlem hattı parametrelerini silmeyi gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/delete-pipeline-parameter.png)

## <a name="trigger-a-pipeline-run-with-pipeline-parameters"></a>İşlem hattı parametreleriyle işlem hattı çalıştırmasını tetikleme 

Bu bölümde, işlem hattı parametrelerini ayarlarken işlem hattı çalıştırmasını nasıl gönderebileceğinizi öğreneceksiniz.

### <a name="resubmit-a-pipeline-run"></a>İşlem hattı çalıştırmasını yeniden gönderin

İşlem hattı parametreleriyle bir işlem hattı gönderdikten sonra, farklı parametrelerle bir işlem hattı çalıştırmasını yeniden gönderebilirsiniz:

1. İşlem hattı ayrıntısı sayfasına gidin. İşlem **hattı çalıştırma genel bakış** penceresinde, geçerli işlem hattı parametrelerini ve değerlerini kontrol edebilirsiniz.

1. Yeniden **Gönder**' i seçin.
1. Kurulum işlem **hattı çalıştırmasında** yeni işlem hattı parametrelerinizi belirtin. 

![İşlem hattını ardışık düzen parametreleriyle yeniden gönder ' i gösteren ekran görüntüsü](media/how-to-use-pipeline-parameter/resubmit-pipeline-run.png)

### <a name="use-published-pipelines"></a>Yayınlanan işlem hatlarını kullanma

İşlem hattı parametrelerini kullanmak için de bir işlem hattı yayımlayabilirsiniz. Yayımlanmış bir işlem **hattı** , istemci UYGULAMALARıN bir REST uç noktası aracılığıyla çağırabileceği bir işlem kaynağına dağıtılan bir ardışık düzen.

Yayımlanan uç noktalar, özellikle yeniden eğitim ve toplu tahmin senaryoları için yararlıdır. Daha fazla bilgi için bkz. [tasarımcıda modelleri yeniden eğitme](how-to-retrain-designer.md) veya [tasarımcıda toplu tahminleri çalıştırma](how-to-run-batch-predictions-designer.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, tasarımcıda işlem hattı parametreleri oluşturmayı öğrendiniz. Daha sonra, [modelleri yeniden eğitme](how-to-retrain-designer.md) veya [toplu tahminleri gerçekleştirme işlem](how-to-run-batch-predictions-designer.md)hattı parametrelerini nasıl kullanabileceğinizi öğrenin.

Ayrıca [, işlem HATLARıNı SDK ile programlama yoluyla nasıl kullanacağınızı](how-to-deploy-pipelines.md)da öğrenebilirsiniz.
