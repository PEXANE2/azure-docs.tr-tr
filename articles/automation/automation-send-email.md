---
title: Azure Otomasyonu runbook 'tan e-posta gönderme
description: Bir runbook içinden e-posta göndermek için SendGrid 'i nasıl kullanacağınızı öğrenin.
services: automation
ms.subservice: process-automation
ms.date: 07/15/2019
ms.topic: tutorial
ms.openlocfilehash: d4b35458c76da82b33dfcb530cfdc71ee3da3bb6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81604777"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Öğretici: Azure Otomasyonu runbook 'tan e-posta gönderme

PowerShell kullanarak, [SendGrid](https://sendgrid.com/solutions) ile runbook 'tan bir e-posta gönderebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Azure Key Vault oluşturun.
> * `SendGrid` API anahtarınızı anahtar kasasında depolayın.
> * API anahtarınızı alan ve [Azure Key Vault](/azure/key-vault/)depolanan bir API anahtarı kullanarak bir e-posta gönderen yeniden kullanılabilir bir runbook oluşturun.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği: henüz yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* [SendGrid hesabı oluşturun](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account).
* Runbook 'u depolamak ve yürütmek için **az** modüllerle [Otomasyon hesabı](automation-offering-get-started.md) ve [bağlantı olarak çalıştır](automation-create-runas-account.md).

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Aşağıdaki PowerShell betiğini kullanarak bir Azure Key Vault oluşturabilirsiniz. Değişken değerlerini ortamınıza özgü değerlerle değiştirin. Gömülü Azure Cloud Shell, kod bloğunun sağ üst köşesinde bulunan **TRY It** düğmesini kullanarak kullanın. Yerel makinenizde [Azure PowerShell modülü](/powershell/azure/install-az-ps) yüklüyse, kodu yerel olarak da kopyalayabilir ve çalıştırabilirsiniz.

> [!NOTE]
> API anahtarınızı almak için [SendGrid API anahtarınızı bulun](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key)bölümünde bulunan adımları kullanın.

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

## <a name="import-required-modules-to-your-automation-account"></a>Gerekli modülleri Otomasyon hesabınıza aktarın

Bir runbook içinde Azure Key Vault kullanmak için, Otomasyon hesabınızın aşağıdaki modülleri olması gerekir:

* [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile)
* [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault)

**Yükleme seçenekleri**altındaki Azure Otomasyonu sekmesinde **Azure Otomasyonu 'na dağıt** ' a tıklayın. Bu eylem Azure portal açar. Içeri Aktar sayfasında Otomasyon hesabınızı seçin ve **Tamam**' ı tıklatın.

Gerekli modülleri eklemeye yönelik ek yöntemler için bkz. [modülleri Içeri aktarma](/azure/automation/shared-resources/modules#importing-modules).

## <a name="create-the-runbook-to-send-an-email"></a>E-posta göndermek için Runbook 'u oluşturma

Bir Anahtar Kasası oluşturup `SendGrid` API anahtarınızı depoladıktan sonra API anahtarını alan ve bir e-posta gönderen runbook 'u oluşturmak zaman alır.

Bu runbook, `AzureRunAsConnection` Azure Key Vault gizli anahtarı almak için Azure ile kimlik doğrulaması yapmak üzere farklı [Çalıştır hesabı](automation-create-runas-account.md) olarak kullanır.

**Send-GridMailMessage**adlı bir runbook oluşturmak için bu örneği kullanın. PowerShell betiğini değiştirebilir ve farklı senaryolar için onu yeniden kullanabilirsiniz.

1. Azure Otomasyonu hesabınıza gidin.
2. **Işlem Otomasyonu**altında **runbook 'lar**' ı seçin.
3. Runbook 'ların listesinin en üstünde **+ runbook oluştur**' u seçin.
4. **Runbook Ekle** sayfasında, Runbook adı için **Send-gridmailmessage** yazın. Runbook türü için **PowerShell**' i seçin. Ardından **Oluştur**' u seçin.
   ![Runbook oluştur](./media/automation-send-email/automation-send-email-runbook.png)
5. Runbook oluşturulur ve **PowerShell Runbook'unu Düzenle** sayfası açılır.
   ![Runbook 'U düzenleme](./media/automation-send-email/automation-send-email-edit.png)
6. Aşağıdaki PowerShell örneğini **düzenleme** sayfasına kopyalayın. `$VaultName` Anahtar kasayızı oluştururken belirttiğiniz ad olduğundan emin olun.

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

Runbook 'un başarıyla yürütüldüğünü doğrulamak için, [runbook 'U test](manage-runbooks.md#testing-a-runbook) etme veya [runbook 'u başlatma](start-runbooks.md)bölümündeki adımları izleyebilirsiniz.
Test e-postanızı başlangıçta görmüyorsanız, **Önemsiz** ve **istenmeyen posta** klasörlerinizi denetleyin.

## <a name="clean-up"></a>Temizleme

İhtiyacınız kalmadıysa runbook'u silebilirsiniz. Bunu yapmak için runbook listesinden runbook'u seçip **Sil**'e tıklayın.

[Remove-Azkeykasası](https://docs.microsoft.com/powershell/module/az.keyvault/remove-azkeyvault?view=azps-3.7.0) cmdlet 'ini kullanarak anahtar kasasını silin.

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'u oluşturma veya başlatma sorunları için bkz. [runbook 'larda hata giderme](./troubleshoot/runbooks.md).
* Otomasyon hesabınızdaki modülleri güncelleştirmek için bkz. [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)].
* Runbook yürütmesini izlemek için bkz. [Otomasyon 'Dan Azure izleyici günlüklerine iş durumu ve iş akışları iletme](automation-manage-send-joblogs-log-analytics.md).
* Bir runbook 'u uyarı kullanarak tetiklemek için bkz. [Azure Otomasyonu runbook 'u tetiklemek için uyarı kullanma](automation-create-alert-triggered-runbook.md).
