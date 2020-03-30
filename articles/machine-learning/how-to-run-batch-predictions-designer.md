---
title: Azure Machine Learning tasarımcısını (önizleme) kullanarak toplu tahminleri çalıştırma
titleSuffix: Azure Machine Learning
description: Bir modeli nasıl eğitecekve tasarımcıyı kullanarak bir toplu iş tahmin ardışık hattı nasıl ayarlayayın. Ardışık hattı, herhangi bir HTTP kitaplığından tetiklenebilen parametrelendirilmiş bir web hizmeti olarak dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: peterlu
author: peterclu
ms.date: 02/24/2020
ms.custom: Ignite2019
ms.openlocfilehash: 01d69bffcf2c17abceba8ba2e0893360bead8b12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477230"
---
# <a name="run-batch-predictions-using-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısını (önizleme) kullanarak toplu tahminleri çalıştırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu makalede, bir toplu iş tahmin ardışık oluşturmak için tasarımcıyı nasıl kullanacağınızı öğrenirsiniz. Toplu oluşturma tahmini, herhangi bir HTTP kitaplığından tetiklenebilen bir web hizmetini kullanarak sürekli olarak isteğe bağlı olarak büyük veri kümeleri elde etmenizi sağlar.

Bu nasıl yapılsa, aşağıdaki görevleri yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Toplu çıkarım ardışık hatlar oluşturma ve yayımlama
> * Bir ardışık nokta uç noktası tüketin
> * Uç nokta sürümlerini yönetme

SDK'yı kullanarak toplu puanlama hizmetlerinin nasıl ayarlangerektiğini öğrenmek için, aşağıdaki [nasıl yapılacağını](how-to-run-batch-predictions.md)öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl zaten bir eğitim boru hattı var sayıyor. Tasarımcı için rehberli bir giriş için, tasarımcı öğretici tam [bir parçası](tutorial-designer-automobile-price-train-score.md). 

## <a name="create-a-batch-inference-pipeline"></a>Toplu çıkarım ardışık işlem hattı oluşturma

Eğitim boru hattınız, çıkarlayıcı bir boru hattı oluşturabilmek için en az bir kez çalıştırılmalıdır.

1. Çalışma alanınızdaki **Tasarımcı** sekmesine gidin.

1. Tahmin de bulunmak için kullanmak istediğiniz modeli eğiten eğitim ardışık hattını seçin.

1. Boru hattını **gönderin.**

    ![Boru hattını gönderme](./media/how-to-run-batch-predictions-designer/run-training-pipeline.png)

Artık eğitim ardışık alanı çalıştırıldığına göre, toplu çıkarım ardışık bir işlem hattı oluşturabilirsiniz.

1. **Gönder'in**yanında, yeni açılır **çıkarım ardışık hattını**seçin.

1. **Toplu çıkarım ardışık hattını**seçin.

    ![Toplu çıkarım ardışık işlem hattı oluşturma](./media/how-to-run-batch-predictions-designer/create-batch-inference.png)
    
Sonuç varsayılan toplu çıkarım ardışık bir iş. 

### <a name="add-a-pipeline-parameter"></a>Boru hattı parametresi ekleme

Yeni verilerle ilgili öngörüler oluşturmak için, bu ardışık işlem kümesi taslak görünümünde farklı bir veri kümesini el ile bağlayabilir veya veri kümeniz için bir parametre oluşturabilirsiniz. Parametreler, çalışma zamanında toplu çıkarTma işleminin davranışını değiştirmenize izin sağlar.

Bu bölümde, öngörülerde bulunmak için farklı bir veri kümesi belirtmek için bir veri kümesi parametresi oluşturursunuz.

1. Veri seti modülünü seçin.

1. Tuvalin sağında bir bölme görünür. Bölmenin alt **kısmında, ardışık parametre olarak Ayarla'yı**seçin.
   
    Parametre için bir ad girin veya varsayılan değeri kabul edin.

## <a name="publish-your-batch-inferencing-pipeline"></a>Toplu iş inizi çıkartma ardışık hattınızı yayımlama

Şimdi çıkaran boru hattını dağıtmaya hazırsınız. Bu, ardışık alanı dağıtacak ve başkalarının kullanması için kullanılabilir hale getirecektir.

1. **Yayımla** düğmesini seçin.

1. Görünen iletişim kutusunda, PipelineEndpoint için açılır noktayı genişletin ve **Yeni PipelineEndpoint'i**seçin. **PipelineEndpoint**

