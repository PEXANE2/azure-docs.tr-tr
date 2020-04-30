---
title: Azure Otomasyonu 'nda sertifikayı yönetme
description: Sertifikalar Azure Otomasyonu 'nda güvenli bir şekilde depolanır, böylece runbook 'lar veya DSC yapılandırmalarının Azure ve üçüncü taraf kaynaklarında kimlik doğrulaması yapmak için bunlara erişebilmesi sağlanır. Bu makalede, sertifikaların ayrıntıları ve hem metin hem de grafik yazarken bunlarla nasıl çalışılacağı açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 69bc1a0333365e15452c6d3b253266d37d99b608
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732820"
---
# <a name="manage-certificates-in-azure-automation"></a>Azure Otomasyonu 'nda sertifikaları yönetme

Sertifikalar Azure Otomasyonu 'nda güvenli bir şekilde depolanır, böylece Azure Resource Manager kaynaklar için [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) etkinliği kullanılarak runbook 'LAR veya DSC yapılandırmalarının bunlara erişilebilir. Güvenli sertifika depolama, kimlik doğrulaması için sertifika kullanan veya Azure ya da üçüncü taraf kaynaklarına ekleyebileceğiniz runbook 'lar ve DSC yapılandırması oluşturmanızı sağlar.

Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Bu anahtar, sistem tarafından yönetilen bir Key Vault depolanır. Güvenli bir varlık depolamadan önce, anahtar Key Vault yüklenir ve ardından varlığı şifrelemek için kullanılır. Bu işlem Azure Otomasyonu tarafından yönetiliyor.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="az-powershell-cmdlets"></a>Az PowerShell cmdlet 'leri

Az, aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanılabilen [az. Automation modülünün](/powershell/azure/overview)bir parçası olarak gönderilir.

|Cmdlet |Açıklama|
| --- | ---|
|[Add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate)|Belirtilen bulut hizmeti için bir hizmet sertifikası yükler.|
|[Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0)|Runbook veya DSC yapılandırmasında kullanılacak sertifikayla ilgili bilgileri alır. Yalnızca `Get-AutomationCertificate` etkinliğini kullanarak sertifikayı alabilirsiniz.|
|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.7.0)|Azure Otomasyonu 'nda yeni bir sertifika oluşturur.|
|[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationCertificate?view=azps-3.7.0)|Azure Otomasyonu 'ndan bir sertifikayı kaldırır.|
|[Set-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationCertificate?view=azps-3.7.0)|Sertifika dosyasını karşıya yükleme ve bir **. pfx** dosyası için parolayı ayarlama dahil olmak üzere mevcut bir sertifikanın özelliklerini ayarlar.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmalarında sertifikalara erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|`Get-AutomationCertificate`|Runbook veya DSC yapılandırmasında kullanmak için bir sertifika alır. [System. Security. Cryptography. X509Certificates. X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) nesnesi döndürür.|

> [!NOTE] 
> Bir runbook veya DSC yapılandırmasında, `Name` `Get-AutomationCertificate` içindeki değişkenleri kullanmaktan kaçının. Bu parametrede değişkenlerin kullanılması, runbook 'lar ya da DSC yapılandırma ve otomasyon değişkenleri arasındaki bağımlılıkların tasarım zamanında bulunmasını karmaşıklaştırır.

## <a name="python-2-functions"></a>Python 2 işlevleri

Aşağıdaki tablodaki işlev, bir Python 2 runbook 'unda sertifikalara erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| `automationassets.get_automation_certificate` | Bir sertifika varlığı hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine erişmek için `automationassets` Python runbook 'ınızın başlangıcında modülünü içeri aktarmanız gerekir.

## <a name="creating-a-new-certificate"></a>Yeni bir sertifika oluşturma

Yeni bir sertifika oluşturduğunuzda bir. cer veya. pfx dosyasını Azure Otomasyonu 'na yüklersiniz. Sertifikayı dışarı aktarılabilir olarak işaretlerseniz, Azure Otomasyonu sertifika deposundan dışarı aktarabilirsiniz. Dışarı aktarılabilir değilse, yalnızca runbook veya DSC yapılandırmasında oturum açmak için kullanılabilir. Azure Otomasyonu, sağlayıcının **Microsoft tarafından GELIŞTIRILMIŞ RSA ve AES şifreleme sağlayıcısına**sahip olmasını gerektirir.

### <a name="create-a-new-certificate-with-the-azure-portal"></a>Azure portal yeni bir sertifika oluşturun

1. Otomasyon hesabınızdan varlıklar sayfasını açmak için **varlıklar** ' a tıklayın.
2. Sertifikalar sayfasını açmak için **Sertifikalar** ' ı seçin.
3. Sayfanın üst kısmındaki **sertifika ekle** ' ye tıklayın.
4. **Ad** alanına sertifika için bir ad yazın.
5. Bir **. cer** veya **. pfx** dosyasına gitmek Için, **sertifika dosyası yükle**altında **bir dosya seç** ' e tıklayın. Bir **. pfx**dosyası seçerseniz, bir parola belirtin ve verilemeyeceğini belirtin.
6. Yeni sertifika varlığını kaydetmek için **Oluştur** ' a tıklayın.

### <a name="create-a-new-certificate-with-powershell"></a>PowerShell ile yeni bir sertifika oluşturma

Aşağıdaki örnekte, yeni bir Otomasyon sertifikasının nasıl oluşturulduğu ve dışarı aktarılabilir olarak işaretlenme gösterilmiştir. Bu örnek, var olan bir **. pfx** dosyasını içeri aktarır.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla yeni bir sertifika oluşturma

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
New-AzResourceGroupDeployment -Name NewCert -ResourceGroupName TestAzureAuto -TemplateFile .\template.json
```

## <a name="using-a-certificate"></a>Sertifika kullanma

Bir sertifika kullanmak için `Get-AutomationCertificate` etkinliğini kullanın. [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) cmdlet 'ini, sertifika varlığı hakkında bilgi döndürdüğünden, sertifikanın kendisi değil, kullanamazsınız.

### <a name="textual-runbook-example"></a>Metinsel runbook örneği

Aşağıdaki örnek, bir runbook 'ta bir bulut hizmetine nasıl sertifika ekleneceğini gösterir. Bu örnekte, parola şifrelenmiş bir Otomasyon değişkeninden alınır.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-example"></a>Grafik runbook örneği

Kitaplık bölmesinde `Get-AutomationCertificate` sertifikaya sağ tıklayıp **tuvale Ekle**' yi seçerek grafik runbook 'una bir etkinlik ekleyin.

![Tuvale sertifika ekleme](../media/certificates/automation-certificate-add-to-canvas.png)

Aşağıdaki görüntüde bir grafik runbook 'unda sertifika kullanmanın bir örneği gösterilmektedir. Bu, bir metin runbook 'tan bir bulut hizmetine nasıl sertifika ekleneceğini gösteren önceki örnekle aynıdır.

![Örnek grafik yazma](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python-2-example"></a>Python 2 örneği

Aşağıdaki örnek, Python2 runbook 'larda sertifikalara nasıl erişegösterdiğini gösterir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook 'larınız tarafından gerçekleştirilen etkinliklerin mantıksal akışını denetlemek için bağlantılarla çalışma hakkında daha fazla bilgi edinmek için bkz. [grafik yazma Içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow). 
