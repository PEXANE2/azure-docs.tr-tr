---
title: Azure Laboratuvar Hizmetleri'nde (UI) şablon VM'de iç içe sanallaştırmayı etkinleştirme | Microsoft Dokümanlar
description: İçinde birden çok VM içeren bir şablon VM'yi nasıl oluşturabilirsiniz öğrenin.  Başka bir deyişle, Azure Lab Hizmetlerinde bir şablon VM'de iç içe sanallaştırmayı etkinleştirin.
services: lab-services
author: emaher
ms.service: lab-services
ms.topic: article
ms.date: 1/23/2020
ms.author: enewman
ms.openlocfilehash: 10f8d2760474631fbcabac4d66139aedf4c7560c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503081"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services-manually"></a>Azure Lab Hizmetleri'ndeki şablon sanal makinede iç içe sanallaştırmayı el ile etkinleştirme

İç içe sanallaştırma, bir laboratuvarın şablon sanal makinesinin içinde çoklu VM ortamı oluşturmanıza olanak tanır. Şablonun yayımı, laboratuardaki her kullanıcıya içinde birden çok VM'nin yer verdiği sanal bir makine sağlar.  İç içe sanallaştırma ve Azure Laboratuvar Hizmetleri hakkında daha fazla bilgi için azure [Lab Hizmetlerinde şablon sanal makinede iç içe sanallaştırmayı etkinleştir'e](how-to-enable-nested-virtualization-template-vm.md)bakın.

Bu makalede, doğrudan Windows rolleri ve araçlarını kullanarak Azure Lab Hizmetleri'nde bir şablon makinesinde iç içe sanallaştırmanın nasıl ayarlanılır.  Bir sınıfın iç içe sanallaştırmayı kullanmasını sağlamak için gereken birkaç şey vardır.  Aşağıdaki adımlar, Hyper-V ile bir Laboratuvar Hizmetleri makine şablonu nasıl el ile ayarlanacak açıklanacaktır.  Adımlar Windows Server 2016 veya Windows Server 2019 için tasarlanmıştır.  

>[!IMPORTANT]
>Laboratuarı oluştururken sanal makine boyutu için **Büyük (iç içe sanallaştırma)** veya **Orta (iç içe sanallaştırma)** seçeneğini belirleyin.  İç içe sanallaştırma aksi takdirde çalışmaz.  

## <a name="enable-hyper-v-role"></a>Hyper-V rolünü etkinleştirme

Aşağıdaki adımlar, Windows Server'da Hyper-V'yi etkinleştirmek için gereken eylemleri Server Manager'ı kullanarak açıklar.  Yükleme başarılı olduktan sonra, Hyper-V yöneticisi istemci sanal makineleri eklemek, değiştirmek ve silmek için kullanılabilir olacaktır.

1. **Sunucu**Yöneticisi'nde, Pano **sayfasında, Rol ve Özellikler Ekle'yi**tıklatın.
2. **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
3. Yükleme **türü seç** sayfasında, Varsayılan Rol tabanlı veya özellik tabanlı yükleme seçimini tutun ve sonra **İleri'yi**tıklatın.
4. Hedef **sunucu** seç sayfasında, sunucu havuzundan sunucu seç'i seçin.   Geçerli sunucu zaten seçilecektir.  İleri'ye tıklayın.
5. Sunucu **rollerini seç** sayfasında **Hyper-V'yi**seçin.  
6. **Rolleri ve Özellikleri Ekle Sihirbazı** açılır pencere görüntülenir.  **Yönetim araçlarını ekle'yi seçin (varsa)**.  Özellikler **Ekle** düğmesini tıklatın.
7. **Sunucu rolleri seç** sayfasında, **İleri** öğesine tıklayın.
8. Özellikleri **Seç sayfasında** **, İleri'yi**tıklatın.
9. **Hyper-V** sayfasında **İleri'yi**tıklatın.
10. Sanal **Anahtarlar Oluştur** sayfasında varsayılanları kabul edin ve **İleri'yi**tıklatın.
11. Sanal **Makine Geçişi** sayfasında varsayılanları kabul edin ve **İleri'yi**tıklatın.
12. Varsayılan **Mağazalar** sayfasında varsayılanları kabul edin ve **İleri'yi**tıklatın.
13. Yükleme **seçimlerini onayla** sayfasında, **gerekirse hedef sunucuyu otomatik olarak yeniden başlat'ı**seçin.
14. Roller **ve Özellikler Ekle Sihirbazı** açılır pencere göründüğünde **Evet'i**tıklatın.
15. **Yükle'yi**tıklatın.
16. Hyper-V rolünün tamamladığını belirtmek için **Yükleme ilerleme** sayfasının bekleyin.  Makine yüklemenin ortasında yeniden başlatılabilir.
17. **Kapat'ı**tıklatın.

