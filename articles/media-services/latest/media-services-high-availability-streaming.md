---
title: Azure Medya Hizmetleri yüksek kullanılabilirlik akışı
description: Bölgesel bir veri merkezi kesintisi veya hatası oluşursa, ikincil bir Medya Hizmetleri hesabına nasıl geçemeyeceğinızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78899236"
---
# <a name="media-services-high-availability-streaming"></a>Medya Hizmetleri yüksek kullanılabilirlik akışı

Azure Medya Hizmetleri, bölgesel bir veri merkezi kesintisi veya altta yatan bileşen veya bağımlı hizmetlerin başarısız olması durumunda şu anda hizmetin anında başarısızlığını sağlamaz. Bu makalede, video-on-demand çapraz bölge akışı oluşturmak için nasıl rehberlik verir.

## <a name="prerequisites"></a>Ön koşullar

Bölgeler arası kodlama sistemi oluşturma yı gözden [geçirin](media-services-high-availability-encoding.md)

## <a name="how-to-build-video-on-demand-cross-region-streaming"></a>Video-on-demand çapraz bölge akışı oluşturma 

* Video-on-demand çapraz bölge akışı [Varlıklar](assets-concept.md)çoğaltılan içerir , [İçerik Anahtar İlkeleri](content-key-policy-concept.md) (kullanılırsa), Akış [İlkeleri,](streaming-policy-concept.md)ve [Akış Locators](streaming-locators-concept.md). 
* Her iki bölgede de ilkeleri oluşturmanız ve güncel tutmanız gerekir. 
* Akış bulucularını oluşturduğunuzda, aynı Konumbelirleyici Kimliği değerini, ContentKey ID değerini ve ContentKey değerini kullanmak istersiniz.  
* İçeriği kodlıyorsanız, içeriği A bölgesindeki kodlamanız ve yayımlamanız, ardından kodlanmış içeriği B bölgesine kopyalamanız ve A bölgesinden aynı değerleri kullanarak yayımlamanız önerilir.
* Trafik Yöneticisi'ni, ana bilgisayar adlarında kaynak ve anahtar teslim hizmeti için kullanabilirsiniz (Media Services yapılandırmasında bu özel bir anahtar sunucu URL'si gibi görünür).

## <a name="next-steps"></a>Sonraki adımlar

Kontrol etme:

* [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akış](stream-files-dotnet-quickstart.md)
* [kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
