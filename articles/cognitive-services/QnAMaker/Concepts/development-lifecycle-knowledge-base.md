---
title: Bilgi Bankası 'nın yaşam döngüsü-Soru-Cevap Oluşturma
description: Soru-cevap oluşturma, bir model değişikliği, söylenişi örnekleri, yayımlama ve uç nokta sorgularından veri toplamayı en iyi şekilde öğrenir.
ms.topic: conceptual
ms.date: 02/27/2020
ms.openlocfilehash: 98fbd81baa717c981486f33cfb2b3a608cec27c7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77914961"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Soru-Cevap Oluşturma Bilgi Bankası yaşam döngüsü
Soru-cevap oluşturma, bir model değişikliği, söylenişi örnekleri, yayımlama ve uç nokta sorgularından veri toplamayı en iyi şekilde öğrenir.

![Yazma döngüsü](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi tabanı oluşturma
Soru-Cevap Oluşturma Bilgi Bankası (KB) uç noktası, KB içeriğine göre bir Kullanıcı sorgusuna en iyi eşleşme yanıtı sağlar. Bilgi Bankası oluşturma, soruların, yanıtların ve ilişkili meta verilerin bir içerik deposunu ayarlamaya yönelik tek seferlik bir işlemdir. SSS sayfaları, ürün el kitapları veya yapılandırılmış Q-A çiftleri gibi önceden var olan içerikler gezilirken bir Bilgi Bankası oluşturulabilir. [Bilgi Bankası oluşturma](../quickstarts/create-publish-knowledge-base.md)hakkında bilgi edinin.

## <a name="testing-and-updating-the-knowledge-base"></a>Bilgi Bankası 'nı test etme ve güncelleştirme

Bilgi Bankası, içeriğe göre veya otomatik ayıklama yoluyla doldurulduktan sonra test etmeye hazırdır. Etkileşimli test, **Test** paneli aracılığıyla soru-cevap oluşturma portalında, ortak kullanıcı sorguları girerek ve yanıtların doğru yanıt ve yeterli Güvenirlik puanı ile döndürülen yanıtları doğrulayarak yapılabilir.

* **Düşük güvenirlik puanlarını onarmak için**: alternatif sorular ekleyin.
* **Sorgu yanlış bir şekilde [varsayılan yanıtı](../How-to/change-default-answer.md)döndürdüğünde**: doğru soruya yeni yanıtlar ekleyin.

Bu sıkı test-güncelleştirme döngüsü, sonuçlardan memnun olana kadar devam eder. [Bilgi Bankalarınızı test](../How-To/test-knowledge-base.md)etme hakkında bilgi edinin.

Büyük KBs için, yayımlanan bilgi tabanı yerine `test` Bilgi Bankası [yanıtını sorgulayan generateanswer API 'si](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) ve `isTest` Body özelliği ile otomatikleştirilmiş test kullanın.

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama
Bilgi bankasını test etmeyi tamamladıktan sonra yayımlayabilirsiniz. Yayımlama, sınanan Bilgi Bankası 'nın en son sürümünü **yayınlanan** Bilgi Bankası 'nı temsil eden adanmış bir Azure bilişsel arama dizinine gönderir. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.

Bu şekilde, Bilgi Bankası 'nın test sürümünde yapılan tüm değişiklikler, bir üretim uygulamasında canlı olabilecek yayımlanmış sürümü etkilemez.

Bu bilgi tabanlarının her biri, test etmek için ayrı olarak hedeflenmelidir. API 'Leri kullanarak, Bilgi Bankası 'nın test sürümünü generateAnswer çağrısında Body özelliği ile `isTest` hedefleyebilirsiniz.

[Bilgi tabanınızı yayımlamayı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="monitor-usage"></a>Kullanımı izleme
Hizmetinizin sohbet günlüklerini günlüğe kaydedebilmek için, [soru-cevap oluşturma hizmetinizi](../How-To/set-up-qnamaker-service-azure.md)oluştururken Application Insights etkinleştirmeniz gerekir.

Hizmet kullanımınızın çeşitli analizlerini alabilirsiniz. Application Insights 'ı kullanarak [soru-cevap oluşturma hizmetiniz için analiz](../How-To/get-analytics-knowledge-base.md)alma hakkında daha fazla bilgi edinin.

Analizinizden öğrendiklerinize bağlı olarak, [bilgi tabanınızdan uygun güncelleştirmeleri](../How-To/edit-knowledge-base.md)yapın.

## <a name="version-control-for-data-in-your-knowledge-base"></a>Bilgi bankasındaki veriler için sürüm denetimi

Veriler için sürüm denetimi, Soru-Cevap Oluşturma portalındaki **Ayarlar** sayfasında içeri/dışarı aktarma özellikleri aracılığıyla sağlanır.

Bilgi bankasını, ya da `.tsv` `.xls` biçiminde dışarı aktararak bir Bilgi Bankası yedekleyebilirsiniz. Verildikten sonra, bu dosyayı normal kaynak denetimi denetiminizin bir parçası olarak dahil edin.

Belirli bir sürüme geri dönmelisiniz, bu dosyayı yerel sisteminizden içeri aktarmanız gerekir. Dışarı aktarılan bilgi tabanı yalnızca **Ayarlar** sayfasında içeri aktarma **aracılığıyla kullanılmalıdır.** Dosya veya URL belgesi veri kaynağı olarak kullanılamaz. Bu, bilgi bankasındaki soruların ve yanıtların içeri aktarılan dosyanın içeriğiyle yerini alır.

## <a name="test-and-production-knowledge-base"></a>Test ve üretim Bilgi Bankası
Bilgi Bankası, Soru-Cevap Oluşturma aracılığıyla oluşturulan, tutulan ve kullanılan soruların ve yanıt kümelerinin deposudur. Her Soru-Cevap Oluşturma kaynak birden fazla bilgi bankasını tutabilir.

Bilgi Bankası 'nda iki durum vardır: *Test* ve *yayımlandı*.

### <a name="test-knowledge-base"></a>Test bilgi tabanı

*Test bilgi tabanı* , şu anda düzenlenmiş, kaydedilmiş ve yanıtların doğruluğu ve tamamlanmamaları için test edilmiş sürümüdür. Test Bilgi Bankası 'nda yapılan değişiklikler uygulamanızın son kullanıcısını veya sohbet bot 'ı etkilemez. Test bilgi tabanı, HTTP isteğinde olarak `test` bilinir. Bilgi `test` , soru-cevap oluşturma portalı etkileşimli **Test** bölmesi ile kullanılabilir.

### <a name="production-knowledge-base"></a>Üretim Bilgi Bankası

*Yayımlanan bilgi tabanı* , sohbet bot veya uygulamanızda kullanılan sürümdür. Bilgi Bankası yayımlama eylemi, bilgi bankasındaki yayımlanmış sürüme test bilgi tabanı içeriğini koyar. Yayımlanan bilgi tabanı uygulamanın uç nokta aracılığıyla kullandığı sürüm olduğundan, içeriğin doğru ve iyi test edildiğinden emin olun. Yayımlanan bilgi tabanı, HTTP isteğinde olarak `prod` bilinir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Etkin öğrenme önerileri](./active-learning-suggestions.md)