## <a name="enable-dhcp-role"></a>DHCP rolünü etkinleştirme

Oluşturulan herhangi bir Hyper-V istemci sanal makineleri, NAT ağında bir IP adresi gerekir.  NAT ağını daha sonra oluşturacağız.  IP adreslerini atamanın bir yolu, ana bilgisayarı, bu durumda bir DHCP sunucusu olarak laboratuvar sanal makine şablonu olarak ayarlamaktır.  Aşağıda DHCP rolünü etkinleştirmek için gereken adımlar verilmiştir.

1. **Sunucu Yöneticisi'nde,** **Pano** **sayfasında, Rol ve Özellikler Ekle'yi**tıklatın.
2. **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
3. Yükleme **türü seç** sayfasında **Rol tabanlı veya özellik tabanlı yüklemeyi** seçin ve **ardından İleri'yi**tıklatın.
4. Hedef **sunucu** yu seç sayfasında, sunucu havuzundan geçerli sunucuyu seçin ve sonra **İleri'yi**tıklatın.
5. Sunucu **rollerini seç** sayfasında **DHCP Server'ı**seçin.  
6. **Rolleri ve Özellikleri Ekle Sihirbazı** açılır pencere görüntülenir.  **Yönetim araçlarını ekle'yi seçin (varsa)**.  **Özellikler Ekle'yi**tıklatın.

    >[!NOTE]
    >Statik IP adresi bulunmadığını belirten bir doğrulama hatası görebilirsiniz.  Bu uyarı senaryomuz için yoksayılabilir.

7. **Sunucu rolleri seç** sayfasında, **İleri** öğesine tıklayın.
8. Özellikleri **Seç** sayfasında **İleri'yi**tıklatın.
9. **DHCP Server** sayfasında **İleri'yi**tıklatın.
10. **Yükleme seçimlerini onaylayın** sayfasında, **Yükle**'ye tıklayın.
11. DHCP rolünün tamamladığını belirtmek için **Yükleme ilerleme sayfasının** bekleyin.
12. Kapat’a tıklayın.

## <a name="enable-routing-and-remote-access-role"></a>Yönlendirme ve Uzaktan Erişim rolünü etkinleştirme

1. **Sunucu Yöneticisi'nde,** **Pano** **sayfasında, Rol ve Özellikler Ekle'yi**tıklatın.
2. **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
3. Yükleme **türü seç** sayfasında **Rol tabanlı veya özellik tabanlı yüklemeyi** seçin ve **ardından İleri'yi**tıklatın.
4. Hedef **sunucu** yu seç sayfasında, sunucu havuzundan geçerli sunucuyu seçin ve sonra **İleri'yi**tıklatın.
5. Sunucu **rollerini seç** sayfasında **Uzaktan Erişim'i**seçin. **Tamam**'a tıklayın.
6. Özellikleri **Seç** sayfasında **İleri'yi**tıklatın.
7. Uzaktan **Erişim** sayfasında **İleri'yi**tıklatın.
8. Rol **Hizmetleri** sayfasında **Yönlendirme'yi**seçin.
9. **Rolleri ve Özellikleri Ekle Sihirbazı** açılır pencere görüntülenir.  **Yönetim araçlarını ekle'yi seçin (varsa)**.  **Özellikler Ekle'yi**tıklatın.
10. **İleri**'ye tıklayın.
11. **Web Sunucusu Rolü (IIS)** sayfasında **İleri**’ye tıklayın.
12. **Rol hizmetlerini seçin** sayfasında, **İleri**’ye tıklayın.
13. **Yükleme seçimlerini onaylayın** sayfasında, **Yükle**'ye tıklayın.
14. Uzaktan Erişim rolünün tamamladığını belirtmek için **Yükleme ilerleme** sayfasının bekleyin.  
15. **Kapat'ı**tıklatın.

