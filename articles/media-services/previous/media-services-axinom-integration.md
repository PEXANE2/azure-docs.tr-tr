---
title: Azure Medya Hizmetlerine Widevine lisansları sunmak için Axinom'u kullanma | Microsoft Dokümanlar
description: Bu makalede, Hem PlayReady hem de Widevine DRM'lerle AMS tarafından dinamik olarak şifrelenmiş bir akış sunmak için Azure Medya Hizmetleri'ni (AMS) nasıl kullanabileceğiniz açıklanmaktadır. PlayReady lisansı Media Services PlayReady lisans sunucusundan gelir ve Widevine lisansı Axinom lisans sunucusu tarafından teslim edilir.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan
ms.reviewer: Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 2ec3276b9b02c29b80d46e5fd31298c909857182
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197173"
---
# <a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için Axinom kullanma 
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Azure Medya Hizmetleri (AMS), Google Widevine dinamik koruması ekledi (ayrıntılar için [Mingfei'nin bloguna](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) bakın). Buna ek olarak, Azure Media Player (AMP) ayrıca Widevine desteği ekledi (ayrıntılar için [AMP belgesine](https://amp.azure.net/libs/amp/latest/docs/) bakın). Bu, MSE ve EME ile donatılmış modern tarayıcılarda çok yerel DRM (PlayReady ve Widevine) ile CENC tarafından korunan DASH içeriğiakışında büyük bir başarıdır.

Medya Hizmetleri .NET SDK sürüm 3.5.2 ile başlayarak, Medya Hizmetleri Widevine lisans şablonu yapılandırmak ve Widevine lisansları almak sağlar. Widevine lisansları teslim etmenize yardımcı olması için şu AMS ortaklarını da kullanabilirsiniz: [Axinom](https://www.axinom.com), [EZDRM](https://ezdrm.com/), [castLabs](https://castlabs.com/company/partners/azure/).

Bu makalede, Axinom tarafından yönetilen Widevine lisans sunucusunun nasıl tümleştirilen ve test edilebilen açıklanmaktadır. Özellikle, kapsar:  

* Dinamik Ortak Şifrelemeyi multi-DRM (PlayReady ve Widevine) ile ilgili lisans satın alma URL'leri ile yapılandırmak;
* Lisans sunucusu gereksinimlerini karşılamak için JWT belirteci oluşturma;
* JWT belirteç kimlik doğrulaması ile lisans edinimi işleyen Azure Media Player uygulamasını geliştirme;

Tam sistem ve içerik anahtarı, anahtar kimliği, anahtar tohumu, JTW belirteci ve iddiaları akışı en iyi aşağıdaki diyagram ile açıklanabilir:

![DASH ve CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Content Protection
Dinamik koruma ve anahtar teslim ilkesini yapılandırmak için lütfen Mingfei'nin bloguna bakın: [Widevine ambalajını Azure Medya Hizmetleri ile nasıl yapılandırAbilirsiniz.](https://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

Aşağıdakilerden her ikisine sahip DASH akışı için çoklu DRM ile dinamik CENC korumasını yapılandırabilirsiniz:

1. Microsoft Edge ve IE11 için PlayReady koruması, bir belirteç yetkilendirme kısıtlaması olabilir. Belirteç kısıtlı ilkesine, Azure Etkin Dizini gibi Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç eşlik etmelidir;
2. Chrome için Widevine koruması, başka bir STS tarafından verilen belirteç ile belirteç kimlik doğrulaması gerektirebilir. 

Azure Active Directory'nin Axinom'un Widevine lisans sunucusu için neden STS olarak kullanılamadığına dair [JWT Belirteç Oluşturma](media-services-axinom-integration.md#jwt-token-generation) bölümüne bakın.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
1. Anahtar teslim hizmetini yapılandırmak için içerik anahtarını oluşturmak için Axinom belirtilen anahtar tohumunu (8888000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000) ve oluşturulan veya seçilen anahtar kimliğini kullanarak anahtar teslim hizmetini yapılandırmak için içerik anahtarını oluşturmalısınız. Aksinom lisans sunucusu, hem test hem de üretim için geçerli olan aynı anahtar tohumuna dayalı içerik anahtarları içeren tüm lisansları yayınlar.
2. Test için Widevine lisans [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense)edinme URL'si: . Hem HTTP hem de HTTS'ye izin verilir.

## <a name="azure-media-player-preparation"></a>Azure Media Player Hazırlama
AMP v1.4.0, hem PlayReady hem de Widevine DRM ile dinamik olarak paketlenmiş AMS içeriğinin oynatıltını destekler.
Widevine lisans sunucusu belirteç kimlik doğrulaması gerektirmiyorsa, Widevine tarafından korunan bir DASH içeriğini sınamak için yapmanız gereken ek bir şey yoktur. Örneğin, AMP ekibi, Widevine ile PlayReady ve Chrome ile Microsoft Edge ve IE11'de çalıştığını görebileceğiniz basit bir [örnek](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevineFairPlay_notoken.html)sağlar.
Axinom tarafından sağlanan Widevine lisans sunucusu JWT belirteç kimlik doğrulaması gerektirir. JWT belirteci bir HTTP başlık "X-AxDRM-Message" üzerinden lisans isteği ile gönderilmesi gerekir. Bu amaçla, kaynağı ayarlamadan önce AMP barındırma web sayfasına aşağıdaki javascript eklemeniz gerekir:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

AMP kodunun geri [kalanı, amp](https://amp.azure.net/libs/amp/latest/docs/)belgesinde olduğu gibi standart AMP API'dir.

Amp resmi uzun vadeli yaklaşım yayımlanmadan önce özel yetkilendirme üstbilgi ayarı için yukarıdaki javascript hala kısa vadeli bir yaklaşımdır.

## <a name="jwt-token-generation"></a>JWT Belirteç Üretimi
Test için Axinom Widevine lisans sunucusu JWT belirteç kimlik doğrulaması gerektirir. Buna ek olarak, JWT belirtecindeki iddialardan biri, ilkel veri türü yerine karmaşık bir nesne türüne aittir.

Ne yazık ki, Azure AD yalnızca ilkel türleri ile JWT belirteçleri verebilir. Benzer şekilde, .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler ve JwtPayload) yalnızca karmaşık nesne türünü talep olarak giriş yapmanızı sağlar. Ancak, iddialar hala dize olarak serihale edilir. Bu nedenle, Widevine lisans isteği için JWT belirteci oluşturmak için ikisini kullanamayız.

John Sheehan'ın [JWT NuGet paketi](https://www.nuget.org/packages/JWT) ihtiyaçları karşılar, bu yüzden bu NuGet paketini kullanacağız.

Aşağıda test etmek için Axinom Widevine lisans sunucusu tarafından gerekli olduğu gibi gerekli iddiaları ile JWT belirteci oluşturmak için kod:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;

    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.

                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };

                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);

                return token;
            }

            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }

                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }

                return HexAsBytes;
            }

        }  

    }  

