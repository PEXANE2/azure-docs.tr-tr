---
title: Azure Otomasyonu'ndaki bağlantı varlıkları
description: Azure Otomasyonu'ndaki bağlantı varlıkları, bir runbook veya DSC yapılandırmasından harici bir hizmete veya uygulamaya bağlanmak için gereken bilgileri içerir. Bu makalede, bağlantıların ayrıntıları ve hem metinsel hem de grafiksel yazarlıkta onlarla nasıl çalışılabilenler açıklanmaktadır.
services: automation
ms.subservice: shared-capabilities
ms.date: 01/13/2020
ms.topic: conceptual
ms.openlocfilehash: b6276153921feb0e6f27194d36d1c32c1d0ffb3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75940825"
---
# <a name="connection-assets-in-azure-automation"></a>Azure Otomasyonu'ndaki bağlantı varlıkları

Otomasyon bağlantı varlığı, bir runbook veya DSC yapılandırmasından harici bir hizmete veya uygulamaya bağlanmak için gereken bilgileri içerir. Bu, URL veya bağlantı noktası gibi bağlantı bilgilerine ek olarak kullanıcı adı ve parola gibi kimlik doğrulama için gereken bilgileri içerebilir. Bir bağlantının değeri, birden çok değişken oluşturmak yerine belirli bir uygulamaya bağlanmaözelliklerinin tümünü tek bir kıymette tutmaktır. Kullanıcı bir bağlantının değerlerini tek bir yerde ayarlayabilir ve bir bağlantının adını tek bir parametrede runbook'a veya DSC yapılandırmasına geçirebilirsiniz. Bağlantı özelliklerine **Get-AutomationConnection** etkinliği yle runbook veya DSC yapılandırmasında erişilebilir.

