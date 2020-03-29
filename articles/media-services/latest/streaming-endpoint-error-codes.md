---
title: Azure Medya Hizmetleri paketleme ve kaynak hataları | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri Akış Bitiş Noktası (Orgin) hizmetinden alabileceğiniz hataları açıklar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: juliako
ms.openlocfilehash: ebcda6026f79bc88df91471d8be88316ba57bfc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65411384"
---
# <a name="streaming-endpoint-origin-errors"></a>Akış Bitiş Noktası (Başlangıç) hataları 

Bu konu, Azure Medya Hizmetleri Akış [Bitiş Noktası hizmetinden](streaming-endpoint-concept.md)alabileceğiniz hataları açıklar.

## <a name="400-bad-request"></a>400 Kötü İstek

İstek geçersiz bilgiler içerir ve bu hata kodları ile ve aşağıdaki nedenlerden biri nedeniyle reddedilir:

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_BAD_URL_SYNTAX |0x80890201|BIR URL sözdizimi veya biçim hatası. Örnekler geçersiz bir tür, geçersiz bir parça veya geçersiz bir parça için istekleri içerir. |
|MPE_ENC_ENCRYPTION_NOT_SPECIFIED_IN_URL |0x8088024C|İstek URL'de şifreleme etiketi yoktur. CMAF istekleri, URL'de bir şifreleme etiketi gerektirir. Birden fazla şifreleme türüyle yapılandırılan diğer protokoller de farklılaşma için şifreleme etiketi gerektirir. |
|MPE_STORAGE_BAD_URL_SYNTAX |0x808900E9|İsteği yerine getirmek için depolama isteği bir Hatalı İstek hatası ile başarısız oldu. |

## <a name="403-forbidden"></a>403 Yasak

Aşağıdaki nedenlerden biri nedeniyle istek kabul edilmez:

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_AUTHENTICATION_FAILED |0x808900EA|İsteği yerine getirmek için depolama isteği kimlik doğrulama hatasıyla birlikte başarısız oldu. Depolama anahtarları döndürülürse ve hizmet depolama anahtarlarını eşitleyemiyorsa, bu durum olabilir. <br/><br/>Azure portalında [Yardım + desteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) giderek Azure desteğine başvurun.|
|MPE_STORAGE_INSUFFICIENT_ACCOUNT_PERMISSIONS |0x808900EB |Depolama İşlemi hatası, yetersiz Hesap İzinleri nedeniyle erişim başarısız oldu. |
|MPE_STORAGE_ACCOUNT_IS_DISABLED |0x808900EC |Depolama hesabı Devre Dışı bırakıldığından, isteği yerine getirmek için depolama isteği başarısız oldu. |
|MPE_STORAGE_AUTHENTICATION_FAILURE |0x808900F3 |Depolama İşlemi hatası, genel hatalar nedeniyle erişim başarısız oldu. |
|MPE_OUTPUT_FORMAT_BLOCKED |0x80890207 |Akış Politikası'ndaki yapılandırma nedeniyle çıktı biçimi engellenir. |
|MPE_ENC_ENCRYPTION_REQUIRED |0x8088021E |İçerik için şifreleme gereklidir, çıkış biçimi için teslim ilkesi gereklidir. |
|MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY |0x8088024D |Şifreleme, teslim ilkesi ayarlarında ayarlanmaz. |

## <a name="404-not-found"></a>404 Bulunamadı

