---
title: Azure Otomasyonu 'nda Python 2 paketlerini yönetme
description: Bu makalede, Azure Automation 'da Python 2 paketlerinin nasıl yönetileceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2019
ms.topic: conceptual
ms.custom: tracking-python
ms.openlocfilehash: f3ba52c1396928d8c76fb85fda3f29c625e60919
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84561860"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Azure Otomasyonu 'nda Python 2 paketlerini yönetme

Azure Otomasyonu, Azure 'da ve Linux karma runbook çalışanları üzerinde Python 2 runbook çalıştırmanızı sağlar. Runbook 'ların basitleştirmesi konusunda yardımcı olmak için, gereksinim duyduğunuz modülleri içeri aktarmak üzere Python paketlerini kullanabilirsiniz. Bu makalede, Azure Automation 'da Python paketlerinin nasıl yönetileceği ve kullanılacağı açıklanmaktadır.

## <a name="import-packages"></a>Paketleri içeri aktarma

Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **Python 2 paketleri** ' ni seçin. **+ Python 2 paketi Ekle**' ye tıklayın.

![Python paketi Ekle](media/python-packages/add-python-package.png)

Python 2 paketi Ekle sayfasında, karşıya yüklenecek yerel bir paket seçin. Paket bir **. WHL** veya **. tar. gz** dosyası olabilir. Paket seçildiğinde, yüklemek için **Tamam** ' ı tıklatın.

![Python paketi Ekle](media/python-packages/upload-package.png)

Paket içeri aktarıldıktan sonra, Otomasyon hesabınızdaki Python 2 paketleri sayfasında listelenir. Bir paketi kaldırmanız gerekiyorsa, paketi seçin ve **Sil**' e tıklayın.

![Paket listesi](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>Bağımlılıkları olan paketleri içeri aktarma

Azure Otomasyonu, içeri aktarma işlemi sırasında Python paketlerinin bağımlılıklarını çözmez. Tüm bağımlılıklarıyla bir paketi içeri aktarmanın iki yolu vardır. Paketleri Otomasyon hesabınıza aktarmak için aşağıdaki adımlardan yalnızca birinin kullanılması gerekir.

### <a name="manually-download"></a>El ile indir

[Python 2.7](https://www.python.org/downloads/release/latest/python2) ve [PIP](https://pip.pypa.io/en/stable/) yüklü bir Windows 64-bit makinede, bir paketi ve tüm bağımlılıklarını indirmek için aşağıdaki komutu çalıştırın:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Paketler indirildikten sonra Otomasyon hesabınıza içeri aktarabilirsiniz.

### <a name="runbook"></a>Runbook

 Bir runbook almak için, [Pypı 'Den Otomasyon hesabınıza Python 2 paketlerini Azure Otomasyonu hesabına aktarın](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) . Çalıştırma ayarlarının **Azure** olarak ayarlandığından emin olun ve Runbook 'u parametrelerle başlatın. Runbook, Otomasyon hesabının çalışması için bir farklı çalıştır hesabı gerektirir. Her parametre için, aşağıdaki listede ve görüntüde görüldüğü gibi anahtarı ile başlattığınızdan emin olun:

* -s\<subscriptionId\>
* -g\<resourceGroup\>
* -a\<automationAccount\>
* -a\<modulePackage\>

![Paket listesi](media/python-packages/import-python-runbook.png)

Runbook, hangi paketin indirileceği belirtmenizi sağlar. Örneğin, `Azure` parametresinin kullanımı tüm Azure modüllerini ve tüm bağımlılıkları indirir (yaklaşık 105).

Runbook tamamlandıktan sonra, paketin doğru bir şekilde içeri aktarıldığını doğrulamak için Otomasyon hesabınızdaki **paylaşılan kaynaklar** altında **Python 2 paketlerini** kontrol edebilirsiniz.

## <a name="use-a-package-in-a-runbook"></a>Runbook 'ta paket kullanma

İçeri aktarılmış bir paket ile runbook 'ta kullanabilirsiniz. Aşağıdaki örnek [Azure Otomasyonu yardımcı programı paketini](https://github.com/azureautomation/azure_automation_utility)kullanır. Bu paket, Azure Otomasyonu ile Python kullanmayı kolaylaştırır. Paketi kullanmak için GitHub deposundaki yönergeleri izleyin ve Runbook 'a ekleyin. Örneğin, `from azure_automation_utility import get_automation_runas_credential` Farklı Çalıştır hesabını almak için işlevini içeri aktarmak için kullanabilirsiniz.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Runbook 'ları çevrimdışı geliştirme ve test etme

Python 2 runbook 'larınızı çevrimdışı ortamda geliştirmek ve test etmek için GitHub 'da [Azure Otomasyonu Python öykünmesi varlıkları](https://github.com/azureautomation/python_emulated_assets) modülünü kullanabilirsiniz. Bu modül kimlik bilgileri, değişkenler, bağlantılar ve sertifikalar gibi paylaşılan kaynaklarınıza başvurmasına olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Bir Python runbook 'u hazırlamak için bkz. [Python runbook oluşturma](learn/automation-tutorial-runbook-textual-python2.md).
