---
title: Azure Otomasyonu'ndaki sertifika varlıkları
description: Sertifikalar Azure Otomasyonu'nda güvenli bir şekilde kullanılabilir, böylece azure ve üçüncü taraf kaynaklarına karşı kimlik doğrulama için runbook'lar veya DSC yapılandırmaları tarafından erişilebilir.  Bu makalede, sertifikaların ayrıntıları ve hem metinsel hem de grafiksel yazarlıkta onlarla nasıl çalışılabilenler açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a66f73e028594cf90f1fa1765910a3df3adbad1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849488"
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Otomasyonu'ndaki sertifika varlıkları

Sertifikalar Azure Kaynak Yöneticisi kaynakları için **Get-AzureRmAutomationCertificate** etkinliğini kullanarak runbook'lar veya DSC yapılandırmaları tarafından erişilebilmeleri için Azure Otomasyonu'nda güvenli bir şekilde depolanır. Bu özellik, kimlik doğrulaması için sertifikaları kullanan veya bunları Azure veya üçüncü taraf kaynaklarına ekleyen runbook'lar ve DSC yapılandırmaları oluşturmanıza olanak tanır.

>[!NOTE]
>Azure Otomasyonu'ndaki güvenli varlıklar kimlik bilgilerini, sertifikaları, bağlantıları ve şifreli değişkenleri içerir. Bu varlıklar, her otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu anahtar, anahtar kasa yönetilen bir sistemde saklanır. Güvenli bir kıymeti depolamadan önce, anahtar Key Vault'tan yüklenir ve sonra varlığı şifrelemek için kullanılır. Bu işlem Azure Automation tarafından yönetilir.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlets

AzureRM için, aşağıdaki tablodaki cmdlet'ler Windows PowerShell ile otomasyon kimlik bilgileri varlıkları oluşturmak ve yönetmek için kullanılır. Otomasyon runbook'larında ve DSC yapılandırmalarında kullanılabilen [AzureRM.Automation modülünün](/powershell/azure/overview)bir parçası olarak gönderiyaparlar.

