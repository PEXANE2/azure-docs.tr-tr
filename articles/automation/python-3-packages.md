---
title: Azure Otomasyonu 'nda Python 3 paketlerini yönetme
description: Bu makalede, Azure Otomasyonu 'nda Python 3 paketlerinin (Önizleme) nasıl yönetileceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 02/19/2021
ms.topic: conceptual
ms.openlocfilehash: fd4d8ee92b670bc2544619a0dce16a26d9342c13
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102122043"
---
# <a name="manage-python-3-packages-preview-in-azure-automation"></a>Azure Otomasyonu 'nda Python 3 paketlerini yönetme (Önizleme)

Azure Otomasyonu, Azure 'da ve Linux karma runbook çalışanları üzerinde Python 3 runbook 'ları (Önizleme) çalıştırmanıza olanak tanır. Runbook 'ların basitleştirmesi konusunda yardımcı olmak için, gereksinim duyduğunuz modülleri içeri aktarmak üzere Python paketlerini kullanabilirsiniz. Tek bir paketi içeri aktarmak için bkz. [paketi Içeri aktarma](#import-a-package). Birden çok pakete sahip bir paketi içeri aktarmak için bkz. [bir paketi bağımlılıklarla Içeri aktarma](#import-a-package-with-dependencies). Bu makalede, Azure Otomasyonu 'nda Python 3 paketlerinin (Önizleme) nasıl yönetileceği ve kullanılacağı açıklanmaktadır.

## <a name="import-a-package"></a>Bir paketi içeri aktar

Otomasyon hesabınızda, **paylaşılan kaynaklar** altında **Python paketleri** ' ni seçin. **+ bir Python paketi Ekle**' yi seçin.

:::image type="content" source="media/python-3-packages/add-python-3-package.png" alt-text="Python 3 paketleri sayfasının ekran görüntüsü Sol menüdeki Python 3 paketlerini gösterir ve bir Python 2 paketi vurgulanmış olarak ekler.":::

**Python paketi Ekle** sayfasında **Sürüm** için **Python 3** ' ü seçin ve karşıya yüklenecek yerel bir paket seçin. Paket bir **. WHL** veya **. tar. gz** dosyası olabilir. Paket seçildiğinde, karşıya yüklemek için **Tamam** ' ı seçin.

:::image type="content" source="media/python-3-packages/upload-package.png" alt-text="Ekran görüntüsü, karşıya yüklenen bir tar. gz dosyası seçiliyken Python 3 paketi Ekle sayfasını gösterir.":::

Paket içeri aktarıldıktan sonra, **Python 3 paketleri (Önizleme)** sekmesinde Otomasyon hesabınızdaki Python paketleri sayfasında listelenir. Bir paketi kaldırmanız gerekiyorsa, paketi seçin ve **Sil**' e tıklayın.

:::image type="content" source="media/python-3-packages/python-3-packages-list.png" alt-text="Ekran görüntüsü, bir paket içe aktarıldıktan sonra Python 3 paketleri sayfasını gösterir.":::

### <a name="import-a-package-with-dependencies"></a>Bağımlılıkları olan bir paketi içeri aktarma

Aşağıdaki Python betiğini bir Python 3 runbook 'una aktararak ve sonra çalıştırarak bir Python 3 paketini ve bağımlılıklarını içeri aktarabilirsiniz.

```cmd
https://github.com/azureautomation/runbooks/blob/master/Utility/Python/import_py3package_from_pypi.py
```

#### <a name="importing-the-script-into-a-runbook"></a>Betiği runbook 'a aktarma
Runbook 'u içeri aktarma hakkında bilgi için bkz. [Azure Portal runbook 'U Içeri aktarma](manage-runbooks.md#import-a-runbook-from-the-azure-portal). Dosyayı GitHub 'dan, içeri aktarmayı çalıştırmadan önce portalın erişebileceği depolama alanına kopyalayın.

**Runbook 'U Içeri aktarma** sayfası, runbook adını betiğin adıyla eşleşecek şekilde varsayılan olarak ayarlar. Alana erişiminiz varsa, adı değiştirebilirsiniz. **Runbook türü** varsayılan olarak **Python 2**' dir. Varsa, **Python 3**' e değiştirdiğinizden emin olun.

:::image type="content" source="media/python-3-packages/import-python-3-package.png" alt-text="Ekran görüntüsü Python 3 runbook içeri aktarma sayfasını gösterir.":::

#### <a name="executing-the-runbook-to-import-the-package-and-dependencies"></a>Paketi ve bağımlılıkları içeri aktarmak için Runbook yürütülüyor

Runbook 'u oluşturup yayımladıktan sonra, paketi içeri aktarmak için çalıştırın. Runbook 'u çalıştırma hakkında ayrıntılı bilgi için bkz. [Azure Otomasyonu 'nda runbook başlatma](start-runbooks.md) .

Betik ( `import_py3package_from_pypi.py` ) aşağıdaki parametreleri gerektirir.

| Parametre | Açıklama |
|---------------|-----------------|
|subscription_id | Otomasyon hesabının abonelik KIMLIĞI |
| resource_group | Otomasyon hesabının tanımlandığı kaynak grubunun adı |
| automation_account | Otomasyon hesabı adı |
| module_name | İçeri aktarılacak modülün adı `pypi.org` |

Runbook 'larla parametreleri kullanma hakkında daha fazla bilgi için bkz. [work with runbook Parameters](start-runbooks.md#work-with-runbook-parameters).

## <a name="use-a-package-in-a-runbook"></a>Runbook 'ta paket kullanma

Paketi içeri aktarıldığında, bir runbook 'ta kullanabilirsiniz. Bir Azure aboneliğindeki tüm kaynak gruplarını listelemek için aşağıdaki kodu ekleyin.

```python
import os  
import azure.mgmt.resource  
import automationassets  

def get_automation_runas_credential(runas_connection):  
    from OpenSSL import crypto  
    import binascii  
    from msrestazure import azure_active_directory  
    import adal 

    # Get the Azure Automation RunAs service principal certificate  
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")  
    pks12_cert = crypto.load_pkcs12(cert)  
    pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())  

    # Get run as connection information for the Azure Automation service principal 
    application_id = runas_connection["ApplicationId"]  
    thumbprint = runas_connection["CertificateThumbprint"]  
    tenant_id = runas_connection["TenantId"]  

    # Authenticate with service principal certificate  
    resource ="https://management.core.windows.net/"  
    authority_url = ("https://login.microsoftonline.com/"+tenant_id)  
    context = adal.AuthenticationContext(authority_url)  
    return azure_active_directory.AdalAuthentication(  
    lambda: context.acquire_token_with_client_certificate(  
            resource,  
            application_id,  
            pem_pkey,  
            thumbprint) 
    ) 

# Authenticate to Azure using the Azure Automation RunAs service principal  
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")  
azure_credential = get_automation_runas_credential(runas_connection)  

# Intialize the resource management client with the RunAs credential and subscription  
resource_client = azure.mgmt.resource.ResourceManagementClient(  
    azure_credential,  
    str(runas_connection["SubscriptionId"]))  

# Get list of resource groups and print them out  
groups = resource_client.resource_groups.list()  
for group in groups:  
    print(group.name) 
```

## <a name="next-steps"></a>Sonraki adımlar

Bir Python runbook 'u hazırlamak için bkz. [Python runbook oluşturma](learn/automation-tutorial-runbook-textual-python-3.md).
