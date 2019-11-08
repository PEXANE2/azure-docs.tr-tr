---
title: include dosyası
description: include dosyası
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 11/06/2019
ms.openlocfilehash: 66f5c72fcabb62e21f0110cb981b7271244c0648
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73800195"
---
>[!IMPORTANT]
>Diğer Azure Machine Learning öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak oluşturduğunuz kaynakları kullanabilirsiniz.

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz herhangi bir şeyi kullanmayı planlamıyorsanız, herhangi bir ücret ödemeniz için tüm kaynak grubunu silin:

1. Azure portal, pencerenin sol tarafındaki **kaynak grupları** ' nı seçin.
 
   ![Azure portalında kaynak grubunu silme](./media/aml-ui-cleanup/delete-resources.png)

1. Listede, oluşturduğunuz kaynak grubunu seçin.

1. **Kaynak grubunu sil**'i seçin.

Kaynak grubunun silinmesi, tasarımcıda oluşturduğunuz tüm kaynakları da siler.  

### <a name="delete-individual-assets"></a>Tek tek varlıkları Sil

Denemenizi oluşturduğunuz tasarımcıda, her bir varlığı seçerek ve ardından **Sil** düğmesini seçerek tek tek varlıkları silin.

Burada oluşturduğunuz işlem hedefi, kullanılmadıysa *otomatik olarak sıfır düğümlere otomatik olarak ölçeklendirilir* . Bu, ücretleri en aza indirmektir. İşlem hedefini silmek istiyorsanız şu adımları uygulayın:

![Varlıkları Sil](./media/aml-ui-cleanup/delete-asset.png)

Veri kümelerinin her bir veri kümesini seçip **kayıt Sil**' i seçerek çalışma alanınızdan kaydı yapılabilir.

![Veri kümesinin kaydını sil](./media/aml-ui-cleanup/unregister-dataset.png)

Bir veri kümesini silmek için Azure portal veya Depolama Gezgini kullanarak depolama hesabına gidin ve bu varlıkları el ile silin.


