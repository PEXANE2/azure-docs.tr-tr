---
title: Azure anahtar kasasına erişmek için uygulamalara izin verme - Azure Key Vault | Microsoft Dokümanlar
description: Önemli bir kasaya erişmek için birçok uygulamaya nasıl izin verebilirsiniz öğrenin
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 09/27/2019
ms.author: mbaldwin
ms.openlocfilehash: a3be46f2b50afbe2347445a8b3220c74d1bfc52c
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656981"
---
# <a name="provide-key-vault-authentication-with-an-access-control-policy"></a>Erişim denetimi ilkesiyle Key Vault kimlik doğrulaması sağlayın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Key Vault'a bulut tabanlı bir uygulamanın kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](managed-identity.md)  Bir ön hazırlık uygulaması oluşturuyorsanız, yerel geliştirme yapıyorsanız veya yönetilen bir kimliği kullanamıyorsanız, bunun yerine bir hizmet ilkesini el ile kaydedebilir ve bir erişim denetimi ilkesini kullanarak anahtar kasanıza erişim sağlayabilirsiniz.  

Key vault, 1024'e kadar erişim ilkesi girişini destekler ve her giriş bir "ana para" için ayrı bir izin kümesi verir: Örneğin, [.NET quickstart için Azure Key Vault istemci kitaplığındaki](quick-create-net.md) konsol uygulaması anahtar kasasına böyle erişir.

Key Vault erişim denetimi hakkında tüm ayrıntılar için [Azure Key Vault güvenliği: Kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management)bilgisine bakın. [Anahtarlar, Sırlar ve Sertifikalar](about-keys-secrets-and-certificates.md) erişim denetimi hakkında tüm ayrıntılar için bkz: 

