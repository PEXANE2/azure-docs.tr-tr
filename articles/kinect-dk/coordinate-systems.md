---
title: Azure Kinect DK koordinat sistemleri
description: Azure ınect DK koordinat sistemleri Azure DK sensörleri ile ilişkili açıklama
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, algılayıcı, SDK, derinlik Kamerası, TOF, ilkeler, performans, ınvalidation
ms.openlocfilehash: 4bb1c3b79862b918870cff786042d9b4c66270d1
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277646"
---
# <a name="azure-kinect-dk-coordinate-systems"></a>Azure Kinect DK koordinat sistemleri

Bu makalede, 2B ve 3B koordinat sistemleri için kullanılan kuralları açıklıyoruz.  Her bir algılayıcı cihazınızla ilişkili ayrı koordinat sistemleri vardır ve [ayarlama işlevlerinin](use-calibration-functions.md) aralarında noktaları dönüştürme izni vardır. [Dönüştürme işlevleri](use-image-transformation.md) , koordinat sistemleri arasında tüm görüntüleri dönüştürür.  

## <a name="2d-coordinate-systems"></a>2B koordinat sistemleri

 Hem derinlik hem de renkli kameralar bağımsız bir 2B koordinat sistemi ile ilişkilendirilir. Bir [x, y]-koordinatı, *x* aralığının 0 ile Width-1 ve *y* aralığından 0-yükseklik-1 aralığında olduğu piksel birimleri halinde temsil edilir. Genişlik ve yükseklik, derinlik ve renkli kameraların işletibileceği seçili moda bağlıdır. Piksel koordinatı `[0,0]` görüntünün sol üst pikseline karşılık gelir. Piksel koordinatları, alt piksel koordinatlarını temsil eden kesirli olamaz.

2B koordinat sistemi 0-ortalanmış, diğer bir deyişle, alt piksel koordinatı `[0.0, 0.0]` `[0.5,0.5]` aşağıda gösterildiği gibi pikselin orta ve sağ alt köşesini temsil eder.

   ![2B koordinat sistemi](./media/concepts/concepts-coordinate-systems/coordinate-systems-sdk-2d-system.png)

## <a name="3d-coordinate-systems"></a>3B koordinat sistemleri

Her kamera, ivme ölçer ve cayroscope, bağımsız bir 3B koordinat alan sistemiyle ilişkilendirilir.

3B koordinat sistemlerindeki noktalara, ölçü birimi (X, Y, Z]), milimetre cinsinden birimler ile koordinasyon sağlar.

### <a name="depth-and-color-camera"></a>Derinlik ve renk Kamerası

Kaynak, `[0,0,0]` kameranın odak noktasında bulunur. Koordinat sistemi, pozitif X ekseni noktaları doğru, pozitif Y ekseni işaret eder ve pozitif Z ekseni noktaları ileri doğru olur.

Derinlik Kamerası, aşağıda gösterildiği gibi, renk kamerasının aşağı doğru 6 derece aşağı gösterilmiştir. 

Derinlik kamerası tarafından kullanılan iki aydınlatma vardır. Daraltma alanı-of-View (NFOV) modlarında kullanılan aydınlatma, derinlik kamera durumuyla hizalanır, bu nedenle aydınlatma tarafından görünmez. Geniş görünüm alanı (WFOV) modlarında kullanılan aydınlatma, derinlik kamerasına göre daha fazla 1,3 derece aşağı doğru bir şekilde eğimli olarak düzenlenmiş.

![3B koordinat kuralları](./media/concepts/concepts-coordinate-systems/coordinate-systems-camera-features.png)

### <a name="gyroscope-and-accelerometer"></a>Jroscope ve ivometer

Cayroscope 'ın kaynağı, `[0,0,0]` derinlik kameranın kaynağıyla aynıdır. İvometer saatle çakışan fiziksel konumuyla ilgili kaynak. İvometer ve cayroscope koordinat sistemleri sağ elli. Koordinat sisteminin pozitif X ekseni geri doğru, pozitif Y ekseni noktaları sol ve pozitif Z ekseni aşağıda gösterildiği gibi aşağı işaret eder.

![IMU koordinat sistemi](./media/concepts/concepts-coordinate-systems/coordinate-systems-gyroscope.png)

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Kinect algılayıcı SDK 'Sı hakkında bilgi edinin](about-sensor-sdk.md)