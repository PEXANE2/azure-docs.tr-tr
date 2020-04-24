---
title: Azure Otomasyonu farklı çalıştır hesaplarını yönetme
description: Bu makalede, farklı çalıştır hesaplarınızın PowerShell ile veya portaldan nasıl yönetileceği açıklanır.
services: automation
ms.subservice: shared-capabilities
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 5cb3752e5a74f26936efcbb9dba5cdcda76e01f4
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82113315"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Azure Otomasyonu farklı çalıştır hesaplarını yönetme

Azure Otomasyonu 'nda farklı çalıştır hesapları Azure cmdlet 'lerini kullanarak Azure 'da kaynak yönetimine yönelik kimlik doğrulaması sağlar. Farklı Çalıştır hesabı oluşturduğunuzda, Azure Active Directory (AD) içinde yeni bir hizmet sorumlusu kullanıcısı oluşturur ve bu kullanıcıya abonelik düzeyinde katılımcı rolü atar.

## <a name="types-of-run-as-accounts"></a>Farklı Çalıştır hesabı türleri

Azure Otomasyonu iki farklı çalıştır hesabı türü kullanır:

* Azure farklı çalıştır hesabı
* Azure klasik farklı çalıştır hesabı

>[!NOTE]
>Azure bulut çözümü sağlayıcısı (CSP) abonelikleri yalnızca Azure Resource Manager modelini destekler. Azure Resource Manager olmayan hizmetler programda yok. CSP aboneliği kullanırken, Azure klasik farklı çalıştır hesabı oluşturulmaz, ancak Azure farklı çalıştır hesabı oluşturulur. CSP abonelikleri hakkında daha fazla bilgi edinmek için bkz. [CSP aboneliklerinde kullanılabilir hizmetler](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).

Farklı Çalıştır hesabı için hizmet sorumlusu varsayılan olarak Azure AD 'yi okuma izinlerine sahip değildir. Azure AD 'yi okuma veya yönetme izinleri eklemek istiyorsanız, **API izinleri**altında hizmet sorumlusu için izinleri vermeniz gerekir. Daha fazla bilgi için bkz. [Web API 'lerine erişim Izinleri ekleme](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

### <a name="run-as-account"></a>Farklı Çalıştır hesabı

Farklı Çalıştır hesabı, [Kaynak Yöneticisi dağıtım modeli](../azure-resource-manager/management/deployment-models.md) kaynaklarını yönetir. Aşağıdaki görevleri yapar.

* Otomatik olarak imzalanan bir sertifika ile Azure AD uygulaması oluşturur, Azure AD’de bu uygulama için bir hizmet sorumlusu hesabı oluşturur ve geçerli aboneliğinizde hesap için Katkıda Bulunan rolünü atar. Sertifika ayarını sahip veya başka herhangi bir rol olarak değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md).
  
* Belirtilen Otomasyon hesabında adlı `AzureRunAsCertificate` bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlığı, Azure AD uygulaması tarafından kullanılan sertifika özel anahtarını içerir.
  
* Belirtilen Otomasyon hesabında adlı `AzureRunAsConnection` bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı uygulama KIMLIĞI, kiracı KIMLIĞI, abonelik KIMLIĞI ve sertifika parmak izini barındırır.

### <a name="azure-classic-run-as-account"></a>Azure Klasik Farklı Çalıştır Hesabı

Klasik Azure farklı çalıştır hesabı [klasik dağıtım modeli](../azure-resource-manager/management/deployment-models.md) kaynaklarını yönetir. Bu hesap türünü oluşturmak veya yenilemek için abonelikte ortak yönetici olmanız gerekir.

Azure klasik farklı çalıştır hesabı aşağıdaki görevleri gerçekleştirir.

  * Abonelikte bir yönetim sertifikası oluşturur.

  * Belirtilen Otomasyon hesabında adlı `AzureClassicRunAsCertificate` bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlığı, yönetim sertifikası tarafından kullanılan sertifika özel anahtarını içerir.

  * Belirtilen Otomasyon hesabında adlı `AzureClassicRunAsConnection` bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı, abonelik adı, abonelik KIMLIĞI ve sertifika varlık adını içerir.

