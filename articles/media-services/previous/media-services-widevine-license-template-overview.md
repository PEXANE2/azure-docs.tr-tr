---
title: Widevine lisans şablonuna genel bakış | Microsoft Dokümanlar
description: Bu konu, Widevine lisanslarını yapılandırmak için kullanılan Widevine lisans şablonuna genel bir bakış sağlar.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: c7511279e66ab598e4ae3c26f053915b7393b39d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74978399"
---
# <a name="widevine-license-template-overview"></a>Widevine lisans şablonuna genel bakış 
Azure Medya Hizmetlerini, Google Widevine lisanslarını yapılandırmak ve istemek için kullanabilirsiniz. Oyuncu Widevine korumalı içeriğinizi oynatmaya çalıştığında, lisans almak için lisans teslim hizmetine bir istek gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. İstemciye gönderilir ve belirtilen içeriğin şifresini çözmek ve oynatmak için kullanılır.

Widevine lisans isteği JSON iletisi olarak biçimlendirilir.  

>[!NOTE]
> Hiçbir değeri olmayan boş bir ileti{}oluşturabilirsiniz, sadece " dedi. Ardından varsayılanlarla bir lisans şablonu oluşturulur. Varsayılan çoğu servis talebi için çalışır. Microsoft tabanlı lisans teslim senaryoları her zaman varsayılanları kullanmalıdır. "Sağlayıcı" ve "content_id" değerlerini ayarlamanız gerekiyorsa, sağlayıcının Widevine kimlik bilgileriyle eşleşmesi gerekir.

    {  
       "payload": "<license challenge>",
       "content_id": "<content id>" 
       "provider": "<provider>"
       "allowed_track_types": "<types>",
       "content_key_specs": [  
          {  
             "track_type": "<track type 1>"
          },
          {  
             "track_type": "<track type 2>"
          },
          …
       ],
       "policy_overrides": {  
          "can_play": <can play>,
          "can persist": <can persist>,
          "can_renew": <can renew>,
          "rental_duration_seconds": <rental duration>,
          "playback_duration_seconds": <playback duration>,
          "license_duration_seconds": <license duration>,
          "renewal_recovery_duration_seconds": <renewal recovery duration>,
          "renewal_server_url": "<renewal server url>",
          "renewal_delay_seconds": <renewal delay>,
          "renewal_retry_interval_seconds": <renewal retry interval>,
          "renew_with_usage": <renew with usage>
       }
    }

## <a name="json-message"></a>JSON iletisi
| Adı | Değer | Açıklama |
| --- | --- | --- |
| yük |Base64 kodlanmış dize |İstemci tarafından gönderilen lisans isteği. |
| content_id |Base64 kodlanmış dize |Identifier, her content_key_specs,track_type için anahtar kimliği ve içerik anahtarını türetilmiştir. |
| Sağlayıcı |string |İçerik anahtarlarını ve ilkelerini aramak için kullanılır. Microsoft anahtar teslimi Widevine lisans teslimi için kullanılırsa, bu parametre yoksayılır. |
| policy_name |string |Daha önce kaydedilmiş bir ilkenin adı. İsteğe bağlı. |
| allowed_track_types |enum |SD_ONLY ya da SD_HD. Lisansa hangi içerik anahtarlarının dahil edildiğini denetler. |
| content_key_specs |JSON yapıları dizisi, bölüme bakın "İçerik anahtar özellikleri."  |İçerik anahtarlarının döndürülmek için hangi hassas grenli denetim. Daha fazla bilgi için "İçerik anahtar özellikleri" bölümüne bakın. yalnızca bir allowed_track_types ve content_key_specs değerleri belirtilebilir. |
| use_policy_overrides_exclusively |Boolean, doğru veya yanlış |policy_overrides tarafından belirtilen ilke özniteliklerini kullanın ve önceden depolanmış tüm ilkeleri atlayın. |
| policy_overrides |JSON yapısı, bölüme bakın "İlke geçersiz kılar." |Bu lisans için ilke ayarları.  Bu varlığın önceden tanımlanmış bir ilkesi olması durumunda, bu belirtilen değerler kullanılır. |
| session_init |JSON yapısı, bölüme bakın "Oturum başlatma." |İsteğe bağlı veriler lisansa aktarılır. |
| parse_only |Boolean, doğru veya yanlış |Lisans isteği ayrıştırılır, ancak lisans verilmez. Ancak, lisans isteğindeki değerler yanıtta döndürülür. |

## <a name="content-key-specs"></a>İçerik anahtar özellikleri
Önceden varolan bir ilke varsa, içerik anahtarı spec değerleri herhangi bir belirtmeye gerek yoktur. Bu içerikle ilişkili önceden varolan ilke, Yüksek bant genişliğine sahip Dijital İçerik Koruması (HDCP) ve Copy General Management System (CGMS) gibi çıktı korumasını belirlemek için kullanılır. Önceden varolan bir ilke Widevine lisans sunucusuna kaydedilmemişse, içerik sağlayıcısı değerleri lisans isteğine enjekte edebilir.   

use_policy_overrides_exclusively seçeneğine bakılmaksızın, tüm parçalar için her content_key_specs değeri belirtilmelidir. 

