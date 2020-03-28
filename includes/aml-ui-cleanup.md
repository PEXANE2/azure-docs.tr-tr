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
ms.openlocfilehash: 754c9799ed4c2fd90cbcf1e9717b0be21edd54cb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75659890"
---
>[!IMPORTANT]
>Oluşturduğunuz kaynakları diğer Azure Machine Learning öğreticileri ve nasıl yapılan makaleler için ön koşul olarak kullanabilirsiniz.

### <a name="delete-everything"></a>Her şeyi silme

Oluşturduğunuz herhangi bir şeyi kullanmayı planlamıyorsanız, herhangi bir ücret ödememek için tüm kaynak grubunu silin.

1. Azure portalında, pencerenin sol tarafındaki **Kaynak gruplarını** seçin.
 
   ![Azure portalında kaynak grubunu silme](./media/aml-ui-cleanup/delete-resources.png)

1. Listede, oluşturduğunuz kaynak grubunu seçin.

1. **Kaynak grubunu sil**'i seçin.

Kaynak grubunu silmek, tasarımcıda oluşturduğunuz tüm kaynakları da siler. 

### <a name="delete-individual-assets"></a>Tek tek varlıkları silme

Denemenizi oluşturduğunuz tasarımcıda, varlıkları seçerek ve sonra **Sil** düğmesini seçerek tek tek varlıkları silin.

Burada oluşturduğunuz işlem hedefi, kullanılmadığı zaman otomatik olarak sıfır düğümlere *otomatik ölçeklendirilir.* Bu eylem ücretleri en aza indirmek için alınır.İşlem hedefini silmek istiyorsanız aşağıdaki adımları izleyin:

![Varlıkları silme](./media/aml-ui-cleanup/delete-asset.png)

Her veri kümesini seçerek ve **KayıtTan Çıkar'ı**seçerek çalışma alanınızdan veri kümelerini çıkarabilirsiniz.

![Veri kümesini kayıt dışı bırak](./media/aml-ui-cleanup/unregister-dataset1225.png)

Bir veri kümesini silmek için Azure portalını veya Azure Depolama Gezgini'ni kullanarak depolama hesabına gidin ve bu varlıkları el ile silin.


