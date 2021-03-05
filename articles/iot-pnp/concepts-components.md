---
title: IoT Tak ve Kullan modellerdeki bileşenleri anlama | Microsoft Docs
description: Bileşenleri kullanmayan bileşenleri ve modelleri kullanan IoT Tak ve Kullan DTDL modelleri arasındaki farkı anlayın.
author: ericmitt
ms.author: ericmitt
ms.date: 07/07/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: eef8179567d83e3727c3ab949eef2706ce2a9b16
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102175813"
---
# <a name="iot-plug-and-play-components-in-models"></a>Modellerdeki IoT Tak Çalıştır bileşenleri

IoT Tak Çalıştır kurallarında, bir cihaz IoT hub'ına bağlandığında dijital ikizler tanım dili (DTDL) model kimliğini gösteriyorsa bir IoT Tak Çalıştır cihazıdır.

Aşağıdaki kod parçacığında bazı örnek model kimlikleri gösterilir:

```json
 "@id": "dtmi:com:example:TemperatureController;1"
 "@id": "dtmi:com:example:Thermostat;1",
```

## <a name="no-components"></a>Bileşen yok

Basit bir model, gömülü veya basamaklı bileşenleri kullanmaz. Telemetri, özellik ve komutları tanımlamak için üst bilgi bilgileri ve bir içerik bölümü içerir.

Aşağıdaki örnek, bileşenleri kullanmayan basit bir modelin bir parçasını gösterir:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:Thermostat;1",
  "@type": "Interface",
  "displayName": "Thermostat",
  "description": "Reports current temperature and provides desired temperature control.",
  "contents": [
    {
      "@type": [
        "Telemetry",
        "Temperature"
      ],
      "name": "temperature",
      "displayName": "Temperature",
      "description": "Temperature in degrees Celsius.",
      "schema": "double",
      "unit": "degreeCelsius"
    },
    {
      "@type": [
        "Property",
...
```

Model açıkça bir bileşen tanımlamadığı halde, tüm telemetri, özellik ve komut tanımlarıyla tek bir _varsayılan bileşen_ gibi davranır.

Aşağıdaki ekran görüntüsünde, modelin Azure IoT gezgin aracında nasıl görüntülendiği gösterilmektedir:

:::image type="content" source="media/concepts-components/default-component.png" alt-text="Azure IoT Gezgini 'nde varsayılan bileşen":::

Model KIMLIĞI, aşağıdaki ekran görüntüsünde gösterildiği gibi bir Device ikizi özelliğinde depolanır:

:::image type="content" source="media/concepts-components/twin-model-id.png" alt-text="Digital ikizi özelliğindeki model KIMLIĞI":::

Bileşenleri olmayan bir DTDL modeli, tek bir telemetri, özellik ve komut kümesine sahip bir cihaz veya IoT Edge modülü için kullanışlı bir basitleştiripdir. Bileşenleri kullanmayan bir model, var olan bir cihazı veya modülü IoT Tak ve Kullan cihazı veya modülü olarak geçirmeyi kolaylaştırır. herhangi bir bileşen tanımlamaya gerek kalmadan gerçek cihazınızı veya modülünüzü açıklayan bir DTDL modeli oluşturursunuz.

> [!TIP]
> Modül bir cihaz [modülü](../iot-hub/iot-hub-devguide-module-twins.md) veya [IoT Edge modülü](../iot-edge/about-iot-edge.md)olabilir.

## <a name="multiple-components"></a>Birden çok bileşen

Bileşenler, model arabirimini diğer arabirimlerin derlemesi olarak oluşturmanıza imkan tanır.

Örneğin, [termostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) arabirimi bir model olarak tanımlanır. [Isı denetleyicisi modelini](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)tanımlarken bu arabirimi bir veya daha fazla bileşen olarak ekleyebilirsiniz. Aşağıdaki örnekte, bu bileşenler ve olarak adlandırılır `thermostat1` `thermostat2` .

Birden çok bileşeni olan bir DTDL modeli için, iki veya daha fazla bileşen bölümü vardır. Her bölüm `@type` `Component` , aşağıdaki kod parçacığında gösterildiği gibi bir şemaya ayarlanmış ve açıkça başvurur:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:TemperatureController;1",
  "@type": "Interface",
  "displayName": "Temperature Controller",
  "description": "Device with two thermostats and remote reboot.",
  "contents": [
...
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat1",
      "displayName": "Thermostat One",
      "description": "Thermostat One of Two."
    },
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat2",
      "displayName": "Thermostat Two",
      "description": "Thermostat Two of Two."
    },
    {
      "@type": "Component",
      "schema": "dtmi:azure:DeviceManagement:DeviceInformation;1",
      "name": "deviceInformation",
      "displayName": "Device Information interface",
      "description": "Optional interface with basic device hardware information."
    }
...
```

Bu modelde, içerik bölümünde iki bileşen ve bir bileşen tanımlanmış üç bileşen vardır `Thermostat` `DeviceInformation` . Ayrıca, varsayılan bir bileşen de vardır.

## <a name="next-steps"></a>Sonraki adımlar

Artık model bileşenleri hakkında bilgi edindiğinize göre, bazı ek kaynaklar aşağıda verilmiştir:

- [DTDL yazma araçlarını yükle ve kullan](howto-use-dtdl-authoring-tools.md)
- [Dijital TWINS tanım dili v2 (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [Model depoları](./concepts-model-repository.md)
