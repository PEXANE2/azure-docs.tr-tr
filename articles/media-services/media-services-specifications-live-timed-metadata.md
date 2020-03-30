---
title: Azure Medya Hizmetleri - Canlı Akışta Zamanlanmış Meta Verilerin Sinyalizasyonu
description: Bu belirtim, Azure Medya Hizmetleri'ni sindirirken ve akışta zamanlanmış meta verileri sinyalleme yöntemlerini özetler. Buna genel zamanlı meta veri sinyalleri (ID3) desteğinin yanı sıra reklam ekleme ve splice durum sinyali için SCTE-35 sinyali de dahildir.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2019
ms.author: johndeu
ms.openlocfilehash: 551fb0cb9f3745a62d5d84f2c4878bbbbe5ad9a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137331"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Canlı Akışta Zamanlanmış Meta Verilerin Sinyalizasyonu 

Son Güncelleme: 2019-08-22

### <a name="conformance-notation"></a>Uygunluk Gösterimi

Bu belgede yer alan "MUST", "MUST", "MUST NOT", "MUST", "SHOULD NOT", "SHOULD", "SHOULD", "RECOMMENDED", "MAY" ve "OPTIONAL" anahtar kelimeleri 2119 sayılı Khk'da açıklandığı şekilde yorumlanmalıdır.

## <a name="1-introduction"></a>1. Giriş 

Bir istemci oynatıcıya reklam veya özel meta veri olayları ekleme sinyali vermek için, yayıncılar genellikle videonun içine gömülü zamanlanmış meta verileri kullanırlar. Bu senaryoları etkinleştirmek için, Medya Hizmetleri, zamanlanmış meta verilerin canlı akış kanalının en yutma noktasından istemci uygulamasına taşınması için destek sağlar.
Bu belirtim, canlı akış sinyalleri içindeki zamanlanmış meta veriler için Medya Hizmetleri tarafından desteklenen çeşitli modları özetler.

1. [SCTE-35] [SCTE-35], [SCTE-214-1], [SCTE-214-3] ve [RFC8216] tarafından belirtilen standartlara uygun sinyal

2. [SCTE-35] RTMP reklam sinyali için eski [Adobe-Primetime] belirtimine uygun sinyal.
   
3. [SCTE-35] **Değİl** VE [ID3v2] veya uygulama geliştiricisi tarafından tanımlanan diğer özel şemaları taşıyabilen iletiler için genel zamanlı meta veri sinyalmodu.

## <a name="11-terms-used"></a>1.1 Kullanılan Terimler

| Sözleşme Dönemi                | Tanım                                                                                                                                                                                                                                    |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Reklam Sonu            | Bir veya daha fazla reklamın yayıniçin zamanlanabileceği bir konum veya nokta; boşuna ve yerleştirme fırsatı olarak aynıdır.                                                                                                                     |
| Reklam Karar Servisi | kullanıcıya hangi reklam(lar) ve sürelerin gösterilene karar veren harici hizmet. Hizmetler genellikle bir ortak tarafından sağlanır ve bu belgenin kapsamı dışındadır.                                                                    |
| Işaret                 | Yaklaşan reklam molasının zaman ve parametrelerinin göstergesi. İpuçlarının bekleyen bir reklam molası, reklam molası içinde bir sonraki reklama geçiş ve reklam molasından ana içeriğe geçiş beklemeyi gösterebileceğini unutmayın.           |
| Paketleyici            | Azure Medya Hizmetleri "Streaming Endpoint", DASH ve HLS için dinamik paketleme özellikleri sağlar ve medya endüstrisinde "Paketleyici" olarak adlandırılır.                                                                              |
| Sunum Süresi   | Bir olayın görüntüleyiciye sunulduğu saat. Zaman, medya zaman çizelgesinde izleyicinin olayı göreceği anı temsil eder. Örneğin, bir SCTE-35 splice_info() komut iletisinin sunu zamanı splice_time(). |
| Varış Saati        | Olay iletisinin geldiği saat. Olay iletileri olayın sunu saatinden önce gönderildiğinden, zaman genellikle olayın sunu zamanından farklıdır.                                                    |
| Seyrek parça        | sürekli olmayan ve bir üst veya denetim parçasıyla eşitlenen ortam izleme.                                                                                                                                                  |
| Kaynak              | Azure Medya Akış Hizmeti                                                                                                                                                                                                             |
| Kanal Lavabo        | Azure Media Canlı Akış Hizmeti                                                                                                                                                                                                        |
| HLS                 | Apple HTTP Canlı Akış protokolü                                                                                                                                                                                                            |
| Demir                | HTTP Üzerinden Dinamik Adaptif Akış                                                                                                                                                                                                          |
| Pürüzsüz              | Düzgün Akış Protokolü                                                                                                                                                                                                                     |
| MPEG2-TS            | MPEG 2 Taşıma Akışları                                                                                                                                                                                                                      |
| RTMP                | Gerçek Zamanlı Multimedya Protokolü                                                                                                                                                                                                                 |
| uimsbf              | İmzasız birsayı, en önemli bit ilk.                                                                                                                                                                                                 |

---

## <a name="12-normative-references"></a>1.2 Normatif Referanslar

Aşağıdaki belgeler, bu metinde referans yoluyla, bu belgenin hükümlerini oluşturan hükümleri içerir. Tüm belgeler standartlar kuruluşları tarafından gözden geçirilir ve okuyucuların aşağıda listelenen belgelerin en son sürümlerini uygulama olasılığını araştırmaları önerilir. Okuyuculara ayrıca, başvurulan belgelerin yeni sürümlerinin Azure Medya Hizmetleri için zamanlanmış meta veri belirtiminin bu sürümüyle uyumlu olmayabileceği de hatırlatılır.


