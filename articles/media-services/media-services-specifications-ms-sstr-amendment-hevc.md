---
title: HEVC için Sorunsuz Akış Protokolü (MS-SSTR) Değişikliği - Azure
description: Bu belirtim, Azure Medya Hizmetleri'nde HEVC ile parçalanmış MP4 tabanlı canlı akış için protokolü ve biçimi açıklar. "(Değişiklik Yok)" ibaresi, metnin yalnızca açıklama için kopyalandığını belirtmeleri dışında, yalnızca HEVC teslim etmek için gereken değişiklikler bu makalede belirtilmiştir.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: ''
ms.assetid: f27d85de-2cb8-4269-8eed-2efb566ca2c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2019
ms.author: johndeu
ms.openlocfilehash: be4009d418f2f8f3dff755e2e990efee593f070b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514230"
---
# <a name="smooth-streaming-protocol-ms-sstr-amendment-for-hevc"></a>HEVC için Düzgün Akış Protokolü (MS-SSTR) Değişikliği 

## <a name="1-introduction"></a>1 Giriş 

Bu makalede, HEVC kodlanmış videonun Sorunsuz Akışını sağlamak için Düzgün Akış Protokolü belirtimine [MS-SSTR] uygulanacak ayrıntılı değişiklikler sağlar. Bu belirtimde, yalnızca HEVC video codec'ini sunmak için gereken değişiklikleri ana hatlar. Makale[ MS-SSTR] belirtimi ile aynı numaralandırma şeasını izler. Makale boyunca sunulan boş başlıklar, okuyucuyu [MS-SSTR] belirtiminde konumlarına yönlendirmek için sağlanır.  "(Değişiklik Yok)" metnin yalnızca açıklama amacıyla kopyalanır olduğunu gösterir.

Makale, Bir Smooth Streaming manifestosunda HEVC video codec'inin ('hev1' veya 'hvc1' formatında parçalar kullanılarak) sinyalizasyonu için teknik uygulama gereksinimleri sağlar ve normatif referanslar geçerli MPEG standartlarına başvurmak üzere güncellenir HEVC, HEVC'nin Ortak Şifrelemesi ve ISO Temel Medya Dosya Biçimi için kutu adları en son belirtimlerle tutarlı olacak şekilde güncellenmiştir. 

