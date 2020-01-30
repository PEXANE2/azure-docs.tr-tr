---
title: Bilgi Bankası - soru-cevap Oluşturucu yaşam döngüsü
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b1978e45a7554358ddd948879143411f89e4c1b2
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843414"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Soru-cevap Oluşturucu, Bilgi Bankası yaşam döngüsü
Soru-cevap Oluşturucu, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir.

![Yazma döngüsü](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası oluşturma
Soru-cevap Oluşturucu Bilgi Bankası (KB) uç noktası, bir kullanıcı sorgu KB içeriğine göre en iyi eşleşme Yanıtla sağlar. Bilgi Bankası oluşturma, bir içerik deposu soru, yanıt ve ilişkili meta verileri ayarlama için tek seferlik bir işlemdir. Bilgi Bankası, SSS sayfaları, ürün kılavuzlarını veya yapılandırılmış Q A çiftleri gibi önceden varolan içerikte tarafından oluşturulabilir. Bilgi edinmek için nasıl [Bilgi Bankası oluşturma](../quickstarts/create-publish-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Test ve Bilgi Bankası güncelleştirme

Bilgi Bankası bilgi bankanızı düzenleyerek veya otomatik ayıklama aracılığıyla içerikle doldurulur sonra test etmek için hazırdır. Etkileşimli test, **Test** paneli aracılığıyla soru-cevap oluşturma portalında, ortak kullanıcı sorguları girerek ve yanıtların doğru yanıt ve yeterli Güvenirlik puanı ile döndürülen yanıtları doğrulayarak yapılabilir.

* **Düşük güvenirlik puanlarını onarmak için**: alternatif sorular ekleyin.
* **Sorgu yanlış bir şekilde [varsayılan yanıtı](../How-to/change-default-answer.md)döndürdüğünde**: doğru soruya yeni yanıtlar ekleyin.

Sonuçlardan memnun olana kadar test güncelleştirme sıkı Bu döngü devam eder. Bilgi edinmek için nasıl [bilgi bankanızı test](../How-To/test-knowledge-base.md).

Büyük KBs için, [Generateanswer API 'si](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) ile otomatik test ve yayımlanan bilgi tabanı yerine `test` Bilgi Bankası ' nı sorgulayan `isTest` Body özelliğini kullanın.

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama
İşiniz bittiğinde Bilgi Bankası sınanması, bunu yayımlayabilirsiniz. Yayımlama, sınanan Bilgi Bankası 'nın en son sürümünü **yayınlanan** Bilgi Bankası 'nı temsil eden adanmış bir Azure bilişsel arama dizinine gönderir. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.

Bu şekilde, bir üretim uygulamasında Canlı olabilecek yayımlanmış sürümüne Bilgi Bankası test sürümüne yapılan her değişikliği etkilemez.

Bu bilgi bankalarından herbiri ayrı olarak test etmek için hedeflenebilir. API 'Leri kullanarak, Bilgi Bankası 'nın test sürümünü generateAnswer çağrısında `isTest` Body özelliği ile hedefleyebilirsiniz.

Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="monitor-usage"></a>Kullanımı izleme
Sohbet günlükleri hizmetinizin oturum açabilmesi için Application Insights'ı etkinleştirmek ihtiyacınız zaman, [soru-cevap Oluşturucu hizmetinizi oluşturun](../How-To/set-up-qnamaker-service-azure.md).

Hizmet kullanımınızın çeşitli analiz elde edebilirsiniz. Almak için application ınsights'ı kullanma hakkında daha fazla bilgi edinin [soru-cevap Oluşturucu hizmetiniz için analytics](../How-To/get-analytics-knowledge-base.md).

Bağlı olarak, analytics'ten öğrenin, uygun hale [bilgi bankanızı güncelleştirmeleri](../How-To/edit-knowledge-base.md).

## <a name="version-control-for-data-in-your-knowledge-base"></a>Bilgi bankasındaki veriler için sürüm denetimi

Veriler için sürüm denetimi, Soru-Cevap Oluşturma portalındaki **Ayarlar** sayfasında içeri/dışarı aktarma özellikleri aracılığıyla sağlanır.

Bilgi bankasını `.tsv` ya da `.xls` biçiminde dışarı aktararak bir Bilgi Bankası yedekleyebilirsiniz. Verildikten sonra, bu dosyayı normal kaynak denetimi denetiminizin bir parçası olarak dahil edin.

Belirli bir sürüme geri dönmelisiniz, bu dosyayı yerel sisteminizden içeri aktarmanız gerekir. Dışarı aktarılan bilgi tabanı yalnızca **Ayarlar** sayfasında içeri aktarma **aracılığıyla kullanılmalıdır.** Dosya veya URL belgesi veri kaynağı olarak kullanılamaz. Bu, bilgi bankasındaki soruların ve yanıtların içeri aktarılan dosyanın içeriğiyle yerini alır.

## <a name="test-and-production-knowledge-base"></a>Test ve üretim Bilgi Bankası
Bilgi Bankası, Soru-Cevap Oluşturma aracılığıyla oluşturulan, tutulan ve kullanılan soruların ve yanıt kümelerinin deposudur. Her Soru-Cevap Oluşturma kaynak birden fazla bilgi bankasını tutabilir.

Bilgi Bankası 'nda iki durum vardır: *Test* ve *yayımlandı*.

### <a name="test-knowledge-base"></a>Test bilgi tabanı

*Test bilgi tabanı* , şu anda düzenlenmiş, kaydedilmiş ve yanıtların doğruluğu ve tamamlanmamaları için test edilmiş sürümüdür. Test Bilgi Bankası 'nda yapılan değişiklikler uygulamanızın son kullanıcısını veya sohbet bot 'ı etkilemez. Test bilgi tabanı, HTTP isteğinde `test` olarak bilinir. `test` bilgi Soru-Cevap Oluşturma portalı etkileşimli **Test** bölmesi ile kullanılabilir.

### <a name="production-knowledge-base"></a>Üretim Bilgi Bankası

*Yayımlanan bilgi tabanı* , sohbet bot veya uygulamanızda kullanılan sürümdür. Bilgi Bankası yayımlama eylemi, bilgi bankasındaki yayımlanmış sürüme test bilgi tabanı içeriğini koyar. Yayımlanan bilgi tabanı uygulamanın uç nokta aracılığıyla kullandığı sürüm olduğundan, içeriğin doğru ve iyi test edildiğinden emin olun. Yayımlanan Bilgi Bankası, HTTP isteğinde `prod` olarak bilinir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenilirlik puanı](./confidence-score.md)