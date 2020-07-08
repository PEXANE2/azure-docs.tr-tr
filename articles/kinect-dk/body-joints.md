---
title: Azure Kinect gövdesi izleme yukarı noktaları
description: Azure Kinect DK 'de gövde çerçevesini, yukarı noktaları, Birleşik koordinatları ve Birleşik hiyerarşiyi anlayın.
author: qm13
ms.author: quentinm
ms.reviewer: cedmonds, abalan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, taşıma, gövde, izleme, Joint, Hierarchy, kemik, bağlantı
ms.openlocfilehash: 4cf6ac13a93d0674f9fa144abcc3153a2d7c3350
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277692"
---
# <a name="azure-kinect-body-tracking-joints"></a>Azure Kinect gövdesi izleme yukarı noktaları

Azure Kinect gövde izlemesi aynı anda birden çok insan gövdesini izleyebilir. Her gövde, çerçeveler ve Kinematic iskelet arasında zamana bağlı bağıntı için bir KIMLIK içerir. Her karede algılanan gövdeler aracılığıyla elde edilebilir `k4abt_frame_get_num_bodies()` .

## <a name="joints"></a>Neşeli

Birleşik konum ve yönlendirme, başvurunun genel derinlik algılayıcısı çerçevesine göre tahminlerdir. Konum milimetre olarak belirtilir. Yön, normalleştirilmiş bir dörde olarak ifade edilir.

## <a name="joint-coordinates"></a>Birleşik koordinatlar

Her bir eklem formunun kendi ortak koordinat sistemine ait konumu ve yönü. Tüm Birleşik koordinat sistemleri, derinlik Kamerası 3B koordinat sistemine göre mutlak koordinat sistemleridir.

> [!NOTE]
> Eklem koordinatları eksen yönindedir. Eksen yönü, ticari avatarlar, oyun motorları ve işleme yazılımıyla yaygın olarak kullanılır. Eksen yönünün kullanılması yansıtılmış hareketleri basitleştirir, örneğin her iki kollu bir 20 derece yükseltin.

![Birleşik koordinatlar](./media/concepts/joint-coordinates.png)

## <a name="joint-hierarchy"></a>Eklem hiyerarşisi

Bir çatı, gövdenin extremities Merkez merkezinden akışı olan 32 birleşme bilgilerini içerir. Her bağlantı (kemik) üst öğeyi bir alt Ekle bağlar. Şekil, insanların gövdesiyle ilgili olan Birleşik konumları ve bağlantıları gösterir.

![Eklem hiyerarşisi](./media/concepts/joint-hierarchy.png)

Aşağıdaki tablo, standart Birleşik bağlantıları sıralar.

|Dizin oluşturma |Birleşik ad     | Üst öğe birleşme   |
|------|---------------|----------------|
| 0    |PELVIS         | -              |
| 1    |SPINE_NAVAL    | PELVIS         |
| 2    |SPINE_CHEST    | SPINE_NAVAL    |
| 3    |Boynu           | SPINE_CHEST    |
| 4    |CLAVICLE_LEFT  | SPINE_CHEST    |
| 5    |SHOULDER_LEFT  | CLAVICLE_LEFT  |
| 6    |ELBOW_LEFT     | SHOULDER_LEFT  |
| 7    |WRIST_LEFT     | ELBOW_LEFT     |
| 8    |HAND_LEFT      | WRIST_LEFT     |
| 9    |HANDTIP_LEFT   | HAND_LEFT      |
| 10   |THUMB_LEFT     | WRIST_LEFT     |
| 11   |CLAVICLE_RIGHT | SPINE_CHEST    |
| 12   |SHOULDER_RIGHT | CLAVICLE_RIGHT |
| 13   |ELBOW_RIGHT    | SHOULDER_RIGHT |
| 14   |WRIST_RIGHT    | ELBOW_RIGHT    |
| 15   |HAND_RIGHT     | WRIST_RIGHT    |
| 16   |HANDTIP_RIGHT  | HAND_RIGHT     |
| 17   |THUMB_RIGHT    | WRIST_RIGHT    |
| 18   |HIP_LEFT       | PELVIS         |
| 19   |KNEE_LEFT      | HIP_LEFT       |
| 20   |ANKLE_LEFT     | KNEE_LEFT      |
| 21   |FOOT_LEFT      | ANKLE_LEFT     |
| 22   |HIP_RIGHT      | PELVIS         |
| 23   |KNEE_RIGHT     | HIP_RIGHT      |
| 24   |ANKLE_RIGHT    | KNEE_RIGHT     |
| 25   |FOOT_RIGHT     | ANKLE_RIGHT    |
| 26   |HEAD           | Boynu           |
| 27   |NU           | HEAD           |
| 28   |EYE_LEFT       | HEAD           |
| 29   |EAR_LEFT       | HEAD           |
| 30   |EYE_RIGHT      | HEAD           |
| 31   |EAR_RIGHT      | HEAD           |

## <a name="next-steps"></a>Sonraki adımlar

[Gövde izleme dizin eşlemesi](body-index-map.md)
