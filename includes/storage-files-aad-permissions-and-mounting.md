---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/12/2019
ms.author: rogara
ms.custom: include file
ms.openlocfilehash: 23550c83e76631e44d5036e0a038f01b61a79f1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79208242"
---
## <a name="assign-access-permissions-to-an-identity"></a>Bir kimliğe erişim izinleri atama

Kimlik tabanlı kimlik doğrulamayla Azure Dosyaları kaynaklarına erişmek için, bir kimliğin (kullanıcı, grup veya hizmet sorumlusu) paylaşım düzeyinde gerekli izinlere sahip olması gerekir. Bu işlem, belirli bir kullanıcının dosya paylaşımına sahip olduğu erişim türünü belirttiğiniz Windows paylaşım izinlerini belirtmeye benzer. Bu bölümdeki kılavuz, bir kimlik için dosya paylaşımı için okuma, yazma veya silme izinlerinin nasıl atayılabildiğini gösterir.

Kullanıcılara paylaşım düzeyi izinleri vermek için üç Azure yerleşik rolü sunduk:

- **Depolama Dosya Verileri SMB Share Reader,** Azure Depolama dosya paylaşımlarında Kobİ üzerinden okuma erişimine izin verir.
- **Depolama Dosya Verileri SMB Share Contributor,** Azure Depolama dosya paylaşımlarında SMB üzerinden erişimin okunmasına, yazılmasına ve silinmesine izin verir.
- **Depolama Dosya Verileri SMB Share Yüksek Katılımcı,** Azure Depolama dosya paylaşımlarında NTFS izinlerini SMB üzerinden okumasına, yazmasına, silmesine ve değiştirmesine olanak tanır.

> [!IMPORTANT]
> Bir dosyanın sahipliğini alma olanağı da dahil olmak üzere bir dosya paylaşımının tam yönetim denetimi, depolama hesabı anahtarının kullanılmasını gerektirir. Yönetim denetimi Azure AD kimlik bilgileriyle desteklenmez.

Yerleşik rolleri bir kullanıcının Azure AD kimliğine paylaşım düzeyi izinleri vermek için atamak için Azure portalını, PowerShell'i veya Azure CLI'yi kullanabilirsiniz.

> [!NOTE]
> AD kimlik doğrulaması için REKLAM'ınızı kullanmayı planlıyorsanız, REKLAM kimlik bilgilerinizi Azure AD ile eşitlemeyi unutmayın. AD'den Azure AD'ye parola karma eşitleme isteğe bağlıdır. AD'den senkronize edilen Azure AD kimliğine hisse düzeyi izni verilir.

