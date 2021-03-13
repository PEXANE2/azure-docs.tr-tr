---
title: Eşleme için öznitelikleri Azure AD 'ye eşitler
description: Şirket içi Active Directory özniteliklerini Azure AD 'ye eşitlemeyi öğrenin. SaaS uygulamalarına Kullanıcı sağlamayı yapılandırırken, varsayılan olarak eşitlenmemiş kaynak özniteliklerini eklemek için dizin uzantısı özelliğini kullanın.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 03/12/2021
ms.author: kenwith
ms.openlocfilehash: 0f8369c80a7a219b159f31aacb7d10a0dd009d00
ms.sourcegitcommit: df1930c9fa3d8f6592f812c42ec611043e817b3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103418683"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Bir uygulamaya sağlamak için şirket içi Active Directory bir özniteliği Azure AD 'ye eşitleyin

Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirirken, eşlemek istediğiniz özniteliğin **kaynak öznitelik** listesinde görünmediğini fark edebilirsiniz. Bu makalede, şirket içi Active Directory (AD) ile Azure Active Directory (Azure AD) arasında eşitleme yaparak eksik özniteliği nasıl ekleyeceğiniz gösterilmektedir.

Azure AD, Azure AD 'den bir SaaS uygulamasına kullanıcı hesapları sağlarken bir kullanıcı profili oluşturmak için gereken tüm verileri içermelidir. Bazı durumlarda, verileri kullanılabilir hale getirmek için şirket içi AD 'nizden Azure AD 'ye yönelik öznitelikleri eşitlemeniz gerekebilir. Azure AD Connect, bazı öznitelikleri otomatik olarak Azure AD 'ye eşitler, ancak tüm özniteliklere vermez. Ayrıca, varsayılan olarak eşitlenen bazı öznitelikler (SAMAccountName gibi) Microsoft Graph API kullanılarak gösterilmeyebilir. Bu durumlarda, özniteliği Azure AD ile eşleştirmek için Azure AD Connect Directory uzantısı özelliğini kullanabilirsiniz. Bu şekilde, öznitelik Microsoft Graph API 'SI ve Azure AD sağlama hizmeti tarafından görünür olur.

Sağlama için ihtiyaç duyduğunuz veriler Active Directory, ancak yukarıda açıklanan nedenlerden dolayı sağlama için kullanılamazsa, uzantı öznitelikleri oluşturmak için Azure AD Connect veya PowerShell kullanabilirsiniz. 
 
## <a name="create-an-extension-attribute-using-azure-ad-connect"></a>Azure AD Connect kullanarak uzantı özniteliği oluşturma

1. Azure AD Connect Sihirbazı 'nı açın, görevler ' i ve ardından **eşitleme seçeneklerini Özelleştir**' i seçin.

   ![Azure Active Directory Connect Sihirbazı ek görevler sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Azure AD Genel Yöneticisi olarak oturum açın. 

3. **Isteğe bağlı özellikler** sayfasında, **dizin uzantısı öznitelik eşitleme**' yi seçin.
 
   ![Azure Active Directory Connect sihirbaz Isteğe bağlı özellikler sayfası](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Azure AD 'ye genişletmek istediğiniz öznitelikleri seçin.
   > [!NOTE]
   > **Kullanılabilir öznitelikler** altında arama büyük/küçük harfe duyarlıdır.

   !["Dizin uzantıları" seçim sayfasını gösteren ekran görüntüsü](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Azure AD Connect Sihirbazı 'nı tamamlayıp tam eşitleme döngüsünün çalışmasına izin verin. Cycle tamamlandığında, şema genişletilir ve yeni değerler, şirket içi AD ile Azure AD arasında eşitlenir.
 
6. Azure portal, [Kullanıcı özniteliği eşlemelerini düzenlemekte](customize-application-attributes.md)olduğunuz sırada, **kaynak öznitelik** listesi artık eklenen özniteliği biçiminde içerecektir `<attributename> (extension_<appID>_<attributename>)` . Özniteliği seçin ve sağlama için hedef uygulamayla eşleyin.

   ![Azure Active Directory Connect Sihirbazı Dizin uzantıları seçim sayfası](./media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> **ManagedBy** veya **DN/distinguishedName 'dir** gibi şirket içi ad 'den başvuru öznitelikleri sağlama özelliği bugün desteklenmez. Bu özelliği [Kullanıcı seste](https://feedback.azure.com/forums/169401-azure-active-directory)isteyebilirsiniz. 

## <a name="create-an-extension-attribute-using-powershell"></a>PowerShell kullanarak uzantı özniteliği oluşturma
PowerShell kullanarak özel bir uzantı oluşturun ve bir kullanıcıya bir değer atayın. 

```
#Connect to your Azure AD tenant   
Connect-AzureAD

#Create an application (you can instead use an existing application if you would like)
$App = New-AzureADApplication -DisplayName “test app name” -IdentifierUris https://testapp

#Create a service principal
New-AzureADServicePrincipal -AppId $App.AppId

#Create an extension property
New-AzureADApplicationExtensionProperty -ObjectId $App.ObjectId -Name “TestAttributeName” -DataType “String” -TargetObjects “User”

#List users in your tenant to determine the objectid for your user
Get-AzureADUser

#Set a value for the extension property on the user. Replace the objectid with the id of the user and the extension name with the value from the previous step
Set-AzureADUserExtension -objectid 0ccf8df6-62f1-4175-9e55-73da9e742690 -ExtensionName “extension_6552753978624005a48638a778921fan3_TestAttributeName”

#Verify that the attribute was added correctly.
Get-AzureADUser -ObjectId 0ccf8df6-62f1-4175-9e55-73da9e742690 | Select -ExpandProperty ExtensionProperty

```

## <a name="next-steps"></a>Sonraki adımlar

* [Sağlama için kimin kapsam içinde olduğunu tanımlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
