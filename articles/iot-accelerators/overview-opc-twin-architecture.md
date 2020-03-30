---
title: OPC Twin mimarisi - Azure | Microsoft Dokümanlar
description: Bu makalede, OPC Twin mimarisine genel bir bakış sağlar. Sunucunun keşfi, etkinleştirilmesi, taramayı ve izlenmesini açıklar.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: b8d4424c92ff24c36650e34a5d050bdc5f0f9091
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73819848"
---
# <a name="opc-twin-architecture"></a>OPC Twin mimarisi

Aşağıdaki diyagramlar OPC Twin mimarisini göstermektedir.

## <a name="discover-and-activate"></a>Keşfedin ve etkinleştirin

1. Operatör, modülde ağ taraması yapılmasını sağlar veya bir keşif URL'si kullanarak tek seferlik bir keşif yapar. Keşfedilen uç noktalar ve uygulama bilgileri, telemetri aracılığıyla işlenmek üzere onboarding acentesine gönderilir.  OPC UA aygıtı, opc Twin IoT Edge modülü tarafından gönderilen opc UA sunucu bulma olaylarını keşif veya tetkik modunda işler. Keşif olayları, Uygulama Kaydı ve OPC UA aygıt kayıt defterinde güncelleştirmeler ile sonuçlanır.

   ![OPC Twin nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin1.png)

1. Operatör keşfedilen bitiş noktasının sertifikasını inceler ve erişim için kayıtlı uç nokta ikizini etkinleştirir. 

   ![OPC Twin nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin2.png)

## <a name="browse-and-monitor"></a>Göz atın ve izleyin

1. Etkinleştirildikten sonra, operatör sunucu bilgi modeline göz atmak veya incelemek, nesne değişkenlerini okumak/yazmak ve arama yöntemlerini incelemek için Twin service REST API'yi kullanabilir.  Kullanıcı, HTTP ve JSON'da tam olarak ifade edilen basitleştirilmiş bir OPC UA API kullanır.

   ![OPC Twin nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin3.png)

1. İkiz hizmet REST arabirimi, OPC Publisher'da izlenen öğeler ve abonelikler oluşturmak için de kullanılabilir. OPC Publisher telemetrinin OPC UA sunucu sistemlerinden IoT Hub'a gönderilmesine izin verir. OPC Publisher hakkında daha fazla bilgi için [OPC Publisher nedir'e](overview-opc-publisher.md)bakın.

   ![OPC Twin nasıl çalışır?](media/overview-opc-twin-architecture/opc-twin4.png)
