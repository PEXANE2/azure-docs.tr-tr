---
title: Azure önyükleme tanılaması
description: Azure önyükleme tanılaması ve yönetilen önyükleme tanılama 'ya genel bakış
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: e2ba5d909a3aa43921f52295d2f7216aac76bc32
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88067095"
---
# <a name="azure-boot-diagnostics"></a>Azure önyükleme tanılaması

Önyükleme Tanılaması, Azure sanal makineler (VM) için VM önyükleme hatalarının tanılanmasına izin veren bir hata ayıklama özelliğidir. Önyükleme Tanılaması, seri günlük bilgilerini ve ekran görüntülerini toplayarak bir kullanıcının VM 'nin durumunu gözlemlemeye olanak sağlar.

## <a name="boot-diagnostics-view"></a>Önyükleme tanılaması görünümü
Sanal makine dikey penceresinde bulunan önyükleme tanılama seçeneği Azure portal *destek ve sorun giderme* bölümünün altındadır. Önyükleme tanılaması seçildiğinde, bir ekran görüntüsü ve seri günlüğü bilgileri görüntülenir. Seri günlüğü çekirdek mesajlaşma içerir ve ekran görüntüsü, VM 'Lerin geçerli durumunun bir anlık görüntüsüdür. VM 'nin çalışıyor olması veya Linux 'un, beklenen ekran görüntüsünün nasıl görüneceğini belirler. Windows için, kullanıcılar bir masaüstü arka planı ve Linux için, kullanıcılar bir oturum açma istemi görür.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Linux önyükleme tanılaması ekran görüntüsü":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Windows önyükleme tanılaması 'nın ekran görüntüsü":::


## <a name="limitations"></a>Sınırlamalar
- Önyükleme tanılaması yalnızca Azure Resource Manager VM 'Ler için kullanılabilir. 
- Önyükleme tanılaması için Premium depolama hesabı kullanılıyorsa, önyükleme tanılaması Premium Depolama hesaplarını desteklemez `StorageAccountTypeNotSupported` . 

## <a name="next-steps"></a>Sonraki adımlar

Azure [seri konsolu](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-overview) ve [Azure 'da sanal makinelerde sorun gidermek](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics)için önyükleme tanılamayı kullanma hakkında daha fazla bilgi edinin.
