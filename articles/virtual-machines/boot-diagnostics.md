---
title: Azure önyükleme tanılaması
description: Azure önyükleme tanılaması ve yönetilen önyükleme tanılama 'ya genel bakış
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: 9ffd5a6397fa01efcf9aece93333dcb5e5b418cc
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88042980"
---
# <a name="azure-boot-diagnostics"></a>Azure önyükleme tanılaması

Önyükleme Tanılaması, Azure sanal makineler (VM) için VM önyükleme hatalarının tanılanmasına izin veren bir hata ayıklama özelliğidir. Önyükleme Tanılaması, seri günlük bilgilerini ve ekran görüntülerini toplayarak bir kullanıcının VM 'nin durumunu gözlemlemeye olanak sağlar.

## <a name="boot-diagnostics-storage-account"></a>Önyükleme tanılama depolama hesabı
Varsayılan olarak, Azure portal kullanılarak oluşturulan tüm VM 'Ler için önyükleme tanılaması etkindir ve yönetilen bir depolama hesabı kullanır. Yönetilen bir depolama hesabı kullanarak, kullanıcılar VM dağıtım zamanında önemli bir gelişmeye sahip olacak. Bu nedenle, müşterilerin tüm VM 'Ler için yönetilen bir depolama hesabıyla önyükleme tanılamayı kullanmasını öneririz.

> [!NOTE]
> Azure müşterileri, 1 Ekim 2020 ' e kadar, yönetilen bir depolama hesabıyla önyükleme tanılamayı kullanmaya çalışırken depolama alanı için ücretlendirilmeyecektir.

Alternatif bir önyükleme tanılaması deneyimi, özel bir depolama hesabı kullanmaktır. Bir Kullanıcı yeni bir depolama hesabı oluşturabilir ya da var olan bir depolama hesabı kullanabilir. Özel depolama hesapları hakkında daha fazla bilgi için bkz. [depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-portal.png" alt-text="Önyükleme tanılamayı nasıl etkinleştireceğinizi gösteren ekran görüntüsü":::

## <a name="boot-diagnostics-view"></a>Önyükleme tanılaması görünümü
Sanal makine dikey penceresinde bulunan önyükleme tanılama seçeneği Azure portal *destek ve sorun giderme* bölümünün altındadır. Önyükleme tanılaması seçildiğinde, bir ekran görüntüsü ve seri günlüğü bilgileri görüntülenir. Seri günlüğü çekirdek mesajlaşma içerir ve ekran görüntüsü, VM 'Lerin geçerli durumunun bir anlık görüntüsüdür. VM 'nin çalışıyor olması veya Linux 'un, beklenen ekran görüntüsünün nasıl görüneceğini belirler. Windows için, kullanıcılar bir masaüstü arka planı ve Linux için, kullanıcılar bir oturum açma istemi görür.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Linux önyükleme tanılaması ekran görüntüsü":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Windows önyükleme tanılaması 'nın ekran görüntüsü":::


## <a name="limitations"></a>Sınırlamalar
- Önyükleme tanılaması yalnızca Azure Resource Manager (ARM) VM 'Ler için kullanılabilir. 
- Önyükleme tanılaması için Premium depolama hesabı kullanılıyorsa, önyükleme tanılaması Premium Depolama hesaplarını desteklemez `StorageAccountTypeNotSupported` . 
- Azure seri konsol, şu anda önyükleme tanılaması için yönetilen bir depolama hesabını desteklememektedir.

## <a name="next-steps"></a>Sonraki Adımlar

Azure [seri konsolu](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-overview) ve [Azure 'da sanal makinelerde sorun gidermek](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics)için önyükleme tanılamayı kullanma hakkında daha fazla bilgi edinin.
