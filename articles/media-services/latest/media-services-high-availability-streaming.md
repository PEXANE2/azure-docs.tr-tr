---
title: Yüksek kullanılabilirlik akışı Azure Media Services
description: Bölgesel bir veri merkezi kesintisi veya hatası oluşursa ikincil Media Services hesaba yük devretme hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: 1492dd392eabc4331f8e3d4604fb245a89dedff5
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899236"
---
# <a name="media-services-high-availability-streaming"></a>Yüksek kullanılabilirlik akışı Media Services

Azure Media Services Şu anda, bölgesel bir veri merkezi kesintisi veya temeldeki bileşen ya da bağımlı hizmetler hatası varsa hizmetin anında yük devretmesini sağlamaz. Bu makalede, isteğe bağlı video çapraz bölge akışını nasıl oluşturabileceğiniz konusunda rehberlik sunulmaktadır.

## <a name="prerequisites"></a>Önkoşullar

[Çapraz bölgesel kodlama sistemi oluşturma](media-services-high-availability-encoding.md) konusunu inceleyin

## <a name="how-to-build-video-on-demand-cross-region-streaming"></a>İsteğe bağlı video çapraz bölge akışı oluşturma 

* İsteğe bağlı video çapraz bölge akışı, [varlıkları](assets-concept.md)çoğaltma, [içerik anahtarı ilkeleri](content-key-policy-concept.md) (kullanılıyorsa), [akış ilkeleri](streaming-policy-concept.md)ve [akış bulıcıları](streaming-locators-concept.md)içerir. 
* Her iki bölgede de ilke oluşturmanız ve bunları güncel tutmanız gerekir. 
* Akış bulucuyu oluştururken, aynı Bulucu KIMLIĞI değerini, ContentKey ID değerini ve ContentKey değerini kullanmak isteyeceksiniz.  
* İçeriği kodladıysanız, A bölgesindeki içeriği kodlamak ve yayımlamak, sonra kodlanmış içeriği B bölgesine kopyalamanız ve A bölgesiyle aynı değerleri kullanarak yayımlamanız önerilir.
* Kaynak ve anahtar teslim hizmeti için ana bilgisayar adlarında Traffic Manager kullanabilirsiniz (Media Services yapılandırmada bu, özel bir anahtar sunucusu URL 'SI gibi görünür).

## <a name="next-steps"></a>Sonraki adımlar

Kontrol etme:

* [Öğretici: URL 'yi temel alarak uzak bir dosyayı kodlayın ve videoyu akışa sunun](stream-files-dotnet-quickstart.md)
* [kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
