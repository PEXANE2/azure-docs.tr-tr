---
title: Azure Otomasyonu'nda sertifikayı yönetme
description: Sertifikalar Azure Otomasyonu'nda güvenli bir şekilde depolanır, böylece runbook'lar veya DSC yapılandırmaları Azure ve üçüncü taraf kaynaklarına karşı kimlik doğrulamalarına erişebilir. Bu makalede, sertifikaların ayrıntıları ve hem metinsel hem de grafiksel yazarlıkta onlarla nasıl çalışılabilenler açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 69bc1a0333365e15452c6d3b253266d37d99b608
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732820"
---
# <a name="manage-certificates-in-azure-automation"></a>Azure Otomasyonu'nda sertifikaları yönetme

Sertifikalar Azure Kaynak Yöneticisi kaynakları için [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) etkinliğini kullanarak runbook'lar veya DSC yapılandırmaları tarafından erişilebilmeleri için Azure Otomasyonu'nda güvenli bir şekilde depolanır. Güvenli sertifika depolama, kimlik doğrulaması için sertifikaları kullanan veya Bunları Azure veya üçüncü taraf kaynaklarına ekleyen runbook'lar ve DSC yapılandırmaları oluşturmanıza olanak tanır.

Azure Otomasyonu'ndaki güvenli varlıklar kimlik bilgilerini, sertifikaları, bağlantıları ve şifreli değişkenleri içerir. Bu varlıklar, her otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu anahtar, sistem tarafından yönetilen key vault'ta saklanır. Güvenli bir kıymeti depolamadan önce, anahtar Key Vault'tan yüklenir ve sonra varlığı şifrelemek için kullanılır. Bu işlem Azure Automation tarafından yönetilir.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="az-powershell-cmdlets"></a>Az PowerShell cmdlets

Az için, aşağıdaki tablodaki cmdletler Windows PowerShell ile otomasyon kimlik bilgileri varlıkları oluşturmak ve yönetmek için kullanılır. Otomasyon runbook'larında ve DSC yapılandırmalarında kullanılabilen [Az.Automation modülünün](/powershell/azure/overview)bir parçası olarak gönderi yaparlar.

|Cmdlet |Açıklama|
| --- | ---|
|[Add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate)|Belirtilen bulut hizmeti için bir hizmet sertifikası yükler.|
|[Al-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0)|Bir runbook veya DSC yapılandırmasında kullanılacak bir sertifika hakkındaki bilgileri alır. Yalnızca `Get-AutomationCertificate` etkinliği kullanarak sertifikanın kendisini alabilirsiniz.|
|[Yeni-AzAutomationSertifikası](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.7.0)|Azure Otomasyonu'nda yeni bir sertifika oluşturur.|
|[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationCertificate?view=azps-3.7.0)|Azure Otomasyonundan bir sertifika kaldırır.|
|[Set-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationCertificate?view=azps-3.7.0)|Sertifika dosyasını yükleme ve **.pfx** dosyasının parolasını ayarlama dahil olmak üzere varolan bir sertifikanın özelliklerini ayarlar.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler, bir runbook ve DSC yapılandırmalarında sertifikalara erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|`Get-AutomationCertificate`|Runbook veya DSC yapılandırmasında kullanılacak bir sertifika alır. [System.Security.Cryptography.X509Certificates.X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) nesnesini döndürür.|

> [!NOTE] 
> Bir runbook veya DSC `Name` `Get-AutomationCertificate` yapılandırmaparametresinde değişkenleri kullanmaktan kaçınmalısınız. Bu parametredeki değişkenlerin kullanımı, tasarım zamanında runbook'lar veya DSC yapılandırmaları ile Otomasyon değişkenleri arasındaki bağımlılıkların keşfini zorlaştırır.

## <a name="python-2-functions"></a>Python 2 fonksiyonları

Aşağıdaki tablodaki işlev Python 2 runbook'taki sertifikalara erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| `automationassets.get_automation_certificate` | Sertifika kıymeti hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine `automationassets` erişmek için modülü Python runbook'unuzun başında içe aktarmanız gerekir.

