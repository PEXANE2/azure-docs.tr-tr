---
title: Azure Automation Karma Runbook Worker'da runbook'lar çalıştırın
description: Bu makalede, Karma Runbook Worker rolüyle yerel veri merkezinizdeki veya bulut sağlayıcınızdaki makinelerde runbook'ların çalıştırılma hakkında bilgi verilmektedir.
services: automation
ms.subservice: process-automation
ms.date: 01/29/2019
ms.topic: conceptual
ms.openlocfilehash: b65c72e0c65cf9aa84cb614478fbdf78258f3054
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405821"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Karma Runbook Worker'da runbook çalıştırma

Karma Runbook İşçisini hedefleyen runbook'lar genellikle yerel bilgisayardaki kaynakları veya çalışanın dağıtıldığı yerel ortamdaki kaynaklara karşı yönetir. Azure Otomasyonundaki runbook'lar genellikle Azure bulutundaki kaynakları yönetir. Farklı kullanılsalar da, Azure Otomasyonu'nda çalışan runbook'lar ve Karma Runbook Worker'da çalışan runbook'lar yapı olarak aynıdır.

Bir Karma Runbook Worker'da çalışacak bir runbook yazarken, çalışma kitabını işçiyi barındıran makinede ayarlamalı ve test etmeniz gerekir. Ana bilgisayar makinesi, yerel kaynakları yönetmek ve bunlara erişmek için gereken tüm PowerShell modüllerine ve ağ erişimine sahiptir. Runbook'u Karma Runbook İşçisi makinesinde test ettikten sonra, bu kitabı çalışan üzerinde çalıştırılabildiği Azure Otomasyon ortamına yükleyebilirsiniz. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Karma Runbook Çalışanı için Runbook izinleri

Azure olmayan kaynaklara erişirken, Karma Runbook Çalışanı üzerinde çalışan runbook'lar, Azure kaynaklarına kimlik doğrulaması veren runbook'lar tarafından genellikle kullanılan kimlik doğrulama mekanizmasını kullanamaz. Runbook, yerel kaynaklara kendi kimlik doğrulamasını sağlar veya [Azure kaynakları için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)kullanarak kimlik doğrulamasını yapılandırır. Ayrıca, tüm runbook'lar için kullanıcı bağlamı sağlamak için Bir Run As hesabı da belirtebilirsiniz.

### <a name="runbook-authentication"></a>Runbook kimlik doğrulaması

Varsayılan olarak, runbook'lar yerel bilgisayarda çalıştırın. Windows için, yerel **Sistem** hesabı bağlamında çalışırlar. Linux için, onlar özel kullanıcı hesabı **nxautomation**bağlamında çalıştırın. Her iki senaryoda da, runbook'ların eriştükleri kaynaklara kendi kimlik doğrulamasını sağlaması gerekir.

