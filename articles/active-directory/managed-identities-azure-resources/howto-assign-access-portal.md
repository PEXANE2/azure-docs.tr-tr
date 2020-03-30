---
title: Azure portalını kullanarak kaynağa yönetilen bir kimlik erişimi atama - Azure AD
description: Azure portalını kullanarak, bir kaynağa başka bir kaynağa erişime yönetilen bir kimlik atamak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: e24c97909870c4d76b07ec837e5f624a509bd1f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547280"
---
# <a name="assign-a-managed-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portalını kullanarak kaynağa yönetilen bir kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir Azure kaynağını yönetilen bir kimliğe göre yapılandırdıktan sonra, yönetilen kimlik erişimini herhangi bir güvenlik ilkesi gibi başka bir kaynağa verebilirsiniz. Bu makalede, Azure portalını kullanarak bir Azure sanal makineveya sanal makine ölçeği kümesinin bir Azure depolama hesabına yönetilen kimlik erişimini nasıl verebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için RBAC'ı kullanma

[Azure VM](qs-configure-portal-windows-vm.md) veya Azure [VMSS](qs-configure-portal-windows-vmss.md)gibi bir Azure kaynağında yönetilen kimliği etkinleştirdikten sonra:

1. Yönetilen kimliği yapılandırdığınız Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.

2. Erişim denetimini değiştirmek istediğiniz istediğiniz kaynağa gidin. Bu örnekte, bir depolama hesabına Azure sanal makine erişimi veriyoruz, bu nedenle depolama hesabına yönlendiriyoruz.

3. Kaynağın **Access denetimi (IAM)** sayfasını seçin ve **rol ataması ekle '** yi seçin. Ardından **Role,** **Access'e atayı**ve ilgili **Aboneliği**belirtin. Arama ölçütleri alanı altında, kaynağı görmeniz gerekir. Kaynağı seçin ve **Kaydet'i**seçin. 

   ![Erişim denetimi (IAM) ekran görüntüsü](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
     
## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Azure sanal makinesinde yönetilen kimliği etkinleştirmek [için, Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma'ya](qs-configure-portal-windows-vm.md)bakın.
- Azure sanal makine ölçeğinde yönetilen kimliği etkinleştirmek [için, Azure portalını kullanarak sanal makine ölçeğinde Azure kaynakları için yönetilen kimlikleri yapılandırma'ya](qs-configure-portal-windows-vmss.md)bakın.


