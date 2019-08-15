---
title: Desteklenen işletim sistemleri, kapsayıcı altyapıları - Azure IOT Edge | Microsoft Docs
description: Azure IOT Edge arka plan programı ve çalışma zamanı ve üretim cihazlarınız için desteklenen kapsayıcı altyapıları hangi işletim sistemini çalıştırabileceğiniz öğrenin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: fdecc34b49b853c34624cabf4688e0387c303a16
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985809"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IOT Edge desteklenen sistemleri

Bu makalede, resmi olarak veya önizlemeye bakılmaksızın IoT Edge tarafından desteklenen sistemler ve bileşenler hakkında ayrıntılı bilgi sağlanır. 

Azure IoT Edge hizmetini kullanırken sorunlarla karşılaşırsanız, destek almak için çeşitli yollar vardır. Destek için aşağıdaki kanallardan birini deneyin:

**Hata Raporlama** – Azure IOT Edge ürün şeklinde ulaştığı geliştirme çoğunu IOT Edge açık kaynaklı proje gerçekleşir. Hatalar rapor üzerinde [sorunlar sayfasında](https://github.com/azure/iotedge/issues) proje. Düzeltmeleri projesi ile aşamalarından geçerek ürün güncelleştirmelerini hızlı bir şekilde yapın.

**Microsoft müşteri destek ekibinin** – sahip kullanıcılar bir [destek planı](https://azure.microsoft.com/support/plans/) Microsoft müşteri destek ekibinin doğrudan bir destek bileti oluşturarak görüşebilirsiniz [Azure portalında](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Özellik istekleri** – Azure IOT Edge ürün ürünün özellik isteklerini izleyen [User Voice sayfa](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Kapsayıcı altyapıları

Azure IoT Edge modüller kapsayıcı olarak uygulanır, bu nedenle IoT Edge bir kapsayıcı altyapısının bunları başlatması gerekir. Microsoft, bu gereksinimi karşılamak için moby-altyapısı, bir kapsayıcı altyapısı sağlar. Bu kapsayıcı altyapısı, Moby açık kaynaklı projeyi temel alır. Docker CE ve Docker EE diğer popüler kapsayıcı motorlardır. Ayrıca, Moby açık kaynaklı projeye dayalıdır ve Azure IoT Edge uyumludur. Microsoft bu kapsayıcı altyapılarını kullanan sistemler için en iyi çaba desteğini sağlar; Bununla birlikte, Microsoft bu sorunları gidermeye yönelik düzeltmeler de teslim edemiyor. Bu nedenle, üretim sistemlerine moby altyapısı kullanarak Microsoft önerir.

<br>
<center>

![Kapsayıcı çalışma zamanına göre Moby](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>İşletim sistemleri
Azure IoT Edge, kapsayıcıları çalıştıratabilen çoğu işletim sisteminde çalışır; Ancak, bu sistemlerin hepsi eşit olarak desteklenmez. İşletim sistemleri, kullanıcıları bekleyebileceğiniz destek düzeyini temsil eden katmanlarda gruplandırılır.
* Katman 1 sistemleri desteklenir. Katman 1 sistemleri için, Microsoft:
    * Bu işletim sistemine otomatik testlerde sahiptir
    * bunların yükleme paketleri sağlar
* Katman 2 sistemleri Azure IoT Edge uyumludur ve görece kolay bir şekilde kullanılabilir. Katman 2 sistemleri için:
    * Microsoft, platformlar üzerinde geçici test yaptık veya platformda Azure IoT Edge bir iş ortağının başarıyla çalıştığını biliyor
    * Diğer platformlar için yükleme paketleri bu platformlarda çalışabilir
    
Ana bilgisayar işletim sistemi ailesi her zaman bir modülün kapsayıcısı içinde kullanılan Konuk işletim sisteminin ailesiyle aynı olmalıdır. Diğer bir deyişle, yalnızca Windows 'da Linux ve Windows kapsayıcılarında Linux kapsayıcıları kullanabilirsiniz. Windows kullanırken, Hyper-V yalıtılmış kapsayıcılar değil yalnızca işlem yalıtılmış kapsayıcılar desteklenir.  

<br>
<center>

![Konak IŞLETIM sistemi Konuk IŞLETIM sistemiyle eşleşiyor](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Katman 1

Aşağıdaki tabloda listelenen sistemler Microsoft tarafından desteklenir, genel olarak kullanılabilir ya da genel önizleme aşamasındadır ve her yeni sürümle test edilmiştir. 

| İşletim sistemi | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Raspbian Uzat |  | ![Raspbian uzat + ARM32v7](./media/tutorial-c-module/green-check.png) |  |
| Ubuntu Server 16.04 | ![Ubuntu Server 16,04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Genel önizleme  |
| Ubuntu Server 18.04 | ![Ubuntu Server 18,04 + AMD64](./media/tutorial-c-module/green-check.png) |  | Genel önizleme |
| Windows 10 IoT Enterprise, derleme 17763 | ![Windows 10 IoT Enterprise + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server 2019, derleme 17763 | ![Windows Server 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server IoT 2019, derleme 17763 | ![Windows Server IoT 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows 10 IoT Core, derleme 17763 | ![Windows IoT Core + AMD64](./media/tutorial-c-module/green-check.png) |  |  |


Yukarıda listelenen Windows işletim sistemleri, Windows üzerinde Windows kapsayıcıları çalıştıran cihazlara yönelik gereksinimlerdir. Bu yapılandırma, üretim için desteklenen tek yapılandırmadır. Windows için Azure IoT Edge yükleme paketleri Windows üzerinde Linux kapsayıcılarının kullanılmasına izin verir; Ancak, bu yapılandırma yalnızca geliştirme ve test içindir. Windows üzerinde Linux kapsayıcıları kullanımı, üretim için desteklenen bir yapılandırma değildir. Bu geliştirme senaryosunda herhangi bir Windows 10 Build 14393 veya daha yeni sürümü ve Windows Server 2016 ya da daha yeni sürümü kullanılabilir.

### <a name="tier-2"></a>Katman 2

Aşağıdaki tabloda listelenen sistemler Azure IoT Edge uyumlu olarak değerlendirilir, ancak etkin bir şekilde sınanmamıştır veya korunmaz. 

| İşletim sistemi | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| CentOS 7.5 | ![CentOS + AMD64](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 8 | ![Desek8 + AMD64](./media/tutorial-c-module/green-check.png) | ![Desek8 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Desek8 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 9 | ![Dete 9 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debir 9 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debir 9 + ARM64](./media/tutorial-c-module/green-check.png) |
| De, 10<sup>1</sup> | ![De, 10 + AMD64](./media/tutorial-c-module/green-check.png) | ![De, 10 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![De, 10 + ARM64](./media/tutorial-c-module/green-check.png) |
| RHEL 7.5 | ![RHEL 7,5 + AMD64](./media/tutorial-c-module/green-check.png) | ![RHEL 7,5 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![RHEL 7,5 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 16.04 | ![Ubuntu 16,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16,04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 18.04 | ![Ubuntu 18,04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18,04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18,04 + ARM64](./media/tutorial-c-module/green-check.png) |
| 8 Rüzgar Irmağı | ![Rüzgar River 8 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Yocto | ![Yocto + AMD64](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM64](./media/tutorial-c-module/green-check.png) |
| Raspbian Buster<sup>1</sup> |  | ![Raspbian Buster + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Raspbian Buster + ARM64](./media/tutorial-c-module/green-check.png) |

Raspian Buster dahil olmak üzere <sup>1</sup> dekon, IoT Edge desteklemediği bir OpenSSL sürümü kullanır. IoT Edge yüklemeden önce önceki bir sürümü yüklemek için aşağıdaki komutu kullanın: 

```bash
sudo apt-get install libssl1.0.2
```

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
