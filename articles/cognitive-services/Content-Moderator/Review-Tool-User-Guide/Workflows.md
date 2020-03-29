---
title: İnceleme aracı yla içerik iş akışlarını tanımlayın ve kullanın - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Azure İçerik Moderatöriş akışı tasarımcısını, içerik ilkelerinize göre özel iş akışları ve eşikleri tanımlamak için kullanabilirsiniz.
services: cognitive-services
author: PatrickFarley
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: pafarley
ms.openlocfilehash: 0be77dc1ac249c37f9b59078451d8fbe35f17458
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72754249"
---
# <a name="define-and-use-moderation-workflows"></a>Moderasyon iş akışlarını tanımlama ve kullanma

Bu kılavuzda, [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com) web sitesinde [iş akışlarının](../review-api.md#workflows) nasıl ayarlanıp kullanılacağını öğreneceksiniz. İş akışları, içeriği daha verimli işlemek için kullanabileceğiniz bulut tabanlı özelleştirilmiş filtrelerdir. İş akışları, içeriği farklı şekillerde filtrelemek ve ardından uygun eylemi yapmak için çeşitli hizmetlere bağlanabilir. Bu kılavuz, içeriği filtrelemek ve tipik bir ılımlılık senaryosunda insan yorumları ayarlamak için İçerik Moderatör bağlayıcısını (varsayılan olarak dahil) nasıl kullanacağınızı gösterir.

## <a name="create-a-new-workflow"></a>Yeni bir iş akışı oluşturma

[İçerik Moderatör İnceleme aracına](https://contentmoderator.cognitive.microsoft.com/) gidin ve oturum açın. **Ayarlar** sekmesinde **İş Akışları'nı**seçin.

![İş akışları ayarı](images/2-workflows-0.png)

Bir sonraki ekranda **İş Akışı Ekle'yi**seçin.

![İş akışı ekleme](images/2-workflows-1.png)

### <a name="assign-a-name-and-description"></a>Ad ve açıklama atama

İş akışınızı adlandırın, bir açıklama girin ve iş akışının görüntüleri veya metni işleyip işlemeyeceğini seçin.

![İş akışı adı ve açıklaması](images/image-workflow-create.PNG)

### <a name="define-evaluation-criteria"></a>Değerlendirme kriterlerini tanımlama

Bir sonraki ekranda **If** bölümüne gidin. Üst açılır menüde **Koşul'u**seçin. Bu, iş akışının hangi durumda işlem yapacağı koşulu yapılandırmanızı sağlar. Birden çok koşul kullanmak istiyorsanız, bunun yerine **Kombinasyon'u** seçin. 

Ardından, bir bağlayıcı seçin. Bu örnekte **İçerik Moderatörü**kullanır. Seçtiğiniz bağlayıcıbağlı olarak, veri çıkışı için farklı seçenekler alırsınız. Diğer bağlayıcıları nasıl ayarlayacaklarını öğrenmek için İnceleme aracı ayarları kılavuzunun [Bağlayıcılar](./configure.md#connectors) bölümüne bakın.

![İş akışı bağlayıcısı seçin](images/image-workflow-connect-to.PNG)

Kullanmak için istenilen çıktıyı seçin ve karşı kontrol etmek için koşulları ayarlayın.

![İş akışı koşulunu tanımlama](images/image-workflow-condition.PNG)

### <a name="define-the-action"></a>Eylemi tanımlama

Bir eylem seçtiğiniz **Then** bölümüne gidin. Aşağıdaki örnek, bir görüntü incelemesi oluşturur ve bir etiket atar. İsteğe bağlı olarak, alternatif (Else) yol ekleyebilir ve bunun için de bir eylem ayarlayabilirsiniz.

![İş akışı eylemini tanımlama](images/image-workflow-action.PNG)

### <a name="save-the-workflow"></a>İş akışını kaydetme

İş akışı adını not edin; İş Akışı API'si ile ılımlılık işine başlamak için ada ihtiyacınız vardır (aşağıya bakın). Son olarak, sayfanın üst kısmındaki **Kaydet** düğmesini kullanarak iş akışını kaydedin.

## <a name="test-the-workflow"></a>İş akışını test etme

Artık özel bir iş akışı tanımladığınıza göre, örnek içerikle test edin. İş **Akışları'na** gidin ve ilgili **İş Akışını Yürüt** düğmesini seçin.

![İş akışı testi](images/image-workflow-execute.PNG)

Bu [örnek görüntüyü](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg) yerel sürücünüze kaydedin. Ardından **Dosya(lar) seçin** ve görüntüyü iş akışına yükleyin.

![Görüntüye bir alıntı eklenmiş bir koşucu](images/sample-text.jpg)

### <a name="track-progress"></a>İlerleme izleme

Bir sonraki açılır pencerede iş akışının ilerlemesini görüntüleyebilirsiniz.

![İş akışı yürütmeyi izleme](images/image-workflow-job.PNG)

### <a name="verify-workflow-action"></a>İş akışı eylemini doğrulama

**Gözden Geçir'in** altındaki **Resim** sekmesine gidin ve yeni oluşturulan bir görüntü incelemesi olduğunu doğrulayın.

![Görüntüleri inceleme](images/image-workflow-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, İçerik Moderatör [İnceleme aracından](https://contentmoderator.cognitive.microsoft.com)ılımlılık iş akışlarını nasıl ayarlayıp kullanacağınızı öğrendiniz. Ardından, iş akışlarının programlı olarak nasıl oluşturulabildiğini öğrenmek için [REST API kılavuzuna](../try-review-api-workflow.md) bakın.