Çalışma kitabınızda [Kimlik Bilgileri](automation-credentials.md) ve [Sertifika](automation-certificates.md) varlıklarını, çalışma kitabının farklı kaynaklara kimlik doğrulaması yapabilmesi için kimlik bilgilerini belirtmenize olanak tanıyan cmdletlerle kullanabilirsiniz. Aşağıdaki örnek, bir bilgisayarı yeniden başlatan bir runbook bölümünü gösterir. Bir kimlik bilgisi varlığından kimlik bilgilerini ve bilgisayarın adını değişken bir varlıktan alır `Restart-Computer` ve bu değerleri cmdlet ile kullanır.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Bir [InlineScript](automation-powershell-workflow.md#inlinescript) etkinliği de kullanabilirsiniz. `InlineScript`[PSCredential ortak parametre](/powershell/module/psworkflow/about/about_workflowcommonparameters)tarafından belirtilen kimlik bilgileri ile başka bir bilgisayarda kod blokları çalıştırmanızı sağlar.

### <a name="run-as-account"></a>Farklı Çalıştır hesabı

Runbook'unuzun yerel kaynaklara kendi kimlik doğrulamasını sağlamasını sağlamak yerine, Karma Runbook İşçi grubu için Bir Run As hesabı belirtebilirsiniz. Bunu yapmak için, yerel kaynaklara erişimi olan bir [kimlik bilgisi varlık](automation-credentials.md) tanımlamanız gerekir. Bu kaynaklar, gruptaki bir Karma Runbook Çalışanı'nda sertifika depolarını ve bu kimlik bilgileri altında çalışan tüm runbook'ları içerir.

Kimlik bilgisinin kullanıcı adı aşağıdaki biçimlerden birinde olmalıdır:

* etki alanı\kullanıcı adı
* username@domain
* kullanıcı adı (şirket içi bilgisayara yerel hesaplar için)

Karma Runbook Worker grubu için Bir Run As hesabı belirtmek için aşağıdaki yordamı kullanın.

1. Yerel [kaynaklara](automation-credentials.md) erişimi olan bir kimlik bilgisi varlığı oluşturun.
2. Azure portalında Otomasyon hesabını açın.
3. Karma **İşçi Grupları** döşemesini seçin ve ardından grubu seçin.
4. **Tüm ayarları**seçin, ardından Karma alt **grup ayarlarını.**
5. **Varsayılan** Dan **Özel** **olarak Çalıştır** değerini değiştirin.
6. Kimlik bilgisini seçin ve **Kaydet'i**tıklatın.

### <a name="managed-identities-for-azure-resources"></a><a name="managed-identities-for-azure-resources"></a>Azure Kaynakları için Yönetilen Kimlikler

Azure sanal makinelerdeki Karma Runbook Çalışanları, Azure kaynaklarında kimlik doğrulama yapmak için yönetilen kimlikleri kullanabilir. Çalıştır hesapları yerine Azure kaynakları için yönetilen kimlikleri kullanmak, şunları yapmanız gerekmedığından avantajlar sağlar:

* Çalıştır Sertifikasını dışa aktarın ve ardından Karma Runbook Worker'a aktarın.
* Run As hesabı tarafından kullanılan sertifikayı yenileyin.
* Run The Run Bağlantı Nesnesi'ni runbook kodunuzda işleyebilir.

Karma Runbook Çalışanı'nda Azure kaynakları için yönetilen bir kimlik kullanmak için sonraki adımları izleyin.

1. Azure VM oluşturun.
2. VM'deki Azure kaynakları için yönetilen kimlikleri yapılandırın. Bkz. [Azure portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma.](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm)
3. Kaynak Yöneticisi'ndeki bir kaynak grubuna VM erişimi verin. Kaynak [Yöneticisi'ne erişmek için Windows VM sistemine atanmış yönetilen kimlik kullanma'ya](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)bakın.
4. Hibrit Runbook çalışanını VM'ye yükleyin. Bkz. [Bir Windows Karma Runbook Worker dağıtın.](automation-windows-hrw-install.md)
5. Azure kaynaklarına kimlik doğrulamak için `Identity` parametreyle [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) cmdlet'i kullanmak için runbook'u güncelleştirin. Bu yapılandırma, Bir Run As hesabı nı kullanma ve ilişkili hesap yönetimini gerçekleştirme gereksinimini azaltır.

```powershell
    # Connect to Azure using the managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
    Connect-AzAccount -Identity

    # Get all VM names from the subscription
    Get-AzVM | Select Name
```

> [!NOTE]
> `Connect-AzAccount -Identity`Sistem tarafından atanan bir kimlik ve kullanıcı tarafından atanmış tek bir kimlik kullanarak Bir Karma Runbook Çalışanı için çalışır. Karma Runbook Worker'da birden çok kullanıcı tarafından atanmış kimlik kullanıyorsanız, `Connect-AzAccount` runbook'unuzun belirli bir kullanıcı tarafından atanmış bir kimliği seçmek için `AccountId` parametreyi belirtmesi gerekir.

### <a name="automation-run-as-account"></a><a name="runas-script"></a>Otomasyon Hesabı Olarak Çalıştır

Azure'da kaynak dağıtmak için otomatik yapı işleminizin bir parçası olarak, bir görevi veya dağıtım sıranızdaki adımlar kümesini desteklemek için şirket içi sistemlere erişmeniz gerekebilir. Çalıştır'ın Hesabı'nı kullanarak Azure'a karşı kimlik doğrulaması sağlamak için, Hesap Olarak Çalıştır sertifikasını yüklemeniz gerekir.

**Export-RunAsCertificateToHybridWorker**adı verilen aşağıdaki PowerShell runbook, Azure Otomasyon hesabınızdan Çalıştır sertifikasını dışa aktarın. Runbook, sertifikayı aynı hesaba bağlı karma runbook çalışanı üzerinde yerel makine sertifikası deposuna indirir ve içeri yetirer. Runbook, bu adımı tamamladıktan sonra, çalıştır'ın Çalıştır hesabını kullanarak azure'a başarılı bir şekilde doğrulanabileceğini doğrular.

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
>PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` `Connect-AzAccount`diğer adlar . Kitaplık öğelerinizde arama yaparken, `Connect-AzAccount`göremiyorsanız, `Add-AzAccount`modüllerinizi kullanabilir veya Modüllerinizi Otomasyon hesabınızda güncelleştirebilirsiniz.

Run As hesabını hazırlamayı bitirmek için:

1. **Dışa Aktar-RunAsCertificateToHybridWorker** runbook'u **.ps1** uzantılı bilgisayarınıza kaydedin.
2. Otomasyon hesabınıza aktarın.
3. `Password` Değişkenin değerini kendi parolanızla değiştirerek runbook'u edin. 
4. Runbook'u yayımlayın.
5. Run As hesabını kullanarak runbook'u çalıştıran ve doğrulayan Karma Runbook İşçi grubunu hedefleyen runbook'u çalıştırın. 
6. Sertifikayı yerel makine deposuna alma girişimini bildirdiğini ve birden çok satırla izlediğini görmek için iş akışını inceleyin. Bu davranış, aboneliğinizde kaç Otomasyon hesabı tanımladığınıza ve kimlik doğrulamanın başarı derecesine bağlıdır.

## <a name="job-behavior-on-hybrid-runbook-workers"></a>Hibrit Runbook İşçilerinde İş Davranışı

Azure Otomasyonu, Karma Runbook Çalışanları'ndaki işleri Azure ve Zum Kutuları'nda çalıştırılandan biraz farklı işlerişler. Önemli bir fark, runbook işçilerin iş süresi sınırı olmamasıdır. Azure ve Azure'da çalıştırılabildiği runbook'lar, [adil paylaşım](automation-runbook-execution.md#fair-share)nedeniyle üç saatle sınırlıdır.

Uzun soluklu bir runbook için, örneğin, işçiyi barındıran makine yeniden başlatıyorsa, olası yeniden başlatmaya karşı dayanıklı olduğundan emin olmak istersiniz. Karma Runbook Worker ana bilgisayar makinesi yeniden başlatılırsa, herhangi bir runbook işi başlangıçtan veya PowerShell İş Akışı runbook'ları için son denetim noktasından yeniden başlatılır. Bir runbook işi üç defadan fazla yeniden başlatıldıktan sonra askıya alınır.

Hibrit Runbook Çalışanları için işlerin Windows'daki yerel Sistem hesabı veya Linux'taki **nxautomation** hesabı altında çalıştırıldığını unutmayın. Linux için, **nxautomation** hesabının runbook modüllerinin depolandığı konuma erişebilmesini sağlamalısınız. [Install-Module](/powershell/module/powershellget/install-module) cmdlet'i kullandığınızda, `Scope` **nxautomation** hesabının erişimini sağlamak için parametre için AllUsers'ı belirttiğinden emin olun. Linux'ta PowerShell hakkında daha fazla bilgi için [Windows Dışı Platformlarda PowerShell için Bilinen Sorunlar'a](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms)bakın.

## <a name="starting-a-runbook-on-a-hybrid-runbook-worker"></a>Hibrit Runbook Worker'da runbook başlatma

[Azure Otomasyonu'nda bir runbook başlatma,](automation-starting-a-runbook.md) bir runbook başlatmak için farklı yöntemleri açıklar. Karma Runbook Worker'daki bir runbook için başlatma, Karma Runbook Worker grubunun adını belirtmenize olanak tanıyan bir **Run on** seçeneğini kullanır. Bir grup belirtildiğinde, bu gruptaki işçilerden biri çalışma defterini alır ve çalıştırılır. Runbook'unuzun bu seçeneği belirtmesi durumunda, Azure Otomasyon runbook'u her zamanki gibi çalıştırAr.

Azure portalında bir runbook başlattığınızda, **Azure** veya **Karma Çalışan'ı**seçebileceğiniz **Çalıştır** seçeneği sunulur. **Karma İşçi'yi**seçerseniz, açılan bir açılır dosyadan Karma Runbook Worker grubunu seçebilirsiniz.

`RunOn` [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationRunbook?view=azps-3.7.0) cmdlet ile parametreyi kullanın. Aşağıdaki örnek, MyHybridGroup adlı Karma Runbook Worker grubunda **Test-Runbook** adlı bir runbook başlatmak için Windows PowerShell kullanır.

```azurepowershell-interactive
Start-AzAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> Daha önce bir tane yüklüyseniz [en son PowerShell sürümünü indirmelisiniz.](https://azure.microsoft.com/downloads/) Bu sürümü yalnızca PowerShell'den runbook'u başlattığınız iş istasyonuna yükleyin. Bu bilgisayardan runbook'ları başlatmayı düşünmüyorsanız, bu bilgisayarı Karma Runbook Worker bilgisayarına yüklemeniz gerekmez.

## <a name="working-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>Windows Karma Runbook Worker üzerinde imzalı runbook'larla çalışma

Bir Windows Karma Runbook Worker'ı yalnızca imzalı runbook'ları çalıştıracak şekilde yapılandırabilirsiniz.

> [!IMPORTANT]
> Bir Karma Runbook İşçisini yalnızca imzalı runbook'ları çalıştıracak şekilde yapılandırdıktan sonra, imzalanmış runbook'lar çalışan üzerinde yürütülmez.

### <a name="create-signing-certificate"></a>İmza Sertifikası Oluşturma

Aşağıdaki örnek, runbook'ları imzalamak için kullanılabilecek kendi imzalı bir sertifika oluşturur. Bu kod sertifikayı oluşturur ve Karma Runbook Worker'ın daha sonra içe aktarabilmesi için sertifikayı dışa aktarAbilir. Parmak izi, sertifikaya başvuruda daha sonra kullanılmak üzere de döndürülür.

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

### <a name="import-certificate-and-configure-workers-for-signature-validation"></a>İmza doğrulama için sertifika alma ve yapılandırma işçileri

Oluşturduğunuz sertifikayı bir gruptaki her Karma Runbook Çalışanı için kopyalayın. Sertifikayı almak ve çalışanları runbook'larda imza doğrulaması kullanacak şekilde yapılandırmak için aşağıdaki komut dosyasını çalıştırın.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Sertifikayı kullanarak runbook'larınızı imzalayın

Yalnızca imzalı runbook'ları kullanacak şekilde yapılandırılan Karma Runbook Çalışanları ile, Karma Runbook Worker'da kullanılacak runbook'ları imzalamanız gerekir. Bu runbook'ları imzalamak için aşağıdaki örnek PowerShell kodunu kullanın.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Bir runbook imzalandığında, bu kitabı Otomasyon hesabınıza aktarmanız ve imza bloğuyla yayımlamanız gerekir. Runbook'ları nasıl içe aktarırılabildiğini öğrenmek için [dosyadan Azure Otomasyonuna runbook alma'ya](manage-runbooks.md#importing-a-runbook)bakın.

## <a name="working-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>Linux Hybrid Runbook Worker üzerinde imzalı runbook'larla çalışma

İmzalı runbook'larla çalışabilmek için, Bir Linux Hybrid Runbook Worker'ın [GPG'yi](https://gnupg.org/index.html) yerel makinede çalıştırılabilir hale getirmek gerekir.

> [!IMPORTANT]
> Bir Karma Runbook İşçisini yalnızca imzalı runbook'ları çalıştıracak şekilde yapılandırdıktan sonra, imzalanmış runbook'lar çalışan üzerinde yürütülmez.

### <a name="create-a-gpg-keyring-and-keypair"></a>GPG anahtarlık ve anahtar çifti oluşturma

GPG anahtarlığı ve anahtar çifti oluşturmak için Hybrid Runbook Worker **nxautomation** hesabını kullanın.

1. **Nxautomation** hesabı olarak oturum açabilmek için sudo uygulamasını kullanın.

    ```bash
    sudo su – nxautomation
    ```

2. **Nxautomation'ı**kullandıktan sonra GPG tuş çiftini oluşturun. GPG adımlar boyunca size rehberlik eder. Ad, e-posta adresi, son kullanma süresi ve parola sağlamanız gerekir. Daha sonra, makinede anahtarın oluşturulması için yeterli entropi olana kadar beklersiniz.

    ```bash
    sudo gpg --generate-key
    ```

3. GPG dizini sudo ile oluşturulduğundan, aşağıdaki komutu kullanarak sahibini **nxautomation** olarak değiştirmeniz gerekir.

    ```bash
    sudo chown -R nxautomation ~/.gnupg
    ```

### <a name="make-the-keyring-available-to-the-hybrid-runbook-worker"></a>Anahtarlığı Karma Runbook Worker'ın kullanımına sun

Anahtarlık oluşturulduktan sonra, Karma Runbook Worker için kullanılabilir hale getirin. Ayarlar dosyasını **/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf** ayarlarını dosya bölümüne `[worker-optional]`aşağıdaki örnek kodu eklemek için değiştirin.

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>İmza doğrulamanın üzerinde olduğunu doğrulama

Makinede imza doğrulaması devre dışı bırakılmışsa, aşağıdaki sudo komutunu çalıştırarak açmanız gerekir. Çalışma `<LogAnalyticsworkspaceId>` alanı kimliğinizle değiştirin.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>Runbook'u imzalama

İmza doğrulaması yapılandırdıktan sonra, bir runbook imzalamak için aşağıdaki GPG komutunu kullanın.

```bash
gpg –-clear-sign <runbook name>
```

İmzalı runbook ** <runbook name>.asc**denir.

Artık imzalı runbook'u Azure Automation'a yükleyebilir ve normal bir runbook gibi çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook başlatma yöntemleri hakkında daha fazla bilgi edinmek için Azure [Otomasyonunda Runbook Başlatma'ya](automation-starting-a-runbook.md)bakın.
* Metin düzenleyicisinin Azure Otomasyonu'nda PowerShell runbook'larıyla çalışmak için nasıl kullanılacağını anlamak için Azure [Otomasyonunda Runbook Düzenleme'ye](automation-edit-textual-runbook.md)bakın.
* Runbook'larınız başarıyla tamamlalamıyorsa, [runbook yürütme hataları](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails)için sorun giderme kılavuzunu inceleyin.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
* PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
