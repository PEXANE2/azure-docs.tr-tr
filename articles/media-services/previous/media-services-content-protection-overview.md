---
title: İçeriğinizi Azure Media Services koruyun | Microsoft Docs
description: Bu makalede Azure Media Services V2 ile içerik korumasına genel bakış sunulmaktadır.
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
ms.openlocfilehash: 4ff4025941e9a77148daa91995ecf182231d1f0b
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976290"
---
# <a name="content-protection-overview"></a>Content protection genel bakış 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz. [v2 'den v3 'e geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady ve Google Widevine Apple FairPlay. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki resimde Media Services content protection iş akışı gösterilmektedir: 

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Bu makalede, kavramlar ve terminoloji content protection ile Media Services anlamak için ilgili açıklanmaktadır. Makale Ayrıca içeriği nasıl koruyabileceğinizi tartışan makalelere bağlantılar sağlar. 

## <a name="dynamic-encryption"></a>Dinamik şifreleme
 Media Services PlayReady, Widevine ve FairPlay kullanarak AES şifresiz anahtarını veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinde şifreleyebilirsiniz. Aşamalı indirmelerde şifreleme desteklenmez. Her şifreleme yöntemi aşağıdaki akış protokollerini destekler:

- AES: MPEG-DASH, Kesintisiz Akış ve HLS
- PlayReady: MPEG-DASH, Kesintisiz Akış ve HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Bir varlığı şifrelemek için bir şifreleme içerik anahtarını varlığınızla ilişkilendirmeniz ve ayrıca anahtar için bir Yetkilendirme İlkesi yapılandırmanız gerekir. İçerik anahtarları belirtilebilir veya Media Services tarafından otomatik olarak oluşturulabilir.

Ayrıca varlığın teslim ilkesini de yapılandırmanız gerekir. Depolama ile şifrelenen bir varlığı akışa almak istiyorsanız, varlık teslim ilkesini yapılandırarak nasıl teslim etmek istediğinizi belirttiğinizden emin olun.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akışın şifresini çözmek için, Player anahtarı Media Services anahtar teslim hizmetinden ister. Kullanıcının anahtarı almak için yetkilendirilip yetkilendirilmeyeceğine karar vermek için, hizmet anahtar için belirttiğiniz yetkilendirme ilkelerini değerlendirir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 şifresiz anahtar ve DRM
Müşteriler genellikle bunlar AES şifrelemesi veya DRM sistem kullanması gerekip gerekmediğini merak ediyor. İki sistem arasındaki başlıca fark, AES şifreleme ile içerik anahtarını şifresiz bir biçimde ("clear") istemciye iletilir ' dir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar düz metin içinde bulunan istemciye bir ağ izleme görüntülenebilir. AES-128 şifresiz anahtar şifrelemesi Görüntüleyicisi (çalışanlar tarafından görüntülenmek üzere şirket içinde dağıtılmış gibi şifreleme şirket videolarınızı) güvenilen taraf olduğu kullanım örnekleri için uygundur.

PlayReady, Widevine ve FairPlay tüm şifreleme kıyasla daha yüksek bir düzeyde AES-128 şifresiz anahtar şifrelemesiyle koruyun. İçerik anahtarı şifrelenmiş biçimde iletilir. Ayrıca, şifre çözme, kötü niyetli bir kullanıcı saldırı daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda ele alınır. DRM burada Görüntüleyicisi güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-encryption"></a>Depolama şifrelemesi
AES 256 bit şifrelemeyi kullanarak açık içeriğinizi yerel olarak şifrelemek için depolama şifrelemesini kullanabilirsiniz. Daha sonra bu dosyayı Azure depolama 'ya yükleyebilirsiniz ve burada şifreli olarak depolanır. Depolama şifrelemesi ile korunan varlıklar, kodlama öncesinde otomatik olarak şifrelenmez ve şifreli bir dosya sistemine yerleştirilir. Varlıklar, yeni bir çıkış varlığı olarak geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama şifrelemesi için birincil kullanım örneği, yüksek kaliteli giriş medya dosyalarınızı diskte bekleyen bir şekilde güçlü şifreleme ile güvenli hale getirmek istediğinizde kullanılır.

