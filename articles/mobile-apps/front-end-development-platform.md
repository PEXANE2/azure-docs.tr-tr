---
title: Visual Studio ve Azure hizmetleriyle istemci uygulamaları oluşturmak için ön uç geliştirme platformunu seçin
description: İstemci uygulamaları oluşturmak için desteklenen yerel ve platformlar arası diller hakkında bilgi edinin.
author: elamalani
manager: elamalani
ms.service: vs-appcenter
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: c55c9c36d84c8545f4192e1c9a65d8af0c580396
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453148"
---
# <a name="choose-mobile-development-frameworks"></a>Mobil geliştirme çerçeveleri seçin
Geliştiriciler, platformlar arası bir yaklaşım için belirli çerçeveleri ve desenleri kullanarak mobil uygulamalar oluşturmak için istemci tarafı teknolojileri kullanabilirler. Geliştiriciler, karar faktörlerine göre şunları oluşturabilir:
- Hedef C ve Java gibi dilleri kullanarak yerel tek platformlu uygulamalar
- Xamarin, .NET ve kullanarak platformlar arası uygulamalarC#
- Cordova ve türevlerini kullanarak karma uygulamalar

## <a name="native-platforms"></a>Yerel platformlar
Yerel bir uygulama oluşturmak, platforma özgü programlama dilleri, SDK 'lar, geliştirme ortamları ve işletim sistemi satıcıları tarafından sunulan diğer araçları gerektirir.

### <a name="ios"></a>iOS
Apple tarafından oluşturulup geliştirilen iOS, iPhone ve iPad gibi Apple cihazlarında uygulamalar oluşturmak için kullanılır.

- **Programlama dilleri**: amaç-C, Swift
- **IDE**: Xcode
- **SDK**: IOS SDK 'sı

### <a name="android"></a>Android
Google ve dünyanın en popüler işletim sistemi tarafından tasarlanan Android, bir dizi akıllı telefonlar ve tablette çalışabilen uygulamalar oluşturmak için kullanılır.

- **Programlama dili**: Java, Kotlin 
- **IDE**: Android Studio ve Android Geliştirici Araçları 
- **SDK**: Android SDK

### <a name="windows"></a>Windows
- **Programlama dili**:C#
- **IDE**: Visual Studio, Visual Studio Code
- **SDK**: Windows SDK

#### <a name="pros"></a>Uzmanları
- İyi kullanıcı deneyimi
- Yüksek performansa sahip ve yerel kitaplıklarla arabirim özelliği olan yanıt veren uygulamalar
- Yüksek oranda güvenli uygulamalar

#### <a name="cons"></a>Simgeler
- Uygulama yalnızca bir platformda çalışır
- Uygulama oluşturmak için daha fazla geliştirici kaynağı kullanımı ve pahalıdır
- Daha düşük kod yeniden kullanımı

## <a name="cross-platforms-and-hybrid-applications"></a>Platformlar arası ve karma uygulamalar
Platformlar arası uygulamalar, yerel mobil uygulamaları bir kez yazmak, kod paylaşmak ve iOS, Android ve Windows 'da çalıştırmak için size güç sağlar.

### <a name="xamarin"></a>Xamarin
Microsoft 'un sahip olduğu [Xamarin](https://visualstudio.microsoft.com/xamarin/) , ' de C#güçlü, platformlar arası mobil uygulamalar oluşturmak için kullanılır. Xamarin 'in iOS, Android ve Windows gibi birçok platformda çalıştırılan bir sınıf kitaplığı ve çalışma zamanı vardır. Ayrıca, yüksek performans sağlayan yerel (Yorumlanmamış) uygulamaları derler. Xamarin, yerel platformların tüm yeteneklerini birleştirir ve kendi güçlü özelliklerinin bir sayısını ekler.

- **Programlama dili**:C#
- **IDE**: Windows veya Mac üzerinde Visual Studio

### <a name="react-native"></a>React Native
2015 ' de Facebook tarafından yayınlanan, [yerel olarak tepki](https://facebook.github.io/react-native/) verme, IOS ve Android için mobil uygulamaları gerçek zamanlı olarak işlemeye yönelik [Açık kaynaklı](https://github.com/facebook/react-native) bir JavaScript çerçevesidir. Kullanıcı arabirimleri oluşturmak için, Facebook 'un JavaScript kitaplığı tepki verme tabanlıdır. Tarayıcıyı hedeflemek yerine mobil platformları hedefler. Doğal olarak, derleme blokları olarak Web bileşenleri yerine yerel bileşenleri kullanır.
 
- **Programlama dili**: JavaScript
- **IDE**: Visual Studio Code

### <a name="unity"></a>Unity
 Unity, oyunlar oluşturmak için en iyi duruma getirilmiş bir altyapıdır. Windows, iOS, Android ve Xbox gibi platformlar C# için yüksek kaliteli 2B veya 3B uygulamalar oluşturabilirsiniz.

### <a name="cordova"></a>Cordova
Cordova, Apache Cordova için Visual Studio Araçları veya Cordova uzantıları ile Visual Studio Code kullanarak karma uygulamalar oluşturmanıza olanak tanır. Karma yaklaşım sayesinde, bileşenleri Web siteleriyle paylaşabilir ve Web sunucusu tabanlı uygulamaları Cordova tabanlı Web uygulaması yaklaşımları ile yeniden kullanabilirsiniz.

#### <a name="pros"></a>Uzmanları
- Birden çok platform için bir kod temeli oluşturarak daha fazla kod kullanılabilirliği
- Birçok platformda daha geniş bir hedef kitleye karşı karşıya
- Geliştirme sırasında çarpıcı bir azalma
- Başlatma ve güncelleştirme kolay

#### <a name="cons"></a>Simgeler
- Düşük performans
- Esneklik eksikliği
- Her platformun, yerel uygulamayı daha yaratıcı hale getirmek için benzersiz bir özellikler ve işlevler kümesi vardır
- Artan UI tasarım zamanı
- Araç sınırlaması
