---
title: Azure Anahtar Kasası 'na erişmek için uygulamalara izin verme-Azure Key Vault | Microsoft Docs
description: Birçok uygulamaya bir anahtar kasasına erişmek için izin verme hakkında bilgi edinin
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 09/27/2019
ms.author: mbaldwin
ms.openlocfilehash: abb61afab3391f9a53ada4881cb186aa9fae3187
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83005924"
---
# <a name="provide-key-vault-authentication-with-an-access-control-policy"></a>Erişim denetimi ilkesiyle Key Vault kimlik doğrulaması sağlama

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Key Vault için bulut tabanlı bir uygulamanın kimlik doğrulamasının en kolay yolu, yönetilen bir kimlikle; Ayrıntılar için [Azure Key Vault erişmek üzere App Service yönetilen bir kimlik kullanma](managed-identity.md) konusuna bakın.  Şirket içi bir uygulama oluşturuyorsanız, yerel geliştirme yapıyor veya yönetilen bir kimlik kullanmıyorsanız, bunun yerine bir hizmet sorumlusunu el ile kaydedebilir ve bir erişim denetim ilkesi kullanarak anahtar kasanıza erişim sağlayabilirsiniz.  

Anahtar Kasası, her giriş "asıl" için ayrı bir izin kümesi veren en fazla 1024 erişim ilkesi girişini destekler: Örneğin, [.net hızlı başlangıç için Azure Key Vault istemci kitaplığındaki](../secrets/quick-create-net.md) konsol uygulamasının anahtar kasasına erişmesi.

Key Vault Access Control hakkında tam Ayrıntılar için bkz. [Azure Key Vault güvenliği: kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management). Erişim denetimiyle ilgili tüm ayrıntılar için bkz.: 

- [Anahtarlar](../keys/index.yml)
- [Gizli dizi erişim denetimi](../secrets/index.yml)
- [Sertifikalar erişim denetimi](../certificates/index.yml)

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

