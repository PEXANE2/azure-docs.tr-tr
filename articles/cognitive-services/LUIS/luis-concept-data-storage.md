---
title: Veri depolama-LUSıS
titleSuffix: Azure Cognitive Services
description: LUO, anahtar tarafından belirtilen bölgeye karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: e3028a9f046edb4013d8a2d6ec88d8c3cb2ca489
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79220022"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Language Understanding (LUSıS) bilişsel hizmetler 'de veri depolama ve kaldırma
LUO, anahtar tarafından belirtilen bölgeye karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar. Bu veriler 30 gün boyunca depolanır. 

## <a name="export-and-delete-app"></a>Uygulamayı dışarı ve silme
Kullanıcılar, uygulamayı [dışarı](luis-how-to-start-new-app.md#export-app) ve [silmeye](luis-how-to-start-new-app.md#delete-app) yönelik tam denetime sahiptir. 

## <a name="utterances"></a>Konuşmalar

Utterslar iki farklı yerde depolanabilir. 

* **Yazma işlemi**sırasında, yazmalar oluşturulur ve amaç içinde depolanır. Başarılı bir Lua uygulaması için amaçlarla ilgili bir amaç gereklidir. Uygulama yayımlandıktan ve uç noktada sorguları aldıktan sonra, uç nokta isteğinin QueryString `log=false`, uç nokta ile depolandığını belirler. Uç nokta depolanıyorsa, bu, **Gözden geçirme uç noktası utaları** bölümünde, portalın **Build** bölümünde bulunan etkin öğrenme utslerini bir parçası haline gelir. 
* **Uç nokta konutlerini gözden geçirdikten**ve bir amaca yönelik olarak bir e-posta eklediğinizde, söylenişi artık incelenmek üzere uç nokta çeşidinin bir parçası olarak depolanmaz. Uygulamanın hedefleri için eklenmiştir. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Bir amacınızdan örnek söyleyeni silme

Eðssıs eğitimi için kullanılan örnekleri silin [LUIS](luis-reference-regions.md). LUSıS uygulamanızın bir örneğini silerseniz, bu, LUSıS Web hizmetinden kaldırılır ve dışarı aktarma için kullanılamaz.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Etkin öğrenime göre gözden geçirdiklerinizi silme

Konuşma **[uç noktası sıralayıcısı sayfasında](luis-how-to-review-endpoint-utterances.md)**, luya 'nın önerdiği Kullanıcı arasları listesinden gelen noktaları silebilirsiniz. Bu listedeki söyleymeleri silmek, bunların önerilmesine izin vermez, ancak bunları günlüklerden silmez.

Etkin öğrenimi istemiyorsanız, [etkin öğrenmeyi devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning)bırakabilirsiniz. Etkin öğrenmeyi devre dışı bırakmak, günlüğü de devre dışı bırakır

### <a name="disable-logging-utterances"></a>Günlüğe kaydetme belgelerini devre dışı bırak
[Etkin öğrenmeyi devre dışı bırakmak](luis-how-to-review-endpoint-utterances.md#disable-active-learning) günlüğe kaydetmeyi devre dışı bırakır.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Hesap silme
Bir hesabı silerseniz, tüm uygulamalar, örnek ve günlükleriyle birlikte silinir. Veriler, hesap ve veriler kalıcı olarak silinmeden önce 60 gün boyunca tutulur.

Hesap silme, **Ayarlar** sayfasından kullanılabilir. **Ayarlar** sayfasına ulaşmak için sağ üst gezinti çubuğundan hesap adınızı seçin.

## <a name="data-inactivity-as-an-expired-subscription"></a>Süresi biten bir abonelik olarak verilerin etkin olmaması
Veri saklama ve silme amaçları doğrultusunda, etkin olmayan bir _Luo uygulaması Microsoft 'un_ kullanım açısından süresi geçen bir abonelik olarak kabul edilebilir. Son 90 gün için aşağıdaki ölçütleri karşılıyorsa bir uygulama etkin değil olarak kabul edilir: 

* Kendisine **hiçbir** çağrı yapılmadı.
* Değiştirilmedi.
* Kendisine atanmış geçerli bir anahtarı yok.
* Bir kullanıcının oturum açması gerekiyordu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulamayı dışarı ve silmeyi öğrenin](luis-how-to-start-new-app.md)
