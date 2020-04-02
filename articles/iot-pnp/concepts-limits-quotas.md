---
title: Limitler ve kotalar IoT Tak ve Çalıştır Önizleme | Microsoft Dokümanlar
description: IoT Tak ve Çalıştır Önizleme'yi kullandığınızda uygulanan sınırları, kotaları ve azaltmayı anlayın.
author: miagdp
ms.author: miag
ms.date: 04/01/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: cce99b7d9de09134fd01afb36c41bce3966e8536
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518181"
---
# <a name="iot-plug-and-play-preview-limits-quotas-and-throttles"></a>IoT Tak ve Çalıştır Önizleme limitleri, kotalar ve azaltmalar

Bu makalede, Genel önizlemede geçerli olan IoT Tak ve Çalıştır'a özgü limitler, kotalar ve azaltma açıklanmaktadır. Mevcut [IoT Hub kotaları ve azaltma](../iot-hub/iot-hub-devguide-quotas-throttling.md) da geçerlidir.

## <a name="iot-hub"></a>IoT Hub

Genel önizleme için, aşağıdaki sınırlar ve kotalar bir IoT hub'ına uygulanır:

| Sınırlar, kısıtlamalar ve azaltmalar | Değer | Notlar |
|-----|-----|-----|
| Hub başına kaydedilebilen aygıt yetenek modelleri (DCM) veya arabirim sayısı | 1500 ||
| Aygıt başına kaydedilebilen maksimum arabirim sayısı | 40 ||
| Cihaz başına kaydedilebilen maksimum DC Sayısı | 1 ||
| Arabirim/DCM dosyasının maksimum boyutu | 512 KB ||
| Bir arabirim adının maksimum boyutu | 256 şarampole ||
| Bir özellik adının maksimum boyutu  | 64 bayt, derinliği 7 seviyeleri (ve ilk `$iotin`seviye için ayrılmıştır) | İzin verilen karakterler: a-z, A-Z, 0-9 (ilk karakter olarak değil) ve altını çizer. |
| Özellik değerinin maksimum boyutu | 512 bayt ||
| Komut adının maksimum boyutu | 100 bayt ||
| Cihaz ikiz boyutu | [IoT Hub Sınırları](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) ile Aynı ||
| SKU genelinde çözünürlük API çağrıları (birimlerne bakılmaksızın) | 100 istek/saniye ||

## <a name="model-repository"></a>Model Deposu

Genel önizleme için, aşağıdaki sınırlar ve kotalar bir model deposu için geçerlidir:

| Sınırlar, kısıtlamalar ve azaltmalar | Değer |
|-----|-----|
| Azure Etkin Dizin kiracıbaşına şirket modeli deposayısı | 1 |
| Model deposu başına yetki anahtarı sayısı | 10  |
| Şirket modeli deposu başına model (DCM veya arabirim) sayısı| 1500  |
| Azure Etkin Dizin kiracıbaşına genel model deposundaki model (DC'ler veya arabirimler) sayısı| 1500  |
| Şirket modeli deposunda silinen DC'lerin veya arabirimlerin sayısı | Saniyede 10 sorgu (QPS)|
| Kiracı tarafından oluşturulan/güncelleştirilen model depolarının sayısı| 1 QPS |
| Model deposunda oluşturulan/güncellenen/silinen yetkilendirme anahtarlarının sayısı | 1 QPS|
| Şirket modeli deposunda oluşturulan DC Sayısı | 10 QPS |
| Şirket modeli deposunda oluşturulan arabirim sayısı | 10 QPS|
| Ortak model deposunda oluşturulan DC'lerin sayısı | 10 QPS|
| Ortak model deposunda oluşturulan arabirim sayısı | 10 QPS|

## <a name="parser-library"></a>Parser kütüphanesi

Parser [kitaplığı, Dijital İkiz Tanımlı Dil](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL)için geçerli olan sınırları izler.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen bir sonraki adım, [bir IoT Tak ve Çalıştır aygıtına nasıl bağlanıp etkileşimde bulundurunmayı](./howto-develop-solution.md)öğrenmektir.