İşlem, artık var olmayan bir kaynak üzerinde hareket etmeye çalışıyor. Örneğin, kaynak zaten silinmiş olabilir.

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_EGRESS_TRACK_NOT_FOUND |0x80890209 |İstenen parça bulunamadı. |
|MPE_RESOURCE_NOT_FOUND |0x808901F9 |İstenen kaynak bulunamadı. |
|MPE_UNAUTHORIZED |0x80890244 |Erişim yetkisiz. |
|MPE_EGRESS_TIMESTAMP_NOT_FOUND |0x8089020A |İstenen zaman damgası bulunamadı. |
|MPE_EGRESS_FILTER_NOT_FOUND |0x8089020C |İstenen dinamik bildirim filtresi bulunamadı. |
|MPE_FRAGMENT_BY_INDEX_NOT_FOUND |0x80890252 |İstenen parça dizini geçerli aralığın dışındadır. |
|MPE_LIVE_MEDIA_ENTRIES_NOT_FOUND |0x80890254 |Moov arabelleği almak için canlı ortam girişleri bulunamıyor. |
|MPE_FRAGMENT_TIMESTAMP_NOT_FOUND |0x80890255 |Belirli bir parça için istenen zamanda parçayı bulamıyor.<br/><br/>Parça depoda değil olabilir. Sunuyu farklı bir katmanda bir parça olabilir deneyin. |
|MPE_MANIFEST_MEDIA_ENTRY_NOT_FOUND |0x80890256 |Bildirimde istenen bitrate için ortam girişi bulunamıyor. <br/><br/>Oynatıcı, manifestoda olmayan belirli bir bitatın video parçasını istemişimdir.|
|MPE_METADATA_NOT_FOUND |0x80890257 |Bildirimde belirli meta verileri bulamamış veya depolama alanından rebase bulamıyor. |
|MPE_STORAGE_RESOURCE_NOT_FOUND |0x808900ED |Depolama İşlemi hatası, kaynak bulunamadı. |

## <a name="409-conflict"></a>409 Çatışma

Bir `PUT` veya `POST` işlem de bir kaynak için sağlanan kimlik varolan bir kaynak tarafından alınmıştır. Bu sorunu gidermek için kaynak için başka bir kimlik kullanın.

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_CONFLICT  |0x808900EE  |Depolama İşlemi hatası, çakışma hatası.  |

## <a name="410"></a>410

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_FILTER_FORCE_END_LEFT_EDGE_CROSSED_DVR_WINDOW|0x80890263|Canlı akış için, zorla Son Zaman Damgası'na sahip filtre doğru ayarlandığında, başlangıç veya bitiş zaman damgası geçerli DVR penceresinin dışındadır.|

## <a name="412-precondition-failure"></a>412 Ön Koşul Hatası

İşlem, sunucuda bulunan sürümden farklı bir eTag, yani iyimser bir eşzamanlılık hatası belirtdi. Kaynağın en son sürümünü okuduktan ve istek üzerine eTag'ı güncelledikten sonra isteği yeniden deneyin.

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_FRAGMENT_NOT_READY |0x80890200 |İstenen parça hazır değil.|
|MPE_STORAGE_PRECONDITION_FAILED| 0x808900EF|Depolama işlemi hatası, ön koşul hatası.|

## <a name="415-unsupported-media-type"></a>415 Desteklenmeyen Ortam Türü

