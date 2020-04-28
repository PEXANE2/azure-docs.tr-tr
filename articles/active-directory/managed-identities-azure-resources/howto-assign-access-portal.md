---
title: Azure AD Azure portal kullanarak bir kaynağa yönetilen kimlik erişimi atama
description: Azure portal kullanarak, bir kaynakta yönetilen kimlik atamaya yönelik adım adım yönergeler.
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
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74547280"
---
# <a name="assign-a-managed-identity-access-to-a-resource-by-using-the-azure-portal"></a>Azure portal kullanarak bir kaynağa yönetilen kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen kimlikle bir Azure kaynağı yapılandırdıktan sonra, herhangi bir güvenlik sorumlusu gibi, yönetilen kimliğe başka bir kaynağa erişim izni verebilirsiniz. Bu makalede, Azure sanal makinesine veya sanal makine ölçek kümesinin yönetilen kimlik erişimine Azure portal kullanarak bir Azure depolama hesabına nasıl izin vereceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için RBAC kullanma

Azure [VM](qs-configure-portal-windows-vm.md) veya [Azure VMSS](qs-configure-portal-windows-vmss.md)gibi bir Azure kaynağında yönetilen kimlik etkinleştirildikten sonra:

1. Yönetilen kimliği yapılandırdığınız Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.

2. Erişim denetimini değiştirmek istediğiniz istenen kaynağa gidin. Bu örnekte, bir depolama hesabına Azure sanal makine erişimi veriyoruz, bu nedenle depolama hesabına gittik.

3. Kaynağın **erişim denetimi (IAM)** sayfasını seçin ve **+ rol ataması Ekle**' yi seçin. Ardından, **rolü**belirtin, **erişimi atayın**ve ilgili **aboneliği**belirtin. Arama ölçütleri alanında, kaynağı görmeniz gerekir. Kaynağı seçin ve **Kaydet**' i seçin. 

   ![Erişim denetimi (ıAM) ekran görüntüsü](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
     
## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Azure sanal makinesinde yönetilen kimliği etkinleştirmek için, bkz. [Azure Portal kullanarak BIR VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-portal-windows-vm.md).
- Azure sanal makine ölçek kümesinde yönetilen kimliği etkinleştirmek için, bkz. [Azure Portal kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-portal-windows-vmss.md).


