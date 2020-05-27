---
title: Azure Otomasyonu runbook 'tan e-posta gönderme
description: Bu makalede, runbook 'tan bir e-posta gönderme yöntemi açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 07/15/2019
ms.topic: conceptual
ms.openlocfilehash: a92f65bd88a5aec79a179a6e2d53de15c274add4
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834569"
---
# <a name="send-an-email-from-a-runbook"></a>Runbook’tan e-posta gönderme

PowerShell kullanarak, [SendGrid](https://sendgrid.com/solutions) ile runbook 'tan bir e-posta gönderebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* [SendGrid hesabı](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account).
* **Az** modüllerle [Otomasyon hesabı](automation-offering-get-started.md) .
* Runbook 'u depolamak ve yürütmek için [Farklı Çalıştır hesabı](automation-create-runas-account.md) .

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Aşağıdaki PowerShell betiğini kullanarak bir Azure Key Vault oluşturabilirsiniz. Değişken değerlerini ortamınıza özgü değerlerle değiştirin. Gömülü Azure Cloud Shell, kod bloğunun sağ üst köşesinde bulunan **TRY It** düğmesini kullanarak kullanın. Yerel makinenizde [az modüller](/powershell/azure/install-az-ps) yüklüyse, kodu yerel olarak kopyalayabilir ve çalıştırabilirsiniz.

> [!NOTE]
> API anahtarınızı almak için [SendGrid API anahtarınızı bulma](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key)bölümündeki adımları kullanın.

```azurepowershell-interactive
$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
# If you omit the SubscriptionId parameter, the default subscription is selected.
Connect-AzAccount -SubscriptionId $SubscriptionId

# Use Get-AzLocation to see your available locations.
$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "<Enter a universally unique vault name>"
$SendGridAPIKey = "<SendGrid API key>"
$AutomationAccountName = "testaa"

# Create new Resource Group, or omit this step if you already have a resource group.
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region
$resourceId = $newKeyVault.ResourceId

# Convert the SendGrid API key into a SecureString
$Secret = ConvertTo-SecureString -String $SendGridAPIKey -AsPlainText -Force
Set-AzKeyVaultSecret -VaultName $VaultName -Name 'SendGridAPIKey' -SecretValue $Secret

# Grant access to the Key Vault to the Automation Run As account.
$connection = Get-AzAutomationConnection -ResourceGroupName $KeyVaultResourceGroupName -AutomationAccountName $AutomationAccountName -Name AzureRunAsConnection
$appID = $connection.FieldDefinitionValues.ApplicationId
Set-AzKeyVaultAccessPolicy -VaultName $VaultName -ServicePrincipalName $appID -PermissionsToSecrets Set, Get
```

Azure Key Vault oluşturup gizli dizi depolamanın diğer yolları için bkz. [Key Vault hızlı](/azure/key-vault/)başlangıçlar.

## <a name="import-required-modules-into-your-automation-account"></a>Gerekli modülleri Otomasyon hesabınıza aktarın

Bir runbook içinde Azure Key Vault kullanmak için aşağıdaki modülleri Otomasyon hesabınıza aktarmanız gerekir:

    * [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile)
    * [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault)

Yönergeler için bkz. [Import az modules](shared-resources/modules.md#import-az-modules).

## <a name="create-the-runbook-to-send-an-email"></a>E-posta göndermek için Runbook 'u oluşturma

Bir Key Vault oluşturup API anahtarınızı depoladıktan sonra `SendGrid` API anahtarını alan ve bir e-posta gönderen runbook 'u oluşturmak zaman alır. `AzureRunAsConnection`Azure Key Vault parolayı almak Için Azure ile kimlik doğrulaması yapmak üzere farklı [Çalıştır hesabı](automation-create-runas-account.md) olarak kullanan bir runbook kullanalım. Runbook **Send-GridMailMessage iletisini**çağıracağız. Örnek amaçlar için kullanılan PowerShell betiğini değiştirebilir ve farklı senaryolar için onu yeniden kullanabilirsiniz.

1. Azure Otomasyonu hesabınıza gidin.
2. **Işlem Otomasyonu**altında **runbook 'lar**' ı seçin.
3. Runbook 'ların listesinin en üstünde **+ runbook oluştur**' u seçin.
4. Runbook Ekle sayfasında, Runbook adı için **Send-GridMailMessage** yazın. Runbook türü için **PowerShell**' i seçin. Ardından **Oluştur**' u seçin.
   ![Runbook oluştur](./media/automation-send-email/automation-send-email-runbook.png)
5. Runbook oluşturulur ve PowerShell Runbook'unu Düzenle sayfası açılır.
   ![Runbook 'U düzenleme](./media/automation-send-email/automation-send-email-edit.png)
6. Aşağıdaki PowerShell örneğini düzenleme sayfasına kopyalayın. `VaultName`Key Vault için seçtiğiniz adı belirttiğinden emin olun.

    ```powershell-interactive
    Param(
      [Parameter(Mandatory=$True)]
      [String] $destEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $fromEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $subject,
      [Parameter(Mandatory=$True)]
      [String] $content
    )

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Out-Null
    $VaultName = "<Enter your vault name>"
    $SENDGRID_API_KEY = (Get-AzKeyVaultSecret -VaultName $VaultName -Name "SendGridAPIKey").SecretValueText
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Authorization", "Bearer " + $SENDGRID_API_KEY)
    $headers.Add("Content-Type", "application/json")

    $body = @{
    personalizations = @(
        @{
            to = @(
                    @{
                        email = $destEmailAddress
                    }
            )
        }
    )
    from = @{
        email = $fromEmailAddress
    }
    subject = $subject
    content = @(
        @{
            type = "text/plain"
            value = $content
        }
    )
    }

    $bodyJson = $body | ConvertTo-Json -Depth 4

    $response = Invoke-RestMethod -Uri https://api.sendgrid.com/v3/mail/send -Method Post -Headers $headers -Body $bodyJson
    ```

7. Runbook 'u kaydetmek ve yayımlamak için **Yayımla** ' yı seçin.

Runbook 'un başarıyla yürütüldüğünü doğrulamak için, [runbook 'U test](manage-runbooks.md#test-a-runbook) etme veya [runbook 'u başlatma](start-runbooks.md)bölümündeki adımları izleyebilirsiniz.

Test e-postanızı başlangıçta görmüyorsanız, istenmeyen ve **Istenmeyen posta** **klasörlerinizi denetleyin** .

## <a name="clean-up-resources-after-the-email-operation"></a>E-posta işleminden sonra kaynakları temizle

1. Runbook artık gerekli olmadığında runbook listesinden seçin ve **Sil**' e tıklayın.

2. [Remove-Azkeykasası](https://docs.microsoft.com/powershell/module/az.keyvault/remove-azkeyvault?view=azps-3.7.0) cmdlet 'ini kullanarak Key Vault silin.

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

* Log Analytics çalışma alanınıza runbook iş verilerini göndermek için bkz. [Azure Otomasyonu iş verilerini Azure izleyici günlüklerine iletme](automation-manage-send-joblogs-log-analytics.md).
* Taban düzeyi ölçümlerini ve günlüklerini izlemek için bkz. [Azure Otomasyonu runbook 'u tetiklemek için uyarı kullanma](automation-create-alert-triggered-runbook.md).
* Runbook işlemleri sırasında kaynaklanan sorunları düzeltmek için bkz. [runbook sorunlarını giderme](./troubleshoot/runbooks.md).