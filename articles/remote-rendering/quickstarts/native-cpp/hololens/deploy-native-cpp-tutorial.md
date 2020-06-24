---
title: Yerel C++ öğreticisini HoloLens 'e dağıtma
description: HoloLens 'te yerel C++ öğreticisinin nasıl çalıştırılacağını gösteren hızlı başlangıç
author: florianborn71
ms.author: flborn
ms.date: 06/08/2020
ms.topic: quickstart
ms.openlocfilehash: 6b3909281cf475a003ffaaef6f6f48441337728e
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84810258"
---
# <a name="quickstart-deploy-native-c-sample-to-hololens"></a>Hızlı başlangıç: HoloLens 'e yerel C++ örneği dağıtma

Bu hızlı başlangıçta, yerel C++ öğretici uygulamasının bir HoloLens 2 ' de nasıl dağıtılacağı ve çalıştırılacağı ele alınmaktadır.

Bu hızlı başlangıçta şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
>
>* HoloLens için öğretici uygulaması oluşturun.
>* Kaynak kodundaki ARR kimlik bilgilerini değiştirin.
>* Örneği cihaza dağıtın ve çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

Azure uzaktan Işleme hizmetine erişim sağlamak için önce [bir hesap oluşturmanız](../../../how-tos/create-an-account.md)gerekir.

Aşağıdaki yazılım yüklü olmalıdır:

* Windows SDK 10.0.18362.0 [(İndir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Visual Studio 2019 ' un en son sürümü [(indirme)](https://visualstudio.microsoft.com/vs/older-downloads/)
* [Karma gerçeklik Için Visual Studio Araçları](https://docs.microsoft.com/windows/mixed-reality/install-the-tools). Özellikle, aşağıdaki *Iş yükü* yüklemeleri zorunludur:
  * **C++ ile masaüstü geliştirme**
  * **Evrensel Windows Platformu (UWP) geliştirme**
* GIT [(İndir)](https://git-scm.com/downloads)

## <a name="clone-the-arr-samples-repository"></a>ARR örnekleri deposunu kopyalama

İlk adım olarak, genel Azure uzaktan Işleme örneklerinin barındırıldığı Git deposunu kopyaladık. Bir komut istemi açın ( `cmd` Windows Başlat menüsünde yazın) ve ARR örnek projesini depolamak istediğiniz bir dizine geçin.

Aşağıdaki komutları çalıştırın:

```cmd
mkdir ARR
cd ARR
git clone https://github.com/Azure/azure-remote-rendering
```

Son komut, Azure uzaktan Işleme için çeşitli örnek projelerini içeren ARR dizininde bir alt dizin oluşturur.

C++ HoloLens öğreticisi, *Nativecpp/HoloLens*alt dizininde bulunabilir.

## <a name="build-the-project"></a>Projeyi derleme

Visual Studio 2019 ile *Nativecpp/HoloLens* alt dizininde bulunan *holographicapp. sln* çözüm dosyasını açın.

Derleme yapılandırmasını *hata ayıklama* (veya *Release*) ve *ARM64*olarak değiştirin. Ayrıca, hata ayıklayıcı modunun *Uzak makinenin*aksine *cihaz* olarak ayarlandığından emin olun:

![Visual Studio yapılandırması](media/vs-config-native-cpp-tutorial.png)

Hesap kimlik bilgileri öğreticinin kaynak kodunda kodlandığı için, bunları geçerli kimlik bilgileriyle değiştirin. Bunun için, dosyayı `HolographicAppMain.cpp` Visual Studio içinde açın ve sınıfın Oluşturucusu içinde ön uç 'nin oluşturulduğu parçayı değiştirin `HolographicAppMain` :


```cpp
// 2. Create front end
{
    // Users need to fill out the following with their account data and model
    RR::AzureFrontendAccountInfo init;
    init.AccountId = "00000000-0000-0000-0000-000000000000";
    init.AccountKey = "<account key>";
    init.AccountDomain = "westus2.mixedreality.azure.com"; // <change to your region>
    m_modelURI = "builtin://Engine";
    m_sessionOverride = ""; // If there is a valid session ID to re-use, put it here. Otherwise a new one is created
    m_frontEnd = RR::ApiHandle(RR::AzureFrontend(init));
}
```

Özellikle, aşağıdaki değerleri değiştirin:
* `init.AccountId`ve `init.AccountKey` Hesap verilerinizi kullanmak için. [Hesap bilgilerini alma](../../../how-tos/create-an-account.md#retrieve-the-account-information)hakkında paragrafa bakın.
* Örneğin `init.AccountDomain` , diğer bölgelere ait dizenin bölge bölümü `westus2``"westeurope.mixedreality.azure.com"`
* Ayrıca, `m_sessionOverride` var olan bir oturum kimliği ile değiştirilebilir. Oturumlar, örneğin [PowerShell betiği](../../../samples/powershell-example-scripts.md#script-renderingsessionps1) kullanılarak veya [oturum REST API](../../../how-tos/session-rest-api.md#create-a-session) doğrudan kullanılarak bu örneğin dışında oluşturulabilir.
Örnek dışında bir oturum oluşturmak, örneğin örnek birden çok kez çalıştırılması önerilir. Hiçbir oturum geçirilmemişse, örnek her başlatma sırasında yeni bir oturum oluşturur ve bu işlem birkaç dakika sürebilir.

Artık uygulama derlenebilir.

## <a name="launch-the-application"></a>Uygulamayı başlatma

1. HoloLens 'i bir USB kablosu ile BILGISAYARıNıZA bağlayın.
1. HoloLens 'i açın ve Başlat menüsü görünene kadar bekleyin.
1. Visual Studio 'da hata ayıklayıcıyı başlatın (F5). Uygulamayı cihaza otomatik olarak dağıtır.

Örnek uygulama, geçerli uygulama durumu hakkında sizi bilgilendiren bir metin paneli görünmelidir. Başlangıç saatinde durum yeni bir oturum başlatıyor veya mevcut bir oturuma bağlanıyor. Model yüklemesi tamamlandıktan sonra, yerleşik motor modeli, baş konumundayken hemen görünür. Bir şekilde, altyapı modeli yerel olarak işlenen dönen küple düzgün şekilde etkileşim kurar.

 Daha sonra örneği ikinci kez başlatmak isterseniz, bunu HoloLens başlangıç menüsünden de bulabilirsiniz, ancak bu, kendisine derlenmiş bir oturum KIMLIĞI olduğunu fark edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç, tüm uzaktan Işleme ilgili parçaları bir stok *holographic uygulamasıyla*tümleştirmeyi açıklayan bir öğreticinin sonucunu temel alır. Hangi adımların gerekli olduğunu öğrenmek için bu öğreticiyi izleyin:

> [!div class="nextstepaction"]
> [Öğretici: uzaktan Işlemeyi bir HoloLens holographic uygulamasına tümleştirme](../../../tutorials/native-cpp/hololens/integrate-remote-rendering-into-holographic-app.md)
