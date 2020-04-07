---
title: Unity ile bir model oluşturma
description: Bir modeli işlemek için kullanıcıya yol gösteren hızlı başlatma
author: florianborn71
ms.author: flborn
ms.date: 01/23/2020
ms.topic: quickstart
ms.openlocfilehash: b0af45ba4a6b1ca7f9e751af082ff0db80776ec0
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679789"
---
# <a name="quickstart-render-a-model-with-unity"></a>Quickstart: Unity ile bir model oluşturma

Bu hızlı başlatma, Azure Uzaktan İşleme (ARR) hizmetini kullanarak yerleşik bir modeli uzaktan işleyen bir Birlik örneğinin nasıl çalıştırılabildiğini kapsar.

ARR API'nin kendisi veya yeni bir Birlik projesinin nasıl kurulması hakkında ayrıntılı bilgi vermeyececeğiz. Bu konular [Tutorial: Sıfırdan bir Birlik projesi kurma](../tutorials/unity/project-setup.md)kapsamındadır.

Bu hızlı başlangıçta nasıl öğreneceksiniz:
> [!div class="checklist"]
>
>* Yerel geliştirme ortamınızı ayarlama
>* Unity için ARR Quickstart örnek uygulamasını alın ve oluşturun
>* ARR Quickstart örnek uygulamasında bir model oluşturma

## <a name="prerequisites"></a>Ön koşullar

Azure Uzaktan İşlem hizmetine erişmek için öncelikle [bir hesap oluşturmanız](../how-tos/create-an-account.md)gerekir.

Aşağıdaki yazılım yüklenmelidir:

* Windows SDK 10.0.18362.0 [(indir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Visual Studio 2019'un en son sürümü [(indir)](https://visualstudio.microsoft.com/vs/older-downloads/)
* GIT [(indir)](https://git-scm.com/downloads)
* Birlik 2019.3.1 [(indir)](https://unity3d.com/get-unity/download)
  * Bu modülleri Birlik olarak yükleyin:
    * **UWP** - Universal Windows Platform Build Desteği
    * **IL2CPP** - Windows Yapı Desteği (IL2CPP)

## <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Komut istemini açın `cmd` (Windows başlat menüsünde yazın) ve ARR örnek projesini depolamak istediğiniz bir dizine değiştirin.

Aşağıdaki komutları çalıştırın:

```cmd
mkdir ARR
cd ARR
git clone https://github.com/Azure/azure-remote-rendering
```

Son komut, ARR dizininde Azure Uzaktan İşleme için çeşitli örnek projeleri içeren bir alt dizini oluşturur.

Unity için hızlı başlatma örnek uygulaması alt dizin *Birlik /Quickstart*bulunur.

## <a name="rendering-a-model-with-the-unity-sample-project"></a>Unity örnek projesi ile bir model oluşturma

Unity Hub'ı açın ve *ARR\azure-uzaktan işleme\Unity\Quickstart* klasörü olan örnek projeyi ekleyin.
Projeyi açın. Gerekirse, Unity'nin projeyi yüklü sürümünüze yükseltmesine izin verin.

İşlediğimiz varsayılan model yerleşik bir [örnek modeldir.](../samples/sample-model.md) Bir [sonraki quickstart'ta](convert-model.md)ARR dönüşüm hizmetini kullanarak özel bir modelin nasıl dönüştürüleceğini göstereceğiz.

### <a name="enter-your-account-info"></a>Hesap bilgilerinizi girin

1. Unity varlık *tarayıcısında, Sahneler* klasörüne gidin ve **Quickstart** sahnesini açın.
1. *Hiyerarşi'den* **RemoteRendering** oyun nesnesini seçin.
1. *Müfettiş*olarak, [hesap kimlik bilgilerinizi](../how-tos/create-an-account.md)girin.

![ARR Hesap Bilgileri](./media/arr-sample-account-info.png)

> [!IMPORTANT]
> Azure Portalı hesabınızın etki alanını yalnızca *mixedreality.azure.com*olarak görüntüler. Bu, başarılı bir şekilde bağlanmak için yeterli değildir.
> **AccountDomain'i** `<region>.mixedreality.azure.com`, `<region>` [yakınınıza gelen kullanılabilir bölgelerden biri](../reference/regions.md)olan yere ayarlayın.

Daha sonra bu projeyi bir HoloLens'e dağıtmak ve bu cihazdan Uzaktan İşlem hizmetine bağlanmak istiyoruz. Aygıttaki kimlik bilgilerini girmenin kolay bir yolu olmadığından, hızlı başlatma örneği **Birlik sahnesindekimlik bilgilerini kaydeder.**

> [!WARNING]
> Kaydedilmiş kimlik bilgilerinizle projeyi gizli giriş bilgilerini sızdıracağı bir depoya kontrol etmemeyi unutmayın!

### <a name="create-a-session-and-view-the-default-model"></a>Oturum oluşturma ve varsayılan modeli görüntüleme

Oturumu başlatmak için Unity'nin **Oynat** düğmesine basın. *Oyun* panelinde görünüm portunun alt kısmında durum metni içeren bir bindirme görmeniz gerekir. Oturumda bir dizi devlet geçişi yapılacak. **Başlangıç** durumunda, uzak VM döndürüldü, bu da birkaç dakika sürer. Başarı üzerine, **Hazır** durumuna geçişler. Şimdi oturum, bu VM'de işleme çalışma süresine ulaşmaya çalıştığı **Bağlama** durumuna girer. Başarılı olduğunda, örnek **Bağlı** duruma geçiş eder. Bu noktada, işleme modeli indirmeye başlar. Modelin boyutu nedeniyle, karşıdan yükleme birkaç dakika daha sürebilir. Sonra uzaktan işlenen model görüntülenir.

![Örnekten çıktı](media/arr-sample-output.png)

Tebrikler! Şimdi uzaktan işlenmiş bir model görüntüleme!

## <a name="inspecting-the-scene"></a>Olay yerini inceleme

Uzaktan işleme bağlantısı çalıştırıladıktan sonra, Denetçi paneli ek durum bilgileriyle güncellenir:

![Unity örnek oynama](./media/arr-sample-configure-session-running.png)

Artık yeni düğümü seçerek ve Denetçi'deki çocukları **göster'i** tıklatarak sahne grafiğini keşfedebilirsiniz.

![Birlik Hiyerarşisi](./media/unity-hierarchy.png)

Olay yerinde kesik bir [düzlem](../overview/features/cut-planes.md) nesnesi var. Özelliklerinde etkinleştirmeyi ve hareket ettirin:

![Kesme düzlemini değiştirme](media/arr-sample-unity-cutplane.png)

Dönüşümleri eşitlemek için **şimdi Eşitle'yi** tıklatın veya **her kareyi Eşitle** seçeneğini denetleyin. Bileşen özellikleri için bunları değiştirme yeterlidir.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki hızlı başlangıçta, uzaktan işlenen modeli orijinal boyutunda görüntülemek için örneği bir HoloLens'e dağıtacağız.

> [!div class="nextstepaction"]
> [Quickstart: HoloLens'e Birlik örneğini dağıtın](deploy-to-hololens.md)

Alternatif olarak, örnek bir masaüstü bilgisayara da dağıtılabilir.

> [!div class="nextstepaction"]
> [Quickstart: Unity örneğini Masaüstüne dağıtın](deploy-to-desktop.md)