---
title: Temel ve özel denetimler
description: IoT taban çizgisi için Azure Güvenlik Merkezi kavramı hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2019
ms.author: mlottner
ms.openlocfilehash: c52a3e55e3801eaaac885b9a3c364283f74906ba
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311652"
---
# <a name="azure-security-center-for-iot-baseline-and-custom-checks"></a>IoT taban çizgisi ve özel denetimler için Azure Güvenlik Merkezi

Bu makalede, IoT taban çizgisi için Azure Güvenlik Merkezi açıklanmaktadır ve temel özel denetimlerin ilişkili tüm özelliklerini özetler.

## <a name="baseline"></a>Taban çizgisi

Bir taban çizgisi her aygıt için standart davranış kurar ve beklenen normlardan olağandışı davranış veya sapma oluşturmayı kolaylaştırır.

## <a name="baseline-custom-checks"></a>Temel özel denetimler

Temel özel denetimler, aygıtın **Modül kimlik ikizini** kullanarak her aygıt taban çizgisi için özel bir denetim listesi belirler.

## <a name="setting-baseline-properties"></a>Taban çizgisi özelliklerini ayarlama

1. IoT Hub'ınızda değiştirmek istediğiniz aygıtı bulun ve seçin.
1. Aygıta tıklayın ve ardından **azureiotsecurity** modüllerini tıklatın.
1. **Modül Kimlik İkiz'ini**tıklatın.
1. Temel **özel denetim** dosyasını aygıta yükleyin.
1. Güvenlik modülüne temel özellikleri ekleyin ve **Kaydet'i**tıklatın.

### <a name="baseline-custom-check-file-example"></a>Temel özel denetim dosyası örneği

Temel özel denetimleri yapılandırmak için:

   ```json
    "desired": {
      "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
        "baselineCustomChecksEnabled": {
          "value" : true
        },
        "baselineCustomChecksFilePath": {
          "value" : "/home/user/full_path.xml"
        },
        "baselineCustomChecksFileHash": {
          "value" : "#hashexample!"
        }
      }
    },
   ```

## <a name="baseline-custom-check-properties"></a>Temel özel denetim özellikleri

| Adı| Durum | Geçerli değerler| Varsayılan değerler| Açıklama |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|taban çizgisiCustomChecksEnabled|Gerekli: doğru |Geçerli değerler: **Boolean** |Varsayılan değer: **false** |Yüksek öncelikli iletiler gönderilmeden önce maksimum zaman aralığı.|
|taban çizgisiCustomChecksFilePath |Gerekli: doğru|Geçerli değerler: **String**, **null** |Varsayılan değer: **null** |Temel xml yapılandırmasının tam yolu|
|taban çizgisiCustomChecksFileHash |Gerekli: doğru|Geçerli değerler: **String**, **null** |Varsayılan değer: **null** |`sha256sum`xml yapılandırma dosyasının. Ek bilgi için [sha256sum referansını](https://linux.die.net/man/1/sha256sum) kullanın. |

Ek taban çizgisi örneklerini gözden geçirmek için [bkz: özel taban çizgisi örneği -1](https://ascforiot.blob.core.windows.net/public/custom_baseline_example_hyperv_ubuntu1804.xml) ve [özel taban çizgisi örneği -2.](https://ascforiot.blob.core.windows.net/public/oms_audits.xml)

## <a name="next-steps"></a>Sonraki adımlar

- Ham [güvenlik verilerinize](how-to-security-data-access.md) erişin
- [Cihazı araştırma](how-to-investigate-device.md)
- [Güvenlik önerilerini](concept-recommendations.md) anlama ve keşfetme
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama ve keşfetme
