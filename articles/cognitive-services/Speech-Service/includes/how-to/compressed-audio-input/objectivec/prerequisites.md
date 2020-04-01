---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 28f6ef7248fada8286bbe90c868cd9f947f0435d
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409607"
---
Sıkıştırılmış ses işleme [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisans lama nedenleriyle GStreamer ikilileri derlenmez ve Konuşma SDK ile bağlantılı değildir. Bunun yerine, bu işlevleri içeren bir sarmalayıcı kitaplığı oluşturulması ve SDK kullanarak uygulamalarla birlikte gönderilmesi gerekir.

Bu sarıcı kitaplığını oluşturmak için, ilk olarak [GStreamer SDK'yı](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg)indirin ve kurun. Ardından, [sarmalayıcı kitaplığı](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper)için **Xcode** projesini indirin.

Projeyi **Xcode'da** açın ve **Genel iOS Aygıt** hedefi için oluşturun - belirli bir hedef için oluşturmak için *çalışmaz.*

Yapı adımı adı ile gerekli tüm mimariler için dinamik bir kütüphane `GStreamerWrapper.framework`ile dinamik bir çerçeve paketi oluşturacaktır.

Bu çerçeve, Konuşma hizmeti SDK ile sıkıştırılmış ses akışları kullanan tüm uygulamalara dahil edilmelidir.

Bunu gerçekleştirmek için **Xcode** projenizde aşağıdaki ayarları uygulayın:

1. Kopya `GStreamerWrapper.framework` sadece inşa ve Bilişsel Hizmetler Konuşma SDK çerçeve, [buradan](https://aka.ms/csspeech/iosbinary)indirebilirsiniz , örnek proje içeren dizine.
1. *Yolları Proje Ayarları'ndaki*çerçevelere ayarlayın.
   1. **Gömülü İkili üstbilginin** altındaki **Genel** sekmesinde, SDK kitaplığını çerçeve olarak ekleyin: **Katıştırılmış ikililer** > **ekleyin Diğerini ekleyin...** > Seçtiğiniz dizine gidin ve her iki çerçeveyi seçin.
   1. **Derleme Ayarları** sekmesine gidin ve **Tümü** ayarını etkinleştirin.
1. `$(SRCROOT)/..` dizinini **Arama Yolları** başlığı altında _Çerçeve Arama Yolları_’na ekleyin.