Bir bağlantı oluşturduğunuzda, bir *bağlantı türü*belirtmeniz gerekir. Bağlantı türü, bir özellik kümesini tanımlayan bir şablondur. Bağlantı, bağlantı türünde tanımlanan her özellik için değerleri tanımlar. Bağlantı türleri, tümleştirme modüllerinde Azure Otomasyonu'na eklenir veya tümleştirme modülü bir bağlantı türü içeriyorsa ve Otomasyon hesabınıza aktarılıyorsa [Azure Otomasyon API'si](/previous-versions/azure/reference/mt163818(v=azure.100)) ile oluşturulur. Aksi takdirde, bir Otomasyon bağlantı türü belirtmek için bir meta veri dosyası oluşturmanız gerekir. Bu konuda daha fazla bilgi için [Entegrasyon Modülleri'ne](automation-integration-modules.md)bakın.

>[!NOTE]
>Azure Otomasyonu'ndaki güvenli varlıklar kimlik bilgilerini, sertifikaları, bağlantıları ve şifreli değişkenleri içerir. Bu varlıklar, her otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu anahtar, anahtar kasa yönetilen bir sistemde saklanır. Güvenli bir kıymeti depolamadan önce, anahtar Key Vault'tan yüklenir ve sonra varlığı şifrelemek için kullanılır. Bu işlem Azure Automation tarafından yönetilir.

## <a name="connection-types"></a>Bağlantı türleri

Azure Otomasyonu'nda üç tür yerleşik bağlantı vardır:

* **Azure** - Bu bağlantı klasik kaynakları yönetmek için kullanılabilir.
* **AzureClassicCertificate** - Bu bağlantı **AzureClassicRunAs** hesabı tarafından kullanılır.
* **AzureServicePrincipal** - Bu bağlantı **AzureRunAs** hesabı tarafından kullanılır.

Çoğu durumda, bir [RunAs hesabı](manage-runas-account.md)oluşturduğunuzda oluşturulduğu için bir bağlantı kaynağı oluşturmanız gerekmez.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell Cmdlet'leri

Aşağıdaki tablodaki cmdletler Windows PowerShell ile Otomasyon bağlantıları oluşturmak ve yönetmek için kullanılır. Otomasyon runbook'larında ve DSC yapılandırmalarında kullanılabilen [Azure PowerShell modülünün](/powershell/azure/overview)bir parçası olarak gönderiyaparlar.

|Cmdlet|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Bir bağlantı alır. Bağlantı alanlarının değerlerini içeren bir karma tablo içerir.|
|[New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Yeni bir bağlantı oluşturur.|
|[Remove-AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Mevcut bir bağlantıyı kaldırır.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Mevcut bir bağlantı için belirli bir alanın değerini ayarlar.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler, bir runbook veya DSC yapılandırmasındaki bağlantılara erişmek için kullanılır.

|Etkinlikler|Açıklama|
|---|---|
|Get-AutomationConnection | Kullanmak için bir bağlantı alır. Bağlantı özellikleriyle bir karma tablo döndürür.|

>[!NOTE]
>Runbook'lar veya DSC yapılandırmaları ve bağlantı varlıkları arasındaki bağımlılıkları keşfetmeyi zorlaştırabileceğinden, **Get-AutomationConnection'ın** -Ad parametresi olan değişkenleri kullanmaktan kaçınmalısınız.


## <a name="python2-functions"></a>Python2 fonksiyonları

Aşağıdaki tablodaki işlev Python2 runbook'taki bağlantılara erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_connection | Bir bağlantı alır. Bağlantının özelliklerini içeren bir sözlük döndürür. |

> [!NOTE]
> Varlık işlevlerine erişmek için Python runbook'unuzun en üstündeki "otomasyon varlıkları" modülünün içe aktarılaması gerekir.

## <a name="creating-a-new-connection"></a>Yeni bir Bağlantı Oluşturma

### <a name="to-create-a-new-connection-with-the-azure-portal"></a>Azure portalı ile yeni bir bağlantı oluşturmak için

1. Otomasyon hesabınızdan **Varlıklar** bıyıkını açmak için **Varlıklar** bölümünü tıklatın.
2. **Bağlantılar** bıçağını açmak için **Bağlantılar** bölümünü tıklatın.
3. Bıçağın üst kısmında **ki bağlantı ekle'yi** tıklatın.
4. **Tür** açılır düşüşünde, oluşturmak istediğiniz bağlantı türünü seçin. Form, belirli bir türiçin özellikleri sunacak.
5. Formu doldurun ve yeni bağlantıyı kaydetmek için **Oluştur'u** tıklatın.

### <a name="to-create-a-new-connection-with-windows-powershell"></a>Windows PowerShell ile yeni bir bağlantı oluşturmak için

[Yeni AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) cmdlet'ini kullanarak Windows PowerShell ile yeni bir bağlantı oluşturun. Bu cmdlet bağlantı türü tarafından tanımlanan özelliklerin her biri için değerleri tanımlayan bir [karma tablo](https://technet.microsoft.com/library/hh847780.aspx) bekleyen **ConnectionFieldValues** adlı bir parametre vardır.

Hizmet anasını kullanarak runbook'ları doğrulamak için Otomasyon [Çalıştır hesabını](automation-sec-configure-azure-runas-account.md) biliyorsanız, portaldan Run As hesabı oluşturmak için alternatif olarak sağlanan PowerShell komut dosyası, aşağıdaki örnek komutları kullanarak yeni bir bağlantı varlığı oluşturur.

```powershell
$ConnectionAssetName = "AzureRunAsConnection"
$ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
```

Bağlantı varlığını oluşturmak için komut dosyasını kullanabilirsiniz, çünkü Otomasyon hesabınızı oluşturduğunuzda, **AzureRunAsConnection** bağlantı varlığını oluşturmak için **AzureServicePrincipal** bağlantı türüyle birlikte varsayılan olarak birkaç küresel modülü otomatik olarak içerir. Bunu göz önünde bulundurmanız önemlidir, çünkü bir hizmete veya uygulamaya farklı bir kimlik doğrulama yöntemiyle bağlanmak için yeni bir bağlantı varlığı oluşturmaya çalışırsanız, bağlantı türü Otomasyon hesabınızda zaten tanımlanmadığından başarısız olur. [PowerShell Galerisi'nden](https://www.powershellgallery.com)özel veya modülünüz için kendi bağlantı türünüz hakkında daha fazla bilgi için [Entegrasyon Modüllerine](automation-integration-modules.md) bakın

## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Runbook veya DSC yapılandırmasında bağlantı kullanma

**Get-AutomationConnection** cmdlet ile bir runbook veya DSC yapılandırmasında bir bağlantı alırsınız. [AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection) etkinliğini kullanamazsınız. Bu etkinlik, bağlantıdaki farklı alanların değerlerini alır ve bunları, runbook veya DSC yapılandırmasındaki uygun komutlarla kullanılabilecek karma [tablo](https://go.microsoft.com/fwlink/?LinkID=324844)olarak döndürür.

### <a name="textual-runbook-sample"></a>Metin çalışma kitabı örneği

Aşağıdaki örnek komutlar, runbook'unuzdaki Azure Kaynak Yöneticisi kaynaklarıyla kimlik doğrulaması yapmak için daha önce bahsedilen Çalıştır hesabının nasıl kullanılacağını gösterir. Kimlik bilgilerine değil, sertifika tabanlı hizmet ilkesine başvuran Run As hesabını temsil eden bağlantı varlığını kullanır.

```powershell
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
```

> [!IMPORTANT]
> **Add-AzureRmAccount** artık **Connect-AzureRMAccount**takma adıdır. Kitaplık öğelerinizde arama yaparken, **Connect-AzureRMAccount'u** **görmüyorsanız, Ekle-AzureRmAccount'u**kullanabilir veya modüllerinizi Otomasyon Hesabınızda güncelleyebilirsiniz.

### <a name="graphical-runbook-samples"></a>Grafik çalışma kitabı örnekleri

Grafik düzenleyicisinin **Kitaplık** bölmesindeki bağlantıya sağ tıklayarak ve **tuvale Ekle'yi**seçerek grafik çalışma kitabına **Get-AutomationConnection** etkinliği eklersiniz.

![tuvale ekle](media/automation-connections/connection-add-canvas.png)

Aşağıdaki resim, grafik çalışma kitabında bağlantı kullanma örneğini gösterir. Bu, metinsel bir runbook ile Çalıştır hesabının kimlik doğrulamasını kullanmak için yukarıda gösterilen örnekle aynıdır. Bu örnek, kimlik doğrulaması için bir bağlantı nesnesi kullanan **RunAs Bağlantısı Al** etkinliği için **Sabit değer** veri kümesini kullanır. ServicePrincipalCertificate parametre kümesi tek bir nesne beklediğinden, burada bir [boru hattı bağlantısı](automation-graphical-authoring-intro.md#links-and-workflow) kullanılır.

![bağlantı kurma](media/automation-connections/automation-get-connection-object.png)

### <a name="python2-runbook-sample"></a>Python2 runbook örneği

Aşağıdaki örnek, Python2 runbook'ta Çalıştır Bağlantısı kullanılarak nasıl kimlik doğrulamasının yapılacağını gösterir.

```python
""" Tutorial to show how to authenticate against Azure resource manager resources """
import azure.mgmt.resource
import automationassets

def get_automation_runas_credential(runas_connection):
    """ Returns credentials to authenticate against Azure resoruce manager """
    from OpenSSL import crypto
    from msrestazure import azure_active_directory
    import adal

    # Get the Azure Automation Run As service principal certificate
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
    pks12_cert = crypto.load_pkcs12(cert)
    pem_pkey = crypto.dump_privatekey(
        crypto.FILETYPE_PEM, pks12_cert.get_privatekey())

    # Get Run As connection information for the Azure Automation service principal
    application_id = runas_connection["ApplicationId"]
    thumbprint = runas_connection["CertificateThumbprint"]
    tenant_id = runas_connection["TenantId"]

    # Authenticate with service principal certificate
    resource = "https://management.core.windows.net/"
    authority_url = ("https://login.microsoftonline.com/" + tenant_id)
    context = adal.AuthenticationContext(authority_url)
    return azure_active_directory.AdalAuthentication(
        lambda: context.acquire_token_with_client_certificate(
            resource,
            application_id,
            pem_pkey,
            thumbprint)
    )


# Authenticate to Azure using the Azure Automation Run As service principal
runas_connection = automationassets.get_automation_connection(
    "AzureRunAsConnection")
azure_credential = get_automation_runas_credential(runas_connection)
```

## <a name="next-steps"></a>Sonraki adımlar

- Runbook'larınızdaki mantık akışını nasıl yönlendirip denetleyeniz gerektiğini anlamak için [grafik yazarlıktaki Bağlantıları](automation-graphical-authoring-intro.md#links-and-workflow) gözden geçirin.

- Azure Otomasyonu'nun PowerShell modüllerini kullanması ve Azure Otomasyonu içinde Entegrasyon Modülleri olarak çalışmak üzere kendi PowerShell modüllerinizi oluşturmak için en iyi uygulamalar hakkında daha fazla bilgi edinmek [için, Entegrasyon Modüllerine](automation-integration-modules.md)bakın.