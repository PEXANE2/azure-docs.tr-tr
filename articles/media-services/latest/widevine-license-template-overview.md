---
title: Widevine lisans şablonuna genel bakış ile Azure Media Services v3
description: Bu konu, Widevine lisanslarını yapılandırmak için kullanılan Widevine lisans şablonuna genel bir bakış sağlar.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 94ce5e45a9a43e81020096ddc0a67429b286d9b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705641"
---
# <a name="media-services-v3-with-widevine-license-template-overview"></a>Widevine lisans şablonuna genel bakış ile Medya Hizmetleri v3

Azure Medya Hizmetleri, içeriğinizi **Google Widevine**ile şifrelemenizi sağlar. Medya Hizmetleri ayrıca Widevine lisansları sunmak için bir hizmet sunmaktadır. Widevine lisanslarını yapılandırmak için Azure Medya Hizmetleri API'lerini kullanabilirsiniz. Bir oyuncu Widevine korumalı içeriğinizi oynatmaya çalıştığında, lisansı almak için lisans teslim hizmetine bir istek gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. İstemciye gönderilir ve belirtilen içeriğin şifresini çözmek ve oynatmak için kullanılır.

Widevine lisans isteği JSON iletisi olarak biçimlendirilir.  

>[!NOTE]
> Hiçbir değeri olmayan boş bir ileti{}oluşturabilirsiniz, sadece " dedi. Ardından varsayılanlarla bir lisans şablonu oluşturulur. Varsayılan çoğu servis talebi için çalışır. Microsoft tabanlı lisans teslim senaryoları her zaman varsayılanları kullanmalıdır. "Sağlayıcı" ve "content_id" değerlerini ayarlamanız gerekiyorsa, sağlayıcının Widevine kimlik bilgileriyle eşleşmesi gerekir.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage":<renew with usage>
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
Önceden varolan bir ilke varsa, içerik anahtarı spec değerleri herhangi bir belirtmeye gerek yoktur. Yüksek bant genişliğine sahip Dijital İçerik Koruması (HDCP) ve Copy General Management System (CGMS) gibi çıktı korumasını belirlemek için bu içerikle ilişkili önceden varolan ilke kullanılır. Önceden varolan bir ilke Widevine lisans sunucusuna kayıtlı değilse, içerik sağlayıcısı değerleri lisans isteğine enjekte edebilir.   

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
| policy_overrides&#46;can_play |Boolean, doğru veya yanlış |İçeriğin oynatıncaya izin verildiğini gösterir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_persist |Boolean, doğru veya yanlış |Lisansın çevrimdışı kullanım için geçici olmayan depolamada kalıcı olabileceğini gösterir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_renew |Boolean, doğru veya yanlış |Bu lisansın yenilenmesine izin verildiğini gösterir. Doğruysa, lisansSüresi kalp atışı ile uzatılabilir. Varsayılan değer false’tur. |
| policy_overrides&#46;license_duration_seconds |int64 |Bu özel lisansın zaman penceresini gösterir. 0 değeri, süre için bir sınır olmadığını gösterir. Varsayılan değer 0'dır. |
| rental_duration_seconds&#46;policy_overrides |int64 |Oynatma yada izin verilirken zaman penceresini gösterir. 0 değeri, süre için bir sınır olmadığını gösterir. Varsayılan değer 0'dır. |
| policy_overrides&#46;playback_duration_seconds |int64 |Oynatmadan sonraki sürenin görüntüleme penceresi lisans süresi içinde başlar. 0 değeri, süre için bir sınır olmadığını gösterir. Varsayılan değer 0'dır. |
| policy_overrides&#46;renewal_server_url |string |Bu lisans için tüm sinyal (yenileme) istekleri belirtilen URL'ye yönlendirilir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides&#46;renewal_delay_seconds |int64 |Yenilemeden önce license_start_time kaç saniye sonra ilk denenir. Bu alan yalnızca can_renew doğruysa kullanılır. Varsayılan değer 0'dır. |
| policy_overrides&#46;renewal_retry_interval_seconds |int64 |Arıza durumunda, sonraki lisans yenileme istekleri arasındaki gecikmeyi saniye cinsinden belirtir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides&#46;renewal_recovery_duration_seconds |int64 |Yenileme denenirken oynatmanın devam edebileceği, ancak lisans sunucusundaki arka uç sorunları nedeniyle başarısız olduğu zaman penceresi. 0 değeri, süre için bir sınır olmadığını gösterir. Bu alan yalnızca can_renew doğruysa kullanılır. |
| policy_overrides&#46;renew_with_usage |Boolean, doğru veya yanlış |Kullanım başladığında lisansın yenileme için gönderildiğini gösterir. Bu alan yalnızca can_renew doğruysa kullanılır. |

