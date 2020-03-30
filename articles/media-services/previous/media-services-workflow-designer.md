---
title: İş Akışı Tasarımcısı ile Gelişmiş Kodlama İş Akışları Oluşturun | Microsoft Dokümanlar
description: İş Akışı Tasarımcısı ile gelişmiş kodlama iş akışları oluşturma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: anilmur
ms.reviewer: juliako;johndeu
ms.openlocfilehash: 4dceb558532305c6d2e84563e25ab05508423090
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72801945"
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>İş Akışı Tasarımcısı ile Gelişmiş Kodlama İş Akışları Oluşturma  
## <a name="overview"></a>Genel Bakış
**İş Akışı Tasarımcısı,** **Media Encoder Premium İş Akışı**ile kodlama için özel iş akışları tasarlamak ve oluşturmak için kullanılan bir Windows masaüstü aracıdır.
İş akışı tasarımcısı aracının gücünü kullanarak, **Media Encoder Premium'da**çalışacak karmaşık iş akışları tasarlayabilir ve oluşturabilirsiniz.  

İş akışları, giriş kaynağı dosyasının özelliklerine göre müşteri karar mantığını ve dallanmayı içerebilir. En karmaşık kodlama görevlerini bile bulutta tekrarlamayı ve özelleştirmeyi kolaylaştırmak için, geçersiz kılınabilir özelliklere ve dinamik değerlere sahip iş akışları oluşturabilirsiniz.

Oluşturabileceğiniz örnek iş akışları şunlardır:

* Çözüm için kaynak içeriği inceleyen ve yalnızca istenen çıktı parçalarını kodlayan karar tabanlı iş akışları.  Bu, kaynak içeriği yanlışlıkla yükselterek oluşturulacak boşa harcanan parçaları ortadan kaldırarak yararlıdır.
* Altyazıları, bindirmeleri ve birleştirme içeriğini desteklemek için birden çok giriş dosyası kullanılabilir. 

Bu araç, [yayımlanmış iş akışlarımızdan](media-services-workflow-designer.md#existing_workflows)herhangi birini değiştirmek için de kullanılabilir. 

> [!NOTE]
> İş Akışı Tasarımcısı aracının kopyasını almak mepd@microsoft.comiçin lütfen .

Bir iş akışı dosyası oluşturulduktan sonra, varlık olarak yüklenebilir ve daha sonra ortam dosyalarını kodlamak için kullanılabilir. **.NET**kullanarak **Media Encoder Premium İş Akışı** ile nasıl kodlanır hakkında bilgi için, Media [Encoder Premium İş Akışı ile Gelişmiş kodlama](media-services-encode-with-premium-workflow.md)bakın.

## <a name="modify-existing-workflows"></a><a id="existing_workflows"></a>Varolan iş akışlarını değiştirme
Varsayılan [yayımlanmış iş akışları](media-services-workflow-designer.md#existing_workflows) tasarımcı aracı kullanılarak değiştirilebilir. Varsayılan iş akışı dosyalarını [buradan](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)alabilirsiniz. Klasör ayrıca bu dosyaların açıklamasını da içerir.

Aşağıdaki videolar tasarımcının nasıl kullanılacağını göstermektedir.

### <a name="day-1--getting-started"></a>Gün 1 – Başlarken
Gün 1 video kapakları:

* Tasarımcıya Genel Bakış
* Temel İş Akışları – "Hello World"
* Azure Medya Hizmetleri akışıyla kullanılmak üzere birden çok çıkışlı MP4 dosyası oluşturma

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>2. Gün
Gün 2 video kapakları:

* Değişen Kaynak dosya senaryoları – ses işleme
* Gelişmiş Mantık ile İş Akışları
* Grafik aşamaları

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>3. Gün
Gün 3 video kapakları:

* İş Akışları/Planlar içinde komut dosyası
* Geçerli Kodlayıcı ile kısıtlamalar
* Soru-Cevap

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

[Yeni destek isteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) yönlendirerek destek bileti açabilirsiniz

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Premium Encoder İş Akışı Tasarımcısı Eğitim Videoları](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

