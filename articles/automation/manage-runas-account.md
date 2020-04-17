---
title: Azure Otomasyonu Hesabı Olarak Çalıştır'ı Yönet
description: Bu makalede, PowerShell'deki veya portaldaki Run As hesaplarınızı nasıl yönetiniziz açıklanmaktadır.
services: automation
ms.subservice: shared-capabilities
ms.date: 05/24/2019
ms.topic: conceptual
ms.openlocfilehash: 4a043bcc2f81214b68b166d443baead6cd230184
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457510"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Azure Otomasyonu Hesabı Olarak Çalıştır'ı Yönet

Azure Otomasyonundaki Run As hesapları, Azure cmdlets'i kullanarak Azure'daki kaynakları yönetmek için kimlik doğrulaması sağlar. Bir Run As hesabı oluşturduğunuzda, Azure Etkin Dizini'nde (AD) yeni bir hizmet yöneticisi kullanıcı oluşturur ve Katılımcı rolünü bu kullanıcıya abonelik düzeyinde atar. Azure sanal makinelerinde Karma Runbook Çalışanları kullanan runbook'lar için, Azure [kaynaklarınızda](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) kimlik doğrulaması yapmak için Çalıştır hesapları yerine Azure kaynakları için yönetilen kimlikleri kullanabilirsiniz.

Bir Run as Hesabının hizmet sorumlusunun varsayılan olarak Azure AD'yi okuma izinleri yoktur. Azure AD'yi okumak veya yönetmek için izin eklemek istiyorsanız, **API izinleri**altında hizmet sorumlusundaki izinleri vermeniz gerekir. Daha fazla bilgi için [bkz.](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="types-of-run-as-accounts"></a>Hesap Olarak Çalıştırma Türleri

Azure Otomasyonu iki tür Çalıştır Hesabı kullanır:

* Azure Çalıştır Hesabı
* Azure Klasik Çalıştır Hesabı

>[!NOTE]
>Azure Bulut Çözüm Sağlayıcısı (CSP) abonelikleri yalnızca Azure Kaynak Yöneticisi modelini destekler. Programda Azure Kaynak Yöneticisi olmayan hizmetler kullanılamaz. Bir CSP aboneliği kullandığınızda, Azure Klasik Çalıştır Hesabı oluşturulmaz, ancak Azure Çalıştır Hesabı oluşturulur. CSP abonelikleri hakkında daha fazla bilgi edinmek için [CSP aboneliklerinde kullanılabilir hizmetlere](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services)bakın.

### <a name="run-as-account"></a>Farklı Çalıştır hesabı

Çalıştır Hesabı Kaynak [Yöneticisi dağıtım modeli](../azure-resource-manager/management/deployment-models.md) kaynaklarını yönetir. Aşağıdaki görevleri yapar.

* Otomatik olarak imzalanan bir sertifika ile Azure AD uygulaması oluşturur, Azure AD’de bu uygulama için bir hizmet sorumlusu hesabı oluşturur ve geçerli aboneliğinizde hesap için Katkıda Bulunan rolünü atar. Sertifika ayarını Sahibi veya başka bir rol olarak değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md).
  
* Belirtilen Otomasyon hesabında adı `AzureRunAsCertificate` geçen bir Otomasyon sertifikası varlığı oluşturur. Sertifika kıymeti, Azure AD uygulamasının kullandığı sertifika özel anahtarını tutar.
  
* Belirtilen Otomasyon hesabında adı `AzureRunAsConnection` geçen bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı uygulama kimliğini, kiracı kimliğini, abonelik kimliğini ve sertifika parmak izini tutar.

### <a name="azure-classic-run-as-account"></a>Azure Klasik Farklı Çalıştır Hesabı

Azure Classic Run As [hesabı, Klasik dağıtım modeli](../azure-resource-manager/management/deployment-models.md) kaynaklarını yönetir. Bu tür bir hesap oluşturmak veya yenilemek için abonelikte ortak yönetici olmalısınız.

Azure Classic Run As hesabı aşağıdaki görevleri gerçekleştirir.

  * Abonelikte bir yönetim sertifikası oluşturur.

  * Belirtilen Otomasyon hesabında adı `AzureClassicRunAsCertificate` geçen bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlığı, yönetim sertifikası tarafından kullanılan sertifika özel anahtarını içerir.

  * Belirtilen Otomasyon hesabında adı `AzureClassicRunAsConnection` geçen bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı abonelik adını, abonelik kimliğini ve sertifika varlık adını tutar.

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Hesap izinleri olarak çalıştır