| Adı | Değer | Açıklama |
| --- | --- | --- |
| content_key_specs. track_type |string |Bir parça türü adı. content_key_specs lisans isteğinde belirtilmişse, tüm parça türlerini açıkça belirttiğinden emin olun. Aksi takdirde son 10 saniye geri oynatılamaması ile sonuçlanır. |
| content_key_specs  <br/> security_level |uint32 |Oynatma için istemci sağlamlık gereksinimlerini tanımlar. <br/> - Yazılım tabanlı beyaz kutu şifreleme gereklidir. <br/> - Yazılım şifrelemesi ve obfuscated kod çözücü gereklidir. <br/> - Anahtar malzeme ve şifreleme işlemleri donanım destekli güvenilir yürütme ortamında yapılmalıdır. <br/> - İçeriğin şifrelemesi ve kod çözmesi donanım destekli güvenilir yürütme ortamında yapılmalıdır.  <br/> - Şifreleme, kod çözme ve ortamın tüm kullanımı (sıkıştırılmış ve sıkıştırılmamış) donanım destekli güvenilir yürütme ortamında ele alınmalıdır. |
| content_key_specs <br/> required_output_protection.hdc |dize, HDCP_NONE biri, HDCP_V1, HDCP_V2 |HDCP'nin gerekli olup olmadığını gösterir. |
| content_key_specs <br/>anahtar |Taban64-<br/>kodlanmış dize |Bu parça için kullanılacak içerik anahtarı. Belirtilirse, track_type veya key_id gereklidir. İçerik sağlayıcısı, Widevine lisans sunucusunun bir anahtar oluşturmasına veya aramasına izin vermek yerine bu parçanın içerik anahtarını enjekte etmek için bu seçeneği kullanabilir. |
| content_key_specs.key_id |Base64 kodlanmış dize ikilisi, 16 bayt |Anahtar için benzersiz tanımlayıcı. |

## <a name="policy-overrides"></a>İlke geçersiz kılar
| Adı | Değer | Açıklama |
| --- | --- | --- |
| policy_overrides. can_play |Boolean, doğru veya yanlış |İçeriğin oynatıncaya izin verildiğini gösterir. Varsayılan değer false’tur. |
| policy_overrides. can_persist |Boolean, doğru veya yanlış |Lisansın çevrimdışı kullanım için geçici olmayan depolamada kalıcı olabileceğini gösterir. Varsayılan değer false’tur. |
| policy_overrides. can_renew |Boolean, doğru veya yanlış |Bu lisansın yenilenmesine izin verildiğini gösterir. Doğruysa, lisansSüresi kalp atışı ile uzatılabilir. Varsayılan değer false’tur. |
| policy_overrides. license_duration_seconds |int64 |Bu özel lisansın zaman penceresini gösterir. 0 değeri, süre için bir sınır olmadığını gösterir. Varsayılan değer 0'dır. |
| policy_overrides. rental_duration_seconds |int64 |Oynatma yada izin verilirken zaman penceresini gösterir. 0 değeri, süre için bir sınır olmadığını gösterir. Varsayılan değer 0'dır. |
| policy_overrides. playback_duration_seconds |int64 |Oynatmadan sonraki sürenin görüntüleme penceresi lisans süresi içinde başlar. 0 değeri, süre için bir sınır olmadığını gösterir. Varsayılan değer 0'dır. |
| policy_overrides. renewal_server_url |string |Bu lisans için tüm sinyal (yenileme) istekleri belirtilen URL'ye yönlendirilir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides. renewal_delay_seconds |int64 |Yenilemeden önce license_start_time kaç saniye sonra ilk denenir. Bu alan yalnızca can_renew doğruysa kullanılır. Varsayılan değer 0'dır. |
| policy_overrides. renewal_retry_interval_seconds |int64 |Arıza durumunda, sonraki lisans yenileme istekleri arasındaki gecikmeyi saniye cinsinden belirtir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides. renewal_recovery_duration_seconds |int64 |Yenileme denenirken oynatmanın devam edebileceği, ancak lisans sunucusundaki arka uç sorunları nedeniyle başarısız olduğu zaman penceresi. 0 değeri, süre için bir sınır olmadığını gösterir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides. renew_with_usage |Boolean, doğru veya yanlış |Kullanım başladığında lisansın yenileme için gönderildiğini gösterir. Bu alan yalnızca can_renew doğruysa kullanılır. |

## <a name="session-initialization"></a>Oturum başlatma
| Adı | Değer | Açıklama |
| --- | --- | --- |
| provider_session_token |Base64 kodlanmış dize |Bu oturum belirteci lisansta geri geçirilir ve sonraki yenilemelerde bulunur. Oturum belirteci oturumların ötesinde devam etmez. |
| provider_client_token |Base64 kodlanmış dize |İstemci, lisans yanıtını geri göndermek için belirteç. Lisans isteği bir istemci belirteci içeriyorsa, bu değer yoksayılır. İstemci belirteci lisans oturumları ötesinde devam eder. |
| override_provider_client_token |Boolean, doğru veya yanlış |Yanlış ve lisans isteği bir istemci belirteci içeriyorsa, bu yapıda bir istemci belirteci belirtilmiş olsa bile istekten belirteci kullanın. Doğruysa, her zaman bu yapıda belirtilen belirteci kullanın. |

## <a name="configure-your-widevine-licenses-by-using-net-types"></a>.NET türlerini kullanarak Widevine lisanslarınızı yapılandırın
Medya Hizmetleri, Widevine lisanslarınızı yapılandırmak için kullanabileceğiniz .NET API'leri sağlar. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Medya Hizmetleri .NET SDK'da tanımlanan sınıflar
Aşağıdaki sınıflar bu tür tanımları şunlardır:

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Örnek
Aşağıdaki örnek, basit bir Widevine lisansını yapılandırmak için .NET API'lerin nasıl kullanılacağını gösterir:

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma](media-services-protect-with-playready-widevine.md)

