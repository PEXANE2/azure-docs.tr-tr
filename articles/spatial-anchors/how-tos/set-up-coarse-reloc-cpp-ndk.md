---
title: C++ ' da kaba yeniden yerelleştirme/NDK
description: C++/NDK'DE kaba reyerelleştirme kullanarak bağlantıları oluşturma ve bulma hakkında ayrıntılı açıklama.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: b8b59873526247c0406c85ea29cf61cd55c4725b
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96018044"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-cndk"></a>C++ ' da kaba reyerelleştirme kullanarak Tutturucular oluşturma ve bulma/N/NDK

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android Java](set-up-coarse-reloc-java.md)
> * [C++/NDK](set-up-coarse-reloc-cpp-ndk.md)
> * [C++/WinRT](set-up-coarse-reloc-cpp-winrt.md)

Azure uzamsal bağlantıları, cihaz üzerinde ilişki oluşturabilir ve algılayıcı verilerini, oluşturduğunuz tutturucularla konumlamayı sağlayabilir. Bu veriler, cihazınızın yakınında yer olup olmadığını hızlı bir şekilde tespit etmek için de kullanılabilir. Daha fazla bilgi için bkz. [kaba yeniden yerelleştirme](../concepts/coarse-reloc.md).

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu gerçekleştirmek için şunları yaptığınızdan emin olun:

- C++ ve <a href="https://developer.android.com/ndk/" target="_blank">Android yerel geliştirme seti</a>hakkında temel bilgi.
- [Azure uzamsal Tutturucuların genel bakış](../overview.md)bölümünü okuyun.
- [5 dakikalık hızlı](../index.yml)başlangıçlardan biri tamamlandı.
- [Oluşturma ve bağlantıları bulma ile nasıl yapılacağını](../create-locate-anchors-overview.md)okuyun.

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```cpp
// Create the sensor fingerprint provider
std::shared_ptr<PlatformLocationProvider> sensorProvider;
sensorProvider = std::make_shared<PlatformLocationProvider>();

// Allow GPS
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->GeoLocationEnabled(true);

// Allow WiFi scanning
sensors->WifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
std::vector<std::string> uuids;
uuids.push_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.push_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

// Allow the set of known BLE beacons
sensors->BluetoothEnabled(true);
sensors->KnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```cpp
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession->LocationProvider(sensorProvider);

// Configure the near-device criteria
auto nearDeviceCriteria = std::make_shared<NearDeviceCriteria>();
nearDeviceCriteria->DistanceInMeters(5.0f);
nearDeviceCriteria->MaxResultCount(25);

// Set the session's locate criteria
auto anchorLocateCriteria = std::make_shared<AnchorLocateCriteria>();
anchorLocateCriteria->NearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession->CreateWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]