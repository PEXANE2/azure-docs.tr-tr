---
title: İstemci tarafına reklam ekleme | Microsoft Dokümanlar
description: Bu makalede, müşteri tarafında medyanıza nasıl reklam ekilir gösteriş.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 274ee09ae98dd229b255e58261f462e322be9f89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77565749"
---
# <a name="inserting-ads-on-the-client-side"></a>İstemci tarafına reklam ekleme
Bu makalede, istemci tarafında çeşitli reklam türleri eklemek için nasıl bilgi içerir.

Canlı akış videolarında kapalı altyazı ve reklam desteği hakkında bilgi için [Desteklenen Kapalı Altyazı ve Reklam Ekleme Standartları'na](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads)bakın.

> [!NOTE]
> Azure Media Player şu anda Reklamları desteklemiyor.
> 
> 

## <a name="inserting-ads-into-your-media"></a><a id="insert_ads_into_media"></a>Medyanıza Reklam Ekleme
Azure Medya Hizmetleri, Windows Media Platformu: Player Frameworks aracılığıyla reklam ekleme desteği sağlar. Windows 8, Silverlight, Windows Phone 8 ve iOS aygıtları için reklam destekli oyuncu çerçeveleri kullanılabilir. Her oyuncu çerçevesi, bir oyuncu uygulamasını nasıl uygulayacağınızı gösteren örnek kod içerir. Medyalistenize ekebileceğiniz üç farklı türde reklam vardır: listeniz.

* **Doğrusal** – ana videoyu duraklatan tam kare reklamlar.
* **Doğrusal olmayan** – ana video yürütülderken görüntülenen kaplama reklamları, genellikle oynatıcının içine yerleştirilen bir logo veya diğer statik görüntü.
* **Companion** – oyuncunun dışında görüntülenen reklamlar.

Reklamlar, ana videonun zaman çizgisinin herhangi bir noktasına yerleştirilebilir. Oyuncuya reklamı ne zaman oynatacaklarını ve hangi reklamları niçin oynayacağını söylemelisiniz. Bu, standart XML tabanlı bir dizi dosya kullanılarak yapılır: Video Reklam Hizmeti Şablonu (VAST), Dijital Video Çoklu Reklam Çalma Listesi (VMAP), Media Abstract Sıralama Şablonu (MAST) ve Dijital Video Oynatıcı Reklam Arabirimi Tanımı (VPAID). VAST dosyaları hangi reklamların görüntülenerebiyi belirtir. VMAP dosyaları çeşitli reklamların ne zaman oynatılacaklarını belirtir ve VAST XML içerir. MAST dosyaları, VAST XML de içerebilen reklamları sıralamanın başka bir yoludur. VPAID dosyaları video oynatıcı ile reklam veya reklam sunucusu arasında bir arabirim tanımlar.

Her oyuncu çerçevesi farklı çalışır ve her biri kendi makalesinde ele alınacaktır. Bu makalede, reklam eklemek için kullanılan temel mekanizmalar açıklanmaktadır. Video oynatıcı uygulamaları bir reklam sunucusundan reklam isteğinde bulunur. Reklam sunucusu çeşitli şekillerde yanıt verebilir:

* VAST dosyayı döndürme
* Bir VMAP dosyayı döndürme (gömülü VAST ile)
* MAST dosyalarını döndürme (gömülü VAST ile)
* VPAID reklamlarıyla VAST dosyalarını döndürme

### <a name="using-a-video-ad-service-template-vast-file"></a>Görüntülü Reklam Hizmeti Şablonu (VAST) Dosyakullanma
VAST dosyası, hangi reklam ın veya reklamların görüntülenemeye karar verebolur. Aşağıdaki XML doğrusal bir reklam için VAST dosyasının bir örneğidir:

```xml
    <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
```

Doğrusal reklam, <**Lineer**> öğesi tarafından açıklanmıştır. Reklamın süresini, olayları izlemeyi, tıklamayı, tıklama izlemeyi ve bir dizi **MediaFile** öğesini belirtir. İzleme olayları <**İzleme Olayları**> öğesi içinde belirtilir ve bir reklam sunucusunun reklamı görüntülerken meydana gelen çeşitli olayları izlemesine olanak sağlar. Bu durumda başlangıç, orta nokta, tamamlama ve genişletme olayları izlenir. Başlangıç olayı, reklam görüntülendiğinde oluşur. Orta nokta olayı, reklamın zaman çizelgesinin en az %50'si görüntülendiğinde oluşur. Olayın tamamı, reklam sonuna kadar yayınlandığında gerçekleşir. Genişletme olayı, kullanıcı video oynatıcıyı tam ekrana genişlettiğinde oluşur. Tıklamalar, <**VideoClicks**> öğesi içindeki <**ClickThrough**> öğesiyle belirtilir ve kullanıcı reklamı tıkladığında görüntülemek üzere bir kaynağa URI belirtir. ClickTracking, ayrıca **<VideoClicks**> öğesi içinde yer alan <**ClickTracking**> öğesinde belirtilir ve kullanıcı reklamı tıkladığında oyuncunun talep etmesi için bir izleme kaynağı belirtir. **MediaFile** <> öğeleri, bir reklamın belirli bir kodlaması hakkında bilgi belirtir. Birden fazla <**MediaFile**> öğesi olduğunda, video oynatıcı platform için en iyi kodlamayı seçebilir.

Doğrusal reklamlar belirli bir sırada görüntülenebilir. Bunu yapmak için, `<Ad>` VAST dosyasına ek öğeler ekleyin ve sıra özniteliğini kullanarak sırayı belirtin. Aşağıdaki örnekte bu gösterilmektedir:

```xml
    <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
```

Doğrusal olmayan reklamlar `<Creative>` da bir öğede belirtilir. Aşağıdaki örnekte `<Creative>` doğrusal olmayan bir reklamı açıklayan bir öğe gösterilmektedir.

```xml
    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>
```

Doğrusal **Olmayan Adlar**> öğesi <, her biri doğrusal olmayan bir reklamı tanımlayabilen bir veya daha fazla <**Doğrusal Olmayan**> öğesi içerebilir. **Doğrusal Olmayan** <> öğesi doğrusal olmayan reklam Için kaynak belirtir. Kaynak, <**staticresource**>, <bir **IFrameResource**> veya <bir **HTMLResource**> olabilir. \<**StaticResource**>, HTML olmayan bir kaynağı açıklar ve kaynağın nasıl görüntüleneceğini belirten bir creativeType özniteliği tanımlar:

Resim/gif, resim/jpeg, resim/png – kaynak html <**img**> etiketinde görüntülenir.

Uygulama/x-javascript – kaynak HTML <**komut dosyası**> etiketinde görüntülenir.

Uygulama/x-shockwave-flash – kaynak bir Flash oynatıcıda görüntülenir.

**IFrameResource, IFrame'de** görüntülenebilen bir HTML kaynağını açıklar. **HTMLResource,** bir web sayfasına eklenebilecek bir HTML kodu parçasını açıklar. **İzleme Olaylar,** olay gerçekleştiğinde istekte bulunmak için izleme olaylarını ve URI'yi belirtir. Bu örnekte, acceptInvitation ve collapse olayları izlenir. **Doğrusal Olmayan Ads** öğesi ve çocukları hakkında daha fazla bilgi için IAB.NET/VAST bakın. **TrackingEvents** öğesinin **Doğrusal Olmayan** öğe yerine **Doğrusal Olmayan Ads** öğesi içinde bulunduğunu unutmayın.

Tamamlayıcı reklamlar bir `<CompanionAds>` öğe içinde tanımlanır. Öğe `<CompanionAds>` bir veya daha `<Companion>` fazla öğe içerebilir. Her `<Companion>` öğe, bir tamamlayıcı reklam `<StaticResource>` `<IFrameResource>`açıklar `<HTMLResource>` ve doğrusal olmayan bir reklamla aynı şekilde belirtilmiş bir , veya içerebilir. VAST dosyası birden çok yardımcı reklam içerebilir ve oynatıcı uygulaması görüntülemek için en uygun reklamı seçebilir. VAST hakkında daha fazla bilgi için [VAST 3.0'a](https://www.iab.net/media/file/VASTv3.0.pdf)bakın.

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Dijital Video Çoklu Reklam Çalma Listesi (VMAP) Dosyakullanma
Bir VMAP dosyası, reklam sonlarının ne zaman oluştuğunu, her molanın ne kadar süreceğini, mola sırasında kaç reklamın görüntülenebileceğini ve mola sırasında ne tür reklamların görüntülenebileceğini belirtmenize olanak tanır. Tek bir reklam sonu tanımlayan örnek bir VMAP dosyasında aşağıdakiler:

```xml
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[https://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scalable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
```

Bir VMAP dosyası, `<VMAP>` her biri bir `<AdBreak>` reklam sonu tanımlayan bir veya daha fazla öğe içeren bir öğeyle başlar. Her reklam sonu, bir kesme türü, kesme kimliği ve zaman mahsupu belirtir. breakType özniteliği, ara sırasında oynatılabilir reklam türünü belirtir: doğrusal, doğrusal olmayan veya görüntü. Reklamları VAST eşlik eden reklamlara gösterin. Virgül (boşluk yok) ayrılmış listede birden fazla reklam türü belirtilebilir. BreakID, reklam için isteğe bağlı bir tanımlayıcıdır. TimeOffset, reklamın ne zaman görüntüleneceğini belirtir. Aşağıdaki yollardan biriyle belirtilebilir:

1. Zaman – hh:mm:ss veya hh:mm:ss.mmm formatında .mmm milisaniyedir. Bu özniteliğin değeri, video zaman çizelgesinin başlangıcından reklam sonu başına kadar olan zamanı belirtir.
2. Yüzde – n% formatında nerede n reklam oynatmadan önce oynatmak için video zaman çizelgesi yüzdesi
3. Başlat/Bitiş – bir reklamın video görüntülenmeden önce veya sonra görüntülenmesi gerektiğini belirtir
4. Konum – reklam sonlarının zamanlaması bilinmediğinde (örneğin, canlı akışta olduğu gibi reklam sonlarının sırasını belirtir. Her reklam sonu sırası, n'nin tamsayı 1 veya daha büyük olduğu #n biçiminde belirtilir. 1 reklamın ilk fırsatta oynanması gerektiğini, 2 reklamın ikinci fırsatta oynanması gerektiğini ve benzeri anlamına dalıdır.

Öğe `<AdBreak>` içinde, bir <**AdSource**> öğesi olabilir. <**AdSource**> öğesi aşağıdaki öznitelikleri içerir:

1. Id – reklam kaynağı için bir tanımlayıcı belirtir
2. allowMultipleAds – reklam molası sırasında birden fazla reklamın görüntülenip görüntülenemeyeceğini belirten bir Boolean değeri
3. takipRedirects - video oynatıcı bir reklam yanıtı içinde yönlendirmeler onur eğer belirtir isteğe bağlı boolean değeri

<**AdSource**> öğesi oyuncuya bir satır içinde reklam yanıtı veya bir reklam yanıtına başvuru sağlar. Aşağıdaki öğelerden birini içerebilir:

* `<VASTAdData>`Bir VAST reklam yanıtının VMAP dosyasına gömülü olduğunu gösterir
* `<AdTagURI>`başka bir sistemden bir reklam yanıtı referansbir URI
* `<CustomAdData>`-engin olmayan bir yanıtı temsil eden rasgele bir dize

Bu örnekte, vast reklam yanıtı içeren `<VASTAdData>` bir öğeyle sıralı bir reklam yanıtı belirtilir. Diğer öğeler hakkında daha fazla bilgi için [VMAP'e](https://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap)bakın.

<**AdBreak**> öğesi, bir <**İzleme Olayları**> öğesi de içerebilir. <**İzleme Olayları**> öğesi, reklam molasının başlangıcını veya sonunu veya reklam arasırasında bir hata oluşup oluşmadığını izlemenize olanak tanır. <**İzleme Olayları**> öğesi, her biri bir izleme olayı ve izleme URI'si belirten bir veya daha fazla <**Izleme**> öğesi içerir. Olası izleme olayları şunlardır:

1. breakStart – reklam molasının başlangıcını izler
2. breakEnd – reklam molasının tamamlanmasını izleme
3. hata – reklam molası sırasında oluşan bir hatayı izler

Aşağıdaki örnekte, izleme olaylarını belirten bir VMAP dosyası gösterilmektedir

```xml
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
```

<**İzleme Etkinlikleri**> öğesi ve çocukları hakkında daha fazla bilgi için bkz.http://iab.net/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Ortam Soyut Sıralama Şablonu (MAST) Dosyakullanma
MAST dosyası, bir reklamın görüntülendiğinde belirleyen tetikleyicileri belirtmenize olanak tanır. Aşağıda, bir ön rulo reklam, bir orta rulo reklam ve rulo sonrası reklam için tetikleyiciler içeren örnek bir MAST dosyası verilmiştir.

```xml
    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>
```


MAST dosyası, bir **tetikleyici** öğesi içeren bir **MAST** öğesi yle başlar. Öğe, `<triggers>` bir reklamın ne zaman oynatılması gerektiğini tanımlayan bir veya daha fazla **tetikleyici** öğe içerir.

**Tetikleyici** öğesi, bir reklamın ne zaman oynatılmaya başlayacağını belirten bir **başlangıç Koşulları** öğesi içerir. **Başlangıç Koşulları** öğesi bir `<condition>` veya daha fazla öğe içerir. Her `<condition>` biri doğru olarak değerlendirdiğinde, tetikleyicinin sırasıyla bir `<condition>` **başlangıç Koşulları** veya **bitiş Koşulları** öğesi içinde bulunup bulunmayacağına bağlı olarak başlatılır veya iptal edilir. Birden `<condition>` çok öğe bulunduğunda, bunlar örtülü veya olarak kabul edilir, doğru değerlendiren herhangi bir koşul tetikleyici başlatmak için neden olacaktır. `<condition>`elemanları iç içe olabilir. Alt `<condition>` öğeler önceden ayarlandığında, bunlar örtülü olarak kabul edilir VE, tetikleyici başlatmak için tüm koşulların doğru değerlendirmek gerekir. Öğe, `<condition>` koşulu tanımlayan aşağıdaki öznitelikleri içerir:

1. **türü** – koşul, olay veya özellik türünü belirtir
2. **adı** – değerlendirme sırasında kullanılacak özellik veya olayın adı
3. **değeri** – bir mülkün karşısına çıkacak değer
4. **operatör** – değerlendirme sırasında kullanılacak işlem: EQ (eşit), NEQ (eşit değil), GTR (büyük), GEQ (büyük veya eşit), LT (Az), LEQ (az veya eşit), MOD (modulo)

**endConditions** da `<condition>` öğeleri içerir. Bir koşul doğru olarak değerlendirildiğinde tetikleyici sıfırlanır. Öğe, `<trigger>` bir `<sources>` veya daha fazla `<source>` öğe içeren bir öğe de içerir. Öğeler, `<source>` reklam yanıtına uri'yi ve reklam yanıtı türünü tanımlar. Bu örnekte, bir URI VAST yanıtı verilir.

```xml
    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
```

### <a name="using-video-player-ad-interface-definition-vpaid"></a>Video Oynatıcı-Reklam Arabirim Tanımını Kullanma (VPAID)
VPAID, çalıştırılabilir reklam birimlerinin bir video oynatıcıyla iletişim kurmasını sağlayan bir API'dir. Bu, son derece etkileşimli reklam deneyimleri sağlar. Kullanıcı reklamla etkileşimkurabilir ve reklam görüntüleyici tarafından gerçekleştirilen eylemlere yanıt verebilir. Örneğin, bir reklam, kullanıcının daha fazla bilgi veya reklamın daha uzun bir sürümünü görüntülemesine olanak tanıyan düğmeleri görüntüleyebilir. Video oynatıcı VPAID API'yi desteklemeli ve çalıştırılabilir reklam API'yi uygulamalıdır. Bir oyuncu bir reklam sunucusundan reklam istediğinde sunucu, VPAID reklamı içeren bir VAST yanıtıyla yanıt verebilir.

Yürütülebilir bir reklam, Adobe Flash™ veya JavaScript gibi bir web tarayıcısında yürütülebilen bir çalışma zamanı ortamında yürütülmesi gereken kodda oluşturulur. Bir reklam sunucusu VPAID reklamı içeren bir VAST yanıtı verdiğinde, `<MediaFile>` öğedeki apiFramework özniteliğinin değeri "VPAID" olmalıdır. Bu özellik, içerdiği reklamın VPAID yürütülebilir bir reklam olduğunu belirtir. Tür özniteliği, "application/x-shockwave-flash" veya "application/x-javascript" gibi çalıştırılabilir mime türüne ayarlanmalıdır. Aşağıdaki XML snippet, `<MediaFile>` VPAID çalıştırılabilir reklam içeren VAST yanıtındaki öğeyi gösterir.

```xml
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type="application/x-shockwaveflash"
                  width="640" height="480" apiFramework="VPAID">
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
```

Yürütülebilir bir reklam, VAST `<AdParameters>` yanıtındaki `<Linear>` `<NonLinear>` öğe veya öğeler kullanılarak başolarak lanse edilebilir. `<AdParameters>` Öğe hakkında daha fazla bilgi için [VAST 3.0'a](https://www.iab.net/media/file/VASTv3.0.pdf)bakın. VPAID API hakkında daha fazla bilgi için [VPAID 2.0'a](https://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf)bakın.

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Reklam Destekli Windows veya Windows Phone 8 Oynatıcı Uygulama
Microsoft Media Platform: Windows 8 için Player Framework ve Windows Phone 8, çerçeveyi kullanarak bir video oynatıcı uygulamasını nasıl uygulayacağınızı gösteren örnek uygulamalar koleksiyonu içerir. Player Framework'ü ve örnekleri [Windows 8 ve Windows Phone 8 için Player Framework'den](https://playerframework.codeplex.com)indirebilirsiniz.

Microsoft.PlayerFramework.Xaml.Samples çözümünü açtığınızda, proje içinde bir dizi klasör görürsünüz. Reklam klasörü, reklam destekli bir video oynatıcı oluşturmakla ilgili örnek kodu içerir. Reklam klasörünün içinde, her biri reklamların nasıl farklı bir şekilde ekyapılacağını gösteren bir dizi XAML/cs dosyası bulunur. Aşağıdaki liste her açıklar:

* AdPodPage.xaml Bir reklam bölmesini nasıl görüntülenin.
* AdSchedulingPage.xaml Reklamların nasıl zamanlandığını gösterir.
* FreeWheelPage.xaml Reklamları zamanlamak için FreeWheel eklentisinin nasıl kullanılacağını gösterir.
* MastPage.xaml MAST dosyasıyla reklamların nasıl zamanlabildiğini gösterir.
* ProgrammaticAdPage.xaml Reklamların videoya nasıl programyapılacağını gösterir.
* ScheduleClipPage.xaml VAST dosyası olmadan bir reklamın nasıl zamanlabildiğini gösterir.
* VastLinearCompanionPage.xaml doğrusal ve yardımcı bir reklamın nasıl ekyapılacağını gösterir.
* VastNonLinearPage.xaml Doğrusal olmayan bir reklamın nasıl ekyapılacağını gösterir.
* VmapPage.xaml VMAP dosyası olan reklamların nasıl belirtilir olduğunu gösterir.

Bu örneklerin her biri, oynatıcı çerçevesi tarafından tanımlanan MediaPlayer sınıfını kullanır. Örneklerin çoğu, çeşitli reklam yanıt biçimleri için destek ekleyen eklentiler kullanır. ProgrammaticAdPage örneği bir MediaPlayer örneğiyle programlanabilir şekilde etkileşime girer.

### <a name="adpodpage-sample"></a>AdPodPage Örneği
Bu örnek, bir reklamın ne zaman görüntülenece görüntülenecek olduğunu tanımlamak için AdSchedulerPlugin'i kullanır. Bu örnekte, bir orta rulo reklam beş saniye sonra çalınması planlanıyor. Reklam bölmesi (sırayla görüntülenecek bir reklam grubu) bir reklam sunucusundan döndürülen VAST dosyasında belirtilir. ENGIN dosyasına URI `<RemoteAdSource>` öğesinde belirtilir.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>
```

AdSchedulerPlugin hakkında daha fazla bilgi için [Windows 8 ve Windows Phone 8'de Player Framework'de Reklamcılık](https://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Bu örnek aynı zamanda AdSchedulerPlugin kullanır. Üç reklam, bir ön rulo reklam, bir rulo reklam ve bir post-roll reklamı planlar. Her reklam için VAST URI bir `<RemoteAdSource>` öğe olarak belirtilir.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="freewheelpage"></a>FreeWheelPage
Bu örnek, reklam içeriğinin yanı sıra reklam zamanlama bilgilerini belirten bir SmartXML dosyasına işaret eden bir URI belirten bir Kaynak özniteliği belirten FreeWheelPlugin'i kullanır.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="mastpage"></a>MastPage
Bu örnek, mast dosyasını kullanmanıza olanak tanıyan MastSchedulerPlugin'i kullanır. Kaynak özniteliği, MAST dosyasının konumunu belirtir.
```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="programmaticadpage"></a>ProgramlıAdPage
Bu örnek MediaPlayer ile programlı olarak etkileşime girebeder. ProgrammaticAdPage.xaml dosyası MediaPlayer instantiates:

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>
```

ProgrammaticAdPage.xaml.cs dosyası bir AdHandlerPlugin oluşturur, bir reklamın ne zaman görüntüleneceğini belirtmek için bir Zaman Çizelgesi Işaretçisi ekler ve ardından Bir REMOTEAdSource'u VAST dosyasına URI belirten bir RemoteAdSource yükleyen ve sonra reklamı çalan MarkerReached olayı için bir işleyici ekler.

```csharp
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }
```

### <a name="scheduleclippage"></a>ProgramClipPage
Bu örnek, reklamı içeren bir .wmv dosyası belirterek bir ara reklam zamanlamak için AdSchedulerPlugin'i kullanır.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
Bu örnek, bitişik bir reklamla orta rulo doğrusal bir reklam zamanlamak için AdSchedulerPlugin'in nasıl kullanılacağını göstermektedir. Öğe `<RemoteAdSource>` VAST dosyasının konumunu belirtir.

```xml
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
Bu örnek, doğrusal ve doğrusal olmayan bir reklam zamanlamak için AdSchedulerPlugin'i kullanır. VAST dosya konumu `<RemoteAdSource>` öğeyle birlikte belirtilir.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="vmappage"></a>VMAPPage
Bu örnek, vmap dosyasını kullanarak reklam zamanlamak için VmapSchedulerPlugin'i kullanır. VMAP dosyasına URI `<VmapSchedulerPlugin>` öğenin Kaynak özniteliği belirtilir.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Reklam Destekli iOS Video Oynatıcı Uygulama
Microsoft Medya Platformu: iOS için Player Framework, çerçeveyi kullanarak bir video oynatıcı uygulamasını nasıl uygulayacağınızı gösteren örnek uygulamalar koleksiyonu içerir. Player Framework'ü ve örnekleri [Azure Media Player Framework'den](https://github.com/Azure/azure-media-player-framework)indirebilirsiniz. GitHub sayfasında, oyuncu çerçevesi hakkında ek bilgiler ve oyuncu örneğine giriş içeren bir Wiki bağlantısı vardır: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>VMAP ile Reklam Planlama
Aşağıdaki örnekte, bir VMAP dosyasını kullanarak reklamların nasıl zamanlanın gösterilmektedir.

```csharp
    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }
            }
```

### <a name="scheduling-ads-with-vast"></a>VAST ile Reklam Planlama
Aşağıdaki örnek, geç ciltlenmiş bir VAST reklamının nasıl zamanlabildiğini gösterir.


```csharp
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

   Aşağıdaki örnek, erken ciltlenmiş bir VAST reklamının nasıl zamanlabildiğini gösterir.

```csharp
    //Example:4 Schedule an early binding VAST ad
    //Download the VAST file
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]])
    {
        [self logFrameworkError];
    }
    else
    {
        adLinearTime.startTime = 7;
        adLinearTime.duration = 0;

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }
```

Aşağıdaki örnek, Kaba Kesme Düzenleme (RCE) kullanarak bir reklamın nasıl eklenir olduğunu gösterir

```csharp
    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }
```

Aşağıdaki örnekte, bir reklam bölmesinin nasıl zamanlanın gösterilmektedir.

```csharp
    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Aşağıdaki örnek, yapışkan olmayan bir orta rulo reklamın nasıl zamanlabildiğini gösterir. Yapışkan olmayan bir reklam, görüntüleyenin herhangi bir arayışına bakılmaksızın yalnızca bir kez oynatılır.

```csharp
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Aşağıdaki örnek, yapışkan bir orta rulo reklamın nasıl zamanlabildiğini gösterir. Video zaman çizelgesinde belirtilen noktaya her ulaşıldığında yapışkan bir reklam görüntülenir.

```csharp
    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Aşağıdaki örnekte, rulo sonrası reklamın nasıl zamanlanın gösterilmektedir.

```csharp
    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Aşağıdaki örnekte, bir ön rulo reklamın nasıl zamanlanın gösterilmektedir.

```csharp
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```

Aşağıdaki örnekte, bir orta rulo yer kaplama sıyrık reklamının nasıl zamanlanın gösterilmektedir.

```csharp
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
