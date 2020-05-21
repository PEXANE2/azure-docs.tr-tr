---
title: Unity ile model işleme
description: Bir modeli işleme adımlarında kullanıcıya kılavuzluk eden hızlı başlangıç
author: florianborn71
ms.author: flborn
ms.date: 01/23/2020
ms.topic: quickstart
ms.openlocfilehash: be5bc8ec4e8f363d9b6079d22f3bba1af2d4d5fb
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682561"
---
# <a name="quickstart-render-a-model-with-unity"></a>Hızlı başlangıç: Unity ile model Işleme

Bu hızlı başlangıçta, yerleşik bir modeli Azure uzaktan Işleme (ARR) hizmetini kullanarak uzaktan işleyen bir Unity örneğinin nasıl çalıştırılacağı ele alınmaktadır.

ARR API 'SI ile ilgili ayrıntılarla karşılaşmayacağız veya yeni bir Unity projesi nasıl ayarlanır? Bu konular [öğreticide ele alınmıştır: bir Unity projesini sıfırdan ayarlama](../tutorials/unity/project-setup.md).

Bu hızlı başlangıçta şunları nasıl yapacağınızı öğreneceksiniz:
> [!div class="checklist"]
>
>* Yerel geliştirme ortamınızı ayarlama
>* Unity için ARR hızlı başlangıç örnek uygulamasını edinme ve derleme
>* ARR hızlı başlangıç örnek uygulamasında bir model işleme

## <a name="prerequisites"></a>Ön koşullar

Azure uzaktan Işleme hizmetine erişim sağlamak için önce [bir hesap oluşturmanız](../how-tos/create-an-account.md)gerekir.

Aşağıdaki yazılım yüklü olmalıdır:

* Windows SDK 10.0.18362.0 [(İndir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Visual Studio 2019 ' un en son sürümü [(indirme)](https://visualstudio.microsoft.com/vs/older-downloads/)
* GIT [(İndir)](https://git-scm.com/downloads)
* Unity 2019.3.1 [(indirme)](https://unity3d.com/get-unity/download)
  * Bu modülleri Unity 'ye yükler:
    * **UWP** -Evrensel Windows platformu derleme desteği
    * **IL2CPP** -Windows derleme DESTEĞI (IL2CPP)

## <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Bir komut istemi açın ( `cmd` Windows Başlat menüsünde yazın) ve ARR örnek projesini depolamak istediğiniz bir dizine geçin.

Aşağıdaki komutları çalıştırın:

```cmd
mkdir ARR
cd ARR
git clone https://github.com/Azure/azure-remote-rendering
```

Son komut, Azure uzaktan Işleme için çeşitli örnek projelerini içeren ARR dizininde bir alt dizin oluşturur.

Unity için hızlı başlangıç örnek uygulaması *Unity/hızlı başlangıç*alt dizininde bulunur.

## <a name="rendering-a-model-with-the-unity-sample-project"></a>Unity örnek projesiyle model işleme

Unity hub 'ını açın ve *ARR\azure-Remote-rendering\Unity\Quickstart* klasörü olan örnek projeyi ekleyin.
Projeyi açın. Gerekirse, Unity 'nin projeyi yüklü sürüme yükseltmesine izin verin.

İşlediğimiz varsayılan model [yerleşik bir örnek modeldir](../samples/sample-model.md). Bir [sonraki hızlı](convert-model.md)başlangıçta ARR dönüştürme hizmetini kullanarak özel bir modeli nasıl dönüştürecağız.

### <a name="enter-your-account-info"></a>Hesap bilgilerinizi girin

1. Unity varlık tarayıcısında, *sahneler* klasörüne gidin ve **hızlı başlangıç** sahneyi açın.
1. *Hiyerarşisinden* **RemoteRendering** Game nesnesini seçin.
1. *Denetçisinde* [Hesap kimlik bilgilerinizi](../how-tos/create-an-account.md)girin. Henüz bir hesabınız yoksa, [bir hesabınız oluşturun](../how-tos/create-an-account.md).

![ARR hesap bilgileri](./media/arr-sample-account-info.png)

> [!IMPORTANT]
> Azure portalı, hesabınızın etki alanını yalnızca *mixedreality.Azure.com*olarak görüntüler. Bu, başarıyla bağlanmak için yeterli değil.
> **Accountdomain** öğesini olarak ayarlayın `<region>.mixedreality.azure.com` ; burada, `<region>` [size yakın olan kullanılabilir bölgelerden biridir](../reference/regions.md).

Daha sonra bu projeyi bir HoloLens 'e dağıtmak ve bu cihazdan uzaktan Işleme hizmetine bağlanmak istiyoruz. Cihazdaki kimlik bilgilerini girmek için kolay bir yol olmadığı için hızlı başlangıç örneği, **kimlik bilgilerini Unity sahnede kaydeder**.

> [!WARNING]
> Kayıtlı kimlik bilgilerinizle projeyi, gizli oturum açma bilgilerini sızan bir depoya denetlediğinizden emin olun!

### <a name="create-a-session-and-view-the-default-model"></a>Oturum oluşturma ve varsayılan modeli görüntüleme

Oturumu başlatmak için Unity 'nin **oynat** düğmesine basın. *Oyun* panelindeki görünüm penceresinin alt kısmında durum metni içeren bir kaplama görmeniz gerekir. Oturum, bir dizi durum geçişine devam edecektir. **Başlangıç** durumunda uzak VM, birkaç dakika sürer. Başarılı olduğunda, **hazırlık** durumuna geçer. Artık oturum, bu VM üzerinde işleme çalışma zamanına ulaşmaya çalıştığı **bağlantı** durumuna girer. Başarılı olduğunda, örnek **bağlı** duruma geçiş yapar. Bu noktada, işleme için model indirilbaşlayacaktır. Modelin boyutu nedeniyle indirme birkaç dakika daha sürebilir. Daha sonra uzaktan işlenen model görüntülenir.

![Örnekten çıkış](media/arr-sample-output.png)

Tebrikler! Artık uzaktan işlenmiş bir modeli görüntülüyorsunuz!

## <a name="inspecting-the-scene"></a>Sahneyi İnceleme

Uzaktan işleme bağlantısı çalışmaya başladıktan sonra Denetçi paneli ek durum bilgileriyle güncelleştirilir:

![Unity örnek oynama](./media/arr-sample-configure-session-running.png)

Artık yeni düğümü seçip Inspector 'da **alt öğeleri göster** ' i tıklatarak sahne grafiğini inceleyebilirsiniz.

![Unity hiyerarşisi](./media/unity-hierarchy.png)

Sahnede [kesilmiş bir düzlem](../overview/features/cut-planes.md) nesnesi var. Özelliklerinde etkinleştirmeyi ve taşınmasını deneyin:

![Kesilen düzlemi değiştirme](media/arr-sample-unity-cutplane.png)

Dönüşümleri eşitlemek için **Şimdi Eşitle** ' ye tıklayın ya da **her kareyi Eşitle** seçeneğini işaretleyin. Bileşen özellikleri için, yalnızca bunları değiştirmeniz yeterlidir.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki hızlı başlangıçta, özgün boyutunda uzaktan işlenmiş modeli görüntülemek için örneği bir HoloLens 'e dağıtacağız.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Unity örneğini HoloLens 'e dağıtma](deploy-to-hololens.md)

Alternatif olarak, örnek bir masaüstü BILGISAYARA da dağıtılabilir.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Unity örneğini masaüstüne dağıtma](deploy-to-desktop.md)