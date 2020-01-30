---
title: Android Studio ile mobil uygulama geliştirmeyi öğretmek için laboratuvar kurma
titleSuffix: Azure Lab Services
description: Android Studio kullanan veri mobil uygulama geliştirme sınıfı öğretmek için laboratuvar ayarlamayı öğrenin.  Makale Ayrıca, Azure 'daki bir sanal makinede Android Studio kullanırken yapılacak ayarlamaları tartışır.
services: lab-services
author: emaher
ms.service: lab-services
ms.topic: article
ms.date: 1/23/2020
ms.author: enewman
ms.openlocfilehash: 0c257589a2e93ac4c15a639e7156d0c0944b033c
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76849792"
---
# <a name="set-up-a-lab-to-teach-data-mobile-application-development-with-android-studio"></a>Android Studio ile veri mobil uygulama geliştirmesi öğretmek için laboratuvar kurma

Bu makalede, tanıtım amaçlı bir mobil uygulama geliştirme sınıfı ayarlama gösterilmektedir.  Bu sınıf, [Google Play Store](https://play.google.com/store/apps)yayımlanmakta olabilecek Android mobil uygulamalarına odaklanır.  Öğrenciler, uygulama derlemek için [Android Studio](https://developer.android.com/studio) kullanmayı öğrenin.  [Android Için Visual Studio öykünücüsü](https://visualstudio.microsoft.com/vs/msft-android-emulator/) , uygulamayı yerel olarak test etmek için kullanılır.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu Laboratuvarı ayarlamak için, başlamak üzere bir Azure aboneliğine ve laboratuvar hesabına sahip olmanız gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni laboratuvar hesabı oluşturma hakkında daha fazla bilgi için bkz. [öğretici, laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md).  Ayrıca var olan bir laboratuvar hesabı da kullanabilirsiniz.

Yeni bir laboratuvar oluşturmak için [derslik Laboratuvarı ayarlama öğreticisini](tutorial-setup-classroom-lab.md) izleyin ve sonra aşağıdaki ayarları uygulayın:

| Sanal makine boyutu | Resim |
| -------------------- | ----- |
| Orta (Iç Içe sanallaştırma) | Windows Server 2019 Datacenter |

## <a name="template-machine-configuration"></a>Şablon makine yapılandırması

Şablon makinesi oluşturma işlemi tamamlandıktan sonra, aşağıdaki görevleri gerçekleştirmek için [makineyi başlatın ve bu sunucuya bağlanın](how-to-create-manage-template.md#update-a-template-vm) :

1. Hyper-V rolü Ekle
2. Java indirin ve yükleyin.  
3. Android için Visual Studio öykünücüsü ' nü indirip yükleyin.
4. Android Studio indirin ve yükleyin.
5. Android Studio için Visual Studio öykünücüsü yapılandırma.

## <a name="add-hyper-v-role"></a>Hyper-V rolü Ekle

Android için Visual Studio öykünücüsü yüklemesinin başarılı olması için Hyper-V etkinleştirilmelidir.  [Bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme](how-to-enable-nested-virtualization-template-vm.md) makalesindeki yönergeleri izleyin.

## <a name="install-java"></a>Java 'Yı yükler

Android Studio Java gerektirir.  Java 'nın en son sürümünü indirmek için aşağıdaki adımları izleyin.

1. [Java indirme sayfasına](https://www.java.com/download/)gidin. **Java indir** düğmesine tıklayın.
2. Java Web için 64-bit Windows sayfasında, kabul et etiketli **ve ücretsiz Indirmeyi Başlatan**düğmeye tıklayın.
3. **Java kurulum** yükleyicisi göründüğünde, **yükleme**' ye tıklayın.
4. Yükleyici başlığı Java kurulumuna değişene kadar bekleyin **– Tamam**.  **Kapat** düğmesine tıklayın.

## <a name="install-visual-studio-emulator-for-android"></a>Android için Visual Studio öykünücüsü 'nü yükler

Android uygulamasını yerel olarak test etmek için Android cihazının sanallaştırılmış bir sürümünü kullanması gerekir.  Bir geliştiricinin uygulamalarının makinesinden test yapmasına imkan tanıyan birkaç Android öykünücüsü mevcuttur.  İç içe sanallaştırmayı destekleyen bir öykünücü olduğundan, Android için Visual Studio öykünücüsü kullanıyoruz.  Laboratuvar Servisi VM zaten bir sanal makine olduğundan, iç içe sanallaştırmayı destekleyen bir öykünücü gerekir.  Android Studio için yerleşik öykünücü, iç içe sanallaştırmayı desteklemez.  Hangi öykünücülerin iç içe sanallaştırmayı desteklediğini görmek için bkz. [öykünücü performansı için donanım hızlandırma (Hyper-V & HAXM)](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration).

Android için Visual Studio öykünücüsü 'nü indirmek ve yüklemek için aşağıdaki yönergeleri kullanın.

1. [Android Için Visual Studio öykünücüsü](https://visualstudio.microsoft.com/vs/msft-android-emulator/) giriş sayfasına gidin.
2. **Öykünücüyü indir** düğmesine tıklayın.
3. Vs_emulatorsetup. exe dosyası indirildiyse, çalıştırılabiliri çalıştırın.
4. Visual Studio Kurulum iletişim kutusu göründüğünde, **yükleme** düğmesine tıklayın.
5. Yükleyicinin tamamlanmasını bekleyin.  Bilgisayarı yeniden başlatmak için **Şimdi yeniden Başlat** düğmesine tıklayın ve yüklemeyi sona erdirin.

Android Studio kullanarak uygulamanızı dağıtmaya başlamadan önce öykünücüyü başlatın.  Android için Visual Studio öykünücüsü hakkında daha fazla bilgi için bkz. [Android Için Visual Studio öykünücüsü belgeleri](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-emulator-for-android).

## <a name="install-android-studio"></a>Android Studio yüklensin

[Android Studio](https://developer.android.com/studio)indirmek ve yüklemek için aşağıdaki yönergeleri izleyin.

1. [Android Studio indirme sayfasına](https://developer.android.com/studio#downloads)gidin.  
    > [!NOTE]
    > Internet Explorer bu site tarafından desteklenmiyor.
2. Windows (64-bit) yürütülebilir Android Studio paketine tıklayın.
3. Açılan pencerede yazılı yasal koşulları okuyun.  Devam etmeye hazırsanız, **Yukarıdaki hüküm ve koşullar onay kutusunu okudum ve kabul** ediyorum onay kutusunu Işaretleyin ve **Windows için Android Studio indir** düğmesine tıklayın.
4. Android Studio Kurulum yürütülebilir dosyası indirildikten sonra yürütülebilir dosyayı çalıştırın.
5. **Android Studio Kurulum** yükleyicisinin **Android Studio Kurulum 'a hoş geldiniz** sayfasında **İleri**' ye tıklayın.
6. **Yapılandırma ayarları** sayfasında, **İleri**' ye tıklayın.
7. **Başlat Menüsü klasörünü Seç** sayfasında, **yüklensin**' e tıklayın.
8. Kurulumun tamamlanmasını bekleyin.
9. **Yükleme Tamam** sayfasında, **İleri**' ye tıklayın.
10. **Android Studio Kurulumu Tamamlanıyor** sayfasında.  **Finish (Son)** düğmesine tıklayın.
11. Android Studio, Kurulum bittikten sonra otomatik olarak başlatılır.
12. **Android ayarlarını Içeri aktar...** iletişim kutusunda **Ayarları içeri aktarma**' yı seçin. **Tamam**’a tıklayın.
13. **Android Studio Kurulum sihirbazının** **hoş geldiniz** sayfasında **İleri**' ye tıklayın.
14. **Tür yüklensin** sayfasında **Standart**' ı seçin. **İleri**’ye tıklayın.
15. **UI temasını Seç** sayfasında istenen Tema ' yı seçin. **İleri**’ye tıklayın.
16. **Ayarları doğrula** sayfasında, **İleri**' ye tıklayın.
17. **Bileşenleri karşıdan yükleme** sayfasında, tüm bileşenler indirilene kadar bekleyin.  **Finish (Son)** düğmesine tıklayın.

    > [!IMPORTANT]
    > HAXM yüklemesinin başarısız olması beklenmektedir.  HAXM, bu makalede daha önce Android için Visual Studio öykünücüsü 'nü yüklediğimiz iç içe sanallaştırmayı desteklemez.

18. **Android Studio hoş geldiniz** iletişim kutusu, Kurulum Sihirbazı tamamlandığında görüntülenir.

## <a name="configure-android-studio-and-visual-studio-emulator-for-android"></a>Android için Android Studio ve Visual Studio öykünücüsü yapılandırma

Android Studio neredeyse kullanıma hazırlanıyor.  Hala Android SDK yüklendiği Android için Visual Studio öykünücüsü 'ne söylememiz gerekiyor.  Bu, Android için Visual Studio 'da çalışan tüm öykünücüleri Android Studio hata ayıklama için dağıtım hedefleri olarak gösterir.

Android SDK 'Sının bulunduğu Android için Visual Studio öykünücüsü 'ne bildirmek üzere belirli bir kayıt defteri anahtarı ayarlamanız gerekir.  Gerekli kayıt defteri anahtarını ayarlamak için aşağıdaki betiği çalıştırın.  Aşağıdaki PowerShell betiği, Android SDK için varsayılan yüklemesi konumunu varsayar.  Android SDK 'nizi başka bir konuma yüklediyseniz, betiği çalıştırmadan önce `$androidSdkPath` değerini değiştirin.

```powershell
$androidSdkPath = Resolve-Path $(Join-Path "$($env:APPDATA)" "../Local/Android/Sdk")

$registryKeyPath = "HKLM:Software\WOW6432NODE\Android Sdk Tools"
New-Item -Path $registryKeyPath
New-ItemProperty -Path $registryKeyPath -Name Path -PropertyType String -Value $androidSdkPath
```

> [!IMPORTANT]
> Android için Visual Studio öykünücüsü 'nü yeniden başlatın ve Yeni ayarın kullanılması için Android Studio.

Visual Studio öykünücüsü 'nde ihtiyacınız olan sürümü başlatın.  Android Studio 'da Android uygulamanız için bir dağıtım hedefi olarak görünür.  Android Studio projesi için en düşük sürüm, Android için Visual Studio öykünücüsü ' nde çalışan sürümü desteklemelidir.  Artık Android Studio ve Android için Visual Studio öykünücüsü kullanarak projeleri oluşturmaya ve hata ayıklamaya hazırsınız.  Herhangi bir sorununuz varsa bkz. Android öykünücüsü sorun giderme.

## <a name="cost"></a>Maliyet

Bu laboratuvarın maliyetini tahmin etmek istiyorsanız aşağıdaki örneği izleyebilirsiniz.
20 saatlik zamanlanan sınıf süresi ve ev ödevi veya atamalar için 10 saatlik kota içeren 25 öğrenciye sahip bir sınıf için, laboratuvar fiyatı  

25 öğrenc\* (20 zamanlanan + 10 kota) saat * 55 laboratuvar birimi * 0,01 saat başına USD = 412,5 ABD Doları

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar, herhangi bir laboratuvarı ayarlamak için ortaktır.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekleme](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
