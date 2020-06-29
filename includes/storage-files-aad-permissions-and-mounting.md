---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/11/2019
ms.author: rogara
ms.custom: include file
ms.openlocfilehash: 5fc106bfd97e8decd47ac7d43383907dcbbbda9c
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792997"
---
## <a name="2-assign-access-permissions-to-an-identity"></a>2 kimliğe erişim izinleri atama

Kimlik tabanlı kimlik doğrulaması ile Azure dosyaları kaynaklarına erişmek için bir kimlik (Kullanıcı, Grup veya hizmet sorumlusu), paylaşma düzeyinde gerekli izinlere sahip olmalıdır. Bu işlem, belirli bir kullanıcının dosya paylaşımında sahip olduğu erişim türünü belirttiğiniz Windows paylaşma izinlerinin belirtilmesine benzer. Bu bölümdeki kılavuzda, bir kimliğe bir dosya paylaşımının okuma, yazma veya silme izinlerinin nasıl atanacağı gösterilmektedir. 

Kullanıcılara paylaşma düzeyi izinleri vermek için üç Azure yerleşik rolü ekledik:

- **Depolama dosyası VERI SMB paylaşımı okuyucusu** , SMB üzerinden Azure depolama dosya paylaşımlarında okuma erişimine izin verir.
- **Depolama dosyası VERI SMB paylaşımı katılımcısı** , SMB üzerinden Azure depolama dosya paylaşımlarında okuma, yazma ve silme erişimine izin verir.
- **Depolama dosyası VERI SMB paylaşımı yükseltilmiş katkıda bulunanlar** , SMB üzerinden Azure depolama dosya paylaşımlarında okuma, yazma, silme ve değiştirme izinleri sağlar.

> [!IMPORTANT]
> Bir dosyanın sahipliğini alma özelliği de dahil olmak üzere bir dosya paylaşımının tam yönetim denetimi, depolama hesabı anahtarının kullanılmasını gerektirir. Yönetim denetimi, Azure AD kimlik bilgileriyle desteklenmez.

Azure portal, PowerShell veya Azure CLı kullanarak, yerleşik rolleri, bir kullanıcının Azure AD kimliğine, paylaşma düzeyi izinleri vermek için atayabilirsiniz. Paylaşma düzeyi RBAC rol atamasının etkili bir zaman sürebildiği farkında olun. 