## <a name="session-initialization"></a>Oturum başlatma
| Adı | Değer | Açıklama |
| --- | --- | --- |
| provider_session_token |Base64 kodlanmış dize |Bu oturum belirteci lisansta geri geçirilir ve sonraki yenilemelerde bulunur. Oturum belirteci oturumların ötesinde devam etmez. |
| provider_client_token |Base64 kodlanmış dize |İstemci, lisans yanıtını geri göndermek için belirteç. Lisans isteği bir istemci belirteci içeriyorsa, bu değer yoksayılır. İstemci belirteci lisans oturumları ötesinde devam eder. |
| override_provider_client_token |Boolean, doğru veya yanlış |Yanlış ve lisans isteği bir istemci belirteci içeriyorsa, bu yapıda bir istemci belirteci belirtilmiş olsa bile istekten belirteci kullanın. Doğruysa, her zaman bu yapıda belirtilen belirteci kullanın. |

## <a name="configure-your-widevine-license-with-net"></a>Widevine lisansınızı .NET ile yapılandırın 

Medya Hizmetleri, Widevine lisansLarını yapılandırmanıza olanak tanıyan bir sınıf sağlar. Lisans oluşturmak için, [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate)Için JSON geçmek.

Şablonu yapılandırmak için şunları yapabilirsiniz:

### <a name="directly-construct-a-json-string"></a>Doğrudan bir JSON dizesi oluşturmak

Bu yöntem hataya yatkın olabilir. [Gerekli sınıfları tanımla ve JSON'a serihale olarak](#classes)açıklanan diğer yöntemin kullanılması önerilir.

```csharp
ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
{
    WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
};
```

### <a name="define-needed-classes-and-serialize-to-json"></a><a id="classes"></a>Gerekli sınıfları tanımlayın ve JSON'a serileştirin

#### <a name="define-classes"></a>Sınıfları tanımlama

Aşağıdaki örnek, Widevine JSON şemasına eşleyen sınıfların tanımlarının bir örneğini gösterir. JSON dizesine serileştirmeden önce sınıfları anında atabilirsiniz.  

```csharp
public class PolicyOverrides
{
    public bool CanPlay { get; set; }
    public bool CanPersist { get; set; }
    public bool CanRenew { get; set; }
    public int RentalDurationSeconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
    public int PlaybackDurationSeconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
    public int LicenseDurationSeconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
}

public class ContentKeySpec
{
    public string TrackType { get; set; }
    public int SecurityLevel { get; set; }
    public OutputProtection RequiredOutputProtection { get; set; }
}

public class OutputProtection
{
    public string HDCP { get; set; }
}

public class WidevineTemplate
{
    public string AllowedTrackTypes { get; set; }
    public ContentKeySpec[] ContentKeySpecs { get; set; }
    public PolicyOverrides PolicyOverrides { get; set; }
}
```

#### <a name="configure-the-license"></a>Lisansı yapılandırma

[WidevineTemplate'i](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate)yapılandırmak için kullanılan JSON oluşturmak için önceki bölümde tanımlanan sınıfları kullanın:

```csharp
private static ContentKeyPolicyWidevineConfiguration ConfigureWidevineLicenseTempate()
{
    WidevineTemplate template = new WidevineTemplate()
    {
        AllowedTrackTypes = "SD_HD",
        ContentKeySpecs = new ContentKeySpec[]
        {
            new ContentKeySpec()
            {
                TrackType = "SD",
                SecurityLevel = 1,
                RequiredOutputProtection = new OutputProtection()
                {
                    HDCP = "HDCP_V2"
                }
            }
        },
        PolicyOverrides = new PolicyOverrides()
        {
            CanPlay = true,
            CanPersist = true,
            CanRenew = false,
            RentalDurationSeconds = 2592000,
            PlaybackDurationSeconds = 10800,
            LicenseDurationSeconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(template)
    };
    return objContentKeyPolicyWidevineConfiguration;
}
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar

DRM ile nasıl [korunacaklarına](protect-with-drm.md) göz atın
