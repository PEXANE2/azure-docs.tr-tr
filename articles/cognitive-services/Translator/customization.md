---
title: Çeviri Özelleştirme - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Tercih ettiğiniz terminoloji ve stili kullanarak kendi makine çeviri sisteminizi oluşturmak için Microsoft Translator Hub'ını kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: be9c06d45da4d83c26f82343c9cb7b19ba19b4ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71257615"
---
# <a name="customize-your-text-translations"></a>Metin çevirilerinizi özelleştirme

Microsoft Özel Çevirmen, kullanıcıların Çevirmen Metin API'sini (yalnızca sürüm 3) kullanarak metni çevirirken Microsoft Translator'ın gelişmiş sinir makinesi çevirisini özelleştirmelerine olanak tanıyan Microsoft Translator hizmetinin özelliğidir.

Bu özellik, [Bilişsel Hizmetler Konuşması](https://docs.microsoft.com/azure/cognitive-services/speech-service/)ile kullanıldığında konuşma çevirisini özelleştirmek için de kullanılabilir.

## <a name="custom-translator"></a>Özel Çevirmen

Custom Translator ile, kendi işinizde ve endüstrinizde kullanılan terminolojiyi anlayan nöral çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sistemi daha sonra varolan uygulamalara, iş akışlarına ve web sitelerine entegre olur.

### <a name="how-does-it-work"></a>Nasıl çalışır?

Etki alanına özel terminolojinizi ve stilinizi standart bir çeviri sisteminden daha iyi yansıtan bir çeviri sistemi oluşturmak için önceden çevrilmiş belgelerinizi (broşürler, web sayfaları, belgeler, vb.) kullanın. Kullanıcılar TMX, XLIFF, TXT, DOCX ve XLSX belgelerini yükleyebilir.  

Sistem ayrıca belge düzeyinde paralel olan ancak henüz cümle düzeyinde hizalanmış olmayan verileri de kabul eder. Kullanıcılar birden çok dilde ancak ayrı belgelerde aynı içeriğin sürümlerine erişebiliyorsa, Özel Çevirmen cümleleri belgeler arasında otomatik olarak eşleştirebilir.  Sistem, çevirileri geliştirmek için paralel eğitim verilerini tamamlamak için her iki dilde veya her iki dilde de tek dilli verileri kullanabilir.

Özelleştirilmiş sistem daha sonra kategori parametresini kullanarak Microsoft Translator Text API'ye düzenli bir çağrı ile kullanılabilir.

Uygun eğitim veritürü ve miktarı göz önüne alındığında, Özel Çevirmen kullanarak çeviri kalitesinde 5 ile 10 arasında kazanç veya daha fazla BLEU noktası beklemek nadir değildir.

Mevcut verilere dayalı çeşitli özelleştirme düzeyleri hakkında daha fazla ayrıntı [Özel Çevirmen Kullanım Kılavuzu'nda](https://aka.ms/CustomTranslatorDocs)bulunabilir.


## <a name="microsoft-translator-hub"></a>Microsoft Translator Hub

> [!NOTE]
> Microsoft Translator Hub,17 Mayıs 2019'da kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihlerini görüntüleyin.](https://www.microsoft.com/translator/business/hub/)  

## <a name="custom-translator-versus-hub"></a>Özel Çevirmen ve Hub

|   | **Hub** | **Özel Çevirmen**|
|:-----|:----:|:----:|
|Özelleştirme Özelliği Durumu   | Genel Kullanılabilirlik  | Genel Kullanılabilirlik |
| Metin API sürümü  | Yalnızca V2   | Yalnızca V3 |
| SMT özelleştirme | Evet   | Hayır |
| NMT özelleştirme | Hayır    | Evet |
| Yeni birleştirilmiş Konuşma hizmetleri özelleştirme | Hayır    | Evet |
| [İz Yok](https://www.aka.ms/notrace) | Evet  | Evet |

## <a name="collaborative-translations-framework"></a>İşbirlikçi Çeviriler Çerçevesi

> [!NOTE]
> 1 Şubat 2018 itibariyle, AddTranslation() ve AddTranslationArray() artık Translator Text API V2.0 ile kullanılamaz. Bu yöntemler başarısız olacak ve hiçbir şey yazılacaktır. Çevirmen Metin API V3.0 bu yöntemleri desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Çevirmen kullanarak özelleştirilmiş bir dil sistemi ayarlama](https://aka.ms/CustomTranslatorDocs)
