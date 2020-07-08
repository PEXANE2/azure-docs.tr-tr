---
title: Azure Kinect üretici yazılımı aracı
description: Azure Kinect üretici yazılımı aracını kullanarak cihaz üretici yazılımını sorgulama ve güncelleştirme hakkında bilgi edinin.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, bellenim, güncelleştirme
ms.openlocfilehash: f8b89751d27fb5a4b18d635f45f63f4f36bd05f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277679"
---
# <a name="azure-kinect-dk-firmware-tool"></a>Azure Kinect DK bellenim aracı

Azure Kinect üretici yazılımı aracı, Azure Kinect DK 'nin cihaz bellenimini sorgulamak ve güncelleştirmek için kullanılabilir.

## <a name="list-connected-devices"></a>Bağlı cihazları listeleme

-L seçeneğini kullanarak bağlı cihazların bir listesini alabilirsiniz.  `AzureKinectFirmwareTool.exe -l`

```console
 == Azure Kinect DK Firmware Tool ==
Found 2 connected devices:
0: Device "000036590812"
1: Device "000274185112"
```

## <a name="check-device-firmware-version"></a>Cihaz üretici yazılımı sürümünü denetleme

-Q seçeneğini kullanarak, örneğin, ilk eklenen cihazın geçerli bellenim sürümlerini kontrol edebilirsiniz `AzureKinectFirmwareTool.exe -q` .

```console
 == Azure Kinect DK Firmware Tool ==
Device Serial Number: 000036590812
Current Firmware Versions:
    RGB camera firmware:      1.5.92
    Depth camera firmware:    1.5.66
    Depth config file:        6109.7
    Audio firmware:           1.5.14
    Build Config:             Production
    Certificate Type:         Microsoft
```

Birden çok cihaz iliştirilmişse, komuta tam seri numarasını ekleyerek hangi cihazı sorgulamak istediğinizi belirtebilirsiniz, örneğin:

`AzureKinectFirmwareTool.exe -q 000036590812`

## <a name="update-device-firmware"></a>Cihaz üretici yazılımını güncelleştirme

Bu aracın en yaygın kullanımı, cihaz bellenimini güncelleştirmedir. Seçeneğini kullanarak aracı çağırarak güncelleştirmeyi yapın `-u` . Üretici yazılımı güncelleştirmesi, hangi bellenim dosyalarının güncelleştirilmesi gerektiğini bağlı olarak birkaç dakika sürebilir.

Adım adım bellenim güncelleştirme yönergesi için bkz. [Azure Kinect üretici yazılımı güncelleştirmesi](update-device-firmware.md).  

`AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin`

Birden fazla cihaz iliştirilmişse, komuta tam seri numarasını ekleyerek hangi cihazı sorgulamak istediğinizi belirtebilirsiniz.

`AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin 000036590812`

## <a name="reset-device"></a>Cihaz sıfırlama

Cihazı bilinen bir duruma almanız gerekiyorsa, ekli bir Azure Kinect DK-r seçeneği kullanılarak sıfırlanabilir.

Birden fazla cihaz iliştirilmişse, komuta tam seri numarasını ekleyerek hangi cihazı sorgulamak istediğinizi belirtebilirsiniz.

`AzureKinectFirmwareTool.exe -r 000036590812`

## <a name="inspect-firmware"></a>Üretici yazılımını İncele

Üretici yazılımı inceleniyor, gerçek bir cihazı güncelleştirmeden önce bir bellenim bin dosyasından sürüm bilgilerini almanızı sağlar.

`AzureKinectFirmwareTool.exe -i firmware\AzureKinectDK_Fw_1.5.926614.bin`

```console
 == Azure Kinect DK Firmware Tool ==
Loading firmware package ..\tools\updater\firmware\AzureKinectDK_Fw_1.5.926614.bin.
File size: 1228844 bytes
This package contains:
  RGB camera firmware:      1.5.92
  Depth camera firmware:    1.5.66
  Depth config files: 6109.7 5006.27
  Audio firmware:           1.5.14
  Build Config:             Production
  Certificate Type:         Microsoft
  Signature Type:           Microsoft
```

## <a name="firmware-update-tool-options"></a>Bellenim güncelleştirme aracı seçenekleri

```console
 == Azure Kinect DK Firmware Tool ==
* Usage Info *
    AzureKinectFirmwareTool.exe <Command> <Arguments>

Commands:
    List Devices: -List, -l
    Query Device: -Query, -q
        Arguments: [Serial Number]
    Update Device: -Update, -u
        Arguments: <Firmware Package Path and FileName> [Serial Number]
    Reset Device: -Reset, -r
        Arguments: [Serial Number]
    Inspect Firmware: -Inspect, -i
        Arguments: <Firmware Package Path and FileName>

    If no Serial Number is provided, the tool will just connect to the first device.

Examples:
    AzureKinectFirmwareTool.exe -List
    AzureKinectFirmwareTool.exe -Update c:\data\firmware.bin 0123456
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Cihaz bellenimini güncelleştirmek için adım adım yönergeler](update-device-firmware.md)
