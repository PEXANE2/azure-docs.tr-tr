---
title: Azure dosya paylaşımlarına erişimi denetleme-şirket içi AD DS kimlik doğrulaması
description: Depolama hesabınızı temsil eden bir Active Directory Domain Services kimliğine izin atamayı öğrenin. Bu, kimlik tabanlı kimlik doğrulamasıyla erişimi denetlemenize olanak tanır.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: rogarana
ms.openlocfilehash: eda083265d94888a4ccfd466f084982614770c41
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84268502"
---
# <a name="part-two-assign-share-level-permissions-to-an-identity"></a>İkinci kısım: bir kimliğe bir kimlik için paylaşma düzeyi izinleri atama

Bu makaleye başlamadan önce, önceki makaleyi tamamladığınızdan emin olun, [hesabınız için AD DS kimlik doğrulamasını etkinleştirin](storage-files-identity-ad-ds-enable.md).

Depolama hesabınızda Active Directory Domain Services (AD DS) kimlik doğrulamasını etkinleştirdikten sonra, dosya paylaşımlarınız için erişim sağlamak üzere paylaşım düzeyi izinleri yapılandırmanız gerekir. Azure dosya paylaşma kaynaklarına erişmek istediğiniz kimliğin hem AD DS hem de Azure AD 'de bulunan bir karma kimlik olması gerekir. Örneğin, AD DS bir kullanıcı olduğunu user1@onprem.contoso.com ve Azure AD 'ye user1@contoso.com Azure AD Connect eşitleme kullanarak eşitletiğinizi varsayalım. Bu kullanıcının Azure dosyalarına erişmesine izin vermek için, öğesine paylaşma düzeyi izinleri atamanız gerekir user1@contoso.com . Aynı kavram, gruplar veya hizmet sorumluları için de geçerlidir. Bu nedenle, Azure AD Connect eşitleme kullanarak AD DS kullanıcıları ve grupları Azure AD 'ye eşitlemeniz gerekir. 

Azure dosya paylaşımınızda AD DS kimlik doğrulamasını desteklemek için AD DS aynı kullanıcıyı veya grubu temsil eden Azure AD kimliğine Share-Level izinleri atanmalıdır. Azure Yönetilen kimlikler (MSU) gibi yalnızca Azure AD 'de bulunan kimliklere yönelik kimlik doğrulama ve yetkilendirme AD DS kimlik doğrulamasıyla desteklenmez. Bu makalede bir kimlik için bir dosya paylaşımının paylaşma düzeyi izinlerinin nasıl atanacağı gösterilmektedir.


## <a name="share-level-permissions"></a>Paylaşma düzeyi izinleri

Genellikle, bir Kullanıcı ve kimlik grubunu temsil eden bir Azure AD grubuna üst düzey erişim yönetimi için paylaşma düzeyi izinlerinin kullanılmasını ve ardından dizin/dosya düzeyine ayrıntılı erişim denetimi için Windows ACL 'Lerini kullanmanızı öneririz. 

Kullanıcılara paylaşma düzeyi izinleri vermek için üç Azure yerleşik rolü vardır:

- **Depolama dosyası VERI SMB paylaşımı okuyucusu** , SMB üzerinden Azure depolama dosya paylaşımlarında okuma erişimine izin verir.
- **Depolama dosyası VERI SMB paylaşımı katılımcısı** , SMB üzerinden Azure depolama dosya paylaşımlarında okuma, yazma ve silme erişimine izin verir.
- **Depolama dosyası VERI SMB paylaşımı yükseltilmiş katılımcısı** , SMB üzerinden Azure depolama dosya paylaşımlarında okuma, yazma, silme ve değiştirme olanağı sağlar.

> [!IMPORTANT]
> Bir dosyanın sahipliğini alma özelliği de dahil olmak üzere bir dosya paylaşımının tam yönetim denetimi, depolama hesabı anahtarının kullanılmasını gerektirir. Yönetim denetimi, Azure AD kimlik bilgileriyle desteklenmez.

Azure portal, Azure PowerShell veya Azure CLı ' yi kullanarak, yerleşik rolleri, bir kullanıcının Azure AD kimliğine, Share-Level izinleri verme amacıyla atayabilirsiniz.

## <a name="assign-an-rbac-role"></a>RBAC rolü atama

### <a name="azure-portal"></a>Azure portal

[Azure Portal](https://portal.azure.com)kullanarak BIR Azure AD kimliğine RBAC rolü atamak için aşağıdaki adımları izleyin:

1. Azure portal dosya paylaşımınıza gidin veya [bir dosya paylaşma oluşturun](storage-how-to-create-file-share.md).
1. **Access Control (IAM)** seçeneğini belirleyin.
1. **Rol ataması Ekle** ' yi seçin
1. **Rol ataması Ekle** dikey penceresinde, **rol** listesinden uygun yerleşik rolü (depolama dosyası veri SMB paylaşma okuyucusu, depolama dosya veri SMB paylaşma katılımcısı) seçin. Varsayılan ayarda **erişime ata erişimi** bırakın: **Azure AD kullanıcısı, Grup veya hizmet sorumlusu**. Hedef Azure AD kimliğini ada veya e-posta adresine göre seçin. Seçilen Azure AD kimliği bir karma kimlik olmalıdır ve yalnızca bulut kimliği olamaz. Bu, aynı kimliğin AD DS de gösterildiği anlamına gelir.
1. Rol atama işlemini gerçekleştirmek için **Kaydet** ' i seçin.

### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği, oturum açma adına göre bir Azure AD kimliğine RBAC rolü atamayı göstermektedir. PowerShell ile RBAC rolleri atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure PowerShell kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-powershell.md).

Aşağıdaki örnek betiği çalıştırmadan önce, köşeli ayraçlar dahil yer tutucu değerlerini değerlerinizle değiştirin.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

### <a name="cli"></a>CLI
  
Aşağıdaki CLı 2,0 komutu, oturum açma adına göre Azure AD kimliğine bir RBAC rolü atar. Azure CLı ile RBAC rolleri atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-cli.md). 

Aşağıdaki örnek betiği çalıştırmadan önce, parantez dahil yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="next-steps"></a>Sonraki adımlar

[Üçüncü kısım: SMB üzerinden dizin ve dosya düzeyi izinleri yapılandırma](storage-files-identity-ad-ds-configure-permissions.md)