>[!NOTE]
>Azure klasik farklı çalıştır hesabı, bir Otomasyon hesabı oluştururken aynı anda varsayılan olarak oluşturulmaz. Bu hesap, bu makalenin ilerleyen kısımlarında açıklanan adımları izleyerek tek tek oluşturulur.

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Farklı Çalıştır hesabı izinleri

Bu bölümde hem normal farklı Çalıştır hesaplarının hem de klasik Farklı Çalıştır hesaplarının izinleri tanımlanmaktadır.

### <a name="permissions-to-configure-run-as-accounts"></a>Farklı Çalıştır hesaplarını yapılandırma izinleri

Farklı Çalıştır hesabı oluşturmak veya güncelleştirmek için, belirli ayrıcalıklara ve izinlere sahip olmanız gerekir. Azure Active Directory ve bir abonelikteki bir sahip uygulama Yöneticisi tüm görevleri tamamlayabilir. Görevlerinin ayrılmanız durumunda aşağıdaki tabloda, görevlerin bir listesi, eşdeğer cmdlet ve gerekli izinler gösterilmektedir:

|Görev|Cmdlet  |Minimum Izinler  |İzinleri ayarladığınız yer|
|---|---------|---------|---|
|Azure AD uygulaması oluşturma|[New-AzADApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azadapplication)     | Uygulama geliştirici rolü<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure AD > uygulama kayıtlarını > giriş |
|Uygulamaya bir kimlik bilgisi ekleyin.|[New-AzADAppCredential](https://docs.microsoft.com/powershell/module/az.resources/new-azadappcredential)     | Uygulama Yöneticisi veya genel yönetici<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure AD > uygulama kayıtlarını > giriş|
|Azure AD hizmet sorumlusu oluşturma ve edinme|[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal)</br>[Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal)     | Uygulama Yöneticisi veya genel yönetici<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure AD > uygulama kayıtlarını > giriş|
|Belirtilen sorumlu için RBAC rolünü ata veya al|[New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment)</br>[Get-AzRoleAssignment](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleAssignment)      | Kullanıcı erişimi Yöneticisi veya sahibi ya da aşağıdaki izinlere sahip:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Abonelik](../role-based-access-control/role-assignments-portal.md)</br>Ana > abonelikleri abonelik \<adı\> >-Access Control (IAM)|
|Otomasyon sertifikası oluşturma veya kaldırma|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate)</br>[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationcertificate)     | Kaynak grubunda katkıda bulunan         |Otomasyon hesabı kaynak grubu|
|Otomasyon bağlantısı oluşturma veya kaldırma|[New-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection)</br>[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationconnection)|Kaynak grubunda katkıda bulunan |Otomasyon hesabı kaynak grubu|

<sup>1</sup> Azure AD kiracınızdaki yönetici olmayan kullanıcılar, Kullanıcı ayarları sayfasında **uygulamalar KAYDEDEBILIYORSANıZ** , [ad uygulamalarını kayıt](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) **edebilir.** Uygulama kayıt ayarı **Hayır**ise, bu eylemi gerçekleştiren kullanıcının bu tabloda tanımlanmış olması gerekir.

Aboneliğin genel yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğinin bir üyesi değilseniz, konuk olarak eklendiniz. Bu durumda, Otomasyon hesabı Ekle sayfasında `You do not have permissions to create…` bir uyarı alırsınız. 

Genel yönetici rolü atandığında aboneliğin Active Directory örneğinin bir üyesiyseniz, Otomasyon hesabı Ekle sayfasında de bir `You do not have permissions to create…` uyarı alabilirsiniz. Bu durumda, aboneliğin Active Directory örneğinden kaldırma isteğinde bulunabilir ve sonra, Active Directory bir tam Kullanıcı olacak şekilde yeniden eklenmeleri istenir.

Hata iletisini üreten durumun giderilmiş olduğunu doğrulamak için:

1. Azure portal Azure Active Directory bölmesinden **Kullanıcılar ve gruplar**' ı seçin. 
2. **Tüm kullanıcılar**' ı seçin.
3. Adınızı seçip **profil**' i seçin. 
4. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin değerinin **Konuk**olarak ayarlı olmadığından emin olun.

### <a name="permissions-to-configure-classic-run-as-accounts"></a><a name="permissions-classic"></a>Klasik farklı çalıştır hesaplarını yapılandırma izinleri

Klasik farklı çalıştır hesaplarını yapılandırmak veya yenilemek için, abonelik düzeyinde ortak yönetici rolüne sahip olmanız gerekir. Klasik abonelik izinleri hakkında daha fazla bilgi için bkz. [Azure klasik abonelik yöneticileri](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="creating-a-run-as-account-in-azure-portal"></a>Azure portal farklı çalıştır hesabı oluşturma

Azure portal Azure Otomasyonu hesabınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin. Farklı Çalıştır ve klasik farklı çalıştır hesaplarını ayrı ayrı oluşturun. Klasik kaynak oluşturmanıza gerek yoksa yalnızca Azure Farklı Çalıştır hesabını oluşturabilirsiniz.

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. **Otomasyon hesaplarını**arayın ve seçin.
3. Otomasyon hesapları sayfasında, listeden Otomasyon hesabınızı seçin.
4. Sol bölmede hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.
5. Gereken hesaba bağlı olarak **Azure Farklı Çalıştır Hesabı**’nı veya **Azure Klasik Farklı Çalıştır Hesabı**’nı seçin. 
6. İlgilendiğiniz hesaba bağlı olarak, **Azure farklı çalıştır** veya **Azure klasik farklı çalıştır hesabı** Ekle bölmesini kullanın. Genel bakış bilgilerini inceledikten sonra **Oluştur**' a tıklayın.
6. Azure Farklı Çalıştır hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz. Hesabın oluşturulduğunu belirten bir başlık de görüntülenir. İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="creating-a-run-as-account-using-powershell"></a>PowerShell kullanarak farklı çalıştır hesabı oluşturma

Aşağıdaki listede, PowerShell 'de bir farklı çalıştır hesabı oluşturma gereksinimleri verilmiştir. Bu gereksinimler her iki farklı çalıştır hesabı türü için de geçerlidir.

* Windows 10 veya Windows Server 2016 Azure Resource Manager modüller 3.4.1 ve üzeri. PowerShell betiği Windows 'un önceki sürümlerini desteklemez.
* Azure PowerShell 1.0 ve üzeri. PowerShell 1.0 sürümü hakkında bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).
* `AutomationAccountName` Ve `ApplicationDisplayName` parametrelerinin değeri olarak başvurulan bir Otomasyon hesabı.
* [Farklı Çalıştır hesaplarını yapılandırmak Için gerekli izinler](#permissions)bölümünde listelenenlere eşdeğer izinler.

, `SubscriptionId` `ResourceGroupName`Ve için, PowerShell betiği için gerekli parametreler olan değerleri almak için sonraki adımları uygulayın.

1. Azure portal **Otomasyon hesapları**' nı seçin.
1. Otomasyon hesapları sayfasında Otomasyon hesabınızı seçin.
1. Hesap ayarları bölümünde **Özellikler**' i seçin.
1. Özellikler sayfasındaki **ad**, **abonelik KIMLIĞI**ve **kaynak grubu** değerlerini göz önünde edin. Bu değerler, sırasıyla `AutomationAccountName`, `SubscriptionId`, ve `ResourceGroupName` PowerShell betiği parametrelerinin değerlerine karşılık gelir.

   ![Otomasyon hesabı Özellikler sayfası](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>Farklı Çalıştır hesabı oluşturmak için PowerShell betiği

Bu bölüm bir farklı çalıştır hesabı oluşturmak için bir PowerShell betiği sağlar. Betik birkaç yapılandırma için destek içerir.

* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturun.
* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.
* Kuruluş sertifika yetkiliniz (CA) tarafından verilen bir sertifika kullanarak bir Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı oluşturun.
* Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.

Betik, kaynak oluşturmak için birden çok Azure Resource Manager cmdlet 'i kullanır. Cmdlet 'ler ve gereken izinler için, bkz. [Farklı Çalıştır hesaplarını yapılandırma izinleri](#permissions-to-configure-run-as-accounts).

Betiği, **New-RunAsAccount. ps1**dosya adını kullanarak bilgisayarınıza kaydedin.

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
>`Add-AzAccount`ve `Add-AzureRMAccount` [Connect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0)için diğer adlardır. Bu cmdlet 'leri kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleştirebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncelleştirmeniz gerekebilir.

### <a name="execute-the-powershell-script"></a>PowerShell betiğini yürütme

1. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
1. Yükseltilmiş komut satırı kabuğundan, komut dosyanızı içeren klasöre gidin.
1. Gerekli yapılandırmanın parametre değerlerini kullanarak betiği yürütün.
1. Klasik farklı çalıştır hesabı oluşturuyorsanız, betik yürütüldükten sonra, genel sertifikayı (**. cer** dosya adı uzantısı) Otomasyon hesabının oluşturulduğu abonelik için yönetim deposuna yükleyin.

Betik yürütüldükten sonra Azure ile kimlik doğrulaması yapmanız istenir. Abonelik yöneticileri rolünün üyesi ve aboneliğin ortak Yöneticisi olan bir hesapla oturum açın.

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

Kurumsal ortak sertifika (**. cer** dosyası) Ile bir klasik farklı çalıştır hesabı oluşturduysanız, bu sertifikayı kullanın. Bkz. [Azure Portal bir YÖNETIM API sertifikası yükleme](../azure-api-management-certs.md).

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate-in-the-azure-government-cloud"></a>Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma

```powershell
.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
```

Otomatik olarak imzalanan bir ortak sertifika (**. cer** dosyası) Ile bir klasik farklı çalıştır hesabı oluşturduysanız, betik dosyayı oluşturur ve bilgisayarınızdaki geçici dosyalar klasörüne kaydeder. PowerShell oturumunu yürütmek için kullandığınız kullanıcı profilinde `%USERPROFILE%\AppData\Local\Temp`bulunabilir.

## <a name="deleting-a-run-as-or-classic-run-as-account"></a>Farklı Çalıştır veya klasik farklı çalıştır hesabını silme

Bu bölümde, farklı çalıştır veya klasik farklı çalıştır hesabının nasıl silineceği açıklanmaktadır. Bu eylemi gerçekleştirdiğinizde Otomasyon hesabı korunur. Hesabı sildikten sonra Azure portal yeniden oluşturabilirsiniz.

1. Azure portalında Otomasyon hesabınızı açın.

2. Sol bölmede hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

3. Farklı Çalıştır Hesapları özellikleri sayfasında silmek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin. 

4. Seçili hesabın Özellikler bölmesinde **Sil**' e tıklayın.

   ![Farklı Çalıştır hesabını silme](media/manage-runas-account/automation-account-delete-runas.png)

5. Hesap silinirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

6. Hesap silindikten sonra Farklı Çalıştır Hesapları özellikler sayfasında **Azure Farklı Çalıştır Hesabı** seçeneğini belirleyerek hesabı yeniden oluşturabilirsiniz.

   ![Otomasyon Farklı Çalıştır hesabını yeniden oluşturma](media/manage-runas-account/automation-account-create-runas.png)

## <a name="renewing-a-self-signed-certificate"></a><a name="cert-renewal"></a>Kendinden imzalı bir sertifikayı yenileme

Farklı Çalıştır hesabı için oluşturduğunuz otomatik olarak imzalanan sertifika, oluşturma tarihinden itibaren bir yıl sonra dolar. Farklı Çalıştır hesabınızın süresi dolmadan önce sertifikayı yenilemeniz gerekir. Bu süreyi, süresi dolmadan önce yenileyebilirsiniz. 

Otomatik olarak imzalanan sertifikayı yenilediğinizde, kuyruğa alınan veya etkin olarak çalışan ve farklı çalıştır hesabıyla kimlik doğrulaması yapılan tüm runbook 'ların olumsuz şekilde etkilenmemesini sağlamak için geçerli geçerli sertifika tutulur. Sertifika, sona erme tarihine kadar geçerliliğini sürdürür.

>[!NOTE]
>Farklı çalıştır hesabının tehlikede olduğunu düşünüyorsanız, otomatik olarak imzalanan sertifikayı silip yeniden oluşturabilirsiniz.

>[!NOTE]
>Farklı Çalıştır hesabınızı, kuruluş sertifika yetkiliniz tarafından verilen bir sertifika kullanmak üzere yapılandırdıysanız ve otomatik olarak imzalanan bir sertifika seçeneğini yenileme seçeneğini kullanırsanız, kurumsal sertifika otomatik olarak imzalanan bir sertifika ile değiştirilmiştir.

Otomatik olarak imzalanan sertifikayı yenilemek için aşağıdaki adımları kullanın.

1. Azure portalında Otomasyon hesabınızı açın.

1. Hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

    ![Otomasyon hesabı özellikleri bölmesi](media/manage-runas-account/automation-account-properties-pane.png)

1. Farklı Çalıştır hesapları özellikleri sayfasında, sertifikayı yenilemek istediğiniz farklı çalıştır hesabını veya klasik farklı çalıştır hesabını seçin.

1. Seçili hesabın Özellikler bölmesinde **Sertifikayı Yenile**' ye tıklayın.

    ![Farklı Çalıştır hesabının sertifikasını yenileme](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Sertifika yenilenirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

## <a name="setting-up-automatic-certificate-renewal-with-an-automation-runbook"></a><a name="auto-cert-renewal"></a>Otomatik sertifika yenilemeyi bir Otomasyon Runbook 'u ile ayarlama

Sertifikaları otomatik olarak yenilemek için bir Otomasyon Runbook 'unu kullanabilirsiniz. [GitHub](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) 'daki bu betik Otomasyon hesabınızda bu işlevselliği sunar.

>[!NOTE]
>Betiği yürütmek için Azure AD 'de bir genel yönetici veya şirket yöneticisi olmanız gerekir.

Bu betik, farklı çalıştır hesabı sertifikalarını yenilemek için haftalık bir zamanlama oluşturur. Otomasyon hesabınıza bir **Update-AutomationRunAsCredential** runbook 'u ekler. Runbook kodunu GitHub 'da, [Update-AutomationRunAsCredential. ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1)betiği içinde görüntüleyebilirsiniz. Gerektiğinde, sertifikaları el ile yenilemek için PowerShell kodunu kullanabilirsiniz.

Yenileme işlemini hemen test etmek için aşağıdaki adımları kullanın.

1. **Update-AutomationRunAsCredential** runbook 'unu düzenleyin ve **Çıkış (1)** komutunun önüne 122 numaralı satıra bir açıklama karakteri (#) koyun.

   ```powershell
   #Exit(1)
   ```

2. Runbook 'u yayımlayın.
3. Runbook 'u başlatın.
4. Aşağıdaki kodla başarılı yenilemeyi doğrulayın:

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```
    Çıktı:

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. Testten sonra, runbook 'u düzenleyin ve adım 1 ' de eklediğiniz açıklama karakterini kaldırın.
6. Runbook 'u yayımlayın.

## <a name="limiting-run-as-account-permissions"></a><a name="limiting-run-as-account-permissions"></a>Farklı Çalıştır hesabı izinlerini sınırlandırma

Azure 'daki kaynaklara karşı Otomasyon hedeflemesini denetlemek için [Update-AutomationRunAsAccountRoleAssignments. ps1](https://aka.ms/AA5hug8) betiğini çalıştırabilirsiniz. Bu betik, özel bir rol tanımı oluşturmak ve kullanmak için mevcut farklı çalıştır hesabı hizmet sorumlunuzu değiştirir. Rolün [Key Vault](https://docs.microsoft.com/azure/key-vault/)hariç tüm kaynaklar için izinleri vardır.

>[!IMPORTANT]
>**Update-AutomationRunAsAccountRoleAssignments. ps1** betiğini çalıştırdıktan sonra, Farklı Çalıştır hesaplarının kullanımı üzerinden Key Vault erişen runbook 'lar artık çalışmaz. Betiği çalıştırmadan önce, Azure Key Vault çağrılar için hesabınızdaki runbook 'ları gözden geçirmeniz gerekir. Azure Otomasyonu runbook 'lardan Key Vault erişimi etkinleştirmek için, [Farklı Çalıştır hesabını Key Vault izinlerine eklemeniz](#add-permissions-to-key-vault)gerekir.

Farklı Çalıştır hizmeti sorumlusunu kısıtlamak istiyorsanız, özel rol tanımının `NotActions` öğesine başka kaynak türleri de ekleyebilirsiniz. Aşağıdaki örnek, erişimini kısıtlar `Microsoft.Compute/*`. Bu kaynak türünü rol tanımı için öğesine `NotActions` eklerseniz, rol herhangi bir işlem kaynağına erişemez. Rol tanımları hakkında daha fazla bilgi edinmek için bkz. [Azure kaynakları için rol tanımlarını anlama](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Farklı Çalıştır hesabınız tarafından kullanılan hizmet sorumlusunun katkıda bulunan rol tanımında mi yoksa özel bir tane mi olduğunu belirleyebilirsiniz. 

1. Otomasyon hesabınıza gidin ve hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.
2. **Azure farklı çalıştır hesabı**' nı seçin. 
3. Kullanılmakta olan rol tanımını bulmak için **rol** ' i seçin.

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

Farklı Çalıştır hesapları tarafından birden çok abonelik veya Otomasyon hesabı için kullanılan rol tanımını da belirleyebilirsiniz. Bunu, PowerShell Galerisi [Check-AutomationRunAsAccountRoleAssignments. ps1](https://aka.ms/AA5hug5) betiğini kullanarak yapın.

### <a name="add-permissions-to-key-vault"></a>Key Vault izinler ekleme

Key Vault ve farklı çalıştır hesabı hizmet sorumlunun özel bir rol tanımı kullandığını doğrulamak için Azure Otomasyonu 'na izin verebilirsiniz. Şunları yapmanız gerekir:

* Key Vault izin verin.
* Erişim ilkesini ayarlayın.

Key Vault için PowerShell Galerisi farklı çalıştır hesabınızın izinlerini sağlamak üzere içindeki [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault. ps1](https://aka.ms/AA5hugb) betiğini kullanabilirsiniz. Key Vault izinlerini ayarlama hakkında daha fazla bilgi için bkz. [uygulamalara bir Anahtar Kasası erişimi verme](../key-vault/general/group-permissions-for-apps.md) .

## <a name="resolving-misconfiguration-issues-for-run-as-accounts"></a>Farklı Çalıştır hesapları için yanlış yapılandırma sorunlarını çözme

Farklı Çalıştır veya klasik farklı çalıştır hesabı için gerekli bazı yapılandırma öğeleri, ilk kurulum sırasında silinmiş veya hatalı oluşturulmuş olabilir. Hatalı yanlış yapılandırma örnekleri şunları içerir:

* Sertifika varlığı
* Bağlantı varlığı
* Farklı Çalıştır hesabı, katkıda bulunan rolünden kaldırıldı
* Azure AD'de hizmet sorumlusu veya uygulama

Bu tür yanlış yapılandırma örnekleri için, Otomasyon hesabı değişiklikleri algılar ve hesap için farklı çalıştır `Incomplete` hesapları Özellikler bölmesinde durumunu görüntüler.

![Tamamlanmamış Farklı Çalıştır hesabı yapılandırma durumu](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Farklı Çalıştır hesabını seçtiğinizde, hesap özellikleri bölmesinde aşağıdaki hata iletisi görüntülenir:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Hesabı silip yeniden oluşturarak Farklı Çalıştır hesabıyla ilgili bu sorunları hızlı bir şekilde çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Hizmet sorumluları hakkında daha fazla bilgi için bkz. [uygulama nesneleri ve hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md).
* Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için bkz. [azure Cloud Services sertifikalara genel bakış](../cloud-services/cloud-services-certs-create.md).
