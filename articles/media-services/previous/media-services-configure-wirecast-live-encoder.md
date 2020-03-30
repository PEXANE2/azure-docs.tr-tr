---
title: Telestream Wirecast kodlayıcısını tek bir bit hızı canlı akışı göndermek için yapılandırın | Microsoft Dokümanlar
description: 'Bu konu, canlı kodlama için etkinleştirilen AMS kanallarına tek bir bithızı akışı göndermek için Wirecast canlı kodlayıcısını nasıl yapılandıracağını gösterir. '
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.reviewer: cenkdin;anilmur
ms.openlocfilehash: 1d9d63aa6b3da1b8d8389722bd5af0eeed585d03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77134969"
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bir bithızı canlı akışı göndermek için Wirecast kodlayıcısını kullanın 
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Tristaster](media-services-configure-tricaster-live-encoder.md)
>
>

Bu makalede, [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) canlı kodlayıcıcanlı kodlama için etkin AMS kanallarına tek bir bithızı akışı göndermek için nasıl yapılandırılabildiğini gösterir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Medya Hizmetleri Gezgini (AMSE) aracıyla Azure Medya Hizmetlerinin (AMS) nasıl yönetilenini gösterir. Bu araç yalnızca Windows PC'de çalışır. Mac veya Linux kullanıyorsanız, kanallar ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) oluşturmak [programs](media-services-portal-creating-live-encoder-enabled-channel.md)için Azure portalını kullanın.

> [!NOTE]
> Kodlayıcılar RTMPS protokollerini kullanırken TLS 1.2'yi desteklemelidir. TLS 1.2 gereksinimi nedeniyle Wirecast sürümünü 13.0.2 veya üzeri kullanın.

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

    ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Bir kanal adı belirtin, açıklama alanı isteğe bağlıdır. Kanal Ayarları altında, Giriş Protokolü **RTMP**olarak ayarlanmış Canlı Kodlama seçeneği için **Standart'ı** seçin. Diğer tüm ayarları olduğu gibi bırakabilirsiniz.

    **Yeni kanalı şimdi Başlat'ın seçildiğinden** emin olun.

3. **Kanal Oluştur'u**tıklatın.

   ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> Kanalın başlaması 20 dakika kadar sürebilir.
>
>

Kanal başlatılırken, [kodlayıcıyı yapılandırabilirsiniz.](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp)

> [!IMPORTANT]
> Kanal hazır duruma geçer girmez faturalandırma başlar. Daha fazla bilgi için [Kanal'ın durumlarına](media-services-manage-live-encoder-enabled-channels.md#states)bakın.
>
>

## <a name="configure-the-telestream-wirecast-encoder"></a><a id="configure_wirecast_rtmp" />Telestream Wirecast kodlayıcısını yapılandırma
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
1. Kullanılan makinede Telestream Wirecast uygulamasını açın ve RTMP akışı için ayarlayın.
2. **Çıktı** sekmesine gidip Çıktı Ayarları'nı seçerek çıktıyı yapılandırın... . **Output Settings…**

    **Çıktı Hedefi'nin** **RTMP Server**olarak ayarlandıklarına emin olun.
3. **Tamam**'a tıklayın.
4. Ayarlar sayfasında, **Hedef** alanını Azure **Medya Hizmetleri**olarak ayarlayın.

    Kodlama profili **Azure H.264 720p 16:9 (1280x720)** için önceden seçilmiştir. Bu ayarları özelleştirmek için açılır bırakmanın sağındaki vites simgesini seçin ve ardından **Yeni Hazır Ayar'ı**seçin.

    ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Kodlayıcı ön ayarlarını yapılandırın.

    Önceden ayarlanmışı adlandırın ve aşağıdaki önerilen ayarları denetleyin:

    **Video**

   * Kodlayıcı: MainConcept H.264
   * Saniyede Kareler: 30
   * Ortalama bit hızı: 5000 kbit/sn (Ağ sınırlamalarına göre ayarlanabilir)
   * Profil: Ana
   * Anahtar kare her: 60 kare

     **Ses**

   * Hedef bit hızı: 192 kbit/sn
   * Örnek hızı: 44.100 kHz

     ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. **Kaydet**’e basın.

    Kodlama alanı artık seçim için yeni oluşturulan profile sahiptir.

    Yeni profilin seçildiğinden emin olun.
7. Wirecast **RTMP Endpoint'e**atamak için kanalın giriş URL'sini alın.

    AMSE aracına geri gidin ve kanal tamamlama durumunu denetleyin. Durum **Başlangıç'tan** **Çalıştırmaya**geçtikten sonra giriş URL'sini alabilirsiniz.

    Kanal çalışırken, kanal adını sağ tıklatın, **Panoya Kopya girişi URL'sinin** üzerine gitmek için aşağı gidin ve ardından **Birincil Giriş URL'sini**seçin.  

    ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. Wirecast **Çıkış Ayarları** penceresinde, bu bilgileri çıktı bölümünün **Adres** alanına yapıştırın ve bir akış adı atayın.

    ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. **Tamam'ı**seçin.
2. Ana **Wirecast** ekranında, video ve ses için giriş kaynaklarının hazır olduğunu onaylayın ve sol üst köşede **Stream'e** çarptı.

    ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> **Akış'ı**tıklatmadan önce Kanal'ın hazır olduğundan **emin** olmalısınız.
> Ayrıca, 15 dakikadan uzun bir giriş katkısı akışı olmadan Kanalı hazır bir durumda bırakmamaya özen >.
>
>

## <a name="test-playback"></a>Test oynatma

AMSE aracına gidin ve test edilecek kanala sağ tıklayın. Menüden, **Önizlemeyi Oynat'ın** üzerine geçin ve **Azure Media Player ile**seçin.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Akış oynatıcıda görünüyorsa, kodlayıcı AMS'e bağlanmak için düzgün şekilde yapılandırılmıştır.

Bir hata alınırsa, kanalın sıfırlanması ve kodlayıcı ayarlarının ayarlanması gerekir. Kılavuz için [sorun giderme](media-services-troubleshooting-live-streaming.md) makalesine bakın.  

## <a name="create-a-program"></a>Program oluşturma
1. Kanal oynatma onaylandıktan sonra bir program oluşturun. AMSE aracındaki **Canlı** sekmesinin altında, program alanı içinde sağ tıklatın ve **Yeni Program Oluştur'u**seçin.  

    ![kablolu yayın](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
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

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