#### <a name="azure-portal"></a>Azure portalında
[Azure portalını](https://portal.azure.com)kullanarak bir Azure REKLAM kimliğine RBAC rolü atamak için aşağıdaki adımları izleyin:

1. Azure portalında, dosya paylaşımınıza gidin veya [dosya paylaşımı oluşturun.](../articles/storage/files/storage-how-to-create-file-share.md)
2. **Erişim Denetimi 'ni (IAM)** seçin.
3. **Rol ataması ekle'yi** seçin
4. Rol **atama ekle** bıçağında, **Rol** listesinden uygun yerleşik rolü (Depolama Dosyası Verileri SMB Share Reader, Storage File Data SMB Share Contributor) seçin. Varsayılan ayarda **erişim ata** masını bırakın: **Azure AD kullanıcısı, grubu veya hizmet sorumlusu.** Hedef Azure AD kimliğini ada veya e-posta adresine göre seçin.
5. Rol atama işlemini tamamlamak için **Kaydet'i** seçin.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği, oturum açma adına dayalı olarak Azure AD kimliğine nasıl Bir RBAC rolü atanın gösteriş yapılacağını gösterir. PowerShell ile RBAC rolleri atama hakkında daha fazla bilgi için Bkz. [RBAC ve Azure PowerShell kullanarak erişimi yönet.](../articles/role-based-access-control/role-assignments-powershell.md)

Aşağıdaki örnek komut dosyasını çalıştırmadan önce, parantezler de dahil olmak üzere yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
Aşağıdaki CLI 2.0 komutu, oturum açma adına bağlı olarak Azure AD kimliğine RBAC rolünün nasıl atayılabildiğini gösterir. Azure CLI ile RBAC rolleri atama hakkında daha fazla bilgi için Bkz. [RBAC ve Azure CLI'yi kullanarak erişimi yönet.](../articles/role-based-access-control/role-assignments-cli.md) 

Aşağıdaki örnek komut dosyasını çalıştırmadan önce, parantezler de dahil olmak üzere yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="configure-ntfs-permissions-over-smb"></a>NTFS izinlerini Kobİ üzerinden yapılandırma 
RBAC ile paylaşım düzeyi izinleri atadıktan sonra, kök, dizin veya dosya düzeyinde uygun NTFS izinleri atamanız gerekir. Paylaşım düzeyi izinlerini, kullanıcının paylaşıma erişip erişemeyeceğini belirleyen üst düzey kapı cısı olarak düşünün. NTFS izinleri ise, kullanıcının dizin veya dosya düzeyinde neler yapabileceğini belirlemek için daha ayrıntılı bir düzeyde hareket eder.

Azure Files, NTFS temel ve gelişmiş izinlerinin tam kümesini destekler. Paylaşımı monte edip Windows File Explorer'ı kullanarak veya Windows [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) veya [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) komutunu çalıştırarak Azure dosya paylaşımındaki Dizinler ve dosyalardaki NTFS izinlerini görüntüleyebilir ve yapılandırabilirsiniz. 

NTFS'yi süper kullanıcı izinleriyle yapılandırmak için, alan adı ile birleştirilmiş VM'nizden depolama hesabı anahtarınızı kullanarak payı birleştirmeniz gerekir. Komut isteminden bir Azure dosya paylaşımı oluşturmak ve NTFS izinlerini buna göre yapılandırmak için bir sonraki bölümdeki yönergeleri izleyin.

Aşağıdaki izin kümeleri, dosya paylaşımının kök dizininde desteklenir:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\Kullanıcılar:(RX)
- BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
- NT AUTHORITY\Authenticated Kullanıcılar:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- YARATICI SAHİBİ:(OI)(CI)(IO)(F)

### <a name="configure-ntfs-permissions-with-icacls"></a>NTFS izinlerini icacls ile yapılandırma
Kök dizini de dahil olmak üzere dosya paylaşımı altındaki tüm dizinlere ve dosyalara tam izin vermek için aşağıdaki Windows komutunu kullanın. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

NTFS izinlerini ayarlamak için icacls'in nasıl kullanılacağı hakkında daha fazla bilgi için ve desteklenen izinlerin farklı türleri hakkında daha fazla bilgi [için icacls için komut satırı başvurusuna](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls)bakın.

### <a name="mount-a-file-share-from-the-command-prompt"></a>Komut isteminden dosya paylaşımını montaj

Azure dosya paylaşımını monte etmek için Windows **net kullanım** komutunu kullanın. Aşağıdaki örnekte yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Dosya paylaşımlarını montaj hakkında daha fazla bilgi için bkz. Windows [ile Azure dosya paylaşımı nı kullan.](../articles/storage/files/storage-how-to-use-files-windows.md)

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```
### <a name="configure-ntfs-permissions-with-windows-file-explorer"></a>NTFS izinlerini Windows Dosya Gezgini ile yapılandırma
Kök dizini de dahil olmak üzere dosya paylaşımı altındaki tüm dizinlere ve dosyalara tam izin vermek için Windows Dosya Gezgini'ni kullanın.

1. Windows Dosya Gezgini'ni açın ve dosya/dizini sağ tıklatın ve **Özellikler'i** seçin
2. **Güvenlik** sekmesine tıklayın
3. **Edit'e tıklayın... .** izinleri değiştirmek için düğme
4. Mevcut kullanıcıların iznini değiştirebilir veya yeni kullanıcılara izin vermek için **Ekle...'u** tıklatabilirsiniz
5. Yeni kullanıcı eklemek için istem penceresinde, kutuyu seçmek için **nesne adlarını girin** izni vermek istediğiniz hedef kullanıcı adını girin ve hedef kullanıcının tam UPN adını bulmak için Adları **Denetle'yi** tıklatın.
7.  **Tamam'a** tıklayın
8.  Güvenlik sekmesinde, yeni eklenen kullanıcıya vermek istediğiniz tüm izinleri seçin
9.  **Uygula'ya** tıklayın

## <a name="mount-a-file-share-from-a-domain-joined-vm"></a>Etki alanı birleştirilmiş VM'den dosya paylaşımı nı montaj

Aşağıdaki işlem, dosya paylaşımınızın ve erişim izinlerinizin doğru şekilde ayarlandığını ve etki alanı yla birleştirilmiş bir VM'den Azure Dosyası paylaşımına erişebileceğinizi doğrular:

Aşağıdaki resimde gösterildiği gibi, izin vermiş olduğunuz Azure AD kimliğini kullanarak VM'de oturum açın. Azure Dosyaları için AD kimlik doğrulamasını etkinleştirdiyseniz, AD kimlik belgesini kullanın. Azure AD DS kimlik doğrulaması için Azure AD kimlik bilgisi ile oturum açın.

![Kullanıcı kimlik doğrulaması için Azure AD oturum açma ekranLarını gösteren ekran görüntüsü](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

Azure dosya paylaşımını monte etmek için aşağıdaki komutu kullanın. Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kimliğinizi doğruladığıiçin depolama hesabı anahtarını, AD kimlik bilgilerini veya Azure AD kimlik bilgilerini sağlamanız gerekmez. Ad veya Azure AD DS ile kimlik doğrulama için tek oturum açma deneyimi desteklenir.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```
