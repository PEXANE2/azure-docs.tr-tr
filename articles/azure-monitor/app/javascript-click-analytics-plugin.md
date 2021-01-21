---
title: Application Insights JavaScript SDK 'Sı için analiz otomatik toplama eklentisi ' ne tıklayın
description: Application Insights JavaScript SDK 'Sı için Analytics otomatik toplama eklentisi ' ni yükleyip kullanma.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: lagayhar
ms.openlocfilehash: b2e9c267b0a3723c9ac7b3edd49e23b95741962f
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98660462"
---
# <a name="click-analytics-auto-collection-plugin-for-application-insights-javascript-sdk"></a>Application Insights JavaScript SDK 'Sı için analiz otomatik toplama eklentisi ' ne tıklayın

Bu eklenti Web sayfalarında tıklama olaylarını otomatik olarak izler ve olay telemetrisini doldurmak için HTML öğelerindeki Data-* özniteliklerini kullanır.

## <a name="getting-started"></a>Başlarken

Kullanıcılar NPM aracılığıyla Analytics otomatik toplama eklentisini ayarlayabilir.

### <a name="npm-setup"></a>NPM kurulumu

NPM paketini Install:

```bash
npm install --save @microsoft/applicationinsights-clickanalytics-js @microsoft/applicationinsights-web
```

```js

import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { ClickAnalyticsPlugin } from '@microsoft/applicationinsights-clickanalytics-js';

const clickPluginInstance = new ClickAnalyticsPlugin();
// Click Analytics configuration
const clickPluginConfig = {
  autoCapture: true
};
// Application Insights Configuration
const configObj = {
  instrumentationKey: "YOUR INSTRUMENTATION KEY",
  extensions: [clickPluginInstance],
  extensionConfig: {
    [clickPluginInstance.identifier]: clickPluginConfig
  },
};

const appInsights = new ApplicationInsights({ config: configObj });
appInsights.loadAppInsights();
```

## <a name="how-to-effectively-use-the-plugin"></a>Eklentiyi etkin bir şekilde kullanma

1. Tıklama olaylarından oluşturulan telemetri verileri `customEvents` , Azure portal Application Insights bölümünde olarak depolanır.
2. `name`CustomEvent 'in bu kuralları aşağıdaki kurallara göre doldurulur:
    1.  `id`İçinde belirtilen, `data-*-id` CustomEvent adı olarak kullanılacaktır. Örneğin, tıklanan HTML öğesinde "Data-Sample-id" = "Button1" özniteliği varsa, "Button1", customEvent adı olacaktır.
    2. Böyle bir öznitelik yoksa ve eğer `useDefaultContentNameOrId` yapılandırma içinde olarak ayarlandıysa, `true` TıKLANAN öğenin HTML özniteliği `id` veya öğenin Içerik adı, CustomEvent adı olarak kullanılır.
    3. `useDefaultContentNameOrId`Yanlışsa, customEvent adı "not_specified" olacaktır.

    > [!TIP]
    > Önerilerimiz `useDefaultContentNameOrId` anlamlı veriler oluşturmak için true olarak ayarlanmıştır.  
3. `parentDataTag` iki şey vardır:
    1. Bu etiket varsa, eklenti, `data-*` tıklanan öğenin tüm üst HTML öğelerinden öznitelikler ve değerler getirir.
    2. , Verimliliği artırmak için, eklenti bu etiketi bir bayrak olarak kullanır. Bu, karşılaştığı zaman, DOM 'ın (Belge Nesne Modeli) yukarı doğru işlemesini durdurur.
    
    > [!CAUTION]
    > Bir kez `parentDataTag` kullanıldığında, SDK yalnızca KULLANDıĞıNıZ HTML öğesini değil, uygulamanın tamamında üst etiketleri aramaya başlar.
