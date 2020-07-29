---
title: SAP HANA 'de kdump etkinleştirme betiği (büyük örnekler) | Microsoft Docs
description: SAP HANA 'de kdump etkinleştirme betiği (büyük örnekler) HLI tür ı, HLI tür II
services: virtual-machines-linux
documentationcenter: ''
author: prtyag
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/30/2020
ms.author: prtyag
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d723e95212e457a81eedf7726bf3c5bd2499643
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84488894"
---
# <a name="enable-kdump-service"></a>Kdump hizmetini etkinleştir

Bu belgede, Azure HANA büyük örneği (**tür ı ve tür II**) üzerinde kdump hizmetini etkinleştirme ayrıntıları açıklanmaktadır

## <a name="supported-skus"></a>Desteklenen SKU 'Lar

|  Hana büyük örnek türü   |  İşletim sistemi satıcısı   |  İşletim sistemi paketi sürümü   |  SKU        |
|-----------------------------|--------------|-----------------------|-------------|
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S224m      |
|   Tür ı                    |  SuSE        |   SLES 12 SP4         |  S224m      |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S72        |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S72m       |
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S72m       |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S96        |
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S96        |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S192       |
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S192       |
|   Tür ı                    |  SuSE        |   SLES 12 SP4         |  S192       |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S192m      |
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S192m      |
|   Tür ı                    |  SuSE        |   SLES 12 SP4         |  S192m      |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S144       |
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S144       |
|   Tür ı                    |  SuSE        |   SLES 12 SP2         |  S144m      |
|   Tür ı                    |  SuSE        |   SLES 12 SP3         |  S144m      |
|   Tür II                   |  SuSE        |   SLES 12 SP2         |  S384       |
|   Tür II                   |  SuSE        |   SLES 12 SP3         |  S384       |
|   Tür II                   |  SuSE        |   SLES 12 SP4         |  S384       |
|   Tür II                   |  SuSE        |   SLES 12 SP2         |  S384xm     |
|   Tür II                   |  SuSE        |   SLES 12 SP3         |  S384xm     |
|   Tür II                   |  SuSE        |   SLES 12 SP4         |  S384xm     |
|   Tür II                   |  SuSE        |   SLES 12 SP2         |  S576m      |
|   Tür II                   |  SuSE        |   SLES 12 SP3         |  S576m      |
|   Tür II                   |  SuSE        |   SLES 12 SP4         |  S576m      |

## <a name="prerequisites"></a>Ön koşullar

- Kdump hizmeti `/var/crash` , dökümleri yazmak için dizini kullanır, bu dizine karşılık gelen bölümün dökümlerini barındırmak için yeterli alana sahip olduğundan emin olun.

## <a name="setup-details"></a>Kurulum Ayrıntıları

- Kdump etkinleştirme betiği [burada](https://github.com/Azure/sap-hana/blob/master/tools/enable-kdump.sh) bulunabilir

- Aşağıdaki komutu kullanarak bu betiği HANA büyük örneğinde çalıştırın

    > [!NOTE]
    > Bu komutu çalıştırmak için gerekli olan sudo ayrıcalığı.

    ```bash
    sudo bash enable-kdump.sh
    ```

- Komut çıktıları kdump başarıyla etkinse, lütfen değişikliği uygulamak için sistemi yeniden başlatın ve kdump başarıyla etkinleştirilir. Değişiklikleri uygulamak için sistemi yeniden başlatın.

- Komut çıktısı belirli bir işlemi yapamadığımızı!!!!, çıkarken kdump hizmeti etkinleştirilmemiştir. Bölüm [destek sorununa](#support-issue)bakın.

## <a name="test-kdump"></a>Test kdump

> [!NOTE]
>  Aşağıdaki işlem, bir çekirdek kilitlenme ve sistem yeniden başlatma tetikleyecektir.

- Çekirdek kilitlenmesinin tetiklenmesi

    ```bash
    echo c > /proc/sysrq-trigger
    ```

- Sistem başarıyla yeniden başlatıldıktan sonra `/var/crash` çekirdek kilitlenme günlüklerinin dizinini denetleyin.

- `/var/crash`Geçerli tarihi olan dizini varsa, kdump başarıyla etkinleştirilir.

## <a name="support-issue"></a>Destek sorunu

Betik hata vererek başarısız olursa veya kdump etkin değilse, aşağıdaki ayrıntılarla Microsoft destek ekibi ile hizmet isteği yükseltin.

* HLI abonelik KIMLIĞI

* Sunucu adı

* İşletim sistemi satıcısı

* İşletim sistemi sürümü

* Çekirdek sürümü
