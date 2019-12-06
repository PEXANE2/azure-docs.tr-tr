---
title: Azure Otomasyonu 'nda Python 2 paketlerini yönetme
description: Bu makalede, Azure Automation 'da Python 2 paketlerinin nasıl yönetileceği açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f20865b92df2a197410f209cf921c5e573723286
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850202"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Azure Otomasyonu 'nda Python 2 paketlerini yönetme

Azure Otomasyonu, Azure 'da ve Linux karma runbook çalışanları üzerinde Python 2 runbook çalıştırmanızı sağlar. Runbook 'ların basitleştirmesi konusunda yardımcı olmak için, gereksinim duyduğunuz modülleri içeri aktarmak üzere Python paketlerini kullanabilirsiniz. Bu makalede, Azure Otomasyonu 'nda Python paketlerini yönetme ve kullanma işlemlerinin nasıl yapılacağı açıklanır.

## <a name="import-packages"></a>Paketleri içeri aktarma

Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **Python 2 paketleri** ' ni seçin. **+ Python 2 paketi Ekle**' ye tıklayın.

![Python paketi Ekle](media/python-packages/add-python-package.png)

**Python 2 paketi Ekle** sayfasında, karşıya yüklenecek yerel bir paket seçin. Paket bir `.whl` dosyası veya `.tar.gz` dosyası olabilir. Seçildiğinde, paketi karşıya yüklemek için **Tamam** ' ı tıklatın.

![Python paketi Ekle](media/python-packages/upload-package.png)

Paket içeri aktarıldıktan sonra, Otomasyon hesabınızdaki **Python 2 paketleri** sayfasında listelenir. Bir paketi kaldırmanız gerekiyorsa, paketi seçin ve paket sayfasında **Sil** ' i seçin.

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

Python runbook 'unu içeri aktarma Python 2 paketlerini Galeriden Otomasyon hesabınıza [Azure Otomasyonu hesabına aktarın](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) . Çalıştırma ayarlarının **Azure** olarak ayarlandığından emin olun ve Runbook 'u parametrelerle başlatın. Runbook, Otomasyon hesabının çalışması için bir farklı çalıştır hesabı gerektirir. Her parametre için, aşağıdaki listede ve görüntüde görüldüğü gibi anahtarı ile başlattığınızdan emin olun:

* -s \<SubscriptionID\>
* -g \<resourceGroup\>
* -bir \<automationAccount\>
* -a \<modulePackage\>

![Paket listesi](media/python-packages/import-python-runbook.png)

Runbook, hangi paketin indirileceği, örneğin `Azure` (dördüncü parametre) tüm Azure modüllerini ve tüm bağımlılıklarını (105) indirecek şekilde belirtmenizi sağlar.

Runbook tamamlandıktan sonra, paketinin doğru bir şekilde içeri aktarıldığını doğrulamak için Otomasyon hesabınızdaki **paylaşılan kaynaklar** altındaki **Python 2 paketleri** sayfasını kontrol edebilirsiniz.

## <a name="use-a-package-in-a-runbook"></a>Runbook 'ta paket kullanma

Bir paketi içeri aktardıktan sonra, artık bunu bir runbook 'ta kullanabilirsiniz. Aşağıdaki örnek [Azure Otomasyonu yardımcı programı paketini](https://github.com/azureautomation/azure_automation_utility)kullanır. Bu paket, Azure Otomasyonu ile Python kullanmayı kolaylaştırır. Paketi kullanmak için GitHub deposundaki yönergeleri izleyin ve `from azure_automation_utility import get_automation_runas_credential` kullanarak RunAs hesabını alma işlevini içeri aktarmak için Runbook 'a ekleyin.

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

Python 2 runbook 'ları kullanmaya başlamak için bkz. [Ilk Python 2 runbook](automation-first-runbook-textual-python2.md) 'u
