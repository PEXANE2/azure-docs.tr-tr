---
title: Bilgi tabanının yaşam döngüsü - QnA Maker
description: QnA Maker, model değişiklikleri, sözcük örnekleri, yayımlama ve bitiş noktası sorgularından veri toplama yinelemeli bir döngüiçinde en iyi yitirmede öğrenir.
ms.topic: conceptual
ms.date: 02/27/2020
ms.openlocfilehash: 98fbd81baa717c981486f33cfb2b3a608cec27c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914961"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>QnA Maker'da bilgi tabanı yaşam döngüsü
QnA Maker, model değişiklikleri, sözcük örnekleri, yayımlama ve bitiş noktası sorgularından veri toplama yinelemeli bir döngüiçinde en iyi yitirmede öğrenir.

![Yazma döngüsü](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>QnA Maker bilgi tabanı oluşturma
QnA Maker bilgi tabanı (KB) bitiş noktası, KB'nin içeriğine dayalı bir kullanıcı sorgusuna en iyi eşleşme yanıtı sağlar. Bilgi tabanı oluşturmak, sorular, yanıtlar ve ilişkili meta verilerden oluşan bir içerik deposu oluşturmak için tek seferlik bir eylemdir. SSS sayfaları, ürün kılavuzları veya yapılandırılmış Q-A çiftleri gibi önceden varolan içeriği taranarak bir bilgi tabanı oluşturulabilir. Nasıl [bir bilgi tabanı oluşturabilirsiniz](../quickstarts/create-publish-knowledge-base.md)öğrenin.

## <a name="testing-and-updating-the-knowledge-base"></a>Bilgi tabanını test etme ve güncelleme

Bilgi bankası, editoryal olarak veya otomatik çıkarma yoluyla içerikle doldurulur sınandıktan sonra sınamaya hazırdır. İnteraktif test, ortak kullanıcı sorguları girerek ve yanıtların doğru yanıt ve yeterli güven puanıyla geri döndüğünü doğrulayarak **Test** paneli aracılığıyla QnA Maker portalında yapılabilir.

* **Düşük güven puanları düzeltmek için:** alternatif sorular ekleyin.
* **Bir sorgu [varsayılan yanıtı](../How-to/change-default-answer.md)yanlış döndürdüğünde:** doğru soruya yeni yanıtlar ekleyin.

Bu sıkı test güncelleştirme döngüsü, sonuçlardan memnun olana kadar devam ediyor. Bilgi tabanınızı nasıl [test edebilirsiniz](../How-To/test-knowledge-base.md)öğrenin.

Büyük KB'ler için, yayınlanan bilgi tabanı yerine `isTest` bilgi tabanını `test` sorgulayan [generateAnswer API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) ve gövde özelliği yle otomatik test kullanın.

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama
Bilgi tabanını test etmeyi bitirdikten sonra yayımlayabilirsiniz. Yayımlama, test edilmiş bilgi tabanının en son **sürümünü, yayımlanmış** bilgi tabanını temsil eden özel bir Azure Bilişsel Arama dizinine iter. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.

Bu şekilde, bilgi tabanının test sürümünde yapılan değişiklikler, bir üretim uygulamasında canlı olabilecek yayımlanmış sürümü etkilemez.

Bu bilgi üslerinin her biri ayrı ayrı test etmek için hedeflenebilir. API'leri kullanarak, generateAnswer aramasında gövde `isTest` özelliğiyle bilgi tabanının test sürümünü hedefleyebilirsiniz.

Bilgi tabanınızı nasıl [yayınlayacağınızı](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)öğrenin.

## <a name="monitor-usage"></a>Kullanımı izleme
Hizmetinizin sohbet günlüklerini günlüğe kaydedebilmek için [QnA Maker hizmetinizi oluştururken](../How-To/set-up-qnamaker-service-azure.md)Uygulama Öngörüleri'ni etkinleştirmeniz gerekir.

Hizmet kullanımınızın çeşitli analizlerini alabilirsiniz. [QnA Maker hizmetiniz için analitik](../How-To/get-analytics-knowledge-base.md)almak için uygulama öngörülerini nasıl kullanacağınız hakkında daha fazla bilgi edinin.

Analizlerinizden öğrendiklerinize bağlı olarak, [bilgi tabanınızda](../How-To/edit-knowledge-base.md)uygun güncellemeler yapın.

## <a name="version-control-for-data-in-your-knowledge-base"></a>Bilgi tabanınızdaki veriler için sürüm denetimi

Veriler için sürüm denetimi, QnA Maker portalındaki **Ayarlar** sayfasındaki alma/dışa aktarma özellikleri aracılığıyla sağlanır.

Bilgi tabanını dışa aktararak bilgi tabanını `.tsv` ya `.xls` da biçimde yedekleyebilirsiniz. Dışa aktarıladıktan sonra, bu dosyayı normal kaynak denetimi denetiminizin bir parçası olarak ekleyin.

Belirli bir sürüme geri dönmeniz gerektiğinde, bu dosyayı yerel sisteminizden almanız gerekir. Dışa aktarılan bir bilgi **tabanı** yalnızca **Ayarlar** sayfasından içe aktarma yoluyla kullanılmalıdır. Dosya veya URL belge veri kaynağı olarak kullanılamaz. Bu, bilgi tabanında bulunan sorular ve yanıtların yerini alınan dosyanın içeriğiyle değiştirir.

## <a name="test-and-production-knowledge-base"></a>Test ve üretim bilgi tabanı
Bilgi tabanı, QnA Maker aracılığıyla oluşturulan, tutulan ve kullanılan soru ve yanıt kümelerinin deposudur. Her QnA Maker kaynağı birden çok bilgi tabanı tutabilir.

Bir bilgi tabanının iki noktası vardır: *test* ve *yayınlanmıştır.*

### <a name="test-knowledge-base"></a>Test bilgi tabanı

*Test bilgi tabanı,* yanıtların doğruluğu ve eksiksizliği için şu anda düzenlenen, kaydedilmiş ve sınanan sürümdür. Test bilgi tabanında yapılan değişiklikler uygulamanızın veya sohbet botunuzun son kullanıcısını etkilemez. Test bilgi tabanı HTTP `test` isteği olarak bilinir. Bilgi `test` QnA Maker portal interaktif **Test** bölmesi ile kullanılabilir.

### <a name="production-knowledge-base"></a>Üretim bilgi tabanı

*Yayınlanan bilgi bankası,* sohbet botunuzda veya uygulamanızda kullanılan sürümdür. Bir bilgi tabanı yayımlama eylemi, test bilgi tabanının içeriğini bilgi tabanının yayınlanmış sürümüne koyar. Yayınlanan bilgi tabanı, uygulamanın bitiş noktası boyunca kullandığı sürüm olduğundan, içeriğin doğru ve iyi sınandığından emin olun. Yayınlanan bilgi tabanı HTTP `prod` isteği olarak bilinir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Aktif öğrenme önerileri](./active-learning-suggestions.md)