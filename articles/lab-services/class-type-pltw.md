---
title: Proje lideri, Azure Lab Services ile laboratuvarları ayarlama
description: Proje lideri sınıfları öğretmek için laboratuvarları ayarlamayı öğrenin.
ms.topic: article
ms.date: 10/28/2020
ms.openlocfilehash: 8585d09759319eef04da5ed68fec603cfa390093
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94497274"
---
# <a name="set-up-labs-for-project-lead-the-way-classes"></a>Proje için laboratuvarları ayarlama sınıfları

[Proje lideri (PLTW)](https://www.pltw.org/) , bilgisayar bilimi, mühendislik ve biyotıp bilimi içindeki Birleşik Devletler Için prek-12 ' yi sağlayan kar amacı gütmeyen bir kuruluştur.  Öğrenciler her bir PLTW sınıfında, uygulamalı öğrenme deneyiminin bir parçası olarak çeşitli yazılım uygulamaları kullanır.  Yazılım uygulamalarının birçoğu hızlı bir CPU veya bazı durumlarda bir GPU gerektirir.  Bu makalede, genellikle 9-12 ' de öğrencilerle sunulan aşağıdaki PLTW sınıfları için laboratuvarların nasıl ayarlanacağı gösterilmektedir:

- **Mühendislik tasarımına giriş**

    Öğrenciler, 3D modelleme için [Autodesk 'Nin Inventor bilgisayar destekli tasarım (CAD)](https://www.autodesk.com/products/inventor/new-features) yazılımını kullanmayı da kapsayan mühendislik tasarımı işlemine sunulmuştur.

- **Mühendislik ilkeleri**
    
    Öğrenciler mühendislik mekanizmaları, structural\material gücü ve otomasyon hakkında bilgi edinin.  Bu sınıf [md Kesintisds](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/MD+Solids/MD+Solids+Software+Installation+Guide.pdf), [Batı Point köprü tasarımcısı](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/West+Point+Bridge+Builder/Installation+Guide+for+West+Point+Bridge+Designer.pdf)ve [Amerika 'nın Army simülasyonu](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/America's+Army/Installation+Guide+for+Americas+Army+Simulation+17-18.pdf)gibi yazılımları kullanır.

- **Hukuki mühendislik ve mimari**

    Öğrenciler, 3D oluşturma bilgileri modelleme (BıM) için [Autodesk 'in](https://www.autodesk.com/products/revit/overview) daha önce kullanılan mimari tasarım yazılımını kullanarak bir bina ve site tasarımı ve geliştirme aşamasındadır.

- **Bilgisayar tümleşik üretimi**

    Öğrenciler, Robotics ve otomasyon içeren modern üretim süreçlerini keşfedebilir.   Bu sınıfta, öğrenciler [Autodesk 'Nin ıNVENTOR CAD](https://www.autodesk.com/products/inventor/new-features) ve [Autodesk 'nin Inventor bilgisayar DESTEKLI üretim (cam)](https://www.autodesk.com/products/inventor-cam/overview) yazılımını kullanır. 

- **Dijital elektronik aygıtlar**

    Öğrenciler, [Ulusal gerecin Multisım](https://www.ni.com/en-us/shop/electronic-test-instrumentation/application-software-for-electronic-test-and-instrumentation-category/what-is-multisim.html) simülasyonu ve devre tasarımı yazılımını kullanarak elektronik mantık devreleri ve cihazları incelidir.

- **Mühendislik tasarımı ve geliştirme**

    Öğrenciler, mühendislerin bir paneline sundukları araştırmayı, tasarımı ve test etmeyi birleştiren uçtan uca bir çözüme katkıda bulunur.  Bu sınıfta, öğrenciler [Autodesk 'Nin Inventor CAD](https://www.autodesk.com/products/inventor/new-features) yazılımını kullanır.

- **Bilgisayar bilimi temelleri**

    Öğrenciler, hesaplama kavramları ve araçlarına eklenmiştir.  Bunlar blok tabanlı programlama ile başlar ve ardından [Vexcode v5 blokları](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/VEXcode+V5+Blocks/VexCode+V5+Blocks+Installation+Guide.pdf)gibi kodlama ortamlarını kullanarak metin tabanlı kodlama kullanmak üzere hareket ederler.

- **Bilgisayar bilimi Ilkeleri**
    
    Öğrenciler, [Microsoft 'un Visual Studio Code geliştirme ortamını](https://code.visualstudio.com/)kullanarak kendi programlama uzmanlığını [Python](https://www.python.org/) ile büyüyürken. 

- **Bilgisayar bilimi A**

    Böylece öğrenciler, mobil uygulama geliştirmeyi öğrenerek bu sınıftaki programlama uzmanlıklarını büyümektedir.  Bu sınıfta, [Microsoft 'un Visual Studio Code geliştirme ortamını](https://code.visualstudio.com/)kullanarak [Java](https://www.java.com/) öğrenirler.  Öğrenciler ayrıca, kullanıcıların mobil uygulama kodlarını çalıştırmasına ve test etmesine izin veren bir öykünücü kullanır.  Azure Labs 'de öykünücü ayarlama hakkında daha fazla bilgi için, adresinden bizimle iletişim kurun azlabspilot@microsoft.com .

Her sınıf için [yazılımın tam listesi](https://www.pltw.org/pltw-software) için pltw 'ın sitesine bakın.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması
PLTW için Labs 'i kurmak üzere başlamak için bir Azure aboneliğiniz ve laboratuvar hesabınızın olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni bir laboratuar hesabı oluşturma hakkında daha fazla bilgi için [Laboratuvar hesabı ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account)hakkında öğreticiye bakın. Ayrıca var olan bir laboratuvar hesabı da kullanabilirsiniz.

Bir laboratuar hesabınız olduğunda, okulunuzun sunduğu bir PLTW sınıfının her oturumunda ayrı Labs oluşturmalısınız.  Ayrıca, her PLTW sınıfı türü için ayrı görüntüler oluşturmanızı öneririz.  Laboratuvarlarınızı ve görüntülerinizi nasıl yapılandıracağınıza ilişkin daha fazla ayrıntı için blog gönderisini okuyun: [fiziksel laboratuvardan Azure Lab Services taşıma](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931).

### <a name="lab-account-settings"></a>Laboratuvar hesabı ayarları
Laboratuvar hesabı için aşağıdaki tabloda açıklanan ayarları etkinleştirin. Market görüntülerinin nasıl etkinleştirileceği hakkında daha fazla bilgi için, [Laboratuvar oluşturucuları için kullanılabilen Market görüntülerinin nasıl belirtilme](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)hakkındaki makaleye bakın.

| Laboratuvar hesabı ayarı | Yönergeler |
| -------------------- | ----- |
| Market görüntüsü | Laboratuvar hesabınızda kullanılmak üzere Windows 10 Pro görüntüsünü etkinleştirin. |

### <a name="lab-settings"></a>Laboratuvar ayarları
PLTW sınıfları için kullanmayı önerdiğimiz sanal makinenin boyutu, öğrencilerinizin sınıfta yaptığı iş yüklerinin türlerine bağlıdır.  Yukarıdaki sınıflar için büyük ve küçük GPU (görselleştirme) VM boyutlarını kullanmanızı öneririz.  PLTW sınıflarınız için laboratuvarları ayarlarken aşağıdaki tablodaki kılavuza bakın.

| Laboratuvar ayarı | Değer/yönergeler |
| ------------ | ------------------ |
|Sanal makine boyutu| **Küçük GPU (görselleştirme)**.  Bu VM, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir.  Aşağıdaki PLTW sınıfları için bu boyutu kullanmanızı öneririz: hukuki mühendislik ve mimari, dijital elektronik, Bilgisayar tümleşik üretimi; ve mühendislik tasarımı ve geliştirme.
|Sanal makine boyutu| **Büyük**.  Bu boyut daha hızlı CPU, daha iyi yerel disk performansı, büyük veritabanları, büyük bellek önbellekler gerektiren uygulamalar için idealdir.  Aşağıdaki PLTW sınıfları için bu boyutu kullanmanızı öneririz: mühendislik tasarımına, mühendislik Ilkelerine, bilgisayar bilimi esalarına, bilgisayar bilimi Ilkelerine ve bilgisayar bilimi ilkelerine giriş.

### <a name="licensing-server"></a>Lisanslama sunucusu
Yukarıdaki PLTW sınıflarında kullanılan çoğu yazılım, bir lisanslama sunucusuna erişim gerektirir. *_not_*  Ancak, aşağıdaki yazılımlar için Autodesk 'nin ağ lisanslama modelini kullanmayı planlıyorsanız bir lisanslama sunucusuna erişmeniz gerekir:
-   Uyant
-   Inventor
-   Inventor CAM

Autodesk 'in yazılımıyla ağ lisansını kullanmak için, Pltw, Autodesk 'in lisans yöneticisini lisans sunucunuza yüklemek için [ayrıntılı adımlar sağlar](https://www.pltw.org/pltw-software) .  Bu lisans sunucusu, genellikle şirket içi ağınızda bulunur veya Azure sanal ağ (VNet) içindeki bir Azure sanal makinesinde (VM) barındırılır.

Lisans sunucunuz kurulduktan sonra, VNet 'i [Laboratuvar hesabınıza](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account) [eşetmeniz](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network) gerekir. Laboratuvar sanal makinelerinin lisans sunucusuna ve diğer bir yönteme erişebilmeleri için Laboratuvar oluşturma _before * ağ eşlemesi yapılmalıdır.

Autodesk tarafından oluşturulan lisans dosyaları lisanslama sunucusunun MAC adresini gömer.  Lisanslama sunucunuzu bir Azure VM kullanarak barındırmaya karar verirseniz, lisanslama sunucunuzun MAC adresinin değişmediğinden emin olmak önemlidir.   Aksi halde, MAC adresi değiştiğinde, lisanslama dosyalarınızın yeniden oluşturulması gerekir.  MAC adresinizin değiştirilmesini engellemek için şu ipuçlarını izleyin:

- Lisanslama sunucunuzu barındıran Azure VM için [statik bir özel IP ve MAC adresi ayarlayın](https://docs.microsoft.com/azure/lab-services/how-to-create-a-lab-with-shared-resource#static-private-ip-and-mac-address) .
- Bu kaynakları yeni bir region\location öğesine taşımaya gerek kalmaması için, hem laboratuvar hesabınızı hem de lisanslama sunucusunun sanal ağ sürümünü yeterli VM kapasitesi olan bir region\location içinde ayarladığınızdan emin olun.

Ayrıca, daha fazla bilgi için [bir lisanslama sunucusunu paylaşılan kaynak olarak ayarlama](https://docs.microsoft.com/azure/lab-services/how-to-create-a-lab-with-shared-resource) makalesini okuyun.

### <a name="template-machine"></a>Şablon makinesi
PLTW için ihtiyacınız olan yükleme dosyalarından bazıları büyük ve bunları bir laboratuvarın şablon makinesine indirdiğinizde kopyalanması uzun sürer.

Yükleme dosyalarını şablon makinesine indirmek ve her şeyi yüklemek yerine, fiziksel ortamınızda PLTW görüntülerini oluşturmanız önerilir.  Daha sonra, Laboratuvarlarınızı oluşturmak için bu özel görüntüleri kullanabilmeniz için resimleri paylaşılan görüntü galerisine aktarabilirsiniz.  Ayrıntılar için aşağıdaki makaleyi okuyun: [paylaşılan görüntü galerisine özel bir görüntü yükleyin](https://docs.microsoft.com/azure/lab-services/upload-custom-image-shared-image-gallery).

Bu öneriyi izleyerek laboratuvar ayarlamaya yönelik başlıca görevler şunlardır:

1. Fiziksel ortamınızda, sınıfının görüntüsünü oluşturun.

    a.  Yükleme dosyalarını indirmek ve gerekli yazılımı yüklemek için PLTW 'ın ayrıntılı adımlarını kullanın.

    > [!NOTE]    
    > Autodesk 'nin uygulamalarını yüklediğinizde, Autodesk 'yi yüklemekte olduğunuz bilgisayarın lisanslama sunucusuyla iletişim kurabilmesi gerekir (Autodesk 'nin Yükleme Sihirbazı, lisans sunucusunun barındırıldığı makinenin bilgisayar adını belirtmenizi ister).  Lisanslama sunucunuzu bir Azure VM 'de barındırıyorsanız, Autodesk 'nin Install sihirbazının lisanslama sunucunuza erişebilmesi için, laboratuvarın şablon makinesine Autodesk 'yi yüklemeyi beklemeniz gerekebilir

    b.  OneDrive 'ı (veya okulunuzun kullanabileceği diğer yedekleme seçeneklerini) [yükleyin ve yapılandırın](https://docs.microsoft.com/azure/lab-services/how-to-prepare-windows-template#install-and-configure-onedrive) .
    
    c.  [Windows güncelleştirmelerini yükleyip yapılandırın](https://docs.microsoft.com/azure/lab-services/how-to-prepare-windows-template#install-and-configure-updates).

1.  Özel görüntüyü [Laboratuvar hesabınıza ekli paylaşılan görüntü galerisine](https://docs.microsoft.com/azure/lab-services/how-to-attach-detach-shared-image-gallery)yükleyin.

1.  Bir laboratuvar oluşturun ve önceki adımda karşıya yüklediğiniz özel görüntüyü seçin.

1.  Laboratuvar oluşturulduktan sonra, görüntünün beklendiği gibi çalışıp çalışmadığını doğrulamak için başlatın ve şablon makinesine bağlanın.

1.  Son olarak, öğrenci sanal makinelerini oluşturmak için şablon makinesini yayımlayın.

## <a name="student-devices"></a>Öğrenci cihazları
Öğrencileriniz, Windows\Mac bilgisayarlardan ve Kmebook 'tan laboratuvar VM 'lerine bağlanabilir.  Bu seçeneklerin her biri için yönergelerin bağlantıları aşağıda verilmiştir:

- [Windows 'dan Bağlan](https://docs.microsoft.com/azure/lab-services/how-to-use-classroom-lab#connect-to-the-vm)
- [Mac 'ten Bağlan](https://docs.microsoft.com/azure/lab-services/connect-virtual-machine-mac-remote-desktop)
- [Kmebook 'tan Bağlan](https://docs.microsoft.com/azure/lab-services/connect-virtual-machine-chromebook-remote-desktop)

## <a name="cost"></a>Maliyet
Yukarıdaki PLTW sınıfları için olası bir maliyet tahminini kapsayalım.  Bu tahmin, bir lisans sunucusu çalıştırmanın veya paylaşılan görüntü galerisinin kullanımı maliyetini içermez.  25 öğrencilerden oluşan bir sınıf kullanacağız.  20 saatlik zamanlanan sınıf zamanı vardır.  Ayrıca, her öğrenci, zamanlanan sınıf süresi dışında ev ödevleri veya atamaları için 10 saatlik kota alır.  Hem **büyük** hem de **küçük GPU (görselleştirme)** boyutları için aşağıdaki maliyet tahminlerine bakın.

- **Büyük VM**

    25 öğrenci x (20 zamanlanan saat + 10 kota saati) x 70 laboratuvar birimi x 0,01 saat başına USD = 525,00 ABD Doları

- **Küçük GPU (görselleştirme)**

    25 öğrenci x (20 zamanlanan saat + 10 kota saati) x 160 laboratuvar birimi x 0,01 saat başına USD = 1200,00 ABD Doları

> [!IMPORTANT] 
> Maliyet tahmini yalnızca örnek amaçlıdır.  Fiyatlandırma hakkındaki güncel Ayrıntılar için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).

> [!NOTE] 
> PLTW sınıflarının birçoğu, bir tarayıcı aracılığıyla erişilen, MıT App Inventor gibi uygulamalar kullanır.  Bu tarayıcı tabanlı uygulamalar, hızlı bir CPU veya GPU gerektirmez ve internet bağlantısı olan herhangi bir cihazdan erişilebilir.  Öğrenciler bu tür uygulamaları kullanırken, kendi laboratuvar VM 'lerinde tarayıcıyı kullanmak yerine, tarayıcıyı fiziksel cihazından kullanmalarını öneririz.  Bu, yalnızca hızlı bir CPU veya GPU gerektiren uygulamalar için laboratuvar VM 'lerini kullanarak maliyetleri düşük tutmaya yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar
Sonraki adımlar herhangi bir laboratuvarı ayarlamak için ortaktır:

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Öğrenciler Için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users). 