| Standart          | Tanım                                                                                                                                                                                                     |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Adobe-Primetime] | [Primetime Dijital Program Ekleme Sinyal Özellikleri 1.2](https://www.adobe.com/content/dam/acom/en/devnet/primetime/PrimetimeDigitalProgramInsertionSignalingSpecification.pdf)                       |
| [Adobe-Flash-AS]  | [FLASH ActionScript Dil Referansı](https://help.adobe.com/archive/en_US/as2/flashlite_2.x_3.x_aslr.pdf)                                                                                                   |
| [AMF0]            | ["Eylem İleti formatı AMF0"](https://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)                                                                                                              |
| [DASH-IF-IOP]     | DASH Sanayi Forumu Interop Rehberlik v 4.2[https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html)    |
| [HLS-TMD]         | HTTP Canlı Akış için Zamanlı Meta veri -[https://developer.apple.com/streaming](https://developer.apple.com/streaming)                                                                                        |
| [CMAF-ID3]        | [Ortak Medya Uygulama Formatında Zamanlı Meta Veriler (CMAF)](https://github.com/AOMediaCodec/id3-emsg)                                                                                                        |
| [ID3v2]           | ID3 Etiket sürüm 2.4.0[http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure)                                                                                                                |
| [ISO-14496-12]    | ISO/IEC 14496-12: Bölüm 12 ISO temel medya dosya formatı, FourthEdition 2012-07-15                                                                                                                                 |
| [MPEGDASH]        | Bilgi teknolojisi -- HTTP (DASH) üzerinden dinamik uyarlanabilir akış -- Bölüm 1: Medya sunum açıklaması ve segment biçimleri. Mayıs 2014. Yayınlanan. Url:https://www.iso.org/standard/65274.html         |
| [MPEGCMAF]        | Bilgi teknolojisi -- Multimedya uygulama biçimi (MPEG-A) -- Bölüm 19: Parçalı ortam için ortak ortam uygulama biçimi (CMAF). Ocak 2018. Yayınlanan. Url:https://www.iso.org/standard/71975.html |
| [MPEGCENC]        | Bilgi teknolojisi -- MPEG sistem teknolojileri -- Bölüm 7: ISO temel medya dosya formatında ortak şifreleme. Şubat 2016. Yayınlanan. Url:https://www.iso.org/standard/68042.html                   |
| [MS-SSTR]         | ["Microsoft Smooth Streaming Protocol", May 15, 2014](https://docs.microsoft.com/openspecs/windows_protocols/ms-sstr/8383f27f-7efe-4c60-832a-387274457251)                                                     |
| [MS-SSTR-Yutma]  | [Azure Medya Hizmetleri Parçalı MP4 Canlı Yutma Belirtimi](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview)                                                      |
| [RFC8216]         | R. Pantos, Ed.; W. May. HTTP Canlı Yayın. Ağustos 2017. Bilgi. [https://tools.ietf.org/html/rfc8216](https://tools.ietf.org/html/rfc8216)                                                            |
| [RFC4648]         | Base16, Base32 ve Base64 Veri Kodlamaları -[https://tools.ietf.org/html/rfc4648](https://tools.ietf.org/html/rfc4648)                                                                                     |
| [RTMP]            | ["Adobe'nin Gerçek Zamanlı Mesajlaşma Protokolü", 21 Aralık 2012](https://www.adobe.com/devnet/rtmp.html)                                                                                                            |
| [SCTE-35-2019]    | SCTE 35: 2019 - Kablo için Dijital Program Ekleme İşaretleme Mesajı -https://www.scte.org/SCTEDocs/Standards/ANSI_SCTE%2035%202019r1.pdf                                                                       |
| [SCTE-214-1]      | SCTE 214-1 2016 – IP Tabanlı Kablo Hizmetleri Bölüm 1 için MPEG DASH Bölüm 1: MPD Kısıtlamaları ve Uzantıları                                                                                                                 |
| [SCTE-214-3]      | IP Tabanlı Kablo Hizmetleri Için SCTE 214-3 2015 MPEG DASH Bölüm 3: DASH/FF Profili                                                                                                                                  |
| [SCTE-224]        | SCTE 224 2018r1 – Olay Zamanlama ve Bildirim Arayüzü                                                                                                                                                  |
| [SCTE-250]        | Olay ve Sinyal Yönetimi API (ESAM)                                                                                                                                                                      |

---


## <a name="2-timed-metadata-ingest"></a>2. Zamanlı Meta veri alma

Azure Medya Hizmetleri, hem [RTMP] hem de Düzgün Akış [MS-SSTR-Ingest] protokolleri için gerçek zamanlı bant içi meta verileri destekler. Gerçek zamanlı meta veriler, kendi benzersiz özel şema (JSON, İkili, XML) yanı sıra ID3 veya SCTE-35 gibi endüstri tanımlı biçimleri bir yayın akışında reklam sinyali için özel olayları tanımlamak için kullanılabilir. 

Bu makalede, Azure Medya Hizmetleri'nin desteklenen yutma protokollerini kullanarak özel zamanlanmış meta veri sinyallerinin nasıl gönderilene ilişkin ayrıntılar verilmektedir. Makalede ayrıca HLS, DASH ve Smooth Streaming için manifestoların zamanlanmış meta veri sinyalleriyle nasıl dekore edildiği ve içeriğin HLS için CMAF (MP4 parçaları) veya Transport Stream (TS) segmentleri kullanılarak teslim edildiğinde bant içinde nasıl taşındığı açıklanmaktadır. 

Zamanlanmış meta veriler için yaygın kullanım durum senaryoları şunlardır:

 - Canlı bir etkinlikte veya doğrusal yayında reklam molalarını tetiklemek için SCTE-35 reklam sinyalleri
 - İstemci uygulamasındaki olayları tetikleyebilen özel ID3 meta verileri (tarayıcı, iOS veya Android)
 - İstemci uygulamasındaki olayları tetiklemek için özel tanımlı JSON, İkili veya XML meta verileri
 - Canlı kodlayıcı, IP Kamera veya Drone'dan telemetri
 - Hareket, yüz algılama, vb. gibi bir IP Kameradan gelen olaylar
 - Eylem kamerası, drone veya hareketli cihazdan gelen coğrafi konum bilgileri
 - Şarkı sözleri
 - Doğrusal canlı yayında program sınırları
 - Canlı yayında görüntülenecek görüntüler veya artırılmış meta veriler
 - Spor skorları veya oyun saati bilgileri
 - Tarayıcıda videonun yanında görüntülenecek etkileşimli reklam paketleri
 - Sınavlar veya anketler
  
Azure Medya Hizmetleri Canlı Etkinlikler ve Packager, bu zamanlanmış meta veri sinyallerini alabilir ve HLS ve DASH gibi standartlara dayalı protokolleri kullanarak istemci uygulamalarına ulaşabilen bir meta veri akışına dönüştürebilir.


## <a name="21-rtmp-timed-metadata"></a>2.1 RTMP Zamanlı Meta verileri

[RTMP] protokolü, özel meta veriler ve SCTE-35 reklam sinyalleri de dahil olmak üzere çeşitli senaryolar için zamanlanmış meta veri sinyallerinin gönderilmesine olanak tanır. 

Reklam sinyalleri (işaret iletileri) [RTMP] akışına gömülü [AMF0] işaret iletileri olarak gönderilir. İşaret iletileri, gerçek olaydan önce veya [SCTE35] reklam splice sinyalinin gerçekleşmesi gerektiğinden bir süre önce gönderilebilir. Bu senaryoyu desteklemek için, olayın gerçek sunu zaman damgası işaret iletisi içinde gönderilir. Daha fazla bilgi için [AMF0] bakın.

Aşağıdaki [AMF0] komutları AZURE Media Services tarafından RTMP'yi yutmak için desteklenir:

- **onUserDataEvent** - özel meta veriler veya [ID3v2] zamanlı meta veriler için kullanılır
- **onAdCue** - öncelikle canlı yayında bir reklam yerleştirme fırsatı sinyal için kullanılır. İşaretin iki biçimi, basit bir mod ve "SCTE-35" modu desteklenir. 
- **onCuePoint** - [SCTE35] iletileri sinyal elemental Live kodlayıcı gibi bazı şirket içi donanım kodlayıcılar tarafından desteklenir. 
  

Aşağıdaki tabloda, Medya Hizmetleri'nin hem "basit" hem de [SCTE35] ileti modları için yutacağı AMF ileti yükü biçimi açıklanmaktadır.

[AMF0] iletisinin adı, aynı türden birden çok olay akışını ayırt etmek için kullanılabilir.  Hem [SCTE-35] iletileri hem de "basit" modiçin, AMF iletisinin adı [Adobe-Primetime] belirtiminde gerektiği gibi "onAdCue" olmalıdır.  Aşağıda listelenmemiş tüm alanlar, azure medya hizmetleri tarafından göz ardı edilir.

## <a name="211-rtmp-with-custom-metadata-using-onuserdataevent"></a>2.1.1 RTMP özel meta veri kullanarak "onUserDataEvent"

RTMP protokolünü kullanarak upstream kodlayıcınızdan, IP Kameranızdan, Drone'unuzdan veya aygıtınızdan özel meta veri akışları sağlamak istiyorsanız, "onUserDataEvent" [AMF0] veri iletikomutu türünü kullanın.

**"onUserDataEvent"** veri iletikomutu, Media Services tarafından yakalanacak ve bant içi dosya biçiminin yanı sıra HLS, DASH ve Smooth Streaming bildirimlerine paketlenecek aşağıdaki tanıma sahip bir ileti yükü taşımalıdır.
Zamanlanmış meta veri iletilerinin her 0,5 saniyede bir (500ms) birden fazla sıklıkta gönderilmesi veya canlı akışla ilgili kararlılık sorunlarının oluşmaması önerilir. Çerçeve düzeyinde meta veri sağlamanız gerekiyorsa, her ileti birden çok çerçeveden meta veri toplayabilir. Çok bit hızında akışlar gönderiyorsanız, meta verileri yalnızca bant genişliğini azaltmak ve video/ses işleme ile etkileşimi önlemek için tek bir bit hızında sağlamanız önerilir. 

**"onUserDataEvent"** için yük bir [MPEGDASH] EventStream XML biçimli ileti olmalıdır. Bu, HLS veya DASH protokolleri üzerinden teslim edilen CMAF [MPEGCMAF] içeriği için bant içi 'emsg' yüklerinde taşınabilen özel tanımlı şemalarda geçişi kolaylaştırır. Her DASH Olay Akışı iletisi, URN ileti düzeni tanımlayıcısı olarak işlev gösteren ve iletinin yükünü tanımlayan bir şemaIdUri içerir. [ID3v2], for [ID3v2]veyahttps://aomedia.org/emsg/ID3 **urn:scte:scte35:2013:bin** for [SCTE-35] gibi bazı şemalar birlikte çalışabilirlik için endüstri konsorsiyumları tarafından standartlaştırılmıştır. Herhangi bir uygulama sağlayıcısı, denetledikleri bir URL'yi (sahip oldukları etki alanı) kullanarak kendi özel düzenini tanımlayabilir ve istedikleri takdirde bu URL'de bir belirtim sağlayabilir. Bir oyuncunun tanımlanan düzen için bir işleyicisi varsa, yükü ve protokolü anlaması gereken tek bileşen bu olur.

[MPEG-DASH] EventStream XML yükü için şema olarak tanımlanır (DASH ISO-IEC-23009-1-3rd Edition alıntı). Şu anda "EventStream" başına yalnızca bir "EventType" desteklenir unutmayın. **Olay Akışı'nda**birden çok olay sağlanırsa yalnızca ilk **Olay** öğesi işlenir.

```xml
  <!-- Event Stream -->
  <xs:complexType name="EventStreamType">
    <xs:sequence>
      <xs:element name="Event" type="EventType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute ref="xlink:href"/>
    <xs:attribute ref="xlink:actuate" default="onRequest"/>
    <xs:attribute name="schemeIdUri" type="xs:anyURI" use="required"/>
    <xs:attribute name="value" type="xs:string"/>
    <xs:attribute name="timescale" type="xs:unsignedInt"/>
  </xs:complexType>
  <!-- Event  -->
  <xs:complexType name="EventType">
    <xs:sequence>
      <xs:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="presentationTime" type="xs:unsignedLong" default="0"/>
    <xs:attribute name="duration" type="xs:unsignedLong"/>
    <xs:attribute name="id" type="xs:unsignedInt"/>
    <xs:attribute name="contentEncoding" type="ContentEncodingType"/>
    <xs:attribute name="messageData" type="xs:string"/>
    <xs:anyAttribute namespace="##other" processContents="lax"/>
  </xs:complexType>
```


### <a name="example-xml-event-stream-with-id3-schema-id-and-base64-encoded-data-payload"></a>ÖRNEK XML Olay Akışı, ID3 şema kimliği ve base64 kodlanmış veri yükü ile.  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="https://aomedia.org/emsg/ID3">
         <Event contentEncoding="Base64">
          -- base64 encoded ID3v2 full payload here per [CMAF-TMD] --
         </Event>
   <EventStream>
```

### <a name="example-event-stream-with-custom-schema-id-and-base64-encoded-binary-data"></a>Özel şema kimliği ve base64 kodlanmış ikili veri ile Örnek Olay Akışı  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="urn:example.org:custom:binary">
         <Event contentEncoding="Base64">
          -- base64 encoded custom binary data message --
         </Event>
   <EventStream>
```

### <a name="example-event-stream-with-custom-schema-id-and-custom-json"></a>Özel şema kimliği ve özel JSON ile Örnek Olay Akışı  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="urn:example.org:custom:JSON">
         <Event>
          [
            {"key1" : "value1"},
            {"key2" : "value2"}
          ]
         </Event>
   <EventStream>
```

### <a name="built-in-supported-scheme-id-uris"></a>Dahili destekli Şema Kimliği URL'leri
| Şema Kimliği URI                 | Açıklama                                                                                                                                                                                                                                          |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| https:\//aomedia.org/emsg/ID3 | [ID3v2] meta verilerinin CMAF uyumlu [MPEGCMAF] parçalanmış MP4'te zamanlanmış meta veri olarak nasıl taşınabileceğini açıklar. Daha fazla bilgi için [Ortak Medya Uygulama Formatındaki (CMAF) Zamanlanmış Meta verilerine](https://github.com/AOMediaCodec/id3-emsg) bakın |

### <a name="event-processing-and-manifest-signaling"></a>Olay işleme ve bildirim sinyali

Geçerli bir **"onUserDataEvent"** olayı nın alınması üzerine, Azure Medya Hizmetleri EventStreamType ([MPEGDASH'te tanımlanan) eşleşen geçerli bir XML yükü arayacak, XML yükünü ayrıştırAcak ve canlı arşivde depolamak ve Medya Hizmetleri Paketleyicisine aktarmak için [MPEGCMAF] MP4 parçası 'emsg' sürüm 1 kutusuna dönüştürecektir.   Packager canlı akıştaki 'emsg' kutusunu algılar ve:

- (a) HLS zamanlı meta veri belirtimi [HLS-TMD] uyarınca HLS müşterilerine teslim için TS segmentlerine "dinamik olarak paket" veya
- (b) CMAF parçaları içinde HLS veya DASH üzerinden teslim için geçmek veya 
- (c) Smooth Streaming [MS-SSTR] üzerinden teslimat için seyrek bir parça sinyaline dönüştürün.

HLS için grup içi 'emsg' formatına CMAF veya TS PES paketlerine ek olarak, DASH (MPD) ve Smooth Streaming için manifestolar, bant içi olay akışlarına (Smooth Streaming'de seyrek akış parçası olarak da bilinir) bir başvuru içerir. 

Tek tek olaylar veya bunların veri yükleri doğrudan HLS, DASH veya Smooth manifestolarında çıktı DeğİlDIR. 

### <a name="additional-informational-constraints-and-defaults-for-onuserdataevent-events"></a>onUserDataEvent etkinlikleri için ek bilgilendirme kısıtlamaları ve varsayılanları

- Zaman ölçeği EventStream öğesinde ayarlanmazsa, RTMP 1 kHz zaman ölçeği varsayılan olarak
- OnUserDataEvent iletisinin teslimi her 500ms max'te bir kez ile sınırlıdır. Olayları daha sık gönderirseniz, bant genişliğini ve canlı yayının kararlılığını etkileyebilir

## <a name="212-rtmp-ad-cue-signaling-with-onadcue"></a>2.1.2 RTMP reklam ipucu "onAdCue" ile sinyal

Azure Medya Hizmetleri, canlı akıştaki çeşitli gerçek zamanlı senkronize meta verileri işaretlemek için kullanılabilecek birkaç [AMF0] ileti türünü dinleyebilir ve yanıtlayabilir.  [Adobe-Primetime] belirtimi "basit" ve "SCTE-35" modu olarak adlandırılan iki işaret türünü tanımlar. "Basit" mod için, Medya Hizmetleri "Basit Mod" sinyali için tanımlanan aşağıdaki tabloyla eşleşen bir yük kullanarak "onAdCue" adı verilen tek bir AMF işaret mesajını destekler.  

Aşağıdaki bölümde, HLS, DASH ve Microsoft Smooth Streaming için istemci manifestosuna taşınacak temel bir "spliceOut" reklam sinyalini sinyallemek için kullanılabilecek RTMP "basit" modu" yükü gösterilmektedir. Bu, müşterinin karmaşık bir SCTE-35 tabanlı reklam sinyalizasyon dağıtım veya ekleme sistemi olmadığı ve bir API üzerinden işaret iletisi göndermek için temel bir şirket içi kodlayıcı kullandığı senaryolar için çok yararlıdır. Genellikle şirket içi kodlayıcı, videoya bir IDR çerçevesi ekleyerek ve yeni bir GOP başlatarak video akışını "koşullandırma" sağlayan bu sinyali tetiklemek için REST tabanlı bir API'yi destekler.

## <a name="213--rtmp-ad-cue-signaling-with-onadcue---simple-mode"></a>2.1.3 RTMP reklam ipucu "onAdCue" ile sinyal - Basit Mod

| Alan Adı | Alan Türü | Gerekli mi? | Açıklama                                                                                                                                                                                                                                                                        |
| ---------- | ---------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type       | Dize     | Gerekli  | Olay iletisi.  Basit bir mod splice belirlemek için "SpliceOut" olacaktır.                                                                                                                                                                                                         |
| id         | Dize     | Gerekli  | Birliş veya segmenti açıklayan benzersiz bir tanımlayıcı. İletinin bu örneğini tanımlar                                                                                                                                                                                       |
| süre   | Sayı     | Gerekli  | Birleşmenin süresi. Birimler kesirli saniyelerdir.                                                                                                                                                                                                                           |
| elapsed    | Sayı     | İsteğe bağlı  | Sinyali ayarlamak için tekrarlanırken, bu alan, birleşmenin başlamasından bu yana geçen sunum süresinin miktarı olacaktır. Birimler kesirli saniyelerdir. Basit mod kullanırken, bu değer birleştirmenin özgün süresini aşmamalıdır. |
| time       | Sayı     | Gerekli  | Sunum zamanı, splice zamanı olacaktır. Birimler kesirli saniyelerdir.                                                                                                                                                                                                |

---
 
#### <a name="example-mpeg-dash-manifest-output-when-using-adobe-rtmp-simple-mode"></a>Örnek MPEG DASH, Adobe RTMP basit modunu kullanırken çıktıyı gösterir

Örnek [3.3.2.1 MPEG DASH .mpd EventStream Adobe basit modunu kullanarak bakınız](#3321-example-mpeg-dash-mpd-manifest-signaling-of-rtmp-streaming-using-adobe-simple-mode)

Örnek [3.3.3.1 DASH manifestosuna tek periyotlu ve Adobe basit moduna](#3331-example-mpeg-dash-manifest-mpd-with-single-period-eventstream-using-adobe-simple-mode-signals) bakın

#### <a name="example-hls-manifest-output-when-using-adobe-rtmp-simple-mode"></a>Adobe RTMP basit modunu kullanırken örnek HLS bildirimi çıktısı

Adobe [basit modunu ve EXT-X-CUE etiketini kullanarak örnek 3.2.2 HLS manifestosuna](#322-apple-hls-with-adobe-primetime-ext-x-cue-legacy) bakın

## <a name="214-rtmp-ad-cue-signaling-with-onadcue---scte-35-mode"></a>2.1.4 RTMP reklam ipucu "onAdCue" ile sinyal - SCTE-35 Modu

HLS veya DASH manifestosuna taşınması gereken tam SCTE-35 yük iletisini gerektiren daha gelişmiş bir yayın üretim iş akışıyla çalışırken, [Adobe-Primetime] belirtiminin "SCTE-35 Modu"nu kullanmak en iyisidir.  Bu mod, bant içi SCTE-35 sinyallerinin doğrudan şirket içi canlı kodlayıcıya gönderilmesini destekler ve bu sinyaller [Adobe-Primetime] belirtiminde belirtilen "SCTE-35 Modu"nu kullanarak sinyalleri RTMP akışına kodlar. 

Genellikle SCTE-35 iletileri yalnızca şirket içi kodlayıcıda MPEG-2 aktarım akışı (TS) girişlerinde görünebilir. SCTE-35 içeren bir aktarım akışını nasıl yapılandırıştırılamalı ve Adobe SCTE-35 modunda RTMP'ye geçiş için nasıl etkinleştirin hakkında ayrıntılı bilgi almak için kodlayıcı üreticinize danışın.

Bu senaryoda, aşağıdaki yük **"onAdCue"** [AMF0] ileti türü kullanılarak şirket içi kodlayıcıdan gönderilmesi gerekir.

| Alan Adı | Alan Türü | Gerekli mi? | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                        |
| ---------- | ---------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Işaret        | Dize     | Gerekli  | Olay iletisi.  [SCTE-35] iletileri için, iletilerin HLS, Smooth ve Dash istemcilerine gönderilmesi için bu temel64 kodlanmış [RFC4648] ikili splice_info_section() olmalıdır.                                                                                                                                                                                                                               |
| type       | Dize     | Gerekli  | İleti düzenini tanımlayan bir URL veya URL. [SCTE-35] iletileri **için,** iletilerin [Adobe-Primetime] uyumlu olarak HLS, Smooth ve Dash istemcilerine gönderilmesi için **bu "scte35"** olmalıdır. İsteğe bağlı olarak, URN "urn:scte:scte35:2013:bin" de bir [SCTE-35] iletisinyali için kullanılabilir.                                                                                                        |
| id         | Dize     | Gerekli  | Birliş veya segmenti açıklayan benzersiz bir tanımlayıcı. İletinin bu örneğini tanımlar.  Eşdeğer semantik içeren iletiler aynı değere sahip olacaktır.                                                                                                                                                                                                                                                       |
| süre   | Sayı     | Gerekli  | Biliniyorsa, etkinliğin veya reklam bölümünün süresi. Bilinmiyorsa, değeri 0 **olmalıdır.**                                                                                                                                                                                                                                                                                                                    |
| elapsed    | Sayı     | İsteğe bağlı  | [SCTE-35] reklam sinyali ayarlayabilmek için tekrarlandığında, bu alan, splice'nin başlamasından bu yana geçen sunum süresi nin miktarı olacaktır. Birimler kesirli saniyelerdir. [SCTE-35] modunda, bu değer splice veya segmentin özgün belirtilen süresini aşabilir.                                                                                                                   |
| time       | Sayı     | Gerekli  | Etkinliğin veya reklam ın sunum zamanı.  Sunu süresi ve süresi, [ISO-14496-12] Ek I'de tanımlandığı şekilde, tip 1 veya 2'nin Akış Erişim Noktaları (SAP) ile aynı hizaya **girmelidir.** HLS çıkış için, zaman ve süre segment sınırları ile uyumlu **olmalıdır.** Aynı olay akışı içindeki farklı olay iletilerinin sunu süresi ve süresi çakışmamalıdır. Birimler kesirli saniyelerdir. |

---

#### <a name="example-mpeg-dash-mpd-manifest-with-scte-35-mode"></a>Örnek MPEG DASH .mpd SCTE-35 modu ile tezahür
[Bkz. Bölüm 3.3.3.2 örnek DASH manifestosu SCTE-35 ile](#3332-example-mpeg-dash-manifest-mpd-with-multi-period-eventstream-using-adobe-scte35-mode-signaling)

#### <a name="example-hls-manifest-m3u8-with-scte-35-mode-signal"></a>Örnek HLS scte-35 mod sinyali ile .m3u8 tezahürü
[Bölüm 3.2.1.1 örnek HLS manifestosuna Bakınız SCTE-35](#3211-example-hls-manifest-m3u8-showing-ext-x-daterange-signaling-of-scte-35)

## <a name="215-rtmp-ad-signaling-with-oncuepoint-for-elemental-live"></a>2.1.5 RTMP Reklam Elemental Live için "onCuePoint" ile sinyal

Elemental Live şirket içi kodlayıcı RTMP sinyalindeki reklam işaretçilerini destekler. Azure Medya Hizmetleri şu anda yalnızca RTMP için "onCuePoint" Reklam İşaretçisi türünü destekler.  Bu, Elemental Media Live kodlayıcı ayarlarındaki Adobe RTMP Grup Ayarları'nda veya API'de "**ad_markers**" ile "onCuePoint" olarak ayarlayarak etkinleştirilebilir.  Ayrıntılar için lütfen Elemental Live belgelerine bakın. RTMP Grubu'nda bu özelliği etkinleştirmek, Azure Media Services tarafından işlenecek Adobe RTMP çıktılarına SCTE-35 sinyallerini iletir.

"onCuePoint" ileti türü [Adobe-Flash-AS] tanımlanır ve Elemental Live RTMP çıktısından gönderildiğinde aşağıdaki yük yapısına sahiptir.


| Özellik   | Açıklama                                                                                                                                                                                                                     |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ad       | Elemental Live tarafından '**scte35**' adı olmalıdır.                                                                                                                                                                              |
| time       | Zaman çizelgesi sırasında video dosyasında işaret noktasının oluştuğu saniye cinsinden saat                                                                                                                                           |
| type       | İşaret noktasının türü "**event**" olarak ayarlanmalıdır.                                                                                                                                                                             |
| parametreler | SCTE-35 iletisinden kimlik ve süre dahil bilgileri içeren bir dizi ad/değer çifti dizeleri. Bu değerler Azure Medya Hizmetleri tarafından ayrıştırılır ve bildirim süsleme etiketine dahil edilir. |


Bu reklam işaretçisi modu kullanıldığında, HLS bildirim çıktısı Adobe "Simple" moduna benzer.


#### <a name="example-mpeg-dash-mpd-single-period-adobe-simple-mode-signals"></a>Örnek MPEG DASH MPD, tek dönem, Adobe Simple mod sinyalleri

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="dynamic" publishTime="2020-01-07T18:58:03Z" minimumUpdatePeriod="PT0S" timeShiftBufferDepth="PT58M56S" availabilityStartTime="2020-01-07T17:44:47Z" minBufferTime="PT7S">
    <Period start="PT0S">
        <EventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="scte35" timescale="10000000">
            <Event presentationTime="1583497601000000" duration="300000000" id="1085900"/>
            <Event presentationTime="1583500901666666" duration="300000000" id="1415966"/>
            <Event presentationTime="1583504202333333" duration="300000000" id="1746033"/>
            <Event presentationTime="1583507502666666" duration="300000000" id="2076066"/>
            <Event presentationTime="1583510803333333" duration="300000000" id="2406133"/>
            <Event presentationTime="1583514104000000" duration="300000000" id="2736200"/>
            <Event presentationTime="1583517404666666" duration="300000000" id="3066266"/>
            <Event presentationTime="1583520705333333" duration="300000000" id="3396333"/>
            <Event presentationTime="1583524006000000" duration="300000000" id="3726400"/>
            <Event presentationTime="1583527306666666" duration="300000000" id="4056466"/>
            <Event presentationTime="1583530607333333" duration="300000000" id="4386533"/>
        </EventStream>
        <AdaptationSet id="1" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.4D400C" maxWidth="256" maxHeight="144" startWithSAP="1">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="scte35"/>
            <SegmentTemplate timescale="10000000" presentationTimeOffset="1583486678426666" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="1583495318000000" d="64000000" r="34"/>
                    <S d="43000000"/>
                    <S d="21000000"/>
                    <!-- ... Truncated for brevity of sample-->

                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="1583495318000000" type="0" wallClockTime="2020-01-07T17:59:10.957Z" presentationTime="1583495318000000"/>
            <Representation id="1_V_video_3750956353252827751" bandwidth="149952" width="256" height="144"/>
        </AdaptationSet>
        <AdaptationSet id="2" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2" lang="en">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="scte35"/>
            <Label>ambient</Label>
            <SegmentTemplate timescale="10000000" presentationTimeOffset="1583486678426666" media="QualityLevels($Bandwidth$)/Fragments(ambient=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(ambient=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="1583495254426666" d="64000000" r="35"/>
                    <S d="43093334"/>
                    <S d="20906666"/>
                    <!-- ... Truncated for brevity of sample-->

                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="1583495254426666" type="0" wallClockTime="2020-01-07T17:59:04.600Z" presentationTime="1583495254426666"/>
            <Representation id="5_A_ambient_9125670592623055209" bandwidth="96000" audioSamplingRate="48000"/>
        </AdaptationSet>
    </Period>
</MPD>
~~~

#### <a name="example-hls-playlist-adobe-simple-mode-signals-using-ext-x-cue-tag-truncated--for-brevity"></a>Örnek HLS çalma listesi, ADOBE Simple modu EXT-X-CUE etiketini kullanarak sinyal (kesilmiş "..." kısalık için)

Aşağıdaki örnek, Adobe "basit" mod sinyallerini ve eski [Adobe-Primetime] EXT-X-CUE etiketini kullanarak RTMP yutma akışı için Medya Hizmetleri dinamik paketleyicisinden çıkan çıktıyı gösterir.  

~~~
#EXTM3U
#EXT-X-VERSION:8
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:7
#EXT-X-INDEPENDENT-SEGMENTS
#EXT-X-PROGRAM-DATE-TIME:2020-01-07T17:44:47Z
#EXTINF:6.400000,no-desc
Fragments(video=1583486742000000,format=m3u8-aapl-v8)
#EXTINF:6.400000,no-desc
Fragments(video=1583486806000000,format=m3u8-aapl-v8)
...
#EXTINF:6.166667,no-desc
Fragments(video=1583487638000000,format=m3u8-aapl-v8)
#EXT-X-CUE:ID=95766,TYPE="SpliceOut",DURATION=30.000000,TIME=158348769.966667
#EXTINF:0.233333,no-desc
Fragments(video=1583487699666666,format=m3u8-aapl-v8)
#EXT-X-CUE:ID=95766,TYPE="SpliceOut",DURATION=30.000000,TIME=158348769.966667,ELAPSED=0.233333
#EXTINF:6.400000,no-desc
Fragments(video=1583487702000000,format=m3u8-aapl-v8)
#EXT-X-CUE:ID=95766,TYPE="SpliceOut",DURATION=30.000000,TIME=158348769.966667,ELAPSED=6.633333
#EXTINF:6.400000,no-desc
Fragments(video=1583487766000000,format=m3u8-aapl-v8)
#EXT-X-CUE:ID=95766,TYPE="SpliceOut",DURATION=30.000000,TIME=158348769.966667,ELAPSED=13.033333
#EXTINF:6.400000,no-desc
Fragments(video=1583487830000000,format=m3u8-aapl-v8)
#EXT-X-CUE:ID=95766,TYPE="SpliceOut",DURATION=30.000000,TIME=158348769.966667,ELAPSED=19.433333
#EXTINF:6.400000,no-desc
Fragments(video=1583487894000000,format=m3u8-aapl-v8)
#EXT-X-CUE:ID=95766,TYPE="SpliceOut",DURATION=30.000000,TIME=158348769.966667,ELAPSED=25.833333
#EXTINF:4.166667,no-desc
Fragments(video=1583487958000000,format=m3u8-aapl-v8)
#EXTINF:2.233333,no-desc
Fragments(video=1583487999666666,format=m3u8-aapl-v8)
#EXTINF:6.400000,no-desc
Fragments(video=1583488022000000,format=m3u8-aapl-v8)
...

~~~

### <a name="216-cancellation-and-updates"></a>2.1.6 İptal ve Güncellemeler

İletiler, aynı sunum süresi ne zaman ve kimlikle birden çok ileti göndererek iptal edilebilir veya güncellenebilir. Sunu süresi ve kimliği olayı benzersiz bir şekilde tanımlar ve ön rulo kısıtlamalarını karşılayan belirli bir sunu zamanı için alınan son ileti, harekete geçen iletidir. Güncelleştirilmiş olay, daha önce alınan iletilerin yerini alır. Ön rulo kısıtlaması dört saniyedir. Sunu saatinden en az dört saniye önce alınan iletiler üzerine hareket edilecektir.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 Parçalı MP4 Yutma (Düzgün Akış)

Canlı akış için gereksinimler için [MS-SSTR-Yutma] bakın. Aşağıdaki bölümlerde zamanlanmış sunu meta verilerinin yutulma ile ilgili ayrıntılar sağlayabilirsiniz.  Zamanlanmış sunu meta verileri, Hem Live Server Manifest Box'ta (bkz. MS-SSTR) hem de Movie Box'da ('moov') tanımlanan seyrek bir parça olarak yutulmaktadır.  

Her seyrek parça, 'mdat' kutusunun ikili mesaj olduğu bir Film Parçası Kutusu ('moof') ve Medya Veri Kutusu'ndan ('mdat') oluşur.

Reklamların kare doğru eklenmesini sağlamak için, kodlayıcı, işaretin eklenmesi gereken sunum zamanında parçayı bölmelidir.  [ISO-14496-12] Ek I'de tanımlandığı gibi, yeni oluşturulan IDR çerçevesi veya tip 1 veya 2'nin Akış Erişim Noktaları (SAP) ile başlayan yeni bir parça OLUŞTURULMALıDıR. Bu, Azure Media Packager'ın bir HLS manifestosunu ve yeni Dönemin çerçeve ye uygun splice koşullu sunum zamanında başladığı bir DASH çok noktalı manifestosunu düzgün bir şekilde oluşturmasına olanak tanır.

### <a name="221-live-server-manifest-box"></a>2.2.1 Canlı Sunucu Bildirim Kutusu

Seyrek parça **BIR** ** \<\> textstream** girişi ile Live Server Manifest kutusunda ilan edilmelidir ve aşağıdaki öznitelikleri ayarlanmış **olmalıdır:**

| **Öznitelik Adı** | **Alan Türü** | **Gerekli?** | **Açıklama**                                                                                                                                                                                                              |
| ------------------ | -------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| systemBitrate      | Sayı         | Gerekli      | "0" olması **gerekir,** bilinmeyen, değişken bitrate ile bir parça gösteren.                                                                                                                                                          |
| parentTrackName    | Dize         | Gerekli      | Seyrek parça zaman kodlarızaman ölçeği hizalanmış olduğu üst parçanın adı **olmalıdır.** Ana parça seyrek bir parça olamaz.                                                                             |
| manifestOutput     | Boole        | Gerekli      | Seyrek parçanın Düzgün istemci bildirimine katıştırılmış olacağını belirtmek için "doğru" olması **gerekir.**                                                                                                                        |
| Alt            | Dize         | Gerekli      | **Dört** karakter kodu "DATA" olmalıdır.                                                                                                                                                                                  |
| Düzen             | Dize         | Gerekli      | İleti düzenini tanımlayan bir URL veya URL **olmalıdır.** [SCTE-35] iletileri **için,** iletilerin HLS, Smooth ve Dash istemcilerine [SCTE-35] uygun olarak gönderilebilmesi için bu "urn:scte:scte35:2013:bin" olmalıdır. |
| trackName          | Dize         | Gerekli      | Seyrek parçanın adı **olmalıdır.** TrackName, aynı şema ile birden çok olay akışını ayırt etmek için kullanılabilir. Her benzersiz olay akışı **MUST** benzersiz bir parça adı var.                                |
| Zaman ölçeği          | Sayı         | İsteğe bağlı      | Ana parçanın zaman ölçeği **olmalıdır.**                                                                                                                                                                               |

---

### <a name="222-movie-box"></a>2.2.2 Film Kutusu

Movie Box ('moov'), seyrek bir parça için akış başlığının bir parçası olarak Live Server Manifest Box'ı izler.

'moov' kutusu **aşağıdaki** kısıtlamalar ile [ISO-14496-12] tanımlandığı gibi bir **TrackHeaderBox ('tkhd))** kutusu içermelidir:

| **Alan Adı** | **Alan Türü**          | **Gerekli?** | **Açıklama**                                                                                                    |
| -------------- | ----------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------ |
| süre       | 64 bit imzasız tümseci | Gerekli      | Parça kutusunda sıfır örnek olduğundan ve parça kutusundaki örneklerin toplam süresi 0 olduğundan, 0 **olmalıdır.** |

---

'moov' **kutusu** aşağıdaki kısıtlamalar ile [ISO-14496-12] tanımlandığı gibi bir **HandlerBox ('hdlr')** içermelidir:

| **Alan Adı** | **Alan Türü**          | **Gerekli?** | **Açıklama**       |
| -------------- | ----------------------- | ------------- | --------------------- |
| handler_type   | 32 bit imzasız tümseci | Gerekli      | **'meta' Olmalıdır.** |

---

'stsd' kutusu **SHOULD** [ISO-14496-12] tanımlandığı gibi bir kodlama adı ile bir MetaDataSampleEntry kutusu içermelidir.  Örneğin, SCTE-35 iletileri için kodlama **adı** 'scte' olmalıdır.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 Film Parçası Kutusu ve Medya Veri Kutusu

Seyrek parça parçaları bir Film Parçası Kutusu ('moof') ve Bir Medya Veri Kutusu ('mdat') oluşur.

> [!NOTE]
> Reklamların kare doğru eklenmesini sağlamak için, kodlayıcı, işaretin eklenmesi gereken sunum zamanında parçayı bölmelidir.  [ISO-14496-12] Ek I'de tanımlandığı gibi, yeni oluşturulan IDR çerçevesi veya tip 1 veya 2'nin Akış Erişim Noktaları (SAP) ile başlayan yeni bir parça OLUŞTURULMALıDıR
> 

MovieFragmentBox ('moof') kutusu **aşağıdaki** alanları ile [MS-SSTR] tanımlandığı gibi bir **TrackFragmentExtendedHeaderBox ('uuid')** kutusu içermelidir:

| **Alan Adı**         | **Alan Türü**          | **Gerekli?** | **Açıklama**                                                                                           |
| ---------------------- | ----------------------- | ------------- | --------------------------------------------------------------------------------------------------------- |
| fragment_absolute_time | 64 bit imzasız tümseci | Gerekli      | Etkinliğin varış saati **olmalıdır.** Bu değer, iletiyi ana parçayla hizalar.           |
| fragment_duration      | 64 bit imzasız tümseci | Gerekli      | Olayın süresi **olmalıdır.** Süre bilinmediğini belirtmek için süre sıfır olabilir. |

---


MediaDataBox ('mdat') kutusu aşağıdaki biçime sahip **olmalıdır:**

| **Alan Adı**          | **Alan Türü**                   | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------------------- | -------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version                 | 32 bit imzasız tümsavar (uimsbf) | Gerekli      | 'mdat' kutusunun içeriğini belirler. Tanınmayan sürümler yoksayılır. Şu anda desteklenen tek sürüm 1'dir.                                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32 bit imzasız tümsavar (uimsbf) | Gerekli      | İletinin bu örneğini tanımlar. Eşdeğer semantik li iletiler aynı değere sahip olacaktır; diğer bir tanesi, aynı kimlikle herhangi bir olay ileti kutusunu işlemek yeterlidir.                                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32 bit imzasız tümsavar (uimsbf) | Gerekli      | TrackFragmentExtendedHeaderBox'ta belirtilen fragment_absolute_time toplamı ve **presentation_time_delta** etkinliğin sunum süresi olmalıdır. Sunu süresi ve süresi, [ISO-14496-12] Ek I'de tanımlandığı şekilde, tip 1 veya 2'nin Akış Erişim Noktaları (SAP) ile aynı hizaya **girmelidir.** HLS çıkış için, zaman ve süre segment sınırları ile uyumlu **olmalıdır.** Aynı olay **akışı** içindeki farklı olay iletilerinin sunu süresi ve süresi çakışmamalıdır. |
| message                 | bayt dizisi                       | Gerekli      | Olay iletisi. [SCTE-35] iletileri için ileti ikili splice_info_section(). [SCTE-35] iletileri **için,** iletilerin HLS, Smooth ve Dash istemcilerine [SCTE-35] uygun olarak gönderilebilmesi için bu splice_info_section() olmalıdır. [SCTE-35] iletileri için, ikili splice_info_section() 'mdat' kutusunun yüküdür ve base64 kodlanmış **değİlDİr.**                                                                                                                     |

---


### <a name="224-cancellation-and-updates"></a>2.2.4 İptal ve Güncellemeler

İletiler, aynı sunum süresi ne zaman ve kimlikle birden çok ileti göndererek iptal edilebilir veya güncellenebilir.  Sunu süresi ve kimliği, olayı benzersiz bir şekilde tanımlar. Belirli bir sunu zamanı için alınan ve rulo öncesi kısıtlamaları karşılayan son ileti, üzerine hareket edilen iletidir. Güncelleştirilmiş ileti, daha önce alınan iletilerin yerini alır.  Ön rulo kısıtlaması dört saniyedir. Sunu saatinden en az dört saniye önce alınan iletiler üzerine hareket edilecektir. 


## <a name="3-timed-metadata-delivery"></a>3 Zamanlı Meta veri teslimi

Olay akışı verileri Medya Hizmetleri için opaktır. Ortam Hizmetleri yalnızca yutulan bitiş noktası ile istemci bitiş noktası arasında üç bilgi parçasını geçirir. Aşağıdaki özellikler[SCTE-35] ve/veya istemcinin akış protokolüne uygun olarak istemciye teslim edilir:

1.  Şema – iletinin düzenini tanımlayan bir URL veya URL.
2.  Sunum Süresi – olayın ortam zaman çizelgesindeki sunum zamanı.
3.  Süre – olayın süresi.
4.  ID – etkinlik için isteğe bağlı benzersiz bir tanımlayıcı.
5.  İleti – olay verileri.

## <a name="31-microsoft-smooth-streaming-manifest"></a>3.1 Microsoft Sorunsuz Akış Bildirimi  

Seyrek bir ileti parçasının nasıl biçimlendirilenle ilgili ayrıntılar için seyrek parça işleme [MS-SSTR] bakın. [SCTE35] iletileri için, Düzgün Akış base64 kodlanmış splice_info_section() seyrek bir parçaya çıkar.
StreamIndex "DATA" alt **türüne** sahip olmalı ve **CustomAttributes** Name="Schema" ve Value="urn:scte:scte35:2013:bin" ile bir Öznitelik içermelidir.

#### <a name="smooth-client-manifest-example-showing-base64-encoded-scte35-splice_info_section"></a>Base64 kodlu [SCTE35] splice_info_section() gösteren Düzgün İstemci Bildirimi Örneği
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<SmoothStreamingMedia MajorVersion="2" MinorVersion="0" TimeScale="10000000" IsLive="true" Duration="0"
  LookAheadFragmentCount="2" DVRWindowLength="6000000000">

  <StreamIndex Type="video" Name="video" Subtype="" Chunks="0" TimeScale="10000000"
    Url="QualityLevels({bitrate})/Fragments(video={start time})">
    <QualityLevel Index="0" Bitrate="230000"
      CodecPrivateData="250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840" FourCC="WVC1"
      MaxWidth="364" MaxHeight="272"/>
    <QualityLevel Index="1" Bitrate="305000"
      CodecPrivateData="250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840" FourCC="WVC1"
      MaxWidth="364" MaxHeight="272"/>
    <c t="0" d="20000000" r="300" />
  </StreamIndex>
  <StreamIndex Type="audio" Name="audio" Subtype="" Chunks="0" TimeScale="10000000"
    Url="QualityLevels({bitrate})/Fragments(audio={start time})">
    <QualityLevel Index="0" Bitrate="96000" CodecPrivateData="1000030000000000000000000000E00042C0"
      FourCC="WMAP" AudioTag="354" Channels="2" SamplingRate="44100" BitsPerSample="16" PacketSize="4459"/>
    <c t="0" d="20000000" r="300" />
  </StreamIndex>
  <StreamIndex Type="text" Name="scte35-sparse-stream" Subtype="DATA" Chunks="0" TimeScale="10000000"
    ParentStreamIndex="video" ManifestOutput="true" 
    Url="QualityLevels({bitrate})/Fragments(captions={start time})">
    <QualityLevel Index="0" Bitrate="0" CodecPrivateData="" FourCC="">
      <CustomAttributes>
        <Attribute Name="Scheme" Value="urn:scte:scte35:2013:bin"/>
      </CustomAttributes>
    </QualityLevel>
    <!-- The following <c> and <f> fragments contains the base64-encoded [SCTE35] splice_info_section() message -->
    <c t="600000000" d="300000000">    <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t="1200000000" d="400000000">      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia>
~~~

## <a name="32-apple-hls-manifest-decoration"></a>3.2 Apple HLS Manifesto Dekorasyon

Azure Medya Hizmetleri, canlı veya isteğe bağlı bir etkinlik sırasında reklam dan yararlanıcı bilgileri bildirmek için aşağıdaki HLS bildirim etiketlerini destekler. 

- ExT-X-DATERANGE Apple HLS [RFC8216] tanımlanan
- EXT-X-CUE [Adobe-Primetime] olarak tanımlanan - bu mod "eski" olarak kabul edilir. Müşteriler mümkün olduğunda EXT-X-DATERANGE etiketini benimsemelidir.

Her etikete veri çıkışı, kullanılan yutma sinyali moduna bağlı olarak değişir. Örneğin, Adobe Simple modu ile RTMP yutma tam SCTE-35 base64 kodlanmış yükü içermez.

## <a name="321-apple-hls-with-ext-x-daterange-recommended"></a>3.2.1 EXT-X-DATERANGE ile Apple HLS (önerilir)

Apple HTTP Live Streaming [RFC8216] belirtimi [SCTE-35] iletilerinin sinyalizasyonuna olanak tanır. İletiler segment çalma listesine "SCTE-35'i EXT-X-DATERANGE'e eşleştirme" başlıklı [RFC8216] bölümü başına bir EXT-X-DATERANGE etiketine eklenir.  İstemci uygulama katmanı M3U çalma listesini ayrıştabilir ve M3U etiketlerini işleyebilir veya olayları Apple oynatıcı çerçevesi üzerinden alabilir.  

Azure Medya Hizmetleri'nde **(sürüm** 3 API) ÖNERILEN yaklaşım, [RFC8216] takip etmek ve [SCTE35] reklam için EXT-X_DATERANGE etiketini kullanmaktır.

## <a name="3211-example-hls-manifest-m3u8-showing-ext-x-daterange-signaling-of-scte-35"></a>3.2.1.1 Örnek HLS manifestosu .m3u8 SCTE-35 EXT-X-DATERANGE sinyalizasyon gösteren

Aşağıdaki örnek, Medya Hizmetleri dinamik paketleyicisinden HLS bildirim çıktısı, akıştaki SCTE-35 olaylarını işaret eden [RFC8216] EXT-X-DATERANGE etiketinin kullanımını gösterir. Buna ek olarak, bu akış [Adobe-Primetime] için "eski" EXT-X-CUE etiketini içerir.

~~~
#EXTM3U
#EXT-X-VERSION:8
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:2
#EXT-X-INDEPENDENT-SEGMENTS
#EXT-X-PROGRAM-DATE-TIME:2020-01-07T19:40:50Z
#EXTINF:1.501500,no-desc
Fragments(video=22567545,format=m3u8-aapl-v8)
#EXTINF:1.501500,no-desc
Fragments(video=22702680,format=m3u8-aapl-v8)
#EXTINF:1.501500,no-desc
Fragments(video=22837815,format=m3u8-aapl-v8)
#EXTINF:1.501500,no-desc
Fragments(video=22972950,format=m3u8-aapl-v8)
#EXTINF:1.501500,no-desc
Fragments(video=23108085,format=m3u8-aapl-v8)
#EXTINF:1.234567,no-desc
Fragments(video=23243220,format=m3u8-aapl-v8)
#EXTINF:0.016689,no-desc
Fragments(video=23354331,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=0.000022
#EXTINF:0.250244,no-desc
Fragments(video=23355833,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=0.250267
#EXTINF:0.850856,no-desc
Fragments(video=23378355,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=1.101122
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:10.610Z",SCTE35-IN=0xFC30200000000005DD00FFF00F05000003EA7F4FFE0165E4D3000101010000607CE85A
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=0.000000,TIME=260.610344,CUE="/DAgAAAAAAXdAP/wDwUAAAPqf0/+AWXk0wABAQEAAGB86Fo="
#EXTINF:0.650644,no-desc
Fragments(video=23454932,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=1.751767
#EXTINF:0.050044,no-desc
Fragments(video=23513490,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=1.801811
#EXTINF:1.451456,no-desc
Fragments(video=23517994,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=3.253267
#EXTINF:1.501500,no-desc
Fragments(video=23648625,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=4.754767
#EXTINF:1.501500,no-desc
Fragments(video=23783760,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=6.256267
#EXTINF:1.501500,no-desc
Fragments(video=23918895,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=7.757767
#EXTINF:1.501500,no-desc
Fragments(video=24054030,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=9.259267
#EXTINF:1.501500,no-desc
Fragments(video=24189165,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=10.760767
#EXTINF:1.501500,no-desc
Fragments(video=24324300,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=12.262267
#EXTINF:1.501500,no-desc
Fragments(video=24459435,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=13.763767
#EXTINF:1.501500,no-desc
Fragments(video=24594570,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=15.265267
#EXTINF:1.501500,no-desc
Fragments(video=24729705,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=16.766767
#EXTINF:1.501500,no-desc
Fragments(video=24864840,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=18.268267
#EXTINF:1.501500,no-desc
Fragments(video=24999975,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=19.769767
#EXTINF:1.501500,no-desc
Fragments(video=25135110,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=21.271267
#EXTINF:1.501500,no-desc
Fragments(video=25270245,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=22.772767
#EXTINF:1.501500,no-desc
Fragments(video=25405380,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=24.274267
#EXTINF:1.501500,no-desc
Fragments(video=25540515,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=25.775767
#EXTINF:1.501500,no-desc
Fragments(video=25675650,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=27.277267
#EXTINF:1.501500,no-desc
Fragments(video=25810785,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=28.778767
#EXTINF:1.501500,no-desc
Fragments(video=25945920,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=30.280267
#EXTINF:1.501500,no-desc
Fragments(video=26081055,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=31.781767
#EXTINF:1.501500,no-desc
Fragments(video=26216190,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=33.283267
#EXTINF:1.501500,no-desc
Fragments(video=26351325,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=34.784767
#EXTINF:1.501500,no-desc
Fragments(video=26486460,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=36.286267
#EXTINF:1.501500,no-desc
Fragments(video=26621595,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=37.787767
#EXTINF:1.501500,no-desc
Fragments(video=26756730,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=39.289267
#EXTINF:1.501500,no-desc
Fragments(video=26891865,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=40.790767
#EXTINF:1.501500,no-desc
Fragments(video=27027000,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=42.292267
#EXTINF:1.501500,no-desc
Fragments(video=27162135,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=43.793767
#EXTINF:1.501500,no-desc
Fragments(video=27297270,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=45.295267
#EXTINF:1.501500,no-desc
Fragments(video=27432405,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=46.796767
#EXTINF:1.501500,no-desc
Fragments(video=27567540,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=48.298267
#EXTINF:1.501500,no-desc
Fragments(video=27702675,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=49.799767
#EXTINF:1.501500,no-desc
Fragments(video=27837810,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=51.301267
#EXTINF:1.501500,no-desc
Fragments(video=27972945,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=52.802767
#EXTINF:1.501500,no-desc
Fragments(video=28108080,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=54.304267
#EXTINF:1.501500,no-desc
Fragments(video=28243215,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=55.805767
#EXTINF:1.501500,no-desc
Fragments(video=28378350,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=57.307267
#EXTINF:1.501500,no-desc
Fragments(video=28513485,format=m3u8-aapl-v8)
#EXT-X-DATERANGE:ID="1002",START-DATE="2020-01-07T19:45:09.509Z",SCTE35-OUT=0xFC30250000000005DD00FFF01405000003EA7FEFFE016461B8FE00526363000101010000F20D5E37
#EXT-X-CUE:ID="1002",TYPE="scte35",DURATION=59.993278,TIME=259.509244,CUE="/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==",ELAPSED=58.808767
#EXTINF:1.501500,no-desc
Fragments(video=28648620,format=m3u8-aapl-v8)

~~~


## <a name="322-apple-hls-with-adobe-primetime-ext-x-cue-legacy"></a>3.2.2 Adobe Primetime EXT-X-CUE ile Apple HLS (eski)

Azure Medya Hizmetleri'nde (sürüm 2 ve 3 API) sağlanan ve [Adobe-Primetime] "SCTE-35 Modu"nda tanımlandığı şekilde EXT-X-CUE etiketini kullanan bir "eski" uygulama da vardır. Bu modda, Azure Medya Hizmetleri BASE64 kodlu [SCTE-35] splice_info_section() ext-X-CUE etiketine yerlebir eder.  

"Eski" EXT-X-CUE etiketi aşağıdaki gibi tanımlanır ve [Adobe-Primetime] belirtiminde atıfta bulunulan normatif olabilir. Bu yalnızca gerektiğinde eski SCTE35 sinyalizasyonu için kullanılmalıdır, aksi takdirde önerilen etiket [RFC8216] EXT-X-DATERANGE olarak tanımlanır. 

| **Öznitelik Adı** | **Tür**                      | **Gerekli?**                             | **Açıklama**                                                                                                                                                                                                                                                                          |
| ------------------ | ----------------------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Işaret                | alıntı dize                 | Gerekli                                  | İleti[RFC4648]'de açıklandığı gibi base64 kodlanmış bir dize olarak kodlanmıştır. [SCTE-35] iletileri için bu base64 kodlanmış splice_info_section().                                                                                                                                      |
| TÜR               | alıntı dize                 | Gerekli                                  | İleti düzenini tanımlayan bir URL veya URL. [SCTE-35] iletileri için tür özel değeri "scte35" alır.                                                                                                                                                                          |
| Kimlik                 | alıntı dize                 | Gerekli                                  | Etkinlik için benzersiz bir tanımlayıcı. İleti yutulduğunda kimlik belirtilmemişse, Azure Medya Hizmetleri benzersiz bir kimlik oluşturur.                                                                                                                                              |
| Süre           | ondalık kayan nokta sayısı | Gerekli                                  | Olayın süresi. Bilinmiyorsa, değeri 0 **olmalıdır.** Birimler hizip saniyeleridir.                                                                                                                                                                                           |
| Geçen            | ondalık kayan nokta sayısı | İsteğe bağlı, ancak kayan pencere için gerekli | Sinyal kayan bir sunu penceresini desteklemek için yinelenirken, bu **alan,** olayın başlamasından bu yana geçen sunum süresinin miktarı olmalıdır. Birimler kesirli saniyelerdir. Bu değer, splice veya segmentin özgün belirtilen süresini aşabilir. |
| TIME               | ondalık kayan nokta sayısı | Gerekli                                  | Etkinliğin sunum zamanı. Birimler kesirli saniyelerdir.                                                                                                                                                                                                                        |


HLS oynatıcı uygulama katmanı, iletinin biçimini tanımlamak, iletiyi çözmek, gerekli zaman dönüşümlerini uygulamak ve olayı işlemek için TYPE'ı kullanır.  Olaylar, olay zaman damgasına göre ana parçanın segment çalma listesinde eşitlenen zamandır.  En yakın segmentten önce eklenirler (#EXTINF etiketi).

### <a name="323-hls-m3u8-manifest-example-using-legacy-adobe-primetime-ext-x-cue"></a>3.2.3 HLS .m3u8 "Legacy" Adobe Primetime EXT-X-CUE kullanarak manifesto örneği

Aşağıdaki örnek, Adobe Primetime EXT-X-CUE etiketini kullanarak HLS manifesto süslemesini gösterir.  "CUE" parametresi yalnızca TYPE ve Duration özelliklerini içerir, bu da bunun Adobe "basit" mod sinyali kullanarak bir RTMP kaynağı olduğu anlamına gelir.  Bu bir SCTE-35 modu sinyali ise, etiket [3.2.1.1 örneğinde](#3211-example-hls-manifest-m3u8-showing-ext-x-daterange-signaling-of-scte-35)görüldüğü gibi base64 kodlanmış ikili SCTE-35 yükünü içerir.

~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-PLAYLIST-TYPE:VOD
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:11
#EXT-X-PROGRAM-DATE-TIME:2019-12-10T09:18:14Z
#EXTINF:10.010000,no-desc
Fragments(video=4011540820,format=m3u8-aapl)
#EXTINF:10.010000,no-desc
Fragments(video=4011550830,format=m3u8-aapl)
#EXTINF:10.010000,no-desc
Fragments(video=4011560840,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000
#EXTINF:8.008000,no-desc
Fragments(video=4011570850,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=0.593000
#EXTINF:4.170000,no-desc
Fragments(video=4011578858,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=4.763000
#EXTINF:9.844000,no-desc
Fragments(video=4011583028,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=14.607000
#EXTINF:10.010000,no-desc
Fragments(video=4011592872,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=24.617000
#EXTINF:10.010000,no-desc
Fragments(video=4011602882,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=34.627000
#EXTINF:10.010000,no-desc
Fragments(video=4011612892,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=44.637000
#EXTINF:10.010000,no-desc
Fragments(video=4011622902,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=54.647000
#EXTINF:10.010000,no-desc
Fragments(video=4011632912,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=64.657000
#EXTINF:10.010000,no-desc
Fragments(video=4011642922,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=74.667000
#EXTINF:10.010000,no-desc
Fragments(video=4011652932,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=84.677000
#EXTINF:10.010000,no-desc
Fragments(video=4011662942,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=94.687000
#EXTINF:10.010000,no-desc
Fragments(video=4011672952,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=104.697000
#EXTINF:10.010000,no-desc
Fragments(video=4011682962,format=m3u8-aapl)
#EXT-X-CUE:ID=4011578265,TYPE="SpliceOut",DURATION=119.987000,TIME=4011578.265000,ELAPSED=114.707000
#EXTINF:10.010000,no-desc
Fragments(video=4011692972,format=m3u8-aapl)
#EXTINF:8.008000,no-desc
Fragments(video=4011702982,format=m3u8-aapl)

~~~

### <a name="324-hls-message-handling-for-legacy-adobe-primetime-ext-x-cue"></a>3.2.4 HLS Mesaj Taşıma "Eski" Adobe Primetime EXT-X-CUE için

Olaylar, her video ve ses parçasının segment çalma listesinde işaretlenir. EXT-X-CUE etiketinin konumu her zaman ilk HLS segmentinden hemen önce (birleştirme veya segment başı için) veya ZAMAN ve SÜRE özniteliklerinin [Adobe-Primetime] gerektirdiği şekilde ifade ettiği son HLS segmentinden (veya segment sonundaki splice veya segment sonu için) hemen önce **olmalıdır.**

Sürgülü bir sunu penceresi etkinleştirildiğinde, EXT-X-CUE **etiketi,** parça çalma listesinde her zaman tam olarak açıklanacak kadar sık tekrarlanmalıdır ve [Adobe-Primetime] tarafından gerekli olduğu şekilde, splice veya segmentin etkin olduğu süreyi belirtmek için ELAPSED **özniteliği** kullanılmalıdır.

Kayan bir sunu penceresi etkinleştirildiğinde, ext-x-CUE etiketleri, başvurulan ortam zamanı kayar sunu penceresinden dışarı yuvarlandığında segment çalma listesinden kaldırılır.

## <a name="33-dash-manifest-decoration-mpd"></a>3.3 DASH Manifesto Dekorasyon (MPD)

[MPEGDASH] olayları sinyallemek için üç yol sağlar:

1.  MPD EventStream'de işaretlenen olaylar
2.  Olay Mesaj Kutusu ('emsg') kullanılarak grup içi işaret lenen olaylar
3.  Hem 1 hem de 2'nin birleşimi

MPD Olay Akışı'nda işaret lenen olaylar VOD akışı için yararlıdır, çünkü istemciler MPD indirildiğinde hemen tüm olaylara erişebilirler. Ayrıca, akış aşağı SSAI satıcısının çok dönemli bir MPD bildiriminden gelen sinyalleri ayrıştırması ve reklam içeriğini dinamik olarak eklemesi gereken SSAI sinyali için de yararlıdır.  Grup içi ('emsg')çözüm, istemcilerin MPD'yi tekrar indirmelerine gerek olmadığı veya istemci ile menşe arasında ssai bildirimi manipülasyonu yapılmadığı canlı akış için yararlıdır. 

DASH için Azure Medya Hizmetleri varsayılan davranışı, Olay İleti Kutusu'nu ('emsg') kullanarak hem MPD EventStream'de hem de bant içi sinyal vermektir.

[RTMP] veya [MS-SSTR-Ingest] üzerinden yutulan işaret iletileri, bant içi 'emsg' kutuları ve/veya MPD Olay Akışları kullanılarak DASH olaylarına eşlenir. 

DASH için bant içi SCTE-35 sinyali[ SCTE-214-3] ve [DASH-IF-IOP] bölüm 13.12.2 ('SCTE35 Events') bölümünde tanımlanan tanımı ve gereksinimleri izler. 

Grup içi [SCTE-35] taşıma için Olay İletisi kutusu ('emsg') şema = "urn:scte:scte35:2013:bin" kullanır. MPD manifesto süslemesi için EventStream şeması "urn:scte:scte35:2014:xml+bin" kullanır.  Bu biçim, yutulmaya gelen tam SCTE-35 iletisinin ikili base64 kodlanmış çıktısını içeren olayın XML gösterimidir. 

[SCTE-35] işaret mesajlarının DASH'te taşınmasının normatif referans tanımları [SCTE-214-1] sn 6.7.4 (MPD) ve [SCTE-214-3] sn 7.3.2 (SCTE 35 işaret mesajlarının taşınması) mevcuttur.

### <a name="331-mpeg-dash-mpd-eventstream-signaling"></a>3.3.1 MPEG DASH (MPD) EventStream Sinyalizasyonu

Olayların Manifestosu (MPD) dekorasyonu, Dönem öğesi içinde görünen EventStream öğesi kullanılarak MPD'de işaretlenir. Kullanılan şema "urn:scte:scte35:2014:xml+bin"dir.

> [!NOTE]
> Kısaltma amacıyla [SCTE-35] tamamen ayrıştırılmış bir işaret mesajının taşınmasına alternatif olarak Signal.Binary elementinde (Signal.SpliceInfoSection elemanı yerine) 64 kodlanmış bölümün kullanımına izin verir.
> Azure Media Services, MPD bildiriminde sinyal vermek için bu 'xml+bin' yaklaşımını kullanır.
> Bu aynı zamanda [DASH-IF-IOP] kullanılan önerilen yöntemdir - [DASH IF IOP kılavuzunun 'Reklam ekleme olay akışları'](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html#ads-insertion-event-streams) başlıklı bölüme bakın
> 

EventStream öğesi aşağıdaki özniteliklere sahiptir:

| **Öznitelik Adı** | **Tür**                | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------ | ----------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| scheme_id_uri      | string                  | Gerekli      | İletinin düzenini tanımlar. Şema, Live Server Manifest kutusundaki Düzen özniteliğinin değerine ayarlanır. Değer, ileti düzenini tanımlayan bir URL veya URL **olmalıdır;** Desteklenen çıkış şemasıId,[SCTE-214-1] sn 6.7.4 (MPD) başına "urn:scte:scte35:2014:xml+bin" olmalıdır, çünkü hizmet şu anda MPD'de kısalık için yalnızca "xml+bin"i destekler. |
| value              | string                  | İsteğe bağlı      | İletinin anlambilimini özelleştirmek için şemanın sahipleri tarafından kullanılan ek bir dize değeri. Aynı şema ile birden çok olay akışı ayırt etmek için, **değer** olay akışı ([MS-SSTR-Ingest] veya [RTMP] yutmak için AMF ileti adı için trackName) adına ayarlanmalıdır.                                                                         |
| Timescale          | 32 bit imzasız tümseci | Gerekli      | Zaman ölçeği, saniyede kene.                                                                                                                                                                                                                                                                                                                                                     |


### <a name="332-example-event-streams-for-mpeg-dash"></a>3.3.2 MPEG DASH için Örnek Olay Akışları

#### <a name="3321-example-mpeg-dash-mpd-manifest-signaling-of-rtmp-streaming-using-adobe-simple-mode"></a>3.3.2.1 Örnek MPEG DASH .mpd Adobe basit modu kullanarak RTMP akışı manifesto sinyali

Aşağıdaki örnekte, Adobe "basit" mod sinyali kullanarak bir RTMP akışı için Medya Hizmetleri dinamik paketleyicisinden bir alıntı EventStream gösterilmektedir.

~~~ xml
<!-- Example EventStream element using "urn:com:adobe:dpi:simple:2015" Adobe simple signaling per [Adobe-Primetime] -->
    <EventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="simplesignal" timescale="10000000">
        <Event presentationTime="1583497601000000" duration="300000000" id="1085900"/>
        <Event presentationTime="1583500901666666" duration="300000000" id="1415966"/>
        <Event presentationTime="1583504202333333" duration="300000000" id="1746033"/>
        <Event presentationTime="1583507502666666" duration="300000000" id="2076066"/>
        <Event presentationTime="1583510803333333" duration="300000000" id="2406133"/>
        <Event presentationTime="1583514104000000" duration="300000000" id="2736200"/>
        <Event presentationTime="1583517404666666" duration="300000000" id="3066266"/>
        <Event presentationTime="1583520705333333" duration="300000000" id="3396333"/>
        <Event presentationTime="1583524006000000" duration="300000000" id="3726400"/>
        <Event presentationTime="1583527306666666" duration="300000000" id="4056466"/>
        <Event presentationTime="1583530607333333" duration="300000000" id="4386533"/>
    </EventStream>
~~~

#### <a name="3322-example-mpeg-dash-mpd-manifest-signaling-of-an-rtmp-stream-using-adobe-scte-35-mode"></a>3.3.2.2 Örnek MPEG DASH .mpd Adobe SCTE-35 modunu kullanarak bir RTMP akışının sinyalizasyonu

Aşağıdaki örnekte, Adobe SCTE-35 mod sinyalini kullanan bir RTMP akışı için Medya Hizmetleri dinamik paketleyicisinden bir alıntı EventStream gösterilmektedir.

~~~ xml
<!-- Example EventStream element using xml+bin style signaling per [SCTE-214-1] -->

      <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" value="scte35" timescale="10000000">
        <Event presentationTime="2595092444" duration="11011000" id="1002">
            <Signal xmlns="http://www.scte.org/schemas/35/2016">
                <Binary>/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==</Binary>
            </Signal>
        </Event>
        <Event presentationTime="2606103444" id="1002">
            <Signal xmlns="http://www.scte.org/schemas/35/2016">
                <Binary>/DAgAAAAAAXdAP/wDwUAAAPqf0/+AWXk0wABAQEAAGB86Fo=</Binary>
            </Signal>
        </Event>
      </EventStream>
~~~

> [!IMPORTANT]
> PresentationTime'ın iletinin varış saatine değil, Dönem Başlangıç saatine göre çevrilen [SCTE-35] etkinliğinin sunu zamanı olduğunu unutmayın.
> [MPEGDASH] Event@presentationTime olarak tanımlar "Dönem başlangıcına göre olayın sunum süresini belirtir.
> Sunu süresinin saniye cinsinden değeri, bu özniteliğin değerinin ve EventStream@timescale özniteliğin değerinin bölünmesidir.
> Yoksa, sunu süresinin değeri 0'dır.

#### <a name="3331-example-mpeg-dash-manifest-mpd-with-single-period-eventstream-using-adobe-simple-mode-signals"></a>3.3.3.1 Örnek MPEG DASH manifestosu (MPD) ile tek dönem, EventStream, Adobe basit mod sinyalleri kullanarak

Aşağıdaki örnek, Adobe "basit" mod reklam sinyali yöntemini kullanarak bir kaynak RTMP akışı için Medya Hizmetleri dinamik paketleyicisinden çıktıgösterir. Çıktı, "urn:com:adobe:dpi:simple:2015" ve "simplesignal" olarak ayarlanan değer özelliğini kullanarak Bir EventStream'i gösteren tek bir dönem bildirimidir.
Her basit @presentationTimesinyal, @durationgelen basit sinyallere @id bağlı olarak doldurulan , ve özellikleri olan bir Olay öğesinde sağlanır.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="static" mediaPresentationDuration="PT28M1.680S" minBufferTime="PT3S">
    <Period>
        <EventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="simplesignal" timescale="1000">
            <Event presentationTime="4011578265" duration="119987" id="4011578265"/>
        </EventStream>
        <AdaptationSet id="1" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.4D4028" maxWidth="1920" maxHeight="1080" startWithSAP="1">
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="simplesignal"/>
            <ProducerReferenceTime id="4011460740" type="0" wallClockTime="2020-01-25T19:35:54.740Z" presentationTime="4011460740"/>
            <SegmentTemplate timescale="1000" presentationTimeOffset="4011460740" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="4011460740" d="2002" r="57"/>
                    <S d="1401"/>
                    <S d="601"/>
                    <S d="2002"/>

                     <!--> ... video segments truncated for sample brevity </-->

                </SegmentTimeline>
            </SegmentTemplate>
            <Representation id="1_V_video_14759481473095519504" bandwidth="6000000" width="1920" height="1080"/>
            <Representation id="1_V_video_1516803357996956148" bandwidth="4000000" codecs="avc1.4D401F" width="1280" height="720"/>
            <Representation id="1_V_video_5430608182379669372" bandwidth="2600000" codecs="avc1.4D401F" width="960" height="540"/>
            <Representation id="1_V_video_3780180650986497347" bandwidth="1000000" codecs="avc1.4D401E" width="640" height="360"/>
            <Representation id="1_V_video_13759117363700265707" bandwidth="699000" codecs="avc1.4D4015" width="480" height="270"/>
            <Representation id="1_V_video_6140004908920393176" bandwidth="400000" codecs="avc1.4D4015" width="480" height="270"/>
            <Representation id="1_V_video_10673801877453424365" bandwidth="200000" codecs="avc1.4D400D" width="320" height="180"/>
        </AdaptationSet>
        <AdaptationSet id="2" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2">
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="simplesignal"/>
            <ProducerReferenceTime id="4011460761" type="0" wallClockTime="2020-01-25T19:35:54.761Z" presentationTime="4011460761"/>
            <Label>audio</Label>
            <SegmentTemplate timescale="1000" presentationTimeOffset="4011460740" media="QualityLevels($Bandwidth$)/Fragments(audio=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(audio=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="4011460761" d="1984"/>
                    <S d="2005" r="1"/>
                    <S d="2006"/>

                    <!--> ... audio segments truncated for example brevity </-->

                </SegmentTimeline>
            </SegmentTemplate>
            <Representation id="5_A_audio_17504386117102112482" bandwidth="128000" audioSamplingRate="48000"/>
        </AdaptationSet>
    </Period>
</MPD>

~~~

#### <a name="3332-example-mpeg-dash-manifest-mpd-with-multi-period-eventstream-using-adobe-scte35-mode-signaling"></a>3.3.3.2 Adobe SCTE35 mod sinyalizasyonunu kullanarak çok periyotlu EventStream ile Örnek MPEG DASH manifestosu (MPD)

Aşağıdaki örnek, Adobe SCTE35 mod sinyalini kullanarak bir kaynak RTMP akışı için Medya Hizmetleri dinamik paketleyicisinden çıktıgösterir.
Bu durumda, çıktı bildirimi eventstream öğesi ile çok noktalı DASH @schemeIdUri .mpd ve özellik "urn:scte:scte35:2014:xml+bin" ve "scte35" olarak ayarlanmış bir @value özelliktir. EventStream'deki her Olay öğesi tam base64 kodlanmış ikili SCTE35 sinyalini içerir 

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="dynamic" publishTime="2020-01-07T19:42:44Z" minimumUpdatePeriod="PT0S" timeShiftBufferDepth="PT58M56S" availabilityStartTime="2020-01-07T19:40:50Z" minBufferTime="PT4S">
    <Period start="PT2M48.168S" id="main-content_0">
        <AdaptationSet id="1" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
            <SegmentTemplate timescale="90000" presentationTimeOffset="15135120" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="15135120" d="135135" r="59"/>
                    <S d="111111"/>
                    <S d="1502"/>
                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="15135120" type="0" wallClockTime="2020-01-07T19:40:50Z" presentationTime="15135120"/>
            <Representation id="1_V_video_5322324134428436312" bandwidth="3500000" width="1280" height="720"/>
            <Representation id="1_V_video_16981495139092747609" bandwidth="2200000" width="960" height="540"/>
            <Representation id="1_V_video_1384718563016940751" bandwidth="1350000" codecs="avc1.64001F" width="704" height="396"/>
            <Representation id="1_V_video_4425970933904124207" bandwidth="850000" codecs="avc1.64001E" width="512" height="288"/>
            <Representation id="1_V_video_11952982975776937431" bandwidth="550000" codecs="avc1.640016" width="384" height="216"/>
            <Representation id="1_V_video_10673801877453424365" bandwidth="200000" codecs="avc1.640015" width="340" height="192"/>
        </AdaptationSet>
        <AdaptationSet id="2" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.5" lang="en">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
            <Label>audio</Label>
            <SegmentTemplate timescale="44100" presentationTimeOffset="7416208" media="QualityLevels($Bandwidth$)/Fragments(audio=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(audio=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="7417856" d="133120"/>
                    <S d="132096" r="1"/>
                    
                    <!--> ... aduio segments truncated for sample brevity </-->

                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="7417856" type="0" wallClockTime="2020-01-07T19:40:50.037Z" presentationTime="7417856"/>
            <Representation id="5_A_audio_17504386117102112482" bandwidth="128000" audioSamplingRate="44100"/>
        </AdaptationSet>
    </Period>
    <Period start="PT4M19.509S" id="scte-35_0">
        <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" value="scte35" timescale="10000000">
            <Event presentationTime="2595092444" duration="11011000" id="1002">
                <Signal xmlns="http://www.scte.org/schemas/35/2016">
                    <Binary>/DAlAAAAAAXdAP/wFAUAAAPqf+/+AWRhuP4AUmNjAAEBAQAA8g1eNw==</Binary>
                </Signal>
            </Event>
            <Event presentationTime="2606103444" id="1002">
                <Signal xmlns="http://www.scte.org/schemas/35/2016">
                    <Binary>/DAgAAAAAAXdAP/wDwUAAAPqf0/+AWXk0wABAQEAAGB86Fo=</Binary>
                </Signal>
            </Event>
        </EventStream>
        <AdaptationSet id="1" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
            <SegmentTemplate timescale="90000" presentationTimeOffset="23355832" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="23355833" d="22522"/>
                    <S d="76577"/>
                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="23355833" type="0" wallClockTime="2020-01-07T19:42:21.341Z" presentationTime="23355833"/>
            <Representation id="1_V_video_5322324134428436312" bandwidth="3500000" width="1280" height="720"/>
            <Representation id="1_V_video_16981495139092747609" bandwidth="2200000" width="960" height="540"/>
            <Representation id="1_V_video_1384718563016940751" bandwidth="1350000" codecs="avc1.64001F" width="704" height="396"/>
            <Representation id="1_V_video_4425970933904124207" bandwidth="850000" codecs="avc1.64001E" width="512" height="288"/>
            <Representation id="1_V_video_11952982975776937431" bandwidth="550000" codecs="avc1.640016" width="384" height="216"/>
            <Representation id="1_V_video_10673801877453424365" bandwidth="200000" codecs="avc1.640015" width="340" height="192"/>
        </AdaptationSet>
        <AdaptationSet id="2" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.5" lang="en">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
            <Label>audio</Label>
            <SegmentTemplate timescale="44100" presentationTimeOffset="11444358" media="QualityLevels($Bandwidth$)/Fragments(audio=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(audio=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="11446272" d="49152"/>
                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="11446272" type="0" wallClockTime="2020-01-07T19:42:21.384Z" presentationTime="11446272"/>
            <Representation id="5_A_audio_17504386117102112482" bandwidth="128000" audioSamplingRate="44100"/>
        </AdaptationSet>
    </Period>
    <Period start="PT4M20.610S" id="main-content_1">
        <AdaptationSet id="1" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
            <SegmentTemplate timescale="90000" presentationTimeOffset="23454931" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="23454932" d="58558"/>
                    <S d="4504"/>
                    <S d="130631"/>
                    <S d="135135" r="12"/>
                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="23454932" type="0" wallClockTime="2020-01-07T19:42:22.442Z" presentationTime="23454932"/>
            <Representation id="1_V_video_5322324134428436312" bandwidth="3500000" width="1280" height="720"/>
            <Representation id="1_V_video_16981495139092747609" bandwidth="2200000" width="960" height="540"/>
            <Representation id="1_V_video_1384718563016940751" bandwidth="1350000" codecs="avc1.64001F" width="704" height="396"/>
            <Representation id="1_V_video_4425970933904124207" bandwidth="850000" codecs="avc1.64001E" width="512" height="288"/>
            <Representation id="1_V_video_11952982975776937431" bandwidth="550000" codecs="avc1.640016" width="384" height="216"/>
            <Representation id="1_V_video_10673801877453424365" bandwidth="200000" codecs="avc1.640015" width="340" height="192"/>
        </AdaptationSet>
        <AdaptationSet id="2" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.5" lang="en">
            <InbandEventStream schemeIdUri="urn:mpeg:dash:event:2012" value="1"/>
            <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
            <Label>audio</Label>
            <SegmentTemplate timescale="44100" presentationTimeOffset="11492916" media="QualityLevels($Bandwidth$)/Fragments(audio=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(audio=i,format=mpd-time-csf)">
                <SegmentTimeline>
                    <S t="11495424" d="28672"/>
                    <S d="1024"/>
                    <S d="131072"/>
                    <S d="132096"/>
                    <S d="133120"/>
                    <S d="132096" r="1"/>
                    <S d="133120"/>
                </SegmentTimeline>
            </SegmentTemplate>
            <ProducerReferenceTime id="11495424" type="0" wallClockTime="2020-01-07T19:42:22.499Z" presentationTime="11495424"/>
            <Representation id="5_A_audio_17504386117102112482" bandwidth="128000" audioSamplingRate="44100"/>
        </AdaptationSet>
    </Period>
</MPD>

~~~
### <a name="334-mpeg-dash-in-band-event-message-box-signaling"></a>3.3.4 MPEG DASH Bant Içi Olay Mesaj Kutusu Sinyalizasyonu

Bant içi olay akışı, MPD'nin Adaptasyon Kümesi düzeyinde bir InbandEventStream öğesine sahip olmasını gerektirir.  Bu öğe, olay ileti kutusunda ('emsg') da görünen zorunlu bir şemaIdUri özniteliği ne de isteğe bağlı zaman ölçeği özniteliğine sahiptir.  MPD'de tanımlanmamış şema tanımlayıcıları olan olay ileti kutuları **bulunmamalıdır.**

Bant içi [SCTE-35] taşıma için, sinyaller şema = "urn:scte:scte35:2013:bin" **kullanmalıdır.**
[SCTE-35] bant içi iletilerin taşınmasının normatif tanımları [SCTE-214-3] sn 7.3.2 (SCTE 35 işaret mesajlarının taşınması) tanımlanır.

Aşağıdaki ayrıntılar, istemcinin [SCTE-214-3] uyarına 'emsg'de beklemesi gereken belirli değerleri ana hatlar:

| **Alan Adı**          | **Alan Türü**          | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                        |
| ----------------------- | ----------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| scheme_id_uri           | string                  | Gerekli      | İletinin düzenini tanımlar. Şema, Live Server Manifest kutusundaki Düzen özniteliğinin değerine ayarlanır. **Değer,** ileti düzenini tanımlayan bir URN olmalıdır. [SCTE-35] iletileri için bu ,[SCTE-214-3] uyarınya uygun olarak "urn:scte:scte35:2013:bin" **olmalıdır.**          |
| Değer                   | string                  | Gerekli      | İletinin anlambilimini özelleştirmek için şemanın sahipleri tarafından kullanılan ek bir dize değeri. Aynı şema ile birden çok olay akışını ayırt etmek için, değer olay akışının adı (RTMP yutmak için Düzgün yutmak için trackName veya AMF ileti adı) olarak ayarlanır. |
| Timescale               | 32 bit imzasız tümseci | Gerekli      | 'emsg' kutusundaki saat ve süre alanlarının saniyedeki kenelerinde zaman ölçeği.                                                                                                                                                                                                            |
| Presentation_time_delta | 32 bit imzasız tümseci | Gerekli      | Olayın sunum zamanının medya sunum zamanı deltası ve bu segmentteki en erken sunum zamanı. Sunu süresi ve süresi, [ISO-14496-12] Ek I'de tanımlandığı şekilde, tip 1 veya 2'nin Akış Erişim Noktaları (SAP) ile aynı hizaya **girmelidir.**                                  |
| event_duration          | 32 bit imzasız tümseci | Gerekli      | Bilinmeyen bir süreyi belirtmek için olayın süresi veya 0xFFFFFFFF.                                                                                                                                                                                                                              |
| Kimlik                      | 32 bit imzasız tümseci | Gerekli      | İletinin bu örneğini tanımlar. Eşdeğer semantik içeren iletiler aynı değere sahip olacaktır. İleti yutulduğunda kimlik belirtilmemişse, Azure Medya Hizmetleri benzersiz bir kimlik oluşturur.                                                                                        |
| Message_data            | bayt dizisi              | Gerekli      | Olay iletisi. [SCTE-35] iletileri için ileti verileri [SCTE-214-3] ile uyumlu ikili splice_info_section()                                                                                                                                                                        |


#### <a name="example-inbandevenstream-entity-for-adobe-simple-mode"></a>Adobe Simple modu için Örnek InBandEvenStream varlığı
~~~ xml

      <InbandEventStream schemeIdUri="urn:com:adobe:dpi:simple:2015" value="amssignal"/>
~~~

### <a name="335-dash-message-handling"></a>3.3.5 DASH İleti İşleme

Olaylar bant içinde, 'emsg' kutusu içinde, hem video hem de ses parçaları için sinyal verilir.  Sinyal, presentation_time_delta 15 saniye veya daha kısa olduğu tüm segment istekleri için oluşur. 

Kayan bir sunu penceresi etkinleştirildiğinde, olay iletisinin süresi ve süresinin toplamı bildirimdeki ortam verilerinin süresinden az olduğunda olay iletileri MPD'den kaldırılır.  Başka bir deyişle, olay iletileri, başvurulan ortam zamanı kayar sunu penceresinden dışarı yuvarlandığında bildirimden kaldırılır.

## <a name="4-scte-35-ingest-implementation-guidance-for-encoder-vendors"></a>4. SCTE-35 Encoder Satıcıları için Uygulama Yönergesi Ingest

Aşağıdaki yönergeler, kodlayıcı satıcısının bu belirtimi uygulamasını etkilenebilen yaygın sorunlardır.  Aşağıdaki yönergeler, bu belirtimi başkaları için daha kolay uygulamak için gerçek dünya ortağı geri bildirimlerine dayalı olarak toplanmıştır. 

[SCTE-35] iletileri [MS-SSTR-Ingest] için **"urn:scte:scte:scte35:2013:bin"** ve [RTMP] için **"scte35"** türü kullanılarak ikili biçimde yutulur. MPEG-2 aktarım akışı sunum süresi damgalarına (PTS) dayanan [SCTE-35] zamanlamasının dönüştürülmesini kolaylaştırmak için, PTS (pts_time + pts_adjustment splice_time()) ve ortam zaman çizelgesi arasında bir haritalama etkinlik sunum süresine göre sağlanır ( Pürüzsüz yutmak için fragment_absolute_time alanı ve RTMP yutmak için zaman alanı). 33 bit PTS değeri yaklaşık her 26,5 saatte bir yuvarlandığı için eşleme gereklidir.

Düzgün Akış [MS-SSTR-Ingest] Medya Veri Kutusu ('mdat') [SCTE-35] tanımlanan **splice_info_section()** içermelidir gerektirir. **MUST** 

RTMP yutma için, AMF iletisinin işaret özniteliği [SCTE-35] tanımlanan base64 kodlanmış **splice_info_section()** olarak ayarlanır.  

İletiler yukarıda açıklanan biçime sahip olduklarında, yukarıda açıklandığı gibi HLS, Smooth ve DASH istemcilerine gönderilir.  

Uygulamanızı Azure Medya Hizmetleri platformuyla sınarken, kodlama LiveEvent'i üzerinde test e geçmeden önce önce bir "geçiş" LiveEvent ile test etmeye başlayın.

---

## <a name="change-history"></a>Değişiklik Geçmişi

| Tarih     | Değişiklikler                                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------------------- |
| 07/2/19  | SCTE35 desteği için revize RTMP yutmak, Elemental Live için RTMP "onCuePoint" eklendi                                  |
| 08/22/19 | Özel meta veriler için RTMP'ye OnUserDataEvent eklemek için güncellendi                                                          |
| 1/08/20  | RTMP Simple ve RTMP SCTE35 modunda hata düzeltildi. "onCuePoint" den "onAdCue" olarak değiştirildi. Güncelleştirilmiş Basit mod tablosu. |

## <a name="next-steps"></a>Sonraki adımlar
Medya Hizmetleri öğrenme yollarını görüntüleyin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
