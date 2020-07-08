---
title: Azure Otomasyonu Güncelleştirme Yönetimi VM 'Leri kaldırma
description: Bu makalede, Güncelleştirme Yönetimi ile yönetilen makinelerin nasıl kaldırılacağı açıklanır.
services: automation
ms.topic: conceptual
ms.date: 06/30/2020
ms.custom: mvc
ms.openlocfilehash: 9745ddea1035f239a9ca65a073fb698a8f42c01f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610056"
---
# <a name="remove-vms-from-update-management"></a>VM’leri Güncelleştirme Yönetimi'nden kaldırma

Ortamınızdaki VM 'lere güncelleştirme dağıtma işiniz bittiğinde [güncelleştirme yönetimi](automation-update-management.md) özelliğinden kaldırabilirsiniz.

## <a name="to-remove-your-vms"></a>VM 'lerinizi kaldırmak için

1. Otomasyon **hesabınızdan güncelleştirme yönetimi altında** **güncelleştirme yönetimi** ' ni seçin.

2. Yönetimden kaldırmak istediğiniz bir makinenin UUID 'sini tanımlamak için aşağıdaki komutu kullanın.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Log Analytics çalışma alanınızda, **genel**altında, kapsam yapılandırması için kaydedilen aramalara erişin `MicrosoftDefaultScopeConfig-Updates` .

4. Kayıtlı arama için `MicrosoftDefaultComputerGroup` , sağdaki üç noktaya tıklayın ve **Düzenle**' yi seçin.

5. VM için UUID 'yi kaldırın.

6. Kaldırılacak diğer VM 'Ler için adımları yineleyin.

7. Kayıt düzenlemesini bitirdiğinizde kayıtlı aramayı kaydedin.

>[!NOTE]
>Son 24 saat içinde değerlendirilen tüm makinelere rapor verdiğimiz için makineler kaydolduktan sonra gösterilmeye devam eder. Makinenin bağlantısı kesildikten sonra, artık listelenmemesi için 24 saat beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme Yönetimi çalışmaya devam etmek için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Genel özellik sorunlarını gidermek için bkz. [sorun giderme güncelleştirme yönetimi sorunları](troubleshoot/update-management.md).
* Windows Update Aracısı ile ilgili sorunlar için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux güncelleştirme aracısındaki sorunlar için bkz. [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).