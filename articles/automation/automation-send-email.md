---
title: Azure Otomasyonu runbook 'tan e-posta gönderme
description: Bir runbook içinden e-posta göndermek için SendGrid 'i nasıl kullanacağınızı öğrenin.
services: automation
ms.subservice: process-automation
ms.date: 07/15/2019
ms.topic: tutorial
ms.openlocfilehash: 6acb68b7bbaa54db2e4143a42e43aede2caed35f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75420701"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Öğretici: Azure Otomasyonu runbook 'tan e-posta gönderme

PowerShell kullanarak, [SendGrid](https://sendgrid.com/solutions) ile runbook 'tan bir e-posta gönderebilirsiniz. Bu öğretici, [Azure Keykasasında](/azure/key-vault/)depolanan bir API anahtarı kullanarak bir e-posta gönderen yeniden kullanılabilir runbook oluşturmayı gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Azure Keykasası oluşturma
> * SendGrid API anahtarınızı Anahtar Kasası 'nda depolayın
> * API anahtarınızı alıp bir e-posta gönderen bir runbook oluşturun

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği: henüz yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* [SendGrid hesabı oluşturun](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account).
* Runbook 'u depolamak ve yürütmek için **az** modüllerle [Otomasyon hesabı](automation-offering-get-started.md) ve [bağlantı olarak çalıştır](automation-create-runas-account.md).

## <a name="create-an-azure-keyvault"></a>Azure Keykasası oluşturma

Aşağıdaki PowerShell betiğini kullanarak bir Azure Keykasası oluşturabilirsiniz. Değişken değerlerini ortamınıza özgü değerlerle değiştirin. Gömülü Azure Cloud Shell, kod bloğunun sağ üst köşesinde bulunan <kbd>TRY It</kbd> düğmesini kullanarak kullanın. Yerel makinenizde [Azure PowerShell modülü](/powershell/azure/install-az-ps) yüklüyse, kodu yerel olarak da kopyalayabilir ve çalıştırabilirsiniz.

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

# Grant access to the KeyVault to the Automation RunAs account.
$connection = Get-AzAutomationConnection -ResourceGroupName $KeyVaultResourceGroupName -AutomationAccountName $AutomationAccountName -Name AzureRunAsConnection
$appID = $connection.FieldDefinitionValues.ApplicationId
Set-AzKeyVaultAccessPolicy -VaultName $VaultName -ServicePrincipalName $appID -PermissionsToSecrets Set, Get
```

Azure Keykasasını oluşturmanın ve bir gizli dizi depolamanın diğer yolları için bkz. [Keykasahızlı](/azure/key-vault/)başlangıçlar.

## <a name="import-required-modules-to-your-automation-account"></a>Gerekli modülleri Otomasyon hesabınıza aktarın

Bir runbook 'ta Azure Keykasasını kullanmak için Otomasyon hesabınızın aşağıdaki modüller gerekir:

* [Az. Profile](https://www.powershellgallery.com/packages/Az.Profile).
* [Az. Keykasası](https://www.powershellgallery.com/packages/Az.KeyVault).

Yükleme seçenekleri altındaki Azure Otomasyonu sekmesinde <kbd>Azure Otomasyonu 'Na dağıt</kbd> ' a tıklayın. Bu eylem Azure portal açar. Içeri Aktar sayfasında Otomasyon hesabınızı seçin ve <kbd>Tamam</kbd>' ı tıklatın.

Gerekli modülleri eklemeye yönelik ek yöntemler için bkz. [modülleri Içeri aktarma](/azure/automation/shared-resources/modules#import-modules).

## <a name="create-the-runbook-to-send-an-email"></a>E-posta göndermek için Runbook 'u oluşturma

Bir Anahtar Kasası oluşturup SendGrid API anahtarınızı depoladıktan sonra API anahtarını alacak ve e-posta gönderen runbook 'u oluşturmak zaman alır.

Bu runbook, Azure Keykasasından gizli dizi almak üzere Azure ile kimlik doğrulaması yapmak için AzureRunAsConnection [Farklı Çalıştır hesabını](automation-create-runas-account.md) kullanır.

**Send-GridMailMessage**adlı bir runbook oluşturmak için bu örneği kullanın. PowerShell betiğini değiştirebilir ve farklı senaryolar için onu yeniden kullanabilirsiniz.

1. Azure Otomasyonu hesabınıza gidin.
2. **Işlem Otomasyonu**altında **runbook 'lar**' ı seçin.
3. Runbook 'ların listesinin en üstünde **+ runbook oluştur**' u seçin.
4. **Runbook Ekle** sayfasında, Runbook adı için **Send-gridmailmessage** yazın. Runbook türü için **PowerShell**' i seçin. Ardından **Oluştur**’u seçin.
   Runbook oluşturma ![](./media/automation-send-email/automation-send-email-runbook.png)
5. Runbook oluşturulur ve **PowerShell Runbook'unu Düzenle** sayfası açılır.
   ![runbook 'U düzenleme](./media/automation-send-email/automation-send-email-edit.png)
6. Aşağıdaki PowerShell örneğini **düzenleme** sayfasına kopyalayın. `$VaultName`, anahtar kasanızı oluştururken belirttiğiniz ad olduğundan emin olun.

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
Test e-postanızı başlangıçta görmüyorsanız, **Önemsiz** ve **istenmeyen posta** klasörlerinizi denetleyin.

## <a name="clean-up"></a>Temizle

İhtiyacınız kalmadıysa runbook'u silebilirsiniz. Bunu yapmak için runbook listesinden runbook'u seçip **Sil**'e tıklayın.

[Remove-Azurermkeykasası](/powershell/module/azurerm.keyvault/remove-azurermkeyvault?view=azurermps) cmdlet 'ini kullanarak anahtar kasasını silin.

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'u oluşturma veya başlatma sorunları için bkz. [runbook 'larda hata giderme](./troubleshoot/runbooks.md).
* Otomasyon hesabınızdaki modülleri güncelleştirmek için bkz. [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)].
* Runbook yürütmesini izlemek için bkz. [Otomasyon 'Dan Azure izleyici günlüklerine iş durumu ve iş akışları iletme](automation-manage-send-joblogs-log-analytics.md).
* Bir runbook 'u uyarı kullanarak tetiklemek için bkz. [Azure Otomasyonu runbook 'u tetiklemek için uyarı kullanma](automation-create-alert-triggered-runbook.md).
