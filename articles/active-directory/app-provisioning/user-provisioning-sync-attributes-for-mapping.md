---
title: Eşleme için öznitelikleri Azure AD 'ye eşitler
description: SaaS uygulamalarına Kullanıcı sağlamayı yapılandırırken, varsayılan olarak eşitlenmemiş kaynak özniteliklerini eklemek için dizin uzantısı özelliğini kullanın.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 03/17/2021
ms.author: kenwith
ms.openlocfilehash: 52f34cdafac76a9bca2b4ff0b00e0b3efaa63f5d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104579442"
---
# <a name="syncing-extension-attributes-attributes"></a>Uzantı öznitelikleri öznitelikleri eşitleniyor

Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirirken, eşlemek istediğiniz özniteliğin **kaynak öznitelik** listesinde görünmediğini fark edebilirsiniz. Bu makalede, şirket içi Active Directory (AD) ile Azure Active Directory (Azure AD) arasında eşitleme yaparak veya yalnızca bulut kullanıcısı için Azure AD 'de uzantı öznitelikleri oluşturarak eksik özniteliği nasıl ekleyeceğiniz gösterilmektedir. 

Azure AD, Azure AD 'den bir SaaS uygulamasına kullanıcı hesapları sağlarken bir kullanıcı profili oluşturmak için gereken tüm verileri içermelidir. Bazı durumlarda, verileri kullanılabilir hale getirmek için şirket içi AD 'nizden Azure AD 'ye yönelik öznitelikleri eşitlemeniz gerekebilir. Azure AD Connect, bazı öznitelikleri otomatik olarak Azure AD 'ye eşitler, ancak tüm özniteliklere vermez. Ayrıca, varsayılan olarak eşitlenen bazı öznitelikler (SAMAccountName gibi) Azure AD Graph API kullanılarak sunulmayabilir. Bu durumlarda, özniteliği Azure AD ile eşleştirmek için Azure AD Connect Directory uzantısı özelliğini kullanabilirsiniz. Bu şekilde, öznitelik Azure AD Graph API ve Azure AD sağlama hizmeti tarafından görünür olur. Sağlama için ihtiyaç duyduğunuz veriler Active Directory, ancak yukarıda açıklanan nedenlerden dolayı sağlama için kullanılamazsa, uzantı öznitelikleri oluşturmak için Azure AD Connect kullanabilirsiniz. 

Çoğu kullanıcı büyük olasılıkla Active Directory eşitlenen karma kullanıcılardır, ancak Azure AD Connect kullanmadan yalnızca bulutta bulunan kullanıcılar için de uzantılar oluşturabilirsiniz. PowerShell veya Microsoft Graph kullanarak yalnızca bulut kullanıcısının şemasını genişletebilirsiniz. 

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

## <a name="create-an-extension-attribute-on-a-cloud-only-user"></a>Yalnızca bulut kullanıcısı üzerinde uzantı özniteliği oluşturma
Müşteriler Kullanıcı şemasını genişletmek için Microsoft Graph ve PowerShell 'i kullanabilir. Bu uzantı öznitelikleri çoğu durumda otomatik olarak bulunur, ancak 1000 ' den fazla hizmet sorumlusu olan müşteriler kaynak öznitelik listesinde eksik olan uzantıları bulabilir. Aşağıdaki adımları kullanarak oluşturduğunuz bir öznitelik, kaynak öznitelik listesinde otomatik olarak görünmezse lütfen uzantı özniteliğinin başarıyla oluşturulduğu grafiği kullanmayı doğrulayın ve sonra şemaya [el ile](https://docs.microsoft.com/azure/active-directory/app-provisioning/customize-application-attributes#editing-the-list-of-supported-attributes)ekleyin. Aşağıdaki grafik isteklerini yaparken, istekleri yapmak için gereken izinleri doğrulamak için lütfen daha fazla bilgi ' ye tıklayın. Bir [grafik Gezginini](https://docs.microsoft.com/graph/graph-explorer/graph-explorer-overview) kullanarak istekleri yapabilirsiniz. 

### <a name="create-an-extension-attribute-on-a-cloud-only-user-using-microsoft-graph"></a>Microsoft Graph kullanarak yalnızca bulutta bulunan bir kullanıcı üzerinde uzantı özniteliği oluşturma
Kullanıcılarınızın şemasını genişletmek için bir uygulama kullanmanız gerekir. Kullanıcı şemasını genişletmek için kullanmak istediğiniz uygulamanın kimliğini belirlemek üzere kiracınızdaki uygulamaları listeleyin. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/application-list?view=graph-rest-1.0&tabs=http)

```json
GET https://graph.microsoft.com/v1.0/applications
```

Uzantı özniteliğini oluşturun. Aşağıdaki **ID** özelliğini, önceki adımda alınan **kimlik** ile değiştirin. "AppID" değil, **"ID"** özniteliğini kullanmanız gerekir. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/application-post-extensionproperty?view=graph-rest-1.0&tabs=http)
```json
POST https://graph.microsoft.com/v1.0/applications/{id}/extensionProperties
Content-type: application/json

{
    "name": "extensionName",
    "dataType": "string",
    "targetObjects": [
        "User"
    ]
}
```

Önceki istek, "extension_appID_extensionName" biçimiyle bir uzantı özniteliği oluşturdu. Bir kullanıcıyı uzantı özniteliğiyle güncelleştirin. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0&tabs=http)
```json
PATCH https://graph.microsoft.com/v1.0/users/{id}
Content-type: application/json

{
  "extension_inputAppId_extensionName": "extensionValue"
}
```
Özniteliğin başarıyla güncelleştirildiğinden emin olmak için kullanıcıyı denetleyin. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0&tabs=http#example-3-users-request-using-select)

```json
GET https://graph.microsoft.com/v1.0/users/{id}?$select=displayName,extension_inputAppId_extensionName
```


### <a name="create-an-extension-attribute-on-a-cloud-only-user-using-powershell"></a>PowerShell kullanarak yalnızca bulutta yer alan bir kullanıcı için uzantı özniteliği oluşturma
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
