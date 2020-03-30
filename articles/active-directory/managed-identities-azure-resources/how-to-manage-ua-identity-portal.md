---
title: Azure portalında kullanıcı tarafından atanmış yönetilen bir kimliği yönetme - Azure AD
description: Kullanıcı tarafından atanan yönetilen bir kimliğe nasıl rol oluşturulacağına, listelenene, silinmeye ve atayalaacağına ilişkin adım adım yönergeler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244140"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Azure portalını kullanarak kullanıcı tarafından atanan yönetilen bir kimliğe rol oluşturun, listelemek, silmek veya atamak

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikler Azure hizmetlerine Azure Etkin Dizini'nde yönetilen bir kimlik sağlar. Bu kimliği, kodunuzda kimlik bilgilerine gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure Portalı'nı kullanarak kullanıcı tarafından atanan yönetilen bir kimliğe nasıl rol oluşturabileceğinizi, listelemeyi, sildiğinizi veya atayabileceğinizi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için, hesabınızın [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

1. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Arama kutusunda Yönetilen *Kimlikler*yazın ve **Hizmetler**altında **Yönetilen Kimlikler'i**tıklatın.
3. Kullanıcı tarafından atanan yönetilen kimlik bölmesi altındaki aşağıdaki alanlarda **değer ekle** ve **gir'i** tıklatın:
   - **Kaynak Adı**: Kullanıcı tarafından atanan yönetilen kimliğinizin adıdır, örneğin UAI1.
   - **Abonelik**: Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için aboneliği seçin
   - **Kaynak Grubu**: Kullanıcı tarafından atanan yönetilen kimliğinizi içerecek yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubunda kullanıcı tarafından atanan yönetilen kimliği oluşturmak için **varolan** kullan'ı seçin.
   - **Konum**: Kullanıcı tarafından atanan yönetilen kimliği dağıtmak için bir konum seçin(örneğin **Batı ABD).**
4. **Oluştur'u**tıklatın.

![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listele

Kullanıcı tarafından atanan yönetilen bir kimliği listelemek/okumak için hesabınızın [Yönetilen Kimlik Operatörü'ne](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya Yönetilen [Kimlik Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

1. Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Arama kutusunda Yönetilen *Kimlikler*yazın ve Hizmetler altında **Yönetilen Kimlikler'i**tıklatın.
3. Aboneliğiniz için kullanıcı tarafından atanan yönetilen kimliklerin listesi döndürülür.  Kullanıcı tarafından atanan yönetilen kimliğin ayrıntılarını görmek için adını tıklatın.

![Kullanıcı tarafından atanan yönetilen kimliği listele](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan yönetilen bir kimliği silmek için, hesabınızın [Yönetilen Kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Atanan bir kullanıcıkimliğini silerken, kimliği atandığı VM'den veya kaynaktan kaldırmaz.  VM görmekten atanan kullanıcı kimliğini kaldırmak [için, kullanıcı tarafından atanan yönetilen kimliği VM'den kaldırın.](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#remove-a-user-assigned-managed-identity-from-a-vm)

1. Kullanıcı tarafından atanan yönetilen kimliği silmek için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Kullanıcı tarafından atanan yönetilen kimliği seçin ve **Sil'i**tıklatın.
3. Onay kutusunun altında seçin, **Evet**.

![Kullanıcı tarafından atanan yönetilen kimliği silme](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliğe rol atama 

Kullanıcı tarafından atanan yönetilen bir kimliğe rol atamak için hesabınızın [Kullanıcı Erişim Yöneticisi](/azure/role-based-access-control/built-in-roles#user-access-administrator) rol atamasına ihtiyacı vardır.

1. Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için Azure aboneliğiyle ilişkili bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Arama kutusunda Yönetilen *Kimlikler*yazın ve Hizmetler altında **Yönetilen Kimlikler'i**tıklatın.
3. Aboneliğiniz için kullanıcı tarafından atanan yönetilen kimliklerin listesi döndürülür.  Bir rol atamak istediğiniz kullanıcı tarafından atanan yönetilen kimliği seçin.
4. **Access denetimi (IAM) ve** ardından rol **ataması ekle'yi**seçin.

   ![Kullanıcı tarafından atanan yönetilen kimlik başlangıcı](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Rol atama ekle bıçağında, aşağıdaki değerleri yapılandırın ve sonra **Kaydet'i**tıklatın:
   - **Rol** - atamak için rol
   - **Erişim atama** - kullanıcı tarafından atanan yönetilen kimliği atamak için kaynak
   - **Seçin** - erişim atamak için üye
   
   ![Kullanıcı tarafından atanan yönetilen kimlik IAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  
