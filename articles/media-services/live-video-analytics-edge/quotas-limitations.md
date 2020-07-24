---
title: IoT Edge kotaları üzerinde canlı video analizi-Azure
description: Bu makalede, IoT Edge kotaları ve sınırlamaları hakkında canlı video analizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 05/22/2020
ms.openlocfilehash: 90141fa850c9ab3e3abbea15001249da0736ac45
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091819"
---
# <a name="quotas-and-limitations"></a>Kotalar ve sınırlamalar

Bu makalede IoT Edge modülündeki canlı video analizlerinin kotaları ve sınırlamaları numaralandırılır.

## <a name="maximum-period-of-disconnected-use"></a>Bağlantısı kesilen kullanım süresi üst sınırı

Edge modülü, ağ bağlantısının geçici kaybını karşılayabilir. Modülün 36 saatten uzun bir kesilmeden bağlantısı kesilirse, çalışmakta olan tüm grafik örneklerini devre dışı bırakır ve daha fazla doğrudan yöntem çağrısı engellenir.

Edge modülünü işlemsel bir duruma getirmek için, ağ bağlantısını geri yüklemeniz ve modülün Azure Media Service hesabıyla başarıyla iletişim kurabilmesi gerekir.

## <a name="maximum-number-of-graph-instances"></a>Maksimum grafik örneği sayısı

Modül başına en fazla 1000 Graf örneğine sahip olabilirsiniz (Graphınstanceset aracılığıyla oluşturulur).

## <a name="maximum-number-of-graph-topologies"></a>Maksimum Graf topolojisi sayısı

Modül başına en fazla 50 Graf topolojisi (Graphtopologyıset aracılığıyla oluşturulur) kullanabilirsiniz.

## <a name="limitations-on-graph-topologies-at-preview"></a>Önizleme aşamasında grafik topolojileriyle ilgili sınırlamalar

Önizleme sürümü ile, farklı düğümlerdeki sınırlamalar bir medya grafik topolojisinde birbirine bağlanabilir.

* RTSP kaynağı
   * Grafik topolojisi başına yalnızca bir RTSP kaynağına izin verilir.
* Çerçeve hızı filtre işlemcisi
   * RTSP kaynağı veya hareket algılama işlemcisinden hemen aşağı akış olmalıdır.
   * Bir HTTP uzantısı işlemcisinin aşağı akış sürümü kullanılamaz.
   * Hareket algılama işlemcisinden yukarı akış olamaz.
* HTTP uzantısı işlemcisi
   * Grafik topolojisi başına en çok bir işlemci olabilir.
* Hareket algılama işlemcisi
   * RTSP kaynağından hemen aşağı akış olmalıdır.
   * Grafik topolojisi başına en çok bir işlemci olabilir.
   * Bir HTTP uzantısı işlemcisinin aşağı akış sürümü kullanılamaz.
* Sinyal kapısı işlemcisi
   * RTSP kaynağından hemen aşağı akış olmalıdır.
* Varlık havuzu 
   * RTSP kaynağı veya sinyal kapısı işlemcisinden hemen aşağı akış olmalıdır.
* Dosya havuzu
   * Sinyal kapısı işlemcisinden hemen aşağı akış olmalıdır.
   * HTTP uzantısı işlemcisinin veya hareket algılama işlemcisinin hemen bir aşağı akış yapılamaz
* IoT Hub havuzu
   * IoT Hub bir kaynağın hemen bir aşağı akış olamaz.

Hem hareket algılama hem de filtre hızı işlemci düğümleri kullanılıyorsa, bu değerler RTSP kaynak düğümüne önde gelen düğümlerin zincirinde olmalıdır.

## <a name="limitations-on-media-service-operations-at-preview"></a>Önizlemede medya hizmeti işlemlerine yönelik sınırlamalar

Önizleme sürümü sırasında IoT Edge canlı video analizi aşağıdakileri desteklemez:

* Medya hizmeti hesabını bir abonelikten diğerine kesinti olmadan geçirme özelliği.
* Medya Hizmeti hesabıyla birden fazla depolama hesabı kullanma özelliği.
* Yeniden başlatma olmadan, modülün istenen özelliklerindeki hizmet sorumlusu bilgilerini dinamik olarak değiştirme özelliği.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](overview.md)