İstemci tarafından gönderilen yük biçimi desteklenmeyen bir biçimdedir.

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_ENC_ALREADY_ENCRYPTED| 0x8088021F| Zaten şifrelenmiş içeriğe şifreleme uygulamamalıdır.|
|MPE_ENC_INVALID_INPUT_ENCRYPTION_FORMAT|0x8088021D |Şifreleme giriş biçimi için geçersizdir.|
|MPE_INVALID_ASSET_DELIVERY_POLICY_TYPE|0x8088021C| Teslim ilkesi türü geçersizdir.|
|MPE_ENC_MULTIPLE_SAME_DELIVERY_TYPE|0x8088024E |Özgün ayarlar birden çok çıktı biçimi tarafından paylaşılabilir.|
|MPE_FORMAT_NOT_SUPPORTED|0x80890205|Ortam biçimi veya türü desteklenmez. Örneğin, Medya Hizmetleri 64'ün üzerindeki kalite düzeyi sayısını desteklemez. FLV video etiketinde, Medya Hizmetleri birden fazla SPS ve birden fazla PPS içeren bir video çerçeveyi desteklemez.|
|MPE_INPUT_FORMAT_NOT_SUPPORTED|0x80890218| İstenen varlığın giriş biçimi desteklenmez. Medya Hizmetleri Düzgün (canlı), MP4 (VoD) ve Aşamalı indirme biçimlerini destekler.|
|MPE_OUTPUT_FORMAT_NOT_SUPPORTED|0x8089020D|İstenen çıktı biçimi desteklenmez. Medya Hizmetleri, Smooth, DASH(CSF, CMAF), HLS (v3, v4, CMAF) ve Progressive indirme biçimlerini destekler.|
|MPE_ENCRYPTION_NOT_SUPPORTED|0x80890208|Desteklenmeyen şifreleme türüyle karşılaştı.|
|MPE_MEDIA_TYPE_NOT_SUPPORTED|0x8089020E|İstenen ortam türü çıktı biçimi tarafından desteklenmez. Desteklenen türleri video, ses veya "SUBT" altyazı vardır.|
|MPE_MEDIA_ENCODING_NOT_SUPPORTED|0x8089020F|Kaynak varlık ortamı, çıktı biçimiyle uyumlu olmayan bir ortam biçimiyle kodlandı.|
|MPE_VIDEO_ENCODING_NOT_SUPPORTED|0x80890210|Kaynak varlık, çıktı biçimiyle uyumlu olmayan bir video biçimiyle kodlandı. H.264, AVC, H.265 (HEVC, hev1 veya hvc1) desteklenir.|
|MPE_AUDIO_ENCODING_NOT_SUPPORTED|0x80890211|Kaynak varlık, çıktı biçimiyle uyumlu olmayan bir ses biçimiyle kodlandı. Desteklenen ses biçimleri AAC, E-AC3 (DD+), Dolby DTS'dir.|
|MPE_SOURCE_PROTECTION_CONVERSION_NOT_SUPPORTED|0x80890212|Kaynak korumalı varlık çıktı biçimine dönüştürülemez.|
|MPE_OUTPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890213|Koruma biçimi çıktı biçimi tarafından desteklenmez.|
|MPE_INPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890219|Koruma biçimi giriş biçimi tarafından desteklenmez.|
|MPE_INVALID_VIDEO_NAL_UNIT|0x80890231|Geçersiz video NAL birimi, örneğin, yalnızca örnekteki ilk NAL AUD olabilir.|
|MPE_INVALID_NALU_SIZE|0x80890260|Geçersiz NAL birim boyutu.|
|MPE_INVALID_NALU_LENGTH_FIELD|0x80890261|Geçersiz NAL birim uzunluk değeri.|
|MPE_FILTER_INVALID|0x80890236|Geçersiz dinamik bildirim filtreleri.|
|MPE_FILTER_VERSION_INVALID|0x80890237|Geçersiz veya desteklenmeyen filtre sürümleri.|
|MPE_FILTER_TYPE_INVALID|0x80890238|Geçersiz filtre türü.|
|MPE_FILTER_RANGE_ATTRIBUTE_INVALID|0x80890239|Geçersiz aralık filtre tarafından belirtilir.|
|MPE_FILTER_TRACK_ATTRIBUTE_INVALID|0x8089023A|Geçersiz parça özniteliği filtre tarafından belirtilir.|
|MPE_FILTER_PRESENTATION_WINDOW_INVALID|0x8089023B|Geçersiz sunu penceresi uzunluğu filtre tarafından belirtilir.|
|MPE_FILTER_LIVE_BACKOFF_INVALID|0x8089023C|Geçersiz canlı geri dönüş filtre tarafından belirtilir.|
|MPE_FILTER_MULTIPLE_SAME_TYPE_FILTERS|0x8089023D|Eski filtrelerde yalnızca bir absTimeInHNS öğesi desteklenir.|
|MPE_FILTER_REMOVED_ALL_STREAMS|0x8089023E|Filtreler uygulandıktan sonra artık akış yok.|
|MPE_FILTER_LIVE_BACKOFF_OVER_DVRWINDOW|0x8089023F|Canlı geri dönüş DVR penceresinin ötesinde.|
|MPE_FILTER_LIVE_BACKOFF_OVER_PRESENTATION_WINDOW|0x80890262|Canlı geri dönüş, sunu penceresinden daha büyüktür.|
|MPE_FILTER_COMPOSITION_FILTER_COUNT_OVER_LIMIT|0x80890246|İzin verilen en fazla on (10) varsayılan filtreyi aştı.|
|MPE_FILTER_COMPOSITION_MULTIPLE_FIRST_QUALITY_OPERATOR_NOT_ALLOWED|0x80890248|Birleştirilmiş istek filtrelerinde birden çok ilk video kalitesi operatörüne izin verilmez.|
|MPE_FILTER_FIRST_QUALITY_ATTRIBUTE_INVALID|0x80890249|Birinci kalite bitrate özniteliklerinin sayısı bir olmalıdır (1).|
|MPE_HLS_SEGMENT_TOO_LARGE|0x80890243|HLS segment süresi DVR penceresinin üçte birinden daha küçük olmalı ve HLS geri çekilmelidir.|
|MPE_KEY_FRAME_INTERVAL_TOO_LARGE|0x808901FE|Parça süreleri yaklaşık 20 saniyeden az veya eşit olmalıdır veya giriş kalitesi düzeyleri zaman hizalanmaz.|
|MPE_DTS_RESERVEDBOX_EXPECTED|0x80890105|DTS'ye özgü hata, DTS kutusunu ayrıştma sırasında DTSSpecficBox'ta bulunması gerektiğinde ReservedBox'ı bulamaz.|
|MPE_DTS_INVALID_CHANNEL_COUNT|0x80890106|DTS'ye özgü hata, DTS kutusunu ayrıştma sırasında DTSSpecficBox'ta kanal bulunmaması.|
|MPE_DTS_SAMPLETYPE_MISMATCH|0x80890107|DTS'ye özgü hata, DTSSpecficBox'ta örnek türü uyuşmazlığı.|
|MPE_DTS_MULTIASSET_DTSH_MISMATCH|0x80890108|DTS'ye özgü hata, çoklu varlık ayarlanır ancak DTSH örnek türü uyuşmazlığı.|
|MPE_DTS_INVALID_CORESTREAM_SIZE|0x80890109|DTS'ye özgü hata, çekirdek akışı boyutu geçersizdir.|
|MPE_DTS_INVALID_SAMPLE_RESOLUTION|0x8089010A|DTS'ye özgü hata, örnek çözümleme geçersizdir.|
|MPE_DTS_INVALID_SUBSTREAM_INDEX|0x8089010B|DTS'ye özgü hata, alt akış uzantısı dizini geçersizdir.|
|MPE_DTS_INVALID_BLOCK_NUM|0x8089010C|DTS'ye özgü hata, alt akış blok numarası geçersizdir.|
|MPE_DTS_INVALID_SAMPLING_FREQUENCE|0x8089010D|DTS'ye özgü hata, örnekleme sıklığı geçersizdir.|
|MPE_DTS_INVALID_REFCLOCKCODE|0x8089010E|DTS'ye özgü hata, alt akış uzantısındaki başvuru saati kodu geçersizdir.|
|MPE_DTS_INVALID_SPEAKERS_REMAP|0x8089010F|DTS'ye özgü hata, hoparlör sayısı yeniden eşlemi kümesi geçersizdir.|

