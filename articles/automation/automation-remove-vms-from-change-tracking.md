---
title: Azure Otomasyonu Değişiklik İzleme ve envanterinden VM 'Leri kaldırma
description: Bu makalede VM 'Leri Değişiklik İzleme ve envanterden nasıl kaldıracakları açıklanır.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 22cb49c414e21e5c47330f2c67fc2cf30e3364b2
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836677"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok özelliğinden VM’leri kaldırma

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

* Değişiklik İzleme ve envanterle çalışmaya devam etmek için bkz. [değişiklik izleme ve stoku yönetme](change-tracking-file-contents.md).
* Genel özellik sorunlarını gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).