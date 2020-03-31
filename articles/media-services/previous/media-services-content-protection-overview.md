---
title: Azure Medya Hizmetleri ile içeriğinizi koruyun | Microsoft Dokümanlar
description: Bu makalede, Azure Media Services v2 ile içerik koruma genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 88e0e1c18722fd86e79fc1fa7722b59b3cb8966a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460968"
---
# <a name="content-protection-overview"></a>İçerik korumaya genel bakış 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Depolama, işleme ve teslim yoluyla bilgisayarınızı terk eden andan itibaren medyanızı güvenli hale getirmek için Azure Medya Hizmetleri'ni kullanabilirsiniz. Media Services ile canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya üç büyük dijital haklar yönetimi (DRM) sisteminden herhangi biriyle dinamik olarak şifrelenmiş olarak teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisansları sunmak için bir hizmet sağlar. 

Aşağıdaki resim, Medya Hizmetleri içerik koruma iş akışını göstermektedir: 

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Bu makalede, Medya Hizmetleri ile içerik korumasını anlamakla ilgili kavramlar ve terminoloji açıklanmaktadır. Makale ayrıca içeriğin nasıl korunabildiğini tartışan makalelere bağlantılar da sağlar. 

## <a name="dynamic-encryption"></a>Dinamik şifreleme

PlayReady, Widevine veya FairPlay kullanarak AES net tuşu veya DRM şifrelemesi ile dinamik olarak şifrelenmiş içeriğinizi sunmak için Medya Hizmetlerini kullanabilirsiniz. İçerik AES açık anahtarıyla şifrelenir ve HTTPS üzerinden gönderilirse, istemciye ulaşana kadar net değildir. 

Her şifreleme yöntemi aşağıdaki akış protokollerini destekler:
 
- AES: MPEG-DASH, Düzgün Akış ve HLS
- PlayReady: MPEG-DASH, Düzgün Akış ve HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Aşamalı karşıdan yüklemelerde şifreleme desteklenmez. 

Bir varlığı şifrelemek için, bir şifreleme içerik anahtarını kıymetinizle ilişkilendirmeniz ve ayrıca anahtar için bir yetkilendirme ilkesi yapılandırmanız gerekir. İçerik anahtarları Media Services tarafından belirtilebilir veya otomatik olarak oluşturulabilir.

Ayrıca varlığın teslim ilkesini yapılandırmanız gerekir. Depolama yla şifrelenmiş bir kıymeti akışaltına almak istiyorsanız, varlık teslim ilkesini yapılandırarak bu kıymeti nasıl teslim etmek istediğinizi belirttiğinizi unutmayın.

Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri AES clear tuşu veya DRM şifrelemesi kullanarak içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır. Akışın şifresini çözmek için oynatıcı anahtarı Media Services anahtar teslim hizmetinden ister. Kullanıcının anahtarı almaya yetkili olup olmadığına karar vermek için, hizmet anahtar için belirlediğiniz yetkilendirme ilkelerini değerlendirir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 net anahtar vs DRM
Müşteriler genellikle AES şifreleme si mi yoksa DRM sistemi mi kullanmaları gerektiğini merak ederler. İki sistem arasındaki temel fark, AES şifrelemesi ile içerik anahtarının istemciye şifrelenmemiş bir biçimde ("açık olarak") iletilmesidir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar, istemcideki bir ağ izinde düz metin olarak görüntülenebilir. AES-128 net anahtar şifrelemesi, görüntüleyenin güvenilir bir taraf olduğu kullanım durumları için uygundur (örneğin, çalışanlar tarafından görüntülenecek bir şirket içinde dağıtılan şirket videolarının şifrelenmesi).

PlayReady, Widevine ve FairPlay'in tümü AES-128 net anahtar şifrelemesine kıyasla daha yüksek düzeyde şifreleme sağlar. İçerik anahtarı şifreli bir biçimde iletilir. Ayrıca, şifre çözme, kötü amaçlı bir kullanıcının saldırmasının daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda işlenir. DRM, görüntüleyenin güvenilir bir taraf olmadığı ve en yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-encryption"></a>Depolama şifrelemesi
AES 256 bit şifreleme kullanarak temiz içeriğinizi yerel olarak şifrelemek için depolama şifrelemesini kullanabilirsiniz. Daha sonra, istirahatte şifrelenmiş olarak depolandığı Azure Depolama'ya yükleyebilirsiniz. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmez ve kodlamadan önce şifreli bir dosya sistemine yerleştirilir. Varlıklar, yeni bir çıktı varlığı olarak geri yüklemeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama şifrelemesi için birincil kullanım örneği, yüksek kaliteli giriş ortam dosyalarınızı diskte güçlü şifreleme yle güvence altına almak istediğinizde olur.

Depolama yla şifrelenmiş bir varlık sunmak için, Medya Hizmetleri'nin içeriğinizi nasıl teslim etmek istediğinizi bilmesi için varlığın teslim politikasını yapılandırmanız gerekir. Varlığınız akış aramadan önce, akış sunucusu belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme olmadan) kullanarak içeriğinizin şifresini çözer ve akışı sağlar.

## <a name="types-of-encryption"></a>Şifreleme türleri
PlayReady ve Widevine ortak şifreleme (AES TO modu) kullanır. FairPlay, AES CBC modu şifrelemesi kullanır. AES-128 net anahtar şifrelemesi zarf şifrelemekullanır.