Şifreleme makaleleri ve örnekleri için bkz:

- [Kavram: içerik koruması](content-protection-overview.md)
- [Kavram: İçerik Anahtar İlkeleri](content-key-policy-concept.md)
- [Kavram: Akış İlkeleri](streaming-policy-concept.md)
- [Örnek: AES şifreleme ile koruyun](protect-with-aes128.md)
- [Örnek: DRM ile koruyun](protect-with-drm.md)

Filtre kılavuzu için bkz:

- [Kavram: dinamik manifestolar](filters-dynamic-manifest-overview.md)
- [Konsept: filtreler](filters-concept.md)
- [Örnek: REST API'leri ile filtreler oluşturma](filters-dynamic-manifest-rest-howto.md)
- [Örnek: .NET ile filtreler oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [Örnek: CLI ile filtreler oluşturma](filters-dynamic-manifest-cli-howto.md)

Canlı makaleler ve örnekler için bkz:

- [Kavram: canlı akış genel bakış](live-streaming-overview.md)
- [Konsept: Canlı Etkinlikler ve Canlı Çıktılar](live-events-outputs-concept.md)
- [Örnek: canlı akış eğitimi](stream-live-tutorial-with-api.md)

## <a name="416-range-not-satisfiable"></a>416 Aralığı Tatmin Edilemez

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_INVALID_RANGE|0x808900F1|Depolama İşlemi hatası, döndürülen http 416 hatası, geçersiz aralık.|

## <a name="500-internal-server-error"></a>500 İç Sunucu Hatası

İsteğin işlenmesi sırasında, Medya Hizmetleri işlemin devam etmesini engelleyen bir hatayla karşılaşır.  

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_SOCKET_TIMEOUT|0x808900F4|Winhttp hata kodundan ERROR_WINHTTP_TIMEOUT (0x00002ee2) alındı ve çevrildi.|
|MPE_STORAGE_SOCKET_CONNECTION_ERROR|0x808900F5|Winhttp hata kodundan ERROR_WINHTTP_CONNECTION_ERROR (0x00002efe) alındı ve çevrildi.|
|MPE_STORAGE_SOCKET_NAME_NOT_RESOLVED|0x808900F6|Winhttp hata kodundan ERROR_WINHTTP_NAME_NOT_RESOLVED (0x00002ee7) alındı ve çevrildi.|
|MPE_STORAGE_INTERNAL_ERROR|0x808900E6|Depolama İşlemi hatası, HTTP 500 hatalarından birinin genel InternalError'ı.|
|MPE_STORAGE_OPERATION_TIMED_OUT|0x808900E7|Depolama İşlemi hatası, http 500 hatalarından birinin genel OperationTimedOut.|
|MPE_STORAGE_FAILURE|0x808900F2|Depolama İşlemi hatası, InternalError veya OperationTimedOut'a göre diğer HTTP 500 hataları.|

## <a name="503-service-unavailable"></a>503 Hizmet Kullanılamıyor

Sunucu şu anda istek alamıyor. Bu hata, hizmete yapılan aşırı isteklerden kaynaklanabilir. Ortam Hizmetleri azaltma mekanizması, hizmete aşırı istekte bulunduran uygulamalar için kaynak kullanımını kısıtlar.

> [!NOTE]
> 503 hatasını alma nedeniniz hakkında daha ayrıntılı bilgi almak için hata iletisini ve hata kodu dizesini denetleyin. Bu hata her zaman azaltma anlamına gelmez.
> 

|Hata kodu|Hexadecimal değer |Hata açıklaması|
|---|---|---|
|MPE_STORAGE_SERVER_BUSY|0x808900E8|Depolama İşlemi hatası, http sunucu meşgul hata 503 aldı.|

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="see-also"></a>Ayrıca bkz.

- [Kodlama hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
- [Azure Medya Hizmetleri kavramları](concepts-overview.md)
- [Kotalar ve Sınırlamalar](limits-quotas-constraints.md)

## <a name="next-steps"></a>Sonraki adımlar

[Örnek: .NET ile ApiException'dan ErrorCode ve İleti'ye erişin](configure-connect-dotnet-howto.md#connect-to-the-net-client)
