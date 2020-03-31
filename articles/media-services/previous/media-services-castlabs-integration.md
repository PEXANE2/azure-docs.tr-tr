---
title: Azure Medya Hizmetleri'ne Widevine lisansları sunmak için castLabs'ı kullanma | Microsoft Dokümanlar
description: Bu makalede, Hem PlayReady hem de Widevine DRM'lerle AMS tarafından dinamik olarak şifrelenmiş bir akış sunmak için Azure Medya Hizmetleri'ni (AMS) nasıl kullanabileceğiniz açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Mingfeiy
manager: femila
editor: ''
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: Juliako
ms.reviewer: willzhan
ms.openlocfilehash: 29a344c739d8d99da2e5c81d41a11c601e48022e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74969149"
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için castLabs kullanma 
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Genel Bakış

Bu makalede, Hem PlayReady hem de Widevine DRM'lerle AMS tarafından dinamik olarak şifrelenmiş bir akış sunmak için Azure Medya Hizmetleri'ni (AMS) nasıl kullanabileceğiniz açıklanmaktadır. PlayReady lisansı Media Services PlayReady lisans sunucusundan gelir ve Widevine lisansı **castLabs** lisans sunucusu tarafından teslim edilir.

CENC (PlayReady ve/veya Widevine) tarafından korunan akış içeriğini oynatmak için [Azure Media Player'ı](https://aka.ms/azuremediaplayer)kullanabilirsiniz. Ayrıntılar için [AMP belgesine](https://amp.azure.net/libs/amp/latest/docs/) bakın.

Aşağıdaki diyagram, üst düzey bir Azure Medya Hizmetleri ve castLabs tümleştirme mimarisini gösterir.

![Entegrasyon](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Tipik sistem kurulumu

* Medya içeriği AMS'de depolanır.
* İçerik anahtarlarının anahtar işleri hem castLabs' da hem de AMS' de depolanır.
* castLabs ve AMS hem dahili belirteç kimlik doğrulaması var. Aşağıdaki bölümlerde kimlik doğrulama belirteçleri tartışılır. 
* İstemci videoyu akışı istediğinde, içerik **Ortak Şifreleme** (CENC) ile dinamik olarak şifrelenir ve AMS tarafından Sorunsuz Akış ve DASH'e kadar dinamik olarak paketlenir. HlS akış protokolü için PlayReady M2TS temel akış şifrelemesi de salıyoruz.
* PlayReady lisansı AMS lisans sunucusundan, Widevine lisansı ise castLabs lisans sunucusundan alınır. 
* Media Player, istemci platformu özelliğine bağlı olarak hangi lisansı alıtacaklarına otomatik olarak karar verir. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Lisans almak için kimlik doğrulama belirteç oluşturma

Hem castLabs hem de AMS, lisans yetkilendirmek için kullanılan JWT (JSON Web Token) belirteci biçimini destekler. 

### <a name="jwt-token-in-ams"></a>AMS'de JWT belirteci

Aşağıdaki tabloDA AMS'de JWT belirteci açıklanmaktadır. 

| Veren | Seçilen Güvenli Belirteç Hizmeti(STS) gelen veren dizesi |
| --- | --- |
| Hedef kitle |Kullanılan STS'den hedef kitle dizesi |
| Talepler |Bir dizi talep |
| NotBefore |Belirteç geçerliliğini başlatın |
| Süre Sonu |Belirteç lerin bitiş geçerliliği |
| İmzaLama Kimlik Bilgileri |PlayReady Lisans Sunucusu, castLabs Lisans Sunucusu ve STS arasında paylaşılan anahtar, simetrik veya asimetrik anahtar olabilir. |

### <a name="jwt-token-in-castlabs"></a>CastLabs JWT belirteç

Aşağıdaki tabloda castLabs JWT belirteci açıklanmaktadır. 

| Adı | Açıklama |
| --- | --- |
| optData |Hakkında bilgi içeren bir JSON dizesi. |
| Crt |Varlık, lisans bilgileri ve oynatma hakları hakkında bilgi içeren bir JSON dizesi. |
| ıat |Dönem'deki geçerli datetime. |
| jti |Bu belirteç hakkında benzersiz bir tanımlayıcı (her belirteç sadece bir kez castLabs sisteminde kullanılabilir). |

## <a name="sample-solution-setup"></a>Örnek çözüm kurulumu

[Örnek çözüm](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) iki projeden oluşur:

* Hem PlayReady hem de Widevine için, halihazırda yutulmuş bir varlık üzerinde DRM kısıtlamalarını ayarlamak için kullanılabilen bir konsol uygulaması.
* Bir STS'nin ÇOK BASITLEŞTIRILMIŞ bir sürümü olarak görülebilecek belirteçleri dağıtan bir Web Uygulaması.

Konsol uygulamasını kullanmak için:

1. AMS kimlik bilgilerini, castLabs kimlik bilgilerini, STS yapılandırmasını ve paylaşılan anahtarı kurmak için app.config'i değiştirin.
2. Bir Varlığı AMS'ye yükleyin.
3. Yüklenen Varlık'tan UUID'yi alın ve Program.cs dosyasındaki Satır 32'yi değiştirin:
   
      var objIAsset = _context. Varlıklar.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf"). firstordefault();
4. CastLabs sistemindeki varlığı adlandırmak için bir AssetId kullanın (Program.cs dosyasındaki Satır 44).
   
   **CastLabs**için AssetId ayarlamak gerekir; eşsiz bir alfasayısal dize olması gerekir.
5. Programı çalıştırın.

Web Uygulamasını (STS) kullanmak için:

1. web.config'i castlabs merchant ID, STS yapılandırması ve paylaşılan anahtar la değiştirin.
2. Azure Web Sitelerine dağıtın.
3. Web sitesine gidin.

## <a name="playing-back-a-video"></a>Videoyu oynatma

Ortak şifrelemeyle (PlayReady ve/veya Widevine) şifrelenmiş bir videoyu oynatmak için [Azure Media Player'ı](https://aka.ms/azuremediaplayer)kullanabilirsiniz. Konsol uygulamasını çalıştırırken İçerik Anahtarı Kimliği ve Manifesto URL'si yankılanır.

1. Yeni bir sekme açın ve STS'nizi başlatın: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Azure [Media Player'a](https://aka.ms/azuremediaplayer)gidin.
3. Akış URL'ye yapıştırın.
4. Gelişmiş **Seçenekler** onay kutusunu tıklatın.
5. **Koruma** açılır düşüşünde PlayReady ve/veya Widevine'i seçin.
6. STS'nizden aldığınız belirteci Token metin kutusuna yapıştırın. 
   
   CastLab lisans sunucusunun belirteci önünde "Bearer=" önekine ihtiyacı yoktur. Bu yüzden lütfen belirteci göndermeden önce kaldırın.
7. Oyuncuyu güncelleştirin.
8. Video oynatılmalı.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

