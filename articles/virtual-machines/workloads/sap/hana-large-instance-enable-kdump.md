---
title: SAP HANA 'de kdump etkinleştirme betiği (büyük örnekler) | Microsoft Docs
description: SAP HANA 'de kdump etkinleştirme betiği (büyük örnekler) HLI tür ı, HLI tür II
services: virtual-machines-linux
documentationcenter: ''
author: prtyag
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/30/2020
ms.author: prtyag
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a799242ecaae7b7152d79b7d341a9cb5cc18d7fe
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654481"
---
# <a name="kdump-for-sap-hana-on-azure-large-instances-hli"></a>Azure üzerinde SAP HANA Büyük Örnekleri için kdump (HLI)

Kdump 'in yapılandırılması ve etkinleştirilmesi, temiz bir neden olmayan sistem Kilitlenmelerinde sorun gidermek için gereken bir adımdır.
Sistemin bir donanım veya altyapı sorunu tarafından açıklanamayan beklenmedik bir şekilde kilitlenme zamanı vardır.
Bu durumlarda, bir işletim sistemi veya uygulama sorunu olabilir ve kdump, SUSE 'in neden bir sistemin kilitlendiğini belirlemesine izin verir.

## <a name="enable-kdump-service"></a>Kdump hizmetini etkinleştir

Bu belgede, Azure HANA büyük örneği (**tür ı ve tür II**) üzerinde kdump hizmetini etkinleştirme ayrıntıları açıklanmaktadır

## <a name="supported-skus"></a>Desteklenen SKU 'Lar

|  Hana büyük örnek türü   |  İşletim sistemi satıcısı   |  İşletim sistemi paketi sürümü   |  SKU |
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

## <a name="prerequisites"></a>Önkoşullar

- Kdump hizmeti `/var/crash` , dökümleri yazmak için dizini kullanır, bu dizine karşılık gelen bölümün dökümlerini barındırmak için yeterli alana sahip olduğundan emin olun.

## <a name="setup-details"></a>Kurulum Ayrıntıları

- Kdump etkinleştirme betiği [burada](https://github.com/Azure/sap-hana/blob/master/tools/enable-kdump.sh) bulunabilir
> [!NOTE]
> Bu betik, laboratuvar kurulumumuza göre yapılır ve müşterinin daha fazla ayarlama için işletim sistemi satıcısına başvurması beklenmektedir.
> Yeni ve var olan sunucular için ayrı LUN sağlanacak ve dökümleri ve betiği kaydetmek için, dosya sisteminin LUN dışında yapılandırılması ele alınacaktır.
> Microsoft, dökümü analiz etmek için sorumlu olmayacaktır. Müşterinin analiz edilmesi için işletim sistemi satıcısı ile bir bilet açması gerekmez.

- Aşağıdaki komutu kullanarak bu betiği HANA büyük örneğinde çalıştırın

    > [!NOTE]
    > Bu komutu çalıştırmak için gerekli olan sudo ayrıcalığı.

    ```bash
    sudo bash enable-kdump.sh
    ```

- Komut çıktıları kdump başarıyla etkinleştirilirse, değişiklikleri başarıyla uygulamak için sistemi yeniden başlattığınızdan emin olun.

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

## <a name="related-documents"></a>İlgili Belgeler
- [Kdump yapılandırma](https://www.suse.com/support/kb/doc/?id=3374462) hakkında daha fazla bilgi için
