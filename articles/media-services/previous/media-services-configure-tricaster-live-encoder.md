---
title: NewTek TriCaster kodlayıcısını tek bir bit hızıcanlı yayın göndermek için yapılandırın | Microsoft Dokümanlar
description: Bu konu, Tricaster canlı kodlayıcısının canlı kodlama için etkinleştirilen AMS kanallarına tek bir bithızı akışı gönderecek şekilde nasıl yapılandırılabildiğini gösterir.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 7909fbb958a66d00616d4ed1b844d02bb47d997e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152508"
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bir bithızı canlı akışı göndermek için NewTek TriCaster kodlayıcısını kullanın  
> [!div class="op_single_selector"]
> * [Tristaster](media-services-configure-tricaster-live-encoder.md)
> * [Elemental Live](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

Bu makalede, canlı kodlama için etkinleştirilen AMS kanallarına tek bir bithızı akışı göndermek için [NewTek TriCaster](https://newtek.com/products/tricaster-40.html) canlı kodlayıcınasıl yapılandırılabilen bir yapı yada işlenir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Medya Hizmetleri Gezgini (AMSE) aracıyla Azure Medya Hizmetlerinin (AMS) nasıl yönetilenini gösterir. Bu araç yalnızca Windows PC'de çalışır. Mac veya Linux kullanıyorsanız, kanallar ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) oluşturmak [programs](media-services-portal-creating-live-encoder-enabled-channel.md)için Azure portalını kullanın.

Canlı kodlama için etkinleştirilen AMS kanallarına katkı akışı göndermek için Tricaster'ı kullanırken, canlı etkinliğinizde, canlı yayınlar arasında hızlı kesme veya kayraklara geçiş gibi tricaster'ın belirli özelliklerini kullanıyorsanız video/ses aksaklıkları olabilir. AMS ekibi bu sorunları gidermek için çalışıyor, o zamana kadar, bu özellikleri kullanmak için tavsiye edilmez.

> [!NOTE]
>  Önceden bellekli TLS sürümü olan TLS 1.2'ye taşınmayı düşünün.

## <a name="prerequisites"></a>Ön koşullar

* [Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)
* Akış Bitiş Noktası nın çalışma olduğundan emin olun. Daha fazla bilgi için bkz: [Medya Hizmetleri Hesabında Akış Uç Noktalarını Yönet](media-services-portal-manage-streaming-endpoints.md)
* [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) aracının en son sürümünü yükleyin.
* Aracı başlatın ve AMS hesabınıza bağlanın.

## <a name="tips"></a>İpuçları

* Mümkünse kablolu internet bağlantısı kullanın.
* Bant genişliği gereksinimlerini belirleme aşamasındaki temel kurallardan biri, akış bit hızlarını ikiyle çarpmaktır. Bu zorunlu bir gereksinim olmasa da, ağ tıkanıklığının etkisini azaltmaya yardımcı olur.
* Yazılım tabanlı kodlayıcılar kullanırken, gereksiz programları kapatın.

## <a name="create-a-channel"></a>Kanal oluşturma

1. AMSE aracında **Canlı** sekmesine gidin ve kanal alanı içinde sağ tıklatın. **Kanal Oluştur'u seçin...** menüden.

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Bir kanal adı belirtin, açıklama alanı isteğe bağlıdır. Kanal Ayarları altında, Giriş Protokolü **RTMP**olarak ayarlanmış Canlı Kodlama seçeneği için **Standart'ı** seçin. Diğer tüm ayarları olduğu gibi bırakabilirsiniz.

    **Yeni kanalı şimdi Başlat'ın seçildiğinden** emin olun.

3. **Kanal Oluştur'u**tıklatın.

   ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Kanalın başlaması 20 dakika kadar sürebilir.
>
>

Kanal başlatılırken, [kodlayıcıyı yapılandırabilirsiniz.](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp)

> [!IMPORTANT]
> Kanal hazır duruma geçer girmez faturalandırma başlar. Daha fazla bilgi için [Kanal'ın durumlarına](media-services-manage-live-encoder-enabled-channels.md#states)bakın.
>
>

## <a name="configure-the-newtek-tricaster-encoder"></a><a id="configure_tricaster_rtmp"/>NewTek TriCaster kodlayıcısını yapılandırın

Bu öğreticide, aşağıdaki çıktı ayarları kullanılır. Bu bölümün geri kalanı yapılandırma adımlarını daha ayrıntılı olarak açıklar.

**Video**:

* Codec: H.264
* Profil: Yüksek (Seviye 4.0)
* Bit hızı: 5000 kbps
* Anahtar kare: 2 saniye (60 saniye)
* Kare Hızı: 30

**Ses**:

* Codec: AAC (LC)
* Bit hızı: 192 kb/sn
* Örnek lem oranı: 44,1 kHz

### <a name="configuration-steps"></a>Yapılandırma adımları

1. Hangi video giriş kaynağının kullanıldığına bağlı olarak yeni bir **NewTek TriCaster** projesi oluşturun.
2. Bu projeye girdikten **sonra, Akış** düğmesini bulun ve akış yapılandırma menüsüne erişmek için yanındaki vites simgesini tıklatın.

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Menü açıldıktan sonra Bağlantı başlığı altında **Yeni'yi** tıklatın. Bağlantı türü için istendiğinde Adobe **Flash'ı**seçin.

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. **Tamam**'a tıklayın.
5. FmLE profili artık **Akış Profili** altında açılan oka tıklayarak ve **Gözat'a**yönlendirilerek içe aktarılabilir.

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Yapılandırılan FMLE profilinin kaydedildiği yere gidin.
7. Seçin ve **Tamam**tuşuna basın.

    Profil yüklendikten sonra bir sonraki adıma geçin.
8. Tricaster **RTMP Endpoint'e**atamak için kanalın giriş URL'sini alın.

    AMSE aracına geri gidin ve kanal tamamlama durumunu denetleyin. Durum **Başlangıç'tan** **Çalıştırmaya**geçtikten sonra giriş URL'sini alabilirsiniz.

    Kanal çalışırken, kanal adını sağ tıklatın, **panoya Kopya girişi URL'sinin** üzerine gidin ve ardından **Birincil Giriş URL'sini**seçin.  

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Bu bilgileri Tricaster projesi kapsamında **Flash Server** altında **Konum** alanına yapıştırın. Ayrıca, **Akış Kimliği** alanında bir akış adı atayın.

    Akış bilgileri FMLE profiline eklendiyse, Bu bölüme **Alma Ayarları'nı**tıklatarak, kaydedilen FMLE profiline gezinerek ve **Tamam'ı**tıklatarak da içe aktarılabilir. İlgili Flash Server alanları FMLE'den gelen bilgilerle doldurulmalıdır.

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Bittiğinde, ekranın alt kısmında **Tamam'ı** tıklatın. Tricaster'a video ve ses girişleri hazır olduğunda, **Akış** düğmesini tıklatarak AMS'ye akış yapmaya başlayın.

     ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> **Akış'ı**tıklatmadan önce Kanal'ın hazır olduğundan **emin** olmalısınız.
> Ayrıca, 15 dakikadan uzun bir giriş katkısı akışı olmadan Kanalı hazır bir durumda bırakmamaya özen >.
>
>

## <a name="test-playback"></a>Test oynatma

AMSE aracına gidin ve test edilecek kanala sağ tıklayın. Menüden, **Önizlemeyi Oynat'ın** üzerine geçin ve **Azure Media Player ile**seçin.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Akış oynatıcıda görünüyorsa, kodlayıcı AMS'e bağlanmak için düzgün şekilde yapılandırılmıştır.

Bir hata alınırsa, kanalın sıfırlanması ve kodlayıcı ayarlarının ayarlanması gerekir. Kılavuz için [sorun giderme](media-services-troubleshooting-live-streaming.md) makalesine bakın.  

## <a name="create-a-program"></a>Program oluşturma

1. Kanal oynatma onaylandıktan sonra bir program oluşturun. AMSE aracındaki **Canlı** sekmesinin altında, program alanı içinde sağ tıklatın ve **Yeni Program Oluştur'u**seçin.  

    ![tristaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. Programı adlandırın ve gerekirse **Arşiv Pencere Uzunluğu'nu** (varsayılan olarak dört saate göre) ayarlayın. Ayrıca bir depolama konumu belirtebilir veya varsayılan olarak bırakabilirsiniz.  
3. Programı **Başlat şimdi** kutusunu işaretleyin.
4. **Program Oluştur'u**tıklatın.  

    >[!NOTE]
    >Program oluşturma, kanal oluşturmadan daha az zaman alır.
        
5. Program çalışmaya başladığında, programı sağ tıklayarak ve **programı oynatmak** için gezinerek oynatmayı onaylayın ve ardından **Azure Media Player ile**seçim yapmayı .  
6. Onaylandıktan sonra, programı tekrar sağ tıklatın ve **Çıktı URL'sini Pano'ya Kopyala'yı** seçin (veya bu **bilgileri Menüden Program bilgileri ve ayarları** seçeneğinden alın).

Akış artık bir oynatıcıya katıştırılacak veya canlı görüntüleme için bir hedef kitleye dağıtılacak şekilde hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme

Kılavuz için [sorun giderme](media-services-troubleshooting-live-streaming.md) makalesine bakın.

## <a name="next-step"></a>Sonraki adım

Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