## <a name="creating-a-new-certificate"></a>Yeni bir sertifika oluşturma

Yeni bir sertifika oluşturduğunuzda, Azure Otomasyonu'na bir .cer veya .pfx dosyası yüklersiniz. Sertifikayı dışa aktarılabilir olarak işaretlerseniz, sertifikayı Azure Otomasyon sertifika deposundan aktarabilirsiniz. Dışa aktarılamıyorsa, yalnızca runbook veya DSC yapılandırması içinde imzalamak için kullanılabilir. Azure Otomasyonu, sertifikanın **Microsoft Enhanced RSA ve AES Şifreleme Sağlayıcısı**sağlayıcısına sahip olmasını gerektirir.

### <a name="create-a-new-certificate-with-the-azure-portal"></a>Azure portalı ile yeni bir sertifika oluşturun

1. Otomasyon hesabınızdan Varlıklar sayfasını açmak için **Varlıklar'ı** tıklatın.
2. Sertifikalar sayfasını açmak için **Sertifikalar'ı** seçin.
3. Sayfanın üst kısmında **sertifika ekle'yi** tıklatın.
4. **Ad** alanına sertifika için bir ad yazın.
5. **.cer** veya **.pfx** dosyasına göz atmak için **sertifika dosyası yükle'nin**altındaki **dosyayı seçin'i** tıklatın. **Bir .pfx**dosyası seçerseniz, bir parola belirtin ve dışa aktarılabiliyor mu belirtin.
6. Yeni sertifika varlığını kaydetmek için **Oluştur'u** tıklatın.

### <a name="create-a-new-certificate-with-powershell"></a>PowerShell ile yeni bir sertifika oluşturun

Aşağıdaki örnek, yeni bir Otomasyon sertifikasının nasıl oluşturulup dışa aktarılabilir olarak işaretlenebildiğini gösterir. Bu örnek varolan bir **.pfx** dosyayı içeri aktarır.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla yeni bir sertifika oluşturma

Aşağıdaki örnek, PowerShell aracılığıyla bir Kaynak Yöneticisi şablonu kullanarak bir sertifikanın Otomasyon hesabınıza nasıl dağıtılabildiğini gösterir:

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

Sertifika kullanmak için `Get-AutomationCertificate` etkinliği kullanın. Sertifika varlığı yla ilgili bilgileri döndürdediğinden, sertifikanın kendisi hakkında bilgi döndüremediği için [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) cmdlet'i kullanamazsınız.

### <a name="textual-runbook-example"></a>Metin çalışma kitabı örneği

Aşağıdaki örnek, bir runbook'taki bulut hizmetine nasıl sertifika ekleyeceğinizgöster. Bu örnekte, parola şifreli bir otomasyon değişkeninden alınır.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-example"></a>Grafik çalışma kitabı örneği

Kitaplık `Get-AutomationCertificate` bölmesindeki sertifikaya sağ tıklayarak ve **tuvale Ekle'yi**seçerek grafik runbook'a etkinlik ekleyin.

![Tuvale sertifika ekleme](../media/certificates/automation-certificate-add-to-canvas.png)

Aşağıdaki resim, grafik çalışma kitabında bir sertifika kullanma örneğini gösterir. Bu, metin seli bir runbook'tan bulut hizmetine nasıl sertifika ekleyeceğini gösteren önceki örnekle aynıdır.

![Örnek Grafik Selyazma](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python-2-example"></a>Python 2 örneği

Aşağıdaki örnek, Python2 runbook'larında sertifikalara nasıl erişilir gösterilmektedir.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook'unuzun gerçekleştirdiği etkinliklerin mantıksal akışını denetlemek için bağlantılarla çalışma hakkında daha fazla bilgi edinmek için [grafik yazarlıktaki Bağlantılar'a](../automation-graphical-authoring-intro.md#links-and-workflow)bakın. 
