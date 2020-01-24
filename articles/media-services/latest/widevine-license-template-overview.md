---
title: Wıdevine lisans şablonuna genel bakış ile Azure Media Services v3
description: Bu konu, Widevine lisanslarını yapılandırmak için kullanılan bir Widevine lisans şablonuna genel bakış sunar.
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
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705641"
---
# <a name="media-services-v3-with-widevine-license-template-overview"></a>Wıdevine lisans şablonuna genel bakış ile Media Services v3

Azure Media Services **Google Widevine**ile içeriğinizi şifrelemenizi sağlar. Media Services Ayrıca Widevine lisansları teslim etmek için bir hizmet sağlar. Wıdevine lisanslarını yapılandırmak için Azure Media Services API 'Leri kullanabilirsiniz. Bir oyuncu Widevine korumalı içeriğinizi yürütmeye çalıştığında, lisans almak için lisans teslim hizmetine bir istek gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. Bu, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriğin yürütmek için kullanılır.

Widevine lisans isteği JSON iletisi olarak biçimlendirilir.  

>[!NOTE]
> Değer olmadan boş bir ileti oluşturabilirsiniz, yalnızca "{}." Ardından, varsayılan olarak bir lisans şablonu oluşturulur. Çoğu durumda varsayılan değer kullanılır. Microsoft tabanlı lisans-teslim senaryoları her zaman varsayılan değerleri kullanmalıdır. "Sağlayıcı" ve "content_id" değerlerini ayarlamanız gerekiyorsa, bir sağlayıcının Wıdevine kimlik bilgileriyle eşleşmesi gerekir.

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

| Ad | Değer | Açıklama |
| --- | --- | --- |
| te |Base64 ile kodlanmış dize |İstemci tarafından gönderilen lisans isteği. |
| content_id |Base64 ile kodlanmış dize |Her bir content_key_specs için anahtar KIMLIĞINI ve içerik anahtarını türetmede kullanılan tanımlayıcı. track_type. |
| sağlayıcısı |string |İçerik anahtarları ve ilkeleri aramak için kullanılır. Widevine lisans teslimi için Microsoft anahtar teslimi kullanılıyorsa, bu parametre yoksayılır. |
| policy_name |string |Daha önce kaydedilen bir ilkenin adı. İsteğe bağlı. |
| allowed_track_types |enum |SD_ONLY veya SD_HD. Bir lisansa hangi içerik anahtarlarının dahil edileceğini denetler. |
| content_key_specs |JSON yapıları dizisi, "Içerik anahtar özellikleri" bölümüne bakın.  |Hangi içerik anahtarlarının dönebileceği üzerinde daha ayrıntılı bir denetim. Daha fazla bilgi için "Içerik anahtarı özellikleri" bölümüne bakın. Allowed_track_types ve content_key_specs değerlerinden yalnızca biri belirtilebilir. |
| use_policy_overrides_exclusively |Boolean, true veya false |Policy_overrides tarafından belirtilen ilke özniteliklerini kullanın ve daha önce depolanan tüm ilkeleri atlayın. |
| policy_overrides |JSON yapısı, "Ilke geçersiz kılmaları" bölümüne bakın. |Bu lisansın ilke ayarları.  Bu varlığın önceden tanımlanmış bir ilkesi varsa, belirtilen değerler kullanılır. |
| session_init |JSON yapısı, "oturum başlatma" bölümüne bakın. |İsteğe bağlı veriler lisansa geçirilir. |
| parse_only |Boolean, true veya false |Lisans isteği ayrıştırılır, ancak lisans verilmez. Ancak, lisans isteğinden alınan değerler yanıtta döndürülür. |

