---
title: Sınırlar ve Kotalar IoT Tak ve Kullan | Microsoft Docs
description: IoT Tak ve Kullan kullandığınızda uygulanan sınırları, kotaları ve azaltmayı anlayın.
author: prashmo
ms.author: prashmo
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: d965d9cb8b87ce0b67f4fe0c07b660fdfd69cc07
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91577994"
---
# <a name="iot-plug-and-play-limits-quotas-and-throttles"></a>IoT Tak ve Kullan sınırları, kotaları ve kısıtları

Bu makalede IoT Tak ve Kullan 'e özgü sınırlar, Kotalar ve azaltma açıklanmaktadır. Aynı zamanda geçerli [IoT Hub kotaları ve azaltma](../iot-hub/iot-hub-devguide-quotas-throttling.md) vardır.

## <a name="iot-hub"></a>IoT Hub

Aşağıdaki sınırlar ve Kotalar bir IoT Hub 'ı için geçerlidir:

| Sınırlar, kısıtlamalar ve kısıtlar | Değer | Notlar |
|-----|-----|-----|
| Hub başına kaydedilenebilir arabirimlerin sayısı | 1500 ||
| Bir bileşen adının en büyük boyutu | 1-64 karakter | İzin verilen karakterler: a-z, A-Z, 0-9 (ilk karakter olarak değil) ve alt çizgi (ilk veya son karakter olarak değil). |
| Bir özellik adının en büyük boyutu | 1-64 karakter | İzin verilen karakterler: a-z, A-Z, 0-9 (ilk karakter olarak değil) ve alt çizgi (ilk veya son karakter olarak değil). |
| Bir özellik değerinin en büyük boyutu | Dijital TWINS tanım dili [özelliği](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#property) ile aynı | 5 düzey derinlikte ve dizi içeren bir dizi ya da herhangi bir karmaşık şema olmayabilir |
| Bir komut adının en büyük boyutu | 1-64 karakter | İzin verilen karakterler: a-z, A-Z, 0-9 (ilk karakter olarak değil) ve alt çizgi (ilk veya son karakter olarak değil).|
| Cihaz ikizi boyutu | [IoT Hub limitleriyle](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) aynı ||

## <a name="parser-library"></a>Ayrıştırıcı kitaplığı

Ayrıştırıcı kitaplığı, [dijital TWINS tanım dili](https://github.com/Azure/opendigitaltwins-dtdl)için uygulanan limitleri izler.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sonraki adım [ıot Tak ve kullan mimarisini](concepts-architecture.md)gözden geçirmeye yönelik olur.
