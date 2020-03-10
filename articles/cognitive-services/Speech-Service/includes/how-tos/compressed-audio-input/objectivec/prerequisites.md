---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 28f6ef7248fada8286bbe90c868cd9f947f0435d
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78943720"
---
Sıkıştırılmış ses işleme, [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisanslama nedeniyle, GStreamer ikili dosyaları derlenmez ve konuşma SDK 'Sı ile bağlantılı değildir. Bunun yerine, bu işlevleri içeren bir sarmalayıcı kitaplığın SDK kullanılarak uygulamalarla oluşturulması ve birlikte kullanılması gerekir.

Bu sarmalayıcı kitaplığı oluşturmak için önce [GStreamer SDK 'sını](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg)indirip yükleyin. Ardından, [sarmalayıcı kitaplığı](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper)Için **Xcode** projesini indirin.

Projeyi **Xcode** 'da açın ve **genel iOS cihaz** hedefi için oluşturun; Bu, belirli bir hedef için oluşturmak *için çalışmaz.*

Yapı adımı, `GStreamerWrapper.framework`adı ile tüm gerekli mimarilere yönelik dinamik bir kitaplık içeren dinamik bir çerçeve paketi oluşturur.

Bu çerçevenin konuşma hizmeti SDK 'Sı ile sıkıştırılmış ses akışları kullanan tüm uygulamalara dahil olması gerekir.

Bunu gerçekleştirmek için aşağıdaki ayarları **Xcode** projenize uygulayın:

1. Yeni oluşturduğunuz `GStreamerWrapper.framework` ve bilişsel hizmetler konuşma SDK 'sının çerçevesini, [buradan](https://aka.ms/csspeech/iosbinary)indirebilirsiniz, örnek projenizi içeren dizine kopyalayın.
1. *Proje ayarlarındaki*çerçeveler için yolları ayarlayın.
   1. **Katıştırılmış Ikili dosyalar** başlığı altındaki **genel** sekmesinde, bir Framework olarak SDK Kitaplığı ekleyin: **katıştırılmış ikililer Ekle** > **diğer...** > seçtiğiniz dizine gidin ve her iki çerçeveyi seçin.
   1. **Derleme Ayarları** sekmesine gidin ve **Tümü** ayarını etkinleştirin.
1. `$(SRCROOT)/..` dizinini _Arama Yolları_ başlığı altında **Çerçeve Arama Yolları**’na ekleyin.
