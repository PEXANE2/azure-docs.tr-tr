---
title: include dosyası
description: include dosyası
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 10/22/2019
ms.openlocfilehash: 5a66212122745d0f4426e48e9487e9d674cec53f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489939"
---
>[!IMPORTANT]
>Diğer Azure Machine Learning öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak oluşturduğunuz kaynakları kullanabilirsiniz.

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz herhangi bir şeyi kullanmayı planlamıyorsanız, herhangi bir ücret ödemeniz için tüm kaynak grubunu silin:

1. Azure portal, pencerenin sol tarafındaki **kaynak grupları** ' nı seçin.
 
   ![Azure portalında kaynak grubunu silme](./media/aml-ui-cleanup/delete-resources.png)

1. Listede, oluşturduğunuz kaynak grubunu seçin.

1. Pencerenin sağ tarafında üç nokta düğmesini ( **...** ) seçin.

1. **Kaynak grubunu sil**'i seçin.

Kaynak grubunun silinmesi, tasarımcıda oluşturduğunuz tüm kaynakları da siler.  

### <a name="delete-only-the-compute-target"></a>Yalnızca işlem hedefini Sil

Burada oluşturduğunuz işlem hedefi, kullanılmadıysa *otomatik olarak sıfır düğümlere otomatik olarak ölçeklendirilir* . Bu, ücretleri en aza indirmektir. İşlem hedefini silmek istiyorsanız şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com), çalışma alanınızı açın.

    ![İşlem hedefini silme](./media/aml-ui-cleanup/delete-compute-target.png)

1. Çalışma alanınızın **işlem** bölümünde kaynağı seçin.

1. **Sil**’i seçin.

### <a name="delete-individual-assets"></a>Tek tek varlıkları Sil

Denemenizi oluşturduğunuz tasarımcıda, her bir varlığı seçerek ve ardından **Sil** düğmesini seçerek tek tek varlıkları silin.

![Varlıkları Sil](./media/aml-ui-cleanup/delete-asset.png)

Veri kümelerinin her bir veri kümesini seçip **kayıt Sil**' i seçerek çalışma alanınızdan kaydı yapılabilir.

![Veri kümesinin kaydını sil](./media/aml-ui-cleanup/unregister-dataset.png)


