---
title: Azure portalını kullanarak içerik koruma ilkelerini yapılandırın | Microsoft Dokümanlar
description: Bu makalede, içerik koruma ilkelerini yapılandırmak için Azure portalının nasıl kullanılacağı gösterilmektedir. Makale ayrıca, varlıklarınız için dinamik şifrelemeyi nasıl etkinleştirini gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 0c2a9612fab6c685cbc690aa9bbc12d1c7b7b746
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74978211"
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Azure portalını kullanarak içerik koruma ilkelerini yapılandırın

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).   > Media Services v2'ye yeni özellikler veya işlevler eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın
>

 Azure Medya Hizmetleri ile, depolama, işleme ve teslim yoluyla bilgisayarınızı terk eden andan itibaren medyanızı güvence altına alabilirsiniz. 128 bit şifreleme anahtarlarını kullanarak Gelişmiş Şifreleme Standardı (AES) ile dinamik olarak şifrelenmiş içeriğinizi sunmak için Medya Hizmetleri'ni kullanabilirsiniz. PlayReady ve/veya Widevine dijital haklar yönetimi (DRM) ve Apple FairPlay'i kullanarak da ortak şifreleme (CENC) ile kullanabilirsiniz. 

Medya Hizmetleri, yetkili müşterilere DRM lisansları ve AES açık anahtarları sunmak için bir hizmet sağlar. Azure portalını, tüm şifreleme türleri için tek bir anahtar/lisans yetkilendirme ilkesi oluşturmak için kullanabilirsiniz.

Bu makalede, portalı kullanarak bir içerik koruma ilkesinin nasıl yapılandırılabildiğini gösterilmektedir. Makale ayrıca, varlıklarınız için dinamik şifrelemenin nasıl uygulanacağı nızı da gösterir.

## <a name="start-to-configure-content-protection"></a>İçerik korumasını yapılandırmaya başlayın
Medya Hizmetleri hesabınızı kullanarak küresel içerik korumasını yapılandırmak için portalı kullanmak için aşağıdaki adımları izleyin:

