---
title: Azure Otomasyonu 'nda bağlantıları yönetme
description: Azure Otomasyonu 'ndaki bağlantı varlıkları, bir runbook veya DSC yapılandırmasından bir dış hizmete veya uygulamaya bağlanmak için gereken bilgileri içerir. Bu makalede, bağlantıların ayrıntıları ve hem metin hem de grafik yazarken bunlarla nasıl çalışılacağı açıklanmaktadır.
services: automation
ms.subservice: shared-capabilities
ms.date: 01/13/2020
ms.topic: conceptual
ms.openlocfilehash: 39a41a60f4cabe995ebd458c4b906438d1e31bde
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82097124"
---
# <a name="manage-connections-in-azure-automation"></a>Azure Otomasyonu 'nda bağlantıları yönetme

Bir Otomasyon bağlantı varlığı, bir runbook veya DSC yapılandırmasından bir dış hizmete veya uygulamaya bağlanmak için gereken bilgileri içerir. Bu, URL veya bağlantı noktası gibi bağlantı bilgilerine ek olarak Kullanıcı adı ve parola gibi kimlik doğrulaması için gereken bilgileri içerebilir. Bir bağlantının değeri, birden çok değişken oluşturma aksine, tek bir varlık içindeki belirli bir uygulamaya bağlanmak için tüm özelliklerin tutulmasını sağlar. Kullanıcı bir bağlantının değerlerini tek bir yerde düzenleyebilir ve bir bağlantı adını tek bir parametrede runbook veya DSC yapılandırmasına geçirebilirsiniz. `Get-AutomationConnection` Etkinlik ile RUNBOOK veya DSC yapılandırmasından bir bağlantının özelliklerine erişilebilir.

Bir bağlantı oluşturduğunuzda bir bağlantı türü belirtmeniz gerekir. Bağlantı türü bir özellikler kümesini tanımlayan bir şablondur. Bağlantı, bağlantı türünde tanımlanan her bir özellik için değerleri tanımlar. Tümleştirme modülü bir bağlantı türü içeriyorsa ve otomasyon hesabınıza içeri aktarıldıysa, bağlantı türleri tümleştirme modüllerinde Azure Otomasyonu 'na eklenir veya [Azure Otomasyonu API 'si](/previous-versions/azure/reference/mt163818(v=azure.100)) ile oluşturulur. Aksi takdirde, bir Otomasyon bağlantı türü belirtmek için bir meta veri dosyası oluşturmanız gerekir. Bunun hakkında daha fazla bilgi için bkz. [tümleştirme modülleri](automation-integration-modules.md).

>[!NOTE]
>Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Bu anahtar, sistem tarafından yönetilen bir Key Vault depolanır. Güvenli bir varlık depolamadan önce, anahtar Key Vault yüklenir ve ardından varlığı şifrelemek için kullanılır. Bu işlem Azure Otomasyonu tarafından yönetiliyor.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="connection-types"></a>Bağlantı türleri

Azure Otomasyonu 'nda üç tür yerleşik bağlantı mevcuttur:

* **Azure** -Bu bağlantı, klasik kaynakları yönetmek için kullanılabilir.
* **AzureClassicCertificate** -Bu bağlantı **AzureClassicRunAs** hesabı tarafından kullanılır.
* **AzureServicePrincipal** -Bu bağlantı **Azurerunas** hesabı tarafından kullanılır.

Çoğu durumda, bir [Farklı Çalıştır hesabı](manage-runas-account.md)oluşturduğunuzda oluşturulduğundan bir bağlantı kaynağı oluşturmanız gerekmez.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell cmdlet’leri

Aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile otomasyon bağlantıları oluşturmak ve yönetmek için kullanılır. Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanılabilecek [Azure PowerShell modülünün](/powershell/azure/overview)bir parçası olarak gönderilir.

|Cmdlet|Açıklama|
|---|---|
|[Get-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationconnection?view=azps-3.7.0)|Bir bağlantı alır. Bağlantı alanlarının değerlerini içeren bir Hashtable içerir.|
|[New-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection?view=azps-3.7.0)|Yeni bir bağlantı oluşturur.|
|[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationConnection?view=azps-3.7.0)|Varolan bir bağlantıyı kaldırır.|
|[Set-AzAutomationConnectionFieldValue](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationConnectionFieldValue?view=azps-3.7.0)|Mevcut bir bağlantı için belirli bir alanın değerini ayarlar.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler bir runbook veya DSC yapılandırmasındaki bağlantılara erişmek için kullanılır.

|Etkinlikler|Açıklama|
|---|---|
|`Get-AutomationConnection` | Kullanmak için bir bağlantı alır. Bağlantının özelliklerine sahip bir karma tablosu döndürür.|

>[!NOTE]
>`Name` Parametresi ile değişkenleri kullanmaktan kaçının `Get-AutomationConnection`. Bu parametrenin kullanılması runbook 'lar ya da DSC yapılandırma ve tasarım zamanında bağlantı varlıkları arasındaki bağımlılıkların bulunmasını karmaşıklaştırır.

## <a name="python-2-functions"></a>Python 2 işlevleri

Aşağıdaki tablodaki işlev, bir Python 2 runbook 'taki bağlantılara erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| `automationassets.get_automation_connection` | Bir bağlantı alır. Bağlantının özelliklerine sahip bir sözlük döndürür. |

> [!NOTE]
> Varlık işlevlerine erişmek için `automationassets` Python runbook 'unun en üstündeki modülünü içeri aktarmanız gerekir.

## <a name="creating-a-new-connection"></a>Yeni bağlantı oluşturma

