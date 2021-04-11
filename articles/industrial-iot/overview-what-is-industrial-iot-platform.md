---
title: Azure endüstriyel IoT Platformu
description: Bu makalede, endüstriyel IoT Platformu ve bileşenleri için bir genel bakış sunulmaktadır.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: conceptual
ms.date: 3/22/2021
ms.openlocfilehash: 24932b17c630190cb3121d5310a865758fa6a920
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104801562"
---
# <a name="what-is-the-industrial-iot-iiot-platform"></a>Endüstriyel IoT (IIoT) platformu nedir?

Azure endüstriyel IoT Platformu, Azure 'da dağıtılan bir Microsoft modül ve hizmet paketidir. Bu modüller ve hizmetler tam olarak bir openlik kullanıyor. Özellikle, Azure 'un yönetilen bir hizmet olarak platform (PaaS) teklifi, MıT lisansı aracılığıyla lisanslanan açık kaynaklı yazılım, iletişim için uluslararası standartlar (OPC UA, AMQP, MQTT, HTTP) ve arabirimler (açık API) ve sektör veri modellerini (OPC UA) açık bir şekilde uygulayacağız.

## <a name="enabling-shopfloor-connectivity"></a>Shopfloor bağlantısını etkinleştirme 

Azure endüstriyel IoT Platformu, Shopfloor varlıkların endüstriyel bağlantısını (OPC UA etkin varlıkların bulunması dahil) kapsayan, verilerini OPC UA biçimine normalleştirir ve varlık telemetri verilerini OPC UA PubSub biçiminde Azure 'a iletir. Burada, telemetri verilerini bir bulut veritabanında depolar. Ayrıca, Platform, buluttan OPC UA aracılığıyla Shopfloor varlıklarına güvenli erişim sağlar. Cihaz yönetimi özellikleri (güvenlik yapılandırması dahil) de yerleşik olarak bulunur. OPC UA işlevselliği, kolay dağıtım ve yönetim için Docker kapsayıcı teknolojisi kullanılarak oluşturulmuştur. OPC UA etkin olmayan varlıklar için önde gelen endüstriyel bağlantı sağlayıcıları ile işbirliği yaptık ve bunlara Azure IoT Edge için OPC UA bağdaştırıcı yazılımını bağlantı noktasıyla yardımcı oluyoruz. Bu bağdaştırıcılar Azure Marketi 'nde kullanılabilir.

## <a name="industrial-iot-components-iot-edge-modules-and-cloud-microservices"></a>Endüstriyel IoT bileşenleri: IoT Edge modüller ve bulut mikro hizmetleri

Edge Hizmetleri Azure IoT Edge modüller olarak uygulanır ve şirket içinde çalıştırılır. Bulut mikro hizmetleri, ASP.NET mikro hizmetleri olarak bir REST arabirimiyle uygulanır ve yönetilen Azure Kubernetes hizmetlerinde veya tek başına Azure App Service çalışır. Hem Edge hem de bulut hizmetleri için, Microsoft Container Registry (MCR) ' de önceden oluşturulmuş Docker Kapsayıcıları sağladık, bu adım müşteri için kaldırılıyor. Kenar ve bulut hizmetleri birbirini kullanır ve birlikte kullanılması gerekir. Ayrıca, tek bir komutla platformun tamamını dağıtmaya izin veren kullanımı kolay dağıtım betikleri sunuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure endüstriyel IoT platformunun ne olduğunu öğrendiğinize göre, OPC yayımcısı hakkında bilgi edinebilirsiniz:

> [!div class="nextstepaction"]
> [OPC yayımcısı nedir?](overview-what-is-opc-publisher.md)