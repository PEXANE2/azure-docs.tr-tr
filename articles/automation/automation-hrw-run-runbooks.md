---
title: Karma Runbook Worker üzerinde Azure Otomasyonu runbook 'ları çalıştırma
description: Bu makalede, karma Runbook Worker ile yerel veri merkezinizdeki veya bulut sağlayıcınızdaki makinelerde runbook 'ların nasıl çalıştırılacağı açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 01/29/2019
ms.topic: conceptual
ms.openlocfilehash: 8ea32b2e393a13f1725ff7a83f4b4f2191b59ddb
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835317"
---
# <a name="run-runbooks-on-a-hybrid-runbook-worker"></a>Karma Runbook Çalışanı üzerinde runbook çalıştırma

[Karma runbook çalışanı](automation-hybrid-runbook-worker.md) üzerinde çalışan runbook 'lar, genellikle yerel bilgisayardaki kaynakları veya çalışanın dağıtıldığı Yerel ortamdaki kaynakları yönetir. Azure Otomasyonu 'ndaki runbook 'lar genellikle Azure bulutundaki kaynakları yönetir. Farklı kullanıldıkları halde, Azure Otomasyonu 'nda çalışan runbook 'lar ve karma Runbook Worker üzerinde çalışan runbook 'lar yapıyla aynıdır.

Bir runbook 'u karma Runbook Worker üzerinde çalıştırmak için yazdığınızda, çalışan makineyi çalıştıran makinede runbook 'u düzenlemeniz ve test etmeniz gerekir. Konak makinede tüm PowerShell modülleri ve yerel kaynakları yönetmek için gereken ağ erişimi vardır. Runbook 'u karma Runbook Worker makinesinde test edildikten sonra, bu dosyayı çalışan üzerinde çalıştırılabileceği Azure Otomasyonu ortamına yükleyebilirsiniz. 

## <a name="plan-runbook-job-behavior"></a>Runbook iş davranışını planlayın

Azure Otomasyonu, karma runbook çalışanları üzerinde işleri, Azure korumalı alanlar 'da çalıştırılan işlerden farklı bir şekilde işler. Uzun süre çalışan bir runbook 'a sahipseniz, olası yeniden başlatmanın dayanıklı olduğundan emin olun. İş davranışının ayrıntıları için bkz. [karma Runbook Worker işleri](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-jobs).

Karma runbook çalışanları için işlerin Windows 'daki yerel **sistem** hesabı veya Linux üzerinde **nxautomation** hesabı altında çalıştırıldığını unutmayın. Linux için, **nxautomation** hesabının, runbook modüllerinin depolandığı konuma erişimi olduğundan emin olun. [Install-Module](/powershell/module/powershellget/install-module) cmdlet 'ini kullandığınızda, `Scope` **nxautomation** hesabının erişimi olduğundan emin olmak için parametresi için ALLUSERS ' ı belirttiğinizden emin olun. Linux üzerinde PowerShell hakkında daha fazla bilgi için bkz. [Windows dışı platformlarda PowerShell Için bilinen sorunlar](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms).

## <a name="set-up-runbook-permissions"></a>Runbook izinlerini ayarlama

Runbook 'larınızın karma Runbook Worker üzerinde çalışması için izinleri aşağıdaki yollarla tanımlayın:

* Runbook 'un yerel kaynaklara kendi kimlik doğrulamasını sağlaması gerekir.
* [Azure kaynakları için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)kullanarak kimlik doğrulamasını yapılandırın. 
* Tüm runbook 'lar için bir kullanıcı bağlamı sağlamak üzere bir farklı çalıştır hesabı belirtin.

## <a name="use-runbook-authentication-to-local-resources"></a>Yerel kaynaklara runbook kimlik doğrulaması kullanma

Kaynaklara kendi kimlik doğrulamasını sağlayan bir runbook hazırlanıyorsanız, runbook 'larınızda [kimlik bilgileri](automation-credentials.md) ve [sertifika](automation-certificates.md) varlıkları kullanın. Runbook 'un farklı kaynaklarda kimlik doğrulaması yapabilmesi için kimlik bilgilerini belirtmenizi sağlayan çeşitli cmdlet 'ler vardır. Aşağıdaki örnek bir runbook 'un bilgisayarı yeniden başlatan bir bölümünü gösterir. Bir kimlik bilgisi varlığından kimlik bilgilerini ve bir değişken varlıkından bilgisayarın adını alır ve cmdlet ile bu değerleri kullanır `Restart-Computer` .

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