Axinom Widevine lisans sunucusu

    <add key="ax:laurl" value="https://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a>Dikkat edilmesi gerekenler
1. AMS PlayReady lisans teslim hizmeti kimlik doğrulama belirtecinden önce "Taşıyıcı=" gerektirmesine rağmen, Axinom Widevine lisans sunucusu bunu kullanmaz.
2. Axinom iletişim anahtarı imza anahtarı olarak kullanılır. Anahtar bir hex dizesi, ancak kodlama bir dize değil bayt bir dizi olarak kabul edilmelidir. Bu yöntem ConvertHexStringToByteArray tarafından elde edilir.

## <a name="retrieving-key-id"></a>Anahtar Kimliğini Alma
Bir JWT belirteci oluşturmak için kod, anahtar kimliği gerekli olduğunu fark etmiş olabilirsiniz. JWT belirteci AMP oynatıcı yüklemeden önce hazır olması gerektiğinden, JWT belirteci oluşturmak için anahtar kimliğinin alınması gerekir.

Tabii ki, anahtar kimliği ele almak için birden fazla yolu vardır. Örneğin, bir veritabanında içerik meta verileri ile birlikte anahtar kimliği depolar. Veya DASH MPD (Medya Sunu açıklaması) dosyasından anahtar kimliği alabilirsiniz. Aşağıdaki kod ikincisi içindir.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }

        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "https://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();

        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);

        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }

        return key_id;
    }

## <a name="summary"></a>Özet

Hem Azure Medya Hizmetleri İçerik Koruması hem de Azure Media Player'da Widevine desteğinin en son eklenmesiyle, AMS ve Widevine lisansında hem PlayReady lisans hizmeti yle DASH + Multi-native-DRM (PlayReady + Widevine) akışını uygulayabiliyoruz aşağıdaki modern tarayıcılar için Axinom sunucu:

* Chrome
* Windows 10'da Microsoft Edge
* Windows 8.1 ve Windows 10'da IE 11
* Mac'teki Firefox (Desktop) ve Safari (iOS değil) silverlight ve Azure Media Player ile aynı URL üzerinden de desteklenir

Axinom Widevine lisans sunucusundan yararlanan mini çözeltide aşağıdaki parametreler gereklidir. Anahtar kimliği dışında, parametrelerin geri kalanı Widevine sunucu kurulumuna göre Axinom tarafından sağlanır.

| Parametre | Nasıl kullanılır? |
| --- | --- |
| İletişim anahtarı kimliği |JWT belirtecindeki "com_key_id" iddiasının değeri olarak eklenmelidir [(bu](media-services-axinom-integration.md#jwt-token-generation) bölüme bakın). |
| İletişim anahtarı |JWT belirteci imza anahtarı olarak kullanılmalıdır [(bu](media-services-axinom-integration.md#jwt-token-generation) bölüme bakın). |
| Anahtar tohumu |Herhangi bir içerik anahtarı kimliğiyle içerik anahtarı oluşturmak için kullanılmalıdır [(bu](media-services-axinom-integration.md#content-protection) bölüme bakın). |
| Widevine Lisans edinme URL'si |DASH akışı için varlık teslim ilkesini yapılandırmada kullanılmalıdır [(bu](media-services-axinom-integration.md#content-protection) bölüme bakın). |
| İçerik Anahtar Kimliği |JWT belirteci Nin Yetki Lendirme İletisi talebinin değerinin bir parçası olarak dahil edilmelidir [(bu](media-services-axinom-integration.md#jwt-token-generation) bölüme bakın). |

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>İlgili kaynaklar
Biz bu belgenin oluşturulmasına katkıda bulunan aşağıdaki insanlar kabul etmek istiyorum: Kristjan Jõgi Axinom, Mingfei Yan ve Amit Rajput.

