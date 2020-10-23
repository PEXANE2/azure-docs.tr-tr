---
title: Azure portal 'ten VM uzantısını etkinleştir
description: Bu makalede, karma bulut ortamlarında çalışan Azure Arc etkin sunucularına sanal makine uzantılarının Azure portal nasıl dağıtılacağı açıklanır.
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: dcab7cb441c329a60b2c6fa3256aeedb2bb5b33d
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462992"
---
# <a name="enable-azure-vm-extensions-from-the-azure-portal"></a>Azure portal Azure VM uzantılarını etkinleştirin

Bu makalede, Azure Arc etkin sunucuları tarafından desteklenen Azure VM uzantılarını Azure portal aracılığıyla bir Linux veya Windows hibrit makinesine dağıtma ve kaldırma işlemi gösterilmektedir.

## <a name="enable-extensions-from-the-portal"></a>Portaldan uzantıları etkinleştirme

VM uzantıları, Azure portal aracılığıyla sunucu tarafından yönetilen makineye yönelik yaya uygulanabilir.

1. Tarayıcınızdan [Azure Portal](https://portal.azure.com)gidin.

2. Portalda, **sunucular-Azure Arc** ' a gidin ve listeden karma makinenizi seçin.

3. **Uzantılar**' ı seçin ve **Ekle**' yi seçin. Kullanılabilir uzantılar listesinden istediğiniz uzantıyı seçin ve sihirbazdaki yönergeleri izleyin. Bu örnekte, Log Analytics VM uzantısını dağıtacağız.

    ![Seçilen makine için VM Uzantısı Seç](./media/manage-vm-extensions/add-vm-extensions.png)

    Aşağıdaki örnek, Azure portal Log Analytics VM uzantısının yüklenmesini gösterir:

    ![Log Analytics VM uzantısını yükler](./media/manage-vm-extensions/mma-extension-config.png)

    Yüklemeyi gerçekleştirmek için çalışma alanı KIMLIĞI ve birincil anahtarı sağlamanız gerekir. Bu bilgilerin nasıl bulunacağını bilmiyorsanız bkz. [çalışma alanı kimliğini ve anahtarını alma](../../azure-monitor/platform/log-analytics-agent.md#workspace-id-and-key).

4. Belirtilen gerekli bilgileri onayladıktan sonra **Oluştur**' u seçin. Dağıtımın bir özeti görüntülenir ve dağıtımın durumunu gözden geçirebilirsiniz.

>[!NOTE]
>Birden çok uzantı birlikte toplanmış ve işlenebilir olsa da, bunlar hizmet temelli olarak yüklenir. İlk uzantı yüklemesi tamamlandıktan sonra, sonraki uzantının yüklenmesi denenir.

## <a name="uninstall-extension"></a>Uzantıyı kaldır

Azure portal bir yay etkin sunucusundan bir veya daha fazla uzantıyı kaldırabilirsiniz. Bir uzantıyı kaldırmak için aşağıdaki adımları gerçekleştirin.

1. Tarayıcınızdan [Azure Portal](https://portal.azure.com)gidin.

2. Portalda, **sunucular-Azure Arc** ' a gidin ve listeden karma makinenizi seçin.

3. **Uzantılar**' ı seçin ve ardından yüklü uzantılar listesinden bir uzantı seçin.

4. **Kaldır** ' ı seçin ve doğrulamanız istendiğinde, devam etmek için **Evet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](manage-vm-extensions-cli.md), [PowerShell](manage-vm-extensions-powershell.md)veya [Azure Resource Manager şablonlarını](manage-vm-extensions-template.md)kullanarak VM uzantılarını dağıtabilir, yönetebilir ve kaldırabilirsiniz.

- Sorun giderme bilgileri, [VM genişletmeleri sorunlarını gider kılavuzunda](troubleshoot-vm-extensions.md)bulunabilir.