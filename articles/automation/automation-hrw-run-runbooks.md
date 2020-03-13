---
title: Azure Otomasyonu karma Runbook Worker üzerinde runbook 'ları çalıştırma
description: Bu makale, karma Runbook Worker rolüyle yerel veri merkezinizdeki veya bulut sağlayıcınızdaki makinelerde runbook çalıştırmaya ilişkin bilgiler sağlar.
services: automation
ms.subservice: process-automation
ms.date: 01/29/2019
ms.topic: conceptual
ms.openlocfilehash: c67fff32770446cac3adef8af50c9e5733077bc7
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79278941"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Karma Runbook Worker üzerinde runbook çalıştırma

Karma Runbook Worker 'ı hedefleyen runbook 'lar genellikle yerel bilgisayardaki kaynakları veya çalışanın dağıtıldığı Yerel ortamdaki kaynakları yönetir. Azure Otomasyonu 'ndaki runbook 'lar genellikle Azure bulutundaki kaynakları yönetir. Farklı kullanıldıkları halde, Azure Otomasyonu 'nda çalışan runbook 'lar ve karma Runbook Worker üzerinde çalışan runbook 'lar yapıyla aynıdır.

Bir runbook 'u karma Runbook Worker üzerinde çalıştırmak için yazdığınızda, çalışan makineyi çalıştıran makinede runbook 'u düzenlemeniz ve test etmeniz gerekir. Konak makinede, tüm PowerShell modülleri ve yerel kaynakları yönetmek ve erişmek için gereken ağ erişimi vardır. Runbook 'u karma Runbook Worker makinesinde test edildikten sonra, bu dosyayı çalışan üzerinde çalıştırılabileceği Azure Otomasyonu ortamına yükleyebilirsiniz. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Karma Runbook Worker için Runbook izinleri

Azure olmayan kaynaklara erişirken, karma runbook çalışanı üzerinde çalışan runbook 'lar, genellikle Azure kaynaklarında kimlik doğrulaması yapan runbook 'lar tarafından kullanılan kimlik doğrulama mekanizmasını kullanamaz. Bir runbook yerel kaynaklara kendi kimlik doğrulamasını sağlar veya [Azure kaynakları için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)kullanarak kimlik doğrulamasını yapılandırır. Ayrıca, tüm runbook 'lar için bir kullanıcı bağlamı sağlamak üzere bir farklı çalıştır hesabı belirtebilirsiniz.

### <a name="runbook-authentication"></a>Runbook kimlik doğrulaması

Varsayılan olarak runbook 'lar yerel bilgisayarda çalışır. Windows için yerel sistem hesabı bağlamında çalışır. Linux için, Özel Kullanıcı hesabı **nxautomation**bağlamında çalışır. Her iki senaryoda de runbook 'ların, eriştikleri kaynaklara kendi kimlik doğrulamasını sağlaması gerekir.

Runbook 'larınızdaki [kimlik bilgilerini ve](automation-credentials.md) [sertifika](automation-certificates.md) varlıklarını, runbook 'un farklı kaynaklarda kimlik doğrulaması yapabilmesi için kimlik bilgilerini belirtmenize imkan tanıyan cmdlet 'lerle birlikte kullanabilirsiniz. Aşağıdaki örnek bir runbook 'un bilgisayarı yeniden başlatan bir bölümünü gösterir. Bir kimlik bilgisi kıymetine ait kimlik bilgilerini ve bir değişken varlıkından bilgisayarın adını alır ve ardından bu değerleri **restart-Computer** cmdlet 'i ile kullanır.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

