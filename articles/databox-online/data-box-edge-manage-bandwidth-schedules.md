---
title: Azure Veri Kutusu Kenarı bant genişliği zamanlamalarını yönetir | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarı'ndaki bant genişliği zamanlamalarını yönetmek için Azure portalını nasıl kullanacağınızı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: f7b762d5502986c306de240519688aa639f58445
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60756890"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-data-box-edge"></a>Azure Veri Kutusu Kenarı'ndaki bant genişliği zamanlamalarını yönetmek için Azure portalını kullanın  

Bu makalede, Azure Veri Kutusu Kenarı'nızdaki kullanıcıların nasıl yönetilenbir şekilde yönetilen. Bant genişliği zamanlamaları, ağ bant genişliği kullanımını birden çok zamanlamaya göre yapılandırmanızı sağlar. Bu zamanlamalar, cihazınızla bulut arasında gerçekleştirilen yükleme ve indirme işlemlerine uygulanabilir.

Azure portalı üzerinden Veri Kutusu Edge'inizin bant genişliği zamanlamalarını ekleyebilir, değiştirebilir veya silebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Zamanlama ekleme
> * Zamanlamayı değiştirme
> * Zamanlamayı silme


## <a name="add-a-schedule"></a>Zamanlama ekleme

Zamanlama eklemek için Azure portalında aşağıdaki adımları yapın.

1. Veri Kutusu Kenarı kaynağınız için Azure portalında **Bant Genişliği'ne**gidin.
2. Sağ bölmede + **Zamanlama ekle'yi**seçin.

    ![Bant Genişliğini Seçin](media/data-box-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. **Zamanlama ekle** sayfasında: 

   1. Zamanlamanın **Başlangıç günü**, **Bitiş günü**, **Başlangıç saati** ve **Bitiş saati** değerlerini belirleyin.
   2. Bu zamanlamanın tüm gün çalışması gerekiyorsa **Tüm Gün** seçeneğini işaretleyin.
   3. **Bant genişliği hızı**, cihazınızda gerçekleştirilen bulutla ilgili işlemler (yükleme ve indirme) için kullanılan bant genişliğidir ve saniye başına megabit (Mb/sn) cinsinden ölçülür. Bu alan için 20 ile 1.000.000.007 arasında bir sayı temin edin.
   4. Veri yükleme ve indirme işlemlerini kısıtlamak istemiyorsanız **Sınırsız** bant genişliğini seçin.
   5. **Ekle'yi**seçin.

      ![Zamanlama ekleme](media/data-box-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Belirtilen parametrelerle bir zamanlama oluşturulur. Bu zamanlama daha sonra portaldaki bant genişliği zamanlamaları listesinde görüntülenir.

    ![Bant genişliği zamanlamalarının güncelleştirilmiş listesi](media/data-box-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Zamanlamayı düzenleme

Bir bant genişliği zamanlamasını düzenlemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında, Veri Kutusu Kenar ı kaynağınıza gidin ve ardından **Bant Genişliği'ne**gidin. 
2. Bant genişliği zamanlamaları listesinden, değiştirmek istediğiniz bir zamanlama seçin ve seçin.
    ![Bant genişliği çizelgesini seçin](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. İstediğiniz değişiklikleri yapın ve değişiklikleri kaydedin.

    ![Kullanıcıyı değiştirme](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. Zamanlama değiştirildikten sonra zamanlama listesi değiştirilen zamanlamayı gösterecek şekilde güncelleştirilir.

    ![Kullanıcıyı değiştirme](media/data-box-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Zamanlamayı silme

Veri Kutusu Edge aygıtınızla ilişkili bant genişliği zamanlamasını silmek için aşağıdaki adımları yapın.

1. Azure portalında, Veri Kutusu Kenar ı kaynağınıza gidin ve ardından **Bant Genişliği'ne**gidin.  

2. Bant genişliği zamanlaması listesinden silmek istediğiniz zamanlamayı seçin. **Düzenle zamanlamada** **Sil'i**seçin. Onay istendiğinde **Evet'i**seçin.

   ![Kullanıcı silme](media/data-box-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. Zamanlama silindikten sonra zamanlama listesi güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar

- [Paylaşımları](data-box-edge-manage-shares.md)nasıl yönetinen öğrenin.
