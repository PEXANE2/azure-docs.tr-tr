---
title: Bilişsel hizmetler işleme işlemi
titleSuffix: Azure Cognitive Services
description: Yeni bilişsel hizmetler kapsayıcıları ve API 'Lerine erken erişim için nasıl uygulanacağını öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: aahi
ms.openlocfilehash: a161cc0675eedee50608b8d6c288c57147fd6d23
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599518"
---
# <a name="gating-process-for-azure-cognitive-services"></a>Azure bilişsel hizmetler için işleme işlemi

> [!NOTE]
> Bir hizmet sunumu geçişli önizlemeyi tamamladıktan sonra, erişim için bir uygulama gerektirmeyen "açılmamış" genel önizlemeye gider. Önizleme işleminden sonra, genel kullanıma açık (GA) olarak serbest bırakılır.

Yeni Azure bilişsel hizmetler teklifleri tanıtıldığında, müşterilerin bir uygulama aracılığıyla erişim istemesi gereken kapılı bir önizlemeye gider. Bu işleme işlemi, yaygın olarak kullanılabilir olmaları için hizmet tekliflerindeki iyileştirmeler için fırsatları belirlemenize yardımcı olur. 

Bu makale, geçitli bilişsel hizmetler teklifleri için uygulama sürecinde size kılavuzluk eder.

## <a name="eligibility-and-approval-process"></a>Uygunluk ve onay süreci

İşleme işlemi, müşteri ihtiyaçlarını ölçmek ve daha iyi anlamak için ilgi çekici hale getirmenize yardımcı olur. Microsoft ekibi, Microsoft ticari müşterilerinin [uygulamalarını](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u) geçerli bir Azure aboneliği ve geçerli bir iş senaryosu kabul eder. Müşterilerin uygulamalarına şu durumlarda Reddedilenler olasılığı vardır:

 - Bunlar herhangi bir kuruluşla ilişkilendirilmez
 - Bu, geçerli bir Azure aboneliğine sahip değildir
 - Uygulama kişisel e-postayla gönderildi ( @hotmail.com , @gmail.com , @yahoo.com )
 - Uygun gerekçe veya iş senaryosu sağlanmadı

Farklı müşteri segmentlerinden talep verildiğinde, onay işlemini hızlandırmaya çalışıyoruz. Ancak, bir zaman çizelgesine teslim edilemez. Bir karar alındıktan sonra, Microsoft ekibi, sonraki adımlar için sizinle ve hesap yönetimi ekibinize başvuracaktır. Öğrendiğimiz ve hamanence bizim için teşekkür ederiz.

Uygulama onaylanırsa, Microsoft ekibi ayrıntılar, belgeler ve kılavuzluk içeren bir e-posta gönderir. Bilişsel hizmetler fiyatlandırma ayrıntıları [burada](https://azure.microsoft.com/pricing/details/cognitive-services/)bulunabilir.


Şu anda aşağıdaki hizmetler, başarı işlemi aracılığıyla sunulmaktadır:

## <a name="gated-web-apis"></a>Geçitli Web API 'Leri

|Hizmet  |
|---------|
|Anomali algılayıcısı v2     | 

## <a name="gated-online-containers"></a>Geçitli çevrimiçi kapsayıcılar

| Hizmet                             | Kapsayıcı (ler)                                                                  |
|-------------------------------------|-------------------------------------------------------------------------------|
| [Görüntü İşleme][cv-containers]    | Okuma                                                                          |
| [Yüz Tanıma][fa-containers]               | Yüz Tanıma                                                                          |
| [Form Tanıma][fr-containers]    | Form Tanıma                                                               |
| [Konuşma Hizmeti API’si][sp-containers] | Konuşmayı metne dönüştürme (özel ve standart) ve metinden konuşmaya (özel ve standart) |
| [Translator Metin Çevirisi][tt-containers]    | Translator Metin Çevirisi                                                               |

> [!IMPORTANT]
> Bir hizmet veya Kapsayıcılı teklif listelenmiyorsa, bu, kapıya da kullanılamaz olur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Geçitli hizmetlere kaydolun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u)

[ad-containers]: ./anomaly-detector/anomaly-detector-container-howto.md
[cv-containers]: ./computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ./face/face-how-to-install-containers.md
[fr-containers]: ./form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ./luis/luis-container-howto.md
[sp-containers]: ./speech-service/speech-container-howto.md
[ta-containers]: ./text-analytics/how-tos/text-analytics-how-to-install-containers.md
[tt-containers]: ./translator/how-to-install-containers.md
