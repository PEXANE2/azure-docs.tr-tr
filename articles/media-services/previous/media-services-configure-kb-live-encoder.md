---
title: Haivision KB kodlayıcısını Azure'a tek bir bit hızı canlı akışı gönderecek şekilde yapılandırın | Microsoft Dokümanlar
description: Bu konu, canlı kodlama için etkinleştirilen AMS kanallarına tek bir bithızı akışı göndermek için Haivision KB canlı kodlayıcıyı nasıl yapılandıracağını gösterir.
services: media-services
documentationcenter: ''
author: dbgeorge
manager: vsood
editor: ''
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: babb5a10818c8108bae34402962fd2503dbecbdc
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641691"
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bir bithızıcanlı yayın göndermek için Haivision KB canlı kodlayıcısını kullanın  
> [!div class="op_single_selector"]
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)

Bu konu, [havision KB canlı kodlayıcı](https://www.haivision.com/products/kb-series/) kodlayıcısının canlı kodlama için etkinleştirilen AMS kanallarına tek bir bithızı akışı göndermek üzere nasıl yapılandırılabildiğini gösterir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Medya Hizmetleri Gezgini (AMSE) aracıyla Azure Medya Hizmetlerinin (AMS) nasıl yönetilenini gösterir. Bu araç yalnızca Windows PC'de çalışır. Mac veya Linux kullanıyorsanız, kanallar ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) oluşturmak [programs](media-services-portal-creating-live-encoder-enabled-channel.md)için Azure portalını kullanın.

## <a name="prerequisites"></a>Ön koşullar
*   SW v5.01 veya daha büyük çalışan bir Haivision KB kodlayıcıya erişim.
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
[Haivision](./media/media-services-configure-kb-live-encoder/channel.png)
2. Bir kanal adı belirtin, açıklama alanı isteğe bağlıdır. Kanal Ayarları altında, Giriş Protokolü **RTMP**olarak ayarlanmış Canlı Kodlama seçeneği için **Standart'ı** seçin. Diğer tüm ayarları olduğu gibi bırakabilirsiniz. **Yeni kanalı şimdi Başlat'ın seçildiğinden** emin olun.
3. **Kanal Oluştur'u**tıklatın.
[Haivision](./media/media-services-configure-kb-live-encoder/livechannel.png)

> [!NOTE]
> Kanalın başlaması 20 dakika kadar sürebilir.

## <a name="configure-the-haivision-kb-encoder"></a>Haivision KB kodlayıcısını yapılandırın
Bu öğreticide, aşağıdaki çıktı ayarları kullanılır. Bu bölümün geri kalanı yapılandırma adımlarını daha ayrıntılı olarak açıklar.

Video:
-   Codec: H.264
-   Profil: Yüksek (Seviye 4.0)
-   Bit hızı: 5000 kbps
-   Anahtar kare: 2 saniye (60 kare)
-   Kare Hızı: 30

Ses:
-   Codec: AAC (LC)
-   Bit hızı: 192 kb/sn
-   Örnek lem oranı: 44,1 kHz

## <a name="configuration-steps"></a>Yapılandırma adımları
1.  Haivision KB kullanıcı arayüzüne giriş yapın.
2.  Kanal kontrol merkezindeki **Menü Düğmesine** tıklayın ve **Kanal Ekle'yi** seçin  
    ![Ekran Görüntüsü 2017-08-14 saat 09.15.09](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Ad alanına **Kanal Adı** yazın ve sonrakini tıklatın.  
    ![Ekran Görüntüsü 2017-08-14 saat 09.19.07](./media/media-services-configure-kb-live-encoder/step3.png)
4.  **Giriş Kaynağı** açılır tarafından Kanal **Giriş Kaynağı'nı** seçin ve sonrakini tıklatın.
    ![Ekran Görüntüsü 2017-08-14 saat 09.20.44](./media/media-services-configure-kb-live-encoder/step4.png)
5.  **Encoder Şablonaçılır** tarafından **H264-720-AAC-192'yi** seçin ve sonrakini tıklatın.
    ![Ekran Görüntüsü 2017-08-14 saat 09.23.15](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Yeni **Çıktı** yı seç'ten **RTMP'yi** seçin ve sonrakini tıklatın.  
    ![Ekran Görüntüsü 2017-08-14 saat 09.27.51](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Kanal **Çıktısı** penceresinden Azure akışı bilgilerini doldurun. **SUNUCU** alanındaki ilk kanal kurulumundan **RTMP** bağlantısını yapıştırın. Kanal adına **Çıktı Adı** alanında. Akış Adı Şablonu alanında, akışı adlandırmak için şablonu %RTMPStreamName_%video_bitrate kullanın.
    ![Ekran Görüntüsü 2017-08-14 saat 09.33.17](./media/media-services-configure-kb-live-encoder/step7.png)
8.  İleri'yi tıklatın ve sonra Bitti'yi tıklatın.
9.  Kodlayıcı kanalını başlatmak için **Oynat düğmesini** tıklatın.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Test oynatma
AMSE aracına gidin ve test edilecek kanala sağ tıklayın. Menüden, Önizlemeyi Oynat'ın üzerine geçin ve Azure Media Player ile seçin.

Akış oynatıcıda görünüyorsa, kodlayıcı AMS'e bağlanmak için düzgün şekilde yapılandırılmıştır.

Bir hata alınırsa, kanalın sıfırlanması ve kodlayıcı ayarlarının ayarlanması gerekir. Kılavuz için sorun giderme makalesine bakın.

## <a name="create-a-program"></a>Program oluşturma
1.  Kanal oynatma onaylandıktan sonra bir program oluşturun. AMSE aracındaki Canlı sekmesinin altında, program alanı içinde sağ tıklatın ve Yeni Program Oluştur'u seçin.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  Programı adlandırın ve gerekirse Arşiv Pencere Uzunluğu'nu (varsayılan olarak dört saate göre) ayarlayın. Ayrıca bir depolama konumu belirtebilir veya varsayılan olarak bırakabilirsiniz.
2.  Programı Başlat şimdi kutusunu işaretleyin.
3.  Program Oluştur'u tıklatın.
4.  Program çalışmaya başladığında, programı sağ tıklayarak ve programı oynatmak için gezinmek ve ardından Azure Media Player ile seçim yaparak oynatmaonaylayın.
5.  Onaylandıktan sonra, programı tekrar sağ tıklatın ve Çıktı URL'sini Pano'ya Kopyala'yı seçin (veya bu bilgileri Menüden Program bilgileri ve ayarları seçeneğinden alın).

Akış artık bir oynatıcıya katıştırılacak veya canlı görüntüleme için bir hedef kitleye dağıtılacak şekilde hazırdır.

> [!NOTE]
> Program oluşturma, kanal oluşturmadan daha az zaman alır.
