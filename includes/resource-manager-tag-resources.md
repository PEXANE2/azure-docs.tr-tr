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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80132250"
---
Bir Kullanıcı etiketleri uygulamak için gerekli erişime sahip değilse, kullanıcıya **etiketi katkıda bulunan** rolünü atayabilirsiniz. Daha fazla bilgi için bkz. [öğretici: Azure KAYNAKLARıNA RBAC ve Azure Portal kullanarak Kullanıcı erişimi verme](../articles/role-based-access-control/quickstart-assign-role-user-portal.md).

1. Bir kaynağın veya kaynak grubunun etiketlerini görüntülemek için genel bakışta var olan etiketleri bulun. Daha önce etiket uygulamadıysanız liste boştur.

   ![Kaynak veya kaynak grubu için etiketleri görüntüle](./media/resource-manager-tag-resources/view-tags.png)

1. Etiket eklemek için, **etiketleri eklemek üzere buraya tıklayın ' ı**seçin.

1. Bir ad ve değer belirtin.

   ![Etiket ekle](./media/resource-manager-tag-resources/add-tag.png)

1. Gerektiğinde Etiketler eklemeye devam edin. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Etiketleri Kaydet](./media/resource-manager-tag-resources/save-tags.png)

1. Etiketler artık genel bakışta görüntülenir.

   ![Etiketleri göster](./media/resource-manager-tag-resources/view-new-tags.png)

1. Etiket eklemek veya silmek için **Değiştir**' i seçin.

1. Bir etiketi silmek için çöp kutusu simgesini seçin. Ardından **Kaydet**’i seçin.

   ![Etiketi Sil](./media/resource-manager-tag-resources/delete-tag.png)

Birden çok kaynağa etiketleri toplu atamak için:

1. Herhangi bir kaynak listesinden etiketi atamak istediğiniz kaynakların onay kutusunu seçin. Ardından, **etiket ata**' yı seçin.

   ![Birden çok kaynak seçin](./media/resource-manager-tag-resources/select-multiple-resources.png)

1. Ad ve değer ekleyin. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Ata seçin](./media/resource-manager-tag-resources/select-assign.png)

Etiketi olan tüm kaynakları görüntülemek için:

1. Azure portal menüsünde, **Etiketler**' i arayın. Kullanılabilir seçeneklerden seçin.

   ![Etikete göre bul](./media/resource-manager-tag-resources/find-tags-general.png)

1. Kaynakları görüntülemek için etiketi seçin.

   ![Etiket Seç](./media/resource-manager-tag-resources/select-tag.png)

1. Bu etikete sahip tüm kaynaklar görüntülenir.

   ![Kaynakları etikete göre görüntüle](./media/resource-manager-tag-resources/view-resources-by-tag.png)