- Bir Anahtar Kasası. Mevcut bir anahtar kasasını kullanabilir veya şu hızlı başlangıçlardan birindeki adımları izleyerek yeni bir tane oluşturabilirsiniz:
   - [Azure CLı ile Anahtar Kasası oluşturma](../secrets/quick-create-cli.md)
   - [Azure PowerShell ile bir Anahtar Kasası oluşturma](../secrets/quick-create-powershell.md)
   - [Azure Portal bir Anahtar Kasası oluşturun](../secrets/quick-create-portal.md).
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview). Alternatif olarak, [Azure Portal](https://portal.azure.com)de kullanabilirsiniz.

## <a name="grant-access-to-your-key-vault"></a>Anahtar kasanıza erişim izni verin

Her Anahtar Kasası erişim ilkesi girişi, bir sorumluya ayrı bir izin kümesi verir:

- **Bir uygulama** Uygulama bulut tabanlıysa, mümkünse [Azure Key Vault erişmek için yönetilen bir kimlik kullanmanız](managed-identity.md)gerekir
- **Bir Azure AD grubu** Anahtar Kasası yalnızca 1024 erişim ilkesi girişlerini desteklediğinden, tek bir Azure AD grubuna birden çok uygulama ve kullanıcı ekleyebilir ve ardından bu grubu, erişim denetimi ilkenize tek bir giriş olarak ekleyebilirsiniz.
- **Bir Kullanıcı** Kullanıcılara bir anahtar kasasına doğrudan erişim verilmesi **önerilmez**. İdeal olarak, kullanıcıların, anahtar kasasına erişim izni verilen bir Azure AD grubuna eklenmesi gerekir. Bkz. [Azure Key Vault güvenliği: kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management).


### <a name="get-the-objectid"></a>ObjectID 'yi al

Anahtar kasanıza bir uygulama, Azure AD grubu veya Kullanıcı erişimi sağlamak için, önce ObjectID 'yi edinmeniz gerekir.

#### <a name="applications"></a>Uygulamalar

Bir uygulama için objectID, ilişkili hizmet sorumlusuna karşılık gelir. Hizmet sorumluları hakkında tam Ayrıntılar için. [Azure Active Directory Içindeki uygulama ve hizmet sorumlusu nesneleri](../../active-directory/develop/app-objects-and-service-principals.md)bölümüne bakın. 

Bir uygulama için ObjectID almanın iki yolu vardır.  Birincisi Azure Active Directory uygulamanızı kaydeder. Bunu yapmak için hızlı başlangıç ' daki adımları izleyerek [Microsoft Identity platformu ile uygulamayı kaydedin](../../active-directory/develop/quickstart-register-app.md). Kayıt tamamlandığında, ObjectID "uygulama (istemci) KIMLIĞI" olarak listelenecektir.

İkincisi, bir Terminal penceresinde bir hizmet sorumlusu oluşturmaktır. Azure CLı ile, [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanın ve "http://&lt;My-Unique-Service-İlkeAdı&gt;" biçiminde benzersiz bir hizmet asıl adı belirtin.

```azurecli-interactive
az ad sp create-for-rbac -n "http://<my-unique-service-principle-name"
```

ObjectID, çıktıda olarak `clientID`listelenecektir.

Azure PowerShell, [New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal?view=azps-2.7.0) cmdlet 'ini kullanın.


```azurepowershell-interactive
New-AzADServicePrincipal -DisplayName <my-unique-service-principle-name>
```

ObjectID, çıkışta (değil `Id` `ApplicationId`) olarak listelenecektir.

#### <a name="azure-ad-groups"></a>Azure AD grupları

Bir Azure AD grubuna birden çok uygulama ve kullanıcı ekleyebilir ve sonra gruba anahtar kasanıza erişim izni verebilirsiniz.  Daha ayrıntılı bilgi için, aşağıdaki [bir Azure AD grubuna üye oluşturma ve ekleme](#creating-and-adding-members-to-an-azure-ad-group) bölümüne bakın.

Azure CLı ile bir Azure AD grubunun ObjectID 'sini bulmak için [az Ad Group List](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-list) komutunu kullanın. Kuruluşunuzda olabilecek çok sayıda grup olduğundan, `--display-name` parametreye bir arama dizesi de sağlamanız gerekir.

```azurecli-interactive
az ad group list --display-name <search-string>
```
ObjectID, JSON içinde döndürülecek:

```json
    "objectId": "48b21bfb-74d6-48d2-868f-ff9eeaf38a64",
    "objectType": "Group",
    "odata.type": "Microsoft.DirectoryServices.Group",
```

Azure PowerShell olan bir Azure AD grubunun ObjectID 'sini bulmak için [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup?view=azps-2.7.0) cmdlet 'ini kullanın. Kuruluşunuzda olabilecek çok sayıda grup olduğundan, muhtemelen `-SearchString` parametreye bir arama dizesi sağlamak isteyeceksiniz.

```azurepowershell-interactive
Get-AzADGroup -SearchString <search-string>
```

Çıkışta ObjectID şöyle `Id`listelenir:

```console
...
Id                    : 1cef38c4-388c-45a9-b5ae-3d88375e166a
...
```

> [!WARNING]
> Azure AD grupları yönetilen kimlikleri desteklemez. Yalnızca hizmet ve Kullanıcı sorumluları desteklenir.

#### <a name="users"></a>Kullanıcılar

Ayrıca, bir anahtar kasasının erişim denetimi ilkesine bireysel bir kullanıcı ekleyebilirsiniz. **Bunu önermeyiz.** Bunun yerine, bir Azure AD grubuna kullanıcı eklemenizi ve grubu ilkelere eklemenizi öneririz.

Azure CLı ile bir Kullanıcı bulmayı Nonetheless isterseniz, kullanıcılar e-posta adresini `--id` parametreye geçirerek [az ad User Show](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-show) komutunu kullanın.


```azurecli-interactive
az ad user show --id <email-address-of-user>
```

Kullanıcının objectID, çıkışta döndürülecek:

```console
  ...
  "objectId": "f76a2a6f-3b6d-4735-9abd-14dccbf70fd9",
  "objectType": "User",
  ...
```

Azure PowerShell olan bir kullanıcıyı bulmak için [Get-AzADUser](/powershell/module/az.resources/get-azaduser?view=azps-2.7.0) cmdlet 'ini kullanın ve kullanıcıların e-posta adresini `-UserPrincipalName` parametreye geçirerek.

```azurepowershell-interactive
 Get-AzAdUser -UserPrincipalName <email-address-of-user>
```

Kullanıcının objectID, çıkışta döndürülür `Id`.

```console
...
Id                : f76a2a6f-3b6d-4735-9abd-14dccbf70fd9
Type              :
```

### <a name="give-the-principal-access-to-your-key-vault"></a>Ana kasanıza sorumlu erişimi verin

Sorumlunuz bir Nesneizine sahip olduğunuza göre, anahtar kasanız için, hem anahtarlar hem de gizlilikler için ve istediğiniz ek izinlerle ilgili Get, List, set ve DELETE izinlerini veren bir erişim ilkesi oluşturabilirsiniz.

Azure CLı ile bu, ObjectID 'yi [az keykasa Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek yapılır.

```azurecli-interactive
az keyvault set-policy -n <your-unique-keyvault-name> --spn <ApplicationID-of-your-service-principal> --secret-permissions get list set delete --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

Azure PowerShell, bu, ObjectID 'yi [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.7.0) cmdlet 'ine geçirerek yapılır. 

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy –VaultName <your-key-vault-name> -PermissionsToKeys create,decrypt,delete,encrypt,get,list,unwrapKey,wrapKey -PermissionsToSecrets get,list,set,delete -ObjectId <Id>

```

## <a name="creating-and-adding-members-to-an-azure-ad-group"></a>Azure AD grubuna üye oluşturma ve ekleme

Bir Azure AD grubu oluşturabilir, gruba uygulamalar ve kullanıcılar ekleyebilir ve gruba anahtar kasanıza erişim izni verebilirsiniz.  Bu, bir anahtar kasasına tek bir erişim ilkesi girişi olarak bir dizi uygulama eklemenize olanak tanır ve kullanıcılara anahtar kasanıza doğrudan erişim verme gereksinimini ortadan kaldırır. Daha ayrıntılı bilgi için bkz. [Azure Active Directory grupları kullanarak uygulama ve kaynak erişimini yönetme](../../active-directory/fundamentals/active-directory-manage-groups.md).

### <a name="additional-prerequisites"></a>Ek önkoşullar

[Yukarıdaki önkoşullara](#prerequisites)ek olarak, Azure Active Directory kiracınızda Grup oluşturma/düzenleme izinlerine de ihtiyacınız olacaktır. İzinleriniz yoksa Azure Active Directory yöneticinize başvurmanız gerekebilir.

PowerShell 'i kullanmayı düşünüyorsanız, [Azure AD PowerShell modülüne](https://www.powershellgallery.com/packages/AzureAD/2.0.2.50) da ihtiyacınız olacaktır.

### <a name="create-an-azure-active-directory-group"></a>Azure Active Directory grubu oluşturma

Azure CLı [az Ad Group Create](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-create) komutunu veya Azure PowerShell [New-azureadgroup](/powershell/module/azuread/new-azureadgroup?view=azureadps-2.0) cmdlet 'ini kullanarak yeni bir Azure Active Directory grubu oluşturun.


```azurecli-interactive
az ad group create --display-name <your-group-display-name> --mail-nickname <your-group-mail-nickname>
```

```powershell
New-AzADGroup -DisplayName <your-group-display-name> -MailNickName <your-group-mail-nickname>
```

Her iki durumda da, aşağıdaki adımlarda gerekli olacak şekilde, yeni oluşturulan gruplar grup grubuna dikkat edin.

### <a name="find-the-objectids-of-your-applications-and-users"></a>Uygulamalarınızın ve kullanıcılarınızın Nesneslarını bulun

Azure CLı kullanarak uygulamalarınızın Nesneslarını, `--show-mine` parametresi ile [az ad SP List](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-list) komutuyla bulabilirsiniz.

```azurecli-interactive
az ad sp list --show-mine
```

[Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal?view=azps-2.7.0) cmdlet 'i ile Azure PowerShell kullanarak uygulamalarınızın nesneslarını bulun ve bir arama dizesini `-SearchString` parametreye geçirerek.

```azurepowershell-interactive
Get-AzADServicePrincipal -SearchString <search-string>
```

Kullanıcılarınızın ObjectID 'leri bulmak için yukarıdaki [Kullanıcılar](#users) bölümündeki adımları izleyin.

### <a name="add-your-applications-and-users-to-the-group"></a>Uygulamalarınızı ve kullanıcılarınızı gruba ekleme

Şimdi, ObjectIDs 'yi yeni oluşturduğunuz Azure AD grubunuza ekleyin.

Azure CLı ile, [az Ad Group member Add](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add)öğesini kullanın, ObjectID öğesini `--member-id` parametresine geçirerek.


```azurecli-interactive
az ad group member add -g <groupId> --member-id <objectId>
```

Azure PowerShell, [Add-AzADGroupMember](/powershell/module/az.resources/add-azadgroupmember?view=azps-2.7.0) cmdlet 'ini kullanın ve objectID `-MemberObjectId` parametresini parametresine geçirerek.

```azurepowershell-interactive
Add-AzADGroupMember -TargetGroupObjectId <groupId> -MemberObjectId <objectId> 
```

### <a name="give-the-ad-group-access-to-your-key-vault"></a>AD grubuna anahtar kasanıza erişim izni verin

Son olarak, Azure CLı [az keykasa Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu veya Azure PowerShell [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.7.0) cmdlet 'ini kullanarak anahtar kasanıza ad grubu izinleri verin. Örnekler için yukarıdaki [uygulama, Azure AD grubu veya Kullanıcı erişimi](#give-the-principal-access-to-your-key-vault) bölümüne bakın.

Uygulamanın Ayrıca anahtar kasasına atanmış en az bir kimlik ve erişim yönetimi (ıAM) rolü olması gerekir. Aksi takdirde, oturum açamayacak ve aboneliğe erişmek için yetersiz haklarla başarısız olacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault güvenliği: kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management)
- [App Service yönetilen bir kimlikle Key Vault kimlik doğrulaması sağlayın](managed-identity.md)
- [Anahtar kasanızın güvenliğini sağlama](secure-your-key-vault.md)).
- [Geliştirici Kılavuzu Azure Key Vault](developers-guide.md)
- [En iyi uygulamaları](best-practices.md) gözden geçirin Azure Key Vault
