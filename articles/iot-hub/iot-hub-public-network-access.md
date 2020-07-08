---
title: Azure IoT Hub için ortak ağ erişimini yönetme
description: IoT Hub için genel ağ erişimini devre dışı bırakma ve etkinleştirme ile ilgili belgeler
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/01/2020
ms.openlocfilehash: c82f98df8fb79fa10f2e30b219c1a02bb646e2de
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937560"
---
# <a name="managing-public-network-access-for-your-iot-hub"></a>IoT Hub 'ınız için ortak ağ erişimini yönetme

Erişimi [sanal ağınızdaki IoT Hub 'ınız için yalnızca özel uç nokta](virtual-network-support.md)ile kısıtlamak için genel ağ erişimini devre dışı bırakın. Bunu yapmak için Azure portal veya `publicNetworkAccess` API kullanın. 

## <a name="turn-off-public-network-access-using-azure-portal"></a>Azure portal kullanarak genel ağ erişimini kapatma

1. [Azure Portal](https://portal.azure.com) ziyaret edin
2. IoT Hub'ınıza gidin.
3. Sol taraftaki menüden **ağ** ' ı seçin.
4. "Genel ağa erişime Izin ver" altında **devre dışı** seçeneğini belirleyin.
5. **Kaydet**'i seçin.

:::image type="content" source="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png" alt-text="Ortak ağ erişiminin kapatılacağı Azure portal gösteren resim" lightbox="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png":::

Ortak ağ erişimini açmak **için seçili ve**sonra **kaydedin**.

## <a name="ip-filter"></a>IP filtresi 

Ortak ağ erişimi devre dışıysa, tüm [IP filtresi](iot-hub-ip-filtering.md) kuralları yok sayılır. Bunun nedeni, genel İnternet 'ten gelen tüm IP 'Lerin engellenmesidir. IP filtresini kullanmak için **SEÇILI IP aralıkları** seçeneğini kullanın.

## <a name="bug-fix-with-built-in-event-hub-compatible-endpoint"></a>Yerleşik Olay Hub 'ı ile uyumlu uç nokta ile hata onarımı

IoT Hub genel ağ erişimi devre dışı bırakıldığında [yerleşik Olay Hub 'ı ile uyumlu uç noktanın](iot-hub-devguide-messages-read-builtin.md) genel İnternet üzerinden erişilebilir olmaya devam ettiği IoT Hub bir hata vardır. Daha fazla bilgi edinmek ve bu hata hakkında bizimle iletişim kurmak için bkz. [IoT Hub için genel ağ erişimini devre dışı bırakma yerleşik Olay Hub 'ı uç noktasına erişimi devre dışı bırakır](https://azure.microsoft.com/updates/iot-hub-public-network-access-bug-fix)