## <a name="content-key-specs"></a>İçerik anahtarı özellikleri
Önceden var olan bir ilke varsa, içerik anahtarı belirtiminde bir değer belirtmeniz gerekmez. Bu içerikle ilişkili önceden varolan ilke, yüksek bant genişliği dijital Content Protection (HDCP) ve kopyalama genel yönetim sistemi (CGMS) gibi çıkış korumasını belirlemede kullanılır. Önceden var olan bir ilke Widevine lisans sunucusuna kayıtlı değilse, içerik sağlayıcısı değerleri lisans isteğine ekleyebilir.   

Her bir content_key_specs değeri, use_policy_overrides_exclusively seçeneğinden bağımsız olarak tüm parçalar için belirtilmelidir. 

| Ad | Değer | Açıklama |
| --- | --- | --- |
| content_key_specs. track_type |string |Bir izleme türü adı. Lisans isteğinde content_key_specs belirtilmişse, tüm izleme türlerini açık olarak belirttiğinizden emin olun. Bunun yapılmaması, son 10 saniye içinde oynatılmasına neden olur. |
| content_key_specs  <br/> security_level |Int32 |Kayıttan yürütme için istemci sağlamlık gereksinimlerini tanımlar. <br/> -Yazılım tabanlı beyaz kutu şifrelemesi gereklidir. <br/> -Yazılım şifrelemesi ve karıştırılmış bir kod çözücü gereklidir. <br/> -Anahtar malzeme ve şifreleme işlemlerinin, donanım ile desteklenen bir güvenilir yürütme ortamında gerçekleştirilmesi gerekir. <br/> -İçerik şifrelemesi ve kodunun çözülmesi, donanım tarafından desteklenen bir güvenilir yürütme ortamında gerçekleştirilmelidir.  <br/> -Şifreleme, kod çözme ve medyanın tüm işlenmesi (sıkıştırılmış ve sıkıştırılmamış), donanım tarafından desteklenen bir güvenilir yürütme ortamında işlenmelidir. |
| content_key_specs <br/> required_output_protection. HDC |dize, biri HDCP_NONE, HDCP_V1 HDCP_V2 |HDCP gerekip gerekmediğini gösterir. |
| content_key_specs <br/>anahtar |Biçiminde<br/>kodlanmış dize |Bu izleme için kullanılacak içerik anahtarı. Belirtilmişse track_type veya key_id gereklidir. İçerik sağlayıcısı, Widevine lisans sunucusunun bir anahtar oluşturmasını veya arama yapmasına izin vermek yerine bu izleme için içerik anahtarını eklemek için bu seçeneği kullanabilir. |
| content_key_specs. key_id |Base64 kodlamalı dize ikili, 16 bayt |Anahtar için benzersiz tanımlayıcı. |