[InlineScript](automation-powershell-workflow.md#use-inlinescript) etkinliğini de kullanabilirsiniz. `InlineScript`kimlik bilgileriyle başka bir bilgisayarda kod blokları çalıştırmanızı sağlar.

## <a name="use-runbook-authentication-with-managed-identities"></a><a name="runbook-auth-managed-identities"></a>Yönetilen kimliklerle runbook kimlik doğrulamasını kullanma

Azure sanal makinelerinde karma runbook çalışanları, Azure kaynaklarında kimlik doğrulaması yapmak için Yönetilen kimlikler kullanabilir. Farklı Çalıştır hesapları yerine Azure kaynakları için yönetilen kimliklerin kullanılması, şunları yapmanıza gerek olmadığı için avantajları sağlar:

* Farklı Çalıştır sertifikasını dışarı aktarın ve ardından karma Runbook Worker 'a aktarın.
* Farklı Çalıştır hesabı tarafından kullanılan sertifikayı yenileyin.
* Runbook kodunuzda farklı çalıştır bağlantısı nesnesini işleyin.

Karma runbook çalışanında Azure kaynakları için yönetilen bir kimlik kullanmak üzere sonraki adımları izleyin:

1. Azure VM oluşturma.
2. VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırın. Bkz. [Azure Portal kullanarak BIR VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).
3. Kaynak Yöneticisi içindeki bir kaynak grubuna VM erişimi verin. [Kaynak Yöneticisi erişmek Için WINDOWS VM sistem tarafından atanan yönetilen kimlik kullanma](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)bölümüne bakın.
4. Karma runbook çalışanını VM 'ye yükler. Bkz. [Windows karma Runbook Worker dağıtma](automation-windows-hrw-install.md) veya [Linux karma Runbook Worker dağıtma](automation-linux-hrw-install.md).
5. Runbook 'u, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) cmdlet 'ini `Identity` Azure kaynaklarında kimlik doğrulaması için parametresiyle birlikte kullanacak şekilde güncelleştirin. Bu yapılandırma, farklı çalıştır hesabı kullanma gereksinimini azaltır ve ilişkili hesap yönetimini gerçekleştirir.

    ```powershell
    # Connect to Azure using the managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
    Connect-AzAccount -Identity

    # Get all VM names from the subscription
    Get-AzVM | Select Name
    ```

    > [!NOTE]
    > `Connect-AzAccount -Identity`sistem tarafından atanan kimlik ve Kullanıcı tarafından atanan tek bir kimlik kullanılarak karma Runbook Worker için geçerlidir. Karma Runbook Worker üzerinde birden çok kullanıcı tarafından atanan kimlik kullanırsanız, runbook 'inizdeki `AccountId` `Connect-AzAccount` belirli bir kullanıcı tarafından atanan kimlik seçmek için parametresi belirtilmelidir.

## <a name="use-runbook-authentication-with-run-as-account"></a>Farklı Çalıştır hesabıyla runbook kimlik doğrulamasını kullanma

Runbook 'unuzu yerel kaynaklara kendi kimlik doğrulamasını sağlamak yerine, karma Runbook Worker grubu için bir farklı çalıştır hesabı belirtebilirsiniz. Bunu yapmak için yerel kaynaklara erişimi olan bir [kimlik bilgisi varlığı](automation-credentials.md) tanımlamanız gerekir. Bu kaynaklar, sertifika depolarını ve tüm runbook 'ların gruptaki bir karma runbook çalışanında bu kimlik bilgileri altında çalışan tüm runbook 'ları içerir.

Kimlik bilgisinin Kullanıcı adı aşağıdaki biçimlerden birinde olmalıdır:

* ın
* username@domain
* Kullanıcı adı (Şirket içi bilgisayarda yerel hesaplar için)

Karma Runbook Worker grubu için bir farklı çalıştır hesabı belirtmek için aşağıdaki yordamı kullanın:

1. Yerel kaynaklara erişimi olan bir [kimlik bilgisi varlığı](automation-credentials.md) oluşturun.
2. Azure portal Otomasyon hesabını açın.
3. **Karma çalışanı grupları**' nı seçin ve ardından belirli grubu seçin.
4. **Tüm ayarlar**' ı ve ardından **karma çalışan grubu ayarları**' nı seçin.
5. **Farklı Çalıştır** değerini **varsayılan** olarak **özel**olarak değiştirin.
6. Kimlik bilgilerini seçin ve **Kaydet**' e tıklayın.

## <a name="install-run-as-account-certificate"></a><a name="runas-script"></a>Farklı Çalıştır hesabı sertifikası 'nı yükler

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
>PowerShell runbook 'ları için `Add-AzAccount` ve `Add-AzureRMAccount` diğer adlardır `Connect-AzAccount` . Kitaplık öğelerinizi ararken, ' yi `Connect-AzAccount` kullanabilir `Add-AzAccount` veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

Farklı Çalıştır hesabını hazırlamayı tamamlayacak:

1. **Export-Runascercertificate Meditohybridworker** runbook 'unu bir **. ps1** uzantısıyla bilgisayarınıza kaydedin.
2. Otomasyon hesabınıza aktarın.
3. Değişkenin değerini kendi parolanızla değiştirerek runbook 'u düzenleyin `Password` . 
4. Runbook 'u yayımlayın.
5. Çalıştıran karma runbook çalışanı grubunu hedefleyerek runbook 'u çalıştırın ve farklı çalıştır hesabını kullanarak runbook 'ların kimliğini doğrular. 
6. Sertifikayı yerel makine deposuna aktarma girişimini rapor etmek ve ardından birden çok satır izlemek için iş akışını inceleyin. Bu davranış, aboneliğinizde kaç tane Otomasyon hesabı tanımladığınıza ve kimlik doğrulamasının başarı derecesine bağlıdır.

## <a name="work-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>Windows karma Runbook Worker 'da imzalı runbook 'larla çalışma

Windows hibrit Runbook Worker 'ı yalnızca imzalı runbook 'ları çalıştıracak şekilde yapılandırabilirsiniz. 

> [!IMPORTANT]
> Karma Runbook Worker 'ı yalnızca imzalı runbook 'ları çalıştıracak şekilde yapılandırdıktan sonra, imzasız runbook 'lar çalışan üzerinde çalıştırılamaz.

### <a name="create-signing-certificate"></a>İmza sertifikası oluştur

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

Bir runbook imzalanmışsa Otomasyon hesabınıza içeri aktarmanız ve imza bloğundan yayımlamanız gerekir. Runbook 'ların nasıl içeri aktarılacağını öğrenmek için bkz. [runbook 'U Içeri aktarma](manage-runbooks.md#import-a-runbook).

## <a name="work-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>Linux karma Runbook Worker 'da imzalı runbook 'larla çalışma

İmzalı runbook 'larla çalışabilmeniz için, bir Linux karma Runbook Worker 'ın yerel makinede [GPG](https://gnupg.org/index.html) yürütülebilir dosyası olması gerekir.

> [!IMPORTANT]
> Karma Runbook Worker 'ı yalnızca imzalı runbook 'ları çalıştıracak şekilde yapılandırdıktan sonra, imzasız runbook 'lar çalışan üzerinde çalıştırılamaz.

### <a name="create-a-gpg-keyring-and-keypair"></a>GPG kimlik anahtarlığı ve KeyPair oluşturma

GPG kimlik anahtarlığı ve KeyPair oluşturmak için karma Runbook Worker [nxautomation hesabını](automation-runbook-execution.md#log-analytics-agent-for-linux)kullanın.

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

Kimlik anahtarlığı oluşturulduktan sonra karma Runbook Worker için kullanılabilir hale getirin. Dosya bölümüne aşağıdaki örnek kodu eklemek için **/var/seçenek/Microsoft/omsagent/State/automationworker/diyı/Worker.exe** adlı ayarlar dosyasını değiştirin `[worker-optional]` .

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>İmza doğrulamasının açık olduğunu doğrulama

Makinede imza doğrulaması devre dışıysa, aşağıdaki sudo komutunu çalıştırarak açmanız gerekir. `<LogAnalyticsworkspaceId>`Çalışma ALANıNıZıN kimliği ile değiştirin.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>Runbook 'u imzala

İmza doğrulamayı yapılandırdıktan sonra, runbook 'u imzalamak için aşağıdaki GPG komutunu kullanın.

```bash
gpg –-clear-sign <runbook name>
```

İmzalı runbook ** <runbook name> . asc**olarak adlandırılır.

Artık imzalı runbook 'u Azure Otomasyonu 'na yükleyebilir ve normal bir runbook gibi yürütebilirsiniz.

## <a name="start-a-runbook-on-a-hybrid-runbook-worker"></a>Karma runbook çalışanında runbook başlatma

[Azure Otomasyonu 'nda runbook başlatma](start-runbooks.md) runbook başlatma için farklı Yöntemler tanımlar. Karma runbook çalışanındaki bir runbook için başlatma, karma Runbook Worker grubunun adını belirtmenize olanak sağlayan bir **Çalıştır** seçeneği kullanır. Bir grup belirtildiğinde, o gruptaki çalışanlarından biri runbook 'u alır ve çalıştırır. Runbook 'larınız bu seçeneği belirtmezse, Azure Otomasyonu her zamanki gibi runbook 'u çalıştırır.

Azure portal bir runbook 'u başlattığınızda, **Azure** veya **hibrit Worker**' ı seçebileceğiniz **Çalıştır** seçeneği sunulur. **Karma çalışanı**seçerseniz, karma Runbook Worker grubunu bir açılan listeden seçebilirsiniz.

PowerShell kullanarak bir runbook 'u başlatırken, `RunOn` parametresini [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationRunbook?view=azps-3.7.0) cmdlet 'i ile birlikte kullanın. Aşağıdaki örnek, MyHybridGroup adlı karma Runbook Worker grubunda **Test-runbook** adlı bir runbook 'u başlatmak Için Windows PowerShell kullanır.

```azurepowershell-interactive
Start-AzAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınız başarıyla tamamlanmadığından, [runbook yürütme hatalarıyla](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails)ilgili sorun giderme kılavuzunu gözden geçirin.
* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/overview)bakın.
* PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation).
