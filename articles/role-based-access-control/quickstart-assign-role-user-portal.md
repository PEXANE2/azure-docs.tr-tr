---
title: 'Öğretici: RBAC ve Azure portalı ile Azure kaynaklarına kullanıcı erişimi verme '
description: Bu eğitimde, Azure portalında rol tabanlı erişim denetimi (RBAC) kullanarak bir kullanıcıya Azure kaynaklarına nasıl erişiriz öğrenin.
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
ms.openlocfilehash: 6281c9a1818a3b46ac4f7a62f8ae76668db56887
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77138086"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Öğretici: RBAC ve Azure portalını kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın

[Rol tabanlı erişim denetimi (RBAC),](overview.md) Azure kaynaklarına erişimi yönetmenin yoludur. Bu öğreticide, bir kaynak grubunda sanal makineler oluşturmak ve yönetmek için bir kullanıcıya erişim sağlarsınız.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak grubu kapsamındaki bir kullanıcıya erişim izni verme
> * Erişimi kaldırma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Gezinti bölmesinde **Kaynak grupları**'na tıklayın.

1. **Kaynak grup** bıçağını açmak için **Ekle'yi** tıklatın.

   ![Yeni kaynak grubu ekleme](./media/quickstart-assign-role-user-portal/resource-group.png)

1. **Kaynak grubu adı için** **rbac-kaynak grubu**girin.

1. Bir abonelik ve bir konum seçin.

1. Kaynak grubunu oluşturmak için **Oluştur**'a tıklayın.

1. Kaynak grupları listesini yenilemek için **Yenile'yi** tıklatın.

   Yeni kaynak grubu kaynak grupları listenizde görünür.

   ![Kaynak grubu listesi](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

1. **Kaynak grupları**listesinde, yeni **rbac-kaynak grubu** kaynak grubunu tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Geçerli rol atamalarının listesini görmek için **Rol atamaları** sekmesine tıklayın.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/quickstart-assign-role-user-portal/access-control.png)

1. Rol Atama ekle bölmesini açmak için**ekle rol atamasını** **tıklatın.** > 

   Rolleri atama izniniz yoksa, rol atama ekle seçeneği devre dışı bırakılır.

   ![Menü ekle](./media/role-assignments-portal/add-menu.png)

   ![Rol atama bölmesi ekleme](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Aşağı açılan **Rol** listesinden **Sanal Makine Katılımcısı**'nı seçin.

1. **Seç** listesinde kendinizi ve başka bir kullanıcıyı seçin.

1. Rol atamasını oluşturmak için **Kaydet**'e tıklayın.

   Birkaç dakika sonra, kullanıcıya rbac-kaynak grubu kaynak grubu kapsamında Sanal Makine Katılımcısı rolü atanır.

   ![Sanal Makine Katılımcısı rolü ataması](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

1. Rol atamaları listesinde, Sanal Makine Katılımcısı rolüyle kullanıcının yanına bir onay işareti ekleyin.

1. **Kaldır**’a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Görünen rol atamasını kaldır iletisinde **Evet'i**tıklatın.

## <a name="clean-up"></a>Temizleme

1. Gezinti bölmesinde **Kaynak grupları**'na tıklayın.

1. Kaynak grubunu açmak için **rbac-kaynak grubunu** tıklatın.

1. Kaynak grubunu silmek için **kaynak grubunu sil'i** tıklatın.

   ![Kaynak grubunu silme](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Bıçak **silmek istediğinizden emin misiniz,** kaynak grubu adını yazın: **rbac-kaynak grubu**.

1. Kaynak grubunu silmek için **Sil'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure PowerShell'i kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın](tutorial-role-assignments-user-powershell.md)
