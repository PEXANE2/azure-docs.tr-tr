---
title: Desteklenen işletim sistemleri, konteyner motorları - Azure IoT Edge
description: Hangi işletim sistemlerinin Azure IoT Edge daemon ve çalışma süresini çalıştırabileceğini ve üretim aygıtlarınız için desteklenen konteyner motorlarını öğrenin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/06/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 42c0a5d0c590f8c395c2afe366a00fcb9c83ce46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536947"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IoT Edge destekli sistemler

Bu makalede, resmi olarak veya önizlemede hangi sistemlerin ve bileşenlerin IoT Edge tarafından desteklendiği ne kadar ayrıntılı bilgi verilmektedir.

Azure IoT Edge hizmetini kullanırken sorun yaşıyorsanız, destek almanın birkaç yolu vardır. Destek için aşağıdaki kanallardan birini deneyin:

**Hataların bildirin** - Azure IoT Edge ürününe giren geliştirmenin çoğu IoT Edge açık kaynak projesinde gerçekleşir. Hatalar projenin sorunlar [sayfasında](https://github.com/azure/iotedge/issues) bildirilebilir. Düzeltmeler hızla projeden ürün güncelleştirmelerine doğru yol alar.

**Microsoft Müşteri Destek ekibi** – [Destek planı](https://azure.microsoft.com/support/plans/) olan kullanıcılar, doğrudan [Azure portalından](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac)bir destek bileti oluşturarak Microsoft Müşteri Destek ekibini meşgul edebilir.

**Özellik istekleri** – Azure IoT Edge ürün parçaları özelliği isteklerini ürünün [Kullanıcı Sesi sayfası](https://feedback.azure.com/forums/907045-azure-iot-edge)üzerinden izler.

## <a name="container-engines"></a>Konteyner motorları

Azure IoT Edge modülleri kapsayıcı olarak uygulandığından, IoT Edge'in bunları başlatmak için bir konteyner motoruna ihtiyacı vardır. Microsoft, bu gereksinimi karşılamak için bir kapsayıcı motoru, moby-engine sağlar. Bu konteyner motoru Moby açık kaynak projesine dayanıyor. Docker CE ve Docker EE diğer popüler konteyner motorları vardır. Ayrıca Moby açık kaynak projesini temel alıyor ve Azure IoT Edge ile uyumludur. Microsoft, bu kapsayıcı motorları kullanan sistemler için en iyi çaba desteği sağlar; ancak, Microsoft bu sorunlar için düzeltmeler gönderemez. Bu nedenle, Microsoft üretim sistemlerinde moby-engine kullanılmasını önerir.

<br>
<center>

![Konteyner çalışma süresi olarak Moby motoru](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>İşletim sistemleri

Azure IoT Edge kapsayıcıları çalıştırabilen çoğu işletim sisteminde çalışır; ancak, bu sistemlerin tümü eşit olarak desteklenmez. İşletim sistemleri, kullanıcıların bekleyebileceği destek düzeyini temsil eden katmanlar halinde gruplandırılır.

* Tier 1 sistemleri desteklenir. Tier 1 sistemleri için Microsoft:
  * otomatik testlerde bu işletim sistemi vardır
  * onlar için kurulum paketleri sağlar
* Tier 2 sistemleri Azure IoT Edge ile uyumludur ve nispeten kolay kullanılabilir. Tier 2 sistemleri için:
  * Microsoft platformlarda gayrı resmi testler yaptı veya platformda Azure IoT Edge'i başarıyla çalıştıran bir iş ortağı nı tanıyor
  * Diğer platformlar için kurulum paketleri bu platformlarda çalışabilir

Ev sahibi işletim sistemi ailesi her zaman bir modülün konteyner içinde kullanılan konuk işletim sistemi ailesi maç gerekir. Başka bir deyişle, Linux kapsayıcılarını yalnızca Windows'da Linux ve Windows kapsayıcılarında kullanabilirsiniz. Windows kullanılırken, Hyper-V yalıtılmış kapsayıcılar değil, yalnızca yalıtılmış kapsayıcıları işleme yedirilir.  

<br>
<center>

![Host OS maçlarını konuk İşletim Sistemi](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Katman 1

Aşağıdaki tabloda listelenen sistemler, genel olarak kullanılabilir veya genel önizlemede Microsoft tarafından desteklenir ve her yeni sürümle sınanmaktadır. 

| İşletim Sistemi | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Raspbian Stretch |  | ![Raspbian Streç + ARM32v7](./media/tutorial-c-module/green-check.png) |  |
| [Ubuntu Server 16.04](https://wiki.ubuntu.com/XenialXerus/ReleaseNotes) | ![Ubuntu Server 16.04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Genel önizleme  |
| [Ubuntu Server 18.04](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes) | ![Ubuntu Server 18.04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Genel önizleme |
| [Windows 10 IoT Core](https://docs.microsoft.com/windows/iot-core/windows-iot-core), yapı 17763 | ![Windows IoT Çekirdek + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| [Windows 10 IoT Enterprise](https://docs.microsoft.com/windows/iot-core/windows-iot-enterprise), yapı 17763 | ![Windows 10 IoT Kurumsal + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| [Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/rel-notes-19), yapı 17763 | ![Windows Server 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| [Windows Server IoT 2019](https://docs.microsoft.com/windows/iot-core/windows-server), yapı 17763 | ![Windows Server IoT 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |

Yukarıda listelenen Windows işletim sistemleri, Windows'da Windows kapsayıcıları çalıştıran aygıtlar için gereksinimlerdir ve bu da üretim için desteklenen tek yapılandırmadır. Windows için Azure IoT Edge yükleme paketleri, Windows'da Linux kapsayıcılarının kullanımına izin verir; ancak, bu yapılandırma yalnızca geliştirme ve sınama içindir. Daha fazla bilgi için, [Linux kapsayıcılarını çalıştırmak için Windows'da IoT Edge kullanın'a](how-to-install-iot-edge-windows-with-linux.md)bakın.

### <a name="tier-2"></a>Katman 2

Aşağıdaki tabloda listelenen sistemler Azure IoT Edge ile uyumlu kabul edilir, ancak Microsoft tarafından etkin olarak sınanmamaktadır veya korunmaz.

| İşletim Sistemi | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| [CentOS 7.5](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7.1804) | ![CentOS + AMD64](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM64](./media/tutorial-c-module/green-check.png) |
| [Debian 8](https://www.debian.org/releases/jessie/) | ![Debian 8 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Debian 9](https://www.debian.org/releases/stretch/) | ![Debian 9 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Debian 10](https://www.debian.org/releases/buster/) <sup>1</sup> | ![Debian 10 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Mentor Gömülü Linux Flex OS](https://www.mentor.com/embedded-software/linux/mel-flex-os/) | ![Mentor Gömülü Linux Flex OS + AMD64](./media/tutorial-c-module/green-check.png) | ![Mentor Gömülü Linux Flex OS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Mentor Gömülü Linux Flex OS + ARM64](./media/tutorial-c-module/green-check.png) |
| [Mentor Gömülü Linux Omni OS](https://www.mentor.com/embedded-software/linux/mel-omni-os/) | ![Mentor Gömülü Linux Omni OS + AMD64](./media/tutorial-c-module/green-check.png) |  | ![Mentor Gömülü Linux Omni OS + ARM64](./media/tutorial-c-module/green-check.png) |
| [RHEL 7.5](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/7.5_release_notes/index) | ![RHEL 7.5 + AMD64](./media/tutorial-c-module/green-check.png) | ![RHEL 7.5 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![RHEL 7.5 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Ubuntu 16.04](https://wiki.ubuntu.com/XenialXerus/ReleaseNotes) | ![Ubuntu 16.04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16.04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16.04 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Ubuntu 18.04](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes) | ![Ubuntu 18.04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18.04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18.04 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Wind River 8](https://docs.windriver.com/category/os-wind_river_linux) | ![Rüzgar Nehri 8 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| [Yocto](https://www.yoctoproject.org/) | ![Yocto + AMD64](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM64](./media/tutorial-c-module/green-check.png) |
| Raspbian Buster <sup>1</sup> |  | ![Raspbian Buster + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Raspbian Buster + ARM64](./media/tutorial-c-module/green-check.png) |

<sup>Raspian</sup> Buster dahil olmak üzere 1 Debian 10 sistemleri, OpenSSL'in IoT Edge'in desteklemediği bir sürümünü kullanır. IoT Edge'i yüklemeden önce önceki bir sürümü yüklemek için aşağıdaki komutu kullanın:

```bash
sudo apt-get install libssl1.0.2
```

## <a name="releases"></a>Yayınlar

IoT Edge sürüm varlıkları ve sürüm notları [azure-iotedge sürümleri](https://github.com/Azure/azure-iotedge/releases) sayfasında mevcuttur. Bu bölüm, her sürümün bileşenlerini daha kolay görselleştirmenize yardımcı olmak için bu sürüm notlarından alınan bilgileri yansıtır.

IoT Edge bileşenleri tek tek yüklenebilir veya güncellenebilir ve eski sürümlerden bileşenlerle geriye doğru uyumludur. Aşağıdaki tablo, her sürümde yer alan bileşenleri listeler:

| Yayınla   | Güvenlik daemon  | Kenar hub'ı<br>Kenar aracısı | Libiothsm | Moby  |
| --------- | ---------------- | ---------------------- | --------- | ----- |
| **1.0.9** | 1.0.9            | 1.0.9                  | 1.0.9     |       |
| **1.0.8** | 1.0.8            | 1.0.8.5<br>1.0.8.4<br>1.0.8.3<br>1.0.8.2<br>1.0.8.1<br>1.0.8 | 1.0.8 | 3.0.6 |
| **1.0.7** | 1.0.7.1<br>1.0.7 | 1.0.7.1<br>1.0.7       | 1.0.7.1<br>1.0.7 | 3.0.5<br>3.0.4 (ARMv7hl, CentOS) |
| **1.0.6** | 1.0.6.1<br>1.0.6 | 1.0.6.1<br>1.0.6       | 1.0.6.1<br>1.0.6 |  |
| **1.0.5** | 1.0.5            | 1.0.5                  | 1.0.5     | 3.0.2 |

IoT Edge, Microsoft.Azure.Device.Client SDK'yı kullanır. Daha fazla bilgi için ,NET başvuru içeriği için [Azure IoT C# SDK GitHub repo'ya](https://github.com/Azure/azure-iot-sdk-csharp) veya [Azure SDK'ya](https://docs.microsoft.com/dotnet/api/overview/azure/iot/client?view=azure-dotnet)bakın. Aşağıdaki liste, istemci SDK'nın her sürümün karşısına test edildiği sürümünü gösterir:

* **IoT Edge 1.0.9**: İstemci SDK 1.21.1
* **IoT Edge 1.0.8**: İstemci SDK 1.20.3
* **IoT Edge 1.0.7**: İstemci SDK 1.20.1
* **IoT Edge 1.0.6**: İstemci SDK 1.17.1
* **IoT Edge 1.0.5**: İstemci SDK 1.17.1

## <a name="virtual-machines"></a>Virtual Machines

Azure IoT Edge sanal makinelerde çalıştırılabilir. Müşteriler mevcut altyapıyı kenar zekasıyla genişletmek istediklerinde sanal bir makineyi IoT Edge aygıtı olarak kullanmak yaygındır. Ev sahibi VM OS'nin ailesi, bir modülün konteynerinde kullanılan konuk işletim sistemi ailesiyle eşleşmelidir. Bu gereksinim, Azure IoT Edge'in doğrudan bir aygıtta çalıştırıldığı yla aynıdır. Azure IoT Edge, temel sanallaştırma teknolojisinin agnostiközelliğidir ve Hyper-V ve vSphere gibi platformlarla desteklenen VM'lerde çalışır.

<br>
<center>

![VM'de Azure IoT Edge](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Minimum sistem gereksinimleri

Azure IoT Edge, Raspberry Pi3 gibi küçük aygıtlarda sunucu sınıfı donanıma kadar harika çalışır. Senaryonuz için doğru donanımı seçmek, çalıştırmak istediğiniz iş yüklerine bağlıdır. Son cihaz kararını verme karmaşık olabilir; ancak, kolayca geleneksel dizüstü bilgisayarlarda veya masaüstü bilgisayarlarda bir çözüm prototip başlayabilirsiniz.

Prototipleme sırasında deneyim son cihaz seçimi rehberlik yardımcı olacaktır. Göz önünde bulundurmanız gereken sorular şunlardır:

* İş yükünde kaç modül var?
* Modüllerinizin konteynerleri kaç katmanı paylaşıyor?
* Modülleriniz hangi dilde yazılıyor?
* Modülleriniz ne kadar veri işleyecek?
* Modüllerinizin iş yüklerini hızlandırmak için özel donanıma ihtiyacı var mı?
* Çözümünüzün istenen performans özellikleri nelerdir?
* Donanım bütçeniz nedir?
