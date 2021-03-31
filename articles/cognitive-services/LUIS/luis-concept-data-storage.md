---
title: Veri depolama-LUSıS
titleSuffix: Azure Cognitive Services
description: LUO, anahtar tarafından belirtilen bölgeye karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/07/2020
ms.openlocfilehash: a58bcff4e39c4a4a907cd8567b47b074ff299bd5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "97008461"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Language Understanding (LUSıS) bilişsel hizmetler 'de veri depolama ve kaldırma

LUO, anahtar tarafından belirtilen [bölgeye](luis-reference-regions.md) karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar. 

* Model, amaçlar ve söyleyme gibi modeli eğitmek için kullanılan veriler, uygulamanın kullanım ömrü için LUO 'ya kaydedilir. Bir sahip veya katkıda bulunan uygulamayı silerse, bu veriler onunla silinir. Uygulama 90 gün içinde kullanılmıyorsa, silinir. 

* Uygulama yazarları, yayımlanan bir uygulamaya gönderilen söyleyler üzerinde [günlüğe kaydetmeyi etkinleştirmeyi](luis-how-to-review-endpoint-utterances.md#log-user-queries-to-enable-active-learning) seçebilir. Etkinleştirilirse, bu işlem 30 gün boyunca kaydedilir ve uygulama yazarı tarafından görüntülenebilir. Uygulama yayımlandığında günlüğe kaydetme etkinleştirilmemişse, bu veriler depolanmaz.

## <a name="export-and-delete-app"></a>Uygulamayı dışarı ve silme
Kullanıcılar, uygulamayı [dışarı](luis-how-to-start-new-app.md#export-app) ve [silmeye](luis-how-to-start-new-app.md#delete-app) yönelik tam denetime sahiptir. 

## <a name="utterances"></a>İfadeler

Utterslar iki farklı yerde depolanabilir. 

* **Yazma işlemi** sırasında, yazmalar oluşturulur ve amaç içinde depolanır. Başarılı bir Lua uygulaması için amaçlarla ilgili bir amaç gereklidir. Uygulama yayımlandıktan ve uç noktada sorguları aldıktan sonra, uç nokta isteğinin QueryString, `log=false` uç nokta ile depolandığını belirler. Uç nokta depolanıyorsa, bu, **Gözden geçirme uç noktası utaları** bölümünde, portalın **Build** bölümünde bulunan etkin öğrenme utslerini bir parçası haline gelir. 
* **Uç nokta konutlerini gözden geçirdikten** ve bir amaca yönelik olarak bir e-posta eklediğinizde, söylenişi artık incelenmek üzere uç nokta çeşidinin bir parçası olarak depolanmaz. Uygulamanın hedefleri için eklenmiştir. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Bir amacınızdan örnek söyleyeni silme

Eðssıs eğitimi için kullanılan örnekleri silin [](luis-reference-regions.md). LUSıS uygulamanızın bir örneğini silerseniz, bu, LUSıS Web hizmetinden kaldırılır ve dışarı aktarma için kullanılamaz.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Etkin öğrenime göre gözden geçirdiklerinizi silme

Konuşma **[uç noktası sıralayıcısı sayfasında](luis-how-to-review-endpoint-utterances.md)**, luya 'nın önerdiği Kullanıcı arasları listesinden gelen noktaları silebilirsiniz. Bu listedeki söyleymeleri silmek, bunların önerilmesine izin vermez, ancak bunları günlüklerden silmez.

Etkin öğrenimi istemiyorsanız, [etkin öğrenmeyi devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning)bırakabilirsiniz. Etkin öğrenmeyi devre dışı bırakmak, günlüğü de devre dışı bırakır

### <a name="disable-logging-utterances"></a>Günlüğe kaydetme belgelerini devre dışı bırak
[Etkin öğrenmeyi devre dışı bırakmak](luis-how-to-review-endpoint-utterances.md#disable-active-learning) günlüğe kaydetmeyi devre dışı bırakır.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Hesap silme
Geçirdiyseniz, hesabınızı silebilirsiniz ve tüm uygulamalarınız, örnek deneyleriyle ve günlükleriyle birlikte silinecektir. Veriler, hesap ve veriler kalıcı olarak silinmeden önce 90 gün boyunca tutulur.

Hesap silme, **Ayarlar** sayfasından kullanılabilir. **Ayarlar** sayfasına ulaşmak için sağ üst gezinti çubuğundan hesap adınızı seçin.

## <a name="delete-an-authoring-resource"></a>Bir yazma kaynağını silme
[Bir yazma kaynağına geçiş](./luis-migration-authoring.md)yaptıysanız, kaynağın kendisini Azure Portal silmek, bu kaynakla ilişkili tüm uygulamalarınızı, örnek dıklarıyla ve günlükleriyle birlikte silecek. Veriler kalıcı olarak silinmeden önce 90 gün boyunca tutulur.    

Kaynağınızı silmek için [Azure Portal](https://ms.portal.azure.com/#home) gıdın ve lusıs Authoring Resource ' ı seçin. **Genel bakış** sekmesine gidin ve sayfanın üst kısmındaki **Sil** düğmesine tıklayın. Ardından kaynağınızın silindiğini doğrulayın. 

## <a name="data-inactivity-as-an-expired-subscription"></a>Süresi biten bir abonelik olarak verilerin etkin olmaması
Veri saklama ve silme amaçları doğrultusunda, etkin olmayan bir _Luo uygulaması Microsoft 'un_ kullanım açısından süresi geçen bir abonelik olarak kabul edilebilir. Son 90 gün için aşağıdaki ölçütleri karşılıyorsa bir uygulama etkin değil olarak kabul edilir: 

* Kendisine **hiçbir** çağrı yapılmadı.
* Değiştirilmedi.
* Kendisine atanmış geçerli bir anahtarı yok.
* Bir kullanıcının oturum açması gerekiyordu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulamayı dışarı ve silmeyi öğrenin](luis-how-to-start-new-app.md)