---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 03/19/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a00291182059506aeab9cde965fa4cbd5177ecf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132250"
---
Bir kullanıcı etiketleri uygulamak için gerekli erişime sahip değilse, **Etiket Katılımcısı** rolünü kullanıcıya atayabilirsiniz. Daha fazla bilgi için [Bkz. Öğretici: RBAC ve Azure portalını kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın.](../articles/role-based-access-control/quickstart-assign-role-user-portal.md)

1. Kaynak veya kaynak grubunun etiketlerini görüntülemek için genel bakışta varolan etiketlere bakın. Daha önce etiket uygulamadıysanız liste boştur.

   ![Kaynak veya kaynak grubu için etiketleri görüntüleme](./media/resource-manager-tag-resources/view-tags.png)

1. Etiket eklemek **için, etiket eklemek için buraya tıklayın'ı**seçin.

1. Bir ad ve değer sağlayın.

   ![Etiket ekle](./media/resource-manager-tag-resources/add-tag.png)

1. Gerektiğinde etiket eklemeye devam edin. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Etiketleri kaydet](./media/resource-manager-tag-resources/save-tags.png)

1. Etiketler artık genel bakışta görüntülenir.

   ![Etiketleri göster](./media/resource-manager-tag-resources/view-new-tags.png)

1. Etiket eklemek veya silmek için **değiştir'i**seçin.

1. Etiketi silmek için çöp kutusu simgesini seçin. Ardından **Kaydet**’i seçin.

   ![Etiketi silme](./media/resource-manager-tag-resources/delete-tag.png)

Etiketleri birden çok kaynağa toplu atamak için:

1. Herhangi bir kaynak listesinden, etiketi atamak istediğiniz kaynakların onay kutusunu seçin. Ardından, **etiketleri atay'ı**seçin.

   ![Birden çok kaynak seçin](./media/resource-manager-tag-resources/select-multiple-resources.png)

1. Adlar ve değerler ekleyin. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Atama'yı seçin](./media/resource-manager-tag-resources/select-assign.png)

Etiketli tüm kaynakları görüntülemek için:

1. Azure portalı menüsünde **etiketleri**arayın. Kullanılabilir seçeneklerarasından seçin.

   ![Etikete göre bul](./media/resource-manager-tag-resources/find-tags-general.png)

1. Kaynakları görüntülemek için etiketi seçin.

   ![Etiketi seçin](./media/resource-manager-tag-resources/select-tag.png)

1. Bu etikete sahip tüm kaynaklar görüntülenir.

   ![Kaynakları etikete göre görüntüleme](./media/resource-manager-tag-resources/view-resources-by-tag.png)
