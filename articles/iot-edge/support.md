---
title: Desteklenen işletim sistemleri, kapsayıcı altyapıları-Azure IoT Edge
description: Hangi işletim sistemlerinin Azure IoT Edge Daemon ve çalışma zamanını çalıştırabileceğinizi ve üretim cihazlarınız için desteklenen kapsayıcı altyapılarını öğrenin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/09/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b17f1f32a3e49e9161afe92d62b85a162affcd9f
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98630539"
---
# <a name="azure-iot-edge-supported-systems"></a>Desteklenen sistemleri Azure IoT Edge

Bu makalede, resmi olarak veya önizlemeye bakılmaksızın IoT Edge tarafından desteklenen sistemler ve bileşenler hakkında ayrıntılı bilgi sağlanır.

Azure IoT Edge hizmetini kullanırken sorunlarla karşılaşırsanız, destek almak için çeşitli yollar vardır. Destek için aşağıdaki kanallardan birini deneyin:

**Raporlama hataları** – Azure IoT Edge ürüne giden geliştirmenin büyük bölümü IoT Edge açık kaynaklı projede gerçekleşir. Hatalar, projenin [sorunlar sayfasında](https://github.com/azure/iotedge/issues) bildirilebilir. Düzeltmeler, ' deki projeden ürün güncelleştirmelerinde hızlı bir şekilde yapılır.

**Microsoft müşteri destek ekibi** – [Destek planına](https://azure.microsoft.com/support/plans/) sahip olan kullanıcılar, doğrudan [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac)bir destek bileti oluşturarak Microsoft müşteri destek ekibine geçebilir.

**Özellik istekleri** – Azure IoT Edge ürünü, ürünün [Kullanıcı sesi sayfası](https://feedback.azure.com/forums/907045-azure-iot-edge)aracılığıyla özellik isteklerini izler.

## <a name="container-engines"></a>Kapsayıcı altyapıları

Azure IoT Edge modüller kapsayıcı olarak uygulanır, bu nedenle IoT Edge bir kapsayıcı altyapısının bunları başlatması gerekir. Microsoft, bu gereksinimi karşılamak için Moby-Engine kapsayıcı altyapısı sağlar. Bu kapsayıcı altyapısı, Moby açık kaynaklı projeyi temel alır. Docker CE ve Docker EE, diğer popüler kapsayıcı altyapılardır. Ayrıca, Moby açık kaynaklı projeye dayalıdır ve Azure IoT Edge uyumludur. Microsoft bu kapsayıcı altyapılarını kullanan sistemler için en iyi çaba desteğini sağlar; Bununla birlikte, Microsoft bu sorunları gidermeye yönelik düzeltmeler de teslim edemiyor. Bu nedenle, Microsoft, üretim sistemlerinde Moby-Engine kullanılmasını önerir.

<br>
<center>

![Kapsayıcı çalışma zamanı olarak Moby altyapısı](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>İşletim sistemleri

Azure IoT Edge, kapsayıcıları çalıştıratabilen çoğu işletim sisteminde çalışır; Ancak, bu sistemlerin hepsi eşit olarak desteklenmez. İşletim sistemleri, kullanıcıların tahmin edebilecekleri destek düzeyini temsil eden katmanlar halinde gruplandırılır.

* Katman 1 sistemleri desteklenir. Katman 1 sistemleri için, Microsoft:
  * Bu işletim sistemine otomatik testlerde sahiptir
  * Bunlar için yükleme paketleri sağlar
* Katman 2 sistemleri Azure IoT Edge uyumludur ve görece kolay bir şekilde kullanılabilir. Katman 2 sistemleri için:
  * Microsoft, platformlar üzerinde resmi olmayan test yaptık veya platformda başarıyla çalışan bir iş ortağı biliyor Azure IoT Edge
  * Diğer platformlar için yükleme paketleri, bu platformlarda çalışabilir

Ana bilgisayar işletim sistemi ailesi her zaman bir modülün kapsayıcısı içinde kullanılan Konuk işletim sisteminin ailesiyle aynı olmalıdır. Diğer bir deyişle, yalnızca Windows 'da Linux ve Windows kapsayıcılarında Linux kapsayıcıları kullanabilirsiniz. Windows kullanırken, Hyper-V yalıtılmış kapsayıcılar değil yalnızca işlem yalıtılmış kapsayıcılar desteklenir.  

Windows üzerinde Linux için IoT Edge, Windows ana bilgisayarında çalışan bir Linux sanal makinesinde IoT Edge kullanır. Bu şekilde, Linux modüllerini bir Windows cihazında çalıştırabilirsiniz.

### <a name="tier-1"></a>Katman 1

Aşağıdaki tablolarda listelenen sistemler Microsoft tarafından desteklenir, genel olarak kullanılabilir ya da genel önizleme aşamasındadır ve her yeni sürümle test edilmiştir.

Azure IoT Edge, Linux veya Windows kapsayıcıları olarak oluşturulan modülleri destekler. Linux kapsayıcıları, Linux cihazlarına dağıtılabilir veya Windows üzerinde Linux için IoT Edge kullanılarak Windows cihazlarına dağıtılabilir. Windows kapsayıcıları yalnızca Windows cihazlarına dağıtılabilir.

#### <a name="linux-containers"></a>Linux kapsayıcıları

Linux kapsayıcıları olarak oluşturulan modüller, Linux veya Windows cihazlarına dağıtılabilir. Linux cihazlarında, IoT Edge çalışma zamanı doğrudan konak cihaza yüklenir. Windows cihazlarında, IoT Edge çalışma zamanına göre önceden oluşturulmuş bir Linux sanal makinesi, konak cihazında çalışır.

Windows üzerinde Linux için IoT Edge Şu anda genel önizlemededir, ancak Windows cihazlarda IoT Edge çalıştırmanın önerilen yoludur.

| Operating System | 'TÜR | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Raspberry PI OS Esnetme |  | ![Raspberry PI OS Esneti + ARM32v7](./media/tutorial-c-module/green-check.png) |  |
| Ubuntu Server 16.04 | ![Ubuntu Server 16,04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Genel Önizleme  |
| Ubuntu Server 18.04 | ![Ubuntu Server 18,04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Genel Önizleme |
| Windows 10 Pro | Genel Önizleme |  |  |
| Windows 10 Enterprise | Genel Önizleme |  |  |
| Windows 10 IoT Enterprise | Genel Önizleme |  |  |
| Windows Server 2019 | Genel Önizleme |  |  |

Tüm Windows işletim sistemleri sürüm 1809 (derleme 17763) veya üzeri olmalıdır.

#### <a name="windows-containers"></a>Windows kapsayıcıları

Windows kapsayıcıları olarak oluşturulan modüller yalnızca Windows cihazlarına dağıtılabilir.

| Operating System | 'TÜR | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Windows 10 IoT Enterprise | ![check1](./media/tutorial-c-module/green-check.png) |  |  |
| Windows 10 IoT Core<sup>1</sup><br> | ![check1](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server 2019  | ![check1](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server IoT 2019<br> | ![check1](./media/tutorial-c-module/green-check.png) |  |  |

<sup>1</sup> Windows 10 IoT Core, sürüm 1.0.10 sonrasında desteklenmez

Tüm Windows işletim sistemleri sürüm 1809 (derleme 17763) olmalıdır. Windows kapsayıcıları sürümü, ana bilgisayar Windows cihazının sürümü ile tam olarak eşleşmesi gerektiğinden Windows için belirli bir Windows derlemesi IoT Edge gerekir. Windows kapsayıcıları Şu anda yalnızca 17763 derlemesini kullanır.

### <a name="tier-2"></a>Katman 2

Aşağıdaki tabloda listelenen sistemler Azure IoT Edge uyumlu olarak değerlendirilir, ancak Microsoft tarafından etkin bir şekilde sınanmamıştır veya korunmaz.

| Operating System | 'TÜR | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| [CentOS 7.5](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7.1804) | ![CentOS + AMD64](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM64](./media/tutorial-c-module/green-check.png) |
| [Debian 8](https://www.debian.org/releases/jessie/) | ![Desek8 + AMD64](./media/tutorial-c-module/green-check.png) | ![Desek8 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Desek8 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Debian 9](https://www.debian.org/releases/stretch/) | ![Dete 9 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debir 9 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debir 9 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Debian 10](https://www.debian.org/releases/buster/) | ![De, 10 + AMD64](./media/tutorial-c-module/green-check.png) | ![De, 10 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![De, 10 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Mentor Embedded Linux Flex OS](https://www.mentor.com/embedded-software/linux/mel-flex-os/) | ![Mentor Embedded Linux Flex OS + AMD64](./media/tutorial-c-module/green-check.png) | ![Mentor Embedded Linux Flex OS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Mentor Embedded Linux Flex OS + ARM64](./media/tutorial-c-module/green-check.png) |
| [Mentor Embedded Linux OMNI OS](https://www.mentor.com/embedded-software/linux/mel-omni-os/) | ![Mentor Embedded Linux OMNI OS + AMD64](./media/tutorial-c-module/green-check.png) |  | ![Mentor Embedded Linux OMNI OS + ARM64](./media/tutorial-c-module/green-check.png) |
| [RHEL 7.5](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/7.5_release_notes/index) | ![RHEL 7,5 + AMD64](./media/tutorial-c-module/green-check.png) | ![RHEL 7,5 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![RHEL 7,5 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Ubuntu 16.04](https://wiki.ubuntu.com/XenialXerus/ReleaseNotes) | ![Ubuntu 16,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16,04 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Ubuntu 18.04](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes) | ![Ubuntu 18,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18,04 + ARM64](./media/tutorial-c-module/green-check.png) |
| [Wind River 8](https://docs.windriver.com/category/os-wind_river_linux) | ![Rüzgar River 8 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| [Yocto](https://www.yoctoproject.org/) | ![Yocto + AMD64](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM64](./media/tutorial-c-module/green-check.png) |
| Raspberry PI OS Buster |  | ![Raspberry PI OS Buster + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Raspberry PI OS Buster + ARM64](./media/tutorial-c-module/green-check.png) |
| [Ubuntu 20,04 <sup>1</sup>](https://wiki.ubuntu.com/FocalFossa/ReleaseNotes) | ![Ubuntu 20,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 20,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 20,04 + ARM64](./media/tutorial-c-module/green-check.png) |

<sup>1</sup> [Azure IoT Edge sürümlerinin](https://github.com/Azure/azure-iotedge/releases) depolarından çıkan 9 paket, Ubuntu 20,04 ile kutudan çıkmalıdır.

## <a name="releases"></a>Yayınlar

IoT Edge sürüm varlıkları ve sürüm notları, [Azure-ıotedge yayınları](https://github.com/Azure/azure-iotedge/releases) sayfasında bulunabilir. Bu bölüm, her bir sürümün bileşenlerini daha kolay görselleştirmenize yardımcı olmak için bu sürüm notlarındaki bilgileri yansıtır.

IoT Edge bileşenleri tek tek yüklenebilir ve güncelleştirilmiş olabilir ve eski sürümlerden bileşenlerle geriye doğru uyumludur. Aşağıdaki tabloda her sürümde yer alan bileşenler listelenmiştir:

| Yayınla | Güvenlik cini | Edge hub 'ı<br>Edge Aracısı | Libiothsm | Moby |
|--|--|--|--|--|
| **1.0.10** | 1.0.10 | 1.0.10 | 1.0.10 |  |
| **1.0.9** | 1.0.9.5<br>1.0.9.4<br>1.0.9.3<br>1.0.9.2<br>1.0.9.1<br>1.0.9 | 1.0.9.5<br>1.0.9.4<br>1.0.9.3<br>1.0.9.2<br>1.0.9.1<br>1.0.9 | 1.0.9.5<br>1.0.9.4<br>1.0.9.3<br>1.0.9.2<br>1.0.9.1<br>1.0.9 |  |
| **1.0.8** | 1.0.8 | 1.0.8.5<br>1.0.8.4<br>1.0.8.3<br>1.0.8.2<br>1.0.8.1<br>1.0.8 | 1.0.8 | 3.0.6 |
| **1.0.7** | 1.0.7.1<br>1.0.7 | 1.0.7.1<br>1.0.7 | 1.0.7.1<br>1.0.7 | 3.0.5<br>3.0.4 (ARMv7hl, CentOS) |
| **1.0.6** | 1.0.6.1<br>1.0.6 | 1.0.6.1<br>1.0.6 | 1.0.6.1<br>1.0.6 |  |
| **1.0.5** | 1.0.5 | 1.0.5 | 1.0.5 | 3.0.2 |

IoT Edge, Microsoft. Azure. Devices. Client SDK 'sını kullanır. Daha fazla bilgi için bkz. [Azure ıOT C# SDK GitHub deposu](https://github.com/Azure/azure-iot-sdk-csharp) veya [.NET için Azure SDK başvuru içeriği](/dotnet/api/overview/azure/iot/client). Aşağıdaki listede, her sürümün test ettiği istemci SDK sürümü gösterilmektedir:

* **IoT Edge 1.0.10**: istemci SDK 'sı 1.28.0
* **IoT Edge 1.0.9**: istemci SDK 'sı 1.21.1
* **IoT Edge 1.0.8**: istemci SDK 'sı 1.20.3
* **IoT Edge 1.0.7**: istemci SDK 'sı 1.20.1
* **IoT Edge 1.0.6**: istemci SDK 'sı 1.17.1
* **IoT Edge 1.0.5**: istemci SDK 'sı 1.17.1

## <a name="virtual-machines"></a>Virtual Machines

Azure IoT Edge, sanal makinelerde çalıştırılabilir. Bir sanal makinenin IoT Edge cihaz olarak kullanılması, müşteriler var olan altyapıyı Edge zekası ile genişletmek istediğinizde yaygındır. Konak VM işletim sistemi ailesi, bir modülün kapsayıcısı içinde kullanılan Konuk işletim sisteminin ailesiyle eşleşmelidir. Bu gereksinim, Azure IoT Edge doğrudan bir cihazda çalıştırıldığı zaman ile aynıdır. Azure IoT Edge, temel alınan sanallaştırma teknolojisinin belirsiz olması ve Hyper-V ve vSphere gibi platformlar tarafından desteklenen VM 'lerde çalışır.

<br>
<center>

![Bir VM 'de Azure IoT Edge](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>En düşük sistem gereksinimleri

Azure IoT Edge, sunucu sınıfı donanımlarına Raspberry pi3 kadar küçük cihazlarda harika bir şekilde çalışır. Senaryonuz için doğru donanımın seçilmesi, çalıştırmak istediğiniz iş yüklerine bağlıdır. Son cihaz kararının yapılması karmaşık olabilir; Ancak, bir çözümü geleneksel dizüstü bilgisayarlarda veya masaüstleri üzerinde kolayca başlatabilirsiniz.

Prototipleme, son cihazınızın seçimine kılavuzluk etmenize yardımcı olacak deneyimdir. Göz önünde bulundurmanız gereken sorular şunlardır:

* İş yükünüze kaç modül var?
* Modüllerinizin kapsayıcıları paylaştığı katman sayısı nedir?
* Modülleriniz hangi dilde yazılmıştır?
* Modüllerinizin ne kadar veri işlemesi gerekir?
* Modüllerinizin iş yüklerini hızlandırarak özel donanımlar mi gerekiyor?
* Çözümünüzün istenen performans özellikleri nelerdir?
* Donanım bütçeniz nedir?
