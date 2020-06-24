---
title: İçeriği Azure portal şifreleyin
description: Bu hızlı başlangıçta, Azure portal Azure Media Services kullanarak içeriğiniz için şifrelemeyi nasıl yapılandıracağınız gösterilir.
ms.topic: quickstart
ms.date: 06/08/2020
ms.openlocfilehash: 6ff3c14039f9199053d3d43c6ce4488c2692b336
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988997"
---
# <a name="quickstart-use-portal-to-encrypt-content"></a>Hızlı başlangıç: içeriği şifrelemek için portalı kullanma

Ortamınızı, depolama, işleme ve teslim aracılığıyla bilgisayarınızdan tamamen ayrılmaları için güvenli hale getirmek üzere Azure Media Services kullanın. Media Services ile canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sisteminden (Microsoft PlayReady, Google Widevine ve Apple FairPlay ile) dinamik olarak dağıtabilirsiniz. Media Services, yetkili istemcilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisanslarını sunmaya yönelik bir hizmet sağlar. 
 
Akışunuzdaki şifreleme seçeneklerini (varsa) belirtmek için bir **akış ilkesi** kullanır ve bunu akış bulucuyu ilişkilendirirsiniz. İçerik anahtarının ( **varlıklarınıza**güvenli erişim sağlayan) son istemcilere teslim edildiğini yapılandırmak için **içerik anahtarı ilkesini** oluşturursunuz. Belirtilen yapılandırmaya sahip anahtarların istemcilere teslim edilebilmesi için karşılanması gereken içerik anahtarı ilkesinde gereksinimleri (kısıtlamalar) ayarlamanız gerekir. 

> [!NOTE]
> İçerik anahtarı ilkesi, açık akış veya indirme için gerekli değildir.

Bir akış bir oyuncu tarafından istendiğinde, Media Services AES şifresiz anahtar veya DRM şifrelemesini kullanarak içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır. Player, akışın şifresini çözmek için Media Services anahtar teslim hizmetinden veya belirttiğiniz anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarı almak için yetkilendirilip yetkilendirilmeyeceğine karar vermek için, hizmet, anahtar için belirttiğiniz **içerik anahtarı ilkesini** değerlendirir.

Bu hızlı başlangıçta, akışa alma sırasında varlığınıza hangi şifrelemenin uygulanacağını belirlediğiniz bir içerik anahtarı ilkesi oluşturma işlemi gösterilir. Hızlı başlangıçta, varlığınızın yapılandırılmış şifrelemenin nasıl ayarlanacağı de gösterilmektedir.

### <a name="suggested-pre-reading"></a>Önerilen önceden okuma

* [Dinamik şifreleme ve anahtar teslimi](content-protection-overview.md)
* [Akış bulucuları](streaming-locators-concept.md)
* [Akış ilkeleri](streaming-policy-concept.md)
* [İçerik anahtarı ilkeleri](content-key-policy-concept.md)

## <a name="prerequisites"></a>Ön koşullar

[Azure Portal varlıkları yönetme](manage-assets-quickstart.md) bölümünde açıklandığı gibi içeriğinizi karşıya yükleyin ve işleyin

## <a name="create-a-content-key-policy"></a>İçerik anahtarı ilkesi oluşturma

İçerik anahtarının ( **varlıklarınıza**güvenli erişim sağlayan) son istemcilere teslim edildiğini yapılandırmak için **içerik anahtarı ilkesini** oluşturun.

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. Media Services hesabınıza bulup tıklayın.
1. **İçerik anahtar ilkeleri (yeni)** seçeneğini belirleyin.
1. Pencerenin üst kısmındaki **+ içerik anahtarı Ilkesi Ekle** ' ye basın. 

**İçerik anahtar Ilkesi oluştur** penceresi görüntülenir. Bu pencerede şifreleme seçeneklerini belirleyin. Dijital hak yönetimi (DRM), Gelişmiş Şifreleme Standardı (AES) veya her ikisini de seçerek medyanızı korumayı seçebilirsiniz.  

![İçerik anahtarı ilkesi oluşturma](./media/encrypt-content-quickstart/create-content-key-policy.png)

DRM seçeneklerinden birini veya AES-128 şifresiz anahtar seçeneğini tercih etmeksizin, kısıtlamaları nasıl yapılandırmak istediğinizi belirtmeniz önerilir. Açık veya belirteç kısıtlaması olmasını seçebilirsiniz. Ayrıntılı açıklama için bkz. [içerik erişimini denetleme](content-protection-overview.md#controlling-content-access).

### <a name="add-a-drm-content-key"></a>DRM içerik anahtarı Ekle

İçeriğinizi Microsoft PlayReady ve/veya Google Widevine veya Apple FairPlay ile korumayı seçebilirsiniz. Her lisans teslim türü, kimlik bilgilerinizi temel alarak içerik anahtarlarını şifreli bir biçimde doğrular.

#### <a name="license-templates"></a>Lisans şablonları

Lisans şablonları hakkında daha fazla bilgi için bkz.

* [Google Widevine lisans şablonu](widevine-license-template-overview.md)

    > [!NOTE]
    > Değer olmadan boş bir lisans şablonu oluşturabilirsiniz, yalnızca " {} ." Ardından, varsayılan olarak bir lisans şablonu oluşturulur. Çoğu durumda varsayılan değer kullanılır.
* [Apple FairPlay lisansı gereksinimleri ve yapılandırması](fairplay-license-overview.md)
* [PlayReady lisans şablonu](playready-license-template-overview.md)

### <a name="add-aes-clear-key"></a>AES şifresiz anahtar Ekle

Ayrıca, içeriğinize bir AES-128 şifresiz anahtar şifrelemesi de ekleyebilirsiniz. İçerik anahtarı istemciye şifresiz biçimde iletilir.

![AES şifresiz anahtar](./media/encrypt-content-quickstart/aes-clear-key-policy.png)

## <a name="create-a-streaming-locator-for-your-asset"></a>Varlığınız için bir akış Bulucu oluşturma

1. Media Services hesabınıza bulup tıklayın.
1. **Varlıkları seçin (yeni)**.
1. Varlıklar listesinden, şifrelemek istediğiniz birini seçin.  
1. Seçili varlık için **akış Bulucu** bölümünde **+ akış Bulucu Ekle**' ye basın. 
1. Yapılandırdığınız **içerik anahtarı ilkesine** uygun bir **akış ilkesi** seçin.

    [Akış ilkeleri](streaming-policy-concept.md) konusu, hangi akış ilkesi ile eşleşen içerik anahtar ilkesi hakkında ayrıntılı bilgi verir.
1. Uygun akış ilkesini seçtikten sonra, açılan listeden içerik anahtarı ilkesini seçebilirsiniz.
1. Akış bulucuyu varlığınıza eklemek için **Ekle** ' ye basın.

    Bu, varlığı yayımlar ve akış URL 'Lerini oluşturur.

![Akış Bulucu](./media/encrypt-content-quickstart/multi-drm.png)

## <a name="cleanup-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

[Varlıkları yönetme](manage-assets-quickstart.md)
