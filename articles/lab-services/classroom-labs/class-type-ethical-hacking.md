---
title: Azure Lab Hizmetleri ile Etik Hacking laboratuvarı kurma | Microsoft Dokümanlar
description: Etik korsanlığı öğretmek için Azure Lab Hizmetlerini kullanarak nasıl bir laboratuvar kurabileceğinizi öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 2b600edc4c360a2b2990be34e44bb8fbd1c8f721
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74133184"
---
# <a name="set-up-a-lab-to-teach-ethical-hacking-class"></a>Etik hack sınıf öğretmek için bir laboratuvar kurmak 
Bu makalede, etik hack adli tarafında odaklanan bir sınıf kurmak için nasıl gösterir. Etik korsanlık topluluğu tarafından kullanılan bir uygulama olan penetrasyon testi, birisi kötü amaçlı bir saldırganın yararlanabileceği güvenlik açıklarını göstermek için sisteme veya ağa erişmeye çalıştığında oluşur. 

Etik bir hack sınıfında, öğrenciler güvenlik açıklarına karşı savunmak için modern teknikler öğrenebilirler. Her öğrenci iki iç içe sanal makine olan bir Windows Server ana bilgisayar sanal makine alır - [Metasploitable3](https://github.com/rapid7/metasploitable3) görüntü ve [Kali Linux](https://www.kali.org/) görüntü ile başka bir makine ile bir sanal makine. Metasploitable sanal makine amaçlar istismar için kullanılır ve Kali sanal makine adli görevleri yürütmek için gerekli araçlara erişim sağlar.

Bu makalede iki ana bölüm vardır. İlk bölümde sınıf laboratuarı nasıl oluşturulacak. İkinci bölümde, iç içe sanallaştırma etkin leştirilmiş şablon makinesinin nasıl oluşturulacak ve gerekli araçlar ve görüntülerle nasıl oluşturulacak ları ele alınır. Bu durumda, Hyper-V görüntüleri barındırmak için etkin olan bir makinede metasploitable görüntü ve Kali Linux görüntü.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması
Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliği aldıktan sonra, Azure Laboratuvar Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilir veya varolan bir hesabı kullanabilirsiniz. Yeni bir laboratuvar hesabı oluşturmak için aşağıdaki öğreticiye bakın: [Bir laboratuvar hesabı kurmak için öğretici.](tutorial-setup-lab-account.md)

Yeni bir laboratuvar oluşturmak ve ardından aşağıdaki ayarları uygulamak için [bu öğreticiyi](tutorial-setup-classroom-lab.md) izleyin:

| Sanal makine boyutu | Görüntü |
| -------------------- | ----- | 
| Orta (İç içe Sanallaştırma) | Windows Server 2019 Datacenter |

## <a name="template-machine"></a>Şablon makinesi 

Şablon makinesi oluşturulduktan sonra, makineyi çalıştırın ve aşağıdaki üç ana görevi tamamlamak için makineye bağlanın. 
 
1. İç içe sanallaştırma için makineyi ayarlayın. Hyper-V gibi tüm uygun windows özelliklerini sağlar ve Hyper-V görüntülerinin birbirleriyle ve internetle iletişim kurabilmesi için ağ kurmayı sağlar.
2. [Kali](https://www.kali.org/) Linux görüntüsünü ayarlayın. Kali penetrasyon testi ve güvenlik denetimi için araçlar içeren bir Linux dağıtımıdır.
3. Metasploitable görüntüsünü ayarlayın. Bu örnekte, [Metasploitable3](https://github.com/rapid7/metasploitable3) görüntü kullanılacaktır. Bu görüntü, güvenlik açıklarına sahip olmak için oluşturulan.

Yukarıda özetlenen görevleri otomatikleştiren bir komut dosyası [Lab Services Etik Hacking Scripts](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/EthicalHacking)mevcuttur.

### <a name="prepare-template-machine-for-nested-virtualization"></a>İç içe sanallaştırma için şablon makinesini hazırlama
İç içe sanallaştırma için şablon sanal makine hazırlamak için [bu makalede](how-to-enable-nested-virtualization-template-vm.md) yönergeleri izleyin. 

### <a name="set-up-a-nested-virtual-machine-with-kali-linux-image"></a>Kali Linux Image ile iç içe bir sanal makine ayarlama
Kali penetrasyon testi ve güvenlik denetimi için araçlar içeren bir Linux dağıtımıdır.

1. Görüntüyü ' [https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/)den indirin.  
    1. Hyper-V için **Kali Linux Hyper-V 64 Bit'i** indirin.
    1. 0,7z dosyasını ayıklayın.  Zaten 7 zip yoksa, [https://www.7-zip.org/download.html](https://www.7-zip.org/download.html)indirin . Daha sonra gereksinim duyduğunuz için çıkarılan klasörün konumunu hatırlayın.
2. İdari Araçlardan **Hyper-V Yöneticisi'ni** açın.
1. **Eylem'i**seçin ve ardından **Sanal Makineyi Aktar'ı**seçin. 
1. **Sanal Makine'yi Al** sihirbazının **Klasörü Bul** sayfasında Kali Linux görüntüsünü tutan ayıklanan klasörün konumunu seçin.

    ![Klasörü Bul iletişim kutusu](../media/class-type-ethical-hacking/locate-folder.png)
1. Sanal **Makine Seç** sayfasında Kali Linux resmini seçin.  Bu durumda, görüntü **kali-linux-2019.3-hyperv**olduğunu.

    ![Kali görüntüsünü seçin](../media/class-type-ethical-hacking/select-kali-image.png)
1.  Tür **Seç** **sayfasında, sanal makineyi kopyala 'yı seçin (yeni bir benzersiz kimlik oluşturun)**.

    ![Alma türünü seçin](../media/class-type-ethical-hacking/choose-import-type.png)
1. **Sanal Makine Dosyaları için Klasörleri Seç'teki** varsayılan ayarları kabul edin ve Sanal Sabit **Diskler sayfalarını depolamak için Klasörleri Seçin** ve ardından **İleri'yi**seçin.
1. Ağı **Bağla** sayfasında, bu makalenin **İç Içe Sanallaştırma Şablonu Hazırlama** bölümünde daha önce oluşturulmuş **LabServicesSwitch'i** seçin ve ardından **İleri'yi**seçin.

    ![Ağ sayfasını bağla](../media/class-type-ethical-hacking/connect-network.png)
1. **Özet** sayfasında **Bitir'i** seçin. Kopyalama ve alma işlemleri tamamlanana kadar bekleyin. Kali Linux sanal makine şimdi Hyper-V satışa sunulacak.
1. **Hyper-V Manager'dan** **Action** -> **Start'ı**seçin, ardından sanal makineye bağlanmak için **Action** -> **Connect'i** seçin.  
12. Varsayılan kullanıcı adı `root` ve parola `toor`. 

    > [!NOTE]
    > Görüntünün kilidini açmanız gerekiyorsa CTRL tuşuna basın ve fareyi yukarı doğru sürükleyin.

## <a name="set-up-a-nested-vm-with-metasploitable-image"></a>Metasploitable Image ile iç içe bir VM ayarlama  
Rapid7 Metasploitable görüntü, güvenlik açıklarıyla kasıtlı olarak yapılandırılan bir görüntüdür. Sorunları test etmek ve bulmak için bu resmi kullanırsınız. Aşağıdaki yönergeler, önceden oluşturulmuş bir Metasploitable görüntüsünü nasıl kullanacağınızı gösterir. Ancak, Metasploitable görüntünün daha yeni bir sürümü [https://github.com/rapid7/metasploitable3](https://github.com/rapid7/metasploitable3)gerekiyorsa, bkz.

1. 'ye [https://information.rapid7.com/download-metasploitable-2017.html](https://information.rapid7.com/download-metasploitable-2017.html)gidin. Resmi indirmek için formu doldurun ve **Gönder** düğmesini seçin.
1. **Metasploitable Now** dosyasını seçin.
1. Zip dosyası indirildiğinde, zip dosyasını ayıklayın ve konumu hatırlayın.
1. Çıkarılan vmdk dosyasını vhdx dosyasına dönüştürün, böylece Hyper-V ile kullanabilirsiniz. Bunu yapmak için PowerShell'i yönetim ayrıcalıklarıyla açın ve vmdk dosyasının bulunduğu klasöre gidin ve aşağıdaki yönergeleri izleyin:
    1. Microsoft [Virtual Machine Converter'ı](https://www.microsoft.com/download/details.aspx?id=42497)indirin ve istendiğinde mvmc_setup.msi dosyasını çalıştırın.
    1. PowerShell modülünü içeri aktarın.  Modülün yüklü olduğu varsayılan konum C:\Program Files\Microsoft Virtual Machine Converter\

        ```powershell
        Import-Module 'C:\Program Files\Microsoft Virtual Machine Converter\MvmcCmdlet.psd1'
        ```
    1. Vmdk'yı Hyper-V tarafından kullanılabilecek bir vhd dosyasına dönüştürün. Bu işlem birkaç dakika sürebilir.
    
        ```powershell
        ConvertTo-MvmcVirtualHardDisk -SourceLiteralPath .\Metasploitable.vmdk -DestinationLiteralPath .\Metasploitable.vhdx -VhdType DynamicHardDisk -VhdFormat vhdx
        ```
    1. Yeni oluşturulan metasploitable.vhdx'i C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\'e kopyalayın. 
1. Yeni bir Hyper-V sanal makine oluşturun.
    1. **Açık Hyper-V Yöneticisi**.
    1. **Eylem** -> **Yeni** -> Sanal**Makine**seçin.
    1. **Yeni Sanal Makine Sihirbazı'nın**Başlamadan **Önce** sayfasında **İleri'yi**tıklatın.
    1. Ad **ve Konum belirt** sayfasında, **ad**için **Metasploitable'ı** girin ve **İleri'yi**seçin.

        ![Yeni VM görüntü sihirbazı](../media/class-type-ethical-hacking/new-vm-wizard-1.png)
    1. Oluşturma **Belirt** sayfasında varsayılanları kabul edin ve **İleri'yi**seçin.
    1. Bellek **Atla** sayfasında, **başlangıç belleği**için **512 MB** girin ve **İleri'yi**seçin. 

        ![Bellek sayfası atama](../media/class-type-ethical-hacking/assign-memory-page.png)
    1. **Ağ** Yapılandırma sayfasında, bağlantıyı **Bağlı Değil**olarak bırakın. Ağ bağdaştırıcısını daha sonra ayarlarsınız.
    1. Sanal **Sabit Diske Bağla** sayfasında **varolan sanal sabit diski kullan'ı**seçin. Önceki adımda oluşturulan **metasploitable.vhdx** dosyasının konumuna göz atın ve **İleri'yi**seçin. 

        ![Sanal ağ disk sayfasını bağlama](../media/class-type-ethical-hacking/connect-virtual-network-disk.png)
    1. Yeni **Sanal Makine Sihirbazı'nı Tamamlama** sayfasında Ve **Finish'i**seçin.
    1. Sanal makine oluşturulduktan sonra Hyper-V Manager'da seçin. Henüz makineyi açma.  
    1. **Eylem** -> **Ayarlarını**seçin.
    1. **Metasploitable** iletişim kutusu için Ayarlar'da **Donanım Ekle'yi**seçin. 
    1. **Eski Ağ Bağdaştırıcısı'nı**seçin ve **Ekle'yi**seçin.

        ![Ağ bağdaştırıcısı sayfası](../media/class-type-ethical-hacking/network-adapter-page.png)
    1. Eski **Ağ Bağdaştırıcısı** sayfasında, **Sanal Geçiş** ayarı için **LabServicesSwitch'i** seçin ve **Tamam'ı**seçin. LabServicesSwitch, **İç içe Sanallaştırma Şablonu Hazırlama** bölümünde Hyper-V için şablon makinesi hazırlanırken oluşturulmuştur.

        ![Eski Ağ bağdaştırıcısı sayfası](../media/class-type-ethical-hacking/legacy-network-adapter-page.png)
    1. Metasploitable görüntü kullanıma hazırdır. **Hyper-V Manager'dan** **Action** -> **Start'ı**seçin, ardından sanal makineye bağlanmak için **Action** -> **Connect'i** seçin.  Varsayılan kullanıcı adı **msfadmin** ve parola **msfadmin**olduğunu. 


Şablon şimdi güncellenir ve etik bir hack penetrasyon test sınıfı için gerekli görüntüler, penetrasyon testi yapmak için araçlar ve keşfetmek için güvenlik açıkları ile başka bir görüntü ile bir görüntü vardır. Şablon görüntüsü artık sınıfa yayımlanabilir. Şablonu laboratuvarda yayınlamak için şablon **sayfasındayayım** düğmesini seçin.
  

## <a name="cost"></a>Maliyet  
Bu laboratuvarın maliyetini tahmin etmek isterseniz, aşağıdaki örneği kullanabilirsiniz: 
 
20 saatlik planlanan ders süresi ve ödev veya ödevler için 10 saatlik kontenjanı olan 25 öğrenciden oluşan bir sınıf için, laboratuvar fiyatı: 

25 öğrenci * (20 + 10) saat * 55 Laboratuvar Birimi * 0,01 USD /saat = 412,50 USD. 

Fiyatlandırma hakkında daha fazla bilgi için Azure [Lab Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/lab-services/)bakın.

## <a name="conclusion"></a>Sonuç
Bu makalede, etik hack sınıfı için bir laboratuvar oluşturmak için adımları ile yürüdü. Bu, ana bilgisayar sanal makine içinde penetran test için iki sanal makine oluşturmak için iç içe sanallaştırma kurmak için adımlar içerir.

## <a name="next-steps"></a>Sonraki adımlar
Sonraki adımlar herhangi bir laboratuvar kurmak için ortak:

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users). 

