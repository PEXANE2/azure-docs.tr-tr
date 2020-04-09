---
title: Azure Media Services v3 ile çevrimdışı PlayReady akışını yapılandırın
description: Bu makalede, Windows 10 için PlayReady çevrimdışı olarak oynatma kakış için Azure Medya Hizmetleri hesabınızı nasıl yapılandırabileceğiniz gösterilmektedir.
services: media-services
keywords: DASH, DRM, Widevine Çevrimdışı Mod, ExoPlayer, Android
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/01/2019
ms.author: willzhan
ms.openlocfilehash: 151aadadb5674f7f144d42b1f9d5115501ed381d
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887240"
---
# <a name="offline-playready-streaming-for-windows-10-with-media-services-v3"></a>Medya Hizmetleri v3 ile Windows 10 için Çevrimdışı PlayReady Streaming

Azure Medya Hizmetleri, DRM koruması yla çevrimdışı indirme/oynatmayı destekler. Bu makale, Windows 10/PlayReady istemcileri için Azure Medya Hizmetlerinin çevrimdışı desteğini kapsar. aşağıdaki makalelerde iOS/FairPlay ve Android/Widevine cihazları için çevrimdışı mod desteği hakkında bilgi edinebilirsiniz:

- [iOS için Çevrimdışı FairPlay Akışı](offline-fairplay-for-ios.md)
- [Android için Çevrimdışı Widevine Streaming](offline-widevine-for-android.md)

> [!NOTE]
> Çevrimdışı DRM, yalnızca içeriği karşıdan yüklediğinizde lisans için tek bir istekte bulunmak için faturalandırılır. Hatalar faturalandırılmez.

## <a name="overview"></a>Genel Bakış

Bu bölümde çevrimdışı mod oynatma bazı arka plan verir, özellikle neden:

* Bazı ülkelerde/bölgelerde, Internet kullanılabilirliği ve/veya bant genişliği hala sınırlıdır.Kullanıcılar, tatmin edici görüntüleme deneyimi için içeriği yeterince yüksek çözünürlükte izleyebilmek için önce indirmeyi seçebilir. Bu durumda, daha sık, sorun ağ kullanılabilirliği değil, daha ziyade sınırlı ağ bant genişliğidir. OTT/OVP sağlayıcıları çevrimdışı mod desteği istiyor.
* Netflix 2016 Q3 hissedar konferansında açıklandığı gibi, içerik indirme "oft-talep edilen bir özellik", ve "biz buna açığız" Netflix CEO'su Reed Hastings tarafından söyledi.
* Bazı içerik sağlayıcılar, bir ülke/bölge sınırının ötesinde DRM lisans teslimine izin verebilir. Bir kullanıcının yurt dışına seyahat etmesi gerekiyorsa ve hala içerik izlemek istiyorsa, çevrimdışı indirme gereklidir.
 
Çevrimdışı modu uygularken karşılaştığımız zorluk aşağıdakigibidir:

* MP4 birçok oyuncu, kodlayıcı araçları tarafından desteklenir, ancak MP4 konteyner ve DRM arasında hiçbir bağlama yoktur;
* Uzun vadede, CENC ile CFF gitmek için bir yoldur. Ancak, bugün, araçlar / oyuncu destek ekosistemi henüz orada değil. Bugün bir çözüme ihtiyacımız var.
 
