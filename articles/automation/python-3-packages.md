---
title: Azure Otomasyonu 'nda Python 3 paketlerini yönetme
description: Bu makalede, Azure Otomasyonu 'nda Python 3 paketlerinin (Önizleme) nasıl yönetileceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 12/22/2020
ms.topic: conceptual
ms.openlocfilehash: 3f39f49ff47b935da7ffc777ee45bd219f5740b5
ms.sourcegitcommit: f7084d3d80c4bc8e69b9eb05dfd30e8e195994d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97734310"
---
# <a name="manage-python-3-packages-preview-in-azure-automation"></a>Azure Otomasyonu 'nda Python 3 paketlerini yönetme (Önizleme)

Azure Otomasyonu, Azure 'da ve Linux karma runbook çalışanları üzerinde Python 3 runbook 'ları (Önizleme) çalıştırmanıza olanak tanır. Runbook 'ların basitleştirmesi konusunda yardımcı olmak için, gereksinim duyduğunuz modülleri içeri aktarmak üzere Python paketlerini kullanabilirsiniz. Bu makalede, Azure Otomasyonu 'nda Python 3 paketlerinin (Önizleme) nasıl yönetileceği ve kullanılacağı açıklanmaktadır.

## <a name="import-packages"></a>Paketleri içeri aktarma

Otomasyon hesabınızda, **paylaşılan kaynaklar** altında **Python paketleri** ' ni seçin. **+ bir Python paketi Ekle**' yi seçin.

:::image type="content" source="media/python-3-packages/add-python-3-package.png" alt-text="Python 3 paketleri sayfasının ekran görüntüsü Sol menüdeki Python 3 paketlerini gösterir ve bir Python 2 paketi vurgulanmış olarak ekler.":::

**Python paketi Ekle** sayfasında **Sürüm** için Python 3 ' ü seçin ve karşıya yüklenecek yerel bir paket seçin. Paket bir **. WHL** veya **. tar. gz** dosyası olabilir. Paket seçildiğinde, karşıya yüklemek için **Tamam** ' ı seçin.

:::image type="content" source="media/python-3-packages/upload-package.png" alt-text="Ekran görüntüsü, karşıya yüklenen bir tar. gz dosyası seçiliyken Python 3 paketi Ekle sayfasını gösterir.":::

Paket içeri aktarıldıktan sonra, **Python 3 paketleri (Önizleme)** sekmesinde Otomasyon hesabınızdaki Python paketleri sayfasında listelenir. Bir paketi kaldırmanız gerekiyorsa, paketi seçin ve **Sil**' e tıklayın.

:::image type="content" source="media/python-3-packages/python-3-packages-list.png" alt-text="Ekran görüntüsü, bir paket içe aktarıldıktan sonra Python 3 paketleri sayfasını gösterir.":::

## <a name="import-packages-with-dependencies"></a>Bağımlılıkları olan paketleri içeri aktarma

Azure Otomasyonu, içeri aktarma işlemi sırasında Python paketlerinin bağımlılıklarını çözmez. Ancak, tüm bağımlılıklarıyla bir paketi içeri aktarmanın bir yolu vardır.

### <a name="manually-download"></a>El ile indir

[Python 3,8](https://www.python.org/downloads/release/python-380/) ve [PIP](https://pip.pypa.io/en/stable/) yüklü bir Windows 64-bit makinesinde, bir paketi ve tüm bağımlılıklarını indirmek için aşağıdaki komutu çalıştırın:

```cmd
C:\Python38\Scripts\pip3.8.exe download -d <output dir> <package name>
```

Paketler indirildikten sonra Otomasyon hesabınıza içeri aktarabilirsiniz.

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