4. `customDataPrefix` Örneğin, Kullanıcı tarafından belirtilen her zaman ile başlamalıdır `data-` `data-sample-` . HTML 'de `data-*` Genel öznitelikler, özel veri öznitelikleri olarak adlandırılan özniteliklerin bir sınıfını oluşturur ve bu, özel bIlgIlerIn HTML ile BETIKLERLE Dom gösterimi arasında değiştirilmesini sağlar. Eski tarayıcılar (Internet Explorer, Safari), ile başlamadıkça anlamadığı öznitelikleri bırakacaktır `data-` .

    `*`İçindeki, `data-*` [XML adları üretim kuralıyla](https://www.w3.org/TR/REC-xml/#NT-Name) aşağıdaki kısıtlamalara göre herhangi bir adla değiştirilebilir:
    - Ad "xml" ile başlamamalıdır, bu harfler için herhangi bir örnek kullanılır.
    - Ad herhangi bir noktalı virgül (U + 003A) içermemelidir.
    - Adda büyük harfler bulunmamalıdır.

## <a name="configuration"></a>Yapılandırma

| Ad                  | Tür                               | Varsayılan | Description                                                                                                                              |
| --------------------- | -----------------------------------| --------| ---------------------------------------------------------------------------------------------------------------------------------------- |
| Oto yakala           | boolean                            | true    | Otomatik yakalama yapılandırması.                                                                                                         |
| callback              | [IValueCallback](#ivaluecallback)  | null    | Geri çağrılar yapılandırması.                                                                                                                 |
| pageTags              | string                             | null    | Sayfa etiketleri.                                                                                                                               |
| Veri etiketleri              | [Icustomdatatags](#icustomdatatags)| null    | Tıklama verilerini yakalamak için kullanılan varsayılan etiketleri geçersiz kılmak için belirtilen özel veri etiketleri.                                                           |
| urlCollectHash        | boolean                            | yanlış   | URL 'nin bir "#" karakterinden sonra değerlerin günlüğe kaydedilmesini etkinleştir.                                                                          |
| urlCollectQuery       | boolean                            | yanlış   | URL 'nin sorgu dizesinin günlüğe kaydedilmesini etkinleştir.                                                                                      |
| behaviorValidator     | İşlev                           | null  | Değer doğrulaması için kullanılacak geri çağırma işlevi `data-*-bhvr` . Daha fazla bilgi için [behaviorValidator bölümüne](#behaviorvalidator)gidin.|
| Defaultrisclick Kbhvr | dize (veya) numarası                 | ''      | Sağ tıklama olayı oluştuğunda varsayılan davranış değeri. Öğesi özniteliği içeriyorsa, bu değer geçersiz kılınır `data-*-bhvr` . |
| Dropınvalidevents     | boolean                            | yanlış   | Faydalı tıklama verilerine sahip olmayan olayları bırakma bayrağı.                                                                                   |

### <a name="ivaluecallback"></a>IValueCallback

| Ad               | Tür     | Varsayılan | Açıklama                                                                             |
| ------------------ | -------- | ------- | --------------------------------------------------------------------------------------- |
| pageName           | İşlev | null    | Varsayılan pageName yakalama davranışını geçersiz kılmak için işlev.                           |
| pageActionPageTags | İşlev | null    | PageAction olayı sırasında toplanan varsayılan pageTags genişletmek için bir geri çağırma işlevi.  |
| contentName        | İşlev | null    | Özelleştirilmiş contentName 'i doldurmak için bir geri çağırma işlevi.                                 |

### <a name="icustomdatatags"></a>Icustomdatatags

| Ad                      | Tür    | Varsayılan   | HTML içinde kullanılacak varsayılan etiket |   Description                                                                                |
|---------------------------|---------|-----------|-------------|----------------------------------------------------------------------------------------------|
| Usedefaultcontentnameorıd | boolean | yanlış     | Yok         |Belirli bir öğe varsayılan customDataPrefix ile etiketlenmemişse veya customDataPrefix Kullanıcı tarafından sağlanmadıysa, contentName için standart HTML özniteliğini toplar. |
| customDataPrefix          | string  | `data-`   | `data-*`| Otomatik yakalama içerik adı ve belirtilen önek ile etiketlenmiş öğelerin değeri. Örneğin, `data-*-id` `data-<yourcustomattribute>` HTML etiketleri içinde kullanılabilir.   |
| aiBlobAttributeTag        | string  | `ai-blob` |  `data-ai-blob`| Eklenti bağımsız öznitelikler yerine JSON blob özniteliğini destekler `data-*` . |
| metaDataPrefix            | string  | null      | Yok  | Yakalama sırasında, Otomatik yakalama HTML baş meta öğesi adı ve belirtilen önek ile içerik. Örneğin, `custom-` HTML meta etiketinde kullanılabilir. |
| captureAllMetaDataContent | boolean | yanlış     | Yok   | Tüm HTML kafasının meta öğe adlarını ve içeriğini otomatik olarak yakala. Varsayılan değer false’tur. Etkinleştirilirse bu, sağlanan metaDataPrefix 'i geçersiz kılar. |
| parentDataTag             | string  | null      |  Yok  | Bu etiketle karşılaşıldığında DOM 'ın içerik adını ve değerlerini yakalamasını engeller. Örneğin, `data-<yourparentDataTag>` HTML etiketlerinde kullanılabilir.|
| dntDataTag                | string  | `ai-dnt`  |  `data-ai-dnt`| Bu özniteliğe sahip HTML öğeleri telemetri verilerini yakalamaya yönelik eklenti tarafından yok sayılacak.|

### <a name="behaviorvalidator"></a>behaviorValidator

BehaviorValidator işlevleri, koddaki etiketli davranışların önceden tanımlanmış bir listeyle uyumlu olup olmadığını otomatik olarak denetler. Bu, etiketli davranışların kuruluşunuzun belirlenen sınıflandırmasında tutarlı olmasını sağlar. Bu gerekli değildir veya çoğu Azure Izleyici müşterilerinin bunu kullanması beklenir, ancak gelişmiş senaryolar için kullanılabilir. Bu uzantının parçası olarak kullanıma sunulan üç farklı behaviorValidator callback işlevi vardır. Ancak, sunulan işlevler gereksiniminizi çözmezse kullanıcılar kendi geri arama işlevlerini kullanabilir. Amaç, kendi davranış veri yapınızı getirmesiyken, eklenti bu Doğrulayıcı işlevini kullanarak veri etiketlerinden davranışları ayıklamaktır.

| Ad                   | Açıklama                                                                        |
| ---------------------- | -----------------------------------------------------------------------------------|
| BehaviorValueValidator | Davranış veri yapınız bir dizeler diziyse bu geri çağırma işlevini kullanın.|
| BehaviorMapValidator   | Davranış veri yapınız bir sözlüktür ise bu geri çağırma işlevini kullanın.       |
| BehaviorEnumValidator  | Davranış veri yapınız bir sabit listesi ise bu geri çağırma işlevini kullanın.            |

#### <a name="sample-usage-with-behaviorvalidator"></a>BehaviorValidator ile örnek kullanım

```js
var clickPlugin = Microsoft.ApplicationInsights.ClickAnalyticsPlugin;
var clickPluginInstance = new clickPlugin();

// Behavior enum values
var behaviorMap = {
  UNDEFINED: 0, // default, Undefined

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Page Experience [1-19]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  NAVIGATIONBACK: 1, // Advancing to the previous index position within a webpage
  NAVIGATION: 2, // Advancing to a specific index position within a webpage
  NAVIGATIONFORWARD: 3, // Advancing to the next index position within a webpage
  APPLY: 4, // Applying filter(s) or making selections
  REMOVE: 5, // Applying filter(s) or removing selections
  SORT: 6, // Sorting content
  EXPAND: 7, // Expanding content or content container
  REDUCE: 8, // Sorting content
  CONTEXTMENU: 9, // Context Menu
  TAB: 10, // Tab control
  COPY: 11, // Copy the contents of a page
  EXPERIMENTATION: 12, // Used to identify a third party experimentation event
  PRINT: 13, // User printed page
  SHOW: 14, //  Displaying an overlay
  HIDE: 15, //  Hiding an overlay
  MAXIMIZE: 16, //  Maximizing an overlay
  MINIMIZE: 17, // Minimizing an overlay
  BACKBUTTON: 18, //  Clicking the back button

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Scenario Process [20-39]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  STARTPROCESS: 20, // Initiate a web process unique to adopter
  PROCESSCHECKPOINT: 21, // Represents a checkpoint in a web process unique to adopter
  COMPLETEPROCESS: 22, // Page Actions that complete a web process unique to adopter
  SCENARIOCANCEL: 23, // Actions resulting from cancelling a process/scenario

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Download [40-59]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  DOWNLOADCOMMIT: 40, // Initiating an unmeasurable off-network download
  DOWNLOAD: 41, // Initiating a download

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Search [60-79]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SEARCHAUTOCOMPLETE: 60, // Auto-completing a search query during user input
  SEARCH: 61, // Submitting a search query
  SEARCHINITIATE: 62, // Initiating a search query
  TEXTBOXINPUT: 63, // Typing or entering text in the text box

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Commerce [80-99]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  VIEWCART: 82, // Viewing the cart
  ADDWISHLIST: 83, // Adding a physical or digital good or services to a wishlist
  FINDSTORE: 84, // Finding a physical store
  CHECKOUT: 85, // Before you fill in credit card info
  REMOVEFROMCART: 86, // Remove an item from the cart
  PURCHASECOMPLETE: 87, // Used to track the pageView event that happens when the CongratsPage or Thank You page loads after a successful purchase
  VIEWCHECKOUTPAGE: 88, // View the checkout page
  VIEWCARTPAGE: 89, // View the cart page
  VIEWPDP: 90, // View a PDP
  UPDATEITEMQUANTITY: 91, // Update an item's quantity
  INTENTTOBUY: 92, // User has the intent to buy an item
  PUSHTOINSTALL: 93, // User has selected the push to install option

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Authentication [100-119]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SIGNIN: 100, // User sign-in
  SIGNOUT: 101, // User sign-out

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Social [120-139]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SOCIALSHARE: 120, // "Sharing" content for a specific social channel
  SOCIALLIKE: 121, // "Liking" content for a specific social channel
  SOCIALREPLY: 122, // "Replying" content for a specific social channel
  CALL: 123, // Click on a "call" link
  EMAIL: 124, // Click on an "email" link
  COMMUNITY: 125, // Click on a "community" link

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Feedback [140-159]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  VOTE: 140, // Rating content or voting for content
  SURVEYCHECKPOINT: 145, // reaching the survey page/form

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Registration, Contact [160-179]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  REGISTRATIONINITIATE: 161, // Initiating a registration process
  REGISTRATIONCOMPLETE: 162, // Completing a registration process
  CANCELSUBSCRIPTION: 163, // Canceling a subscription
  RENEWSUBSCRIPTION: 164, // Renewing a subscription
  CHANGESUBSCRIPTION: 165, // Changing a subscription
  REGISTRATIONCHECKPOINT: 166, // Reaching the registration page/form

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Chat [180-199]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  CHATINITIATE: 180, // Initiating a chat experience
  CHATEND: 181, // Ending a chat experience

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Trial [200-209]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  TRIALSIGNUP: 200, // Signing-up for a trial
  TRIALINITIATE: 201, // Initiating a trial

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Signup [210-219]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SIGNUP: 210, // Signing-up for a notification or service
  FREESIGNUP: 211, // Signing-up for a free service

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Referals [220-229]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  PARTNERREFERRAL: 220, // Navigating to a partner's web property

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Intents [230-239]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  LEARNLOWFUNNEL: 230, // Engaging in learning behavior on a commerce page (ex. "Learn more click")
  LEARNHIGHFUNNEL: 231, // Engaging in learning behavior on a non-commerce page (ex. "Learn more click")
  SHOPPINGINTENT: 232, // Shopping behavior prior to landing on a commerce page

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Video [240-259]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  VIDEOSTART: 240, // Initiating a video
  VIDEOPAUSE: 241, // Pausing a video
  VIDEOCONTINUE: 242, // Pausing or resuming a video.
  VIDEOCHECKPOINT: 243, // Capturing predetermined video percentage complete.
  VIDEOJUMP: 244, // Jumping to a new video location.
  VIDEOCOMPLETE: 245, // Completing a video (or % proxy)
  VIDEOBUFFERING: 246, // Capturing a video buffer event
  VIDEOERROR: 247, // Capturing a video error
  VIDEOMUTE: 248, // Muting a video
  VIDEOUNMUTE: 249, // Unmuting a video
  VIDEOFULLSCREEN: 250, // Making a video full screen
  VIDEOUNFULLSCREEN: 251, // Making a video return from full screen to original size
  VIDEOREPLAY: 252, // Making a video replay
  VIDEOPLAYERLOAD: 253, // Loading the video player
  VIDEOPLAYERCLICK: 254, //  Click on a button within the interactive player
  VIDEOVOLUMECONTROL: 255, //  Click on video volume control
  VIDEOAUDIOTRACKCONTROL: 256, // Click on audio control within a video
  VIDEOCLOSEDCAPTIONCONTROL: 257, //  Click on the closed caption control
  VIDEOCLOSEDCAPTIONSTYLE: 258, //  Click to change closed caption style
  VIDEORESOLUTIONCONTROL: 259, //  Click to change resolution

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  //    Advertisement Engagement [280-299]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  ADBUFFERING: 283, // Ad is buffering
  ADERROR: 284, // Ad error
  ADSTART: 285, // Ad start
  ADCOMPLETE: 286, // Ad complete
  ADSKIP: 287, // Ad skipped
  ADTIMEOUT: 288, // Ad timed-out
  OTHER: 300 // Other
};

// Application Insights Configuration
var configObj = {
  instrumentationKey: "YOUR INSTRUMENTATION KEY",
  extensions: [clickPluginInstance],
  extensionConfig: {
    [clickPluginInstance.identifier]: {
      behaviorValidator: Microsoft.ApplicationInsights.BehaviorMapValidator(behaviorMap),
      defaultRightClickBhvr: 9
    },
  },
};
var appInsights = new Microsoft.ApplicationInsights.ApplicationInsights({
  config: configObj
});
appInsights.loadAppInsights();
```

## <a name="sample-app"></a>Örnek uygulama

[Analytics otomatik toplama eklentisi etkin olan basit Web uygulaması](https://go.microsoft.com/fwlink/?linkid=2152871).

## <a name="next-steps"></a>Sonraki adımlar

- Analiz otomatik toplama eklentisi için [GitHub deposunu](https://github.com/microsoft/ApplicationInsights-JS/tree/master/extensions/applicationinsights-clickanalytics-js) ve [NPM paketini](https://www.npmjs.com/package/@microsoft/applicationinsights-clickanalytics-js) inceleyin.
- En üstteki tıklamaları analiz etmek ve kullanılabilir boyutlara göre dilimlemek için [kullanım deneyiminde olay analizini](usage-segmentation.md) kullanın.
- [Log Analytics](../log-query/log-analytics-tutorial.md#write-a-query)' deki CustomEvents tablosundaki customdimensions özniteliğinde bulunan içerik alanının altında bulunan verileri bulun. Ek rehberlik için bkz. [örnek uygulama](https://go.microsoft.com/fwlink/?linkid=2152871) .
- Tıklama verilerinin özel görselleştirmelerini oluşturmak için bir [çalışma kitabı](../platform/workbooks-overview.md) oluşturun.
