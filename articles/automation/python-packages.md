---
title: Azure Otomasyonu'nda Python 2 paketlerini yönetme
description: Bu makalede, Azure Otomasyonu'nda Python 2 paketlerinin nasıl yönetilenbir şekilde yönetilenanlatılmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2019
ms.topic: conceptual
ms.openlocfilehash: 9f52dfd92d430abffe5857d231898dd4b0e7745e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679914"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Azure Otomasyonu'nda Python 2 paketlerini yönetme

Azure Otomasyonu, Python 2 runbook'larını Azure'da ve Linux Karma Runbook Çalışanları'nda çalıştırmanızı sağlar. Runbook'ların basitleştirilmesine yardımcı olmak için, ihtiyacınız olan modülleri almak için Python paketlerini kullanabilirsiniz. Bu makalede, Azure Otomasyonu'nda Python paketlerinin nasıl yönetilip kullanılacağı açıklanmaktadır.

## <a name="import-packages"></a>Paketleri içeri aktarma

Otomasyon hesabınızda, **Paylaşılan Kaynaklar**altında Python **2 paketlerini** seçin. + **Python 2 paketi ekle'yi**tıklatın.

![Python paketi ekle](media/python-packages/add-python-package.png)

Python Ekle 2 Paketi sayfasında, yüklemek için yerel bir paket seçin. Paket bir **.whl** veya **.tar.gz** dosyası olabilir. Paket seçildiğinde, yüklemek için **Tamam'ı** tıklatın.

![Python paketi ekle](media/python-packages/upload-package.png)

Bir paket alındıktan sonra, Otomasyon hesabınızdaki Python 2 paketleri sayfasında listelenir. Bir paketi kaldırmanız gerekiyorsa, paketi seçin ve **Sil'i**tıklatın.

![Paket listesi](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>Bağımlılıklı paketleri içe aktarma

Azure otomasyonu, alma işlemi sırasında python paketlerine yönelik bağımlılıkları gidermiyor. Tüm bağımlılıkları olan bir paketi almanın iki yolu vardır. Paketleri Otomasyon hesabınıza almak için aşağıdaki adımlardan yalnızca birinin kullanılması gerekir.

### <a name="manually-download"></a>El ile indir

[Python2.7](https://www.python.org/downloads/release/latest/python2) ve [pip](https://pip.pypa.io/en/stable/) yüklü bir Windows 64-bit makinede, bir paketi ve tüm bağımlılıklarını indirmek için aşağıdaki komutu çalıştırın:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Paketler indirildikten sonra, bunları otomasyon hesabınıza aktarabilirsiniz.

### <a name="runbook"></a>Runbook

Python runbook [Import Python 2 paketlerini pypi'den Azure Automation hesabına](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) galeriden Otomasyon hesabınıza aktarın. Çalışma Ayarları'nın **Azure** olarak ayarlandığınızdan emin olun ve parametrelerle runbook'u başlatın. Runbook, Otomasyon hesabının çalışması için Bir Run As hesabı gerektirir. Her parametre için aşağıdaki liste ve resimde görüldüğü gibi anahtar ile başlatmak emin olun:

* -s \<subscriptionId\>
* -g \<kaynakGrubu\>
* -bir \<otomasyonHesabı\>
* -m \<modülüPaket\>

![Paket listesi](media/python-packages/import-python-runbook.png)

Runbook, hangi paketi indireceklerini belirtmenize olanak tanır. Örneğin, `Azure` parametre kullanımı tüm Azure modüllerini ve tüm bağımlılıkları karşıdan yükler (yaklaşık 105).

Runbook tamamlandıktan sonra, paketin doğru şekilde içe aktarıldığını doğrulamak için Otomasyon hesabınızda **Paylaşılan Kaynaklar** altındaki Python **2 paketlerini** kontrol edebilirsiniz.

## <a name="use-a-package-in-a-runbook"></a>Çalışma kitabında paket kullanma

Alınan bir paketle, bunu bir runbook'ta kullanabilirsiniz. Aşağıdaki örnekte [Azure Otomasyon yardımcı programı kullanır.](https://github.com/azureautomation/azure_automation_utility) Bu paket, Azure Otomasyonu ile Python'un kullanımını kolaylaştırır. Paketi kullanmak için GitHub deposundaki yönergeleri izleyin ve runbook'a ekleyin. Örneğin, Run As `from azure_automation_utility import get_automation_runas_credential` hesabını almak için işlevi almak için kullanabilirsiniz.

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

## <a name="develop-and-test-runbooks-offline"></a>Runbook'ları çevrimdışı geliştirme ve test edin

Python 2 runkitaplarınızı çevrimdışı geliştirmek ve test etmek için GitHub'daki [Azure Automation python taklit edilmiş varlıklar modülasyonunu](https://github.com/azureautomation/python_emulated_assets) kullanabilirsiniz. Bu modül, kimlik bilgileri, değişkenler, bağlantılar ve sertifikalar gibi paylaşılan kaynaklarınıza başvuru yapmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Python 2 runbook'ları ile başlamak için [ilk Python 2 runbook'uma](automation-first-runbook-textual-python2.md)bakın.