Fikir: H264/AAC ile düzgün akış ([PIFF](https://docs.microsoft.com/iis/media/smooth-streaming/protected-interoperable-file-format)) dosya formatı PlayReady (AES-128 TO) ile bir bağlayıcıvardır. Tek tek düzgün streaming .ismv dosyası (ses video muxed varsayarak) kendisi bir fMP4 ve oynatma için kullanılabilir. Düzgün bir akış içeriği PlayReady şifrelemesi üzerinden geçerse, her .ismv dosyası PlayReady korumalı parçalı MP4 olur. Tercih edilen bitrate ile bir .ismv dosyası seçebilir ve indirmek için .mp4 olarak yeniden adlandırabilirsiniz.

Aşamalı indirmek için PlayReady korumalı MP4 barındırma için iki seçenek vardır:

* Bu MP4'ü aynı kapsayıcı/medya hizmeti varlığına koyup aşamalı olarak indirmek için Azure Medya Hizmetleri akış bitiş noktasından yararlanabilirsiniz;
* Azure Medya Hizmetlerini atlayarak Doğrudan Azure Depolama'dan kademeli olarak indirmek için SAS bulucu su kullanılabilir.
 
İki tür PlayReady lisans teslimi kullanabilirsiniz:

* Azure Medya Hizmetleri'nde PlayReady lisans teslim hizmeti;
* PlayReady lisans sunucuları her yerde barındırılan.

Aşağıda iki test varlıkları kümesi vardır, ilki AMS'de PlayReady lisans teslimini kullanırken ikincisi, Bir Azure VM'de barındırılan PlayReady lisans sunucumu kullanarak:

Varlık #1:

* Aşamalı indirme URL'si:[https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4](https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4")
* PlayReady LA_URL (AMS):[https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/](https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/)

Varlık #2:

* Aşamalı indirme URL'si:[https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4](https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4)
* PlayReady LA_URL (şirket içinde):[https://willzhan12.cloudapp.net/playready/rightsmanager.asmx](https://willzhan12.cloudapp.net/playready/rightsmanager.asmx)

Oynatma testi için Windows 10'da Evrensel Windows Uygulaması kullandık. [Windows 10 Evrensel örneklerinde,](https://github.com/Microsoft/Windows-universal-samples) [Adaptif Akış Örneği](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming)adı verilen temel bir oynatıcı örneği vardır. Tek yapmamız gereken indirilen videoyu seçip uyarlamalı akış kaynağı yerine kaynak olarak kullanmamız için kodu eklemek. Değişiklikler düğme tıklatma olay işleyicisi bulunmaktadır:

```csharp
private async void LoadUri_Click(object sender, RoutedEventArgs e)
{
    //Uri uri;
    //if (!Uri.TryCreate(UriBox.Text, UriKind.Absolute, out uri))
    //{
    // Log("Malformed Uri in Load text box.");
    // return;
    //}
    //LoadSourceFromUriTask = LoadSourceFromUriAsync(uri);
    //await LoadSourceFromUriTask;

    //willzhan change start
    // Create and open the file picker
    FileOpenPicker openPicker = new FileOpenPicker();
    openPicker.ViewMode = PickerViewMode.Thumbnail;
    openPicker.SuggestedStartLocation = PickerLocationId.ComputerFolder;
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".ismv");
    //openPicker.FileTypeFilter.Add(".mkv");
    //openPicker.FileTypeFilter.Add(".avi");

    StorageFile file = await openPicker.PickSingleFileAsync();

    if (file != null)
    {
       //rootPage.NotifyUser("Picked video: " + file.Name, NotifyType.StatusMessage);
       this.mediaPlayerElement.MediaPlayer.Source = MediaSource.CreateFromStorageFile(file);
       this.mediaPlayerElement.MediaPlayer.Play();
       UriBox.Text = file.Path;
    }
    else
    {
       // rootPage.NotifyUser("Operation cancelled.", NotifyType.ErrorMessage);
    }

    // On small screens, hide the description text to make room for the video.
    DescriptionText.Visibility = (ActualHeight < 500) ? Visibility.Collapsed : Visibility.Visible;
}
```

![PlayReady korumalı fMP4 çevrimdışı mod oynatma](./media/offline-playready-for-windows/offline-playready1.jpg)

Video PlayReady koruması altında olduğundan, ekran görüntüsü videoyu içeremez.

Özetle, Azure Medya Hizmetleri'nde çevrimdışı modu elde ettik:

* İçerik kodlaması ve PlayReady şifrelemesi Azure Medya Hizmetleri'nde veya diğer araçlarda yapılabilir;
* İçerik, aşamalı olarak indirilen Azure Medya Hizmetleri veya Azure Depolama'da barındırılabilir;
* PlayReady lisans teslimi Azure Medya Hizmetleri'nden veya başka bir yerden olabilir;
* Hazırlanan düzgün akış içeriği dash üzerinden çevrimiçi akış için kullanılabilir veya DRM olarak PlayReady ile sorunsuz.

## <a name="next-steps"></a>Sonraki adımlar

[Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
