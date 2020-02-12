---
title: Tek bit hızlı canlı akış göndermek için Telestream Wirecast Kodlayıcı yapılandırma | Microsoft Docs
description: 'Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS göndermek için Wirecast Canlı Kodlayıcı yapılandırma gösterilmektedir. '
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
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77134969"
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Wirecast Kodlayıcı tek bit hızlı canlı akış göndermektir. 
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [TriCaster](media-services-configure-tricaster-live-encoder.md)
>
>

Bu makalede, [Telestream kablolu yayın](https://www.telestream.net/wirecast/overview.htm) Live Encoder 'ın, canlı kodlama için etkinleştirilmiş AMS kanallarına tek bir bit hızı akışı göndermek üzere nasıl yapılandırılacağı gösterilmektedir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğreticide, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetilecek gösterilmektedir. Bu araç yalnızca Windows bilgisayarda çalışır. Mac veya Linux kullanıyorsanız, [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programları](media-services-portal-creating-live-encoder-enabled-channel.md)oluşturmak için Azure Portal kullanın.

> [!NOTE]
> RTMPS protokolleri kullanılırken kodlayıcılar TLS 1,2 ' i desteklemelidir. TLS 1,2 gereksinimi nedeniyle kablolu yayın sürümü 13.0.2 veya üstünü kullanın.

## <a name="prerequisites"></a>Önkoşullar
* [Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)
* Çalışan bir akış uç bulunduğundan emin olun. Daha fazla bilgi için bkz. [Media Services hesapta akış uç noktalarını yönetme](media-services-portal-manage-streaming-endpoints.md)
* [Ami](https://github.com/Azure/Azure-Media-Services-Explorer) aracının en son sürümünü yükler.
* Aracı'nı başlatın ve AMS hesabınızı bağlayın.

## <a name="tips"></a>İpuçları
* Mümkün olduğunda, bir sabit internet bağlantısı kullanın.
* Bir iyi bant genişliği gereksinimlerini belirlerken için udur akış bit hızlarına dönüştürme çift. Bu zorunlu bir gereksinim olmamasına karşın, Ağ Tıkanıklığı etkisini azaltmaya yardımcı olur.
* Yazılım tabanlı kodlayıcılar kullanırken, gereksiz tüm programları kapatın.

## <a name="create-a-channel"></a>Kanal oluşturma
1. AMI aracında, **canlı** sekmesine gidin ve kanal alanının içine sağ tıklayın. Kanal oluştur ' u seçin **...** belirleyin.

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Bir kanal adı belirtirseniz açıklama alanı isteğe bağlıdır. Kanal ayarları altında, Live Encoding seçeneği için **Standart** ' ı seçerek giriş Protokolü **RTMP**olarak ayarlanır. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    **Yeni kanalı Şimdi Başlat** ' ın seçildiğinden emin olun.

3. **Kanal oluştur**' a tıklayın.

   ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> Kanalı başlatmak için 20 dakika sürebilir.
>
>

Kanal Başlarken, [Encoder 'ı yapılandırabilirsiniz](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> Faturalandırma, kanalın bir hazır durumuna geçtiğinde hemen sonra başlar. Daha fazla bilgi için bkz. [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a name="a-idconfigure_wirecast_rtmp-configure-the-telestream-wirecast-encoder"></a>Telestream Kablotür kodlayıcısını yapılandırma <a id="configure_wirecast_rtmp" />
Bu öğreticide, aşağıdaki çıkış ayarları kullanılır. Bu bölümün geri kalanında daha ayrıntılı yapılandırma adımlarını açıklar.

**Video**:

* Codec: H.264
* Profil: Yüksek (düzeyi 4.0)
* Bit hızı: 5000 KB/sn
* Ana kare: 2 saniye (60 saniye)
* Kare oranı: 30

**Ses**:

* Codec: AAC (LC)
* Bit hızı: 192 Kb/sn
* Örnek Hızı: 44,1 kHz

### <a name="configuration-steps"></a>Yapılandırma adımları
1. Kullanılan ve RTMP akış için ayarlanmış olan makine üzerinde Telestream Wirecast uygulamayı açın.
2. **Çıkış sekmesine gidip** çıkış ayarları ' nı seçerek çıktıyı yapılandırın **...**

    **Çıktı hedefinin** **RTMP sunucusu**olarak ayarlandığından emin olun.
3. **Tamam**’a tıklayın.
4. Ayarlar sayfasında, **hedef** alanı **Azure Media Services**olarak ayarlayın.

    Kodlama profili **Azure H. ıbu 16:9 (1280x720)** için önceden seçilmiştir. Bu ayarları özelleştirmek için, açılan kutudan sağ tarafındaki dişli simgesini seçin ve ardından **Yeni önayar**' ı seçin.

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Encoder hazır ayarlarını yapılandırın.

    Ön ayarın adını ve önerilen ayarları için şunları denetleyin:

    **Video**

   * Kodlayıcı: MainConcept H.264
   * Saniyedeki: 30
   * Ortalama bit hızı: 5000 kbit/sn (ayarlanabilir ağ sınırlamalar tabanlı)
   * Profil: ana
   * Anahtar çerçeve her: 60 çerçeveler

     **Müzik**

   * Hedef bit hızı: 192 kbit/sn
   * Örnek Hızı: 44.100 kHz

     ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. **Kaydet**’e basın.

    Yeni oluşturulan profil seçilebilir Encoding alanı artık var.

    Yeni profili seçili olduğundan emin olun.
7. Kablolu atama **RTMP uç noktasına**atamak için kanalın giriş URL 'sini alın.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durum, **çalışmaya** **BAŞLADıKTAN sonra** giriş URL 'sini alabilirsiniz.

    Kanal çalışırken kanal adına sağ tıklayın, **giriş URL 'sini panoya kopyala** üzerine gelin ve ardından **birincil giriş URL 'sini**seçin.  

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. Kablolu yayın **Çıkış ayarları** penceresinde, bu bilgileri çıkış bölümünün **Adres** alanına yapıştırın ve bir akış adı atayın.

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. **Tamam**’ı seçin.
2. Ana **kablolu yayın** ekranında, video ve ses için giriş kaynaklarını onaylayın ve sonra sol üst köşedeki **akış** ' ı ziyaret edin.

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> **Stream**' e tıklamadan önce kanalın Ready olduğundan emin **olmanız gerekir** .
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakmamaya emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme

AMSE aracına gidin ve test edilecek kanal sağ tıklayın. Menüden **önizlemeyi kayıttan yürütme** üzerine gelin ve **Azure Media Player**seçin.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Ardından akış Player'da görünüyorsa, kodlayıcı düzgün AMS'ye bağlanmak için yapılandırıldı.

Bir hata aldıysanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Rehberlik için [sorun giderme](media-services-troubleshooting-live-streaming.md) makalesine bakın.  

## <a name="create-a-program"></a>Bir program oluşturma
1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. AMO aracının **canlı** sekmesi altında, program alanının içine sağ tıklayıp **yeni program oluştur**' u seçin.  

    ![Wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. Programı adlandırın ve gerekirse **Arşiv penceresi uzunluğunu** (varsayılan olarak dört saat) ayarlayın. Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. **Şimdi programa başla** kutusunu işaretleyin.
4. **Program oluştur**' a tıklayın.  

   >[!NOTE]
   >Program oluşturma kanal oluşturmayı daha az zaman alır.
       
5. Program çalışmaya başladıktan sonra programa sağ tıklayıp **programları kayıttan yürütmeyi** ve sonra **Azure Media Player**' yi seçerek kayıttan yürütmeyi onaylayın.  
6. Onaylandıktan sonra programa yeniden sağ tıklayın ve **Çıkış URL 'Sini panoya kopyala** ' yı seçin (veya bu bilgileri menüdeki **Program bilgileri ve ayarlar** seçeneğinden alın).

Akış bir oynatıcı içinde gömülü veya canlı görüntülemek için bir hedef kitle için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme
Rehberlik için [sorun giderme](media-services-troubleshooting-live-streaming.md) makalesine bakın.

## <a name="media-services-learning-paths"></a>Media Services öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
