---
title: Azure Otomasyonu Güncelleştirme Yönetimi VM 'Leri kaldırma
description: Bu makalede VM 'Leri Güncelleştirme Yönetimi nasıl kaldıracakları açıklanır.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 9bb9ee93be4e045b52355255ecb86e54e48e5c5d
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117408"
---
# <a name="remove-vms-from-update-management"></a>VM’leri Güncelleştirme Yönetimi'nden kaldırma

Ortamınızdaki VM 'lere güncelleştirme dağıtma işiniz bittiğinde [güncelleştirme yönetimi](automation-update-management.md) özelliğinden kaldırabilirsiniz.

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. Yönetimden kaldırmak istediğiniz VM 'nin UUID 'sini tanımlamak için aşağıdaki komutu kullanın.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Log Analytics çalışma alanınızda, **genel**altında, kapsam yapılandırması için kaydedilen aramalara erişin `MicrosoftDefaultScopeConfig-Updates` .

4. Kayıtlı arama için `MicrosoftDefaultComputerGroup` , sağdaki üç noktaya tıklayın ve **Düzenle**' yi seçin. 

5. VM için UUID 'yi kaldırın.

6. Kaldırılacak diğer VM 'Ler için adımları yineleyin.

7. Kayıt düzenlemesini bitirdiğinizde kayıtlı aramayı kaydedin. 

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme Yönetimi çalışmaya devam etmek için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Genel özellik sorunlarını gidermek için bkz. [sorun giderme güncelleştirme yönetimi sorunları](troubleshoot/update-management.md).
* Windows Update Aracısı ile ilgili sorunlar için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux güncelleştirme aracısındaki sorunlar için bkz. [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).