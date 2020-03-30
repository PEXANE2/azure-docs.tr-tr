---
title: Azure Active Directory kimlik doğrulamasını yapılandırma
description: Azure AD'yi yapılandırdıktan sonra Azure Active Directory Authentication'ı kullanarak SQL Veritabanı, yönetilen örnek ve SQL Veri Ambarı'na nasıl bağlanış bekleyeceğinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
ms.date: 01/07/2020
ms.openlocfilehash: 881c7076d5131746c730757a07da6fb938429c38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125031"
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql"></a>Azure Active Directory kimlik doğrulamasını SQL ile yapılandırma ve yönetme

Bu makalede, Azure AD'yi nasıl oluşturup doldurabileceğinizi ve ardından Azure [SQL Veritabanı](sql-database-technical-overview.md), [yönetilen örnek](sql-database-managed-instance.md)ve SQL [Veri Ambarı](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)ile Azure AD'yi nasıl kullanacağınızı gösterir. Genel bakış için [bkz.](sql-database-aad-authentication.md)

> [!NOTE]
> Bu makale, Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]  
> Azure VM üzerinde çalışan SQL Server'a bağlanma, Azure Active Directory hesabı kullanılarak desteklenmez. Bunun yerine etki alanı Active Directory hesabı kullanın.

## <a name="create-and-populate-an-azure-ad"></a>Azure REKLAMı oluşturma ve doldurma

Bir Azure REKLAMı oluşturun ve kullanıcılar ve gruplarla doldurun. Azure AD, ilk Azure AD yönetilen etki alanı olabilir. Azure AD, Azure AD ile beslenen şirket içi Bir Active Directory Etki Alanı Hizmetleri de olabilir.

Daha fazla bilgi edinmek için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/hybrid/whatis-hybrid-identity.md), [Kendi etki alanı adınızı Azure AD'ye ekleme](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure artık Windows Server Active Directory ile federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Azure AD dizininizi yönetme](../active-directory/fundamentals/active-directory-administer.md), [Azure AD'yi Windows PowerShell kullanarak yönetme](/powershell/azure/overview) ve [Karma Kimlik için gerekli bağlantı noktaları ve protokoller](../active-directory/hybrid/reference-connect-ports.md).

## <a name="associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Azure Active Directory’ye bir Azure aboneliğini ekleme veya ilişkilendirme

1. Veritabanını barındıran Azure aboneliği için dizin güvenilir bir dizin haline getirerek Azure aboneliğinizi Azure Etkin Dizini ile ilişkilendirin. Ayrıntılar için Azure [aboneliklerinin Azure AD ile nasıl ilişkili olduğunu](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)görün.

2. Etki alanıyla ilişkili abonelmeye geçmek için Azure portalındaki dizin değiştiricisini kullanın.

   > [!IMPORTANT]
   > Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Aboneliğin bir dizinle arasındaki bu güven ilişkisi, bir aboneliğin daha çok abonelik alt kaynakları gibi olan, Azure'daki tüm diğer kaynaklarla (web siteleri, veritabanları ve benzeri) sahip olduğu ilişkiye benzer nitelikte değildir. Bir aboneliğin süresi dolarsa abonelikle ilişkili bu diğer kaynaklara erişim de durdurulur. Ancak dizin Azure içinde kalır, siz de başka bir aboneliği bu dizinle ilişkilendirebilir, dizin kullanıcılarını yönetmeye devam edebilirsiniz. Kaynaklar hakkında daha fazla bilgi için [bkz.](../active-directory/active-directory-b2b-admin-add-users.md) Bu güvenilen ilişki hakkında daha fazla bilgi edinmek [için Azure Etkin Dizin'ine Azure aboneliğini nasıl ilişkilendirleyeceğiniz veya ekleyeceğiniz](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)bölümüne bakın.

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Azure SQL sunucusu için bir Azure AD yöneticisi oluşturma

Her Azure SQL sunucusu (SQL Veritabanı veya SQL Veri Ambarı barındıran) tüm Azure SQL sunucusunun yöneticisi olan tek bir sunucu yöneticisi hesabıyla başlar. Azure AD hesabı olan ikinci bir SQL Server yöneticisi oluşturulmalıdır. Bu ana veritabanında bulunan bir veritabanı kullanıcısı olarak oluşturulur. Yöneticiler olarak, sunucu yöneticisi hesapları her kullanıcı veritabanında **db_owner** rolüüyeleridir ve **dbo** kullanıcı olarak her kullanıcı veritabanıgirin. Sunucu yöneticisi hesapları hakkında daha fazla bilgi için Azure [SQL Veritabanı'nda Veritabanlarını ve Girişleri Yönetme'ye](sql-database-manage-logins.md)bakın.

Azure Etkin Dizin'i coğrafi çoğaltma ile kullanırken, Azure Etkin Dizin yöneticisinin hem birincil hem de ikincil sunucular için yapılandırılması gerekir. Bir sunucuda Azure Active Directory yöneticisi yoksa, Azure Active Directory girişyapar ve kullanıcılar sunucu hatasına "Bağlanamıyor" alır.

> [!NOTE]
> Azure AD hesabını (Azure SQL sunucu yöneticisi hesabı dahil) temel almayan kullanıcılar, önerilen veritabanı kullanıcılarını Azure AD ile doğrulama izni olmadığından Azure AD tabanlı kullanıcılar oluşturamaz.

## <a name="provision-an-azure-active-directory-administrator-for-your-managed-instance"></a>Yönetilen örneğiniz için bir Azure Etkin Dizin yöneticisi sağlama

> [!IMPORTANT]
> Yalnızca yönetilen bir örneği karşılıyorsanız aşağıdaki adımları izleyin. Bu işlem yalnızca Global/Şirket yöneticisi veya Azure AD'de Ayrıcalıklı Rol Yöneticisi tarafından yürütülebilir. Aşağıdaki adımlar, dizinde farklı ayrıcalıklara sahip kullanıcılara izin verme işlemini açıklar.