## <a name="licenses-and-keys-delivery-service"></a>Lisanslar ve anahtarteslim hizmeti
Medya Hizmetleri, DRM (PlayReady, Widevine, FairPlay) lisanslarını ve AES anahtarlarını yetkili müşterilere teslim etmek için önemli bir teslimat hizmeti sunar. Lisanslarınız ve anahtarlarınız için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için .NET için [Azure portalını](media-services-portal-protect-content.md), REST API veya Media Services SDK'yı kullanabilirsiniz.

## <a name="control-content-access"></a>İçerik erişimini denetleme
İçerik anahtarı yetkilendirme ilkesini yapılandırarak içeriğinize kimlerin erişebileceğini kontrol edebilirsiniz. İçerik anahtarı yetkilendirme ilkesi açık veya belirteç kısıtlamasını destekler.

### <a name="open-authorization"></a>Açık yetkilendirme
Açık bir yetkilendirme ilkesiyle, içerik anahtarı herhangi bir istemciye gönderilir (kısıtlama yoktur).

### <a name="token-authorization"></a>Belirteç yetkilendirmesi
Belirteç kısıtlamalı yetkilendirme ilkesiyle, içerik anahtarı yalnızca anahtar/lisans isteğinde geçerli bir JSON Web belirteci (JWT) veya basit bir web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirteç bir güvenlik belirteci hizmeti (STS) tarafından verilmelidir. Azure Etkin Dizini STS olarak kullanabilir veya özel bir STS dağıtabilirsiniz. STS belirtilmiş anahtar ve belirteç kısıtlama yapılandırmasında belirttiğiniz sorun talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Ortam Hizmetleri anahtar teslim hizmeti, belirteç geçerliyse ve belirteçteki talepler anahtar/lisans için yapılandırılanlarla eşleşirse, istenen anahtarı/lisansı istemciye geri verir.

Belirteç kısıtlı ilkesini yapılandırdığınızda, birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı, belirteçle imzalandığı anahtarı içerir. İhraççı, belirteci veren güvenli belirteç hizmetidir. Bazen kapsam olarak adlandırılan hedef kitle, belirteciveya belirteç erişim eizin kaynağının amacını açıklar. Medya Hizmetleri anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

### <a name="token-replay-prevention"></a>Belirteç tekrar önleme

*Token Yeniden Oynatma Önleme* özelliği, Medya Hizmetleri müşterilerinin bir anahtar veya lisans istemek için aynı belirteçlerin kaç kez kullanılabileceğini belirlemesine olanak tanır. Müşteri, belirteç tespinin lisans veya anahtar almak için kaç kez kullanılabileceğinibelirtebilen bir tür `urn:microsoft:azure:mediaservices:maxuses` talebi ekleyebilir. Anahtar Teslimi'ne aynı belirteç içeren sonraki tüm istekler yetkisiz bir yanıt döndürecektir. DrM [örneğine](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601)talebin nasıl ekleyeceğinize bakın.
 
#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Müşteriler belirteç üretimi üzerinde kontrole sahip olmalıdır. İddianın belirteç tesine yerleştirilmesi gerekir.
* Bu özelliği kullanırken, son kullanma süresi, istek alındığı andan itibaren bir saatten fazla olan belirteçlere sahip istekler yetkisiz bir yanıtla reddedilir.
* Belirteçler, imzalarıyla benzersiz bir şekilde tanımlanır. Taşıma yükündeki herhangi bir değişiklik (örneğin, son kullanma süresine veya talepte güncelleştirme) belirteç imzasını değiştirir ve Anahtar Tesliminin daha önce rastlamadığını yeni bir belirteç olarak sayılır.
* Belirteç müşteri tarafından ayarlanan `maxuses` değeri aşmışsa oynatma başarısız olur.
* Bu özellik, varolan tüm korumalı içerik için kullanılabilir (yalnızca verilen belirteç değiştirilmelidir).
* Bu özellik hem JWT hem de SWT ile çalışır.

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınız birden fazla DRM ile şifrelenmişse, akış URL'sinde bir şifreleme etiketi kullanın: (format='m3u8-aapl', encryption='xxx').

Aşağıdaki noktalara dikkat edilmelidir:

* En fazla birden fazla şifreleme türü belirtilebilir.
* Varlığa yalnızca bir şifreleme uygulandıysa, şifreleme türü URL'de belirtilmesi ne kadar önemli değildir.
* Şifreleme türü büyük/küçük harf duyarsız.
* Aşağıdaki şifreleme türleri belirtilebilir:

  * **cenc**: PlayReady veya Widevine için (ortak şifreleme)
  * **cbcs-aapl**: FairPlay için (AES CBC şifreleme)
  * **cbc**: AES zarf şifrelemesi için

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler, içerik koruma ile başlamanıza yardımcı olacak sonraki adımları açıklar:

* [Depolama şifrelemesi ile koruyun](media-services-rest-storage-encryption.md)
* [AES şifrelemeile koruyun](media-services-protect-with-aes128.md)
* [PlayReady ve/veya Widevine ile koruyun](media-services-protect-with-playready-widevine.md)
* [FairPlay ile koruyun](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>İlgili bağlantılar

* [JWT belirteç kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Azure Medya Hizmetleri OWIN MVC tabanlı uygulamayı Azure Active Directory ile tümleştirin ve JWT iddialarına göre içerik anahtarı teslimini kısıtlayın](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
