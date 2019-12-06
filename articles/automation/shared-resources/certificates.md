---
title: Azure Otomasyonu 'nda sertifika varlıkları
description: Azure Otomasyonu, Azure ve üçüncü taraf kaynaklarında kimlik doğrulaması yapmak için Runbook 'lar veya DSC yapılandırmalarının erişebileceği şekilde güvenli bir şekilde yapılır.  Bu makalede, sertifikaların ayrıntıları ve hem metin hem de grafik yazarken bunlarla nasıl çalışılacağı açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a66f73e028594cf90f1fa1765910a3df3adbad1a
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849488"
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Otomasyonu 'nda sertifika varlıkları

Sertifikalar, Azure Resource Manager kaynakları için **Get-AzureRmAutomationCertificate** etkinliği kullanılarak runbook 'LARıN veya DSC yapılandırmalarının erişebilmeleri Için Azure Otomasyonu 'nda güvenli bir şekilde depolanır. Bu özellik, kimlik doğrulaması için sertifikaları kullanan runbook 'ları ve DSC yapılandırması oluşturmanızı veya bunları Azure ya da üçüncü taraf kaynaklarına eklemenizi sağlar.

>[!NOTE]
>Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Bu anahtar, sistem tarafından yönetilen bir Key Vault depolanır. Güvenli bir varlık depolamadan önce, anahtar Key Vault yüklenir ve ardından varlığı şifrelemek için kullanılır. Bu işlem Azure Otomasyonu tarafından yönetiliyor.

## <a name="azurerm-powershell-cmdlets"></a>Azurerd PowerShell cmdlet 'leri

Azurerd için aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanıma sunulan [Azurerd. Automation modülünün](/powershell/azure/overview)bir parçası olarak gelir.

|Cmdlet'ler|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Runbook veya DSC yapılandırmasında kullanılacak sertifikayla ilgili bilgileri alır. Sertifikayı yalnızca Get-AutomationCertificate etkinliğinden alabilirsiniz.|
|[New-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Azure Otomasyonu 'nda yeni bir sertifika oluşturur.|
[Remove-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Azure Otomasyonu 'ndan bir sertifikayı kaldırır.|
|[Set-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Sertifika dosyasını karşıya yükleme ve bir. pfx için parolayı ayarlama dahil olmak üzere var olan bir sertifikanın özelliklerini ayarlar.|
|[Add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate)|Belirtilen bulut hizmeti için bir hizmet sertifikası yükler.|

## <a name="activities"></a>Olaylar

Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmalarında sertifikalara erişmek için kullanılır.

| Olaylar | Açıklama |
|:---|:---|
|Get-AutomationCertificate|Runbook veya DSC yapılandırmasında kullanmak için bir sertifika alır. [System. Security. Cryptography. X509Certificates. X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) nesnesi döndürür.|

> [!NOTE] 
> Runbook veya DSC yapılandırması arasındaki bağımlılıkları keşfetmeyi ve tasarım zamanında Otomasyon değişkenlerini karmaşıklaştırmak üzere bir runbook veya DSC yapılandırmasındaki-Name **parametresinde bulunan değişkenleri** kullanmaktan kaçının.

## <a name="python2-functions"></a>Python2 işlevleri

Aşağıdaki tablodaki işlev, bir Python2 runbook 'daki sertifikalara erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationvarlıklar. get_automation_certificate | Bir sertifika varlığı hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine erişebilmek için Python runbook 'unun başlangıcında **automationvarlıklarının** modülünü içeri aktarmanız gerekir.

## <a name="creating-a-new-certificate"></a>Yeni bir sertifika oluşturma

Yeni bir sertifika oluşturduğunuzda bir. cer veya. pfx dosyasını Azure Otomasyonu 'na yüklersiniz. Sertifikayı dışarı aktarılabilir olarak işaretlerseniz, Azure Otomasyonu sertifika deposundan dışarı aktarabilirsiniz. Dışarı aktarılabilir değilse, yalnızca runbook veya DSC yapılandırmasında oturum açmak için kullanılabilir. Azure Otomasyonu, sertifikanın sağlayıcıya sahip olmasını gerektirir: **Microsoft IYILEŞTIRILMIŞ RSA ve AES şifreleme sağlayıcısı**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Azure portal ile yeni bir sertifika oluşturmak için

1. Otomasyon hesabınızdan **varlıklar kutucuğuna** tıklayarak **varlıklar** sayfasını açın.
2. **Sertifikalar kutucuğuna** tıklayarak **Sertifikalar** sayfasını açın.
3. Sayfanın üst kısmındaki **sertifika ekle** ' ye tıklayın.
4. **Ad** kutusuna sertifika için bir ad yazın.
5. Bir. cer veya. pfx dosyasına gitmek için, **sertifika dosyası yükle**altında **bir dosya seç** ' e tıklayın. Bir. pfx dosyası seçerseniz, bir parola ve verilemeyeceğini belirtin.
6. Yeni sertifika varlığını kaydetmek için **Oluştur** ' a tıklayın.

### <a name="to-create-a-new-certificate-with-powershell"></a>PowerShell ile yeni bir sertifika oluşturmak için

Aşağıdaki örnekte, yeni bir Otomasyon sertifikasının nasıl oluşturulduğu ve dışarı aktarılabilir olarak işaretlenme gösterilmiştir. Bu, var olan bir. pfx dosyasını içeri aktarır.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla yeni bir sertifika oluşturma

Aşağıdaki örnek, PowerShell aracılığıyla Kaynak Yöneticisi şablonu kullanarak Otomasyon hesabınıza nasıl bir sertifikanın dağıtılacağını göstermektedir:

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

Bir sertifika kullanmak için **Get-AutomationCertificate** etkinliğini kullanın. [Get-AzureRmAutomationCertificate](/powershell/module/azurerm.automation/get-azurermautomationcertificate) cmdlet 'ini, sertifika varlığı hakkında bilgi döndürdüğünden, sertifikanın kendisi değil, kullanamazsınız.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek kodda bir runbook 'ta bir bulut hizmetine nasıl sertifika ekleneceği gösterilmektedir. Bu örnekte, parola şifrelenmiş bir Otomasyon değişkeninden alınır.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Grafik runbook örneği

Kitaplık bölmesinde sertifikaya sağ tıklayıp **tuvale Ekle**' yi seçerek bir grafik runbook 'Una bir **Get-AutomationCertificate** eklersiniz.

![Tuvale sertifika ekleme](../media/certificates/automation-certificate-add-to-canvas.png)

Aşağıdaki görüntüde bir grafik runbook 'unda sertifika kullanmanın bir örneği gösterilmektedir. Bu, bir metin runbook 'tan bir bulut hizmetine nasıl sertifika ekleneceğini gösteren önceki örnekle aynıdır.

![Örnek grafik yazma](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2 örneği

Aşağıdaki örnek, Python2 runbook 'larda sertifikalara nasıl erişegösterdiğini gösterir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook 'un gerçekleştirmek üzere tasarlandığı etkinliklerin mantıksal akışını denetlemek için bağlantılarla çalışma hakkında daha fazla bilgi edinmek için bkz. [grafik yazma Içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow). 