1. Bir bitiş noktası adı ve isteğe bağlı açıklama sağlayın.

    İletişim kutusunun alt kısmında, eğitim sırasında kullanılan veri kümesi kimliğinin varsayılan değeriyle yapılandırdığınız parametreyi görebilirsiniz.

1. **Yayımla**’yı seçin.

![Bir ardışık hat lar yayımlama](./media/how-to-run-batch-predictions-designer/publish-inference-pipeline.png)


## <a name="consume-an-endpoint"></a>Bir bitiş noktası tüketin

Şimdi, veri kümesi parametresi olan yayınlanmış bir ardışık hattınız var. Ardışık etki, parametre olarak sağladığınız veri kümesini puanlamak için eğitim ardışık etki alanında oluşturulan eğitilmiş modeli kullanır.

### <a name="submit-a-pipeline-run"></a>Bir ardışık hat lar çalıştır 

Bu bölümde, bir el ile ardışık ardışık işlem kümesi çalıştıracak ve yeni veriler elde etmek için ardışık hatlar parametresini değiştirin. 

1. Dağıtım tamamlandıktan sonra Bitiş **Noktaları** bölümüne gidin.

1. **Pipeline uç noktalarını**seçin.

1. Oluşturduğunuz bitiş noktasının adını seçin.

![Bitiş noktası bağlantısı](./media/how-to-run-batch-predictions-designer/manage-endpoints.png)

1. **Yayımlanmış ardışık lıkları**seçin.

    Bu ekran, bu bitiş noktası altında yayınlanan tüm yayımlanmış ardışık dizileri gösterir.

1. Yayınladığınız ardışık hattı seçin.

    Ardışık hatlar ayrıntıları sayfası, ardışık hattınız için ayrıntılı bir çalışma geçmişi ve bağlantı dizebilgileri gösterir. 
    
1. Ardışık hattın el ile çalışmasını oluşturmak için **Gönder'i** seçin.

    ![Boru hattı ayrıntıları](./media/how-to-run-batch-predictions-designer/submit-manual-run.png)
    
1. Farklı bir veri kümesi kullanmak için parametreyi değiştirin.
    
1. Ardışık hattı çalıştırmak için **Gönder'i** seçin.

### <a name="use-the-rest-endpoint"></a>REST bitiş noktasını kullanma

**Son Noktalar** bölümünde, ardışık nokta uç noktalarının ve yayımlanmış ardışık hattın nasıl tüketilmesi hakkında bilgi bulabilirsiniz.

Çalışma genel bakış panelinde bir ardışık noktanın REST bitiş noktasını bulabilirsiniz. Bitiş noktasını arayarak, varsayılan olarak yayımlanmış ardışık hattını tüketmiş olursunuz.

**Yayınlanan ardışık hatlar** sayfasında yayımlanmış bir ardışık hatlar da tüketebilirsiniz. Yayımlanmış bir ardışık nokta seçin ve bunun REST bitiş noktasını bulun. 

![Dinlenme bitiş noktası ayrıntıları](./media/how-to-run-batch-predictions-designer/rest-endpoint-details.png)

REST araması yapmak için OAuth 2.0 taşıyıcı tipi kimlik doğrulama üstbilgisine ihtiyacınız olacaktır. Çalışma alanınızda kimlik doğrulamayı ayarlama ve parametreli REST araması yapma hakkında daha fazla ayrıntı için aşağıdaki [öğretici bölümüne](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint) bakın.

## <a name="versioning-endpoints"></a>Uç noktaları n

Tasarımcı, bir bitiş noktasına yayımladığınız sonraki her ardışık noktaya bir sürüm atar. REST aramanızda parametre olarak yürütmek istediğiniz ardışık hatlar ekssürümünü belirtebilirsiniz. Bir sürüm numarası belirtmezseniz, tasarımcı varsayılan ardışık lığı kullanır.

Bir ardışık ardışık yol yayımladığınızda, bu bitiş noktası için yeni varsayılan ardışık nokta yapmayı seçebilirsiniz.

![Varsayılan ardışık lığı ayarlama](./media/how-to-run-batch-predictions-designer/set-default-pipeline.png)

Bitiş noktanızın **Yayımlanmış ardışık hatlar** sekmesinde yeni bir varsayılan ardışık nokta ayarlayabilirsiniz.

![Varsayılan ardışık lığı ayarlama](./media/how-to-run-batch-predictions-designer/set-new-default-pipeline.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir regresyon modelini eğitmek ve dağıtmak için tasarımcı [öğreticisini](tutorial-designer-automobile-price-train-score.md) izleyin.
''