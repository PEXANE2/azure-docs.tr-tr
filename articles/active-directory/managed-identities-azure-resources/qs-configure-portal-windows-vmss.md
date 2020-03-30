---
title: Azure VMSS'de yönetilen kimlikleri yapılandırma - Azure AD
description: Azure portalını kullanarak sanal makine ölçeğinde Azure kaynakları için yönetilen kimlikleri yapılandırmak için adım adım yönergeler.
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
ms.date: 02/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32d8944b55794412eb322697469933030e2fbb56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74184030"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-the-azure-portal"></a>Azure portalını kullanarak sanal makine ölçeğinde Azure kaynakları için yönetilen kimlikleri yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, PowerShell'i kullanarak, Sanal makine ölçeği kümesinde Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri nasıl gerçekleştireceklerini öğrenirsiniz:

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için, hesabınızda aşağıdaki Azure rol tabanlı erişim denetimi atamaları gerekir:

    > [!NOTE]
    > Ek Azure AD dizini rol atamaları gerekmez.

    - [Sanal Makine Katılımcısı,](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) sistem tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden etkinleştirmek ve kaldırmak için.

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure portalını kullanarak sisteme atanan yönetilen kimliği nasıl etkinleştireceğinizi ve devre dışı kacağınızı öğreneceksiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesi nin oluşturulması sırasında sistem tarafından atanan yönetilen kimliği etkinleştirme

Şu anda Azure portalı, sanal makine ölçeği kümesi nin oluşturulması sırasında sistem tarafından atanan yönetilen kimliği etkinleştirmeyi desteklememektedir. Bunun yerine, önce sanal makine ölçeği kümesi oluşturmak için aşağıdaki sanal makine ölçeği kümesi Quickstart makalesine bakın ve ardından sanal makine ölçeği kümesinde sistem tarafından atanan yönetilen kimliği etkinleştirme yle ilgili ayrıntılar için bir sonraki bölüme gidin:

- [Azure portalında Sanal Makine Ölçeği Seti Oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>Varolan sanal makine ölçeği kümesinde sistem le atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği, başlangıçta onsuz sağlanan sanal makine ölçeğinde etkinleştirmek için:

1. Sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.

2. İstediğiniz sanal makine ölçeği kümesine gidin.

3. **Sistem atanmış**altında , **Durum**, **On** seçin ve sonra **Kaydet'i**tıklatın :

   ![Yapılandırma sayfası ekran görüntüsü](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png) 

### <a name="remove-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Sistem tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden kaldırma

Artık sistem tarafından atanmış yönetilen bir kimliğe ihtiyaç dolmayan sanal bir makine ölçeği kümeniz varsa:

1. Sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın. Ayrıca, hesabınızın sanal makine ölçeği kümesinde yazma izinleri veren bir role ait olduğundan emin olun.

2. İstediğiniz sanal makine ölçeği kümesine gidin.

3. **Sistem atanmış**altında , **Durum**, **Kapalı** seçin ve sonra **Kaydet'i**tıklatın :

   ![Yapılandırma sayfası ekran görüntüsü](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure portalını kullanarak kullanıcı tarafından atanan yönetilen bir kimliği sanal makine ölçeğinden nasıl ekleyeceğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesi nin oluşturulması sırasında kullanıcı tarafından atanan yönetilen bir kimlik atama

Şu anda Azure portalı, sanal makine ölçeği kümesi nin oluşturulması sırasında kullanıcı tarafından atanan yönetilen bir kimlik atanmasını desteklememektedir. Bunun yerine, önce sanal makine ölçeği kümesi oluşturmak için aşağıdaki sanal makine ölçeği kümesi Quickstart makalesine bakın ve ardından kullanıcı tarafından atanan yönetilen bir kimlik atamayla ilgili ayrıntılar için bir sonraki bölüme gidin:

- [Azure portalında Sanal Makine Ölçeği Seti Oluşturma](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan sanal makine ölçeği kümesine atama

1. Sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. İstenilen sanal makine ölçeği kümesine gidin ve **Kimlik,** **Kullanıcı atanmış** ve sonra ** \+Ekle'yi**tıklatın.

   ![VMSS'ye kullanıcı tarafından atanan kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Sanal makine ölçeği kümesine eklemek istediğiniz kullanıcı tarafından atanan kimliği tıklatın ve sonra **Ekle'yi**tıklatın.
   
   ![VMSS'ye kullanıcı tarafından atanan kimlik ekleme](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden kaldırma

1. VM'yi içeren Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. İstenilen sanal makine ölçeği kümesine gidin ve **Kimlik**, **Kullanıcı atanmış,** silmek istediğiniz kullanıcı tarafından atanan yönetilen kimliğin adını tıklatın ve sonra **Kaldır'ı** tıklatın (onay bölmesinde **Evet'i** tıklatın).

   ![Kullanıcı tarafından atanan kimliği VMSS'den kaldırma](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="next-steps"></a>Sonraki adımlar

- Azure portalını kullanarak, azure sanal makine ölçeği kümesine başka bir [Azure kaynağına](howto-assign-access-portal.md)yönetilen kimlik erişimi verin.


