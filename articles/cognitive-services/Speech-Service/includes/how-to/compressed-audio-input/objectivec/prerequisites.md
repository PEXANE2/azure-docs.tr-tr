---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 7106e139108681e1908b20d2daac5e619a63555d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81421987"
---
Sıkıştırılmış ses işleme, [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisanslama nedeniyle, GStreamer ikili dosyaları derlenmez ve konuşma SDK 'Sı ile bağlantılı değildir. Bunun yerine, bu işlevleri içeren bir sarmalayıcı kitaplığın SDK kullanılarak uygulamalarla oluşturulması ve birlikte kullanılması gerekir.

Bu sarmalayıcı kitaplığı oluşturmak için önce [GStreamer SDK 'sını](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg)indirip yükleyin. Ardından, [sarmalayıcı kitaplığı](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper)Için **Xcode** projesini indirin.

Projeyi **Xcode** 'da açın ve **genel iOS cihaz** hedefi için oluşturun; Bu, belirli bir hedef için oluşturmak *için çalışmaz.*

Derleme adımı, adı taşıyan tüm gerekli mimarilere yönelik dinamik bir kitaplık içeren dinamik bir çerçeve paketi oluşturur `GStreamerWrapper.framework`.

Bu çerçevenin konuşma hizmeti SDK 'Sı ile sıkıştırılmış ses akışları kullanan tüm uygulamalara dahil olması gerekir.

Bunu gerçekleştirmek için aşağıdaki ayarları **Xcode** projenize uygulayın:

1. `GStreamerWrapper.framework` [Buradan](https://aka.ms/csspeech/iosbinary)Indirebileceğiniz bilişsel hizmetler konuşma SDK 'sının oluşturduğunuz ve çerçevesini, örnek projenizi içeren dizine kopyalayın.
1. *Proje ayarlarındaki*çerçeveler için yolları ayarlayın.
   1. **Katıştırılmış ikili dosyaların** altındaki **genel** sekmesinde, bir Framework olarak SDK Kitaplığı ekleyin: **katıştırılmış ikililer** > Ekle**başka...** > seçtiğiniz dizine gidin ve her iki çerçeveyi seçin.
   1. **Derleme Ayarları** sekmesine gidin ve **Tümü** ayarını etkinleştirin.
1. `$(SRCROOT)/..` dizinini **Arama Yolları** başlığı altında _Çerçeve Arama Yolları_’na ekleyin.