### <a name="create-a-new-connection-with-the-azure-portal"></a>Azure portal yeni bir bağlantı oluşturun

1. Otomasyon hesabınızdan **varlıklar bölümüne tıklayarak** **varlıklar** dikey penceresini açın.
2. **Bağlantılar dikey penceresini** açmak için **Bağlantılar** bölümüne tıklayın.
3. Dikey pencerenin üst kısmındaki **bağlantı ekle** ' ye tıklayın.
4. **Tür** açılır listesinde, oluşturmak istediğiniz bağlantı türünü seçin. Form, bu belirli tür için özellikler sunacaktır.
5. Formu doldurun ve yeni bağlantıyı kaydetmek için **Oluştur** ' a tıklayın.

### <a name="create-a-new-connection-with-windows-powershell"></a>Windows PowerShell ile yeni bir bağlantı oluşturma

`New-AzAutomationConnection` Cmdlet 'Ini kullanarak Windows PowerShell ile yeni bir bağlantı oluşturun. Bu cmdlet, bağlantı türü tarafından `ConnectionFieldValues` tanımlanan her bir özellik için değerleri tanımlayan bir [Hashtable](https://technet.microsoft.com/library/hh847780.aspx) bekleyen adlı bir parametreye sahiptir.

Yeni bir bağlantı varlığı oluşturmak için portaldan farklı çalıştır hesabı oluşturmaya alternatif olarak aşağıdaki örnek komutları kullanabilirsiniz.

```powershell
$ConnectionAssetName = "AzureRunAsConnection"
$ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
New-AzAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
```

Otomasyon hesabınızı oluşturduğunuzda, `AzureServicePrincipal` `AzureRunAsConnection` bağlantı varlığını oluşturmak için bağlantı türü ile birlikte, varsayılan olarak çeşitli genel modüller vardır. Farklı bir kimlik doğrulama yöntemiyle bir hizmete veya uygulamaya bağlanmak için yeni bir bağlantı varlığı oluşturmaya çalışırsanız, bağlantı türü Otomasyon hesabınızda zaten tanımlanmadığı için işlem başarısız olur. Özel bir [PowerShell Galerisi](https://www.powershellgallery.com) modülü için kendi bağlantı türünü oluşturma hakkında daha fazla bilgi için bkz. [tümleştirme modülleri](automation-integration-modules.md).

## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Runbook veya DSC yapılandırmasında bağlantı kullanma

`Get-AutomationConnection` Cmdlet 'i ile RUNBOOK veya DSC yapılandırmasında bir bağlantı alın. `Get-AzAutomationConnection` Etkinliği kullanamazsınız. Bu etkinlik, bağlantıdaki farklı alanların değerlerini alır ve bunları bir [karma tablosu](https://go.microsoft.com/fwlink/?LinkID=324844)olarak döndürür. Bu Hashtable daha sonra runbook veya DSC yapılandırmasındaki uygun komutlarla birlikte kullanılabilir.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek komutlarda, runbook 'larınızda Azure Resource Manager kaynaklarla kimlik doğrulamak için daha önce bahsedilen farklı çalıştır hesabının nasıl kullanılacağı gösterilmektedir. Kimlik bilgilerine değil, sertifika tabanlı hizmet sorumlusuna başvuran farklı çalıştır hesabını temsil eden bağlantı varlığını kullanır.

```powershell
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
```

> [!NOTE]
> Grafik olmayan PowerShell runbook 'ları için `Add-AzAccount` ve `Add-AzureRMAccount` [Connect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0)için diğer adlar. Bu cmdlet 'leri kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleştirebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncelleştirmeniz gerekebilir.

### <a name="graphical-runbook-samples"></a>Grafik runbook örnekleri

Grafik düzenleyicinin kitaplık `Get-AutomationConnection` bölmesinde bağlantıya sağ tıklayıp **tuvale Ekle**' yi seçerek bir grafik runbook 'una etkinlik eklersiniz.

![tuvale Ekle](media/automation-connections/connection-add-canvas.png)

Aşağıdaki görüntüde bir grafik runbook 'unda bağlantı kullanmanın bir örneği gösterilmektedir. Bu, bir metinsel runbook ile farklı çalıştır hesabını kullanarak kimlik doğrulaması yapmak için yukarıda gösterilenle aynı örneğidir. Bu örnek, `Constant value` kimlik doğrulaması için bir bağlantı `Get RunAs Connection` nesnesi kullanan etkinliğin veri kümesini kullanır. Parametre kümesi tek bir nesne beklediği için `ServicePrincipalCertificate` burada bir işlem [hattı bağlantısı](automation-graphical-authoring-intro.md#links-and-workflow) kullanılır.

![bağlantıları al](media/automation-connections/automation-get-connection-object.png)

### <a name="python-2-runbook-sample"></a>Python 2 runbook örneği

Aşağıdaki örnek, bir Python 2 runbook 'unda farklı çalıştır bağlantısını kullanarak nasıl kimlik doğrulaması yapılacağını gösterir.

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

- Runbook 'larınızda Logic Flow 'un nasıl yönlendirileceğini ve kontrol olduğunu anlamak için [grafik yazarken bağlantıları](automation-graphical-authoring-intro.md#links-and-workflow) gözden geçirin.
* PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
- Azure Otomasyonu 'nun PowerShell modüllerinin kullanımı ve Azure Otomasyonu 'nda tümleştirme modülleri olarak çalışmak üzere kendi PowerShell modüllerinizi oluşturmak için en iyi uygulamalar hakkında daha fazla bilgi edinmek için bkz. [tümleştirme modülleri](automation-integration-modules.md).