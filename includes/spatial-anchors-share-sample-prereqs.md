---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 07/31/2020
ms.author: rgarcia
ms.openlocfilehash: 310c0f547ee11a3243589c364755a30a84be1a25
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810199"
---
## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

* [Azure uzamsal Tutturucuların genel bakış](../articles/spatial-anchors/overview.md)bölümünü okuyun.
* [5 dakikalık hızlı](../articles/spatial-anchors/index.yml)başlangıçlardan biri tamamlandı.
* C# ve Unity hakkında temel bilgi.
* Android 'i kullanmak istiyorsanız, <a href="https://developers.google.com/ar/discover/" target="_blank">Arcore</a> hakkında temel bilgi veya iOS kullanmak istiyorsanız <a href="https://developer.apple.com/arkit/" target="_blank">ARKit</a> .
* **ASP.net ve Web geliştirme** iş yüküyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017</a> veya sonraki bir sürümünün yüklü olduğu bir Windows bilgisayarı.
* [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download).
* Bir uygulamanın dağıtılacağı ve çalıştırılacağı bir veya daha fazla cihaz (iOS veya Android).
  * Android kullanıyorsanız, şunları yapmanız gerekir:
    * <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3,3</a> veya üzeri, <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019,4 (LTS)</a>ve Windows bilgisayarınızda yüklü olan <a href="https://git-scm.com/download/win" target="_blank">Windows için git</a> .
    * <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">Geliştirici özellikli</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">arçekirdekli</a> bir Android cihaz.
  * İOS kullanıyorsanız, şunları yapmanız gerekir:
    * <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10</a> veya üzeri, <a href="https://cocoapods.org" target="_blank">CocoaPods</a>ve <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019,4 (LTS)</a> yüklü olan MacOS bilgisayarı.
    * Geliştirici özellikli <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit ile uyumlu</a> bir iOS cihazı.
    * Git, homebrew aracılığıyla yüklendi. Terminalde tek bir satırda aşağıdaki komutu girin: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` sonra komutunu çalıştırın `brew install git` .
