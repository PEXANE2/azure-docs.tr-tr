---
title: Veri depolama - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS, şifrelenmiş verileri anahtartarafından belirtilen bölgeye karşılık gelen bir Azure veri deposunda depolar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220022"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Dil Anlama (LUIS) Bilişsel Hizmetlerveri depolama ve kaldırma
LUIS, şifrelenmiş verileri anahtartarafından belirtilen bölgeye karşılık gelen bir Azure veri deposunda depolar. Bu veriler 30 gün boyunca saklanır. 

## <a name="export-and-delete-app"></a>Uygulamayı dışa aktarma ve silme
Kullanıcılar, uygulamayı [dışa aktarma](luis-how-to-start-new-app.md#export-app) ve [silme](luis-how-to-start-new-app.md#delete-app) konusunda tam kontrole sahiptir. 

## <a name="utterances"></a>Konuşmalar

Söyleyişler iki farklı yerde saklanabilir. 

* **Yazma işlemi sırasında,** söyleyerek oluşturulur ve Niyet saklanır. Başarılı bir LUIS uygulaması için niyetler içinde söylenmeler gereklidir. Uygulama yayımlandıktan ve bitiş noktasında sorgular aldıktan sonra, bitiş noktası `log=false`isteğinin sorgu dizesi, bitiş noktası sözcüğün depolanıp depolanmayaçıklanmayonu belirler. Bitiş noktası depolanırsa, portalın **Yapı** bölümünde, Gözden Geçir uç nokta lı söyleyişler bölümünde bulunan etkin öğrenme **sözcüğünün** bir parçası olur. 
* Uç **nokta söyleyişlerini gözden geçirdiğinizde**ve bir amaca bir söz eklediğinizde, söyleyiş artık gözden geçirilecek uç nokta sözcüklerinin bir parçası olarak depolanır. Uygulamanın amaçlarına eklenir. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Örnek açıklamaları bir amaçla silme

[LUIS](luis-reference-regions.md)eğitimi için kullanılan örnek açıklamaları silin. LUIS uygulamanızdan bir örnek açıklama silerseniz, bu açıklama LUIS web hizmetinden kaldırılır ve dışa aktarılmaz.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>İncelemedeki açıklamaları etkin öğrenmeden silme

Luis'in **[Gözden Geçir uç nokta açıklamaları sayfasında](luis-how-to-review-endpoint-utterances.md)** önerdiği kullanıcı söyleyişleri listesinden çıkan ları silebilirsiniz. Bu listeden gelen açıklamalarıssilinmesi, bu açıklamaların önerilmesini engeller, ancak günlüklerden silmez.

Eğer aktif öğrenme söyleyişlerini istemiyorsanız, [aktif öğrenmeyi devre dışı kullanabilirsiniz.](luis-how-to-review-endpoint-utterances.md#disable-active-learning) Etkin öğrenmeyi devre dışı bırakmak da günlük kaydetmeyi devre dışı kılabilir.

### <a name="disable-logging-utterances"></a>Günlük söyleyişlerini devre dışı
[Etkin öğrenmeyi devre dışı bırakmak](luis-how-to-review-endpoint-utterances.md#disable-active-learning) günlük leilgililiği devre dışı bırakmaktır.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Hesap silme
Bir hesabı silerseniz, tüm uygulamalar, örnek açıklamaları ve günlükleriyle birlikte silinir. Veriler, hesap ve veriler kalıcı olarak silinmeden önce 60 gün boyunca saklanır.

Hesap silme, **Ayarlar** sayfasından kullanılabilir. **Ayarlar** sayfasına ulaşmak için sağ üst gezinti çubuğunda hesap adınızı seçin.

## <a name="data-inactivity-as-an-expired-subscription"></a>Süresi dolmuş bir abonelik olarak veri etkinliği
Veri saklama ve silme amacıyla, etkin olmayan bir LUIS uygulaması _Microsoft'un takdirine bağlı_ olarak süresi dolmuş bir abonelik olarak kabul edilebilir. Bir uygulama, son 90 gün için aşağıdaki ölçütleri karşılıyorsa etkin değil sayılır: 

* **Hiç** arama yapılmadı.
* Değiştirilmedi.
* Atanmış geçerli bir anahtarı yok.
* Kullanıcı oturum açmadı.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama nın dışa aktarılmasına ve silmesi hakkında bilgi edinin](luis-how-to-start-new-app.md)
