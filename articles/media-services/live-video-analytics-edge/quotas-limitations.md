---
title: IoT Edge kotalar ve sınırlamalar üzerinde canlı video analizi-Azure
description: Bu makalede, IoT Edge kotaları ve sınırlamaları hakkında canlı video analizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 05/22/2020
ms.openlocfilehash: 68c7b91bb1051348b5a8e52f841d443894f0a632
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97400533"
---
# <a name="quotas-and-limitations"></a>Kotalar ve sınırlamalar

Bu makalede IoT Edge modülündeki canlı video analizlerinin kotaları ve sınırlamaları numaralandırılır.

## <a name="maximum-period-of-disconnected-use"></a>Bağlantısı kesilen kullanım süresi üst sınırı

Edge modülü, internet bağlantısının geçici kaybını karşılayabilir. Modülün 36 saatten uzun bir kesilmeden bağlantısı kesildiyse, çalışmakta olan tüm grafik örneklerini devre dışı bırakır. Diğer tüm doğrudan Yöntem çağrıları engellenir.

Edge modülünü işlemsel bir duruma getirmek için, modülün Azure Media Service hesabıyla başarıyla iletişim kurabilmesi için internet bağlantısını geri yüklemeniz gerekir.

## <a name="maximum-number-of-graph-instances"></a>Maksimum grafik örneği sayısı

Modül başına en çok 1000 grafik örneği (Graphınstanceset aracılığıyla oluşturulur) desteklenir.

## <a name="maximum-number-of-graph-topologies"></a>Maksimum Graf topolojisi sayısı

Modül başına en çok 50 Graf topolojisi (Graphtopologyıset aracılığıyla oluşturulur) desteklenir.

## <a name="limitations-on-graph-topologies-at-preview"></a>Önizleme aşamasında grafik topolojileriyle ilgili sınırlamalar

Önizleme sürümü ile, farklı düğümlerdeki sınırlamalar bir medya grafik topolojisinde birbirine bağlanabilir.

* RTSP kaynağı
   * Grafik topolojisi başına yalnızca bir RTSP kaynağına izin verilir.
* Hareket algılama işlemcisi
   * RTSP kaynağından hemen aşağı akış olmalıdır.
   * Bir HTTP veya gRPC uzantı işlemcisinin aşağı akış sürümü kullanılamaz.
* Sinyal kapısı işlemcisi
   * RTSP kaynağından hemen aşağı akış olmalıdır.
* Varlık havuzu 
   * RTSP kaynağı veya sinyal kapısı işlemcisinden hemen aşağı akış olmalıdır.
* Dosya havuzu
   * Sinyal kapısı işlemcisinden hemen aşağı akış olmalıdır.
   * Bir HTTP veya gRPC uzantısı işlemcisinin veya hareket algılama işlemcisinin hemen bir aşağı akış olamaz
* IoT Hub havuzu
   * IoT Hub bir kaynağın hemen bir aşağı akış olamaz.

## <a name="limitations-on-media-service-operations-at-preview"></a>Önizlemede medya hizmeti işlemlerine yönelik sınırlamalar

Önizleme sürümü sırasında IoT Edge canlı video analizi aşağıdakileri desteklemez:

* Medya hizmeti hesabını bir abonelikten diğerine kesinti olmadan geçirme özelliği.
* Medya Hizmeti hesabıyla birden fazla depolama hesabı kullanma özelliği.
* Yeniden başlatma olmadan, modülün istenen özelliklerindeki hizmet sorumlusu bilgilerini dinamik olarak değiştirme özelliği.

Yalnızca RTSP protokolünü destekleyen IP kameralarını kullanabilirsiniz. [ONVIF uyumlu ürünler](https://www.onvif.org/conformant-products) sayfasında RTSP 'YI destekleyen IP kameralarını bulabilirsiniz. Profiller G, S veya T ile uyumlu olan cihazları arayın.

Ayrıca, bu kameraları H. lenebilir video ve AAC ses kullanmak için yapılandırmanız gerekir. Diğer codec bileşenleri Şu anda desteklenmiyor. 

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](overview.md)
