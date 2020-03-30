---
title: 'Başvuru: Bilinen sorunlar & sorun giderme'
titleSuffix: Azure Data Science Virtual  Machine
description: Azure Veri Bilimi Sanal Makinesi için bilinen sorunların, geçici çözüm ve sorun giderme sorunlarının bir listesini alın
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: b83effa95b17d712d4019f8ab5bf13c4f02a7d2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206529"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Bilinen sorunlar ve Azure Veri Bilimi Sanal Makinesi'nin sorun giderme

Bu makale, Azure Veri Bilimi Sanal Makinesi'ni kullanırken karşılaşabileceğiniz hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="python-package-installation-issues"></a>Python paket yükleme sorunları

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>Pip ile paketleryükleme Linux bağımlılıkları tatili

Paketleri `sudo pip install` `pip install` yüklerken yerine kullanın.

## <a name="disk-encryption-issues"></a>Disk şifreleme sorunları

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Ubuntu DSVM'de disk şifreleme başarısız olur

Azure Disk Şifreleme (ADE) şu anda Ubuntu DSVM'de desteklenmez. Geçici çözüm olarak, [Azure yönetilen disklerin Sunucu Tarafı Şifrelemesini yapılandırmayı](../../virtual-machines/windows/disk-encryption.md)düşünün.

## <a name="tool-appears-disabled"></a>Araç devre dışı görünüyor

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V Windows DSVM'de çalışmıyor

Hyper-V'nin başlangıçta Windows'da çalışmaması beklenen bir davranıştır. Önyükleme performansı için bazı hizmetleri devre dışı bıraktık. Hyper-V'yi etkinleştirmek için:

1. Windows DSVM'nizdeki arama çubuğunu açma
1. "Hizmetler" yazın,
1. Tüm Hyper-V hizmetlerini "Manuel" olarak ayarlayın
1. "Hyper-V Sanal Makine Yönetimi"ni "Otomatik" olarak ayarlayın

Son ekranınız şu şekilde görünmelidir:

   ![Hyper-V'yi etkinleştirme](./media/workaround/hyperv-enable-dsvm.png)

