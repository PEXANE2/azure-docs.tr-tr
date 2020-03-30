---
title: Azure portalını kullanarak yönetilen kimlikleri yapılandırma - Azure AD
description: Azure portalını kullanarak Azure vm'de Azure kaynakları için yönetilen kimlikleri yapılandırmak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/10/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75971dbd35a6b29306bc87988423ab662a343705
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266669"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure portalını kullanarak bir Azure Sanal Makine (VM) için sistemi ve kullanıcı tarafından atanan yönetilen kimlikleri etkinleştirmeyi ve devre dışı kmayı öğrenirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure portalını kullanarak VM için sistem tarafından atanan yönetilen kimliği nasıl etkinleştirdiğinizi ve devre dışı kabileceğinizi öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>VM oluşturma sırasında sistem tarafından yönetilen kimliği etkinleştirme

Oluşturma sırasında bir VM'de sistem tarafından atanan yönetilen kimliği etkinleştirmek için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

- **Kimlik** bölümündeki **Yönetim** sekmesi altında Yönetilen hizmet kimliğini **Açık'a** **geçirin.**  

![VM oluşturma sırasında sistemle atanmış kimliği etkinleştirme](./media/msi-qs-configure-portal-windows-vm/enable-system-assigned-identity-vm-creation.png)

VM oluşturmak için aşağıdaki Quickstarts'a bakın: 

- [Azure portal ile Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) 
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)


### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Var olan bir VM'de sistem tarafından yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği, başlangıçta onsuz sağlanan bir VM'de etkinleştirmek için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'yi içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.

2. İstenilen Sanal Makine'ye gidin ve **Kimlik'i**seçin.

3. **Sistem atanmış**altında , **Durum**, **On** seçin ve sonra **Kaydet'i**tıklatın :

   ![Yapılandırma sayfası ekran görüntüsü](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Sistem tarafından atanan yönetilen kimliği VM'den kaldırma

Sistem tarafından atanan yönetilen kimliği Bir VM'den kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

Artık sistem le atanmış yönetilen kimliğe ihtiyaç dolmayan bir Sanal Makineniz varsa:

1. VM'yi içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın. 

2. İstenilen Sanal Makine'ye gidin ve **Kimlik'i**seçin.

3. **Sistem atanmış**altında , **Durum**, **Kapalı** seçin ve sonra **Kaydet'i**tıklatın :

   ![Yapılandırma sayfası ekran görüntüsü](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

 Bu bölümde, Azure portalını kullanarak kullanıcı tarafından atanan yönetilen bir kimliği vm'den nasıl ekleyeceğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>VM oluşturma sırasında kullanıcı tarafından atanan bir kimlik atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

Şu anda Azure portalı, VM oluşturulması sırasında kullanıcı tarafından atanan yönetilen bir kimlik atanmasını desteklememektedir. Bunun yerine, önce VM oluşturmak için aşağıdaki VM oluşturma Quickstart makalelerinden birine bakın ve ardından VM'ye kullanıcı tarafından atanan yönetilen bir kimlik atamayla ilgili ayrıntılar için bir sonraki bölüme gidin:

- [Azure portal ile Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Azure portal ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir VM'ye atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'yi içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. İstenilen VM gidin ve **Kimlik**tıklayın , **Kullanıcı atanmış** ve sonra ** \+Ekle**.

   ![VM'ye kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. VM'ye eklemek istediğiniz kullanıcı tarafından atanan kimliği tıklatın ve sonra **Ekle'yi**tıklatın.

    ![VM'ye kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Kullanıcı tarafından atanan yönetilen kimliği VM'den kaldırma

Kullanıcı tarafından atanan bir kimliği VM'den kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. VM'yi içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. İstenilen VM'ye gidin ve **Kimlik,** **Kullanıcı atanmış,** silmek istediğiniz kullanıcı tarafından atanan yönetilen kimliğin adını tıklatın ve ardından **Kaldır'ı** tıklatın (onay bölmesinde **Evet'i** tıklatın).

   ![Kullanıcı tarafından atanan yönetilen kimliği VM'den kaldırma](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak, Azure VM'nin başka bir [Azure kaynağına](howto-assign-access-portal.md)yönetilen kimlik erişimi vermesini sağlamak.

