---
title: 'Hızlı Başlangıç: LUSıS portalında yeni bir uygulama oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, LUı portalında yeni bir uygulama oluşturacaksınız. Uygulama, amaç ve varlıkların temel parçalarını oluşturun. Tahmin edilen amacı almak için etkileşimli test paneline örnek bir Kullanıcı yazarak uygulamayı test edin. Uygulama oluşturma ücretsizdir; Azure aboneliği gerektirmez.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: f54643883028d93b56c7e122f43db95bb6d0b8c1
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560868"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Hızlı Başlangıç: LUSıS portalında yeni bir uygulama oluşturma

Bu hızlı başlangıçta, [LUI portalında](https://www.luis.ai)yeni bir uygulama oluşturacaksınız. İlk olarak bir uygulama, **Amaç**ve **varlıkların**temel parçalarını oluşturursunuz. Ardından, tahmin edilen amacı almak için etkileşimli test panelinde örnek bir Kullanıcı yazarak uygulamayı test edersiniz.

Uygulama oluşturma ücretsizdir ve bir Azure aboneliği gerektirmez. Uygulamanızı dağıtmaya hazırsanız, [uygulamayı dağıtmaya yönelik hızlı başlangıç](get-started-portal-deploy-app.md)bölümüne bakın. Azure bilişsel hizmet kaynağı oluşturma ve uygulamaya atama işlemlerinin nasıl yapılacağını gösterir.

## <a name="create-an-app"></a>Uygulama oluşturma

1. Lui [portalını](https://www.luis.ai) bir tarayıcıda açın ve oturum açın. İlk kez oturum açıyorsanız, ücretsiz bir LUU portalı Kullanıcı hesabı oluşturmanız gerekir.

1. Bağlam araç çubuğundan **Yeni uygulama oluştur** ' u seçin.

   [![LUSıS portalında yeni uygulama oluşturma](./media/get-started-portal-build-app/create-app-in-portal.png)](./media/get-started-portal-build-app/create-app-in-portal.png#lightbox)

1. Açılır pencerede, uygulamayı aşağıdaki ayarlarla yapılandırın ve **bitti**' yi seçin.

   |Ayar adı| Value | Amaç|
   |--|--|--|
   |Ad|`myEnglishApp`|Benzersiz LUSıS uygulama adı<br>gerekli|
   |Kültür|**İngilizce**|Kullanıcılardan gelen söylenme dili, **en-US**<br>gerekli|
   |Açıklama|`App made with LUIS Portal`|Uygulamanın açıklaması<br>isteğe bağlı|
   | | | |

   ![Yeni uygulama ayarlarını girin](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Amaç oluşturma

LUIN uygulaması oluşturulduktan sonra, amaçları oluşturmanız gerekir. Amaçlar, kullanıcıdan metin sınıflandırmanız için bir yoldur. Örneğin, insan kaynakları uygulamasında iki işlev olabilir. Kişilere yardım etmek için:

 1. İşler için bul ve Uygula
 1. İşler için uygulanacak formları bulma

Uygulamanın _iki farklı amacı_ aşağıdaki amaçlar için hizalanacaktır:

|Amaç|Kullanıcıdan örnek metin<br>_söylenişi_ olarak bilinen|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Amaç oluşturmak için aşağıdaki adımları izleyin:

1. Uygulama oluşturulduktan sonra, **derleme** bölümünün **amaçlar** sayfasında olursunuz. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

   [![Yeni amaç Oluştur düğmesini seçin](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Amaç adını `FindForm`girip **bitti**' yi seçin.

   ![FindForm 'un amaç adını girin](./media/get-started-portal-build-app/create-new-intent-dialog.png)

## <a name="add-an-example-utterance"></a>Bir örnek utterance Ekle

Amaçları oluşturduktan sonra örnek bir parametre ekleyin. Örnek, kullanıcının bir sohbet bot veya diğer istemci uygulamasına girdiği metindir. Kullanıcı metninin amacı bir LUO amacı ile eşlenir.

Bu örnek uygulamanın `FindForm` amacı için, örnek olarak form numarasını içerecektir. İstemci uygulaması, kullanıcının isteğini yerine getirmek için form numarasına ihtiyaç duyuyor, bu yüzden bunu utterde eklemek önemlidir.

[![FindForm amacı için örnek bir parametre girin](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

`FindForm` Amaç için aşağıdaki 15 örnek utbotları ekleyin.

|#|Örnek konuşmalar|
|--|--|
|1\.|HRF-123456 aranıyor|
|2|İnsan kaynakları ne tür HRF-234591?|
|3|HRF-345623, burada|
|4|Bana HRF-345794 göndermek mümkün|
|5|Bir iç iş için uygulanacak HRF-234695 gerekiyor mu?|
|6|HRF-234091 ile bir iş için yöneticimin uygulamasını öğrenmesi gerekir|
|7|HRF-234918 ' I nereden gönderebilirim? Alındığını bir e-posta yanıtı alıyorum?|
|8|HRF-234555|
|9|HRF-234987 ne zaman güncelleştirildi?|
|10|Mühendislik konumları için uygulanacak HRF-876345 formunu kullanıyorum|
|11|Açık REQ için yeni bir HRF-765234 sürümü gönderildi mi?|
|12|Uluslararası işler için HRF-234234 ' I kullanıyorum mi?|
|13|HRF-234598 yazım hatası|
|14|yeni gereksinimler için HRF-234567 düzenlenecek|
|15|HRF-123456, HRF-123123, HRF-234567|

Bu örnek, tasarıma göre aşağıdaki yollarla farklılık gösterir:

* söylenişi uzunluğu
* edilen
* sözcük seçimi
* fiil zaman hali (yani, was, olacaktır)
* sözcük sırası



## <a name="create-a-regular-expression-entity"></a>Normal ifade varlığı oluşturma

Çalışma zamanı tahmin yanıtında form numarasını döndürmek için formun bir varlık olarak işaretlenmesi gerekir. Form numarası metni yüksek düzeyde yapılandırıldığı için, normal bir ifade varlığı kullanarak işaretleyebilirsiniz. Varlığı aşağıdaki adımlarla oluşturun:

1. Sol taraftaki menüden **varlıklar** ' ı seçin.

1. **Varlıklar** sayfasında **yeni varlık oluştur** ' u seçin.

1. Adı `Human Resources Form Number`girin, **Regex** varlık türünü seçin ve normal ifadeyi `hrf-[0-9]{6}`girin. Bu giriş, tam olarak 6 basamağa `hrf-`izin veren, değişmez karakterlerle eşleşir.

   ![Normal ifade varlığı için varlık bilgilerini girin](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. **Done** (Bitti) öğesini seçin.

## <a name="add-example-utterances-to-the-none-intent"></a>Hiçbiri amacına örnek ekleme

**Hiçbiri** amacı geri dönüş amacını ve boş bırakılmalıdır. Bu amaç, uygulamanın diğer amaçları için eklemiş olduğunuz her 10 örnek için bir söylenişi içermelidir.

**Hiçbir** üyenin örnek utbotları, istemci uygulama etki alanınız dışında olmalıdır.

1. Sol menüden **amaçlar** ' ı seçin ve ardından amaçlar listesinden **hiçbiri** ' ni seçin.

1. Amacına aşağıdaki örnek söyleyeni ekleyin:

   |Hiçbiri amacı örnek söyliği|
   |--|
   |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
   |Order a pizza for me (Bana bir pizza söyle)|
   |Penguins in the ocean (Okyanustaki penguenler)|

   Bu insan kaynakları uygulaması için bu örnek, etki alanının dışındadır. İnsan kaynakları etki alanınız hayvanlar, yiyecek veya okyanus içeriyorsa, **none** amacı için farklı örnek utbotları kullanmanız gerekir.

## <a name="train-the-app"></a>Uygulamayı eğitme

Sağ üst taraftaki menüde, amaç ve varlık modeli değişikliklerini uygulamanın geçerli sürümüne uygulamak için **eğitme** ' yi seçin.

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Örnek mekan normal ifade varlığına bakın

1. Sol menüden **amaçlar** ' a tıklayarak varlığın **FindForm** hedefi ' nde bulunduğunu doğrulayın. Ardından **FindForm** hedefini seçin.

   Varlık, örnek noktada göründüğü yerde işaretlenir. Varlık adı yerine özgün metni görmek isterseniz, **varlıklar görünümünü** araç çubuğundan değiştirin.

   [![Varlıklar ile işaretlenen tüm örnek örnekler](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Etkileşimli test bölmesiyle yeni uygulamanızı test etme

Varlığın, uygulamanın henüz görülmemiş yeni dıklardan ayıklandığını doğrulamak için, lug portalındaki etkileşimli **Test** bölmesini kullanın.

1. Sağ üst menüden **Test** ' i seçin.

1. Yeni bir söylenişi ekleyin ve ardından ENTER tuşuna basın:

   ```Is there a form named hrf-234098```

   ![Test bölmesinde yeni söylenişi test etme](./media/get-started-portal-build-app/test-new-utterance.png)

   En iyi tahmin edilen amaç,% 90 güvenilirlikli (0,977) ile doğru **FindForm** . **Insan kaynakları form numarası** varlığı, HRF-234098 değeri ile ayıklanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç ile işiniz bittiğinde ve sonraki hızlı başlangıca geçmeden, üst gezinti menüsünden **uygulamalarım** ' ı seçin. Ardından listedeki uygulamanın sol onay kutusunu seçin ve listenin üzerindeki bağlam araç çubuğundan **Sil** ' i seçin.

[![Uygulamayı uygulamalarım listesinden sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [2. Uygulama dağıtma](get-started-portal-deploy-app.md)
