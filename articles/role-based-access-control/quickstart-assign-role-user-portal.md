---
title: Öğretici-RBAC ve Azure portal kullanarak Azure kaynaklarına Kullanıcı erişimi verme
description: Bu öğreticide, Azure portal rol tabanlı erişim denetimi 'ni (RBAC) kullanarak Azure kaynaklarına Kullanıcı erişimi verme hakkında bilgi edinin.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/22/2019
ms.author: rolyon
ms.openlocfilehash: f4dd3995df2a068824c4aa6bccca5606d250a165
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74419655"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Öğretici: RBAC ve Azure portal kullanarak Azure kaynaklarına Kullanıcı erişimi verme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) , Azure kaynaklarına erişimi yönetme yöntemidir. Bu öğreticide, bir kaynak grubundaki sanal makineleri oluşturmak ve yönetmek için bir kullanıcı erişimi verirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kaynak grubu kapsamındaki bir kullanıcı için erişim izni verme
> * Erişimi kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Gezinti listesinde, **kaynak grupları**' na tıklayın.

1. **Kaynak grubu** dikey penceresini açmak için **Ekle** ' ye tıklayın.

   ![Yeni kaynak grubu ekleme](./media/quickstart-assign-role-user-portal/resource-group.png)

1. **Kaynak grubu adı**için **RBAC-Resource-Group**yazın.

1. Bir abonelik ve bir konum seçin.

1. Kaynak grubunu oluşturmak için **Oluştur** ' a tıklayın.

1. Kaynak grupları listesini yenilemek için **Yenile** ' ye tıklayın.

   Yeni kaynak grubu kaynak grupları listenizde görünür.

   ![Kaynak grubu listesi](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

1. **Kaynak grupları**listesinde, yeni **RBAC-kaynak grubu** kaynak grubu ' na tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Rol atamalarının geçerli listesini görmek için **rol atamaları** sekmesine tıklayın.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/quickstart-assign-role-user-portal/access-control.png)

1. **Rol ataması ekle > ** **Ekle** ' ye tıklayın.

   Rol atama izniniz yoksa rol ataması Ekle seçeneği devre dışı bırakılır.

   ![Menü ekle](./media/role-assignments-portal/add-menu.png)

   ![Rol atama bölmesi Ekle](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Aşağı açılan **Rol** listesinden **Sanal Makine Katılımcısı**'nı seçin.

1. **Seç** listesinde kendinizi ve başka bir kullanıcıyı seçin.

1. Rol atamasını oluşturmak için **Kaydet** ' e tıklayın.

   Birkaç dakika sonra, Kullanıcı sanal makine katılımcısı rolü RBAC-kaynak grubu kaynak grubu kapsamında atanır.

   ![Sanal Makine Katılımcısı rolü ataması](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

1. Rol atamaları listesinde, sanal makine katılımcısı rolüne sahip kullanıcının yanına bir onay işareti ekleyin.

1. **Kaldır**' a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Görüntülenen rol atamasını Kaldır iletisinde **Evet**' e tıklayın.

## <a name="clean-up"></a>Temizleme

1. Gezinti listesinde, **kaynak grupları**' na tıklayın.

1. Kaynak grubunu açmak için **RBAC-kaynak grubu** ' na tıklayın.

1. Kaynak grubunu silmek için **kaynak grubunu sil** ' e tıklayın.

   ![Kaynak grubunu silme](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Dikey pencereyi **silmek istediğinizden emin misiniz** , kaynak grubu adı: **RBAC-Resource-Group**yazın.

1. Kaynak grubunu silmek için **Sil** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure PowerShell kullanarak Azure kaynaklarına Kullanıcı erişimi verme](tutorial-role-assignments-user-powershell.md)