|Cmdlet’ler|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Bir runbook veya DSC yapılandırmasında kullanılacak bir sertifika hakkındaki bilgileri alır. Sertifikayı yalnızca Get-AutomationCertificate etkinliğinden alabilirsiniz.|
|[New-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Azure Otomasyonu'nda yeni bir sertifika oluşturur.|
[Remove-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Azure Otomasyonundan bir sertifika kaldırır.|
|[Set-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Sertifika dosyasını yükleme ve .pfx parolasını ayarlama dahil olmak üzere varolan bir sertifikanın özelliklerini ayarlar.|
|[Add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate)|Belirtilen bulut hizmeti için bir hizmet sertifikası yükler.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler, bir runbook ve DSC yapılandırmalarında sertifikalara erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|Get-AutomationCertificate|Runbook veya DSC yapılandırmasında kullanılacak bir sertifika alır. [System.Security.Cryptography.X509Certificates.X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) nesnesini döndürür.|

> [!NOTE] 
> Runbook veya DSC yapılandırması arasındaki bağımlılıkları keşfetmeyi zorlaştırdığından, runbook veya DSC yapılandırmasında **-Ad** parametresi -Ad parametresi ve tasarım zamanında Otomasyon değişkenleri kullanarak kaçınmalısınız.

## <a name="python2-functions"></a>Python2 fonksiyonları

Aşağıdaki tablodaki işlev, Python2 runbook'taki sertifikalara erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_certificate | Sertifika kıymeti hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine erişmek için Python runbook'unuzun başında **otomasyon varlıkları** modülünün içe aktarılması gerekir.

## <a name="creating-a-new-certificate"></a>Yeni bir sertifika oluşturma

Yeni bir sertifika oluşturduğunuzda, Azure Otomasyonu'na bir .cer veya .pfx dosyası yüklersiniz. Sertifikayı dışa aktarılabilir olarak işaretlerseniz, sertifikayı Azure Otomasyon sertifika deposundan aktarabilirsiniz. Dışa aktarılamıyorsa, yalnızca runbook veya DSC yapılandırması içinde imzalamak için kullanılabilir. Azure Otomasyonu, sertifikanın sağlayıcıya sahip olmasını gerektirir: **Microsoft Enhanced RSA ve AES Şifreleme Sağlayıcısı.**

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Azure portalı ile yeni bir sertifika oluşturmak için

1. Otomasyon hesabınızdan **Varlıklar** sayfasını açmak için **Varlıklar** döşemesini tıklatın.
2. **Sertifikalar** sayfasını açmak için **Sertifikalar** döşemesini tıklatın.
3. Sayfanın üst kısmında **sertifika ekle'yi** tıklatın.
4. **Ad** kutusuna sertifika için bir ad yazın.
5. .cer veya .pfx dosyasına göz atmak için **sertifika dosyası yükle'nin**altındaki **dosyayı seçin'i** tıklatın. Bir .pfx dosyası seçerseniz, bir parola ve dışa aktarılıp dışlanamayacağını belirtin.
6. Yeni sertifika varlığını kaydetmek için **Oluştur'u** tıklatın.

### <a name="to-create-a-new-certificate-with-powershell"></a>PowerShell ile yeni bir sertifika oluşturmak için

Aşağıdaki örnek, yeni bir Otomasyon sertifikasının nasıl oluşturulup dışa aktarılabilir olarak işaretlenebildiğini gösterir. Bu, varolan bir .pfx dosyayı içeri aktarır.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla yeni bir sertifika oluşturma

Aşağıdaki örnek, PowerShell aracılığıyla Bir Kaynak Yöneticisi şablonunu kullanarak bir sertifikanın Otomasyon Hesabınıza nasıl dağıtılabildiğini gösterir:

```powershell-interactive
$AutomationAccountName = "<automation account name>"
$PfxCertPath = '<PFX cert path'
$CertificatePassword = '<password>'
$certificateName = '<certificate name>'
$AutomationAccountName = '<automation account name>'
$flags = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::PersistKeySet `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::MachineKeySet
# Load the certificate into memory
$PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPath, $CertificatePassword, $flags)
# Export the certificate and convert into base 64 string
$Base64Value = [System.Convert]::ToBase64String($PfxCert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12))
$Thumbprint = $PfxCert.Thumbprint


$json = @"
{
    '`$schema': 'https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#',
    'contentVersion': '1.0.0.0',
    'resources': [
        {
            'name': '$AutomationAccountName/$certificateName',
            'type': 'Microsoft.Automation/automationAccounts/certificates',
            'apiVersion': '2015-10-31',
            'properties': {
                'base64Value': '$Base64Value',
                'thumbprint': '$Thumbprint',
                'isExportable': true
            }
        }
    ]
}
"@

$json | out-file .\template.json
New-AzureRmResourceGroupDeployment -Name NewCert -ResourceGroupName TestAzureAuto -TemplateFile .\template.json
```

## <a name="using-a-certificate"></a>Sertifika kullanma

Sertifika kullanmak için **Get-AutomationCertificate** etkinliğini kullanın. Sertifika varlığı yla ilgili bilgileri döndürdediğinden, sertifikanın kendisi hakkında bilgi döndüremediği için [Get-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/get-azurermautomationcertificate) cmdlet'i kullanamazsınız.

### <a name="textual-runbook-sample"></a>Metin çalışma kitabı örneği

Aşağıdaki örnek kod, bir runbook'taki bulut hizmetine nasıl sertifika ekleyeceğinizi gösterir. Bu örnekte, parola şifreli bir otomasyon değişkeninden alınır.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Grafik çalışma kitabı örneği

Kitaplık bölmesindeki sertifikaya sağ tıklayarak ve **tuvale Ekle'yi**seçerek grafik çalışma kitabına **Get-AutomationCertificate** eklersiniz.

![Tuvale sertifika ekleme](../media/certificates/automation-certificate-add-to-canvas.png)

Aşağıdaki resim, grafik çalışma kitabında bir sertifika kullanma örneğini gösterir. Bu, metin seli bir runbook'tan bulut hizmetine nasıl sertifika ekleyeceğini gösteren önceki örnekle aynıdır.

![Örnek Grafik Selyazma](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2 örneği

Aşağıdaki örnek, Python2 runbook'larında sertifikalara nasıl erişilir gösterilmektedir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook'unuzun gerçekleştirmek üzere tasarladığı mantıksal etkinlik akışını denetlemek için bağlantılarla çalışma hakkında daha fazla bilgi edinmek için [bkz.](../automation-graphical-authoring-intro.md#links-and-workflow) 