> [!NOTE]
> GA'dan önce oluşturulan, ancak GA sonrası çalışmaya devam eden MI için Azure AD yöneticileri için varolan davranışta işlevsel bir değişiklik yoktur. Daha fazla bilgi için mi bölümü [için Yeni Azure AD yöneticisi işlevine](#new-azure-ad-admin-functionality-for-mi) bakın.

Yönetilen örneğinizin, güvenlik grubu üyeliği veya yeni kullanıcılar oluşturma yoluyla kullanıcıların kimlik doğrulaması gibi görevleri başarıyla gerçekleştirmek için Azure AD'yi okumak için izinlere ihtiyacı vardır. Bunun işe yaraması için, Azure AD'yi okumak için yönetilen örneğe izin vermeniz gerekir. Bunu yapmanın iki yolu vardır: Portal ve PowerShell'den. Aşağıdaki her iki yöntem de adımlar.

1. Azure portalında, sağ üst köşede, olası Etkin Dizinlerin listesini bırakmak için bağlantınızı seçin.

2. Varsayılan Azure AD olarak doğru Etkin Dizin'i seçin.

   Bu adım, Active Directory ile ilişkili aboneliği Yönetilen Örnek'e bağlar ve aynı aboneliğin hem Azure REKLAMı hem de Yönetilen Örnek için kullanıldığından emin olun.

3. Yönetilen Örnek'e gidin ve Azure AD tümleştirmesi için kullanmak istediğiniz örneği seçin.

   ![Acar](./media/sql-database-aad-authentication/aad.png)

4. Active Directory yönetici sayfasının üstündeki banner'ı seçin ve geçerli kullanıcıya izin verin. Azure AD'de Global/Şirket yöneticisi olarak oturum açtıysanız, bunu Azure portalından veya aşağıdaki komut dosyasıyla PowerShell'i kullanarak yapabilirsiniz.

    ![hibe izinleri-portal](./media/sql-database-aad-authentication/grant-permissions.png)

    ```powershell
    # Gives Azure Active Directory read permission to a Service Principal representing the managed instance.
    # Can be executed only by a "Company Administrator", "Global Administrator", or "Privileged Role Administrator" type of user.

    $aadTenant = "<YourTenantId>" # Enter your tenant ID
    $managedInstanceName = "MyManagedInstance"

    # Get Azure AD role "Directory Users" and create if it doesn't exist
    $roleName = "Directory Readers"
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    if ($role -eq $null) {
        # Instantiate an instance of the role template
        $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq $roleName}
        Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId
        $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    }

    # Get service principal for managed instance
    $roleMember = Get-AzureADServicePrincipal -SearchString $managedInstanceName
    $roleMember.Count
    if ($roleMember -eq $null) {
        Write-Output "Error: No Service Principals with name '$    ($managedInstanceName)', make sure that managedInstanceName parameter was     entered correctly."
        exit
    }
    if (-not ($roleMember.Count -eq 1)) {
        Write-Output "Error: More than one service principal with name pattern '$    ($managedInstanceName)'"
        Write-Output "Dumping selected service principals...."
        $roleMember
        exit
    }

    # Check if service principal is already member of readers role
    $allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    $selDirReader = $allDirReaders | where{$_.ObjectId -match     $roleMember.ObjectId}

    if ($selDirReader -eq $null) {
        # Add principal to readers role
        Write-Output "Adding service principal '$($managedInstanceName)' to     'Directory Readers' role'..."
        Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId     $roleMember.ObjectId
        Write-Output "'$($managedInstanceName)' service principal added to     'Directory Readers' role'..."

        #Write-Output "Dumping service principal '$($managedInstanceName)':"
        #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
        #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
    }
    else {
        Write-Output "Service principal '$($managedInstanceName)' is already     member of 'Directory Readers' role'."
    }
    ```

5. İşlem başarıyla tamamlandıktan sonra, aşağıdaki bildirim sağ üst köşede gösterecektir:

    ![başarılı](./media/sql-database-aad-authentication/success.png)

6. Artık yönetilen örneğiniz için Azure AD yöneticinizi seçebilirsiniz. Bunun için Active Directory admin sayfasında **Yönetim Komutunu Ayarla** komutunu seçin.

    ![set-admin](./media/sql-database-aad-authentication/set-admin.png)

