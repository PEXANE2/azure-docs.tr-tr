---
title: 'Quickstart: LUIS portalında yeni bir uygulama oluşturun'
description: Bu hızlı başlangıçta, bir uygulamanın temel bölümlerini, niyetlerini ve varlıklarını ve luis portalında örnek bir sözle test emebilirsiniz.
ms.topic: quickstart
ms.date: 03/24/2020
ms.openlocfilehash: f0c8f0c77f832e049dfc494f82e90edb61a8cb2a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80244623"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Quickstart: LUIS portalında yeni bir uygulama oluşturun

Bu hızlı başlangıçta, LUIS portalında yeni bir uygulama oluşturursunuz. İlk olarak, bir uygulamanın temel bölümlerini, **niyetlerini**ve **varlıkları**oluşturun. Ardından, öngörülen amacı elde etmek için etkileşimli test panelinde örnek bir kullanıcı sözcük sağlayarak uygulamayı test edin.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-an-app"></a>Uygulama oluşturma

1. Bağlam araç çubuğundan **konuşma için + Yeni uygulamayı** seçin, ardından konuşma için Yeni **uygulamayı**seçin.

    > [!div class="mx-imgBorder"]
    > [![LUIS portalında yeni uygulama oluşturma](./media/create-app-in-portal.png)](./media/create-app-in-portal.png#lightbox)

1. Açılan pencerede, uygulamayı aşağıdaki ayarlarla yapılandırın ve **ardından Bitti'yi**seçin.

   |Ayar adı| Değer | Amaç|
   |--|--|--|
   |Adı|`myEnglishApp`|Benzersiz LUIS uygulama adı<br>gerekli|
   |Kültür|**Türkçe**|Kullanıcılardan gelen söz dili, **en-us**<br>gerekli|
   |Açıklama (İsteğe Bağlı)|`App made with LUIS Portal`|Uygulamanın açıklaması<br>isteğe bağlı|
   |Tahmin kaynağı (İsteğe bağlı) |-  |Seçmeyin. LUIS, yazma için ücretsiz olarak kullanabileceğiniz bir Başlangıç tuşu ve 1.000 tahmin bitiş noktası isteği verir. |

   ![Yeni uygulama ayarlarını girin](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Amaç oluşturma

LUIS uygulaması oluşturulduktan sonra, niyet oluşturmanız gerekir. Niyetler, kullanıcılardan gelen metni kategorilere ayırmanın bir yoludur. Örneğin, bir insan kaynakları uygulamasının iki işlevi olabilir. İnsanlara yardım etmek için:

 1. İş bulma ve uygulama
 1. İşlere başvuracak formları bulma

Uygulamanın iki farklı _niyeti_ aşağıdaki niyetlerle uyumlu:

|Amaç|Kullanıcıdan örnek metin<br>bir _söyleyiş_ olarak bilinen|
|--|--|
|İş Başvurusu|`I want to apply for the new software engineering position in Cairo.`|
|BulForm|`Where is the job transfer form hrf-123456?`|

Niyet oluşturmak için aşağıdaki adımları tamamlayın:

1. Uygulama oluşturulduktan sonra **Yapı** bölümünün **Niyetler** sayfasındasınız. **Oluştur'u**seçin.

   [![Yeni bir amaç oluşturmak için Oluştur'u seçin](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Niyet adını `FindForm`girin ve ardından **Bitti'yi**seçin.

## <a name="add-an-example-utterance"></a>Örnek bir söz ekle

Niyetler oluşturduktan sonra örnek söz eklersiniz. Örnek deyişle, bir kullanıcının sohbet botuna veya başka bir istemci uygulamasına girdiği metindir. Kullanıcımetninin amacını LUIS niyetiyle eşlerler.

Bu örnek uygulamanın `FindForm` amacı için, örnek söyleyişform numarasını içerecektir. İstemci uygulamasının kullanıcının isteğini yerine getirmek için form numarasına ihtiyacı vardır, bu nedenle kullanıcının isteğine dahil etmek önemlidir.

> [!div class="mx-imgBorder"]
> [![FindForm amacı için örnek söyleyinmeleri girin](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

Aşağıdaki 15 örnek söyleyiyi niyete `FindForm` ekleyin.

|#|Örnek konuşmalar|
|--|--|
|1|hrf-123456 arıyorum|
|2|İnsan kaynakları hrf-234591 formu nerede?|
|3|hrf-345623, nerede|
|4|Bana hrf-345794 göndermek mümkün mü|
|5|Ben hrf-234695 bir iç iş için başvuruda ihtiyacım var mı?|
|6|Menajerim in hrf-234091 ile iş başvurusunda olduğumu bilmesi gerekiyor mu?|
|7|Hrf-234918'i nereye gönderebilirim? Alınan bir e-posta yanıtı alabilir miyim?|
|8|hrf-234555|
|9|HRF-234987 ne zaman güncellendi?|
|10|Mühendislik pozisyonlarına başvurmak için hrf-876345 formunu kullanabilir miyim?|
|11|Hrf-765234 yeni bir sürümü benim açık req için gönderildi mi?|
|12|Hrf-234234'ü uluslararası işlerde kullanıyor muyum?|
|13|hrf-234598 yazım hatası|
|14|hrf-234567 yeni gereksinimler için düzenlenecek|
|15|hrf-123456, hrf-123123, hrf-234567|

Tasarım olarak, bu örnek ifadeler aşağıdaki şekillerde değişir:

* söyleyiş uzunluğu
* noktalama işareti
* kelime seçimi
* verb tense (is, was, will be)
* sözcük sırası



## <a name="create-a-regular-expression-entity"></a>Düzenli bir ifade varlığı oluşturma

Çalışma zamanı tahmin yanıtında form numarasını döndürmek için formun varlık olarak işaretlemesi gerekir. Form numarası metni yüksek yapılandırılmış olduğundan, normal bir ifade varlığı kullanarak işaretleyebilirsiniz. Varlığı aşağıdaki adımlarla oluşturun:

1. Soldaki menüden **Varlıklar'ı** seçin.

1. **Varlıklar** sayfasında **Oluştur'u** seçin.

1. Adını `Human Resources Form Number`girin, **Regex** varlık türünü seçin ve **sonra İleri'yi**seçin.

   ![Düzenli ifade varlığı oluşturma](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. Normal ifade **(RegEx**) `hrf-[0-9]{6}`ifadesini girin, . Bu giriş, gerçek karakterlerle `hrf-`eşleşir ve tam olarak 6 basamaklı sayılara izin verir, sonra **Oluştur'u**seçin.

   ![Varlık için normal ifade girin](./media/get-started-portal-build-app/create-regular-expression-entity-with-expression.png)


## <a name="add-example-utterances-to-the-none-intent"></a>Yok amacına örnek söz ekleme

**None** niyeti geri dönüş niyetidir ve boş bırakılmamalıdır. Bu amaç, uygulamanın diğer amaçları için eklediğiniz her 10 örnek söz için bir söz içermelidir.

**Yok** niyetinin örnek belaları istemci uygulama etki alanıdışında olmalıdır.

1. Sol menüden **Niyetler'i** seçin ve ardından niyet listesinden **Yok'u** seçin.

1. Niyete aşağıdaki örnek ifadeler ekleyin:

   |Hiçbir niyet örnek söyleyinmeler|
   |--|
   |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
   |Order a pizza for me (Bana bir pizza söyle)|
   |Penguins in the ocean (Okyanustaki penguenler)|

   Bu uygulama için, bu örnek söyleyinmeler etki alanının dışındadır. Etki alanınız hayvanlar, gıda veya okyanus içeriyorsa, O zaman **Yok** niyeti için farklı örnek söyleyişler kullanmalısınız.

## <a name="train-the-app"></a>Uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Örnek ifadelerde normal ifade varlığına bakın

1. Sol menüden **Niyetler'i** seçerek **Varlığın FindForm** amacında bulunduğunu doğrulayın. Ardından **FindForm** amacını seçin.

   Varlık, örnek sözlerle göründüğü yerde işaretlenir. Varlık adı yerine özgün metni görmek istiyorsanız, araçlar çubuğundan **Varlıklar Görünümü'ni** geçişten geçiş edin.

   > [!div class="mx-imgBorder"]
   > [![Varlıklarla işaretlenmiş tüm örnek söyleyinmeler](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Etkileşimli test bölmesi ile yeni uygulamanızı test edin

Varlığın uygulamanın henüz görmediği yeni söyleyişlerden çıkarını doğrulamak için LUIS portalındaki etkileşimli **Test** bölmesini kullanın.

1. Sağ üst menüden **Test'i** seçin.

1. Yeni bir söyleyiş ekleyin ve Enter tuşuna basın:

   ```Is there a form named hrf-234098```

    Varlık tahminlerini görmek için **Denetle'yi** seçin.

   > [!div class="mx-imgBorder"]
   > ![Test bölmesinde yeni söyleyiş testi](./media/get-started-portal-build-app/test-new-utterance.png)

   En yüksek öngörülen niyet doğru% 90 'ın üzerinde güven (0,977) ile **FindForm** olduğunu. **İnsan Kaynakları Form Numarası** hrf-234098 değeri ile ayıklanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlatmayı bitirdiyseniz ve bir sonraki hızlı başlatmaya geçmediğinizde, üstteki gezinme menüsünden **Uygulamalarım'ı** seçin. Ardından listeden uygulamanın sol onay kutusunu seçin ve listenin üstündeki bağlam araç çubuğundan **Sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [2. Bir uygulamayı dağıtma](get-started-portal-deploy-app.md)
