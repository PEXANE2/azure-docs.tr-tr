---
title: Eşleme için Azure AD'ye öznitelikleri senkronize etme | Microsoft Dokümanlar
description: Şirket içi Active Directory'nizden Azure AD'ye öznitelikleri nasıl eşitleyiştireceklerini öğrenin. Kullanıcı sağlamayı SaaS uygulamalarına yapılandırırken, varsayılan olarak eşitlenmemiş kaynak öznitelikleri eklemek için dizin uzantısı özelliğini kullanın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522280"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Bir uygulamayı sağlamak için şirket içi Active Directory'nizden Azure AD'ye bir özniteliği senkronize etme

Kullanıcı sağlama için öznitelik eşlemelerini özelleştirirken, eşlediğiniz özniteliğin Kaynak **öznitelik** listesinde görünmediğini görebilirsiniz. Bu makalede, şirket içi Active Directory (AD) ile Azure Etkin Dizini'ne (Azure AD) eşitleyerek eksik özniteliği nasıl ekleyeceğiniz gösterilmektedir.

Azure AD, Azure AD'den bir SaaS uygulamasına kullanıcı hesapları sağlarken kullanıcı profili oluşturmak için gereken tüm verileri içermelidir. Bazı durumlarda, verileri kullanılabilir hale getirmek için şirket içi AD'nizden Azure AD'ye eşitleme özniteliklerine ihtiyacınız olabilir. Azure AD Connect, azure AD ile belirli öznitelikleri otomatik olarak eşitler, ancak tüm özniteliklere eşitlemeyapmaz. Ayrıca, varsayılan olarak eşitlenen bazı öznitelikler (SAMAccountName gibi) Microsoft Graph API kullanılarak açıkolmayabilir. Bu gibi durumlarda, özniteliği Azure AD'ye eşitlemek için Azure AD Connect dizin uzantısı özelliğini kullanabilirsiniz. Bu şekilde, öznitelik Microsoft Graph API ve Azure AD sağlama hizmeti tarafından görülebilir.

Sağlama için ihtiyacınız olan veriler Active Directory'deyse ancak yukarıda açıklanan nedenlerden dolayı sağlama için kullanılamıyorsa, aşağıdaki adımları izleyin.
 
## <a name="sync-an-attribute"></a>Bir özniteliği eşitle 

1. Azure AD Connect sihirbazını açın, Görevler'i seçin ve ardından **eşitleme seçeneklerini özelleştir'i**seçin.

   ![Azure Etkin Dizin Bağlama sihirbazı Ek görevler sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Azure AD Global Yöneticisi olarak oturum açın. 

3. **İsteğe Bağlı Özellikler** sayfasında, **Dizin uzantısı öznitelik eşitle'yi**seçin.
 
   ![Azure Active Directory Connect sihirbazı İsteğe bağlı özellikler sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Azure AD'ye genişletmek istediğiniz öznitelik(ler)'i seçin.
   > [!NOTE]
   > **Kullanılabilir Öznitelikler** altında arama büyük/küçük harf duyarlıdır.

   ![Azure Active Directory Connect sihirbazı Dizin uzantıları seçim sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Azure AD Connect sihirbazını bitirin ve tam bir eşitleme döngüsünün çalışmasına izin verin. Döngü tamamlandığında şema genişletilir ve yeni değerler şirket içi AD ile Azure AD'niz arasında eşitlenir.
 
6. Azure portalında, [kullanıcı öznitelik eşlemelerini düzenlerken,](customize-application-attributes.md) **Kaynak öznitelik** listesi artık biçimde `<attributename> (extension_<appID>_<attributename>)`eklenen özniteliği içerecektir. Özniteliği seçin ve sağlama için hedef uygulamaya eşle.

   ![Azure Active Directory Connect sihirbazı Dizin uzantıları seçim sayfası](./media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> **Şirket** içi AD'den yönetici veya **DN/DistinguishedName**gibi referans özniteliklerini sağlama yeteneği günümüzde desteklenmez. Bu özelliği Kullanıcı [Sesi'nde](https://feedback.azure.com/forums/169401-azure-active-directory)isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlerin sağlama kapsamında olduğunu tanımlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
