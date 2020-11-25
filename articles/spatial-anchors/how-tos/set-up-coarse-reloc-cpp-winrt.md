---
title: C++/Wınrt 'de kaba yeniden yerelleştirme
description: C++/Wınrtrtde kaba reyerelleştirme kullanarak bağlantı oluşturma ve bulma hakkında ayrıntılı açıklama.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7068ecb7fcfe888118396523e0e3d997e5a59e9a
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96005019"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-cwinrt"></a>C++/Wınrt 'de kaba reyerelleştirme kullanarak Tutturucular oluşturma ve bulma

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

- C++ ve <a href="/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt" target="_blank">Windows çalışma zamanı API 'lerinde</a>temel bilgi.
- [Azure uzamsal Tutturucuların genel bakış](../overview.md)bölümünü okuyun.
- [5 dakikalık hızlı](../index.yml)başlangıçlardan biri tamamlandı.
- [Oluşturma ve bağlantıları bulma ile nasıl yapılacağını](../create-locate-anchors-overview.md)okuyun.

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```cpp
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = PlatformLocationProvider();

// Allow GPS
SensorCapabilities sensors = sensorProvider.Sensors()
sensors.GeoLocationEnabled(true);

// Allow WiFi scanning
sensors.WifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
std::vector<winrt::hstring> uuids;
uuids.emplace_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.emplace_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

// Allow the set of known BLE beacons
sensors.BluetoothEnabled(true);
sensors.KnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```cpp
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession.LocationProvider(sensorProvider);

// Configure the near-device criteria
NearDeviceCriteria nearDeviceCriteria = NearDeviceCriteria();
nearDeviceCriteria.DistanceInMeters(5.0f);
nearDeviceCriteria.MaxResultCount(25);

// Set the session's locate criteria
anchorLocateCriteria = AnchorLocateCriteria();
anchorLocateCriteria.NearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession.CreateWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]