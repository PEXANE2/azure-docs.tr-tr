---
title: OPC Vault Nedir - Azure | Microsoft Dokümanlar
description: Bu makalede, OPC Vault'a genel bir bakış sağkalmaktadır. Buluttaki OPC UA uygulamaları için sertifika yaşam döngüsünü yapılandırabilir, kaydedebilir ve yönetebilir.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 66a322d4f60d9553a68207136ae609c1f9b50dbc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73826212"
---
# <a name="what-is-opc-vault"></a>OPC Vault nedir?

OPC Vault, Buluttaki OPC UA sunucusu ve istemci uygulamaları için sertifika yaşam döngüsünü yapılandırabilen, kaydedilebilen ve yönetebilen bir mikro hizmettir. Bu makalede, OPC Vault'un basit kullanım örnekleri açıklanmaktadır.

## <a name="certificate-management"></a>Sertifika yönetimi

Örneğin, bir üretim şirketinin OPC UA sunucu makinesini yeni inşa edilen istemci uygulamasına bağlaması gerekir. Üretici sunucu makinesinin ilk erişimini yaptığında, istemci uygulamasının güvenli olmadığını belirtmek için OPC UA sunucu uygulamasında hemen bir hata iletisi gösterilir. Bu mekanizma, mağaza zemininde kısır hack önler herhangi bir yetkisiz uygulama erişimini önlemek için OPC UA sunucu makinesinde inşa edilmiştir.

## <a name="application-security-management"></a>Uygulama güvenliği yönetimi
OPC Vault sertifika kayıt defteri, depolama ve yaşam döngüsü yönetimi için tüm işlevlere sahip olduğundan, bir güvenlik uzmanı OPC UA sunucusunun herhangi bir istemci uygulamasıyla kolayca iletişim kurmasını sağlamak için OPC Vault microservice'i kullanır. Şimdi OPC UA sunucusu güvenli bir şekilde bağlı, yeni oluşturulan istemci uygulamasına iletişim kurabilir

## <a name="the-complete-opc-vault-architecture"></a>Tam OPC Vault mimarisi
Aşağıdaki diyagram, Tam OPC Vault mimarisini göstermektedir.

![OPC Vault mimarisi](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi OPC Vault ve kullanımları hakkında öğrendim, burada önerilen bir sonraki adımdır:

> [!div class="nextstepaction"]
> [OPC Vault mimarisi](overview-opc-vault-architecture.md)
