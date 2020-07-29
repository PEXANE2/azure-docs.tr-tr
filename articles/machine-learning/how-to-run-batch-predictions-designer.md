---
title: Azure Machine Learning tasarımcısını (önizleme) kullanarak toplu tahminleri çalıştırma
titleSuffix: Azure Machine Learning
description: Tasarımcıyı kullanarak bir modeli eğitme ve Batch tahmini işlem hattı ayarlama hakkında bilgi edinin. İşlem hattını, herhangi bir HTTP kitaplığından tetiklenebilecek parametreli bir Web hizmeti olarak dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: peterlu
author: peterclu
ms.date: 02/24/2020
ms.topic: conceptual
ms.custom: how-to, designer
ms.openlocfilehash: 61465210c9ad476c6d8d2987330498aa0efa39d4
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319618"
---
# <a name="run-batch-predictions-using-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısını (önizleme) kullanarak toplu tahminleri çalıştırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu makalede, bir Batch tahmin işlem hattı oluşturmak için tasarımcıyı nasıl kullanacağınızı öğreneceksiniz. Toplu tahmin, herhangi bir HTTP kitaplığından tetiklenebilecek bir Web hizmeti kullanarak isteğe bağlı olarak büyük veri kümelerini sürekli olarak puanlamanızı sağlar.

Bu nasıl yapılır, aşağıdaki görevleri yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Toplu çıkarım ardışık düzeni oluşturma ve yayımlama
> * Ardışık düzen uç noktası kullanma
> * Uç nokta sürümlerini yönetme

SDK 'yı kullanarak Batch Puanlama hizmetlerini ayarlamayı öğrenmek için bkz. [ile ilgili nasıl yapılır](how-to-run-batch-predictions.md).

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır, zaten bir eğitim ardışık düzenine sahip olduğunuzu varsayar. Tasarımcıya yönelik Kılavuzlu giriş için [Tasarımcı öğreticisinin birinci kısmını](tutorial-designer-automobile-price-train-score.md)doldurun. 

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-batch-inference-pipeline"></a>Toplu çıkarım ardışık düzeni oluşturma

Bir ınsele sınırlama işlem hattı oluşturabilmek için eğitim işlem hattının en az bir kez çalıştırılması gerekir.

1. Çalışma alanınızdaki **Tasarımcı** sekmesine gidin.

1. Tahmin yapmak için kullanmak istediğiniz modeli gösteren eğitim işlem hattını seçin.

1. İşlem hattını **gönderme** .

    ![İşlem hattını gönderme](./media/how-to-run-batch-predictions-designer/run-training-pipeline.png)

Artık eğitim işlem hattı çalıştırıldığına göre, bir toplu çıkarım ardışık düzeni oluşturabilirsiniz.

1. **Gönder**' in yanındaki yeni açılan menü **oluşturma çıkarım**işlem hattını seçin.

1. **Toplu çıkarım ardışık düzeni**' ni seçin.

    ![Toplu çıkarım ardışık düzeni oluşturma](./media/how-to-run-batch-predictions-designer/create-batch-inference.png)
    
Sonuç, varsayılan bir Batch çıkarım ardışık düzeni olur. 

### <a name="add-a-pipeline-parameter"></a>İşlem hattı parametresi ekleme

Yeni veriler üzerinde tahminler oluşturmak için bu işlem hattı Taslak görünümünde farklı bir veri kümesini el ile bağlayabilirsiniz ya da veri kümeniz için bir parametre oluşturabilirsiniz. Parametreler, çalışma zamanında toplu iş örneği işleme işleminin davranışını değiştirmenize izin verir.

Bu bölümde, tahminleri yapmak için farklı bir veri kümesi belirtmek üzere bir veri kümesi parametresi oluşturacaksınız.

1. Veri kümesi modülünü seçin.

1. Tuvalin sağında bir bölme belirir. Bölmenin en altında, **ardışık düzen parametresi olarak ayarla**' yı seçin.
   
    Parametre için bir ad girin veya varsayılan değeri kabul edin.

## <a name="publish-your-batch-inferencing-pipeline"></a>Batch ınkoya sınırlama işlem hattınızı yayımlayın

Şimdi de ınıri sınırlama işlem hattını dağıtmaya hazırsınız. Bu işlem hattı dağıtır ve başkalarının kullanması için kullanılabilir hale getirir.

1. **Yayımla** düğmesini seçin.

1. Görüntülenen iletişim kutusunda, **pipelineendpoint**için açılan liste ' yi genişletin ve **Yeni bir pipelineendpoint**' i seçin.

