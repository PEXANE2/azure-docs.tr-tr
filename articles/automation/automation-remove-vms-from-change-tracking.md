---
title: Azure Otomasyonu Değişiklik İzleme ve envanterinden VM 'Leri kaldırma
description: Bu makalede VM 'Leri Değişiklik İzleme ve envanterden nasıl kaldıracakları açıklanır.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 1bd94beaa40f6ff793b50e261138cc31453f7016
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749168"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Değişiklik İzleme ve envanterden VM 'Leri kaldırma

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

Ortamınızdaki sanal makinelere değişiklikleri dağıtmaya bitirdiğinizde, [değişiklik izleme ve envanter](change-tracking.md) özelliğinden kaldırabilirsiniz.

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **değişiklik izleme** veya **Stok** ' ı seçin.

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

* [Değişiklik İzleme ve envantere genel bakış](change-tracking.md)
* [Değişiklik İzleme ve stoku yönetme](change-tracking-file-contents.md)
* [Değişiklik İzleme ve envanter için çalışma alanının Otomasyon hesabından bağlantısını kaldırma](automation-unlink-workspace-change-tracking.md)
* [Otomasyon hesabından Değişiklik İzleme ve envanteri etkinleştir](automation-enable-changes-from-auto-acct.md)
* [Azure portal Değişiklik İzleme ve stoku etkinleştir](automation-enable-changes-from-browse.md)
* [Runbook 'tan Değişiklik İzleme ve envanteri etkinleştirme](automation-enable-changes-from-runbook.md)
* [Azure VM 'den Değişiklik İzleme ve envanterini etkinleştirme](automation-enable-changes-from-vm.md)
* [Azure VM 'deki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
* [Değişiklik İzleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md)