1. [Portalda,](https://portal.azure.com/)Medya Hizmetleri hesabınızı seçin.

1. **Ayarlar** > **İçeriği korumayı**seçin.

    ![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Anahtar/lisans yetkilendirme ilkesi
Medya Hizmetleri, anahtar veya lisans isteklerinde bulunan kullanıcıların kimliğini doğrulamanın birden çok yolunu destekler. İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. Daha sonra anahtarın/lisansın teslim edilemeden önce müşterinizin poliçeyi karşılaması gerekir. İçerik anahtarı yetkilendirme ilkesinin açık veya belirteç kısıtlaması şeklinde bir veya daha fazla yetkilendirme kısıtlaması olabilir.

Portalı, tüm şifreleme türleri için tek bir anahtar/lisans yetkilendirme ilkesi oluşturmak için kullanabilirsiniz.

### <a name="open-authorization"></a>Açık yetkilendirme
Açık kısıtlama, sistemin anahtarı önemli bir istekte bulunan herkese teslim ettiği anlamına gelir. Bu kısıtlama test amaçları için yararlı olabilir. 

### <a name="token-authorization"></a>Belirteç yetkilendirmesi
Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit web belirteci (SWT) ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Medya Hizmetleri STS sağlamaz. Belirteçleri vermek için özel bir STS oluşturabilir veya Azure Erişim Denetim Hizmeti'ni kullanabilirsiniz. STS belirtilmiş anahtar ve belirteç kısıtlama yapılandırmasında belirttiğiniz sorun talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Belirteç geçerliyse ve belirteçteki talepler anahtar (veya lisans) için yapılandırılanlarla eşleşirse, Medya Hizmetleri anahtar teslim hizmeti istenen anahtarı (veya lisansı) istemciye döndürür.

Belirteç kısıtlamalı ilkeyi yapılandırdığınızda, birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı, belirteçle imzalandığı anahtarı içerir. İhraççı, belirteci veren güvenli belirteç hizmetidir. Hedef kitle (bazen kapsam olarak adlandırılır), belirteci veya belirteç erişim eizin kaynağının amacını açıklar. Medya Hizmetleri anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

![Anahtar/lisans yetkilendirme ilkesi](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>PlayReady lisans şablonu
PlayReady lisans şablonu, PlayReady lisansınızda etkinleştirilen işlevselliği ayarlar. PlayReady lisans şablonu hakkında daha fazla bilgi için [Medya Hizmetleri PlayReady lisans şablonuna genel bakış](media-services-playready-license-template-overview.md)bakın.

### <a name="nonpersistent"></a>Kalıcı olmayan
Bir lisansı kalıcı olmayan olarak yapılandırırsanız, lisans yalnızca oyuncu lisansı kullanırken bellekte tutulur.  

![Kalıcı olmayan içerik koruması](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Kalıcı
Bir lisansı kalıcı olarak yapılandırırsanız, istemciüzerinde kalıcı depolama alanına kaydedilir.

![Kalıcı içerik koruması](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Widevine lisans şablonu
Widevine lisans şablonu, Widevine lisanslarınızda etkinleştirilen işlevselliği ayarlar.

### <a name="basic"></a>Temel
**Temel'i**seçtiğinizde şablon tüm varsayılan değerlerle oluşturulur.

### <a name="advanced"></a>Gelişmiş
Widevine hakları şablonu hakkında daha fazla bilgi için [Widevine lisans şablonuna genel bakış](media-services-widevine-license-template-overview.md)bakın.

![Gelişmiş içerik koruması](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay yapılandırması
FairPlay şifrelemesini etkinleştirmek için **FairPlay yapılandırmasını**seçin. Ardından **Uygulama sertifikasını** seçin ve **Uygulama Gizli Anahtarı'nı**girin. FairPlay yapılandırması ve gereksinimleri hakkında daha fazla bilgi için [HLS içeriğinizi Apple FairPlay veya Microsoft PlayReady ile koruyun'a](media-services-protect-hls-with-FairPlay.md)bakın.

![FairPlay yapılandırması](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Varlığınıza dinamik şifreleme uygulayın
Dinamik şifrelemeden yararlanmak için kaynak dosyanızı uyarlanabilir bit hızında MP4 dosyaları kümesine kodlayın.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Şifrelemek istediğiniz bir varlık seçin
Tüm varlıklarınızı görmek için **Ayarlar** > **Varlıkları'nı**seçin.

![Varlıklar seçeneği](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>AES veya DRM ile şifreleme
Bir varlık için **Şifrele'yi** seçtiğinizde iki seçenek görürsünüz: **AES** veya **DRM**. 

#### <a name="aes"></a>AES
AES net anahtar şifrelemesi tüm akış protokollerinde etkinleştirilir: Düzgün Akış, HLS ve MPEG-DASH.

![Şifreleme yapılandırması](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>Drm
1. **DRM'yi**seçtikten sonra, farklı içerik koruma ilkeleri (bu noktaya göre yapılandırılması gereken) ve bir dizi akış protokolü görürsünüz:

    a. **MPEG-DASH ile PlayReady ve Widevine, MPEG-DASH** akışınızı PlayReady ve Widevine DRM'leri ile dinamik olarak şifreler.

    b. **HLS ile MPEG-DASH + FairPlay ile PlayReady ve Widevine,** MPEG-DASH akışınızı PlayReady ve Widevine DRM'leri ile dinamik olarak şifreleyin. Bu seçenek ayrıca HLS akışlarınızı FairPlay ile de şifreler.

    c. **Yalnızca Smooth Streaming, HLS ve MPEG-DASH ile PlayReady** ile PlayReady DRM ile Smooth Streaming, HLS ve MPEG-DASH akışlarını dinamik olarak şifreler.

    d. **Widevine yalnızca MPEG-DASH ile** MPEG-DASH'inizi Widevine DRM ile dinamik olarak şifreler.
    
    e. **FairPlay yalnızca HLS ile HLS** akışınızı FairPlay ile dinamik olarak şifreler.

1. FairPlay şifrelemesini etkinleştirmek **için, İçerik Koruması Global Ayarlar** bıyıküzerinde **FairPlay yapılandırmasını**seçin. Ardından **Uygulama sertifikasını**seçin ve **Uygulama Gizli Anahtarı'nı**girin.

    ![Şifreleme türü](./media/media-services-portal-content-protection/media-services-content-protection009.png)

1. Şifreleme seçimini yaptıktan sonra **Uygula'yı**seçin.

>[!NOTE] 
>Safari'de AES şifreli bir HLS oynamayı planlıyorsanız, [Safari'deki Şifreli HLS](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)blog gönderisine bakın.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

