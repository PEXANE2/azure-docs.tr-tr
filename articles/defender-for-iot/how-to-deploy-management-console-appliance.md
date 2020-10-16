---
title: IoT için Azure Defender 'da yönetim konsolunu dağıtma
description: IoT için Azure Defender 'da yönetim konsolunun nasıl dağıtılacağı hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/14/2020
ms.author: rkarlin
ms.openlocfilehash: 6af943951b2dba0c4a11b08ad1bcd961cf1c0fc2
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094534"
---
# <a name="deploy-the-management-console"></a>Yönetim konsolunu dağıtma
Bu makalede IoT için Azure Defender 'ın şirket içi yönetim konsolunun nasıl dağıtılacağı açıklanır.

## <a name="the-on-premises-management-console"></a>Şirket içi yönetim konsolu

Şirket içi yönetim konsolu tüm ağ varlıklarının birleştirilmiş bir görünümünü sağlar ve tüm tesislerinizde önemli IoT ve OT risk göstergelerinin ve uyarıların gerçek zamanlı bir görünümünü sunar. SOC iş akışlarınızla sıkı bir şekilde tümleştirilmiş ve Kitaplar çalıştırdığından, risk azaltma etkinliklerinin ve siteler arası tehditlerin kolay bir şekilde önceliklendirilmesi sağlanır.

- Bütünsel-varlık yönetimi, risk ve güvenlik açığı yönetimi ve olay yanıtıyla tehdit izleme için tek bir birleştirilmiş platformun karmaşıklığını azaltın.

- Toplama ve bağıntı: tüm sitelerden toplanan verileri ve Uyarıları görüntüleyin, toplayın ve çözümleyin.

- Tüm algılayıcıları denetle – tüm algılayıcıları tek bir konumdan yapılandırın ve izleyin.

   ![IoT için Azure Defender site yönetim görünümü](media/updates/image2.png)

## <a name="deploy-the-on-premises-management-console-appliance"></a>Şirket içi yönetim konsolu gerecini dağıtma

Bu işlem, kendi donanımınızı almak ve yazılımı yüklemek için gereklidir. Çözüm, sertifikalı gereçler üzerinde çalışır. Sertifikalı gerecinizi satın alırken [IoT Için Azure Defender for IoT donanım belirtimleri Kılavuzu](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) ' na bakın.

ISO görüntüsünü indirme ve algılayıcı yazılımını yükleme hakkında ayrıntılı bilgi için [IoT Için Azure Defender Yükleme Kılavuzu](https://aka.ms/AzureDefenderforIoTInstallSensorISO) 'na bakın.

**Şirket içi yönetim konsolunu dağıtmak için:**

1. IoT için Microsoft Azure Defender 'a gidin.

2. Şirket **içi yönetim konsolunu**seçin.

   ![IoT için Azure Defender şirket Içi Yönetim Konsolu görünümü](media/updates/image15.png)

3. Sürüm **Seç** menüsünden sürüm 3,1 ' yı seçin.

4. **İndir** ve dosyayı Kaydet ' i seçin.

5. Yazılım yüklendikten sonra, ağ kurulum görevlerini gerçekleştirin. Ayrıntılar için [Azure Defender for IoT Network kurulum kılavuzuna](https://aka.ms/AzureDefenderForIoTNetworkSetup) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için, modül yapılandırması için nasıl yapılır kılavuzuna ilerleyin.
> [!div class="nextstepaction"]
> [Modül yapılandırması nasıl yapılır Kılavuzu](./how-to-agent-configuration.md)
