---
title: 'Başvuru: bilinen sorunlar & sorun giderme'
titleSuffix: Azure Data Science Virtual  Machine
description: Azure Veri Bilimi Sanal Makinesi bilinen sorunların, geçici çözümlerin ve sorun gidermenin bir listesini alın
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: d022b1124146a1e506401e6cee257805e3a38fd3
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526561"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Azure Veri Bilimi Sanal Makinesi bilinen sorunlar ve sorun giderme

Bu makale, Azure Veri Bilimi Sanal Makinesi kullanırken oluşan hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="python-package-installation-issues"></a>Python paketi yükleme sorunları

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>Linux 'ta pımlar bağımlılıkları olan paketleri yükleme

Paketler yüklenirken `pip install` yerine `sudo pip install` kullanın.

## <a name="disk-encryption-issues"></a>Disk şifreleme sorunları

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Ubuntu DSVM 'de disk şifrelemesi başarısız oluyor

Azure disk şifrelemesi (ADE), Ubuntu DSVM 'de Şu anda desteklenmiyor. Geçici bir çözüm olarak, [Azure depolama şifrelemesini müşteri tarafından yönetilen anahtarlarla](../../storage/common/storage-encryption-keys-portal.md)yapılandırmayı düşünün.

## <a name="tool-appears-disabled"></a>Araç devre dışı görünüyor

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V, Windows DSVM 'de çalışmıyor

Bu beklenen davranış, önyükleme performansı için bazı hizmetleri devre dışı bırakmış. Yeniden etkinleştirmek için, Windows DSVM 'nizin arama çubuğunu açın, "Hizmetler" yazın, ardından tüm Hyper-V hizmetlerini "El Ile" olarak ayarlayın ve "Hyper-V sanal makine yönetimi" ni "otomatik" olarak ayarlayın.

Son ekranınız şuna benzemelidir:

   ![Hyper-V olanağını etkinleştir](./media/workaround/hyperv-enable-dsvm.png)

