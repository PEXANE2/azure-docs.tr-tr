---
title: Azure Otomasyonu'nda Python 2 paketlerini yönetme
description: Bu makalede, Azure Otomasyonu'nda Python 2 paketlerinin nasıl yönetilenbir şekilde yönetilenanlatılmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2019
ms.topic: conceptual
ms.openlocfilehash: 05d892edf20cda228bc566b30b0b693ea7c4a184
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75417638"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Azure Otomasyonu'nda Python 2 paketlerini yönetme

Azure Otomasyonu, Python 2 runbook'larını Azure'da ve Linux Karma Runbook Çalışanları'nda çalıştırmanızı sağlar. Runbook'ların basitleştirilmesine yardımcı olmak için, ihtiyacınız olan modülleri almak için Python paketlerini kullanabilirsiniz. Bu makalede, Azure Otomasyonu'nda Python paketlerini nasıl yönettiğiniz ve kullandığınız açıklanmaktadır.

## <a name="import-packages"></a>Paketleri içeri aktarma

Otomasyon Hesabınızda, **Paylaşılan Kaynaklar**altında Python **2 paketlerini** seçin. + **Python 2 paketi ekle'yi**tıklatın.

![Python paketi ekle](media/python-packages/add-python-package.png)

Python **Ekle 2 Paketi** sayfasında, yüklemek için yerel bir paket seçin. Paket bir `.whl` dosya veya `.tar.gz` dosya olabilir. Seçildiğinde, paketi yüklemek için **Tamam'ı** tıklatın.

![Python paketi ekle](media/python-packages/upload-package.png)

Bir paket alındıktan sonra, Otomasyon Hesabınızdaki **Python 2 paketleri** sayfasında listelenir. Bir paketi kaldırmanız gerekiyorsa, paketi seçin ve paket sayfasında **Sil'i** seçin.

![Paket listesi](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>Bağımlılıklı paketleri içe aktarma

Azure otomasyonu, alma işlemi sırasında python paketlerine yönelik bağımlılıkları gidermiyor. Tüm bağımlılıkları olan bir paketi almanın iki yolu vardır. Paketleri Otomasyon Hesabınıza aktarmak için aşağıdaki adımlardan yalnızca birinin kullanılması gerekir.

### <a name="manually-download"></a>El ile indir

[Python2.7](https://www.python.org/downloads/release/latest/python2) ve [pip](https://pip.pypa.io/en/stable/) yüklü bir Windows 64-bit makinede, bir paketi ve tüm bağımlılıklarını indirmek için aşağıdaki komutu çalıştırın:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Paketler indirildikten sonra, bunları otomasyon hesabınıza aktarabilirsiniz.

### <a name="runbook"></a>Runbook

Python runbook [Import Python 2 paketlerini pypi'den Azure Otomasyon hesabına](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) galeriden Otomasyon Hesabınıza aktarın. Çalışma Ayarları'nın **Azure** olarak ayarlandığınızdan emin olun ve parametrelerle runbook'u başlatın. Runbook'un çalışması için Otomasyon Hesabı Olarak Çalıştır'ın çalışması için bir Run As Hesabı gerekmektedir. Her parametre için aşağıdaki liste ve resimde görüldüğü gibi anahtar ile başlatmak emin olun:

* -s \<subscriptionId\>
* -g \<kaynakGrubu\>
* -bir \<otomasyonHesabı\>
* -m \<modülüPaket\>

![Paket listesi](media/python-packages/import-python-runbook.png)

Runbook, örneğin hangi paketi karşıdan yükleyeceğinizi belirtmenize olanak `Azure` tanır( dördüncü parametre) tüm Azure modüllerini ve yaklaşık 105 olan tüm bağımlılıklarını indirir.

Runbook tamamlandıktan sonra, paketlerin doğru şekilde içe aktarıldığını doğrulamak için Otomasyon Hesabınızdaki **Paylaşılan Kaynaklar** altındaki Python **2 paketleri** sayfasını kontrol edebilirsiniz.

## <a name="use-a-package-in-a-runbook"></a>Çalışma kitabında paket kullanma

Bir paketi aldıktan sonra, artık bir runbook'ta kullanabilirsiniz. Aşağıdaki örnekte [Azure Otomasyon yardımcı programı kullanır.](https://github.com/azureautomation/azure_automation_utility) Bu paket, Azure Otomasyonu ile Python'un kullanımını kolaylaştırır. Paketi kullanmak için GitHub deposundaki yönergeleri izleyin ve runas hesabını almak `from azure_automation_utility import get_automation_runas_credential` için işlevi almak için örneğin kullanarak runbook'a ekleyin.

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

Python 2 runbook'ları ile başlamak için [ilk Python 2 runbook'uma](automation-first-runbook-textual-python2.md) bakın
