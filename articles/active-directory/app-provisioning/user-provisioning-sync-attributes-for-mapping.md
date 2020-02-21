---
title: Eşleme için öznitelikleri Azure AD 'ye eşitler | Microsoft Docs
description: Şirket içi Active Directory özniteliklerini Azure AD 'ye eşitlemeyi öğrenin. SaaS uygulamalarına Kullanıcı sağlamayı yapılandırırken, varsayılan olarak eşitlenmemiş kaynak özniteliklerini eklemek için dizin uzantısı özelliğini kullanın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mimart
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09d1efaf54bee65bd3274987e68e643f887baade
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77522280"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Bir uygulamaya sağlamak için şirket içi Active Directory bir özniteliği Azure AD 'ye eşitleyin

Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirirken, eşlemek istediğiniz özniteliğin **kaynak öznitelik** listesinde görünmediğini fark edebilirsiniz. Bu makalede, şirket içi Active Directory (AD) ile Azure Active Directory (Azure AD) arasında eşitleme yaparak eksik özniteliği nasıl ekleyeceğiniz gösterilmektedir.

Azure AD, Azure AD 'den bir SaaS uygulamasına kullanıcı hesapları sağlarken bir kullanıcı profili oluşturmak için gereken tüm verileri içermelidir. Bazı durumlarda, verileri kullanılabilir hale getirmek için şirket içi AD 'nizden Azure AD 'ye yönelik öznitelikleri eşitlemeniz gerekebilir. Azure AD Connect, bazı öznitelikleri otomatik olarak Azure AD 'ye eşitler, ancak tüm özniteliklere vermez. Ayrıca, varsayılan olarak eşitlenen bazı öznitelikler (SAMAccountName gibi) Microsoft Graph API kullanılarak gösterilmeyebilir. Bu durumlarda, özniteliği Azure AD ile eşleştirmek için Azure AD Connect Directory uzantısı özelliğini kullanabilirsiniz. Bu şekilde, öznitelik Microsoft Graph API 'SI ve Azure AD sağlama hizmeti tarafından görünür olur.

Sağlama için ihtiyaç duyduğunuz veriler Active Directory, ancak yukarıda açıklanan nedenlerden dolayı sağlama için kullanılamazsa, aşağıdaki adımları izleyin.
 
## <a name="sync-an-attribute"></a>Bir özniteliği eşitleme 

1. Azure AD Connect Sihirbazı 'nı açın, görevler ' i ve ardından **eşitleme seçeneklerini Özelleştir**' i seçin.

   ![Azure Active Directory Connect Sihirbazı ek görevler sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Azure AD Genel Yöneticisi olarak oturum açın. 

3. **Isteğe bağlı özellikler** sayfasında, **dizin uzantısı öznitelik eşitleme**' yi seçin.
 
   ![Azure Active Directory Connect sihirbaz Isteğe bağlı özellikler sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Azure AD 'ye genişletmek istediğiniz öznitelikleri seçin.
   > [!NOTE]
   > **Kullanılabilir öznitelikler** altında arama büyük/küçük harfe duyarlıdır.

   ![Azure Active Directory Connect Sihirbazı Dizin uzantıları seçim sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Azure AD Connect Sihirbazı 'nı tamamlayıp tam eşitleme döngüsünün çalışmasına izin verin. Cycle tamamlandığında, şema genişletilir ve yeni değerler, şirket içi AD ile Azure AD arasında eşitlenir.
 
6. Azure portal, [Kullanıcı özniteliği eşlemelerini düzenlemekte](customize-application-attributes.md)olduğunuz sırada, **kaynak öznitelik** listesi artık eklenen özniteliği `<attributename> (extension_<appID>_<attributename>)`biçiminde içerecektir. Özniteliği seçin ve sağlama için hedef uygulamayla eşleyin.

   ![Azure Active Directory Connect Sihirbazı Dizin uzantıları seçim sayfası](./media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> **ManagedBy** veya **DN/distinguishedName 'dir**gibi şirket içi ad 'den başvuru öznitelikleri sağlama özelliği bugün desteklenmez. Bu özelliği [Kullanıcı seste](https://feedback.azure.com/forums/169401-azure-active-directory)isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sağlama için kimin kapsam içinde olduğunu tanımlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
