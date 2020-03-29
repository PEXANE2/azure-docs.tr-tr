---
title: Limitler ve kotalar - Özel Vizyon Hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, farklı lisans anahtarları türleri ve Özel Görme Hizmeti için sınırlar ve kotalar hakkında açıklanmaktadır.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: anroth
ms.openlocfilehash: 5f481ebf219eea8ae31e9802144bbf771a24e8b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79081786"
---
# <a name="limits-and-quotas"></a>Limitler ve kotalar

Custom Vision hizmeti için iki farklı anahtar katmanı vardır. Azure portalı üzerinden F0 (ücretsiz) veya S0 (standart) aboneliğine kaydolabilirsiniz. Fiyatlandırma ve işlemlerle ilgili ayrıntılar için ilgili [Bilişsel Hizmetler Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) bakın.

Proje başına eğitim görüntülerinin ve proje başına etiket sayısının S0 projeleri için zaman içinde artması beklenmektedir.

||**F0**|**S0**|
|-----|-----|-----|
|Projeler|2|100|
|Proje başına eğitim görüntüleri |5.000|100.000|
|Tahminler / ay|10,000 |Sınırsız|
|Etiketler / proje|50|500|
|Yineleme |10|10|
|Etiket başına Min etiketli görüntüler, Sınıflandırma (50+ önerilir) |5|5|
|Etiket başına Min etiketli görüntüler, Nesne Algılama (50+ önerilir)|15|15|
|Tahmin görüntüleri ne kadar süreyle saklanır|30 gün|30 gün|
|Depolama ile [tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleri (Saniyebaşına İşlemler)|2|10|
|Depolama olmadan [tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleri (Saniyebaşına İşlemler)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API Saniyede çağırır)|2|10|
|[Diğer API aramaları](https://go.microsoft.com/fwlink/?linkid=865446) (Saniyebaşına İşlemler)|10|10|
|Kabul edilen görüntü türleri|jpg, png, bmp, gif|jpg, png, bmp, gif|
|Piksellerde min görüntü yüksekliği/genişliği|256 (nota bakınız)|256 (nota bakınız)|
|Piksellerde maksimum görüntü yüksekliği/genişliği|sınırsız|sınırsız|
|Maksimum görüntü boyutu (eğitim resim yükleme) |6 MB|6 MB|
|Maksimum görüntü boyutu (tahmin)|4 MB|4 MB|
|Nesne algılama eğitim görüntüsü başına maksimum bölgeler|300|300|
|Sınıflandırma görüntüsü başına maksimum etiketler|100|100|

> [!NOTE]
> 256 pikselden küçük görüntüler kabul edilir, ancak lüks bir şekilde ölçeklendirilir.
