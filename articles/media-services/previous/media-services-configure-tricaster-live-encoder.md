---
title: Tek bit hızlı canlı akış göndermek için NewTek karmaşık Aster Kodlayıcısı 'nı yapılandırın | Microsoft Docs
description: Bu konuda, karmaşık kodlama için etkinleştirilmiş AMS kanallarına tek bit hızlı bir akış göndermek için, karmaşık kodlayıcının nasıl yapılandırılacağı gösterilmektedir.
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
ms.openlocfilehash: 11ee8f52a8fd4db2d052eeaeef1387b011d23050
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131561"
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bit hızlı canlı akış göndermek için NewTek karmaşık Aster Kodlayıcısı 'nı kullanın  
> [!div class="op_single_selector"]
> * [TriCaster](media-services-configure-tricaster-live-encoder.md)
> * [Elete canlı](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

Bu makalede, [NewTek](https://newtek.com/products/tricaster-40.html) bir bit hızı Live Encoder 'ın canlı kodlama için etkinleştirilmiş AMS kanallarına tek bir bit hızlı akış göndermek için nasıl yapılandırılacağı gösterilmektedir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğreticide, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetilecek gösterilmektedir. Bu araç yalnızca Windows bilgisayarda çalışır. Mac veya Linux kullanıyorsanız, [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programları](media-services-portal-creating-live-encoder-enabled-channel.md)oluşturmak için Azure Portal kullanın.

> [!NOTE]
> Gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallarına bir katkı akışında göndermek üzere karmaşık bir şekilde kullanırken,, akışlar arasında hızlı kesme veya SLA 'lara geçiş yapma gibi karmaşık olaylarınızın belirli özelliklerini kullanırsanız canlı olayınızdaki video/ses bozuklukları olabilir. AMS ekibi bu sorunları gidermeye çalışarak, bu özelliklerin kullanılması önerilmez.
>
>

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

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Bir kanal adı belirtirseniz açıklama alanı isteğe bağlıdır. Kanal ayarları altında, Live Encoding seçeneği için **Standart** ' ı seçerek giriş Protokolü **RTMP**olarak ayarlanır. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    **Yeni kanalı Şimdi Başlat** ' ın seçildiğinden emin olun.

3. **Kanal oluştur**' a tıklayın.

   ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Kanalı başlatmak için 20 dakika sürebilir.
>
>

Kanal Başlarken, [Encoder 'ı yapılandırabilirsiniz](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Faturalandırma, kanalın bir hazır durumuna geçtiğinde hemen sonra başlar. Daha fazla bilgi için bkz. [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a name="a-idconfigure_tricaster_rtmpconfigure-the-newtek-tricaster-encoder"></a>NewTek karmaşık Aster Encoder 'ı yapılandırma <a id="configure_tricaster_rtmp"/>

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

1. Hangi video giriş kaynağının kullanılmakta olduğuna bağlı olarak yeni bir **NewTek karmaşık Aster** projesi oluşturun.
2. Bu proje içinde bir kez, **Stream** düğmesini bulun ve yanındaki dişli simgesine tıklayarak akış yapılandırma menüsüne erişin.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Menü açıldıktan sonra bağlantı başlığının altındaki **Yeni** ' ye tıklayın. Bağlantı türü sorulduğunda **Adobe Flash**' ı seçin.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. **Tamam**’a tıklayın.
5. Bir FMLE profili artık **akış profili** altında aşağı açılan oka tıklayarak ve **gözatmaya**karşı gezinerek içeri aktarılabilir.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Yapılandırılan FMıLE profilinin kaydedildiği yere gidin.
7. Seçin ve **Tamam**' a basın.

    Profil karşıya yüklendikten sonra, sonraki adıma geçin.
8. Kanalın giriş URL 'sini, bunu karmaşık Aster **RTMP uç noktasına**atamak için alın.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durum, **çalışmaya** **BAŞLADıKTAN sonra** giriş URL 'sini alabilirsiniz.

    Kanal çalışırken kanal adına sağ tıklayın, **giriş URL 'sini panoya kopyala** üzerine gelin ve ardından **birincil giriş URL 'sini**seçin.  

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Bu bilgileri, karmaşık Aster projesindeki **Flash Server** altındaki **konum** alanına yapıştırın. Ayrıca **AKıŞ kimliği** alanına bir akış adı atayın.

    Akış bilgileri FMLE profiline eklendiyse, **ayarları Içeri aktar**' a tıklayarak, kaydedilen FMLE profiline giderek ve **Tamam**' a tıklayarak bu bölüme de aktarılabilir. İlgili Flash Server alanları FMLE 'nin bilgileriyle doldurulmalıdır.

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. İşiniz bittiğinde, ekranın altındaki **Tamam** ' a tıklayın. Daha karmaşık bir şekilde video ve ses girişleri hazırlandıktan sonra **akış** düğmesine tıklayarak AMS 'ye akışa başlayın.

     ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> **Stream**' e tıklamadan önce kanalın Ready olduğundan emin **olmanız gerekir** .
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakmamaya emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme

AMSE aracına gidin ve test edilecek kanal sağ tıklayın. Menüden **önizlemeyi kayıttan yürütme** üzerine gelin ve **Azure Media Player**seçin.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Ardından akış Player'da görünüyorsa, kodlayıcı düzgün AMS'ye bağlanmak için yapılandırıldı.

Bir hata alınıyorsa, kanalın sıfırlanması ve kodlayıcı ayarlarının ayarlanması gerekir. Rehberlik için [sorun giderme](media-services-troubleshooting-live-streaming.md) makalesine bakın.  

## <a name="create-a-program"></a>Bir program oluşturma

1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. AMO aracının **canlı** sekmesi altında, program alanının içine sağ tıklayıp **yeni program oluştur**' u seçin.  

    ![TriCaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
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

## <a name="next-step"></a>Sonraki adım

Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