> [!NOTE]
> Şirket içi AD DS kimlik doğrulaması için kullanmayı planlıyorsanız [AD DS kimlik bilgilerinizi Azure AD 'ye eşitlemeyi](../articles/active-directory/hybrid/how-to-connect-install-roadmap.md) unutmayın. AD DS 'den Azure AD 'ye Parola karması eşitlemesi isteğe bağlıdır. Paylaşma düzeyi izni, şirket içi AD DS eşitlenmiş Azure AD kimliğine verilecek.

Genel öneri, bir Kullanıcı ve kimlik grubunu temsil eden bir AD grubuna yüksek düzeyde erişim yönetimi için paylaşma düzeyi iznini kullanmak ve ardından dizin/dosya düzeyinde ayrıntılı erişim denetimi için NTFS izinlerinden faydalanır. 

#### <a name="azure-portal"></a>Azure portal
[Azure Portal](https://portal.azure.com)kullanarak BIR Azure AD kimliğine RBAC rolü atamak için aşağıdaki adımları izleyin:

1. Azure portal dosya paylaşımınıza gidin veya [bir dosya paylaşma oluşturun](../articles/storage/files/storage-how-to-create-file-share.md).
2. **Access Control (IAM)** seçeneğini belirleyin.
3. **Rol ataması Ekle** ' yi seçin
4. **Rol ataması Ekle** dikey penceresinde, **rol** listesinden uygun yerleşik rolü (depolama dosyası veri SMB paylaşma okuyucusu, depolama dosya veri SMB paylaşma katılımcısı) seçin. Varsayılan ayarda **erişime ata erişimi** bırakın: **Azure AD kullanıcısı, Grup veya hizmet sorumlusu**. Hedef Azure AD kimliğini ada veya e-posta adresine göre seçin.
5. Rol atama işlemini gerçekleştirmek için **Kaydet** ' i seçin.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği, oturum açma adına göre bir Azure AD kimliğine RBAC rolü atamayı göstermektedir. PowerShell ile RBAC rolleri atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure PowerShell kullanarak erişimi yönetme](../articles/role-based-access-control/role-assignments-powershell.md).

Aşağıdaki örnek betiği çalıştırmadan önce, parantez dahil yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
Aşağıdaki CLı 2,0 komutunda, oturum açma adına göre bir Azure AD kimliğine RBAC rolü atama gösterilmektedir. Azure CLı ile RBAC rolleri atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimi yönetme](../articles/role-based-access-control/role-assignments-cli.md). 

Aşağıdaki örnek betiği çalıştırmadan önce, parantez dahil yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="3-configure-ntfs-permissions-over-smb"></a>3 SMB üzerinde NTFS izinlerini yapılandırma 
RBAC ile paylaşma düzeyi izinleri atadıktan sonra, kök, dizin veya dosya düzeyinde uygun NTFS izinleri atamanız gerekir. Bir kullanıcının paylaşıma erişip erişemeyeceğini belirleyen üst düzey ağ geçidi olarak paylaşma düzeyi izinleri düşünün. NTFS izinleri, kullanıcının dizin veya dosya düzeyinde hangi işlemleri yapabileceğini belirlemek için daha ayrıntılı bir düzeyde davranır.

Azure dosyaları, tüm NTFS temel ve gelişmiş izinleri kümesini destekler. Windows Dosya Gezgini 'ni kullanarak veya Windows [ıacl](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) veya [set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-acl) komutunu çalıştırarak bir Azure dosya PAYLAŞıMıNDAKI dizin ve dosyalarda NTFS izinlerini görüntüleyebilir ve yapılandırabilirsiniz. 

NTFS 'yi üst Kullanıcı izinleriyle yapılandırmak için, etki alanına katılmış VM 'nizden depolama hesabı anahtarınızı kullanarak paylaşıma bağlamanız gerekir. Bir sonraki bölümdeki yönergeleri izleyerek bir Azure dosya paylaşımından komut isteminden bağlama yapın ve NTFS izinlerini uygun şekilde yapılandırın.

Aşağıdaki izin kümeleri bir dosya paylaşımının kök dizininde desteklenir:

- BUILTIN\Administrators: (OI) (CI) (F)
- NT AUTHORıTY\SYSTEM: (OI) (Cı) (F)
- BUILTIN\Users: (RX)
- BUILTIN\Users: (OI) (CI) (GÇ) (GR, GE)
- NT Authorıty\authenticated users: (OI) (CI) (ı)
- NT AUTHORıTY\SYSTEM: (F)
- OLUŞTURAN SAHIBI: (OI) (Cı) (GÇ) (F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Komut isteminden bir dosya paylaşma bağlama

Azure dosya paylaşımından bağlamak için Windows **net use** komutunu kullanın. Aşağıdaki örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Dosya paylaşımları bağlama hakkında daha fazla bilgi için bkz. [Windows Ile Azure dosya paylaşımı kullanma](../articles/storage/files/storage-how-to-use-files-windows.md). 

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /user:Azure\<storage-account-name> <storage-account-key>
```

Azure dosyalarına bağlanırken sorunlarla karşılaşırsanız, lütfen [Windows 'Ta Azure dosyaları bağlama hataları için yayımladığımız sorun giderme aracına](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5)bakın. Ayrıca, 445 numaralı bağlantı noktası engellendiğinde senaryolar etrafında çalışmak için [rehberlik](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) sunuyoruz. 


### <a name="configure-ntfs-permissions-with-windows-file-explorer"></a>Windows Dosya Gezgini ile NTFS izinlerini yapılandırma
Kök dizin dahil olmak üzere dosya paylaşımındaki tüm dizinlere ve dosyalara tam izin vermek için Windows Dosya Gezgini 'ni kullanın.

1. Windows Dosya Gezgini 'ni açın ve dosya/dizine sağ tıklayıp **Özellikler**' i seçin.
2. **Güvenlik** sekmesini seçin.
3. Düzenle 'yi seçin **.** izinleri değiştirmek için.
4. Mevcut kullanıcıların izinlerini değiştirebilir veya yeni kullanıcılara izin vermek için **Ekle..** . seçeneğini belirleyebilirsiniz.
5. Yeni Kullanıcı ekleme istemi penceresinde, izin vermek istediğiniz hedef Kullanıcı adını **Seçilecek nesne adlarını girin** kutusuna girin ve hedef kullanıcının tam UPN adını bulmak Için **adları denetle** ' yi seçin.
7.    **Tamam**’ı seçin.
8.    **Güvenlik** sekmesinde, yeni kullanıcıya vermek istediğiniz tüm izinleri seçin.
9.    **Uygula**’yı seçin.

### <a name="configure-ntfs-permissions-with-icacls"></a>Iacl 'lerle NTFS izinlerini yapılandırma
Kök dizin dahil olmak üzere, dosya paylaşımındaki tüm dizin ve dosyalara tam izinler vermek için aşağıdaki Windows komutunu kullanın. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

NTFS izinlerini ve desteklenen farklı türlerdeki izinleri ayarlamak için ıacl 'leri kullanma hakkında daha fazla bilgi için bkz. [ıacl 'ler için komut satırı başvurusu](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="4-mount-a-file-share-from-a-domain-joined-vm"></a>4 etki alanına katılmış bir VM 'den bir dosya paylaşma bağlama

Aşağıdaki işlem, dosya paylaşımınızın ve erişim izninizin doğru ayarlandığını ve bir Azure dosya paylaşımının etki alanına katılmış bir VM 'den erişebileceğini doğrular. Paylaşma düzeyi RBAC rol atamasının etkili bir zaman sürebildiği farkında olun. 

Aşağıdaki görüntüde gösterildiği gibi, izin verdiğiniz Azure AD kimliğini kullanarak VM 'de oturum açın. Azure dosyaları için şirket içi AD DS kimlik doğrulamasını etkinleştirdiyseniz, AD DS kimlik bilgilerinizi kullanın. Azure AD DS kimlik doğrulaması için Azure AD kimlik bilgileriyle oturum açın.

![Kullanıcı kimlik doğrulaması için Azure AD oturum açma ekranını gösteren ekran görüntüsü](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

Azure dosya paylaşımından bağlama için aşağıdaki komutu kullanın. Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kimliğiniz doğrulandıktan sonra depolama hesabı anahtarını, şirket içi AD DS kimlik bilgilerini veya Azure AD DS kimlik bilgilerini sağlamanız gerekmez. Çoklu oturum açma deneyimi, şirket içi AD DS veya Azure AD DS kimlik doğrulaması için desteklenir. AD DS kimlik bilgileriyle bağlama sorunları yaşıyorsanız, kılavuz için bkz. [Windows 'Da Azure dosyaları sorunlarını giderme](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) .

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```
