---
title: İçeriği karşıya yüklemek, kodlamak ve akışa almak için Portal 'ı kullanma-Azure
description: Bu hızlı başlangıçta, Azure Media Services ile içeriği karşıya yüklemek, kodlamak ve akışa almak için Portal 'ın nasıl kullanılacağı gösterilir.
ms.topic: quickstart
ms.date: 08/31/2020
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femila
ms.openlocfilehash: 16a5793f9848a6e16856728877727dbefc3e0805
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89264938"
---
# <a name="quickstart-upload-encode-and-stream-content-with-portal"></a>Hızlı başlangıç: portala karşıya içerik yükleme, kodlama ve akışla

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu hızlı başlangıçta, Azure Media Services ile içerik yükleme, kodlama ve akışa alma için Azure portal nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> Gözden geçirdiğinizden emin olun: [Media Services v3 için Azure Portal sınırlamaları](frequently-asked-questions.md#what-are-the-azure-portal-limitations-for-media-services-v3).
  
## <a name="overview"></a>Genel Bakış

* Azure 'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, çözümlemeye ve akışla başlamak için bir Media Services hesabı oluşturmanız ve yüksek kaliteli dijital medya dosyanızı bir **varlığa**yüklemeniz gerekir. 
    
    > [!NOTE]
    > Videonuz daha önce Media Services v3 API kullanılarak Media Services hesabına yüklenmişse veya içerik canlı bir çıkışa göre oluşturulduysa, Azure portal **kodlama**, **Çözümleme**veya **şifreleme** düğmelerini görmezsiniz. Bu görevleri gerçekleştirmek için Media Services v3 API 'Lerini kullanın.

    Aşağıdakileri gözden geçirin: 

  * [Bulutta karşıya yükleme ve depolama](storage-account-concept.md)
  * [Varlık kavramı](assets-concept.md)
* Yüksek kaliteli dijital medya dosyanızı bir varlığa (bir giriş varlığı) yükledikten sonra, (kodlama veya çözümleme) işlemi gerçekleştirebilirsiniz. İşlenen içerik başka bir varlığa (çıkış varlığı) gider. 
    * Karşıya yüklenen Dosyanızı çok çeşitli tarayıcılarda ve cihazlarda yürütülebilecek biçimlerde [kodlayın](encoding-concept.md) .
    * Karşıya yüklenen dosyayı [çözümleyin](analyzing-video-audio-files-concept.md) . 

        Şu anda Azure portal kullanılırken şunları yapabilirsiniz: bir TTML ve WebVTT kapalı açıklamalı altyazı dosyaları oluşturma. Bu biçimlerdeki dosyalar, ses ve video dosyalarını işitme engelli kişiler için erişilebilir hale getirmek için kullanılabilir. Ayrıca, İçeriğinizden anahtar sözcükleri de ayıklayabilirsiniz.

        Video ve Ses dosyalarınızın içgörüleri ayıklamanızı sağlayan zengin bir deneyim için Media Services v3 ön ayarlarını kullanın ( [öğretici: Media Services v3 ile videoları analiz edin](analyze-videos-tutorial-with-api.md)). <br/>Daha ayrıntılı Öngörüler istiyorsanız [video Indexer](../video-indexer/index.yml) doğrudan kullanın.    
* İçeriğiniz işlendikten sonra, istemci oynatıcılara medya içeriği gönderebilirsiniz. Çıktı kıymetindeki videoların, kayıttan yürütmeye yönelik olarak kullanılabilmesini sağlamak için bir **akış Bulucu**oluşturmanız gerekir. **Akış bulucuyu**oluştururken bir **akış ilkesi**belirtmeniz gerekir. Akış **ilkeleri** , akış **Konumlandırıcıları**için akış protokollerini ve şifreleme seçeneklerini (varsa) tanımlamanıza olanak sağlar.
    
    İncelemeyi

    * [Akış bulucuları](streaming-locators-concept.md)
    * [Akış ilkeleri](streaming-policy-concept.md)
    * [Paketleme ve teslim](dynamic-packaging-overview.md)
    * [Filtreler](filters-concept.md)
* İçeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya/ve üç ana DRM sisteminden (Microsoft PlayReady, Google Widevine ve Apple FairPlay) şifreleyerek koruyabilirsiniz. Azure portal hızlı başlangıç [ile Içerik şifreleme](encrypt-content-quickstart.md) içerik korumasının nasıl yapılandırılacağını gösterir.
        
## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[Media Services hesabı oluşturma](create-account-howto.md#use-the-azure-portal)

## <a name="upload"></a>Karşıya Yükleme

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. Media Services hesabınıza bulup tıklayın.
1. **Varlıkları seçin (yeni)**.
1. Pencerenin üst kısmındaki **karşıya yükle** ' ye basın. 
1. Karşıya yüklemek istediğiniz bir dosyaya sürükleyip bırakın veya tarayın.

Varlıklar pencerenize gittiğinizde, listeye yeni bir varlık eklendiğini görürsünüz:

![Karşıya Yükleme](./media/manage-assets-quickstart/upload.png)

## <a name="encode"></a>Kodlama

1. **Varlıkları seçin (yeni)**.
1. Yeni varlığınızı (son adımla eklenen) seçin.
1. Pencerenin üst kısmındaki **kodla** ' ya tıklayın.

    Bu düğmeye basıldığında kodlama işi başlatılır. Başarıyla tamamlandığında, kodlanmış içeriği içeren bir çıkış varlığı oluşturur.

Varlıklar pencerenize gittiğinizde, çıkış varlığının listeye eklendiğini görürsünüz:

![Kodlama](./media/manage-assets-quickstart/encode.png)

## <a name="monitor-the-job-progress"></a>İşin ilerlemesini izleme

İş durumunu görüntülemek için **işler**' e gidin. İş, genellikle şu durumlardan geçer: zamanlanmış, sıraya alınmış, Işleme, tamamlandı (son durum). İş bir hatayla karşılaştıysa Hata durumunu alırsınız.

![Durum](./media/manage-assets-quickstart/job-status.png)

## <a name="publish-and-stream"></a>Yayımla ve akış

Bir varlığı yayımlamak için, şimdi varlığınıza bir akış Bulucu eklemeniz gerekir.

### <a name="streaming-locator"></a>Akış Bulucu 

1. **Akış Bulucu** bölümünde **+ akış Bulucu Ekle**' ye basın.
    Bu, varlığı yayımlar ve akış URL 'Lerini oluşturur.

    > [!NOTE]
    > Akışın şifrelenmesini istiyorsanız, içerik anahtar ilkesi oluşturmanız ve akış bulucunun üzerinde ayarlamanız gerekir. Ayrıntılar için bkz. [Azure Portal Içeriği şifreleme](encrypt-content-quickstart.md).
1. **Akış Bulucu Ekle** penceresinde, önceden tanımlanmış akış ilkelerinden birini seçersiniz. Ayrıntılı bilgi için bkz. [akış ilkeleri](streaming-policy-concept.md)

    ![Akış Bulucu](./media/manage-assets-quickstart/streaming-locator.png)

Varlık yayımlandıktan sonra portalda doğrudan akış yapabilirsiniz. 

![Oynama](./media/manage-assets-quickstart/publish.png)

Ya da akış URL 'sini kopyalayın ve istemci oynatıcınızda kullanın.

> [!NOTE]
> [Akış uç noktasının](streaming-endpoint-concept.md) çalıştığından emin olun. İlk olarak bir medya hizmeti hesabı oluşturduğunuzda, varsayılan akış uç noktası oluşturulur ve durdurulmuş durumdaysa, içeriğinizi akışa almadan önce bunu başlatmanız gerekir.<br/>Yalnızca akış uç noktanız çalışır durumdaysa faturalandırılırsınız.

## <a name="cleanup-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

[Portal kullanarak içeriği şifreleme](encrypt-content-quickstart.md)
