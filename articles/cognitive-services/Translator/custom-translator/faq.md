---
title: Sık sorulan sorular-özel çevirici
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure bilişsel hizmetler özel çevirmeni hakkında sık sorulan soruların yanıtlarını içerir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: reference
ms.openlocfilehash: fefd3fcd82454d505099f83944b0e251b71410f0
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996916"
---
# <a name="custom-translator-frequently-asked-questions"></a>Özel Translator hakkında sık sorulan sorular

Bu makale, [özel çevirici](https://portal.customtranslator.azure.ai)hakkında sık sorulan soruların yanıtlarını içerir.

## <a name="what-are-the-current-restrictions-in-custom-translator"></a>Özel Çevirmendeki geçerli kısıtlamalar nelerdir?

Dosya boyutu, model eğitimi ve model dağıtımına göre sınırlamalar ve sınırlamalar vardır. Özel çevirmende bir model oluşturmak için öğreticinizi ayarlarken bu kısıtlamaları göz önünde bulundurun.

- Gönderilen dosyalar boyutu 100 MB 'tan az olmalıdır.
- Monolingual verileri desteklenmez.

## <a name="when-should-i-request-deployment-for-a-translation-system-that-has-been-trained"></a>Eğitilen bir çeviri sistemi için ne zaman dağıtım isteymem gerekir?

Projeniz için en uygun çeviri sistemini oluşturmak birkaç önlem alabilir. BLEU puanı ve/veya test sonuçları tatmin edici değilse, daha fazla eğitim verisi veya daha dikkatli Filtrelenmiş veriler kullanmayı denemek isteyebilirsiniz. Ayarlama kümesini ve test kümesini tasarlarken, çevirmek istediğiniz terimleri ve malzeme stillerinin tam olarak temsili olacak şekilde katı ve dikkatli olmanız gerekir. Eğitim verilerinizi oluşturma konusunda daha fazla özgürlük olabilir ve farklı seçeneklerle denemeler yapabilirsiniz. Sistem dağıtımı iste sistem test sonuçlarınızda çevirileri karşıladığınızda, eğitilen sisteminizin geliştirilmesi için eğitime eklenecek daha fazla veri yok ve API 'Ler aracılığıyla eğitilen modele erişmek istiyorsunuz.

## <a name="how-many-trained-systems-can-be-deployed-in-a-project"></a>Bir projede kaç eğitilen sistem dağıtılabilecek?

Her proje için yalnızca bir eğitilen sistem dağıtılabilir. Projeniz için uygun bir çeviri sistemi oluşturmak için çeşitli olanaklar alabilir ve size en iyi sonucu veren bir eğitim dağıtımı istemeniz önerilir. Eğitimin puanına (daha yüksek) göre eğitim kalitesini ve çevirilerin kalitesinin dağıtıma uygun olduğuna karar vermeden önce gözden geçirenlere danışarak saptayabilirsiniz.

## <a name="when-can-i-expect-my-trainings-to-be-deployed"></a>Gelişimin dağıtılmasını ne zaman bekleyebilir?

Dağıtım genellikle bir saatten daha az sürer.

## <a name="how-do-you-access-a-deployed-system"></a>Dağıtılan bir sisteme nasıl erişirsiniz?

Dağıtılmış sistemlere CategoryID belirtilerek Microsoft Translator v3 aracılığıyla erişilebilir. Çevirici hakkında daha fazla bilgi [API başvuru](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) Web sayfasında bulunabilir.

## <a name="how-do-i-skip-alignment-and-sentence-breaking-if-my-data-is-already-sentence-aligned"></a>Verilerim zaten tümce hizalanmışsa hizalama ve tümce Nasıl yaparım? atlayın mi?

Özel çevirmen, TMX dosyaları ve uzantılı metin dosyaları için tümce hizalamasını ve tümce atlamalarını atlar `.align` . `.align`dosyalar, kullanıcılara kusursuz hizalanmış dosyalar için özel çevirmenin tümce ve hizalama sürecini atlama ve daha fazla işleme gerektirmez bir seçenek sunar. `.align`Yalnızca kusursuz hizalanmış dosyalar için uzantı kullanmanızı öneririz.

Ayıklanan Tümcelerin sayısı aynı temel ada sahip iki dosya ile eşleşmezse özel çevirmen, dosyalardaki tümce Aligner 'i çalıştırmaya devam eder `.align` .

## <a name="i-tried-uploading-my-tmx-but-it-says-document-processing-failed"></a>TMX sayfamı karşıya yüklemeye çalıştım, ancak "belge işleme başarısız oldu" diyor.

TMX 'in konumundaki TMX 1.4 b belirtimine uyduğundan emin olun <https://www.gala-global.org/tmx-14b> .