## <a name="create-virtual-nat-network"></a>Sanal NAT ağı oluşturma

Artık gerekli tüm roller yüklendiğinden, NAT ağını oluşturma nın zamanı gelmiştir.  Oluşturma işlemi bir anahtar ve NAT ağı, kendisi oluşturma içerir.  Bir NAT (ağ adresi çevirisi) ağı, internetbağlantısına izin vermek için özel bir ağdaki bir grup VM'ye genel bir IP adresi atar.  Bizim durumumuzda, özel VM grubu iç içe vm'ler olacaktır.  NAT ağı iç içe ki VM'lerin birbirleriyle iletişim kurmasına izin verir. Anahtar, bir ağdaki trafiğin alınmasını ve yönlendirmesini işleyen bir ağ aygıtıdır.

### <a name="create-a-new-virtual-switch"></a>Yeni bir sanal anahtar oluşturma

1. Windows Yönetim Araçlarından **Hyper-V Manager'ı** açın.
2. Sol daki gezinme menüsünde geçerli sunucuyu seçin.
3. **Sanal Anahtar Yöneticisi'ni tıklatın...** **Hyper-V Manager'ın**sağ tarafındaki **Eylemler** menüsünden.
4. Sanal **Anahtar Yöneticisi** açılır pencerede, oluşturmak için anahtar türü için **İç'i** seçin.  **Sanal Anahtar Oluştur'u**tıklatın.
5. Yeni oluşturulan sanal anahtar için, adını unutulmaz bir şeye ayarlayın.  Bu örnekte, 'LabServicesSwitch' kullanacağız.  **Tamam**'a tıklayın.
6. Yeni bir ağ bağdaştırıcısı oluşturulur.  Adı 'vEthernet (LabServicesSwitch)' benzer olacaktır.  **Denetim Masası'nı**açmak için **Ağ ve Internet'i**tıklatın, ağ durumunu ve **görevlerini görüntüle'yi**tıklatın.  Solda **bağdaştırıcı ayarlarını değiştir'i**tıklatın.

### <a name="create-a-nat-network"></a>NAT ağı oluşturma

1. Yönlendirme **ve Uzaktan Erişim** aracını Windows Yönetim Araçları'ndan açın.
2. Sol gezinti sayfasındaki yerel sunucuyu seçin.
3. **Eylem** -> **Yapılandırma'yı seçin ve Yönlendirmeyi ve Uzaktan Erişimi Etkinleştirin.**
4. **Yönlendirme ve Uzaktan Erişim Sunucusu Kurulum Sihirbazı** **göründüğünde, İleri'yi**tıklatın.
5. **Yapılandırma** sayfasında Ağ **adresi çevirisi (NAT)** yapılandırmasını seçin.  **İleri**'ye tıklayın.

    >[!WARNING]
    >'Sanal özel ağ (VPN) erişimi ve NAT' seçeneğini seçmeyin.

6. **NAT Internet Connection** sayfasında 'Ethernet'i seçin.  Hyper-V Manager'da oluşturduğumuz 'vEthernet (LabServicesSwitch)' bağlantısını seçmeyin. **İleri**'ye tıklayın.
7. Sihirbazın son sayfasında **Bitir'i** tıklatın.
8. Hizmeti **Başlat** iletişim kutusu görüntülendiğinde, **Hizmeti Başlat'ı**tıklatın.
9. Hizmet başlayana kadar bekleyin.

## <a name="update-network-adapter-settings"></a>Ağ bağdaştırıcısı ayarlarını güncelleştirme

Ağ bağdaştırıcısı, daha önce oluşturulan NAT ağı için varsayılan ağ geçidi IP'si için kullanılan IP ile ilişkilendirilir.  Bu örnekte, 255.255.255.0 alt ağ maskesi ile 192.168.0.1 ip adresi oluştururuz.  Daha önce oluşturulan sanal anahtarı kullanacağız.

