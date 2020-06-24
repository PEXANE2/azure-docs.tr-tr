---
title: Azure Kinect algılayıcı SDK hakkında
description: Azure Kinect algılayıcısı yazılım geliştirme kiti 'ne (SDK), özelliklerine ve araçlarına genel bakış.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: article
keywords: Azure, Kinect, RGB, IR, kayıt, algılayıcı, SDK, erişim, derinlik, video, kamera, İmu, hareket, algılayıcı, ses, mikrofon, Matroska, algılayıcı SDK, indirme
ms.openlocfilehash: 17c1b33120eacb5d0c6d3c02e692d1488ef474e6
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277687"
---
# <a name="about-azure-kinect-sensor-sdk"></a>Azure Kinect algılayıcı SDK hakkında

Bu makalede, Azure Kinect algılayıcısı yazılım geliştirme seti (SDK), özellikleri ve araçlarına genel bir bakış sunulmaktadır.

## <a name="features"></a>Özellikler

Azure Kinect algılayıcı SDK 'Sı, Azure ınect cihaz yapılandırması ve donanım algılayıcısı akışları için platformlar arası alt düzey erişim sağlar; örneğin:

- Derinlik kamera erişimi ve mod denetimi (pasif IR modu, ayrıca geniş ve dar alan görüntüleme derinliği modları) 
- RGB kamera erişimi ve denetimi (örneğin, pozlama ve beyaz Bakiye) 
- Hareket algılayıcısı (jroscope ve ivometer) erişimi 
- Kameralar arasında yapılandırılabilir gecikmeyle eşitlenmiş derinlik-RGB kamera akışı 
- Cihazlar arasında yapılandırılabilir gecikme fark ile dış cihaz eşitleme denetimi 
- Resim çözünürlüğü, zaman damgası, vb. için kamera çerçevesi meta veri erişimi 
- Cihaz ayarlama veri erişimi 

## <a name="tools"></a>Araçlar

- Cihaz veri akışlarını izlemek ve farklı modlar yapılandırmak için bir [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md) .
- [Matroska kapsayıcı biçimini](record-file-format.md)kullanan bir [Azure Kinect Kaydedicisi](azure-kinect-recorder.md) ve kayıttan yürütme okuyucusu API 'si.
- Bir Azure Kinect DK [üretici yazılımı güncelleştirme aracı](azure-kinect-firmware-tool.md).

## <a name="sensor-sdk"></a>Algılayıcı SDK 'Sı

- [Algılayıcı SDK 'Sını indirin](sensor-sdk-download.md).
- Algılayıcı SDK, [GitHub 'da açık kaynak](https://github.com/microsoft/Azure-Kinect-Sensor-SDK)olarak kullanılabilir.
- Kullanım hakkında daha fazla bilgi için bkz. [ALGıLAYıCı SDK API 'si belgeleri](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/index.html).

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Kinect algılayıcı SDK 'Sı hakkında bilgi edindiniz, ayrıca şunları yapabilirsiniz:
>[!div class="nextstepaction"]
>[Algılayıcı SDK kodunu indir](sensor-sdk-download.md)

>[!div class="nextstepaction"]
>[Cihaz bul ve aç](find-then-open-device.md)