[InlineScript](automation-powershell-workflow.md#inlinescript) etkinliğini de kullanabilirsiniz. InlineScript, [PSCredential ortak parametresiyle](/powershell/module/psworkflow/about/about_workflowcommonparameters)belirtilen kimlik bilgileriyle başka bir bilgisayarda kod blokları çalıştırmanızı sağlar.

### <a name="run-as-account"></a>Farklı Çalıştır hesabı

Runbook 'unuzu yerel kaynaklara kendi kimlik doğrulamasını sağlamak yerine, karma Runbook Worker grubu için bir farklı çalıştır hesabı belirtebilirsiniz. Bunu yapmak için yerel kaynaklara erişimi olan bir [kimlik bilgisi varlığı](automation-credentials.md) tanımlamanız gerekir. Bu kaynaklar, sertifika depolarını ve tüm runbook 'ların gruptaki bir karma runbook çalışanında bu kimlik bilgileri altında çalışan tüm runbook 'ları içerir.

Kimlik bilgisinin Kullanıcı adı aşağıdaki biçimlerden birinde olmalıdır:

* ın
* username@domain
* Kullanıcı adı (Şirket içi bilgisayarda yerel hesaplar için)

Karma Runbook Worker grubu için bir farklı çalıştır hesabı belirtmek için aşağıdaki yordamı kullanın.

1. Yerel kaynaklara erişimi olan bir [kimlik bilgisi varlığı](automation-credentials.md) oluşturun.
2. Azure portal Otomasyon hesabını açın.
3. **Karma çalışanı grupları** kutucuğunu seçin ve ardından grubu seçin.
4. **Tüm ayarlar**' ı ve ardından **karma çalışan grubu ayarları**' nı seçin.
5. **Farklı Çalıştır** değerini **varsayılan** olarak **özel**olarak değiştirin.
6. Kimlik bilgilerini seçin ve **Kaydet**' e tıklayın.

### <a name="managed-identities-for-azure-resources"></a>Azure kaynakları için Yönetilen kimlikler

Azure sanal makinelerinde karma runbook çalışanları, Azure kaynakları için kimlik doğrulaması yapmak üzere Azure kaynakları için Yönetilen kimlikler kullanabilir. Farklı Çalıştır hesapları yerine Azure kaynakları için yönetilen kimliklerin kullanılması, şunları yapmanıza gerek olmadığı için avantajları sağlar:

* Farklı Çalıştır sertifikasını dışarı aktarın ve ardından karma Runbook Worker 'a aktarın
* Farklı Çalıştır hesabı tarafından kullanılan sertifikayı Yenile
* Runbook kodunuzda farklı çalıştır bağlantısı nesnesini işleyin

Karma Runbook Worker üzerinde Azure kaynakları için yönetilen bir kimlik kullanmak üzere sonraki adımları izleyin.

1. Azure VM oluşturma.
2. VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırın. Bkz. [Azure Portal kullanarak BIR VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).
3. Kaynak Yöneticisi içindeki bir kaynak grubuna VM erişimi verin. [Kaynak Yöneticisi erişmek Için WINDOWS VM sistem tarafından atanan yönetilen kimlik kullanma](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)bölümüne bakın.
4. Karma runbook çalışanını VM 'ye yükler. Bkz. [Windows karma Runbook Worker dağıtımı](automation-windows-hrw-install.md).
5. Azure kaynaklarında kimlik doğrulaması yapmak için *kimlik* parametresiyle [Connect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) cmdlet 'ini kullanmak üzere runbook 'u güncelleştirin. Bu yapılandırma, farklı çalıştır hesabı kullanma gereksinimini azaltır ve ilişkili hesap yönetimini gerçekleştirir.

```powershell
    # Connect to Azure using the managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
    Connect-AzAccount -Identity

    # Get all VM names from the subscription
    Get-AzVM | Select Name
```

> [!NOTE]
> `Connect-AzAccount -Identity`, sistem tarafından atanan bir kimlik ve Kullanıcı tarafından atanan tek bir kimlik kullanan karma Runbook Worker için geçerlidir. Karma Runbook Worker üzerinde birden çok kullanıcı tarafından atanan kimlik kullanırsanız, runbook 'lerinizin **Connect-AzAccount** Için *AccountID* parametresini belirtmesi gerekir ve bu kullanıcı tarafından atanan belirli bir kimlik seçin.

### <a name="runas-script"></a>Otomasyon farklı çalıştır hesabı

Azure 'da kaynak dağıtmaya yönelik Otomatikleştirilmiş derleme işleminizin bir parçası olarak, dağıtım dizinizdeki bir görevi veya bir adım kümesini desteklemek için şirket içi sistemlere erişim gerekebilir. Farklı Çalıştır hesabını kullanarak Azure 'da kimlik doğrulaması sağlamak için farklı çalıştır hesabı sertifikasını yüklemelisiniz.

**Export-Runascercertificateatetohybridworker**adlı aşağıdaki PowerShell runbook 'U, Azure Otomasyonu hesabınızdan farklı çalıştır sertifikasını dışarı aktarır. Runbook, sertifikayı indirir ve aynı hesaba bağlı bir karma runbook çalışanındaki yerel makine sertifika deposuna aktarır. Bu adımı tamamladıktan sonra, runbook worker 'ın farklı çalıştır hesabını kullanarak Azure 'da başarıyla kimlik doğrulayabildiğini doğrular.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. Run this runbook on the hybrid worker where you want the certificate installed. This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running on the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test to see if authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzAccount `
    -ServicePrincipal `
    -Tenant $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzContext -Subscription $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm that Azure Resource Manager calls are working
Get-AzAutomationAccount | Select-Object AutomationAccountName
```

>[!NOTE]
>PowerShell runbook 'ları için, **Add-azaccount** ve **Add-AzureRMAccount** , **Connect-azaccount**için diğer adlardır. Kitaplık öğelerinizi ararken, **Connect-AzAccount**' ı görmüyorsanız **Add-azaccount**komutunu kullanabilir veya bir Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

Farklı Çalıştır hesabını hazırlamayı tamamlayacak:

1. **Export-Runascercertificate Meditohybridworker** runbook 'unu bir **. ps1** uzantısıyla bilgisayarınıza kaydedin.
2. Otomasyon hesabınıza aktarın.
3. Runbook 'u düzenleyerek, *parola* değişkeninin değerini kendi parolanınıza göre değiştirin. 
4. Runbook 'u yayımlayın.
5. Çalıştıran karma runbook çalışanı grubunu hedefleyerek runbook 'u çalıştırın ve farklı çalıştır hesabını kullanarak runbook 'ların kimliğini doğrular. 
6. Sertifikayı yerel makine deposuna aktarma girişimini raporluyor ve birden çok satır ile takip eden iş akışını inceleyin. Bu davranış, aboneliğinizde kaç tane Otomasyon hesabı tanımladığınıza ve kimlik doğrulamasının başarı derecesine bağlıdır.

## <a name="job-behavior-on-hybrid-runbook-workers"></a>Karma runbook çalışanları üzerinde iş davranışı

Azure Otomasyonu, karma runbook çalışanları üzerinde işleri, Azure korumalı alanlar 'da çalıştırılan işlerden farklı bir şekilde işler. Bir temel fark, runbook çalışanları üzerinde iş süresi sınırının olmaması olabilir. Azure korumalı alanında çalışan runbook 'lar, [dengeli paylaşımlara](automation-runbook-execution.md#fair-share)göre üç saat ile sınırlıdır.

Uzun süre çalışan bir runbook için, örneğin Worker 'ı barındıran makine yeniden başlatıldıktan sonra, olası yeniden başlatmaya dayanıklı olduğundan emin olmak istersiniz. Karma Runbook Worker konak makinesi yeniden başlatılırsa, çalışan runbook işi başlangıçtan itibaren veya PowerShell Iş akışı runbook 'ları için son denetim noktasından başlatılır. Bir runbook işi üç kez yeniden başlatıldıktan sonra askıya alınır.

Karma runbook çalışanları için işlerin Windows 'daki yerel sistem hesabı veya Linux üzerinde **nxautomation** hesabı altında çalıştırıldığını unutmayın. Linux için, **nxautomation** hesabının, runbook modüllerinin depolandığı konuma erişiminin olduğundan emin olmanız gerekir. [Install-Module](/powershell/module/powershellget/install-module) cmdlet 'ini kullandığınızda, **nxautomation** hesabının erişimi olduğundan emin olmak için, *kapsam* parametresi için **ALLUSERS** ' ı belirttiğinizden emin olun. Linux üzerinde PowerShell hakkında daha fazla bilgi için bkz. [Windows dışı platformlarda PowerShell Için bilinen sorunlar](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms).

## <a name="starting-a-runbook-on-a-hybrid-runbook-worker"></a>Karma runbook çalışanında runbook başlatma

[Azure Otomasyonu 'nda runbook başlatma,](automation-starting-a-runbook.md) runbook başlatmaya yönelik farklı yöntemler açıklar. Karma runbook çalışanındaki bir runbook için başlatma, karma Runbook Worker grubunun adını belirtmenize olanak sağlayan bir **Çalıştır** seçeneği kullanır. Bir grup belirtildiğinde, o gruptaki çalışanlarından biri runbook 'u alır ve çalıştırır. Runbook 'larınız bu seçeneği belirtmezse, Azure Otomasyonu her zamanki gibi runbook 'u çalıştırır.

Azure portal bir runbook 'u başlattığınızda, **Azure** veya **hibrit Worker**' ı seçebileceğiniz **Çalıştır** seçeneği sunulur. **Karma çalışanı**seçerseniz, karma Runbook Worker grubunu bir açılan listeden seçebilirsiniz.

*Runon* parametresini **Start-AzureAutomationRunbook** cmdlet 'i ile birlikte kullanın. Aşağıdaki örnek, MyHybridGroup adlı karma Runbook Worker grubunda **Test-runbook** adlı bir runbook 'u başlatmak Için Windows PowerShell kullanır.

```azurepowershell-interactive
Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> *Runon* parametresi, Microsoft Azure PowerShell sürüm 0.9.1 ' de **Start-AzureAutomationRunbook** öğesine eklendi. Daha önce yüklenmiş bir [sürüm varsa en son sürümü indirmeniz](https://azure.microsoft.com/downloads/) gerekir. Bu sürümü yalnızca runbook 'u PowerShell 'den başlatduyduğunuz iş istasyonuna yükler. Runbook 'ları bu bilgisayardan başlatmayı düşünmüyorsanız karma Runbook Worker bilgisayarına yüklemeniz gerekmez.

## <a name="working-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>Windows karma Runbook Worker 'da imzalı runbook 'larla çalışma

Windows hibrit Runbook Worker 'ı yalnızca imzalı runbook 'ları çalıştıracak şekilde yapılandırabilirsiniz.

> [!IMPORTANT]
> Karma Runbook Worker 'ı yalnızca imzalı runbook 'ları çalıştıracak şekilde yapılandırdıktan sonra, imzalanmamış runbook 'lar çalışan üzerinde yürütülemeyebilir.

### <a name="create-signing-certificate"></a>Imza sertifikası oluştur

Aşağıdaki örnek, runbook 'ları imzalamak için kullanılabilen, kendinden imzalı bir sertifika oluşturur. Bu kod sertifikayı oluşturur ve karma Runbook Worker 'ın daha sonra içeri aktarabilmesi için sertifikayı dışa aktarır. Parmak izi Ayrıca, daha sonra sertifikaya başvuruda bulunarak kullanılmak üzere döndürülür.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="import-certificate-and-configure-workers-for-signature-validation"></a>İmza doğrulama için sertifikayı içeri aktarma ve çalışanları yapılandırma

Oluşturduğunuz sertifikayı bir gruptaki her karma runbook çalışanına kopyalayın. Sertifikayı içeri aktarmak ve çalışanları runbook 'larda imza doğrulamasını kullanacak şekilde yapılandırmak için aşağıdaki betiği çalıştırın.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Sertifikayı kullanarak runbook 'larınızı imzalama

Yalnızca imzalı runbook 'ları kullanacak şekilde yapılandırılmış karma runbook çalışanları sayesinde, karma Runbook Worker 'da kullanılacak runbook 'ları imzalamanız gerekir. Bu runbook 'ları imzalamak için aşağıdaki örnek PowerShell kodunu kullanın.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Bir runbook imzalanmışsa Otomasyon hesabınıza içeri aktarmanız ve imza bloğundan yayımlamanız gerekir. Runbook 'ların nasıl içeri aktarılacağını öğrenmek için bkz. bir [dosyadaki runbook 'U Azure Otomasyonu 'Na aktarma](manage-runbooks.md#import-a-runbook).

## <a name="working-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>Linux karma Runbook Worker 'da imzalı runbook 'larla çalışma

İmzalı runbook 'larla çalışabilmeniz için, bir Linux karma Runbook Worker 'ın yerel makinede [GPG](https://gnupg.org/index.html) yürütülebilir dosyası olması gerekir.

> [!IMPORTANT]
> Karma Runbook Worker 'ı yalnızca imzalı runbook 'ları çalıştıracak şekilde yapılandırdıktan sonra, imzalanmamış runbook 'lar çalışan üzerinde yürütülemeyebilir.

### <a name="create-a-gpg-keyring-and-keypair"></a>GPG kimlik anahtarlığı ve KeyPair oluşturma

GPG kimlik anahtarlığı ve KeyPair oluşturmak için karma Runbook Worker **nxautomation** hesabını kullanın.

1. Sudo uygulamasını kullanarak **nxautomation** hesabı olarak oturum açın.

    ```bash
    sudo su – nxautomation
    ```

2. **Nxautomation**'ı kullanırken GPG KeyPair oluşturun. GPG, adımlarda size rehberlik eder. Ad, e-posta adresi, sona erme saati ve parola sağlamanız gerekir. Daha sonra, makinenin oluşturulması için makinede yeterli entropi olana kadar bekler.

    ```bash
    sudo gpg --generate-key
    ```

3. GPG dizini sudo ile oluşturulduğundan, aşağıdaki komutu kullanarak sahibini **nxautomation** olarak değiştirmeniz gerekir.

    ```bash
    sudo chown -R nxautomation ~/.gnupg
    ```

### <a name="make-the-keyring-available-to-the-hybrid-runbook-worker"></a>Kimlik anahtarlığı karma Runbook Worker için kullanılabilir hale getirme

Kimlik anahtarlığı oluşturulduktan sonra karma Runbook Worker için kullanılabilir hale getirin. Dosya bölümü altına aşağıdaki örnek kodu eklemek için `/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf` ayarlar dosyasını değiştirin **[Worker-optional]** .

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>İmza doğrulamasının açık olduğunu doğrulama

Makinede imza doğrulaması devre dışıysa, aşağıdaki sudo komutunu çalıştırarak açmanız gerekir. `<LogAnalyticsworkspaceId>`, çalışma alanınızın KIMLIĞI ile değiştirin.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>Runbook 'u imzala

İmza doğrulamayı yapılandırdıktan sonra, bir runbook 'u imzalamak için aşağıdaki GPG komutunu kullanın.

```bash
gpg –-clear-sign <runbook name>
```

İmzalı runbook `<runbook name>.asc`olarak adlandırılır.

Artık imzalı runbook 'u Azure Otomasyonu 'na yükleyebilir ve normal bir runbook gibi yürütebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'u başlatma yöntemleri hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'Nda runbook başlatma](automation-starting-a-runbook.md).
* Azure Automation 'da PowerShell runbook 'ları ile çalışmak için metin düzenleyicisinin nasıl kullanılacağını anlamak için bkz. [Azure Otomasyonu 'Nda runbook 'U düzenleme](automation-edit-textual-runbook.md).
* Runbook 'larınız başarıyla tamamlanmadığından, [runbook yürütme hatalarıyla](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails)ilgili sorun giderme kılavuzunu gözden geçirin.
* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/overview)bakın.