1. Bir uç nokta adı ve isteğe bağlı bir açıklama sağlayın.

    İletişim kutusunun alt kısmında, eğitim sırasında kullanılan veri kümesi KIMLIĞI için varsayılan bir değerle yapılandırdığınız parametreyi görebilirsiniz.

1. **Yayımla**’yı seçin.

![İşlem hattı yayımlama](./media/how-to-run-batch-predictions-designer/publish-inference-pipeline.png)


## <a name="consume-an-endpoint"></a>Uç nokta kullanma

Şimdi, bir veri kümesi parametresi olan yayımlanmış bir işlem hattına sahipsiniz. İşlem hattı, bir parametre olarak sağladığınız veri kümesini öğrenmek için eğitim ardışık düzeninde oluşturulan eğitilen modeli kullanır.

### <a name="submit-a-pipeline-run"></a>İşlem hattı çalıştırması gönderme 

Bu bölümde, bir el ile işlem hattı çalıştırması ayarlayacaksınız ve yeni verileri Puanlama için işlem hattı parametresini değiştirecek olursunuz. 

1. Dağıtım tamamlandıktan sonra **uç noktalar** bölümüne gidin.

1. **Ardışık düzen uç noktalarını**seçin.

1. Oluşturduğunuz uç noktanın adını seçin.

![Uç nokta bağlantısı](./media/how-to-run-batch-predictions-designer/manage-endpoints.png)

1. **Yayınlanan işlem hatlarını**seçin.

    Bu ekran, bu uç nokta altında Yayınlanan Tüm yayınlanan işlem hatlarını gösterir.

1. Yayımladığınız işlem hattını seçin.

    İşlem hattı ayrıntıları sayfasında, işlem hattınızla ilgili ayrıntılı çalıştırma geçmişi ve bağlantı dizesi bilgileri görüntülenir. 
    
1. İşlem hattının el ile çalıştırılmasını oluşturmak için **Gönder** ' i seçin.

    ![Ardışık düzen ayrıntıları](./media/how-to-run-batch-predictions-designer/submit-manual-run.png)
    
1. Parametresini farklı bir veri kümesi kullanacak şekilde değiştirin.
    
1. İşlem hattını çalıştırmak için **Gönder** ' i seçin.

### <a name="use-the-rest-endpoint"></a>REST uç noktasını kullanma

**Uç noktalar** bölümünde, ardışık düzen uç noktaları ve yayımlanan işlem hattının nasıl kullanılacağına ilişkin bilgileri bulabilirsiniz.

Bir işlem hattının REST uç noktasını çalışma genel bakış panelinde bulabilirsiniz. Uç noktasını çağırarak, varsayılan yayımlanmış işlem hattını kullanıyor olursunuz.

Yayımlanan **ardışık düzen sayfasında yayımlanmış** bir işlem hattını de kullanabilirsiniz. Yayımlanmış bir işlem hattı seçin ve bunun REST uç noktasını bulun. 

![REST uç noktası ayrıntıları](./media/how-to-run-batch-predictions-designer/rest-endpoint-details.png)

REST çağrısı yapmak için bir OAuth 2,0 taşıyıcı türü kimlik doğrulama üst bilgisi gerekir. Çalışma alanınıza yönelik kimlik doğrulamasını ayarlama ve parametreli bir REST çağrısı yapma hakkında daha fazla ayrıntı için aşağıdaki [öğretici bölümüne](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint) bakın.

## <a name="versioning-endpoints"></a>Sürüm uç noktaları

Tasarımcı, bir uç noktada yayımladığınız her bir sonraki ardışık düzen için bir sürüm atar. REST çağrınızda bir parametre olarak yürütmek istediğiniz işlem hattı sürümünü belirtebilirsiniz. Sürüm numarası belirtmezseniz, tasarımcı varsayılan işlem hattını kullanır.

Bir işlem hattını yayımladığınızda, bu uç nokta için yeni varsayılan işlem hattını yapmayı tercih edebilirsiniz.

![Varsayılan işlem hattını ayarla](./media/how-to-run-batch-predictions-designer/set-default-pipeline.png)

Ayrıca, uç noktanızın **yayınlanan ardışık düzen** sekmesinde yeni bir varsayılan işlem hattı da ayarlayabilirsiniz.

![Varsayılan işlem hattını ayarla](./media/how-to-run-batch-predictions-designer/set-new-default-pipeline.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir regresyon modeli eğitme ve dağıtma için tasarımcı [öğreticisini](tutorial-designer-automobile-price-train-score.md) izleyin.
''