---
title: SUSE Linux Enterprise Server ile bir Azure VM 'yi SUSE 15 SP1 'e yükseltme | Microsoft Docs
description: Bu makalede, SUSE Linux Enterprise Server 'ı bir Azure sanal makinesi için SUSE 15 SP1 'e yükseltmek üzere SUSE dağıtım geçiş sisteminin nasıl kullanılacağına ilişkin genel adımlar sağlanmaktadır.
services: virtual-machines-linux
documentationcenter: ''
author: amkarmak
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 05/21/2020
ms.author: arremana
ms.openlocfilehash: 079574ab4c5846eae6266d1e13240baf99428a69
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449252"
---
# <a name="upgrade-azure-vm-with-sles-12-to-sles-15-sp1"></a>SLES 12 ile Azure VM 'yi SLES 15 SP1 'e yükseltme

Bu makalede, SUSE Linux Enterprise Server (SLES) 12 ' nin bir Azure sanal makinesi (VM) için SLES 15 SP1'E nasıl yükseltileceğiyle ilgili genel adımlar sağlanmaktadır. Daha fazla bilgi için bkz. [SUSE dağıtım geçiş sistemini](https://documentation.suse.com/suse-distribution-migration-system/1.0/single-html/distribution-migration-system/index.html) ve [SUSE Linux Enterprise Server 15 SP1 yükseltme kılavuzunu](https://documentation.suse.com/sles/15-SP1/single-html/SLES-upgrade/index.html#sec-update-preparation-update)kullanma.

## <a name="supported-upgrade-paths"></a>Desteklenen yükseltme yolları
SLES 15 SP1 'e geçebilmeniz için geçerli SLES sürümünün SLES 12 SP4 veya 12 SP5 olması gerekir.

![Desteklenen yükseltme yolu ile ilgili ekran görüntüsü](./media/linux-upgrade-suse-15sp1/upgrade-path.png)

## <a name="prerequisites"></a>Ön koşullar

- Geçiş etkinliğini, onaylanan kapalı kalma süresi penceresine göre planlayın. Bunun nedeni VM 'nin geçiş sırasında yeniden başlatmaları.
- Geçiş etkinliğinden önce VM 'nin tamamen bir yedeğini alın.
- Yedekleme yapılandırılmamışsa, işletim sistemi diskinin anlık görüntü yedeklemesini alın.
- [VM 'Nin v1 veya nesil v2 olduğunu denetleyin](#check-the-generation-version-for-a-vm).

## <a name="upgrade-from-suse-12-sp4-or-sp5-to-suse-15-sp1"></a>SUSE 12 SP4 veya SP5 'den SUSE 15 SP1 'e yükseltme

1. VM için en son paketi yükler:

    ```
    zypper clean --all
    zypper refresh
    zypper update
    ```

2. Yükleme tamamlandıktan sonra VM 'yi yeniden başlatın.

3. Çekirdek ve işletim sistemi sürümünü doğrulayın. Sürümün SUSE 12 SP4 veya SUSE 12 SP5 olduğundan emin olun.

    ```
    uname -a
    cat /etc/os-release
    ```

4. **SUSE-Migration-sle15-Activation**' i yükler. **SUSE-Migration-sle15-Activation** paketi yüklendiğinde, başka bir Package **SLES15 geçişi** de otomatik olarak bir bağımlılık paketi olarak yüklenir. 

   ```
   zypper install suse-migration-sle15-activation
   ```

5. Yükleme tamamlandıktan sonra, `reboot` sanal makineyi yeniden başlatmak için komutunu çalıştırın.

6. [Azure Portal](https://portal.azure.com)gıdın, VM 'yi seçin ve ardından **seri konsol**' ı seçin. Sistemin "yeniden başlatma: sistem yeniden başlatılıyor" saatinde durduğu görüntülenir. Bu işlem yaklaşık 15-45 dakika sürer. 2. nesil VM için, "yeniden başlatma: sistemi yeniden başlatma" ekranında kalmış olabilir. Bu durumda, 45 dakika bekleyin. Hala devam ederse, Azure portal VM 'nin **genel bakış** sayfasına gıdın, VM 'yi durdurun ve ardından yeniden başlatın.

     ![Seri konsolundaki iletilerle ilgili ekran görüntüsü](./media/linux-upgrade-suse-15sp1/reboot-message.png)

8. Sistem Yeni çekirdekle yeniden başlatıldıktan sonra aşağıdaki iletiyi görürsünüz.

     ![Seri konsolundaki iletilerle ilgili ekran görüntüsü](./media/linux-upgrade-suse-15sp1/output-message.png)
9. Sistemin başarıyla yükseltildiğini denetlemek için çekirdek ve işletim sistemi sürümünü doğrulayın.

    ```
    uname -a
    cat /etc/os-release
    ```

## <a name="check-the-generation-version-for-a-vm"></a>VM 'nin nesil sürümünü denetleme

Nesil sürümü denetlemek için aşağıdaki yöntemlerden birini kullanabilirsiniz:

- SLES terminalinde komutunu çalıştırın `dmidecode | grep -i hyper` . Kuşak bir v1 VM ise, hiçbir çıkış döndürülmedi. Nesin v2 VM 'Leri için aşağıdaki çıktıyı görürsünüz:

     ![2. nesil VM çıktısı hakkında ekran görüntüsü](./media/linux-upgrade-suse-15sp1/output-gen2.png)
- [Azure Portal](https://portal.azure.com), sanal makinenin **Özellikler** ' e gidin ve ardından **VM oluşturma** alanını denetleyin.
