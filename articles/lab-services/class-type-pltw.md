---
title: Proje lideri, Azure Lab Services ile laboratuvarları ayarlama
description: Proje lideri sınıfları öğretmek için laboratuvarları ayarlamayı öğrenin.
ms.topic: article
ms.date: 10/28/2020
ms.openlocfilehash: ca4fdae2372895c17c4a98dd3959935108846744
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024628"
---
# <a name="set-up-labs-for-project-lead-the-way-classes"></a>Proje için laboratuvarları ayarlama sınıfları

[Proje lideri (PLTW)](https://www.pltw.org/) , &ndash; bilgisayar bilimi, mühendislik ve biyotıp bilimi Içinde Birleşik Devletler için prek 12 müfreyi sağlayan kar amacı gütmeyen bir kuruluştur.  Öğrenciler her bir PLTW sınıfında, uygulamalı öğrenme deneyiminin bir parçası olarak çeşitli yazılım uygulamaları kullanır.  Yazılım uygulamalarının birçoğu hızlı bir CPU veya bazı durumlarda bir GPU gerektirir.  Bu makalede, genellikle notlar 9 12 ' deki öğrencilerle sunulan aşağıdaki PLTW sınıfları için laboratuvarların nasıl ayarlanacağı gösterilmektedir &ndash; :

- **Mühendislik tasarımına giriş**

    Öğrenciler, 3D modelleme için [Autodesk Inventor bilgisayar destekli tasarım (CAD)](https://www.autodesk.com/products/inventor/new-features) yazılımını kullanmayı da kapsayan mühendislik tasarımı işlemine sunulmuştur.

- **Mühendislik ilkeleri**
    
    Öğrenciler mühendislik mekanizmaları, yapısal ve malzeme gücü ve otomasyon hakkında bilgi edinin.  Bu sınıf [md Kesintisds](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/MD+Solids/MD+Solids+Software+Installation+Guide.pdf), [Batı Point köprü tasarımcısı](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/West+Point+Bridge+Builder/Installation+Guide+for+West+Point+Bridge+Designer.pdf)ve [Amerika 'nın Army simülasyonu](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/America's+Army/Installation+Guide+for+Americas+Army+Simulation+17-18.pdf)gibi yazılımları kullanır.

- **Hukuki mühendislik ve mimari**

    Öğrenciler, 3D oluşturma bilgileri modelleme (BıM) için [Autodesk](https://www.autodesk.com/products/revit/overview) yeniden yapılandırma mimarisi tasarım yazılımını kullanarak oluşturma ve site tasarımı ve geliştirmeyi öğrenmektedir.

- **Bilgisayar tümleşik üretimi**

    Öğrenciler, Robotics ve otomasyon içeren modern üretim süreçlerini keşfedebilir.   Bu sınıfta, öğrenciler [Autodesk ıNVENTOR CAD](https://www.autodesk.com/products/inventor/new-features) ve [Autodesk Inventor bilgisayar DESTEKLI üretim (cam)](https://www.autodesk.com/products/inventor-cam/overview) yazılımını kullanır. 

- **Dijital elektronik aygıtlar**

    Öğrenciler, [ulusal araç Multisım](https://www.ni.com/en-us/shop/electronic-test-instrumentation/application-software-for-electronic-test-and-instrumentation-category/what-is-multisim.html) simülasyonu ve devre tasarımı yazılımlarını kullanarak elektronik mantık devreleri ve cihazları inceleyerek.

- **Mühendislik tasarımı ve geliştirme**

    Öğrenciler, mühendislerin bir paneline sundukları araştırma, tasarım ve testi birleştirerek uçtan uca bir çözüme katkıda bulunur.  Bu sınıfta, öğrenciler [Autodesk Inventor CAD](https://www.autodesk.com/products/inventor/new-features) yazılımını kullanır.

- **Bilgisayar bilimi temelleri**

    Öğrenciler, hesaplama kavramları ve araçlarına eklenmiştir.  Bunlar blok tabanlı programlama ile başlar ve ardından [Vexcode v5 blokları](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/VEXcode+V5+Blocks/VexCode+V5+Blocks+Installation+Guide.pdf)gibi kodlama ortamlarını kullanarak metin tabanlı kodlamaya geçer.

- **Bilgisayar bilimi Ilkeleri**
    
    Öğrenciler, [Microsoft Visual Studio kod geliştirme ortamını](https://code.visualstudio.com/)kullanarak, [Python](https://www.python.org/) ile programlama uzmanlığını büyüyüp genişletmektedir. 

- **Bilgisayar bilimi A**

    Öğrenciler, mobil uygulama geliştirmeyi öğrenerek bu sınıftaki programlama uzmanlarını genişletir.  Bu sınıfta, [Microsoft Visual Studio kod geliştirme ortamını](https://code.visualstudio.com/)kullanarak [Java](https://www.java.com/) öğrenirler.  Öğrenciler ayrıca, kullanıcıların mobil uygulama kodlarını çalıştırmasına ve test etmesine izin veren bir öykünücü kullanır.  Azure Lab Services bir öykünücü ayarlama hakkında daha fazla bilgi için [Azure Lab Services başvurun](mailto:AzLabsCOVIDSupport@microsoft.com).

Sınıf yazılımının tam listesi için her sınıf için [Pltw sitesine](https://www.pltw.org/pltw-software) gidin.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

PLTW için Labs ayarlamaya başlamak üzere bir Azure aboneliğine ve laboratuvar hesabına ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni laboratuvar hesabı oluşturma hakkında daha fazla bilgi için bkz. [Laboratuvar hesabı ayarlama](./tutorial-setup-lab-account.md). Ayrıca var olan bir laboratuvar hesabı da kullanabilirsiniz.

Bir laboratuvar hesabı ayarladıktan sonra okulunuzun sunduğu her PLTW sınıfı oturumunda ayrı bir laboratuvar oluşturmanız gerekir.  Ayrıca, her PLTW sınıfı türü için ayrı görüntüler oluşturmanızı öneririz.  Laboratuvarlarınızı ve görüntülerinizi nasıl yapılandıracağınıza ilişkin daha fazla bilgi için, bkz. [bir fiziksel laboratuvardan Azure Lab Services için](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)blog gönderisi.

### <a name="lab-account-settings"></a>Laboratuvar hesabı ayarları

Aşağıdaki tabloda açıklandığı gibi laboratuvar hesabı ayarlarınızı etkinleştirin. Azure Marketi görüntülerinin nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [Laboratuvar oluşturucuları için kullanılabilen Azure Marketi görüntülerini belirtme](./specify-marketplace-images.md).

| Laboratuvar hesabı ayarı | Yönergeler |
| -------------------- | ----- |
| Market görüntüsü | Laboratuvar hesabınızda kullanılmak üzere Windows 10 Pro görüntüsünü etkinleştirin. |

<br>

### <a name="lab-settings"></a>Laboratuvar ayarları
PLTW sınıfları için kullanmayı öneriyoruz sanal makinenin (VM) boyutu, öğrencilerinizin sınıfta yaptığı iş yüklerinin türlerine bağlıdır.  Daha önce listelenen sınıflar için küçük GPU (görselleştirme) ve büyük VM boyutlarının kullanılmasını öneririz. PLTW sınıflarınız için laboratuvarları ayarlarken aşağıdaki tablodaki kılavuza başvurun:

| Laboratuvar ayarı | Değer ve açıklama | Sınıf önerisi |
| ------------ | ------------------ | --- |
| Sanal makine boyutu | **Küçük GPU (görselleştirme)**<br>OpenGL ve DirectX gibi çerçeveler ile uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. | Aşağıdaki PLTW sınıfları için bu boyutu kullanmanızı öneririz: hukuki mühendislik ve mimari, dijital elektronik, Bilgisayar tümleşik üretim ve mühendislik tasarımı ve geliştirme.
| Sanal makine boyutu | **Büyük**<br>Daha hızlı CPU 'Lar, daha iyi yerel disk performansı, büyük veritabanları ve büyük bellek önbellekler gerektiren uygulamalar için idealdir. | Aşağıdaki PLTW sınıfları için bu boyutu kullanmanızı öneririz: mühendislik tasarımına, mühendislik Ilkelerine, bilgisayar bilimi esalarına, bilgisayar bilimi Ilkelerine ve bilgisayar bilimi ilkelerine giriş. |

<br>

### <a name="license-server"></a>Lisans sunucusu
Daha önce bahsedilen PLTW sınıflarında kullanılan yazılımların çoğu bir lisans sunucusuna *erişim gerektirmez.*  Ancak, aşağıdaki yazılımlar için Autodesk ağ lisanslama modelini kullanmayı planlıyorsanız bir lisans sunucusuna erişmeniz gerekir:
-   Uyant
-   Inventor
-   Inventor CAM

Autodesk yazılımıyla ağ lisansını kullanmak için Pltw, Autodesk ağ Lisans Yöneticisi 'Ni lisans sunucunuza yüklemek için [ayrıntılı adımlar sağlar](https://www.pltw.org/pltw-software) .  Bu lisans sunucusu, genellikle şirket içi ağınızda bulunur veya Azure sanal ağı içindeki içindeki bir Azure sanal makinesinde (VM) barındırılır.

Lisans sunucunuz kurulduktan sonra, sanal ağı [Laboratuvar hesabınızla](./tutorial-setup-lab-account.md) [eşetmeniz](./how-to-connect-peer-virtual-network.md) gerekir. Laboratuvar sanal makinelerinizin lisans sunucusuna erişebilmesi ve tam tersi şekilde laboratuvar oluşturmadan *önce* Ağ eşlemesini yapmanız gerekir.

Autodesk tarafından oluşturulan lisans dosyaları, lisans sunucusunun MAC adresini gömer.  Lisans sunucunuzu bir Azure VM kullanarak barındırmaya karar verirseniz, lisans sunucunuzun MAC adresinin değişmediğinden emin olmak önemlidir. MAC adresi değişirse, lisans dosyalarınızı yeniden oluşturmanız gerekir. MAC adresinin değiştirilmesini engellemek için şunları yapın:

- Lisans sunucunuzu barındıran Azure VM için [statik bir özel IP ve MAC adresi ayarlayın](./how-to-create-a-lab-with-shared-resource.md#static-private-ip-and-mac-address) .
- Bu kaynakları daha sonra yeni bir bölgeye veya konuma taşımanız gerekmiyorsa, laboratuvar hesabınızı ve lisans sunucusunun sanal ağını yeterli VM kapasitesi olan bir bölgede veya konumda ayarladığınızdan emin olun.

Daha fazla bilgi için bkz. [bir lisans sunucusunu paylaşılan kaynak olarak ayarlama](./how-to-create-a-lab-with-shared-resource.md).

### <a name="template-machine"></a>Şablon makinesi
PLTW için ihtiyacınız olan yükleme dosyalarından bazıları büyük. Dosyaları bir laboratuvar şablonu VM 'sine indirdiğinizde kopyalamanın uzun sürme süresini çok uzun sürebilirler.

Yükleme dosyalarını şablon makinesine indirmek ve her şeyi yüklemek yerine, fiziksel ortamınızda PLTW görüntülerini oluşturmanız önerilir.  Daha sonra, Laboratuvarlarınızı oluşturmak için bunları kullanabilmeniz için özel görüntüleri paylaşılan görüntü galerisine aktarabilirsiniz.  Daha fazla bilgi için bkz. [paylaşılan görüntü galerisine özel bir görüntü yükleme](./upload-custom-image-shared-image-gallery.md).

Bu öneriyi takip eden bir laboratuvar ayarlamaya yönelik başlıca görevleri göz önünde bulabilirsiniz:

1. Fiziksel ortamınızda, sınıfının görüntüsünü oluşturun.

    a.  PLTW 'ın yükleme dosyalarını indirmek ve gerekli yazılımı yüklemek için ayrıntılı adımlarını kullanın.

    > [!NOTE]    
    > Autodesk uygulamalarını yüklediğinizde, yüklemekte olduğunuz bilgisayarın lisans sunucusu ile iletişim kurabilmesi gerekir. Autodesk Yükleme Sihirbazı, lisans sunucusunun barındırıldığı makinenin bilgisayar adını belirtmenizi ister.  Lisans sunucunuzu bir Azure VM 'de barındırıyorsanız, yükleme sihirbazının lisans sunucunuza erişebilmesi için bkz. Autodesk 'yi laboratuvar şablonu VM 'ye yüklemek için beklemeniz gerekebilir.

    b.  Okulunuzun kullanabileceği OneDrive veya diğer yedekleme seçeneklerini [yükleyin ve yapılandırın](./how-to-prepare-windows-template.md#install-and-configure-onedrive) .
    
    c.  [Windows güncelleştirmelerini yükleyip yapılandırın](./how-to-prepare-windows-template.md#install-and-configure-updates).

1.  Özel görüntüyü [Laboratuvar hesabınıza eklenmiş paylaşılan görüntü galerisine](./how-to-attach-detach-shared-image-gallery.md)yükleyin.

1.  Bir laboratuvar oluşturun ve ardından önceki adımda karşıya yüklediğiniz özel görüntüyü seçin.

1.  Laboratuvar oluşturulduktan sonra görüntünün beklendiği gibi çalışıp çalışmadığını doğrulamak için, şablonu sanal makineye başlatın ve bağlayın.

1.  Son olarak, Öğrenciler sanal makinelerini oluşturmak için şablon VM 'yi yayımlayın.

## <a name="student-devices"></a>Öğrenci cihazları
Öğrenciler, Windows bilgisayarları, Mac ve Kmebook 'tan laboratuvar VM 'lerine bağlanabilir. Yönergeler için bkz.

- [Windows 'dan Bağlan](./how-to-use-classroom-lab.md#connect-to-the-vm)
- [Mac 'ten Bağlan](./connect-virtual-machine-mac-remote-desktop.md)
- [Kmebook 'tan Bağlan](./connect-virtual-machine-chromebook-remote-desktop.md)

## <a name="cost"></a>Maliyet
PLTW sınıflarının örnek maliyet tahminini kapsayalım.  Bu tahmin, bir lisans sunucusu çalıştırmanın veya paylaşılan bir görüntü galerinin maliyetini içermez. Her birinin 20 saat zamanlanan sınıf zamanı olan 25 öğrencsahip bir sınıfınız olduğunu varsayalım.  Her öğrenciye, zamanlanan sınıf süresi dışında, ödevler veya atamalar için 10 saatlik ek kota saati de vardır.  Tahmini maliyetler aşağıda verilmiştir:

- **Büyük VM**

    25 öğrenci &times; (20 zamanlanan saat + 10 kota saati) &times; 70 laboratuvar birimi, &times; saat başına 0,01 ABD Doları = ABD Doları 525.00

- **Küçük GPU (görselleştirme)**

    25 öğrenci &times; (20 zamanlanan saat + 10 kota saati) &times; 160 laboratuvar birimi, &times; saat başına 0,01 ABD Doları = ABD Doları 1200.00

> [!IMPORTANT] 
> Maliyet tahmini yalnızca örnek amaçlıdır.  Geçerli fiyatlandırma bilgileri için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).

> [!NOTE] 
> PLTW sınıflarının birçoğu, bir tarayıcı aracılığıyla erişilen, MıT App Inventor gibi uygulamalar kullanır.  Bu tarayıcı tabanlı uygulamalar hızlı bir CPU veya GPU gerektirmez ve bunlara internet bağlantısı olan herhangi bir cihazdan erişebilirsiniz.  Öğrenciler bu tür uygulamaları kullanırken, kendi laboratuvar VM 'lerine tarayıcı yerine, tarayıcıyı fiziksel cihazından kullanmalarını öneririz. Öğrenciler, yalnızca hızlı bir CPU veya GPU gerektiren uygulamalar için laboratuvar VM 'leri kullanarak maliyetleri daha düşük tutmaya yardımcı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Laboratuvarınızı ayarlarken aşağıdaki makalelere bakın:

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users) 