## <a name="policy-overrides"></a>İlke geçersiz kılmaları
| Ad | Değer | Açıklama |
| --- | --- | --- |
| policy_overrides&#46;can_play |Boolean, true veya false |İçeriğin kayıttan yürütmeye izin verildiğini belirtir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_persist |Boolean, true veya false |Çevrimdışı kullanım için lisansın kalıcı depolamaya kalıcı olabileceğini belirtir. Varsayılan değer false’tur. |
| policy_overrides&#46;can_renew |Boolean, true veya false |Bu lisansın yenilenmesini izin verildiğini belirtir. Doğru ise, Lisansın süresi sinyal ile genişletilebilir. Varsayılan değer false’tur. |
| policy_overrides&#46;license_duration_seconds |tutulamaz |Bu özel lisansın zaman penceresini gösterir. 0 değeri, Duration için bir sınır olmadığını gösterir. Varsayılan değer 0 ' dır. |
| policy_overrides&#46;rental_duration_seconds |tutulamaz |Kayıttan yürütmeye izin verildiğinde zaman penceresini gösterir. 0 değeri, Duration için bir sınır olmadığını gösterir. Varsayılan değer 0 ' dır. |
| policy_overrides&#46;playback_duration_seconds |tutulamaz |Kayıttan yürütme sonrasında zaman içindeki görüntüleme penceresi lisans süresi içinde başlar. 0 değeri, Duration için bir sınır olmadığını gösterir. Varsayılan değer 0 ' dır. |
| policy_overrides&#46;renewal_server_url |string |Bu lisans için tüm sinyal (yenileme) istekleri belirtilen URL 'ye yönlendirilir. Bu alan yalnızca can_renew true ise kullanılır. |
| policy_overrides&#46;renewal_delay_seconds |tutulamaz |Yenileme ilk denendikten sonra license_start_time kaç saniye sonra. Bu alan yalnızca can_renew true ise kullanılır. Varsayılan değer 0 ' dır. |
| policy_overrides&#46;renewal_retry_interval_seconds |tutulamaz |Hata durumunda sonraki lisans yenileme istekleri arasındaki gecikme süresini saniye cinsinden belirtir. Bu alan yalnızca can_renew true ise kullanılır. |
| policy_overrides&#46;renewal_recovery_duration_seconds |tutulamaz |Yenileme denenirken kayıttan yürütmenin devam edebildiği zaman penceresi, ancak lisans sunucusu ile ilgili arka uç sorunları nedeniyle başarısız oldu. 0 değeri, Duration için bir sınır olmadığını gösterir. Bu alan yalnızca can_renew true ise kullanılır. |
| policy_overrides&#46;renew_with_usage |Boolean, true veya false |Kullanım başladığında lisansın yenileme için gönderileceğini belirtir. Bu alan yalnızca can_renew true ise kullanılır. |

## <a name="session-initialization"></a>Oturum başlatma
| Ad | Değer | Açıklama |
| --- | --- | --- |
| provider_session_token |Base64 ile kodlanmış dize |Bu oturum belirteci lisansa geri geçirilir ve sonraki yenilemelerde bulunur. Oturum belirteci oturumlardan daha fazla kalıcı yapmaz. |
| provider_client_token |Base64 ile kodlanmış dize |Lisans yanıtına geri göndermek için istemci belirteci. Lisans isteği bir istemci belirteci içeriyorsa, bu değer yoksayılır. İstemci belirteci, lisans oturumlarının ötesinde devam ettirir. |
| override_provider_client_token |Boolean, true veya false |Yanlış ise ve lisans isteği bir istemci belirteci içeriyorsa, bu yapıda bir istemci belirteci belirtilmiş olsa bile istekten belirteci kullanın. True ise, her zaman bu yapıda belirtilen belirteci kullanın. |

## <a name="configure-your-widevine-license-with-net"></a>.NET ile Widevine lisansınızı yapılandırma 

Media Services Widevine lisansı yapılandırmanıza imkan tanıyan bir sınıf sağlar. Lisansı oluşturmak için JSON 'u [Widevinetemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate)'e geçirin.

Şablonu yapılandırmak için şunları yapabilirsiniz:

### <a name="directly-construct-a-json-string"></a>Doğrudan bir JSON dizesi oluşturun

Bu yöntem hata durumunda olabilir. [Gerekli sınıfları tanımlama ve JSON için serileştirme](#classes)bölümünde açıklanan diğer yöntemi kullanmanız önerilir.

```csharp
ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
{
    WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
};
```

### <a id="classes"></a>Gerekli sınıfları tanımlayın ve JSON 'a serileştirin

#### <a name="define-classes"></a>Sınıfları tanımlama

Aşağıdaki örnek, Widevine JSON şemasına eşlenen sınıfların tanımlarının bir örneğini gösterir. Sınıfları JSON dizesinde serileştirmek için bir örneği oluşturabilirsiniz.  

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

[Widevınetemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate)'i yapılandırmak IÇIN kullanılan JSON oluşturmak için önceki bölümde tanımlanan sınıfları kullanın:

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

* Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="next-steps"></a>Sonraki adımlar

İşlemlerini nasıl gerçekleştirebileceğinizi [DRM ile koruma](protect-with-drm.md)
