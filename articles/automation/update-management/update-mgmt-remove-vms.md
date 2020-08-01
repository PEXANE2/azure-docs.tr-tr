---
title: Azure Otomasyonu Güncelleştirme Yönetimi VM 'Leri kaldırma
description: Bu makalede, Güncelleştirme Yönetimi ile yönetilen makinelerin nasıl kaldırılacağı açıklanır.
services: automation
ms.topic: conceptual
ms.date: 07/28/2020
ms.custom: mvc
ms.openlocfilehash: d7f7e4aa8b2c192688020b4449c8750f94af29f6
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450439"
---
# <a name="remove-vms-from-update-management"></a>VM’leri Güncelleştirme Yönetimi'nden kaldırma

Ortamınızdaki sanal makinelerinizdeki güncelleştirmeleri yönetmeyi tamamladığınızda, [güncelleştirme yönetimi](update-mgmt-overview.md) özelliğini kullanarak VM 'leri yönetmeyi durdurabilirsiniz.

## <a name="sign-into-the-azure-portal"></a>Azure portalda oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

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

Sanal makinenizi yönetmeyi yeniden etkinleştirmek için bkz. [Azure Portal göz atarak güncelleştirme yönetimi etkinleştirme](update-mgmt-enable-portal.md) veya [bir Azure VM 'den güncelleştirme yönetimi etkinleştirme](update-mgmt-enable-vm.md).