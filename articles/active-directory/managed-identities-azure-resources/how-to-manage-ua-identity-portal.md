---
title: Azure portal Kullanıcı tarafından atanan yönetilen kimliği yönetme-Azure AD
description: Kullanıcı tarafından atanan yönetilen kimlik için rol oluşturma, listeleme, silme ve atama ile ilgili adım adım yönergeler.
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
ms.date: 04/16/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10caa9e5e61dc1dd0c1062583f55a7357c643ce5
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79244140"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Azure portal kullanarak Kullanıcı tarafından atanan yönetilen kimlik için rol oluşturma, listeleme, silme veya atama

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory bir yönetilen kimliğe sahip Azure hizmetleri sağlar. Kodunuzda kimlik bilgileri gerekmeden Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulaması yapmak için bu kimliği kullanabilirsiniz. 

Bu makalede, Azure portalını kullanarak Kullanıcı tarafından atanan yönetilen kimlik için rol oluşturma, listeleme, silme veya atamayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. **[Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

1. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
2. Arama kutusuna *Yönetilen kimlikler*yazın ve **Hizmetler**' ın altında, **Yönetilen kimlikler**' e tıklayın.
3. **Ekle** ' ye tıklayın ve **Kullanıcı tarafından atanan yönetilen kimlik oluştur** altında aşağıdaki alanlara değerleri girin:
   - **Kaynak adı**: Bu, Kullanıcı tarafından atanan yönetilen kimliğinizin adıdır, ÖRNEĞIN, UAI1.
   - **Abonelik**: Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için aboneliği seçin
   - **Kaynak grubu**: Kullanıcı tarafından atanan yönetilen kimliğinizi içerecek yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubunda Kullanıcı tarafından atanan yönetilen kimliği oluşturmak Için **Varolanı kullan** ' ı seçin.
   - **Konum**: Kullanıcı tarafından atanan yönetilen kimliğin dağıtılacağı bir konum seçin, örneğin **Batı ABD**.
4. **Oluştur**'a tıklayın.

![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listeleme

Kullanıcı tarafından atanan yönetilen kimliği listelemek/okumak için hesabınızın [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü ataması gerekir.

1. Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
2. Arama kutusuna *Yönetilen kimlikler*yazın ve hizmetler ' in altında, **Yönetilen kimlikler**' e tıklayın.
3. Aboneliğiniz için Kullanıcı tarafından atanan yönetilen kimliklerin bir listesi döndürülür.  Kullanıcı tarafından atanan yönetilen kimliğin ayrıntılarını görmek için adına tıklayın.

![Kullanıcı tarafından atanan yönetilen kimliği listeleme](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan bir yönetilen kimliği silmek için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

Kullanıcı tarafından atanan bir kimlik silindiğinde, atandığı VM veya kaynaktan kaldırılmaz.  Kullanıcı tarafından atanan kimliği bir VM 'den kaldırmak için bkz. [Kullanıcı tarafından atanan bir yönetilen kimliği BIR VM 'Den kaldırma](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#remove-a-user-assigned-managed-identity-from-a-vm).

1. Kullanıcı tarafından atanan yönetilen kimliği silmek için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
2. Kullanıcı tarafından atanan yönetilen kimliği seçin ve **Sil**' e tıklayın.
3. Onay kutusunun altında, **Evet**' i seçin.

![Kullanıcı tarafından atanan yönetilen kimliği Sil](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliğe rol atama 

Kullanıcı tarafından atanan yönetilen kimliğe bir rol atamak için hesabınızın [Kullanıcı erişimi yönetici](/azure/role-based-access-control/built-in-roles#user-access-administrator) rolü ataması gerekir.

1. Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
2. Arama kutusuna *Yönetilen kimlikler*yazın ve hizmetler ' in altında, **Yönetilen kimlikler**' e tıklayın.
3. Aboneliğiniz için Kullanıcı tarafından atanan yönetilen kimliklerin bir listesi döndürülür.  Rol atamak istediğiniz kullanıcı tarafından atanan yönetilen kimliği seçin.
4. **Erişim denetimi (IAM)** öğesini seçin ve ardından **rol ataması Ekle**' yi seçin.

   ![Kullanıcı tarafından atanan yönetilen kimlik başlangıcı](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Rol ataması Ekle dikey penceresinde aşağıdaki değerleri yapılandırın ve **Kaydet**' e tıklayın:
   - **Rol** -atanacak rol
   - Kullanıcı tarafından atanan yönetilen kimliği atamak için kaynağa **erişim atama**
   - **Select** -erişim atanacak üye
   
   ![Kullanıcı tarafından atanan yönetilen kimlik ıAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  