Depolama ile şifrelenen bir varlık sağlamak için, varlığın teslim ilkesini, Media Services içeriğinizi nasıl teslim etmek istediğinizi bilmek üzere yapılandırmanız gerekir. Varlığınızın akışı silinmeden önce, akış sunucusu belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme olmadan) kullanarak içeriğinizi çözer ve akışlar.

## <a name="types-of-encryption"></a>Şifreleme türleri
PlayReady ve Widevine, ortak şifrelemeyi (AES Mrk modu) kullanır. FairPlay, AES CBC modu şifrelemesini kullanır. AES-128 Clear anahtar şifrelemesi, zarf şifrelemesini kullanır.

## <a name="licenses-and-keys-delivery-service"></a>Lisanslar ve anahtarları teslim hizmeti
Media Services DRM (PlayReady, Widevine, FairPlay) lisansları ve AES anahtarları yetkili istemcilere sunmak için bir anahtar dağıtımı hizmetiyle sağlar. Lisanslar ve anahtarlarınız için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak üzere .NET için [Azure Portal](media-services-portal-protect-content.md), REST API veya Media Services SDK 'sını kullanabilirsiniz.

## <a name="control-content-access"></a>İçerik erişimi denetleme
İçerik anahtarı yetkilendirme ilkesini yapılandırarak içeriğinizi kimlerin erişebileceğini denetleyebilirsiniz. İçerik anahtarı yetkilendirme ilkesi, açık ya da belirteç kısıtlamasını destekler.

### <a name="open-authorization"></a>Yetkilendirmeyi aç
Açık bir yetkilendirme ilkesiyle, içerik anahtarı herhangi bir istemciye gönderilir (kısıtlama yoktur).

### <a name="token-authorization"></a>Belirteç yetkilendirme
Belirteç kısıtlı yetkilendirme ilkesiyle, içerik anahtarı yalnızca anahtar/lisans isteğinde geçerli bir JSON Web Token (JWT) veya basit Web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirteci bir güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Azure Active Directory STS kullanın veya özel STS dağıtın. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar dağıtımı hizmetiyle belirteç geçerliyse ve belirteçteki talepler için bir anahtar/lisans yapılandırılanlar eşleşen istemci için istenen anahtar/lisans döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Verici belirteci veren güvenli belirteç hizmetidir. Belirtecin amacı kapsam olarak da adlandırılan, hedef kitle açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

## <a name="streaming-urls"></a>Akış URL 'Leri
Varlığınız birden fazla DRM ile şifrelendiyse, akış URL 'sinde bir şifreleme etiketi kullanın: (format = 'm 3u8-AAPL ', ENCRYPTION = ' xxx ').

Aşağıdaki noktalara dikkat edilmelidir:

* Birden fazla şifreleme türü belirtilemez.
* Varlığa yalnızca bir şifreleme uygulanmışsa, şifreleme türünün URL 'de belirtilmesi gerekmez.
* Şifreleme türü, büyük/küçük harfe duyarlıdır.
* Aşağıdaki şifreleme türleri belirtilebilir:
  * **CENC**: PlayReady veya Widevine (ortak şifreleme) için
  * **CBCS-AAPL**: Fairplay IÇIN (AES CBC şifrelemesi)
  * **CBC**: AES zarf şifrelemesi için

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler, içerik korumasına başlamanıza yardımcı olacak sonraki adımları anlatmaktadır:

* [Depolama şifrelemesi ile koruma](media-services-rest-storage-encryption.md)
* [AES şifrelemesi ile koruma](media-services-protect-with-aes128.md)
* [PlayReady ve/veya Widevine ile koruma](media-services-protect-with-playready-widevine.md)
* [FairPlay ile koruma](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>İlgili bağlantılar

* [JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Azure Media Services OWIN MVC tabanlı uygulama Azure Active Directory ve JWT taleplerine göre içerik anahtarı teslimini tümleştirin](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
