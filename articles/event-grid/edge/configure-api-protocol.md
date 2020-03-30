---
title: API protokollerini yapılandırma - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: IoT Edge'de Event Grid tarafından maruz kalan API protokollerini yapılandırın.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: ''
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 908bc941ee7379de067621e10adf5fd6ee6df559
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841819"
---
# <a name="configure-event-grid-api-protocols"></a>Olay Izgara API protokollerini yapılandırma

Bu kılavuz, bir Olay Izgara modülünün olası protokol yapılandırmalarına örnekler verir. Olay Izgara modülü, yönetimi ve çalışma zamanı işlemleri için API'yi ortaya çıkarır. Aşağıdaki tablo protokolleri ve bağlantı noktalarını yakalar.

| Protokol | Bağlantı noktası | Açıklama |
| ---------------- | ------------ | ------------ |
| HTTP | 5888 | Varsayılan olarak kapatıldı. Yalnızca sınama sırasında yararlıdır. Üretim iş yükleri için uygun değildir.
| HTTPS | 4438 | Varsayılan

Tüm olası yapılandırmalar için [Güvenlik ve kimlik doğrulama](security-authentication.md) kılavuzuna bakın.

## <a name="expose-https-to-iot-modules-on-the-same-edge-network"></a>HTTPS'yi aynı kenar ağındaki IoT Modüllerine maruz bırak

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-https-to-other-iot-modules-and-non-iot-workloads"></a>HTTPS'yi diğer IoT modüllerine ve IoT dışındaki iş yüklerine etkinleştirin

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> **PortBindings** bölümü, dahili bağlantı noktalarını konteyner ana bilgisayarının bağlantı noktalarıyla eşlemenize olanak tanır. Bu özellik, IoT edge aygıtına genel olarak erişilebiliyorsa, Olay Izgara modülüne IoT Edge konteyner ağının dışından erişmemi mümkün kılar.

## <a name="expose-http-and-https-to-iot-modules-on-the-same-edge-network"></a>HTTP ve HTTPS'yi aynı kenar ağındaki IoT modüllerine maruz bırak

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-http-and-https-to-other-iot-modules-and-non-iot-workloads"></a>HTTP ve HTTPS'yi diğer IoT modüllerine ve IoT dışındaki iş yüklerine etkinleştirme

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ],
      "5888/tcp": [
        {
          "HostPort": "5888"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Varsayılan olarak, her IoT Modülü köprü ağı tarafından oluşturulan IoT Edge çalışma zamanının bir parçasıdır. Aynı ağdaki farklı IoT modüllerinin birbirleriyle iletişim kurmasını sağlar. **PortBindings,** bir konteyner iç bağlantı noktasını ana makineye haritalamanızı sağlar ve böylece herkesin Olay Izgara modülünün bağlantı noktasına dışarıdan erişebilmesini sağlar.

>[!IMPORTANT]
> Bağlantı noktaları IoT Edge ağı dışında erişilebilir hale getirilebilirken, istemci kimlik doğrulaması modüle arama yapmasına gerçekten izin verilen leri zorlar.
