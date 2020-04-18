---
title: Azure Otomasyonu runbook'undan e-posta gönderme
description: Runbook'un içinden e-posta göndermek için SendGrid'i nasıl kullanacağınızı öğrenin.
services: automation
ms.subservice: process-automation
ms.date: 07/15/2019
ms.topic: tutorial
ms.openlocfilehash: d4b35458c76da82b33dfcb530cfdc71ee3da3bb6
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604777"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Öğretici: Azure Otomasyonu çalışma kitabından e-posta gönderme

PowerShell'i kullanarak [SendGrid](https://sendgrid.com/solutions) ile bir runbook'tan e-posta gönderebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Azure Anahtar Kasası oluşturun.
> * API `SendGrid` anahtarınızı anahtar kasasında saklayın.
> * [API](/azure/key-vault/)anahtarınızı alan ve Azure Key Vault'ta depolanan bir API anahtarını kullanarak e-posta gönderen yeniden kullanılabilir bir runbook oluşturun.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği: Henüz bir aboneliğiniz yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
* [Bir SendGrid Hesabı oluşturun.](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account)
* Runbook'u depolamak ve yürütmek için **Az** modülleri ile [otomasyon hesabı](automation-offering-get-started.md) ve [Bağlantı Olarak Çalıştır.](automation-create-runas-account.md)

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Aşağıdaki PowerShell komut dosyasını kullanarak bir Azure Anahtar Kasası oluşturabilirsiniz. Değişken değerlerini ortamınıza özgü değerlerle değiştirin. Kod bloğunun sağ üst köşesinde bulunan **Try It** düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın. Azure [PowerShell Modülü](/powershell/azure/install-az-ps) yerel makinenizde yüklüyse kodu yerel olarak kopyalayabilir ve çalıştırabilirsiniz.

> [!NOTE]
> API anahtarınızı almak için [SendGrid API anahtarınızı bul'da](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key)bulunan adımları kullanın.

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

Azure Anahtar Kasası oluşturmanın ve bir sırrı saklamanın diğer yolları için [Key Vault Quickstarts'a](/azure/key-vault/)bakın.

## <a name="import-required-modules-to-your-automation-account"></a>Gerekli modülleri Otomasyon hesabınıza aktarma

Azure Key Vault'u bir runbook içinde kullanmak için Otomasyon hesabınızın aşağıdaki modüllere ihtiyacı vardır:

* [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile)
* [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault)

**Yükleme Seçenekleri**altındaki Azure Otomasyonu sekmesinde Azure Otomasyonu'na **Dağıt'ı** tıklatın. Bu eylem Azure portalını açar. Alma sayfasında, Otomasyon hesabınızı seçin ve **Tamam'ı**tıklatın.

Gerekli modülleri eklemek için ek yöntemler [için, İthalat Modülleri'ne](/azure/automation/shared-resources/modules#importing-modules)bakın.

## <a name="create-the-runbook-to-send-an-email"></a>E-posta göndermek için runbook'u oluşturma

Anahtar kasası oluşturduktan ve API anahtarınızı `SendGrid` depoladıktan sonra, API anahtarını alan ve bir e-posta gönderen runbook'u oluşturmanın zamanı gelmiştir.

Bu runbook, Azure Key Vault'tan sırrı almak için Azure ile kimlik doğrulaması yapmak için Çalıştır `AzureRunAsConnection` [hesabı](automation-create-runas-account.md) olarak kullanır.

Bu örneği, **Gönder-GridMailMessage**adlı bir runbook oluşturmak için kullanın. PowerShell komut dosyasını değiştirebilir ve farklı senaryolar için yeniden kullanabilirsiniz.

1. Azure Otomasyon hesabınıza gidin.
2. **Proses Otomasyonu**altında **Runbook'ları**seçin.
3. Runbook listesinin en üstünde + **Runbook oluştur'u**seçin.
4. **Runbook Ekle** sayfasında, runbook adı için **GridMailMessage Gönder'i** girin. Runbook türü için **PowerShell'i**seçin. Ardından **Oluştur'u**seçin.
   ![Runbook oluştur](./media/automation-send-email/automation-send-email-runbook.png)
5. Runbook oluşturulur ve **PowerShell Runbook'unu Düzenle** sayfası açılır.
   ![Runbook'u edin](./media/automation-send-email/automation-send-email-edit.png)
6. Aşağıdaki PowerShell örneğini **Edit** sayfasına kopyalayın. `$VaultName` Anahtar kasanızı oluşturduğunuzda belirttiğiniz adın olduğundan emin olun.

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

7. Runbook'u kaydetmek ve yayımlamak için **Yayımla'yı** seçin.

Runbook'un başarılı bir şekilde yürütüldettiğini doğrulamak için [çalışma kitabını test](manage-runbooks.md#testing-a-runbook) etme veya [runbook Başlat'ın](start-runbooks.md)altındaki adımları izleyebilirsiniz.
Test e-postanızı başlangıçta görmüyorsanız, **Önemsiz** ve **Spam** klasörlerinizi kontrol edin.

## <a name="clean-up"></a>Temizleme

İhtiyacınız kalmadıysa runbook'u silebilirsiniz. Bunu yapmak için runbook listesinden runbook'u seçip **Sil**'e tıklayın.

[Remove-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/remove-azkeyvault?view=azps-3.7.0) cmdlet'i kullanarak anahtar kasasını silin.

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'unuzu oluşturan veya başlatan sorunlar [için, runbook'larla ilgili Sorun Giderme hatalarına](./troubleshoot/runbooks.md)bakın.
* Otomasyon hesabınızdaki modülleri güncelleştirmek için [Azure Otomasyonu'nda Azure PowerShell modüllerini nasıl güncelleştirici](automation-update-azure-modules.md)göreceğiz ]
* Runbook yürütmesini izlemek için, [Otomasyon'dan Azure Monitor günlüklerine ileri iş durumu ve iş akışları](automation-manage-send-joblogs-log-analytics.md)na bakın.
* Bir uyarı kullanarak bir runbook'u tetiklemek için azure [otomasyon runbook'u tetiklemek için uyarı kullan'a](automation-create-alert-triggered-runbook.md)bakın.