1. Denetim **Masası'nı**açın, **Ağ ve Internet'i**tıklatın, **ağ durumunu ve görevlerini görüntüle'yi**tıklatın.
2. Solda **bağdaştırıcı ayarlarını değiştir'i**tıklatın.  
3. Ağ **Bağlantıları** penceresinde, **vEthernet (LabServicesSwitch) Durum** ayrıntıları iletişim kutusunu göstermek için 'vEthernet (LabServicesSwitch)' düğmesine çift tıklayın.
4. **Özellikler** düğmesini tıklatın.
5. **Internet Protocol Sürüm 4 (TCP/IPv4)** öğesini seçin ve **Özellikler** düğmesini tıklatın.
6. Internet **Protokolü Sürüm 4 (TCP/IPv4) Özellikleri** iletişim kutusunda **aşağıdaki IP adresini kullan'ı**seçin.  ip adresi için 192.168.0.1 adresini girin. Alt ağ maskesi için 255.255.255.0 girin.  Varsayılan ağ geçidini boş bırakın.  DNS sunucularını da boş bırakın.

    >[!NOTE]
    > NAT ağımız için ürün yelpazemiz, CIDR gösteriminde 192.168.0.0/24 olacaktır.  Bu, 192.168.0.1 ile 192.168.0.254 arasında kullanılabilir IP adresleri nin bir yelpazesini oluşturur.  Kural olarak, ağ geçitleri bir alt ağ aralığındaki ilk IP adresine sahiptir.

7. Tamam'a tıklayın.

## <a name="create-dhcp-scope"></a>DHCP Kapsamı Oluşturma

Aşağıdaki adımlar DHCP kapsamı eklemek için yönerge vardır.  Bu makalede, NAT ağımız CIDR gösteriminde 192.168.0.0/24'dür.  Bu, 192.168.0.1 ile 192.168.0.254 arasında kullanılabilir IP adresleri nin bir yelpazesini oluşturur.  Oluşturulan kapsam, daha önce oluşturulmuş OLAN IP adresi hariç kullanılabilir adresler aralığında olmalıdır.

1. **Yönetim Araçları'nı** açın ve **DHCP** yönetim aracını açın.
2. **DHCP** aracında, geçerli sunucunun düğümünü genişletin ve **IPv4''u**seçin.
3. Eylem menüsünden **Yeni Kapsam'ı seçin...**
4. Yeni **Kapsam Sihirbazı** göründüğünde, Hoş **Geldiniz** sayfasında **İleri'yi** tıklatın.
5. Kapsam **Adı** sayfasında, 'LabServicesDhcpScope' veya ad için unutulmaz başka bir şey girin.  **İleri**'ye tıklayın.
6. IP **Adres Aralığı** sayfasında aşağıdaki değerleri girin.

    - Başlangıç IP adresi için 192.168.0.100
    - IP adresi sonu için 192.168.0.200
    - Uzunluk için 24
    - Subnet maskesi için 255.255.255.0

7. **İleri**'ye tıklayın.
8. **Dışlamaekle ve Geciktir** sayfasında **İleri'yi**tıklatın.
9. Kira **Süresi** sayfasında **İleri'yi**tıklatın.
10. **DHCP Seçeneklerini Yapılandırıyorum** sayfasında **Evet'i seçin, bu seçenekleri şimdi yapılandırmak istiyorum.** **İleri**'ye tıklayın.
11. **Yönlendiricide (Varsayılan Ağ Geçidi)**
12. Zaten yapılmamışsa 192.168.0.1 ekleyin. **İleri**'ye tıklayın.
13. Etki **Alanı Adı ve DNS Sunucuları** sayfasında, önceden yapılmamışsa 168.63.129.16'yı DNS sunucu IP adresi olarak ekleyin.  168.63.129.16, bir Azure statik DNS sunucusunun IP adresidir. **İleri**'ye tıklayın.
14. WINS **Sunucuları** sayfasında **İleri'yi**tıklatın.
15. Bir **Etkinleştir Kapsamı** sayfası, **Evet seçin, Şimdi bu kapsamı etkinleştirmek istiyorum.**  **İleri**'ye tıklayın.
16. Yeni **Kapsam Sihirbazı'nı Tamamlama** sayfasında **Finish'i**tıklatın.

## <a name="conclusion"></a>Sonuç

Artık şablon makineniz Hyper-V sanal makineleri oluşturmaya hazır.   Hyper-V sanal makinelerin nasıl oluşturulacağına ilişkin talimatlar için [Hyper-V'de Sanal Makine Oluştur'a](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) bakın.  Ayrıca, kullanılabilir işletim sistemlerine ve yazılımlara göz atabilmek için [Microsoft Değerlendirme Merkezi'ne](https://www.microsoft.com/evalcenter/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar herhangi bir laboratuvar kurmak için ortak.

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)