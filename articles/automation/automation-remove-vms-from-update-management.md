---
title: Azure Otomasyonu Güncelleştirme Yönetimi VM 'Leri kaldırma
description: Bu makalede VM 'Leri Güncelleştirme Yönetimi nasıl kaldıracakları açıklanır.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 29d6edd5463de78bba039e4ed6219575d924ac10
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749147"
---
# <a name="remove-vms-from-update-management"></a>Güncelleştirme Yönetimi VM 'Leri kaldırma

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

Ortamınızdaki VM 'lere güncelleştirme dağıtma işiniz bittiğinde [güncelleştirme yönetimi](automation-update-management.md) özelliğinden kaldırabilirsiniz.

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. Yönetimden kaldırmak istediğiniz VM 'nin UUID 'sini tanımlamak için aşağıdaki komutu kullanın.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Log Analytics çalışma alanınızda, **genel**altında, kaydedilen aramalara erişin.

4. Kayıtlı arama için `MicrosoftDefaultComputerGroup` , sağdaki üç noktaya tıklayın ve **Düzenle**' yi seçin. 

5. VM için UUID 'yi kaldırın.

6. Kaldırılacak diğer VM 'Ler için adımları yineleyin.

7. Kayıt düzenlemesini bitirdiğinizde kayıtlı aramayı kaydedin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md)
* [Güncelleştirme Yönetimi için çalışma alanının Otomasyon hesabından bağlantısını kaldır](automation-unlink-workspace-update-management.md)
* [Otomasyon hesabından Güncelleştirme Yönetimi etkinleştirme](automation-onboard-solutions-from-automation-account.md)
* [Azure portal Güncelleştirme Yönetimi etkinleştir](automation-onboard-solutions-from-browse.md)
* [Runbook 'tan Güncelleştirme Yönetimi etkinleştirme](automation-onboard-solutions.md)
* [Azure VM 'den Güncelleştirme Yönetimi etkinleştirme](automation-onboard-solutions-from-vm.md)
* [Güncelleştirme Yönetimi sorunlarını giderme](troubleshoot/update-management.md)
* [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md)
* [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md)
