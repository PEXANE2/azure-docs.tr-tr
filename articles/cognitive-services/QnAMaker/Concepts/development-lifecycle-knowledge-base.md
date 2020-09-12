---
title: Bilgi Bankası 'nın yaşam döngüsü-Soru-Cevap Oluşturma
description: Soru-cevap oluşturma, bir model değişikliği, söylenişi örnekleri, yayımlama ve uç nokta sorgularından veri toplamayı en iyi şekilde öğrenir.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 7bbf729fa80e4b41a85b8dfd1080decea1bae108
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89612282"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Soru-Cevap Oluşturma Bilgi Bankası yaşam döngüsü
Soru-cevap oluşturma, bir model değişikliği, söylenişi örnekleri, yayımlama ve uç nokta sorgularından veri toplamayı en iyi şekilde öğrenir.

![Yazma döngüsü](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankasını oluşturma
Soru-Cevap Oluşturma Bilgi Bankası (KB) uç noktası, KB içeriğine göre bir Kullanıcı sorgusuna en iyi eşleşme yanıtı sağlar. Bilgi Bankası oluşturma, soruların, yanıtların ve ilişkili meta verilerin bir içerik deposunu ayarlamaya yönelik tek seferlik bir işlemdir. Aşağıdaki kaynaklar gibi önceden var olan içerik gezilirken bir KB oluşturulabilir:

- SSS sayfaları
- Ürün kılavuzları
- Q-A çiftleri

[Bilgi Bankası oluşturma](../quickstarts/create-publish-knowledge-base.md)hakkında bilgi edinin.

## <a name="testing-and-updating-the-knowledge-base"></a>Bilgi Bankası 'nı test etme ve güncelleştirme

Bilgi Bankası, içeriğe göre veya otomatik ayıklama yoluyla doldurulduktan sonra test etmeye hazırdır. Etkileşimli test, Soru-Cevap Oluşturma portalında, **Test** paneli aracılığıyla yapılabilir. Ortak kullanıcı sorguları girersiniz. Daha sonra, yanıtların doğru yanıt ve yeterli Güvenirlik puanı ile döndürüldüğünü doğrularsınız.


* **Düşük güvenirlik puanlarını onarmak için**: alternatif sorular ekleyin.
* **Sorgu yanlış bir şekilde [varsayılan yanıtı](../How-to/change-default-answer.md)döndürdüğünde**: doğru soruya yeni yanıtlar ekleyin.

Bu sıkı test-güncelleştirme döngüsü, sonuçlardan memnun olana kadar devam eder. [Bilgi Bankalarınızı test](../How-To/test-knowledge-base.md)etme hakkında bilgi edinin.

Büyük KBs için, [generateAnswer API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) `isTest` `test` yayımlanan bilgi tabanı yerine Bilgi Bankası yanıtını sorgulayan generateanswer API 'si ve Body özelliği ile otomatikleştirilmiş test kullanın.

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

Yayımlama eylemi nedeniyle, Bilgi Bankası 'nın test sürümünde yapılan diğer değişiklikler yayımlanmış sürümü etkilenmez. Yayınlanan sürüm, bir üretim uygulamasında canlı olabilir.

Bu bilgi tabanlarının her biri, test etmek için ayrı olarak hedeflenmelidir. API 'Leri kullanarak, Bilgi Bankası 'nın test sürümünü `isTest` generateAnswer çağrısında Body özelliği ile hedefleyebilirsiniz.

[Bilgi tabanınızı yayımlamayı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="monitor-usage"></a>Kullanımı izleme
Hizmetinizin sohbet günlüklerini günlüğe kaydedebilmek için, [soru-cevap oluşturma hizmetinizi](../How-To/set-up-qnamaker-service-azure.md)oluştururken Application Insights etkinleştirmeniz gerekir.

Hizmet kullanımınızın çeşitli analizlerini alabilirsiniz. Application Insights 'ı kullanarak [soru-cevap oluşturma hizmetiniz için analiz](../How-To/get-analytics-knowledge-base.md)alma hakkında daha fazla bilgi edinin.

Analizinizden öğrendiklerinize bağlı olarak, [bilgi tabanınızdan uygun güncelleştirmeleri](../How-To/edit-knowledge-base.md)yapın.

## <a name="version-control-for-data-in-your-knowledge-base"></a>Bilgi bankasındaki veriler için sürüm denetimi

Veriler için sürüm denetimi, Soru-Cevap Oluşturma portalındaki **Ayarlar** sayfasında içeri/dışarı aktarma özellikleri aracılığıyla sağlanır.

Bilgi bankasını, ya da biçiminde dışarı aktararak bir Bilgi Bankası yedekleyebilirsiniz `.tsv` `.xls` . Verildikten sonra, bu dosyayı normal kaynak denetimi denetiminizin bir parçası olarak dahil edin.

Belirli bir sürüme geri dönmelisiniz, bu dosyayı yerel sisteminizden içeri aktarmanız gerekir. Dışarı aktarılan bilgi tabanı yalnızca **Ayarlar** sayfasında içeri aktarma **aracılığıyla kullanılmalıdır.** Dosya veya URL belgesi veri kaynağı olarak kullanılamaz. Bu, bilgi bankasındaki soruların ve yanıtların içeri aktarılan dosyanın içeriğiyle yerini alır.

## <a name="test-and-production-knowledge-base"></a>Test ve üretim Bilgi Bankası
Bilgi Bankası, Soru-Cevap Oluşturma aracılığıyla oluşturulan, tutulan ve kullanılan soruların ve yanıt kümelerinin deposudur. Her Soru-Cevap Oluşturma kaynak birden fazla bilgi bankasını tutabilir.

Bilgi Bankası 'nda iki durum vardır: *Test* ve *yayımlandı*.

### <a name="test-knowledge-base"></a>Bilgi bankasını test etme

*Test bilgi tabanı* , şu anda düzenlenmiş ve kayıtlı olan sürümdür. Sınama sürümü doğruluk açısından ve yanıtların eksiksiz olması için test edilmiştir. Test Bilgi Bankası 'nda yapılan değişiklikler uygulamanızın son kullanıcısını veya sohbet bot 'ı etkilemez. Test bilgi tabanı, `test` http isteğinde olarak bilinir. `test`Bilgi, soru-cevap oluşturma portalı etkileşimli **Test** bölmesi ile kullanılabilir.

### <a name="production-knowledge-base"></a>Üretim Bilgi Bankası

*Yayımlanan bilgi tabanı* , sohbet bot veya uygulamanızda kullanılan sürümdür. Bilgi Bankası yayımlama, test sürümünün içeriğini yayımlanan sürümüne koyar. Yayımlanan bilgi tabanı, uygulamanın uç nokta aracılığıyla kullandığı sürümdür. İçeriğin doğru ve iyi test edilmiş olduğundan emin olun. Yayımlanan bilgi tabanı, `prod` http isteğinde olarak bilinir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Etkin öğrenme önerileri](./active-learning-suggestions.md)