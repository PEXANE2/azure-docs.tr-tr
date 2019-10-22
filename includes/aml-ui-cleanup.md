---
title: include dosyası
description: include dosyası
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 10/16/2019
ms.openlocfilehash: 0071b0df2c2e173eced1722372f88b1de2708afa
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692280"
---
>[!IMPORTANT]
>Diğer Azure Machine Learning hizmet öğreticileri ve nasıl yapılır makaleleri için önkoşul olarak oluşturduğunuz kaynakları kullanabilirsiniz.

### <a name="delete-everything"></a>Her şeyi sil

Oluşturduğunuz herhangi bir şeyi kullanmayı planlamıyorsanız, herhangi bir ücret ödemeniz için tüm kaynak grubunu silin:

1. Azure portal, pencerenin sol tarafındaki **kaynak grupları** ' nı seçin.
 
   ![Azure portalında kaynak grubunu silme](./media/aml-ui-cleanup/delete-resources.png)

1. Listede, oluşturduğunuz kaynak grubunu seçin.

1. Pencerenin sağ tarafında üç nokta düğmesini ( **...** ) seçin.

1. **Kaynak grubunu sil**'i seçin.

Kaynak grubu silindiğinde, görsel arabirimde oluşturduğunuz tüm kaynaklar da silinir.  

### <a name="delete-only-the-compute-target"></a>Yalnızca işlem hedefini Sil

Burada oluşturduğunuz işlem hedefi, kullanılmadıysa *otomatik olarak sıfır düğümlere otomatik olarak ölçeklendirilir* . Bu, ücretleri en aza indirmektir. İşlem hedefini silmek istiyorsanız şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com), çalışma alanınızı açın.

    ![İşlem hedefini silme](./media/aml-ui-cleanup/delete-compute-target.png)

1. Çalışma alanınızın **işlem** bölümünde kaynağı seçin.

1. **Sil**’i seçin.

### <a name="delete-individual-assets"></a>Tek tek varlıkları Sil

Denemenizi oluşturduğunuz görsel arabirimde, her bir varlığı seçerek ve ardından **Sil** düğmesini seçerek tek tek varlıkları silin. Veri kümelerinin her bir veri kümesini seçip **kayıt Sil**' i seçerek çalışma alanınızdan kaydı yapılabilir.

![Varlıkları Sil](./media/aml-ui-cleanup/delete-asset.png)
