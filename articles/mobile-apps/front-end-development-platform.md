---
title: Visual Studio ve Azure hizmetleriyle istemci uygulamaları oluşturmak için ön uç geliştirme platformlarını seçin
description: İstemci uygulamaları oluşturmak için desteklenen yerel ve çapraz platform dilleri hakkında bilgi edinin.
author: codemillmatt
ms.service: vs-appcenter
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 714916c755ce50f6e596cee4be85ceb0b142c789
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240857"
---
# <a name="choose-mobile-development-frameworks"></a>Mobil geliştirme çerçevelerini seçin
Geliştiriciler, platformlar arası bir yaklaşım için belirli çerçeveler ve desenler kullanarak mobil uygulamaları oluşturmak için istemci tarafındaki teknolojileri kullanabilir. Geliştiriciler, karar faktörlerine bağlı olarak şunları oluşturabilir:
- Objective C ve Java gibi dilleri kullanarak yerli tek platformlu uygulamalar
- Xamarin, .NET ve C kullanarak çapraz platform uygulamaları #
- Cordova ve türevleri kullanılarak hibrid uygulamaları

## <a name="native-platforms"></a>Yerel platformlar
Yerel bir uygulama oluşturmak için platforma özgü programlama dilleri, SDK'lar, geliştirme ortamları ve işletim sistemi satıcıları tarafından sağlanan diğer araçlar gerektirir.

### <a name="ios"></a>iOS
Apple tarafından oluşturulan ve geliştirilen iOS, Apple cihazlarında, yani iPhone ve iPad'de uygulamalar oluşturmak için kullanılır.

- **Programlama dilleri**: Objective-C, Swift
- **IDE**: Xcode
- **SDK**: iOS SDK

### <a name="android"></a>Android
Google ve dünyanın en popüler işletim sistemi tarafından tasarlanan Android, çeşitli akıllı telefonlar ve tabletlerüzerinde çalıştırılabilen uygulamalar oluşturmak için kullanılır.

- **Programlama dili**: Java, Kotlin 
- **IDE**: Android Studio ve Android geliştirici araçları 
- **SDK**: Android SDK

### <a name="windows"></a>Windows
- **Programlama dili**: C #
- **IDE**: Visual Studio, Visual Studio Code
- **SDK**: Windows SDK

#### <a name="pros"></a>Artıları
- İyi kullanıcı deneyimi
- Yüksek performanslı ve yerel kitaplıklarla arayüz alabilme özelliğine sahip duyarlı uygulamalar
- Yüksek güvenli uygulamalar

#### <a name="cons"></a>Simgeler
- Uygulama yalnızca tek bir platformda çalışır
- Bir uygulama oluşturmak için daha fazla geliştirici kaynağı yoğun ve pahalı
- Alt kod yeniden kullanımı

## <a name="cross-platforms-and-hybrid-applications"></a>Platformlar arası ve hibrit uygulamalar
Platform ötesi uygulamalar, yerel mobil uygulamaları bir kez yazma, kodu paylaşma ve bunları iOS, Android ve Windows'da çalıştırma gücü verir.

### <a name="xamarin"></a>Xamarin
Microsoft'a ait olan [Xamarin,](https://visualstudio.microsoft.com/xamarin/) C#'da sağlam, çapraz platformmobil uygulamalar oluşturmak için kullanılır. Xamarin,iOS, Android ve Windows gibi birçok platformda çalışan bir sınıf kitaplığı ve çalışma süresine sahiptir. Ayrıca, yüksek performans sağlayan yerel (yorumsuz) uygulamaları da derler. Xamarin yerli platformların tüm yeteneklerini birleştirir ve kendi güçlü özellikleri bir dizi ekler.

- **Programlama dili**: C #
- **IDE**: Windows veya Mac'te Visual Studio

### <a name="react-native"></a>React Native
Facebook tarafından 2015 yılında yayımlanan [React Native,](https://facebook.github.io/react-native/) iOS ve Android için gerçek, doğal olarak mobil uygulamalar yazmak için [açık kaynaklı](https://github.com/facebook/react-native) bir JavaScript çerçevesidir. Kullanıcı arabirimleri oluşturmak için Facebook'un JavaScript kitaplığı react'e dayanır. Tarayıcıyı hedeflemek yerine mobil platformları hedefler. React Native, web bileşenleri yerine yapı taşları olarak yerel bileşenler kullanır.
 
- **Programlama dili**: JavaScript
- **IDE**: Visual Studio Kodu

### <a name="unity"></a>Unity
 Unity oyun oluşturmak için optimize edilmiş bir motordur. Windows, iOS, Android ve Xbox gibi platformlar için C# ile yüksek kaliteli 2D veya 3B uygulamalar yapmak için kullanabilirsiniz.

### <a name="cordova"></a>Cordova
Cordova, Apache Cordova için Visual Studio Tools veya Cordova uzantıları ile Visual Studio Code kullanarak hibrid uygulamalar oluşturmanıza olanak tanır. Karma yaklaşımla bileşenleri web siteleriyle paylaşabilir ve Cordova'yı temel alan barındırılan web uygulaması yaklaşımlarıyla web sunucusu tabanlı uygulamaları yeniden kullanabilirsiniz.

#### <a name="pros"></a>Artıları
- Birden çok platform için bir kod tabanı oluşturarak kod kullanılabilirliği artırıldı
- Birçok platformda daha geniş bir kitleye hitap edin
- Gelişim süresinde dramatik azalma
- Başlatması ve güncellemesi kolay

#### <a name="cons"></a>Simgeler
- Daha düşük performans
- Esneklik eksikliği
- Her platform, yerel uygulamayı daha yaratıcı hale getirmek için benzersiz bir özellik ve işlevselliğe sahiptir
- Artırılmış UI tasarım süresi
- Araç sınırlaması
