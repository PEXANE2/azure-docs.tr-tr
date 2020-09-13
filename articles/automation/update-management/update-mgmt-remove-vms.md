---
title: Azure Otomasyonu Güncelleştirme Yönetimi VM 'Leri kaldırma
description: Bu makalede, Güncelleştirme Yönetimi ile yönetilen makinelerin nasıl kaldırılacağı açıklanır.
services: automation
ms.topic: conceptual
ms.date: 09/09/2020
ms.custom: mvc
ms.openlocfilehash: 66631adbb56a98431e70f956f3e860b16e8f7ea2
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89648645"
---
# <a name="remove-vms-from-update-management"></a>VM’leri Güncelleştirme Yönetimi'nden kaldırma

Ortamınızdaki sanal makinelerinizdeki güncelleştirmeleri yönetmeyi tamamladığınızda, [güncelleştirme yönetimi](update-mgmt-overview.md) özelliğini kullanarak VM 'leri yönetmeyi durdurabilirsiniz. Bunları yönetmeyi durdurmak için, `MicrosoftDefaultComputerGroup` Log Analytics çalışma alanınızda Otomasyon hesabınıza bağlı olan kayıtlı arama sorgusunu düzenlersiniz.

## <a name="sign-into-the-azure-portal"></a>Azure portalda oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="to-remove-your-vms"></a>VM 'lerinizi kaldırmak için

1. Azure portal, Azure portal üst gezintiden **Cloud Shell** başlatın. Azure Cloud Shell hakkında bilginiz yoksa bkz. [Azure Cloud Shell Genel Bakış](../../cloud-shell/overview.md).

2. Yönetimden kaldırmak istediğiniz bir makinenin UUID 'sini tanımlamak için aşağıdaki komutu kullanın.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Azure portal, **Log Analytics çalışma alanları**' na gidin. Listeden çalışma alanınızı seçin.

4. Log Analytics çalışma alanınızda **Günlükler** ' i seçin ve ardından üstteki Eylemler menüsünden **sorgu Gezgini** ' ni seçin.

5. Sağ bölmedeki **sorgu Gezgini** ' nden, **kaydedilmiş Queries\Updates** ' i genişletin ve kayıtlı arama sorgusunu seçerek `MicrosoftDefaultComputerGroup` düzenleyin.

6. Sorgu Düzenleyicisi 'nde, sorguyu gözden geçirin ve VM için UUID 'yi bulun. VM için UUID 'yi kaldırın ve kaldırmak istediğiniz diğer VM 'Ler için adımları tekrarlayın.

7. En üstteki çubuktan **Kaydet** ' i seçerek, kayıt düzenlemesini tamamladıktan sonra kaydedilmiş aramayı kaydedin.

>[!NOTE]
>Son 24 saat içinde değerlendirilen tüm makinelere rapor verdiğimiz için makineler kaydolduktan sonra gösterilmeye devam eder. Makineyi kaldırdıktan sonra, artık listelenmemesi için 24 saat beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenizi yönetmeyi yeniden etkinleştirmek için bkz. [Azure Portal göz atarak güncelleştirme yönetimi etkinleştirme](update-mgmt-enable-portal.md) veya [bir Azure VM 'den güncelleştirme yönetimi etkinleştirme](update-mgmt-enable-vm.md).