Başvurulan Düzgün Akış Protokolü belirtimi [MS-SSTR] ses ve video gibi canlı ve isteğe bağlı dijital ortam ları sağlamak için kullanılan tel biçimini aşağıdaki şekilde açıklar: kodlayıcıdan bir web sunucusuna, sunucudan başka bir sunucuya ve bir HTTP istemcisine sunucu.
Http üzerinden Bir MPEG-4[([MPEG4-RA])](https://go.microsoft.com/fwlink/?LinkId=327787)tabanlı veri yapısı teslimkullanımı sıkıştırılmış medya içeriğinin farklı kalite düzeyleri arasında neredeyse gerçek zamanlı olarak sorunsuz geçiş sağlar. Sonuç, istemci bilgisayar veya aygıt için ağ ve video oluşturma koşulları değişse bile, HTTP istemci son kullanıcısı için sürekli bir oynatma deneyimidir.

## <a name="11-glossary"></a>1.1 Sözlük 

Aşağıdaki terimler *[MS-GLOS]* tanımlanır:

>   **genel olarak benzersiz tanımlayıcı (GUID) evrensel olarak benzersiz tanımlayıcı (UUID)**

Aşağıdaki koşullar bu belgeye özgüdir:

>  **kompozisyon süresi:** [[ISO/IEC-14496-12]](https://go.microsoft.com/fwlink/?LinkId=183695)'de tanımlandığı gibi, bir örneğin istemcide sunulduğu süre.
> 
>   **CENC**: [ISO/IEC 23001-7] Second Edition'da tanımlandığı gibi Ortak Şifreleme.
> 
>   **decode zaman:** [[ISO/IEC 14496-12:2008]](https://go.microsoft.com/fwlink/?LinkId=183695)'de tanımlandığı gibi, bir örneğin istemci üzerinde deşifre edilmesi gereken süre.

**parça:** Bir veya daha fazla **örnekten**oluşan bağımsız olarak indirilebilir **bir ortam** birimi.

>   **HEVC:** Yüksek Verimli Video Kodlama, [ISO/IEC 23008-2] tanımlandığı gibi
> 
>   **manifesto:** Bir **istemcinin ortam**için istekte bulunmasını sağlayan **sunu** hakkındaki meta veriler. **ortam:** Bir **sunuyu**oynatmak için istemci tarafından kullanılan sıkıştırılmış ses, video ve metin verileri. **ortam biçimi:** Sıkıştırılmış **bir örnek**olarak ses veya video temsil etmek için iyi tanımlanmış bir biçim.
> 
>   **sunum:** Tek bir film oynatmak için gereken tüm **akışlar** ve ilgili meta verilerin kümesi. **istek:** [[RFC2616]](https://go.microsoft.com/fwlink/?LinkId=90372) **yanıtında** tanımlandığı şekilde istemciden sunucuya gönderilen bir HTTP iletisi: Sunucudan istemciye gönderilen ve [[RFC2616]'da](https://go.microsoft.com/fwlink/?LinkId=90372) tanımlandığı şekilde bir HTTP iletisi
> 
>   **örnek:** **Ortamın** depolandığı ve işlendiği en küçük temel birim (çerçeve gibi).
> 
>   **MAY, SHOULD, SHOULD, SHOULD, SHOULD, SHOULD, SHOULD, SHOULD NOT:** Bu terimler (tüm büyük harflerde) [[RFC2119]](https://go.microsoft.com/fwlink/?LinkId=90317) Isteğe bağlı davranış kullanımının tüm ifadeleri açıklandığı gibi kullanılır MAY, SHOULD veya SHOULD NOT.

## <a name="12-references"></a>1.2 Referanslar

>   Bağlantılar belgelerin en son sürümüne ve sık sık güncelleştirildiğiiçin, Microsoft Açık Özellikler belgelerine yapılan başvurular bir yayımlama yılı içermez. Diğer belgelere yapılan atıflar, kullanılabilir olduğunda bir yayımlama yılını içerir.

### <a name="121-normative-references"></a>1.2.1 Normatif Referanslar 

>  [MS-SSTR] Sorunsuz Akış Protokolü *v20140502*[https://msdn.microsoft.com/library/ff469518.aspx](https://msdn.microsoft.com/library/ff469518.aspx)
> 
>   [ISO/IEC 14496-12] Uluslararası Standardizasyon Örgütü, "Bilgi teknolojisi -- Görsel-işitsel nesnelerin kodlanması -- Bölüm 12: ISO Base Media Dosya Formatı", ISO/IEC 14496-12:2014, Edition 4, Plus Corrigendum 1, Değişiklikler 1 & 2.
>   <https://standards.iso.org/ittf/PubliclyAvailableStandards/c061988_ISO_IEC_14496-12_2012.zip>
> 
>   [ISO/IEC 14496-15] Uluslararası Standardizasyon Örgütü, "Bilgi teknolojisi -- Görsel-işitsel nesnelerin kodlanması -- Bölüm 15: ISO Base Media Dosya Formatında NAL birim yapılandırılmış videonun taşınması", ISO 14496-15:2015, Edition 3.
>   <https://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=65216>
> 
>   [ISO/IEC 23008-2] Bilgi teknolojisi -- Heterojen ortamlarda yüksek verimli kodlama ve medya dağıtımı -- Bölüm 2: Yüksek verimli video kodlama: 2013 veya en yeni baskı<https://standards.iso.org/ittf/PubliclyAvailableStandards/c035424_ISO_IEC_23008-2_2013.zip>
> 
>   [ISO/IEC 23001-7] Bilgi teknolojisi — MPEG sistem teknolojileri — Bölüm 7: ISO temel medya dosya formatında ortak şifreleme, CENC Edition 02:2015<https://www.iso.org/iso/catalogue_detail.htm?csnumber=65271>
> 
>   [RFC-6381] IETF RFC-6381, "Kova" Ortam Türleri için "Codec' ve 'Profiller' Parametreleri"<https://tools.ietf.org/html/rfc6381>
> 
>   [MPEG4-RA] MP4 Kayıt Yetkilisi, "MP4REG",[http://www.mp4ra.org](https://go.microsoft.com/fwlink/?LinkId=327787)
> 
>   [RFC2119] Bradner, S., "Gereksinim Düzeylerini Göstermek için RFC'lerde kullanılacak anahtar kelimeler", BCP 14, RFC 2119, Mart 1997,[https://www.rfc-editor.org/rfc/rfc2119.txt](https://go.microsoft.com/fwlink/?LinkId=90317)

### <a name="122-informative-references"></a>1.2.2 Bilgilendirici Referanslar 

>   [MS-GLOS] Microsoft Corporation, "*Windows Protokolleri Master Sözlük*."
> 
>   [RFC3548] Josefsson, S., Ed., "Base16, Base32 ve Base64 Veri Kodlamaları", RFC 3548, Temmuz 2003,[https://www.ietf.org/rfc/rfc3548.txt](https://go.microsoft.com/fwlink/?LinkId=90432)
> 
>   [RFC5234] Crocker, D., Ed., ve Overell, P., "Sözdizimi Özellikleri için Artırılmış BNF: ABNF", STD 68, RFC 5234, Ocak 2008,[https://www.rfc-editor.org/rfc/rfc5234.txt](https://go.microsoft.com/fwlink/?LinkId=123096)


## <a name="13-overview"></a>1.3 Genel Bakış 

>   Yalnızca HEVC'nin teslimi için gereken Düzgün Akış belirtimideğişiklikleri aşağıda belirtilmiştir. Başvurulan Düzgün Akış belirtiminde [MS-SSTR] konumunu korumak için değişmemiş bölüm üstleri listelenir.

## <a name="14-relationship-to-other-protocols"></a>1.4 Diğer Protokollerle İlişki 

## <a name="15-prerequisitespreconditions"></a>1.5 Ön koşullar/Ön Koşullar 

## <a name="16-applicability-statement"></a>1.6 Uygulanabilirlik Bildirimi 

## <a name="17-versioning-and-capability-negotiation"></a>1.7 Sürümleme ve Yetenek Müzakeresi 

## <a name="18-vendor-extensible-fields"></a>1.8 Satıcı-Genişletilebilir Alanlar 

>   Aşağıdaki yöntem HEVC video formatını kullanarak akışları tanımlamak için kullanılacaktır:
> 
>   * **Ortam Biçimleri için Özel Açıklayıcı Kodlar:** Bu özellik, bölüm *2.2.2.5'te*belirtildiği gibi **FourCC** alanı tarafından sağlanmaktadır.
>   Uygulayıcılar, [[ISO/IEC-14496-12]](https://go.microsoft.com/fwlink/?LinkId=183695) belirtildiği gibi uzantı kodlarını MPEG4-RA'ya kaydederek uzantıların çakışmamasını sağlayabilir

## <a name="19-standards-assignments"></a>1.9 Standartlar Atamaları 

## <a name="2-messages"></a>2 Mesajlar 

## <a name="21-transport"></a>2.1 Ulaşım

## <a name="22-message-syntax"></a>2.2 İleti Sözdizimi 

### <a name="221-manifest-request"></a>2.2.1 Bildirim Talebi 

### <a name="222-manifest-response"></a>2.2.2 Manifesto Yanıtı 

#### <a name="2221-smoothstreamingmedia"></a>2.2.2.1 SmoothStreamingMedia 

>   **MinorVersion (değişken):** Bildirim Yanıtı iletisinin küçük sürümü. 2 OLARAK AYARLANMALıDıR. (Değişiklik yok)
> 
>   **Zaman Ölçeği (değişken):** Süre özniteliğinin bir saniyedeki artış sayısı olarak belirtilen zaman ölçeği. Varsayılan değer: 
> 1. (Değişiklik yok)
> 
>    Önerilen değer, kesirli kare hızı içeren video çerçevelerinin ve parçaların tam süresini temsil etmek için 90000'dir (örneğin, 30/1.001 Hz).

#### <a name="2222-protectionelement"></a>2.2.2.2 Koruma Elemanı 

Koruma Öğesi, video veya ses akışlarına Ortak Şifreleme (CENC) uygulandığında hazır bulunacaktır. HEVC şifreli akışlar Ortak Şifreleme 2nd Edition [ISO/IEC 23001-7] uyumlu olacaktır. VCL NAL Birimlerinde sadece dilim veri şifreli olacaktır.

#### <a name="2223-streamelement"></a>2.2.2.3 Akış Elemanı 

>   **StreamTimeScale (değişken):** Bu akıştaki süre ve zaman değerlerinin zaman ölçeği, bir saniyedeki artış sayısı olarak belirtilir. HEVC akışları için 90000 değeri önerilir. Ses akışları için dalga formu örnek frekansıyla eşleşen bir değer (örneğin, 48000 veya 44100) önerilir.

##### <a name="22231-streamprotectionelement"></a>2.2.2.3.1 Akarsu Koruma Elemanı

#### <a name="2224-urlpattern"></a>2.2.2.4 UrlPattern 

#### <a name="225-trackelement"></a>2.2.5 İzleme Elemanı 

>   **FourCC (değişken):** Her örnek için hangi ortam biçiminin kullanıldığını tanımlayan dört karakterli bir kod. Aşağıdaki değerler aralığı aşağıdaki anlamsal anlamlarla ayrılmıştır:
> 
> * "hev1": Bu parça için video örnekleri [ISO/IEC-14496-15] belirtilen 'hev1' örnek açıklama biçimini kullanarak HEVC video kullanır.
>
> * "hvc1": Bu parça için video örnekleri [ISO/IEC-14496-15] belirtilen 'hvc1' örnek açıklama biçimini kullanarak HEVC video kullanır.
> 
>   **CodecPrivateData (değişken):** Medya biçimine özgü parametreleri belirten ve parçadaki tüm örnekleriçin ortak olan ve hex kodlu bayt lar dizisi olarak temsil edilen veriler. Bayt dizisinin biçimi ve anlamsal anlamı **FourCC** alanının değerine aşağıdaki gibi değişir:
> 
>   * TrackElement HEVC videosunu açıkladığında, **FourCC** alanı **"hev1"** veya **"hvc1"** ile eşit olur
> 
>   **CodecPrivateData** alanı, ABNF [[RFC5234]'de](https://go.microsoft.com/fwlink/?LinkId=123096) belirtilen aşağıdaki bayt dizisinin hex kodlu dize gösterimini içerir: (MS-SSTR'den değişiklik yoktur)
> 
>   * %x00 %x00 %x00 %x01 SPSField %x00 %x00 %x00 %x01 PPSField
> 
>   * SPSField Sıra Parametre Kümesi (SPS) içerir.
> 
>   * PPSField Dilim Parametre Kümesi (PPS) içerir.
> 
>   Not: Video Parametre Kümesi (VPS) CodecPrivateData'da bulunmaz, ancak 'hvcC' kutusunda depolanan dosyaların dosya üstbilgisinde yer almalıdır. Düzgün Akış Protokolü kullanan sistemler, Özel Atnitelik "codec"leri kullanarak ek kod çözme parametrelerini (örneğin, HEVC Tier) işaret etmelidir.

##### <a name="22251-customattributeselement"></a>2.2.2.5.1 Özel Nitelik Öğesi 

#### <a name="226-streamfragmentelement"></a>2.2.6 Akış Parçası Elemanı 

>   **SmoothStreamingMedia'nın MajorVersion** alanı **2,MinorVersion** alanı ise 2 olarak ayarlanmalıdır. (Değişiklik yok)

##### <a name="22261-trackfragmentelement"></a>2.2.2.6.1 Parça Parça Elemanı 

### <a name="223-fragment-request"></a>2.2.3 Parça İsteği 

>   **Not**: **MinorVersion** 2 ve 'hev1' veya 'hvc1' için istenen varsayılan ortam biçimi, [ISO/IEC 14496-12] ISO Base Media File Format Fourth Edition ve [ISO/IEC 23001-7] Ortak Şifreleme İkinci Sürümünde belirtilen 'iso8' markası ISO Base Media Dosya Biçimidir.

### <a name="224-fragment-response"></a>2.2.4 Parça Yanıtı 

#### <a name="2241-moofbox"></a>2.2.4.1 MoofBox 

#### <a name="2242-mfhdbox"></a>2.2.4.2 MfhdBox 

#### <a name="2243-trafbox"></a>2.2.4.3 TrafBox 

#### <a name="2244-tfxdbox"></a>2.2.4.4 TfxdBox 

>   **TfxdBox** amortismana kesilir ve işlevi [ISO/IEC 14496-12] bölüm 8.8.12'de belirtilen Parça Parçası Kod Çözme Zaman Kutusu ('tfdt') ile değiştirilir.
> 
>   **Not**: İstemci, Parça Çalıştır Kutusu'nda ('trun') listelenen örnek süreleri toplamı yaparak veya varsayılan örneklem süresinin örnek lem sayısını çarparak parçanın süresini hesaplayabilir. 'tfdt' artı parça süresindeki baseMediaDecodeTime, bir sonraki parçanın URL zaman parametresini eşittir.
> 
>   [ISO/IEC 14496-12] bölüm 8.16.5'te belirtildiği gibi, Film Parçası Kutusu tarafından başvurulan ilk örneğin Parça Parçası Decode Time'ına karşılık gelen UTC süresini belirtmek için gerektiğinde bir Film Parçası Kutusu'ndan ('moof') önce bir Yapımcı Referans Zaman Kutusu ('prft') eklenmelidir.

#### <a name="2245-tfrfbox"></a>2.2.4.5 TfrfBox 

>   **TfrfBox** amortismana hazır dır ve işlevi [ISO/IEC 14496-12] bölüm 8.8.12'de belirtilen Parça Parçası Kod Çözme Zaman Kutusu ('tfdt') ile değiştirilir.
> 
>   **Not**: İstemci, Parça Çalıştır Kutusu'nda ('trun') listelenen örnek süreleri toplamı yaparak veya varsayılan örneklem süresinin örnek lem sayısını çarparak parçanın süresini hesaplayabilir. 'tfdt' artı parça süresindeki baseMediaDecodeTime, bir sonraki parçanın URL zaman parametresini eşittir. İleriye bakın adresler, canlı akışı geciktirdikleri için amortismana alınır.

#### <a name="2246-tfhdbox"></a>2.2.4.6 TfhdBox 

>   **TfhdBox** ve ilgili alanlar, parçadaki örnek meta veri başına varsayılanları kapsüller. **TfhdBox** alanının sözdizimi, [[ISO/IEC-14496-12]](https://go.microsoft.com/fwlink/?LinkId=183695) bölüm 8.8.7'de tanımlanan Parça Parçası Üstbilgi Kutusu'nun sözdiziminin sıkı bir alt kümesidir.
> 
>   **BaseDataOffset (8 bayt):** **MdatBox** alanının başlangıcından **MdatBox** alanında örnek alana ofset, bayt olarak. Bu kısıtlamayı işaret etmek için varsayılan taban-is-moof bayrağı (0x020000) ayarlanmalıdır.

#### <a name="2247-trunbox"></a>2.2.4.7 TrunBox 

>   **TrunBox** ve ilgili alanlar, istenen parça için örnek meta verilerine göre kapsüllenir. **TrunBox** sözdizimi,[[ISO/IEC-14496-](https://go.microsoft.com/fwlink/?LinkId=183695)*12]* bölümünde tanımlanan Sürüm 1 Parça Parça Çalıştırma Kutusu'nun sıkı bir alt kümesidir.
> 
>   **SampleCompositionTimeOffset (4 bayt):** Parçada sunulan ilk numunenin sunum süresi, ilk çözülmüş numunenin kod çözme süresine eşit olacak şekilde ayarlanmış her bir örneğin Örnek Kompozisyon Süresi mahsup edilir. Negatif video örnek kompozisyon ofsetleri SHALL kullanılır,
> 
>   [[ISO/IEC-14496-12]](https://go.microsoft.com/fwlink/?LinkId=183695) olarak tanımlanmaktadır.
> 
>   Not: Bu, video gecikmeli sesin en büyük çözülmüş resim arabellek kaldırma gecikmesine eşit olarak neden olduğu bir video eşitleme hatasını önler ve farklı kaldırma gecikmeleri olabilecek alternatif parçalar arasında sunu zamanlamasını korur.
> 
>   ABNF [[RFC5234]'de](https://go.microsoft.com/fwlink/?LinkId=123096) belirtilen bu bölümde tanımlanan alanların sözdizimi aşağıdakiler dışında aynı kalır:
> 
>   SampleCompositionTimeOffset = SIGNED_INT32

#### <a name="2248-mdatbox"></a>2.2.4.8 MdatBox 

#### <a name="2249-fragment-response-common-fields"></a>2.2.4.9 Parça Tepkisi Ortak Alanlar 

### <a name="225-sparse-stream-pointer"></a>2.2.5 Seyrek Akış İşaretçisi 

### <a name="226-fragment-not-yet-available"></a>2.2.6 Fragman Henüz Mevcut Değil 

### <a name="227-live-ingest"></a>2.2.7 Canlı Yutma 

#### <a name="2271-filetype"></a>2.2.7.1 Dosya Türü 

>   **FileType (değişken):** MPEG-4[([MPEG4-RA])](https://go.microsoft.com/fwlink/?LinkId=327787) dosyasının alt türünü ve kullanım amacını ve üst düzey öznitelikleri belirtir.
> 
>   **MajorBrand (değişken):** Medya dosyasının ana markası. MUST "isml" olarak ayarlanmalıdır.
> 
>   **MinorVersion (değişken):** Medya dosyasının küçük sürümü. 1 olarak ayarlanmalıdır.
> 
>   **Uyumlu Markalar (değişken):** MPEG-4'ün desteklenen markalarını belirtir.
>   MUST "ccff" ve "iso8."
> 
>   ABNF [[RFC5234]'de](https://go.microsoft.com/fwlink/?LinkId=123096) belirtilen bu bölümde tanımlanan alanların sözdizimi aşağıdaki gibidir:

    FileType = MajorBrand MinorVersion CompatibleBrands
    MajorBrand = STRING_UINT32
    MinorVersion = STRING_UINT32
    CompatibleBrands = "ccff" "iso8" 0\*(STRING_UINT32)

**Not**: 'ccff' ve 'iso8' uyumluluk markaları, parçaların "Ortak Kapsayıcı Dosya Biçimi" ve Ortak Şifreleme [ISO/IEC 23001-7] ve ISO Base Media Dosya Formatı Sürümü 4 [ISO/IEC 14496-12] uygun olduğunu göstermektedir.

#### <a name="2272-streammanifestbox"></a>2.2.7.2 StreamManifestBox 

##### <a name="22721-streamsmil"></a>2.2.7.2.1 AkarsuMIL 

#### <a name="2273-liveservermanifestbox"></a>2.2.7.3 LiveServerManifestBox 

##### <a name="22731-livesmil"></a>2.2.7.3.1 LiveSMIL 

#### <a name="2274-moovbox"></a>2.2.7.4 MoovBox 

#### <a name="2275-fragment"></a>2.2.7.5 Parça 

##### <a name="22751-track-fragment-extended-header"></a>2.2.7.5.1 Parça Parça Genişletilmiş Başlık 

### <a name="228-server-to-server-ingest"></a>2.2.8 Sunucudan Sunucuya Yutma 

## <a name="3-protocol-details"></a>3 Protokol Detayları 


## <a name="31-client-details"></a>3.1 İstemci Detayları 

### <a name="311-abstract-data-model"></a>3.1.1 Soyut Veri Modeli 

#### <a name="3111-presentation-description"></a>3.1.1.1 Sunum Açıklaması 

>   Sunu Açıklaması veri öğesi, sunu için tüm meta verileri kapsar.
> 
>   Sunu Meta verileri: Sunudaki tüm akışlar için ortak olan bir meta veri kümesi. Sunu Meta verileri, bölüm *2.2.2.1'de*belirtilen aşağıdaki alanları kapsar:
> 
> * **MajorVersion**
> * **MinorVersion**
> * **Zaman ölçeği**
> * **Süre**
> * **IsLive**
> * **LookaheadCount**
> * **DVRWindowLength**
> 
>   HEVC Akışları içeren sunumlar:

    MajorVersion = 2
    MinorVersion = 2

>   LookaheadCount = 0 (Not: Amortismana dahil kutular)
> 
>   Sunular DA AYARLI MI:

    TimeScale = 90000

>   Akış Koleksiyonu: Bölüm *3.1.1.1.2'de*belirtildiği gibi Akış Açıklaması veri öğeleri koleksiyonu.
> 
>   Koruma Açıklaması: Koruma Sistemi Meta veri açıklaması veri öğelerinin toplanması, bölüm *3.1.1.1.1'de*belirtildiği gibi.

##### <a name="31111-protection-system-metadata-description"></a>3.1.1.1.1 Koruma Sistemi Metaveri Açıklaması 

>   Koruma Sistemi Meta Veri Açıklaması veri öğesi, tek bir İçerik Koruma Sistemine özgü meta verileri kapsüller. (Değişiklik yok)
> 
>   Koruma Üstbilgisi Açıklaması: Tek bir İçerik Koruma Sistemi ile ilgili içerik koruma meta verileri. Koruma Üstbilgi Açıklaması, bölüm *2.2.2.2'de*belirtilen aşağıdaki alanları kapsar:
> 
>   * **Systemıd**
>   * **ProtectionHeaderContent**

##### <a name="31112-stream-description"></a>3.1.1.1.2 Akış Açıklaması 

###### <a name="311121-track-description"></a>3.1.1.1.2.1 Parça Açıklaması 

###### <a name="3111211-custom-attribute-description"></a>3.1.1.1.2.1.1 Özel Öznitelik Açıklaması 

##### <a name="3113-fragment-reference-description"></a>3.1.1.3 Parça Referans Açıklaması 

###### <a name="31131-track-specific-fragment-reference-description"></a>3.1.1.3.1 Parçaya Özgü Parça Referans Açıklaması 

#### <a name="3112-fragment-description"></a>3.1.1.2 Parça Açıklaması 

##### <a name="31121-sample-description"></a>3.1.1.2.1 Örnek Açıklama 

### <a name="312-timers"></a>3.1.2 Zamanlayıcılar 

### <a name="313-initialization"></a>3.1.3 Başlatma 

### <a name="314-higher-layer-triggered-events"></a>3.1.4 Yüksek Katmanlı Tetiklenen Olaylar 

#### <a name="3141-open-presentation"></a>3.1.4.1 Açık Sunum 

#### <a name="3142-get-fragment"></a>3.1.4.2 Parça Al 

#### <a name="3143-close-presentation"></a>3.1.4.3 Kapanış Sunumu 

### <a name="315-processing-events-and-sequencing-rules"></a>3.1.5 İşlem Olayları ve Sıralama Kuralları 

#### <a name="3151-manifest-request-and-manifest-response"></a>3.1.5.1 Bildirim İsteği ve Bildirim Yanıtı 

#### <a name="3152-fragment-request-and-fragment-response"></a>3.1.5.2 Parça İstek ve Parça Yanıtı

## <a name="32-server-details"></a>3.2 Sunucu Ayrıntıları

## <a name="33-live-encoder-details"></a>3.3 Canlı Kodlayıcı Detayları 

## <a name="4-protocol-examples"></a>4 Protokol Örnekleri 

## <a name="5-security"></a>5 Güvenlik 

## <a name="51-security-considerations-for-implementers"></a>5.1 Uygulayıcılar için Güvenlik Hususları

>   Bu protokol kullanılarak taşınan içerik yüksek ticari değere sahipse, içeriğin yetkisiz kullanımını önlemek için bir İçerik Koruma Sistemi kullanılmalıdır. **ProtectionElement,** İçerik Koruma Sistemi'nin kullanımıyla ilgili meta verileri taşımak için kullanılabilir. Korumalı ses ve video içeriği MPEG Common Encryption Second Edition tarafından belirtildiği şekilde şifrelenir: 2015 [ISO/IEC 23001-7].
> 
>   **Not**: HEVC videosu için vcl'lerde yalnızca dilim verileri şifrelenir. Dilim üstbilgi ve diğer N'ler, şifre çözmeden önce sunu uygulamalarına erişebilir. güvenli bir video yolunda, şifrelenmiş bilgiler sunu uygulamaları için kullanılamaz.

## <a name="52-index-of-security-parameters"></a>5.2 Güvenlik Parametreleri Dizini 


| **Güvenlik parametresi**  | **Section**         |
|-------------------------|---------------------|
| Koruma Elemanı       | *2.2.2.2*           |
| Ortak Şifreleme Kutuları | *[ISO/IEC 23001-7]* |

## <a name="53-common-encryption-boxes"></a>5.3 Yaygın Şifreleme Kutuları

Ortak Şifreleme uygulandığında aşağıdaki kutular parça yanıtlarında bulunabilir ve [ISO/IEC 23001-7] veya [ISO/IEC 14496-12] olarak belirtilir:

1.  Koruma Sistemi Özel Başlık Kutusu ('pssh')

2.  Örnek Şifreleme Kutusu ('senc')

3.  Örnek Yardımcı Bilgi Ofset Kutusu ('saio')

4.  Örnek Yardımcı Bilgi Boyut Kutusu ('saiz')

5.  Örnek Grup Açıklama Kutusu ('sgpd')

6.  Grup Kutusuna Örnek ('sbgp')

---

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
