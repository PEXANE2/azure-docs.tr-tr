---
title: Azure Active Directory B2C dil özelleştirmesi | Microsoft Docs
description: Dil deneyimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5dd82cbb8370fd4c3ee0dca6a9acd0046c73c0ef
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716789"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C dil özelleştirmesi

Azure Active Directory B2C (Azure AD B2C) dil özelleştirmesi, Kullanıcı akışlarınızın müşteri gereksinimlerinize uyacak şekilde farklı dillere uyum sağlamasına izin verir.  Microsoft, [36 dil](#supported-languages)için Çeviriler sağlar, ancak herhangi bir dil için kendi çevirilerinizi de sağlayabilirsiniz. Deneyiminiz yalnızca tek bir dil için sağlanmış olsa bile, sayfalardaki tüm metinleri özelleştirebilirsiniz.

## <a name="how-language-customization-works"></a>Dil özelleştirmesi nasıl kullanılır?
Kullanıcı akışlarınızın hangi dillerde kullanılabilir olduğunu seçmek için dil özelleştirmesi ' nı kullanırsınız. Özellik etkinleştirildikten sonra, uygulamanızdan sorgu dizesi parametresini `ui_locales`sağlayabilirsiniz. Azure AD B2C ' a çağırdığınızda, sayfanız belirttiğiniz yerel ayara çevrilir. Bu tür bir yapılandırma, Kullanıcı akışlarınızdaki diller üzerinde tüm denetimleri denetlemenizi sağlar ve müşterinin tarayıcısının dil ayarlarını yoksayar.

Müşterinizin hangi dillerde gördüğü üzerinde bu denetim düzeyine ihtiyaç duymayabilir. Bir `ui_locales` parametre sağlamazsanız, müşterinin deneyimi tarayıcının ayarları tarafından belirlenir.  Desteklenen bir dil olarak ekleyerek, Kullanıcı akışlarınızın hangi dillerde çevrildiğinden de denetleyebilirsiniz. Bir müşterinin tarayıcısı, desteklemek istemediğiniz bir dili gösterecek şekilde ayarlandıysa, desteklenen kültürler için varsayılan olarak seçtiğiniz dil bunun yerine gösterilir.

- **UI-yerel ayar belirtilen dil**: Dil özelleştirmesini etkinleştirdikten sonra, Kullanıcı akışınız burada belirtilen dile çevrilir.
- **Tarayıcı tarafından istenen dil**: Hiçbir `ui_locales` parametre belirtilmemişse, *dil destekleniyorsa*Kullanıcı akışınız tarayıcı tarafından istenen dile çevrilir.
- **İlke varsayılan dili**: Tarayıcı bir dil belirtmezse veya desteklenmeyen bir değer belirtiyorsa, Kullanıcı akışı, Kullanıcı akışı varsayılan diline çevrilir.

>[!NOTE]
>Özel Kullanıcı özniteliklerini kullanıyorsanız kendi çevirilerinizi sağlamanız gerekir. Daha fazla bilgi için bkz. [dizelerinizi özelleştirme](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Ui_locales için istenen dilleri destekleme
Dil özelleştirmenin genel kullanıma sunulmadan önce oluşturulan ilkelerin önce bu özelliği etkinleştirmesi gerekir. Sonrasında oluşturulan ilkeler ve Kullanıcı akışları, varsayılan olarak dil özelleştirmesi etkindir.

Bir Kullanıcı akışında dil özelleştirmesini etkinleştirdiğinizde, `ui_locales` parametresini ekleyerek Kullanıcı akışının dilini kontrol edebilirsiniz.
1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
2. Çeviriler için etkinleştirmek istediğiniz kullanıcı akışına tıklayın.
3. **Dilleri**seçin.
4. **Dil özelleştirmesini etkinleştir**' i seçin.

## <a name="select-which-languages-in-your-user-flow-are-enabled"></a>Kullanıcı akışındaki hangi dillerin etkinleştirildiğini seçin
Kullanıcı akışınız için `ui_locales` parametresi olmadan tarayıcı tarafından istendiğinde çevrilecek bir dil kümesi etkinleştirin.
1. Kullanıcı akışınızdan önceki yönergelerden dil özelleştirmesi etkinleştirildiğinden emin olun.
2. Kullanıcı akışının **Diller** sayfasında, desteklemek istediğiniz bir dil seçin.
3. Özellikler bölmesinde, **etkin** ' i **Evet**olarak değiştirin.
4. Özellikler bölmesinin en üstünde **Kaydet** ' i seçin.

>[!NOTE]
>Bir `ui_locales` parametre sağlanmazsa, sayfa yalnızca etkinleştirildiğinde müşterinin tarayıcı diline çevrilir.
>

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirin
Dil özelleştirmesi, Kullanıcı akışındaki herhangi bir dizeyi özelleştirmenize olanak sağlar.
1. Kullanıcı akışınız, önceki yönergelerden dil özelleştirmesi etkinleştirilmiş olduğundan emin olun.
2. Kullanıcı akışının **Diller** sayfasında, özelleştirmek istediğiniz dili seçin.
3. **Sayfa düzeyi-kaynak dosyaları**altında, düzenlemek istediğiniz sayfayı seçin.
4. **Varsayılanları indir** ' i seçin (veya daha önce bu dili düzenlediyseniz, **geçersiz kılmaları indir** ).

Bu adımlar size Dizelerinizin düzenlenmesine başlamak için kullanabileceğiniz bir JSON dosyası sağlar.

### <a name="change-any-string-on-the-page"></a>Sayfadaki herhangi bir dizeyi değiştirme
1. JSON düzenleyicisinde önceki yönergelerden indirilen JSON dosyasını açın.
2. Değiştirmek istediğiniz öğeyi bulun.  Aradığınız dize `StringId` için arama yapabilir veya `Value` değiştirmek istediğiniz özniteliği arayabilirsiniz.
3. Özniteliğini, `Value` görüntülenmesini istediğiniz şekilde güncelleştirin.
4. Değiştirmek istediğiniz her dize için olarak `Override` `true`değiştirin.
5. Dosyayı kaydedin ve değişikliklerinizi karşıya yükleyin. (Karşıya yükleme denetimini, JSON dosyasını indirdiğiniz konum ile aynı yerde bulabilirsiniz.)

>[!IMPORTANT]
>Bir dizeyi geçersiz kılmanız gerekirse `Override` değerini olarak `true`ayarladığınızdan emin olun.  Değer değiştirilmemişse, giriş yok sayılır.
>

### <a name="change-extension-attributes"></a>Uzantı özniteliklerini Değiştir
Bir özel kullanıcı özniteliği için dizeyi değiştirmek isterseniz veya bir JSON öğesine eklemek istiyorsanız, şu biçimdedir:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Özel `<ExtensionAttribute>` Kullanıcı özniteme adıyla değiştirin.

Görüntülenecek `<ExtensionAttributeValue>` yeni dize ile değiştirin.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>LocalizedCollections kullanarak değer listesi sağlama
Yanıtlar için bir değer listesi kümesi sağlamak istiyorsanız, bir `LocalizedCollections` öznitelik oluşturmanız gerekir.  `LocalizedCollections`, `Name` ve`Value` çiftleri dizisidir. Öğelerin sırası görüntülendikleri sıra olacaktır.  Eklemek `LocalizedCollections`için aşağıdaki biçimi kullanın:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType",
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId`, bu `LocalizedCollections` özniteliğin yanıt olduğu Kullanıcı özniteliğidir.
* `Name`, kullanıcıya gösterilen değerdir.
* `Value`Bu seçenek belirlendiğinde talepte döndürülen değer.

### <a name="upload-your-changes"></a>Değişikliklerinizi karşıya yükleyin
1. JSON dosyanızdaki değişiklikleri tamamladıktan sonra B2C kiracınıza geri dönün.
2. **Kullanıcı akışları** ' nı seçin ve çeviriler için etkinleştirmek istediğiniz kullanıcı akışına tıklayın.
3. **Dilleri**seçin.
4. Çevirmek istediğiniz dili seçin.
5. Çevirileri sağlamak istediğiniz sayfayı seçin.
6. Klasör simgesini seçin ve karşıya yüklenecek JSON dosyasını seçin.

Değişiklikler Kullanıcı akışınıza otomatik olarak kaydedilir.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Dil özelleştirmesini kullanarak sayfa Kullanıcı arabirimini özelleştirme

HTML içeriğinizi yerelleştirmenin iki yolu vardır. Bir yol, [dil özelleştirmesini](active-directory-b2c-reference-language-customization.md)açmak için bir yoldur. Bu özelliğin etkinleştirilmesi, Azure AD B2C OpenID Connect parametresini `ui-locales`uç noktanıza iletmesine izin verir.  İçerik sunucunuz bu parametreyi dile özgü özelleştirilmiş HTML sayfaları sağlamak için kullanabilir.

Alternatif olarak, kullanılan yerel ayara göre farklı yerlerden içerik çekebilirsiniz. CORS etkin uç noktanıza, belirli diller için içerik barındırmak üzere bir klasör yapısı ayarlayabilirsiniz. Joker karakter değerini `{Culture:RFC5646}`kullanırsanız, doğru olanı çağıracaksınız.  Örneğin, bunun özel sayfa URI 'SI olduğunu varsayalım:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Sayfasını ' de `fr`yükleyebilirsiniz. Sayfa HTML ve CSS içeriğini çeker, buradan çekilir:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Özel Dil ekle

Ayrıca, Microsoft 'un şu anda çevirisi sağlamayan diller ekleyebilirsiniz. Kullanıcı akışındaki tüm dizeler için çevirileri sağlamanız gerekir.  Dil ve yerel ayar kodları ISO 639-1 standardına göre sınırlandırılmıştır.

1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
2. Özel diller eklemek istediğiniz kullanıcı akışına tıklayın ve sonra **Diller**' e tıklayın.
3. Sayfanın üst kısmından **Özel Dil ekle** ' yi seçin.
4. Açılan bağlam bölmesinde geçerli bir yerel ayar kodu girerek hangi dilde çeviri sağlayadığınızı belirler.
5. Her sayfa için, Ingilizce için bir geçersiz kılmalar kümesi indirebilir ve çeviriler üzerinde çalışma yapabilirsiniz.
6. JSON dosyaları ile işiniz bittiğinde, bunları her bir sayfa için karşıya yükleyebilirsiniz.
7. **Etkinleştir**' i seçtiğinizde Kullanıcı akışınız artık kullanıcılarınız için bu dili görüntüleyebilir.
8. Dili kaydedin.

>[!IMPORTANT]
>Kaydedebilmeniz için özel dilleri etkinleştirmeniz veya için geçersiz kılmaları yüklemeniz gerekir.
>

## <a name="additional-information"></a>Ek bilgiler

### <a name="page-ui-customization-labels-as-overrides"></a>Geçersiz kılmalar olarak sayfa Kullanıcı arabirimi özelleştirme etiketleri
Dil özelleştirmesini etkinleştirdiğinizde, sayfa UI özelleştirmesi kullanan etiketlere yönelik önceki düzenlemeleriniz Ingilizce (en) için bir JSON dosyasında kalıcıdır. Dil özelleştirmesindeki dil kaynaklarını karşıya yükleyerek etiketlerinizi ve diğer dizelerinizi değiştirmeye devam edebilirsiniz.
### <a name="up-to-date-translations"></a>Güncel Çeviriler
Microsoft, kullanım için en güncel çevirileri sağlamaya kararlıdır. Microsoft, çevirileri sürekli olarak geliştirir ve sizin için uyumluluk sağlar. Microsoft, genel terminolojilerin hatalarını ve değişikliklerini belirler ve Kullanıcı akışınızda sorunsuz bir şekilde çalışacak güncelleştirmeler yapar.
### <a name="support-for-right-to-left-languages"></a>Sağdan sola diller için destek
Microsoft şu anda sağdan sola diller için destek sağlamaz. Bu bunu, özel yerel ayarları kullanarak ve dizelerin görüntülenme biçimini değiştirmek için CSS kullanarak gerçekleştirebilirsiniz.  Bu özelliğe ihtiyacınız varsa lütfen [Azure geri bildirimde](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag)bu uygulamayı oylayın.
### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcısı çevirileri
Microsoft, `ui_locales` sosyal oturumlar için OIDC parametresini sağlar. Ancak Facebook ve Google dahil bazı sosyal kimlik sağlayıcıları bunları dikkate almaz.
### <a name="browser-behavior"></a>Tarayıcı davranışı
Chrome ve Firefox, kendi set dilleri için istek. Desteklenen bir dilise, varsayılan değer olarak gösterilir. Microsoft Edge Şu anda bir dil isteyemez ve doğrudan varsayılan dile gider.

### <a name="supported-languages"></a>Desteklenen diller

| Dil              | Dil kodu |
|-----------------------|---------------|
| Bangla                | milyar TL            |
| Çekçe                 | cs            |
| Danca                | da            |
| Almanca                | de            |
| Yunanca                 | el            |
| Türkçe               | tr-TR            |
| İspanyolca               | es            |
| Fince               | fi            |
| Fransızca                | fr            |
| Gucerat dili              | çubuğu            |
| Hintçe                 | n            |
| Hırvatça              | sa.            |
| Macarca             | hu            |
| İtalyanca               | it            |
| Japonca              | ja            |
| Kannada dili               | KN            |
| Korece                | ko            |
| Malayalam dili             | ml            |
| Marathi dili               | Mr            |
| Malay dili                 | ms            |
| Norveççe Bokmal      | nb            |
| Felemenkçe                 | nl            |
| Pencap dili               | VARS            |
| Lehçe                | pl            |
| Portekizce - Brezilya   | pt-br         |
| Portekizce-Portekiz | pt-pt         |
| Rumence              | ro            |
| Rusça               | ru            |
| Slovakça                | sor            |
| İsveççe               | sv            |
| Tamil dili                 | dolu            |
| Telugu dili                | ot            |
| Tay Dili                  | .            |
| Türkçe               | tr            |
| Çince - Basitleştirilmiş  | zh-Hans       |
| Çince - Geleneksel | zh-Hant       |
