---
title: Azure önyükleme tanılaması
description: Azure önyükleme tanılaması ve yönetilen önyükleme tanılama 'ya genel bakış
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: 52a2b5e27cd5857416343e559237d08ea9a591be
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91972399"
---
# <a name="azure-boot-diagnostics"></a>Azure önyükleme tanılaması

Önyükleme Tanılaması, Azure sanal makineler (VM) için VM önyükleme hatalarının tanılanmasına izin veren bir hata ayıklama özelliğidir. Önyükleme Tanılaması, seri günlük bilgilerini ve ekran görüntülerini toplayarak bir kullanıcının VM 'nin durumunu gözlemlemeye olanak sağlar.

## <a name="boot-diagnostics-storage-account"></a>Önyükleme tanılama depolama hesabı
Azure portal içinde bir sanal makine oluştururken, önyükleme tanılaması varsayılan olarak etkindir. Önerilen önyükleme tanılama deneyimi, bir Azure VM oluşturma zamanında önemli performans geliştirmeleri elde eden bir yönetilen depolama hesabı kullanmaktır. Bunun nedeni, bir Azure yönetilen depolama hesabının kullanılması, önyükleme tanılama verilerini depolamak için yeni bir kullanıcı depolama hesabı oluşturmak için gereken süreyi ortadan kaldırır.

Alternatif bir önyükleme tanılaması deneyimi, Kullanıcı tarafından yönetilen bir depolama hesabı kullanmaktır. Bir Kullanıcı yeni bir depolama hesabı oluşturabilir ya da var olan bir depolama hesabı kullanabilir.

> [!IMPORTANT]
> Azure müşterileri, 2020 Ekim aracılığıyla yönetilen bir depolama hesabı kullanılarak önyükleme tanılamaları ile ilişkili depolama maliyetleri için ücretlendirilmeyecektir.
>
> Önyükleme tanılaması veri Blobları (günlüklerin ve anlık görüntü görüntülerinin bulunduğu) yönetilen bir depolama hesabında depolanır. Müşteriler, diskin sağlanan boyutunda değil, Bloblar tarafından yalnızca kullanılan GiBs üzerinden ücretlendirilecektir. Anlık görüntü ölçümleri yönetilen depolama hesabının faturalandırılması için kullanılacaktır. Yönetilen hesaplar standart LRS veya standart ZRS üzerinde oluşturulduğundan, müşteriler yalnızca tanılama veri Blobları için aylık olarak $0,$ GB üzerinden ücretlendirilir. Bu fiyatlandırma hakkında daha fazla bilgi için bkz. [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/). Müşteriler bu ücreti VM Kaynak URI 'sine bağlı olarak görür. 

## <a name="boot-diagnostics-view"></a>Önyükleme tanılaması görünümü
Sanal makine dikey penceresinde bulunan önyükleme tanılama seçeneği Azure portal *destek ve sorun giderme* bölümünün altındadır. Önyükleme tanılaması seçildiğinde, bir ekran görüntüsü ve seri günlüğü bilgileri görüntülenir. Seri günlüğü çekirdek mesajlaşma içerir ve ekran görüntüsü, VM 'Lerin geçerli durumunun bir anlık görüntüsüdür. VM 'nin çalışıyor olması veya Linux 'un, beklenen ekran görüntüsünün nasıl görüneceğini belirler. Windows için, kullanıcılar bir masaüstü arka planı ve Linux için, kullanıcılar bir oturum açma istemi görür.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Linux önyükleme tanılaması ekran görüntüsü":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Linux önyükleme tanılaması ekran görüntüsü":::


## <a name="limitations"></a>Sınırlamalar
- Önyükleme tanılaması yalnızca Azure Resource Manager VM 'Ler için kullanılabilir. 
- Önyükleme tanılaması için Premium depolama hesabı kullanılıyorsa, önyükleme tanılaması Premium Depolama hesaplarını desteklemez `StorageAccountTypeNotSupported` . 
- Yönetilen depolama hesapları, Kaynak Yöneticisi API sürümü "2020-06-01" ve üzeri sürümlerde desteklenir.
- Azure seri konsol Şu anda önyükleme tanılaması için yönetilen bir depolama hesabıyla uyumsuz. [Azure seri konsolu](./troubleshooting/serial-console-overview.md)hakkında daha fazla bilgi edinin.
- Bir yönetim depolama hesabı kullanan önyükleme tanılaması Şu anda yalnızca Azure portal aracılığıyla uygulanabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure [seri konsolu](./troubleshooting/serial-console-overview.md) ve [Azure 'da sanal makinelerde sorun gidermek](./troubleshooting/boot-diagnostics.md)için önyükleme tanılamayı kullanma hakkında daha fazla bilgi edinin.