Bu bölümde, hem normal Run As hesapları hem de Klasik Çalıştır Hesapları için izinler tanımlanır.

### <a name="permissions-to-configure-run-as-accounts"></a>Run As hesaplarını yapılandırma izinleri

Bir Run As hesabı oluşturmak veya güncelleştirmek için belirli ayrıcalıklara ve izinlere sahip olmalısınız. Azure Etkin Dizini'ndeki bir Uygulama yöneticisi ve abonelik sahibi tüm görevleri tamamlayabilir. Görev ayrımına sahip olduğunuz bir durumda, aşağıdaki tablo görevlerin, eşdeğer cmdlet'in ve gereken izinlerin listesini gösterir:

|Görev|Cmdlet  |Minimum İzinler  |İzinleri nerede ayarladığınız|
|---|---------|---------|---|
|Azure AD Uygulaması Oluşturma|[Yeni-AzADUygulaması](https://docs.microsoft.com/powershell/module/az.resources/new-azadapplication?view=azps-3.5.0)     | Uygulama Geliştiricirolü<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure REKLAM > Uygulama Kayıtlarını > Ana Sayfa |
|Uygulamaya bir kimlik bilgisi ekleyin.|[Yeni-AzADAppCredential](https://docs.microsoft.com/powershell/module/az.resources/new-azadappcredential?view=azps-3.5.0)     | Uygulama Yöneticisi veya Genel Yönetici<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure REKLAM > Uygulama Kayıtlarını > Ana Sayfa|
|Azure AD hizmeti ilkesi oluşturma ve alma|[Yeni-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-3.5.0)</br>[Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal?view=azps-3.5.0)     | Uygulama Yöneticisi veya Genel Yönetici<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure REKLAM > Uygulama Kayıtlarını > Ana Sayfa|
|Belirtilen asıl için RBAC rolünü atama veya alma|[New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment?view=azps-3.5.0)</br>[Get-AzRoleAssignment](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleAssignment?view=azps-3.5.0)      | Kullanıcı Erişim Yöneticisi veya Sahibi veya aşağıdaki izinlere sahip:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Abonelik](../role-based-access-control/role-assignments-portal.md)</br>Ev > Abonelikleri \<\> > abonelik adı - Erişim Denetimi (IAM)|
|Otomasyon sertifikası oluşturma veya kaldırma|[Yeni-AzAutomationSertifikası](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.5.0)</br>[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationcertificate?view=azps-3.5.0)     | Kaynak grubundaki katılımcı         |Otomasyon hesabı kaynak grubu|
|Bir Otomasyon bağlantısı oluşturma veya kaldırma|[Yeni-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection?view=azps-3.5.0)</br>[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationconnection?view=azps-3.5.0)|Kaynak grubundaki katılımcı |Otomasyon hesabı kaynak grubu|

<sup>1</sup> Azure AD kiracınızdaki yönetici olmayan kullanıcılar, Azure AD kiracısının Kullanıcıları Kullanıcı ayarları **sayfasındaki uygulamaları kaydedebilirse** [AD uygulamalarını kaydedebilir.](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) **Yes** Uygulama kayıt ayarı **Hayır**ise, bu eylemi gerçekleştiren kullanıcı bu tabloda tanımlandığı gibi olmalıdır.

Aboneliğin Global Administrator rolüne eklenmeden önce aboneliğin Active Directory örneğinin bir üyesi değilseniz, konuk olarak eklenirsiniz. Bu durumda, Otomasyon `You do not have permissions to create…` Hesabı Ekle sayfasında bir uyarı alırsınız. 

Global Yönetici rolü atandığında aboneliğin Active Directory örneğinin bir üyesiyseniz, Otomasyon `You do not have permissions to create…` Hesabı Ekle sayfasında da bir uyarı alabilirsiniz. Bu durumda, aboneliğin Active Directory örneğinden kaldırılmasını ve ardından Yeniden eklenmesini isteyebileceğinizi, böylece Active Directory'de tam kullanıcı olabilirsiniz.

Hata iletisini oluşturan durumun giderildiğini doğrulamak için:

1. Azure portalındaki Azure Etkin Dizin bölmesinden **Kullanıcılar ve gruplar**seçin. 
2. **Tüm kullanıcıları**seçin.
3. Adınızı seçin, ardından **Profil'i**seçin. 
4. Kullanıcı profiliniz altındaki **Kullanıcı türü** özniteliğinin değerinin **Konuk**olarak ayarlanmadığından emin olun.

### <a name="permissions-to-configure-classic-run-as-accounts"></a><a name="permissions-classic"></a>Klasik Çalıştır'ı hesaplar olarak yapılandırma izinleri

Klasik Çalıştır Hesapları'nı yapılandırmak veya yenilemek için abonelik düzeyinde Ortak yönetici rolüne sahip olmalısınız. Klasik abonelik izinleri hakkında daha fazla bilgi edinmek için [Azure klasik abonelik yöneticilerine](../role-based-access-control/classic-administrators.md#add-a-co-administrator)bakın.

## <a name="creating-a-run-as-account-in-azure-portal"></a>Azure portalında Çalıştır hesabı oluşturma

Azure portalında Azure Otomasyon hesabınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin. Çalıştır As ve Klasik Çalıştır As hesaplarını tek tek oluşturun. Klasik kaynak oluşturmanıza gerek yoksa yalnızca Azure Farklı Çalıştır hesabını oluşturabilirsiniz.

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. **Otomasyon Hesaplarını**arayın ve seçin.
3. Otomasyon Hesapları sayfasında, listeden Otomasyon hesabınızı seçin.
4. Sol bölmede, hesap ayarları bölümünde **Hesap Olarak Çalıştır'ı** seçin.
5. Gereken hesaba bağlı olarak **Azure Farklı Çalıştır Hesabı**’nı veya **Azure Klasik Farklı Çalıştır Hesabı**’nı seçin. 
6. İlgi alanı hesabına bağlı olarak, **Azure'a** Ekle As veya **Hesap Olarak Azure Klasik Çalıştır** ekle bölmesini kullanın. Genel bakış bilgilerini inceledikten sonra **Oluştur'u**tıklatın.
6. Azure Farklı Çalıştır hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz. Hesabın oluşturulduğunu belirten bir başlık da görüntülenir. İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="creating-a-run-as-account-using-powershell"></a>PowerShell'i kullanarak Run As hesabı oluşturma

Aşağıdaki liste, PowerShell'de Bir Run As hesabı oluşturmak için gereksinimleri sağlar. Bu gereksinimler her iki run as hesabı türü için de geçerlidir.

* Azure Kaynak Yöneticisi modülleri 3.4.1 ve sonraki modülleri ile Windows 10 veya Windows Server 2016. PowerShell komut dosyası Windows'un önceki sürümlerini desteklemez.
* Azure PowerShell 1.0 ve üzeri. PowerShell 1.0 sürümü hakkında bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).
* Bir Otomasyon hesabı, hangi değer `AutomationAccountName` ve `ApplicationDisplayName` parametreler için başvurulan.
* [Run As hesaplarını yapılandırmak için Gerekli izinlerde](#permissions)listelenenlere eşdeğer izinler.

PowerShell komut `SubscriptionId`dosyası `ResourceGroupName`için gerekli parametreleri almak için sonraki adımları tamamlayın.

1. Azure portalında **Otomasyon Hesapları'nı**seçin.
1. Otomasyon Hesapları sayfasında Otomasyon hesabınızı seçin.
1. Hesap ayarları bölümünde **Özellikler'i**seçin.
1. Özellikler sayfasında **AD,** **ABONELIK Kimliği**ve **KAYNAK GRUBU** değerlerini not edin. Bu değerler, sırasıyla `AutomationAccountName`PowerShell `SubscriptionId` `ResourceGroupName` komut dosyası parametreleri ve PowerShell komut dosyası parametreleri için değerlere karşılık gelir.

   ![Otomasyon hesabı özellikleri sayfası](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>Bir Run As hesabı oluşturmak için PowerShell komut dosyası

Bu bölümde, Bir Run As hesabı oluşturmak için bir PowerShell komut dosyası sağlar. Komut dosyası, çeşitli yapılandırmalar için destek içerir.

* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturun.
* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.
* Kuruluş sertifika yetkiliniz (CA) tarafından verilen bir sertifika kullanarak bir Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı oluşturun.
* Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.

Komut dosyası, kaynak oluşturmak için birden çok Azure Kaynak Yöneticisi cmdlets kullanır. Cmdlets ve gereksinim duydukları izinler [için, Run As hesaplarını yapılandırmak için İzinler'e](#permissions-to-configure-run-as-accounts)bakın.

Dosya adını **kullanarak**komut dosyasını bilgisayarınıza kaydedin.

```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId)
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force
        Remove-AzAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRm.Profile
    Import-Module AzureRm.Resources

    $AureRmProfileVersion = (Get-Module AzureRm.Profile).Version
    if (!(($AzureRmProfileVersion.Major -ge 3 -and $AzureRmProfileVersion.Minor -ge 4) -or ($AzureRmProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    # To use the new Az modules to create your Run As accounts, please uncomment the following lines and ensure you comment out the previous 8 lines that import the AzureRM modules to avoid any issues. To learn about about using Az modules in your Automation account see https://docs.microsoft.com/azure/automation/az-modules.

    # Import-Module Az.Automation
    # Enable-AzureRmAlias


    Connect-AzAccount -Environment $EnvironmentName
    $Subscription = Get-AzSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
```

>[!NOTE]
>`Add-AzAccount`ve `Add-AzureRMAccount` [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0)için takma adlar vardır. Bu cmdlets'i kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleyebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncellemeniz gerekebilir.

### <a name="execute-the-powershell-script"></a>PowerShell komut dosyasını çalıştırma

1. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
1. Yükseltilmiş komut satırı kabuğundan komut dosyanızı içeren klasöre gidin.
1. Gereksinim duyduğunuz yapılandırma için parametre değerlerini kullanarak komut dosyasını çalıştırın.
1. Bir Klasik Çalıştır Hesabı oluşturuyorsanız, komut dosyası yürütüldükten sonra, Ortak Sertifikayı **(.cer** dosya adı uzantısı) Otomasyon hesabının oluşturulduğu abonelik için yönetim deposuna yükleyin.

Komut dosyası yürütüldükten sonra Azure ile kimlik doğrulamanız istenir. Abonelik yöneticileri rolünün üyesi ve aboneliğin eş yöneticisi olan bir hesapla oturum açın.

#### <a name="create-a-run-as-account-by-using-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturma

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-an-enterprise-certificate"></a>Kurumsal sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
```

Kurumsal genel sertifika **(.cer** dosyası) içeren bir Klasik Çalıştır Hesabı oluşturduysanız, bu sertifikayı kullanın. Bkz. [Azure portalına yönetim API sertifikası yükleme.](../azure-api-management-certs.md)

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate-in-the-azure-government-cloud"></a>Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
```

Kendi imzalanmış ortak sertifikaya **(.cer** dosyası) sahip bir Klasik Çalıştır hesabı oluşturduysanız, komut dosyası oluşturur ve bilgisayarınızdaki geçici dosyalar klasörüne kaydeder. PowerShell oturumunu yürütmek `%USERPROFILE%\AppData\Local\Temp`için kullandığınız kullanıcı profilinde bulunabilir.

## <a name="deleting-a-run-as-or-classic-run-as-account"></a>Run As veya Classic Run As hesabını silme

Bu bölümde, Bir Run As veya Classic Run As hesabınasıl silinir. Bu eylemi gerçekleştirdiğinizde Otomasyon hesabı korunur. Hesabı sildikten sonra, hesabı Azure portalında yeniden oluşturabilirsiniz.

1. Azure portalında Otomasyon hesabınızı açın.

2. Sol bölmede, hesap ayarları bölümünde **Hesap Olarak Çalıştır'ı** seçin.

3. Farklı Çalıştır Hesapları özellikleri sayfasında silmek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin. 

4. Seçili hesabın Özellikler bölmesine Sil'i **tıklatın.**

   ![Farklı Çalıştır hesabını silme](media/manage-runas-account/automation-account-delete-runas.png)

5. Hesap silinirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

6. Hesap silindikten sonra Farklı Çalıştır Hesapları özellikler sayfasında **Azure Farklı Çalıştır Hesabı** seçeneğini belirleyerek hesabı yeniden oluşturabilirsiniz.

   ![Otomasyon Farklı Çalıştır hesabını yeniden oluşturma](media/manage-runas-account/automation-account-create-runas.png)

## <a name="renewing-a-self-signed-certificate"></a><a name="cert-renewal"></a>İmzalı sertifikayenileme

Run As hesabı için oluşturduğunuz kendi imzalı sertifikanın süresi, oluşturulma tarihinden itibaren bir yıl sonra sona erer. Run As hesabınız hesap süresi dolmadan önce bir noktada, sertifikayı yenilemeniz gerekir. Süresi dolmadan önce istediğiniz zaman yenileyebilirsiniz. 

Kendi imzalı sertifikayı yenilediğinizde, sıraya dizilmiş veya etkin olarak çalışan ve Run As hesabıyla kimlik doğrulaması yapılan runbook'ların olumsuz etkilenmediğinden emin olmak için geçerli geçerli sertifika korunur. Sertifika, sona erme tarihine kadar geçerliliğini sürdürür.

>[!NOTE]
>Çalıştır hesabının gizliliğinin ihlal edildiğini düşünüyorsanız, kendi imzalı sertifikayı silebilir ve yeniden oluşturabilirsiniz.

>[!NOTE]
>Run As hesabınızı işletme sertifika yetkilisi niz tarafından verilen bir sertifikayı kullanacak şekilde yapılandırıldıysanız ve kendi imzasını taşıyan bir sertifika seçeneğini yeniliyorsanız, kuruluş sertifikası kendi imzalanmış bir sertifikayla değiştirilir.

Kendi imzalı sertifikayı yenilemek için aşağıdaki adımları kullanın.

1. Azure portalında Otomasyon hesabınızı açın.

1. Hesap ayarları bölümünde **Hesap Olarak Çalıştır'ı** seçin.

    ![Otomasyon hesabı özellikleri bölmesi](media/manage-runas-account/automation-account-properties-pane.png)

1. Hesap Olarak Çalıştır özellikleri sayfasında, sertifikayı yenileyebilmek için Çalıştır hesabı veya Klasik Çalıştır Hesabı'nı seçin.

1. Seçili hesabın özellikleri bölmesine **sertifika yı**tıklatın.

    ![Farklı Çalıştır hesabının sertifikasını yenileme](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Sertifika yenilenirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

## <a name="setting-up-automatic-certificate-renewal-with-an-automation-runbook"></a><a name="auto-cert-renewal"></a>Otomasyon runbook ile otomatik sertifika yenileme ayarlama

Sertifikaları otomatik olarak yenilemek için bir otomasyon runbook kullanabilirsiniz. [GitHub'daki](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) bu komut dosyası, Otomasyon hesabınızda bu işlevselliği sağlar.

>[!NOTE]
>Komut dosyasını çalıştırmak için Azure AD'de Genel Yönetici veya Şirket Yöneticisi olmalısınız.

Bu komut dosyası, Run As hesap sertifikalarını yenilemek için haftalık bir zamanlama oluşturur. Otomasyon hesabınıza bir **Güncelleme-AutomationRunAsCredential** runbook ekler. GitHub üzerinde runbook kodunu görüntüleyebilirsiniz, komut dosyası [Güncelleme-AutomationRunAsCredential.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1). Gerektiğinde sertifikaları el ile yenilemek için dosyadaki PowerShell kodunu kullanabilirsiniz.

Yenileme işlemini hemen sınamak için aşağıdaki adımları kullanın.

1. **Update-AutomationRunAsCredential** runbook'u düzenleyin ve **Çıkış(1)** komutunun önüne bir yorum karakteri (#) satırı 122 yerleştirin.

   ```powershell
   #Exit(1)
   ```

2. Runbook'u yayımlayın.
3. Runbook'u başlatın.
4. Başarılı yenilemeyi aşağıdaki kodla doğrulayın:

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```
    Çıktı:

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. Testten sonra, runbook'u edin ve adım 1'de eklediğiniz yorum karakterini kaldırın.
6. Runbook'u yayımlayın.

## <a name="limiting-run-as-account-permissions"></a><a name="limiting-run-as-account-permissions"></a>Hesap İzni Olarak Çalıştır'ı Sınırlama

Otomasyon'un Azure'daki kaynaklara karşı hedeflemesini denetlemek için [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8) komut dosyasını çalıştırabilirsiniz. Bu komut dosyası, özel bir rol tanımı oluşturmak ve kullanmak için varolan Run As hesap hizmeti sorumlusunuzu değiştirir. Rolün [Key Vault](https://docs.microsoft.com/azure/key-vault/)dışındaki tüm kaynaklar için izinleri vardır.

>[!IMPORTANT]
>**Update-AutomationRunAsAccountRoleAssignments.ps1** komut dosyasını çalıştırdıktan sonra, Run As hesapları nın kullanımı yla Key Vault'a erişen runbook'lar artık çalışmaz. Komut dosyasını çalıştırmadan önce, Azure Anahtar Kasası'na yapılan aramalar için hesabınızdaki runbook'ları gözden geçirmelisiniz. Azure Otomasyon runbook'larından Key Vault'a erişimi etkinleştirmek [için, Key Vault'un izinlerine Run As hesabını eklemeniz](#add-permissions-to-key-vault)gerekir.

Hizmet olarak çalıştır asıl'ının yapabileceklerini kısıtlamanız gerekiyorsa, özel rol tanımı `NotActions` öğesine başka kaynak türleri ekleyebilirsiniz. Aşağıdaki örnek, .'a `Microsoft.Compute/*`erişimi kısıtlar. Rol tanımı `NotActions` için bu kaynak türünü eklerseniz, rol herhangi bir İşlem kaynağına erişemez. Rol tanımları hakkında daha fazla bilgi edinmek için azure [kaynakları için rol tanımlarını anlayın'](../role-based-access-control/role-definitions.md)a bakın.

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Run As hesabınız tarafından kullanılan hizmet sorumlusunun Katılımcı rol tanımında mı yoksa özel bir hesapta mı olduğunu belirleyebilirsiniz. 

1. Otomasyon hesabınıza gidin ve hesap ayarları bölümünde **Hesap Olarak Çalıştır'ı** seçin.
2. **Hesap Olarak Azure Çalıştır'ı**seçin. 
3. Kullanılan rol tanımını bulmak için **Rol'u** seçin.

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

Birden çok abonelik veya Otomasyon hesabı için Run As hesapları tarafından kullanılan rol tanımını da belirleyebilirsiniz. Bunu PowerShell Galerisi'ndeki [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) komut dosyasını kullanarak yapın.

### <a name="add-permissions-to-key-vault"></a>Key Vault'a izin ekleme

Azure Otomasyonu'nun Key Vault ve Run As hesap hizmet yöneticinizin özel bir rol tanımı nı kullanıp kullanmadığınızı doğrulamasına izin verebilirsiniz. Şunları yapmalısın:

* Key Vault'a izin ver.
* Erişim ilkesini ayarlayın.

PowerShell Galerisi'ndeki [Extend-AutomationRunAsRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) komut dosyasını kullanarak Run As hesap izinlerinizi Key Vault'a verebilirsiniz. Key Vault'ta izin ayarlama hakkında daha fazla bilgi için [başvurularını önemli bir kasaya erişme](../key-vault/general/group-permissions-for-apps.md) iznine bakın.

## <a name="resolving-misconfiguration-issues-for-run-as-accounts"></a>Run As hesapları için yanlış yapılandırma sorunlarını çözme

Bir Run As veya Classic Run As hesabı için gerekli olan bazı yapılandırma öğeleri ilk kurulum sırasında silinmiş veya yanlış oluşturulmuş olabilir. Olası yapılandırma örnekleri şunlardır:

* Sertifika varlığı
* Bağlantı varlığı
* Katılımcı rolünden kaldırılan hesabı çalıştır
* Azure AD'de hizmet sorumlusu veya uygulama

Bu tür yanlış yapılandırma örnekleri için, Otomasyon hesabı değişiklikleri algılar `Incomplete` ve hesap için Run As Hesapları özellikleri bölmesindeki bir durumu görüntüler.

![Tamamlanmamış Farklı Çalıştır hesabı yapılandırma durumu](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Hesap Olarak Çalıştır'ı seçtiğinizde, hesap özellikleri bölmesi aşağıdaki hata iletisini görüntüler:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Hesabı silip yeniden oluşturarak Farklı Çalıştır hesabıyla ilgili bu sorunları hızlı bir şekilde çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Hizmet ilkeleri hakkında daha fazla bilgi için [Uygulama Nesneleri ve Hizmet Temel Nesneleri'ne](../active-directory/develop/app-objects-and-service-principals.md)bakın.
* Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için [Azure Bulut Hizmetleri için Sertifikalara genel bakış bilgisine](../cloud-services/cloud-services-certs-create.md)bakın.