7. AAD yönetici sayfasında, bir kullanıcıyı arayın, yönetici olacak kullanıcıyı veya grubu seçin ve sonra **Seç'i**seçin.

   Active Directory yönetici sayfası, Etkin Dizininizin tüm üyelerini ve gruplarını gösterir. Gri renkte olan kullanıcılar veya gruplar, Azure AD yöneticileri olarak desteklenmedikleri için seçilenemiyor. [Azure AD Özellikleri ve Sınırlamaları'nda](sql-database-aad-authentication.md#azure-ad-features-and-limitations)desteklenen yöneticilerin listesine bakın. Rol tabanlı erişim denetimi (RBAC) yalnızca Azure portalı için geçerlidir ve SQL Server'a yayılmaz.

    ![Azure Etkin Dizin yöneticisi ekleme](./media/sql-database-aad-authentication/add-azure-active-directory-admin.png)

8. Active Directory yönetici sayfasının üst kısmında **Kaydet'i**seçin.

    ![save](./media/sql-database-aad-authentication/save.png)

    Yöneticiyi değiştirme işlemi birkaç dakika sürebilir. Ardından yeni yönetici Active Directory yönetici kutusunda görünür.

Yönetilen örneğiniz için bir Azure REKLAM yöneticisi oluşturduktan sonra, <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a> sözdizimiyle Azure AD sunucu ilkeleri (oturum açmalar) oluşturmaya başlayabilirsiniz. Daha fazla bilgi için [yönetilen örneğin genel görünümüne](sql-database-managed-instance.md#azure-active-directory-integration)bakın.

> [!TIP]
> Daha sonra Bir Yöneticiyi kaldırmak için, Active Directory admin sayfasının üst **kısmında, yöneticiyi kaldır'ı**seçin ve ardından **Kaydet'i**seçin.

### <a name="new-azure-ad-admin-functionality-for-mi"></a>MI için yeni Azure AD yöneticisi işlevi

Aşağıdaki tablo, MI için genel önizleme Azure AD giriş yöneticisinin işlevselliğini ve Azure AD girişleri için GA ile teslim edilen yeni bir işlevselliği özetleyilmiştir.

| Genel önizleme sırasında MI için Azure AD giriş yöneticisi | MI için Azure AD yöneticisi için GA işlevi |
| --- | ---|
| Azure AD kimlik doğrulaması sağlayan SQL Veritabanı için Azure AD yöneticisi gibi benzer şekilde de benzer şekilde olur, ancak Azure AD yöneticisi MI için ana db'de Azure AD veya SQL girişleri oluşturamaz. | Azure AD yöneticisi sysadmin iznine sahiptir ve MI için ana db'de AAD ve SQL girişleri oluşturabilir. |
| sys.server_principals görünümünde mevcut değil | sys.server_principals görünümünde mevcut mu |
| Tek tek Azure AD konuk kullanıcılarının MI için Azure AD yöneticisi olarak ayarlanmasına izin verir. Daha fazla bilgi için azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](../active-directory/b2b/add-users-administrator.md)bakın. | Bu grubu MI için Bir Azure REKLAM yöneticisi olarak ayarlamak için konuk kullanıcıların üye olduğu bir Azure REKLAM grubu oluşturulması nı gerektirir. Daha fazla bilgi için [iş desteği için Azure REKLAM işletmesine](sql-database-ssms-mfa-authentication.md#azure-ad-business-to-business-support)bakın. |

GA'dan önce oluşturulan ve GA sonrası çalışmaya devam eden MI için mevcut Azure AD yöneticileri için en iyi uygulama olarak, aynı Azure AD kullanıcısı veya grubu için Azure portalı "Yöneticiyi kaldır" ve "Yönetici ayarla" seçeneğini kullanarak Azure AD yöneticisini sıfırlayın.

### <a name="known-issues-with-the-azure-ad-login-ga-for-mi"></a>MI için Azure AD giriş GA ile bilinen sorunlar

- T-SQL komutu `CREATE LOGIN [myaadaccount] FROM EXTERNAL PROVIDER`kullanılarak oluşturulan MI ana veritabanında bir Azure AD girişi varsa, MI için Azure AD yöneticisi olarak ayarlanamaz. Azure AD oturum açma oluşturmak için Azure portalı, PowerShell veya CLI komutlarını kullanarak oturum açmayı Azure AD yöneticisi olarak ayarlama hatasıyla karşılaşırsınız.
  - Hesabın Azure AD yöneticisi olarak oluşturulabilmesi için `DROP LOGIN [myaadaccount]`girişin ana veritabanına komutu kullanılarak bırakılması gerekir.
  - `DROP LOGIN` Azure portalında Azure AD yöneticisi hesabını başarılı olduktan sonra ayarlayın. 
  - Azure AD yöneticisi hesabını ayarlayamıyorsanız, oturum açma için yönetilen örneğin ana veritabanını kontrol edin. Aşağıdaki komutu kullanın:`SELECT * FROM sys.server_principals`
  - MI için bir Azure AD yöneticisi ayarlamak, bu hesap için ana veritabanında otomatik olarak bir oturum açma oluşturur. Azure AD yöneticisinin kaldırılması, giriş ini otomatik olarak ana veritabanından düşürür.

- Bireysel Azure REKLAM konuk kullanıcıları MI için Azure AD yöneticileri olarak desteklenmez. Konuk kullanıcıların Azure AD yöneticisi olarak ayarlanabilmesi için bir Azure REKLAM grubunun parçası olması gerekir. Şu anda Azure portal bıçağı, konuk kullanıcıları başka bir Azure REKLAMı için gri yedirerek kullanıcıların yönetici kurulumuna devam etmesine olanak tanır. Konuk kullanıcıları Azure AD yöneticisi olarak kaydetmek kurulumun başarısız olmasını sağlar.
  - Konuk kullanıcıyı MI için Azure AD yöneticisi yapmak istiyorsanız, konuk kullanıcıyı bir Azure AD grubuna ekleyin ve bu grubu Azure AD yöneticisi olarak ayarlayın.

### <a name="powershell-for-sql-managed-instance"></a>SQL yönetilen örnek için PowerShell

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell cmdlets çalıştırmak için Azure PowerShell yüklü ve çalışan olması gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

Bir Azure AD yöneticisi sağlamak için aşağıdaki Azure PowerShell komutlarını uygulayın:

- Bağlan-AzHesap
- Seç-AzSubscription

SQL yönetilen örnek için Azure AD yöneticisisağlamak ve yönetmek için kullanılan cmdlets:

| Cmdlet adı | Açıklama |
| --- | --- |
| [Set-AzSqlInstanceActiveDirectoryAdministrator](/powershell/module/az.sql/set-azsqlinstanceactivedirectoryadministrator) |Geçerli abonelikteki SQL yönetilen örnek için bir Azure AD yöneticisini hükümler. (Geçerli abonelikten olmalı)|
| [Kaldır-AzSqlInstanceActiveDirectoryAdministrator](/powershell/module/az.sql/remove-azsqlinstanceactivedirectoryadministrator) |Geçerli abonelikteki SQL yönetilen örneği için bir Azure AD yöneticisi kaldırır. |
| [Get-AzSqlInstanceActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlinstanceactivedirectoryadministrator) |Geçerli abonelikte SQL yönetilen örneği için bir Azure REKLAM yöneticisi hakkındaki bilgileri verir.|

Aşağıdaki komut, ResourceGroup01 adlı bir kaynak grubuyla ilişkili YönetilenInstance01 adlı yönetilen bir örnek için bir Azure REKLAM yöneticisi hakkında bilgi alır.

```powershell
Get-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstance01"
```

Aşağıdaki komut, Yönetilen Instance 01 adlı yönetilen örnek için DBAs adlı bir Azure AD yönetici grubunu hükümler. Bu sunucu kaynak grubu ResourceGroup01 ile ilişkilidir.

```powershell
Set-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstance01" -DisplayName "DBAs" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353b"
```

Aşağıdaki komut, Kaynak Grubu01 ile ilişkili Yönetilen InstanceName01 adlı yönetilen örnek için Azure AD yöneticisini kaldırır.

```powershell
Remove-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstanceName01" -Confirm -PassThru
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki CLI komutlarını arayarak SQL yönetilen örneği için bir Azure AD yöneticisi de sağlayabilirsiniz:

| Komut | Açıklama |
| --- | --- |
|[az sql mi ad-admin oluşturmak](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-create) | SQL yönetilen örnek için bir Azure Active Directory yöneticisi ni hükümler. (Geçerli abonelikten olmalı) |
|[az sql mi ad-admin silme](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-delete) | SQL yönetilen örneği için bir Azure Active Directory yöneticisikaldırır. |
|[az sql mi ad-admin listesi](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-list) | Şu anda SQL yönetilen örneği için yapılandırılan bir Azure Active Directory yöneticisi hakkındaki bilgileri verir. |
|[az sql mi ad-admin güncelleme](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-update) | Sql yönetilen bir örnek için Active Directory yöneticisini güncelleştirir. |

CLI komutları hakkında daha fazla bilgi için [az sql mi](/cli/azure/sql/mi).

* * *

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>Azure SQL Veritabanı sunucunuz için bir Azure Active Directory yöneticisi sağlama

> [!IMPORTANT]
> Yalnızca bir Azure SQL Veritabanı sunucusu veya Veri Ambarı'nı uyguluyorsanız aşağıdaki adımları izleyin.

Aşağıdaki iki yordam, Azure portalınızda ve PowerShell'i kullanarak Azure SQL sunucunuz için bir Azure Active Directory yöneticisini nasıl sağabileceğinizi gösterir.

### <a name="azure-portal"></a>Azure portalında

1. Sağ üst köşedeki [Azure portalında,](https://portal.azure.com/)olası Etkin Dizinlerin listesini düşürmek için bağlantınızı seçin. Varsayılan Azure AD olarak doğru Etkin Dizin'i seçin. Bu adım, abonelikle ilişkili Active Directory ile Azure SQL sunucusuna bağlanır ve aynı aboneliğin hem Azure AD hem de SQL Server için kullanıldığından emin olun. (Azure SQL sunucusu Azure SQL Veritabanı veya Azure SQL Veri Ambarı barındırabilir.)

    ![seçim-reklam][8]

2. **SQL sunucusu**için arama yapın ve seçin.

    ![SQL sunucularını arama ve seçme](media/sql-database-aad-authentication/search-for-and-select-sql-servers.png)

    >[!NOTE]
    > Bu sayfada, SQL **sunucularını**seçmeden önce, kategoriyi *favori* almak için adın yanındaki **yıldızı** seçebilir ve sol gezinti çubuğuna **SQL sunucuları** ekleyebilirsiniz.

3. **SQL Server** sayfasında **Active Directory admin'i**seçin.

4. Active **Directory admin** **sayfasında, yönetici ayarla'yı**seçin.

    ![SQL sunucuları Active Directory yönetici sini ayarlar](./media/sql-database-aad-authentication/sql-servers-set-active-directory-admin.png)  

5. Yönetici **Ekle** sayfasında, bir kullanıcıyı arayın, yönetici olacak kullanıcıyı veya grubu seçin ve sonra **Seç'i**seçin. (Active Directory yönetici sayfası, Etkin Dizininizin tüm üyelerini ve gruplarını gösterir. Gri renkte olan kullanıcılar veya gruplar, Azure AD yöneticileri olarak desteklenmedikleri için seçilemez. (SQL [Veritabanı veya SQL Veri Ambarı ile kimlik doğrulama için Azure Active Directory Authentication'ı Kullanma'nın Azure](sql-database-aad-authentication.md)REKLAM Özellikleri ve **Sınırlamaları** bölümünde desteklenen yöneticilerin listesine bakın.) Rol tabanlı erişim denetimi (RBAC) yalnızca portal için geçerlidir ve SQL Server'a yayılmaz.

    ![Azure Active Directory yöneticisini seçin](./media/sql-database-aad-authentication/select-azure-active-directory-admin.png)  

6. **Active Directory yönetici** sayfasının üst kısmında **KAYDET'i**seçin.

    ![yönetici kaydet](./media/sql-database-aad-authentication/save-admin.png)

Yöneticiyi değiştirme işlemi birkaç dakika sürebilir. Ardından yeni yönetici Active **Directory yönetici** kutusunda görünür.

   > [!NOTE]
   > Azure AD yöneticisini ayarlarken, yeni yönetici adı (kullanıcı veya grup) sanal ana veritabanında SQL Server kimlik doğrulama kullanıcısı olarak zaten bulunamaz. Varsa, Azure AD yöneticisi kurulumu başarısız olur; oluşturulmasını geri almak ve böyle bir yöneticinin (adın) zaten var olduğunu belirtmek. Böyle bir SQL Server kimlik doğrulama kullanıcısı Azure AD'nin bir parçası olmadığından, Azure AD kimlik doğrulaması kullanarak sunucuya bağlanma çabası başarısız olur.

Daha sonra Bir Yöneticiyi kaldırmak **için, Active Directory admin** sayfasının üst **kısmında, yöneticiyi kaldır'ı**seçin ve ardından **Kaydet'i**seçin.

### <a name="powershell-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Azure SQL Veritabanı ve Azure SQL Veri Ambarı için PowerShell

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell cmdlets çalıştırmak için Azure PowerShell yüklü ve çalışan olması gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Bir Azure AD yöneticisi sağlamak için aşağıdaki Azure PowerShell komutlarını uygulayın:

- Bağlan-AzHesap
- Seç-AzSubscription

Azure SQL Veritabanı ve Azure SQL Veri Ambarı için Azure AD yöneticisini sağlamak ve yönetmek için kullanılan cmdlets:

| Cmdlet adı | Açıklama |
| --- | --- |
| [Set-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/set-azsqlserveractivedirectoryadministrator) |Azure SQL sunucusu veya Azure SQL Veri Ambarı için bir Azure Active Directory yöneticisini hükümler. (Geçerli abonelikten olmalı) |
| [Kaldır-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/remove-azsqlserveractivedirectoryadministrator) |Azure SQL sunucusu veya Azure SQL Veri Ambarı için bir Azure Etkin Dizin yöneticisikaldırır. |
| [Get-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator) |Şu anda Azure SQL sunucusu veya Azure SQL Veri Ambarı için yapılandırılan bir Azure Active Directory yöneticisi hakkındaki bilgileri döndürür. |

Bu komutların her biri için daha fazla bilgi görmek için PowerShell komut get-help'ini kullanın. Örneğin, `get-help Set-AzSqlServerActiveDirectoryAdministrator`.

Aşağıdaki komut dosyası, **Grup-23**adlı `40b79501-b343-44ed-9ce7-da4c8cc7353f`bir kaynak grubundaki **demo_server** sunucusu için **DBA_Group** (object ID) adlı bir Azure REKLAM yöneticisi grubunu hükümler:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" -DisplayName "DBA_Group"
```

**DisplayName** giriş parametresi, Azure AD ekran adını veya Kullanıcı Ana adı kabul eder. Örneğin, ``DisplayName="John Smith"`` ve ``DisplayName="johns@contoso.com"``. Azure REKLAM grupları için yalnızca Azure AD görüntü adı desteklenir.

> [!NOTE]
> Azure PowerShell ```Set-AzSqlServerActiveDirectoryAdministrator``` komutu, desteklenmeyen kullanıcılar için Azure AD yöneticileri sağlamanızı engellemez. Desteklenmeyen bir kullanıcı sağlanabilir, ancak veritabanına bağlanamaz.

Aşağıdaki örnekte isteğe bağlı **ObjectID**kullanır:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" `
    -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> Azure AD **ObjectID,** **DisplayName** benzersiz olmadığında gereklidir. **ObjectID** ve **DisplayName** değerlerini almak için Azure Klasik Portalı'nın Active Directory bölümünü kullanın ve bir kullanıcı veya grubun özelliklerini görüntüleyin.

Aşağıdaki örnek, Azure SQL sunucusu için geçerli Azure AD yöneticisi hakkında bilgi verir:

```powershell
Get-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Aşağıdaki örnek, bir Azure REKLAM yöneticisini kaldırır:

```powershell
Remove-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki CLI komutlarını arayarak bir Azure AD yöneticisi sağlayabilirsiniz:

| Komut | Açıklama |
| --- | --- |
|[az sql server ad-admin oluşturmak](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-create) | Azure SQL sunucusu veya Azure SQL Veri Ambarı için bir Azure Active Directory yöneticisini hükümler. (Geçerli abonelikten olmalı) |
|[az sql server ad-admin silme](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-delete) | Azure SQL sunucusu veya Azure SQL Veri Ambarı için bir Azure Etkin Dizin yöneticisikaldırır. |
|[az sql server ad-admin listesi](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) | Şu anda Azure SQL sunucusu veya Azure SQL Veri Ambarı için yapılandırılan bir Azure Active Directory yöneticisi hakkındaki bilgileri döndürür. |
|[az sql server ad-admin güncelleme](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-update) | Bir Azure SQL sunucusu veya Azure SQL Veri Ambarı için Active Directory yöneticisini güncelleştirir. |

CLI komutları hakkında daha fazla bilgi için [az sql server'a](/cli/azure/sql/server)bakın.

* * *

> [!NOTE]
> Ayrıca, REST API'lerini kullanarak bir Azure Etkin Dizin Yöneticisi de sağlayabilirsiniz. Daha fazla bilgi için, [Azure SQL Veritabanı için Azure SQL Veritabanı İşlemleri için Hizmet Yönetimi REST API Başvurusu ve İşlemleri'ne](/rest/api/sql/) bakın

## <a name="configure-your-client-computers"></a>İstemci bilgisayarlarınızı yapılandırma

Uygulamalarınızın veya kullanıcılarınızın Azure AD kimliklerini kullanarak Azure SQL Veritabanı'na veya Azure SQL Veri Ambarı'na bağlandığı tüm istemci makinelerinde aşağıdaki yazılımı yüklemeniz gerekir:

- .NET Framework 4.6 [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx)veya daha sonra .
- SQL Server için Azure Active Directory Authentication Library (*ADAL. DLL*). Aşağıda adal içeren en son SSMS, ODBC ve OLE DB sürücüsü yüklemek için indirme bağlantıları *vardır. DLL* kütüphanesi.
    1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)
    1. [SQL Server için ODBC Driver 17](https://www.microsoft.com/download/details.aspx?id=56567)
    1. [SQL Server için OLE DB Driver 18](https://www.microsoft.com/download/details.aspx?id=56730)

Bu gereksinimleri şu şekilde karşılayabilirsiniz:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) veya [SQL Server Data Tools'un](/sql/ssdt/download-sql-server-data-tools-ssdt) en son sürümünün yüklenmesi .NET Framework 4.6 gereksinimini karşılar.
    - SSMS, ADAL'ın x86 sürümünü *yükler. DLL*.
    - SSDT, ADAL'ın amd64 sürümünü *yükler. DLL*.
    - [Visual Studio Downloads'daki](https://www.visualstudio.com/downloads/download-visual-studio-vs) en son Visual Studio ,.NET Framework 4.6 gereksinimini karşılar, ancak ADAL'ın gerekli amd64 sürümünü *yüklemez. DLL*.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Veritabanınızda Azure AD kimliklerine eşlenen veritabanında bulunan veritabanı kullanıcıları oluşturma

> [!IMPORTANT]
> Yönetilen örnek artık Azure AD sunucu ilkelerini (oturum açmalar) destekler ve bu da Azure AD kullanıcılarından, gruplarından veya uygulamalarından oturum açmalar oluşturmanıza olanak tanır. Azure AD sunucu ilkeleri (oturum açmalar), veritabanı kullanıcılarının dahili bir veritabanı kullanıcısı olarak oluşturulmasına gerek kalmadan yönetilen örneğinizin kimliğini doğrulama olanağı sağlar. Daha fazla bilgi için [yönetilen örneğe genel bakış'](sql-database-managed-instance.md#azure-active-directory-integration)a bakın. Azure AD sunucu ilkeleri (oturum açma) oluşturma sözdizimi için CREATE <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">LOGIN'e</a>bakın.

Azure Active Directory kimlik doğrulaması, veritabanı kullanıcılarının içerdiği veritabanı kullanıcıları olarak oluşturulmasını gerektirir. Azure AD kimliğine dayalı olarak bulunan bir veritabanı kullanıcısı, ana veritabanında oturum açmayan ve Veritabanıyla ilişkili Azure AD dizinindeki bir kimlikle eşlendirilen bir veritabanı kullanıcısıdır. Azure AD kimliği tek bir kullanıcı hesabı veya grup olabilir. İçerdiği veritabanı kullanıcıları hakkında daha fazla bilgi için, [İçe Bilgi Verilen Veritabanı Kullanıcıları-Veritabanınızı Taşınabilir Yapma'a](https://msdn.microsoft.com/library/ff929188.aspx)bakın.

> [!NOTE]
> Azure portalı kullanılarak veritabanı kullanıcıları (yöneticiler hariç) oluşturulamaz. RBAC rolleri SQL Server, SQL Database veya SQL Veri Ambarı'na yayılmaz. Azure RBAC rolleri Azure Kaynaklarını yönetmek için kullanılır ve veritabanı izinleri için geçerli değildir. Örneğin, **SQL Server Katılımcısı** rolü, SQL Veritabanı'na veya SQL Veri Ambarı'na bağlanmak için erişim sağlamaz. Erişim izni doğrudan veritabanında Transact-SQL deyimleri kullanılarak verilmelidir.

> [!WARNING]
> T-SQL CREATE `:` LOGIN `&` ve CREATE USER deyimlerinde kullanıcı adı olarak eklendiğinde kolon veya ampersand gibi özel karakterler desteklenmez.

Azure AD tabanlı bir veritabanı kullanıcısı (veritabanının sahibi sunucu yöneticisi dışında) oluşturmak için, veritabanına en azından **ALTER ANY USER** iznine sahip bir kullanıcı olarak Azure AD kimliğiyle bağlanın. Ardından aşağıdaki Transact-SQL sözdizimini kullanın:

```sql
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name,* bir Azure REKLAM kullanıcısının kullanıcı adı veya azure REKLAM grubunun görüntü adı olabilir.

**Örnekler:** Azure AD federe veya yönetilen etki alanı kullanıcısını temsil eden bir veritabanı kullanıcısı oluşturmak için:

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Azure AD veya federe etki alanı grubunu temsil eden bir veritabanı kullanıcısı oluşturmak için bir güvenlik grubunun görüntü adını sağlayın:

```sql
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

Azure AD belirteci kullanarak bağlanan bir uygulamayı temsil eden bir veritabanı kullanıcısı oluşturmak için:

```sql
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

> [!NOTE]
> Bu komut, SQL'in oturum açmış kullanıcı adına Azure AD'ye ("dış sağlayıcı") erişmesini gerektirir. Bazen, Azure AD'nin bir özel durumu SQL'e geri döndürmesine neden olan durumlar ortaya çıkar. Bu gibi durumlarda, kullanıcı Sql hatası 33134'u görür ve bu hata, AAD'ye özgü hata iletisini içermelidir. Çoğu zaman, hata erişimin reddedildiğini veya kullanıcının kaynağa erişmek için MFA'ya kaydolması gerektiğini veya birinci taraf uygulamalar arasındaki erişimin ön yetkilendirme yoluyla ele alınması gerektiğini söyler. İlk iki durumda, sorun genellikle kullanıcının AAD kiracı ayarlanmış Koşullu Erişim ilkeleri neden olur: onlar dış sağlayıcıya erişimini önlemek. '00000000-0000-0000-c000-00000000000000000000' (AAD Graph API uygulama kimliği) uygulamasına erişime izin vermek için CA ilkelerinin güncellenmesi sorunu çözmelidir. Hatanın birinci taraf uygulamalar arasındaki erişimin ön yetkilendirme yoluyla ele alınması gerektiğini söylemesi durumunda, sorun kullanıcının bir hizmet sorumlusu olarak oturum açmış olmasıdır. Komut, bunun yerine bir kullanıcı tarafından yürütülürse başarılı olmalıdır.

> [!TIP]
> Azure aboneliğinizle ilişkili Azure Etkin Dizini dışında bir Azure Etkin Dizini'nden doğrudan bir kullanıcı oluşturamazsınız. Ancak, ilişkili Active Directory'de (dış kullanıcılar olarak bilinir) kullanıcılar alınan diğer Etkin Dizinlerin üyeleri, kiracı Etkin Dizini'ndeki Bir Etkin Dizin grubuna eklenebilir. Bu AD grubu için bir veritabanı kullanıcısı oluşturarak, dış Active Directory'deki kullanıcılar SQL Veritabanı'na erişebilir.

Azure Active Directory kimliklerine dayalı olarak bulunan veritabanı kullanıcıları oluşturma hakkında daha fazla bilgi için CREATE [USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx)bakın.

> [!NOTE]
> Azure SQL sunucusu için Azure Active Directory yöneticisinin kaldırılması, herhangi bir Azure AD kimlik doğrulama kullanıcısının sunucuya bağlanmasını engeller. Gerekirse, kullanılamaz Azure AD kullanıcıları bir SQL Veritabanı yöneticisi tarafından el ile bırakılabilir.

> [!NOTE]
> Bir Bağlantı **Süresi Doldu**alırsanız, bağlantı `TransparentNetworkIPResolution` dizesinin parametresini false olarak ayarlamanız gerekebilir. Daha fazla bilgi için [.NET Framework 4.6.1 - TransparentNetworkIPResolution ile Bağlantı zaman ayarı sorununa](https://blogs.msdn.microsoft.com/dataaccesstechnologies/20../../connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/)bakın.

Bir veritabanı kullanıcısı oluşturduğunuzda, bu kullanıcı **CONNECT** iznini alır ve **bu** veritabanına PUBLIC rolünün bir üyesi olarak bağlanabilir. Başlangıçta kullanıcıtarafından kullanılabilen tek **izinler, PUBLIC** rolüne verilen izinler veya üyesi oldukları azure REKLAM gruplarına verilen izinlerdir. Azure AD tabanlı bir veritabanı kullanıcısı nı temin ettikten sonra, kullanıcıya, diğer kullanıcı türüne izin vermek gibi ek izinler verebilirsiniz. Genellikle veritabanı rollerine izin verir ve kullanıcıları rollere ekleyin. Daha fazla bilgi için [Veritabanı Altyapısı İzni Temelleri'ne](https://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx)bakın. Özel SQL Veritabanı rolleri hakkında daha fazla bilgi için Azure [SQL Veritabanı'nda Veritabanlarını ve Girişleri Yönetme'ye](sql-database-manage-logins.md)bakın.
Dış kullanıcı olarak yönetilen bir etki alanına aktarılan federe etki alanı kullanıcı hesabı, yönetilen etki alanı kimliğini kullanmalıdır.

> [!NOTE]
> Azure AD kullanıcıları veritabanı meta verilerinde E türü (EXTERNAL_USER) ve X türü (EXTERNAL_GROUPS) olan gruplar için işaretlenir. Daha fazla bilgi için [bkz: sys.database_principals.](https://msdn.microsoft.com/library/ms187328.aspx)

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>SSMS veya SSDT kullanarak kullanıcı veritabanına veya veri ambarına bağlanma  

Azure AD yöneticisinin düzgün şekilde ayarlandığı doğrulanması için Azure AD yöneticisi hesabını kullanarak **ana** veritabanına bağlanın.
Azure AD tabanlı bir veritabanı kullanıcısı (veritabanının sahibi sunucu yöneticisi dışında) sağlamak için veritabanına veritabanına erişimi olan bir Azure AD kimliğiyle bağlanın.

> [!IMPORTANT]
> Visual [Studio 2015'te SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ve SQL Server Data [Tools](https://msdn.microsoft.com/library/mt204009.aspx) ile Azure Active Directory kimlik doğrulaması desteği mevcuttur. Ağustos 2016'da SSMS sürümünde, yöneticilerin telefon görüşmesi, kısa mesaj, pinli akıllı kartlar veya mobil uygulama bildirimi kullanarak Çok Faktörlü Kimlik Doğrulama'yı gerektirmesine olanak tanıyan Active Directory Universal Authentication desteği de dahildir.

## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>SSMS veya SSDT kullanarak bağlanmak için Azure AD kimliği kullanma

Aşağıdaki yordamlar, SQL Server Management Studio veya SQL Server Database Tools'u kullanarak Azure AD kimliğine sahip bir SQL veritabanına nasıl bağlanabileceğinizi gösterir.

### <a name="active-directory-integrated-authentication"></a>Active Directory entegre kimlik doğrulama

Federe bir etki alanından Azure Active Directory kimlik bilgilerinizi kullanarak Windows'da oturum açtıysanız bu yöntemi kullanın.

1. Yönetim Stüdyosu'nu veya Veri Araçlarını Başlat ve **Sunucuya Bağlan** (veya Veritabanı **Motoruna Bağla)** iletişim kutusunda, **Kimlik Doğrulama** kutusunda **Active Directory - Integrated'ı**seçin. Bağlantı için mevcut kimlik bilgileriniz sunulanınacağı için parola gerekmez veya girilebilir.

    ![AD Tümleşik Kimlik Doğrulama'yı seçin][11]

2. **Seçenekler** düğmesini seçin ve **Bağlantı Özellikleri** sayfasında, **veritabanına bağlan** kutusunda bağlanmak istediğiniz kullanıcı veritabanının adını yazın. (AD **etki alanı adı veya kiracı kimliği**" seçeneği yalnızca **MFA bağlantı seçenekleriyle Evrensel** için desteklenir, aksi takdirde gri renktedir.)  

    ![Veritabanı adını seçin][13]

## <a name="active-directory-password-authentication"></a>Etkin Dizin parola kimlik doğrulaması

Azure AD yönetilen etki alanını kullanarak bir Azure AD ana adı ile bağlanırken bu yöntemi kullanın. Ayrıca, örneğin uzaktan çalışırken etki alanına erişimi olmayan federe hesaplar için de kullanabilirsiniz.

Yerel veya federe Azure AD kullanıcıları için Azure AD ile SQL DB/DW'ye kimlik doğrulamak için bu yöntemi kullanın. Yerel kullanıcı, Azure AD'de açıkça oluşturulan ve kullanıcı adı ve parola kullanılarak kimlik doğrulaması yapılırken, federe kullanıcı, etki alanı Azure AD ile beslenen bir Windows kullanıcısıdır. İkinci yöntem (kullanıcı & parolakullanarak) bir kullanıcı windows kimlik bilgilerini kullanmak istediğinde kullanılabilir, ancak yerel makine etki alanıyla birleştirilmez (örneğin, uzaktan erişim kullanarak). Bu durumda, bir Windows kullanıcısı etki alanı hesabını ve parolasını gösterebilir ve federe kimlik bilgilerini kullanarak SQL DB/DW'ye kimlik doğrulaması yapabilir.

1. Yönetim Stüdyosu'nu veya Veri Araçlarını Başlat ve **Sunucuya Bağlan** (veya Veritabanı **Motoruna Bağlan)** iletişim kutusunda, **Kimlik Doğrulama** kutusunda **Active Directory - Password'i**seçin.

2. Kullanıcı **adı** kutusuna, Azure Active Directory kullanıcı **adınızı\@domain.com**biçimine yazın. Kullanıcı adları, Azure Etkin Dizini'nden bir hesap veya Azure Etkin Dizini ile bir etki alanı fetodi hesabı olmalıdır.

3. **Parola** kutusuna, Azure Active Directory hesabı veya federe etki alanı hesabı için kullanıcı parolanızı yazın.

    ![AD Parola Kimlik Doğrulamasını seçin][12]

4. **Seçenekler** düğmesini seçin ve **Bağlantı Özellikleri** sayfasında, **veritabanına bağlan** kutusunda bağlanmak istediğiniz kullanıcı veritabanının adını yazın. (Önceki seçenekteki grafiğe bakın.)

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>İstemci uygulamasından bağlanmak için Azure AD kimliğini kullanma

Aşağıdaki yordamlar, istemci uygulamasından Azure AD kimliğine sahip bir SQL veritabanına nasıl bağlanabileceğinizi gösterir.

### <a name="active-directory-integrated-authentication"></a>Active Directory entegre kimlik doğrulama

Tümleşik Windows kimlik doğrulamasını kullanmak için etki alanınızın Etkin Dizini Azure Active Directory ile birlikte federated olmalıdır. Veritabanına bağlanan istemci uygulamanız (veya bir hizmet), kullanıcının etki alanı kimlik bilgileri altında etki alanı birleştirilmiş bir makinede çalışıyor olmalıdır.

Tümleşik kimlik doğrulaması ve Azure AD kimliğini kullanarak bir veritabanına bağlanmak için veritabanı bağlantı dizesindeki Kimlik Doğrulama anahtar sözcüğünün Active Directory Integrated olarak ayarlanması gerekir. Aşağıdaki C# kodu örneği ADO .NET kullanır.

```csharp
string ConnectionString = @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Bağlantı dizesi `Integrated Security=True` anahtar kelimesi Azure SQL Veritabanı'na bağlanmak için desteklenmez. ODBC bağlantısı yaparken boşlukları kaldırmanız ve Kimlik Doğrulama'yı 'ActiveDirectoryIntegrated' olarak ayarlamanız gerekir.

### <a name="active-directory-password-authentication"></a>Etkin Dizin parola kimlik doğrulaması

Tümleşik kimlik doğrulaması ve Azure AD kimliğini kullanarak bir veritabanına bağlanmak için Kimlik Doğrulama anahtar sözcüğünün Active Directory Password olarak ayarlanması gerekir. Bağlantı dizesi Kullanıcı Kimliği/UID ve Parola/PWD anahtar kelimeleri ve değerleri içermelidir. Aşağıdaki C# kodu örneği ADO .NET kullanır.

```csharp
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

[Azure AD Authentication GitHub Demo'da](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)bulunan demo kodu örneklerini kullanarak Azure AD kimlik doğrulama yöntemleri hakkında daha fazla bilgi edinin.

## <a name="azure-ad-token"></a>Azure AD belirteci

Bu kimlik doğrulama yöntemi, orta katman hizmetlerinin Azure Active Directory'den (AAD) bir belirteç alarak Azure SQL Veritabanı'na veya Azure SQL Veri Ambarı'na bağlanmasını sağlar. Sertifika tabanlı kimlik doğrulama dahil olmak üzere gelişmiş senaryolar sağlar. Azure AD belirteç kimlik doğrulamasını kullanmak için dört temel adımı tamamlamanız gerekir:

1. Uygulamanızı Azure Active Directory'ye kaydedin ve kodunuz için istemci kimliğini alın.
2. Uygulamayı temsil eden bir veritabanı kullanıcısı oluşturun. (6. adımda daha önce tamamlanmıştır.)
3. Uygulamayı çalıştıran istemci bilgisayarda bir sertifika oluşturun.
4. Sertifikayı uygulamanız için anahtar olarak ekleyin.

Örnek bağlantı dizesi:

```csharp
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
conn.AccessToken = "Your JWT token"
conn.Open();
```

Daha fazla bilgi için [SQL Server Security Blog'a](https://blogs.msdn.microsoft.com/sqlsecurity/20../../token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)bakın. Sertifika ekleme hakkında bilgi için azure [etkin dizininde sertifika tabanlı kimlik doğrulamaya başlayın'a](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md)bakın.

### <a name="sqlcmd"></a>sqlcmd

Aşağıdaki ifadeler, [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=53591)edinilebilen sqlcmd'nin 13.1 sürümünü kullanarak bağlanın.

> [!NOTE]
> `sqlcmd`komutu `-G` ile sistem kimlikleri ile çalışmıyor ve bir kullanıcı ana giriş gerektirir.

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="troubleshooting-azure-ad-authentication"></a>Azure AD Kimlik Doğrulama sorun giderme

Azure AD Kimlik Doğrulaması ile ilgili sorun giderme yle ilgili kılavuzaşağıdaki blogda bulunabilir:<https://techcommunity.microsoft.com/t5/azure-sql-database/troubleshooting-problems-related-to-azure-ad-authentication-with/ba-p/1062991>

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı'ndaki oturum açma, kullanıcılar, veritabanı rolleri ve izinlere genel bakış [için](sql-database-manage-logins.md)bkz.
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).

<!--Image references-->
[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png
