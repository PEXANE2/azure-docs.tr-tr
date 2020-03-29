---
title: Sık Sorulan Sorular - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Özel Çevirmeni hakkında sık sorulan soruların yanıtları yer almaktadır.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: reference
ms.openlocfilehash: 97d399f4a8ec704fd90eb6c49f0835be7e9e4537
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73836606"
---
# <a name="custom-translator-frequently-asked-questions"></a>Özel Çevirmen sık sorulan sorular

Bu makale, [Özel Çevirmen](https://portal.customtranslator.azure.ai)hakkında sık sorulan soruların yanıtlarını içerir.

## <a name="what-are-the-current-restrictions-in-custom-translator"></a>Özel Çevirmen'deki mevcut kısıtlamalar nelerdir?

Dosya boyutu, model eğitimi ve model dağıtımıyla ilgili kısıtlamalar ve sınırlamalar vardır. Özel Çevirmen'de bir model oluşturmak için eğitiminizi ayarlarken bu kısıtlamaları aklınızda bulundurun.

- Gönderilen dosyaların boyutu 100 MB'dan az olmalıdır.
- Tek dilli veriler desteklenmez.

## <a name="when-should-i-request-deployment-for-a-translation-system-that-has-been-trained"></a>Eğitilmiş bir çeviri sistemi için dağıtım ını ne zaman talep etmeliyim?

Projeniz için en uygun çeviri sistemini oluşturmak için çeşitli eğitimler gerekebilir. BLEU puanı ve/veya test sonuçları tatmin edici değilse, daha fazla eğitim verisi veya daha dikkatli filtrelenmiş veri kullanmayı deneyebilirsiniz. Çevirmek istediğiniz terminoloji ve malzeme stilini tam olarak temsil etmek için ayar setinizi ve test setinizi tasarlarken katı ve dikkatli olmalısınız. Eğitim verilerinizi oluştururken daha liberal olabilir ve farklı seçenekler le denemeler yapabilirsiniz. Sistem test sonuçlarınızdaki çevirilerden memnun olduğunuzda, eğitilmiş sisteminizi geliştirmek için eğitime ekleyecek daha fazla veriniz olmadığında ve eğitilmiş modele API'ler aracılığıyla erişmek istediğinizde sistem dağıtımı isteyin.

## <a name="how-many-trained-systems-can-be-deployed-in-a-project"></a>Bir projede kaç tane eğitilmiş sistem dağıtılabilir?

Proje başına yalnızca bir eğitilmiş sistem dağıtılabilir. Projeniz için uygun bir çeviri sistemi oluşturmak için çeşitli eğitimler gerekebilir ve size en iyi sonucu veren bir eğitimin dağıtılmasını talep etmenizi öneririz. Eğitimin kalitesini BLEU puanına göre belirleyebilirsiniz (daha yüksektir) ve çevirilerin kalitesinin dağıtım için uygun olduğuna karar vermeden önce gözden geçirenlere danışarak.

## <a name="when-can-i-expect-my-trainings-to-be-deployed"></a>Eğitimlerimin ne zaman dağıtılmasını bekleyebilirim?

Dağıtım genellikle bir saatten az sürer.

## <a name="how-do-you-access-a-deployed-system"></a>Dağıtılan bir sisteme nasıl erişilir?

Dağıtılan sistemlere CategoryID belirterek Microsoft Translator Text API V3 üzerinden erişilebilir. Çevirmen Metin API'si hakkında daha fazla bilgiyi [API Başvuru](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) web sayfasında bulabilirsiniz.

## <a name="how-do-i-skip-alignment-and-sentence-breaking-if-my-data-is-already-sentence-aligned"></a>Verilerim zaten cümle hizalanmışsa hizalama ve cümle bölmeyi nasıl atlarım?

Özel Çevirmen, TMX dosyaları ve `.align` uzantılı metin dosyaları için cümle hizalama ve cümle bozmayı atlar. `.align`dosyalar, kullanıcılara, mükemmel bir şekilde hizalanmış ve başka işlem gerekmeyen dosyalar için Özel Çevirmen'in cümle kesme ve hizalama işlemini atlama seçeneği sunar. Uzantıyı `.align` yalnızca mükemmel şekilde hizalanmış dosyalar için kullanmanızı öneririz.

Ayıklanan cümle sayısı iki dosyayla aynı temel adla eşleşmiyorsa, Özel Çevirmen `.align` yine dosyalardaki cümle hizalayıcısını çalıştıracaktır.

## <a name="i-tried-uploading-my-tmx-but-it-says-document-processing-failed"></a>Benim TMX yükleme çalıştı, ama "belge işleme başarısız" diyor.

TMX'in TMX 1.4b Belirtimine uygun <https://www.gala-global.org/tmx-14b>olduğundan emin olun.