- [Anahtarlar erişim denetimi](about-keys-secrets-and-certificates.md#key-access-control)
- [Sırlar erişim denetimi](about-keys-secrets-and-certificates.md#secret-access-control)
- [Sertifikalar erişim denetimi](about-keys-secrets-and-certificates.md#certificate-access-control)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

- Anahtar kasası. Varolan bir anahtar kasası kullanabilir veya aşağıdaki hızlı başlangıçlardan birinde aşağıdaki adımları izleyerek yeni bir tane oluşturabilirsiniz:
   - [Azure CLI ile önemli bir kasa oluşturun](quick-create-cli.md)
   - [Azure PowerShell ile önemli bir kasa oluşturun](quick-create-powershell.md)
   - [Azure portalı ile önemli bir kasa oluşturun.](quick-create-portal.md)
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell.](/powershell/azure/overview) Alternatif olarak, Azure [portalını](https://portal.azure.com)kullanabilirsiniz.

## <a name="grant-access-to-your-key-vault"></a>Anahtar kasanıza erişim izni ver

Her anahtar kasa erişim ilkesi girişi, bir ana müdüre farklı bir izin kümesi verir:

- **Bir uygulama** Uygulama bulut tabanlıysa, bunun yerine [Azure Key Vault'a erişmek için yönetilen bir kimlik kullanmalısınız](managed-identity.md), mümkünse
- **Azure AD grubu** Anahtar kasası yalnızca 1024 erişim ilkesi girişini desteklese de, tek bir Azure REKLAM grubuna birden çok uygulama ve kullanıcı ekleyebilir ve ardından bu grubu erişim denetim ilkenize tek bir giriş olarak ekleyebilirsiniz.
- **Bir Kullanıcı** Kullanıcılara anahtar kasasına doğrudan erişim sağlamak **önerilmez.** İdeal olarak, kullanıcılar bir Azure REKLAM grubuna eklenmelidir ve bu da anahtar kasasına erişim hakkı verilir. Bkz. [Azure Key Vault güvenliği: Kimlik ve erişim yönetimi.](overview-security.md#identity-and-access-management)


### <a name="get-the-objectid"></a>ObjectID'yi alın

Bir uygulamaya, Azure REKLAM grubuna veya anahtar kasanıza kullanıcı erişimi vermek için öncelikle objectId'ini almanız gerekir.

#### <a name="applications"></a>Uygulamalar

Bir uygulama için objectId ilişkili hizmet ilkesiile karşılık gelir. Hizmet müdürleri hakkında tüm ayrıntılar için. Bkz. [Azure Etkin Dizini'ndeki uygulama ve hizmet temel nesneleri.](../active-directory/develop/app-objects-and-service-principals.md) 

Bir uygulama için objectid almanın iki yolu vardır.  Bunlardan ilki, uygulamanızı Azure Active Directory'ye kaydettirmektir. Bunu yapmak için, Microsoft kimlik platformu ile bir uygulamayı hızlı başlat'ta [kaydedin](../active-directory/develop/quickstart-register-app.md)adımlarını izleyin. Kayıt tamamlandığında objectid "Application (istemci) kimliği" olarak listelenir.

İkincisi, terminal penceresinde bir servis ilkesi oluşturmaktır. Azure CLI ile [az reklam sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanın.

```azurecli-interactive
az ad sp create-for-rbac -n "http://mySP"
```

ObjectId çıktıda `clientID`.

Azure PowerShell ile [Yeni-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal?view=azps-2.7.0) cmdlet'i kullanın.


```azurepowershell-interactive
New-AzADServicePrincipal -DisplayName mySP
```

ObjectId çıktıda (değil) `Id` `ApplicationId`olarak listelenir.

#### <a name="azure-ad-groups"></a>Azure REKLAM Grupları

Azure AD grubuna birden çok uygulama ve kullanıcı ekleyebilir ve ardından gruba anahtar kasanıza erişim izni verebilirsiniz.  Daha fazla ayrıntı için aşağıdaki [Azure REKLAM grubu bölümüne üye oluşturma ve ekleme](#creating-and-adding-members-to-an-azure-ad-group) bölümüne bakın.

Azure CLI ile bir Azure REKLAM grubunun objectId'ini bulmak için [az reklam grubu listesi](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-list) komutunu kullanın. Kuruluşunuzdaki çok sayıda grup nedeniyle, `--display-name` parametreye bir arama dizesi de sağlamanız gerekir.

```azurecli-interactive
az ad group list --display-name <search-string>
```
ObjectId JSON döndürülecektir:

```json
    "objectId": "48b21bfb-74d6-48d2-868f-ff9eeaf38a64",
    "objectType": "Group",
    "odata.type": "Microsoft.DirectoryServices.Group",
```

Azure PowerShell ile bir Azure REKLAM grubunun objectId'ini bulmak için [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup?view=azps-2.7.0) cmdlet'ini kullanın. Kuruluşunuzdaki çok sayıda grup nedeniyle, büyük olasılıkla parametreye bir arama dizesi de sağlamak isteyebilirsiniz. `-SearchString`

```azurepowershell-interactive
Get-AzADGroup -SearchString <search-string>
```

Çıktıda objectId şu şekilde `Id`listelenir:

```console
...
Id                    : 1cef38c4-388c-45a9-b5ae-3d88375e166a
...
```

#### <a name="users"></a>Kullanıcılar

Ayrıca, anahtar kasanın erişim denetimi ilkesine tek bir kullanıcı ekleyebilirsiniz. **Bunu önermiyoruz.** Bunun yerine, bir Azure REKLAM grubuna kullanıcı eklemenizi ve ilkeleri grup eklemenizi öneririz.

Yine de Azure CLI'ye sahip bir kullanıcı bulmak [az ad user show](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-show) istiyorsanız, kullanıcıların e-posta adresini `--id` parametreye geçirerek az reklam kullanıcı göster komutunu kullanın.


```azurecli-interactive
az ad user show --id <email-address-of-user>
```

Kullanıcının objectId'si çıktıda döndürülür:

```console
  ...
  "objectId": "f76a2a6f-3b6d-4735-9abd-14dccbf70fd9",
  "objectType": "User",
  ...
```

Azure PowerShell'e sahip bir kullanıcı bulmak için [Get-AzADUser](/powershell/module/az.resources/get-azaduser?view=azps-2.7.0) cmdlet'ini kullanarak kullanıcıların e-posta adresini `-UserPrincipalName` parametreye aktarın.

```azurepowershell-interactive
 Get-AzAdUser -UserPrincipalName <email-address-of-user>
```

Kullanıcının objectId'si çıktıda `Id`.

```console
...
Id                : f76a2a6f-3b6d-4735-9abd-14dccbf70fd9
Type              :
```

### <a name="give-the-principal-access-to-your-key-vault"></a>Anahtar kasanıza ana erişim verin

Artık müdürünüzin objectid'ine sahip olduğunuza göre, anahtar kasanız için hem anahtarlar hem de sırlar için izinleri ve istediğiniz ek izinleri alma, listeleme, ayarlama ve silme izni veren bir erişim ilkesi oluşturabilirsiniz.

Azure CLI ile bu işlem objectId'yi [az keyvault kümesi ilkesi](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek yapılır.

```azurecli-interactive
az keyvault set-policy -n <your-unique-keyvault-name> --spn <ApplicationID-of-your-service-principal> --secret-permissions get list set delete --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

Azure PowerShell ile bu işlem objectId'yi [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.7.0) cmdlet'e geçirerek yapılır. 

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy –VaultName <your-key-vault-name> -PermissionsToKeys create,decrypt,delete,encrypt,get,list,unwrapKey,wrapKey -PermissionsToSecrets get,list,set,delete -ObjectId <Id>

```

## <a name="creating-and-adding-members-to-an-azure-ad-group"></a>Azure REKLAM grubuna üye oluşturma ve ekleme

Bir Azure REKLAM grubu oluşturabilir, gruba uygulamalar ve kullanıcılar ekleyebilir ve grubun anahtar kasanıza erişmesini sağlayabilirsiniz.  Bu, tek bir erişim ilkesi girişi olarak anahtar kasasına bir dizi uygulama eklemenize olanak tanır ve kullanıcılara anahtar kasanıza doğrudan erişim verme gereksinimini ortadan kaldırır (ki bu bizim vazgeçtiğimiz). Daha fazla ayrıntı için Azure [Etkin Dizin gruplarını kullanarak uygulama ve kaynak erişimini yönet'e](../active-directory/fundamentals/active-directory-manage-groups.md)bakın.

### <a name="additional-prerequisites"></a>Ek önkoşullar

Yukarıdaki ön [koşullara](#prerequisites)ek olarak, Azure Etkin Dizin kiracınızda gruplar oluşturmak/oluşturmak için izinlere ihtiyacınız olacaktır. İzinleriniz yoksa, Azure Active Directory yöneticinize başvurmanız gerekebilir.

PowerShell'i kullanmak istiyorsanız, [Azure AD PowerShell modülüne](https://www.powershellgallery.com/packages/AzureAD/2.0.2.50) de ihtiyacınız olacaktır

### <a name="create-an-azure-active-directory-group"></a>Azure Etkin Dizin grubu oluşturma

Azure CLI az reklam grubu oluşturma komutunu veya Azure PowerShell [New-AzureADGroup](/powershell/module/azuread/new-azureadgroup?view=azureadps-2.0) cmdlet'ini kullanarak yeni bir Azure Etkin Dizin grubu [oluşturun.](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-create)


```azurecli-interactive
az ad group create --display-name <your-group-display-name> --mail-nickname <your-group-mail-nickname>
```

```powershell
New-AzADGroup -DisplayName <your-group-display-name> -MailNickName <your-group-mail-nickname>
```

Her iki durumda da, aşağıdaki adımlar için ihtiyacınız olacağından, yeni oluşturulan GroupId gruplarına dikkat edin.

### <a name="find-the-objectids-of-your-applications-and-users"></a>Uygulamalarınızın ve kullanıcılarınızın objectid'lerini bulun

Az [reklam sp listesi](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-list) komutu ile Azure CLI'yi kullanarak uygulamalarınızın objectId'lerini `--show-mine` parametre ile bulabilirsiniz.

```azurecli-interactive
az ad sp list --show-mine
```

[Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal?view=azps-2.7.0) cmdlet ile Azure PowerShell kullanarak uygulamalarınızın objectId'lerini bulun ve `-SearchString` parametreye bir arama dizesi geçirin.

```azurepowershell-interactive
Get-AzADServicePrincipal -SearchString <search-string>
```

Kullanıcılarınızın objectid'lerini bulmak için yukarıdaki [Kullanıcılar](#users) bölümündeki adımları izleyin.

### <a name="add-your-applications-and-users-to-the-group"></a>Uygulamalarınızı ve kullanıcılarınızı gruba ekleme

Şimdi, objectId'leri yeni oluşturduğunuz Azure REKLAM grubuna ekleyin.

Azure CLI ile, [az reklam grubu üyesinin eklentisini](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add) `--member-id` kullanarak objectId'yi parametreye geçirin.


```azurecli-interactive
az ad group member add -g <groupId> --member-id <objectId>
```

Azure PowerShell ile ObjectId'yi parametreye geçirerek [Add-AzADGroupMember](/powershell/module/az.resources/add-azadgroupmember?view=azps-2.7.0) cmdlet'ini `-MemberObjectId` kullanın.

```azurepowershell-interactive
Add-AzADGroupMember -TargetGroupObjectId <groupId> -MemberObjectId <objectId> 
```

### <a name="give-the-ad-group-access-to-your-key-vault"></a>AD grubuna anahtar kasanıza erişim hakkı verin

Son olarak, Azure CLI [az keyvault ayar ilkesi](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu veya Azure PowerShell [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.7.0) cmdlet'i kullanarak AD grubuna anahtar kasanıza izin verin. Örneğin, yukarıdaki [uygulama, Azure REKLAM grubu veya anahtar kasa bölümüne kullanıcı erişimi ver](#give-the-principal-access-to-your-key-vault) bölümüne bakın.

Uygulama ayrıca anahtar kasasına atanan en az bir Kimlik ve Erişim Yönetimi (IAM) rolüne de ihtiyaç duyar. Aksi takdirde oturum açamaz ve aboneye erişmek için yeterli haklara sahip olarak başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault güvenliği: Kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management)
- [Uygulama Hizmeti yönetilen kimlikle Anahtar Kasa kimlik doğrulaması sağlayın](managed-identity.md)
- [Anahtarlar, sırlar ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
- [Anahtar kasanızı emniyete alalı.](key-vault-secure-your-key-vault.md)
- [Azure Key Vault geliştirici kılavuzu](key-vault-developers-guide.md)
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](key-vault-best-practices.md)
