---
title: Android Studio ile mobil uygulama geliştirme yi öğretmek için bir laboratuvar kur
titleSuffix: Azure Lab Services
description: Android Studio kullanan veri mobil uygulama geliştirme sınıföğretmek için bir laboratuvar kurmak öğrenin.  Makalede, Azure'daki sanal bir makinede Android Studio kullanırken yapılacak ayarlamalar da ele alınacaktır.
services: lab-services
author: emaher
ms.service: lab-services
ms.topic: article
ms.date: 1/23/2020
ms.author: enewman
ms.openlocfilehash: 0c257589a2e93ac4c15a639e7156d0c0944b033c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76849792"
---
# <a name="set-up-a-lab-to-teach-data-mobile-application-development-with-android-studio"></a>Android Studio ile veri mobil uygulama geliştirme öğretmek için bir laboratuvar kurmak

Bu makalede, nasıl bir giriş mobil uygulama geliştirme sınıfı kurmak gösterecektir.  Bu [sınıf, Google Play Store'da](https://play.google.com/store/apps)yayınlanabilen Android mobil uygulamalarına odaklanır.  Öğrenciler uygulama oluşturmak için [Android Studio'yu](https://developer.android.com/studio) nasıl kullanacaklarını öğrenirler.  [Android için Visual Studio Emülatörü](https://visualstudio.microsoft.com/vs/msft-android-emulator/) yerel uygulama test etmek için kullanılır.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ve laboratuvar hesabına ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliği ni aldıktan sonra Azure Lab Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni bir laboratuvar hesabı oluşturma hakkında daha fazla bilgi için, [bir laboratuvar hesabı kurmak için öğreticiye](tutorial-setup-lab-account.md)bakın.  Varolan bir laboratuvar hesabını da kullanabilirsiniz.

Yeni bir laboratuvar oluşturmak için [sınıf laboratuarı öğreticisini ayarlayın](tutorial-setup-classroom-lab.md) ve ardından aşağıdaki ayarları uygulayın:

| Sanal makine boyutu | Görüntü |
| -------------------- | ----- |
| Orta (İç içe Sanallaştırma) | Windows Server 2019 Datacenter |

## <a name="template-machine-configuration"></a>Şablon makine yapılandırması

Şablon makinesi oluşturma tamamlandığında, [makineyi çalıştırın ve](how-to-create-manage-template.md#update-a-template-vm) aşağıdaki görevleri tamamlamak için makineye bağlanın:

1. Hyper-V rolü ekle
2. Java'yı indirin ve kurun.  
3. Android için Visual Studio Emülatörü indirin ve kurun.
4. Android Studio'u indirin ve kurun.
5. Android Studio için Visual Studio Emülatörü yapılandırın.

## <a name="add-hyper-v-role"></a>Hyper-V rolü ekle

Android için Visual Studio Emulator'un başarılı bir şekilde kurulumu için Hyper-V etkinleştirilmelidir.  Bir şablon [sanal makine makalesinde iç içe sanallaştırmayı etkinleştirme yönergelerini](how-to-enable-nested-virtualization-template-vm.md) izleyin.

## <a name="install-java"></a>Java'yı Yükle

Android Studio Java gerektirir.  Java'nın en son sürümünü indirmek için aşağıdaki adımları izleyin.

1. [Java indirme sayfasına](https://www.java.com/download/)gidin. Java **İndir** düğmesini tıklatın.
2. Java için 64 bit Windows web sayfasında, **Kabul Et ve Ücretsiz İndir**etiketini tıklatın.
3. **Java Kurulum** yükleyicisi **göründüğünde, Yükle'yi**tıklatın.
4. Yükleyici başlığı Java Kurulumu 'nda değişene kadar bekleyin **– Tamamlayın.**  **Kapat** düğmesini tıklatın.

## <a name="install-visual-studio-emulator-for-android"></a>Android için Visual Studio Emülatör yükleyin

Bir Android uygulamasını yerel olarak test etmek için, bir Android aygıtının sanallaştırılmış sürümünü kullanması gerekir.  Bir geliştirici kendi makineden kendi uygulama test sağlayacak birkaç Android emülatörleri mevcuttur.  Android için Visual Studio Emülatör kullanıyoruz çünkü iç içe sanallaştırmayı destekleyen bir emülatör.  Laboratuvar Hizmeti VM zaten sanal bir makine olduğundan, iç içe sanallaştırmayı destekleyen bir emülatöre ihtiyacımız var.  Android Studio için yerleşik emülatör iç içe sanallaştırma desteklemez.  İç içe sanallaştırmayı hangi emülatörlerin desteklediğini görmek [için, emülatör performansı için donanım ivmesi (Hyper-V & HAXM)](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration)bakın.

Android için Visual Studio Emulator'u indirmek ve yüklemek için aşağıdaki yönergeleri kullanın.

1. Android giriş sayfası [için Visual Studio Emülatör'e](https://visualstudio.microsoft.com/vs/msft-android-emulator/) gidin.
2. **Emülatör İndir düğmesini** tıklatın.
3. vs_emulatorsetup.exe indirildiğinde, çalıştırılabilir çalıştırın.
4. Visual Studio kurulum iletişim kutusu **görüntülendiğinde, Yükle** düğmesini tıklatın.
5. Yükleyicinin tamamlanmasını bekleyin.  Bilgisayarı yeniden başlatmak ve yüklemeyi bitirmek için **Şimdi Yeniden Başlat** düğmesini tıklatın.

Android Studio'yu kullanarak uygulamanızı dağıtmadan önce emülatöre başlayın.  Android için Visual Studio Emülatörü hakkında daha fazla bilgi için, [Android dokümantasyon için Visual Studio Emülatörü'ne](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-emulator-for-android)bakın.

## <a name="install-android-studio"></a>Android Studio'u Yükleyin

[Android Studio'u](https://developer.android.com/studio)indirmek ve yüklemek için aşağıdaki talimatları izleyin.

1. Android [Studio indirme sayfasına](https://developer.android.com/studio#downloads)gidin.  
    > [!NOTE]
    > Internet Explorer bu site tarafından desteklenmez.
2. Windows (64 bit) çalıştırılabilir Android Studio paketini tıklatın.
3. Açılır pencerede yazılan yasal terimleri okuyun.  Devam etmeye hazır olduğunda, **yukarıdaki hüküm ve koşulları okudum ve katılıyorum** kontrol edin ve Windows için Android Studio **İndir** düğmesine tıklayın.
4. Android Studio kurulumu na çalıştırılabilir indirildikten sonra çalıştırılabilir çalıştırın.
5. **Android Studio Kurulum** yükleyicisinin Android Studio Kurulum sayfasına Hoş Geldiniz **sayfasında, İleri'yi**tıklatın. **Welcome to Android Studio Setup**
6. Yapılandırma **Ayarları** sayfasında **İleri'yi**tıklatın.
7. Başlat **Menüsü Klasörünü Seç** sayfasında **Yükle'yi**tıklatın.
8. Kurulumun tamamlanmasını bekleyin.
9. Yükleme **Tamamlandı** sayfasında **İleri'yi**tıklatın.
10. Android **Studio Kurulumunu Tamamlama** sayfasında.  **Son**'a tıklayın.
11. Kurulum bittikten sonra Android Studio otomatik olarak başlayacaktır.
12. Android **Ayarlarını İçe Aktar...** iletişim **kutusunda, ayarları alma'yı**seçin. **Tamam**'a tıklayın.
13. Android Studio Kurulum **Sihirbazı'nın** **HoşGeldiniz** **sayfasında, İleri'yi**tıklatın.
14. Türü **Yükle** sayfasında **Standart'ı**seçin. **İleri**'ye tıklayın.
15. **UI Tema'yı seç** sayfasında istediğiniz teonu seçin. **İleri**'ye tıklayın.
16. Ayarları **Doğrula** sayfasında **İleri'yi**tıklatın.
17. **İndirme Bileşenleri** sayfasında, tüm bileşenler indirilene kadar bekleyin.  **Son**'a tıklayın.

    > [!IMPORTANT]
    > HAXM yüklemesinin başarısız olması beklenmektedir.  HAXM iç içe sanallaştırma desteklemez, bu yüzden bu makalede daha önce Android için Visual Studio Emulator yüklü olmasıdır.

18. Kurulum sihirbazı tamamlandığında **Android Studio'ya Hoş Geldiniz** iletişim kutusu görüntülenir.

## <a name="configure-android-studio-and-visual-studio-emulator-for-android"></a>Android için Android Studio ve Visual Studio Emülatörü yapılandırın

Android Studio kullanıma neredeyse hazırdır.  Android SDK'nın nerede kurulduğunu android için Visual Studio Emulator'a söylememiz gerekiyor.  Bu Android Studio hata ayıklama için dağıtım hedefleri olarak Android göstermek için Visual Studio çalışan herhangi bir emülatör yapacak.

Android Sdk'nın nerede olduğunu Android için Visual Studio Emulator'a söylemek için belirli bir kayıt defteri anahtarı ayarlamamız gerekiyor.  Gerekli kayıt defteri anahtarını ayarlamak için aşağıdaki komut dosyasını çalıştırın.  Aşağıdaki PowerShell komut dosyası, Android Sdk için varsayılan yükleme konumunu varsayar.  Android Sdk'nızı başka bir konuma yüklediyseniz, komut dosyasını çalıştırmadan `$androidSdkPath` önce değeri değiştirin.

```powershell
$androidSdkPath = Resolve-Path $(Join-Path "$($env:APPDATA)" "../Local/Android/Sdk")

$registryKeyPath = "HKLM:Software\WOW6432NODE\Android Sdk Tools"
New-Item -Path $registryKeyPath
New-ItemProperty -Path $registryKeyPath -Name Path -PropertyType String -Value $androidSdkPath
```

> [!IMPORTANT]
> Yeni ayar kullanılan böylece Android ve Android Studio için Visual Studio Emülatörü yeniden başlatın.

İhtiyacınız olan sürümü Visual Studio Emülatörü'nde başlatın.  Android stüdyonuzda Android uygulamanız için bir dağıtım hedefi olarak görünür.  Android Studio projesi için minimum sürüm, Android için Visual Studio Emülatörü'nde çalışan sürümü desteklemelidir.  Şimdi Android Studio ve Android için Visual Studio Emülatörü kullanarak projeler oluşturmak ve hata ayıklamak için hazırsınız.  Herhangi bir sorununz varsa, Android emülatör sorun giderme konusuna bakın.

## <a name="cost"></a>Maliyet

Bu laboratuvarın maliyetini tahmin etmek isterseniz, aşağıdaki örneği takip edebilirsiniz.
20 saatlik planlanan ders süresi ve ödev veya ödevler için 10 saatlik kontenjanı olan 25 öğrenciden oluşan bir sınıf için, laboratuvar fiyatı  

25 \* öğrenci (20 planlanmış + 10 kontenjan) saat * 55 Laboratuvar Birimi * 0,01 USD/saat = 412,5 USD

Fiyatlandırma hakkında daha fazla ayrıntı için Azure [Lab Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/lab-services/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar herhangi bir laboratuvar kurmak için ortak.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
