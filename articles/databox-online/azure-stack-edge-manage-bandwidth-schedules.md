---
title: Azure Stack Edge bant genişliği zamanlamalarını yönetme | Microsoft Docs
description: Azure Stack Kenarunuzdaki bant genişliği zamanlamalarını yönetmek için Azure portal nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: a21828413b8d8750827fafa077002863439f8643
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82569595"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge"></a>Azure Stack Kenarunuzdaki bant genişliği zamanlamalarını yönetmek için Azure portal kullanın  

Bu makalede Azure Stack kenardaki kullanıcıların nasıl yönetileceği açıklanır. Bant genişliği zamanlamaları, ağ bant genişliği kullanımını birden çok zamanlamaya göre yapılandırmanızı sağlar. Bu zamanlamalar, cihazınızla bulut arasında gerçekleştirilen yükleme ve indirme işlemlerine uygulanabilir.

Azure Stack Kenarlarınızın bant genişliği zamanlamalarını Azure portal aracılığıyla ekleyebilir, değiştirebilir veya silebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Zamanlama ekleme
> * Zamanlamayı değiştirme
> * Zamanlamayı silme


## <a name="add-a-schedule"></a>Zamanlama ekleme

Bir zamanlama eklemek için Azure portal aşağıdaki adımları uygulayın.

1. Azure Stack Edge kaynağınız için Azure portal **bant genişliğine**gidin.
2. Sağ bölmede **+ zamanlama Ekle**' yi seçin.

    ![Bant genişliği seçin](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. **Zamanlama ekle** sayfasında: 

   1. Zamanlamanın **Başlangıç günü**, **Bitiş günü**, **Başlangıç saati** ve **Bitiş saati** değerlerini belirleyin.
   2. Bu zamanlamanın her gün çalışması gerekiyorsa, **tüm gün** seçeneğini işaretleyin.
   3. **Bant genişliği hızı**, cihazınızda gerçekleştirilen bulutla ilgili işlemler (yükleme ve indirme) için kullanılan bant genişliğidir ve saniye başına megabit (Mb/sn) cinsinden ölçülür. Bu alan için 20 ile 1.000.000.007 arasında bir sayı girin.
   4. Veri yükleme ve indirme işlemlerini kısıtlamak istemiyorsanız **Sınırsız** bant genişliğini seçin.
   5. **Add (Ekle)** seçeneğini belirleyin.

      ![Zamanlama Ekle](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Belirtilen parametrelerle bir zamanlama oluşturulur. Bu zamanlama daha sonra portaldaki bant genişliği zamanlamaları listesinde görüntülenir.

    ![Bant genişliği zamanlamalarının güncelleştirilmiş listesi](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Zamanlamayı düzenleme

Bir bant genişliği zamanlamasını düzenlemek için aşağıdaki adımları gerçekleştirin.

1. Azure portal, Azure Stack Edge kaynağına gidin ve ardından **bant genişliğine**gidin. 
2. Bant genişliği zamanlamaları listesinden, değiştirmek istediğiniz bir zamanlamayı seçin ve seçin.
    ![Bant genişliği zamanlamasını seçin](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. İstediğiniz değişiklikleri yapın ve değişiklikleri kaydedin.

    ![Kullanıcıyı değiştirme](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. Zamanlama değiştirildikten sonra zamanlama listesi değiştirilen zamanlamayı gösterecek şekilde güncelleştirilir.

    ![Kullanıcıyı değiştirme](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Zamanlamayı silme

Azure Stack Edge cihazlarınız ile ilişkili bir bant genişliği zamanlamasını silmek için aşağıdaki adımları uygulayın.

1. Azure portal, Azure Stack Edge kaynağına gidin ve ardından **bant genişliğine**gidin.  

2. Bant genişliği zamanlaması listesinden silmek istediğiniz zamanlamayı seçin. **Düzenleme zamanlamasında** **Sil**' i seçin. Onay istendiğinde **Evet**' i seçin.

   ![Kullanıcı silme](media/azure-stack-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. Zamanlama silindikten sonra zamanlama listesi güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar

- [Paylaşımları yönetmeyi](azure-stack-edge-manage-shares.md)öğrenin.
