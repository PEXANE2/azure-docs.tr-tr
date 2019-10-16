---
title: VMware olağanüstü durum kurtarma için yapılandırma sunucusunu Azure Site Recovery dağıtma | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile VMware olağanüstü durum kurtarma için bir yapılandırma sunucusunun nasıl dağıtılacağı açıklanır.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/15/2019
ms.author: ramamill
ms.openlocfilehash: 5812cc73fb1da58c591d0593e079851e05bd0940
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331953"
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusunu dağıtma

VMware VM 'Leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarması için [Azure Site Recovery](site-recovery-overview.md) kullandığınızda şirket içi yapılandırma sunucusu dağıtabilirsiniz. Yapılandırma sunucusu, şirket içi VMware ve Azure arasındaki iletişimleri koordine eder. Ayrıca veri çoğaltmasını yönetir. Bu makalede, VMware VM 'lerini Azure 'a çoğaltırken yapılandırma sunucusunu dağıtmak için gereken adımlarda adım adım gösterilmektedir. Fiziksel sunucu çoğaltması için bir yapılandırma sunucusu ayarlamanız gerekiyorsa [Bu makaleyi izleyin](physical-azure-set-up-source.md) .

> [!TIP]
> Azure Site Recovery [mimarinin](vmware-azure-architecture.md)bir parçası olarak yapılandırma sunucusu rolü hakkında bilgi edinebilirsiniz.

## <a name="deployment-of-configuration-server-through-ova-template"></a>OVA şablonu aracılığıyla yapılandırma sunucusu dağıtımı

Yapılandırma sunucusu, belirli minimum donanım ve boyutlandırma gereksinimlerine sahip yüksek oranda kullanılabilir bir VMware VM 'si olarak ayarlanmalıdır. Kullanışlı ve kolay dağıtım için Site Recovery, aşağıda listelenen tüm uygulanan gereksinimleriyle uyumlu yapılandırma sunucusunu ayarlamak için indirilebilir bir OVA (Open Virtualization Application) şablonu sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bir yapılandırma sunucusu için en düşük donanım gereksinimleri aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="azure-active-directory-permission-requirements"></a>Azure Active Directory izin gereksinimleri

Configuration Server 'ı Azure Site Recovery Hizmetleri ile kaydetmek için AAD 'de (Azure Active Directory) ayarlanmış olan **aşağıdaki izinlerden birine** sahip bir kullanıcıya ihtiyacınız vardır.

1. Kullanıcı uygulama oluşturmak için "uygulama geliştiricisi" rolüne sahip olmalıdır.
   1. Doğrulamak için Azure portal oturum açın</br>
   1. Azure Active Directory > Roller ve yöneticiler 'e gidin</br>
   1. "Uygulama geliştiricisi" rolünün kullanıcıya atandığını doğrulayın. Aksi takdirde, bu izinle bir Kullanıcı kullanın veya [izni etkinleştirmek için yöneticiye](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal#assign-roles)ulaşın.
    
1. "Uygulama geliştiricisi" rolü atanamaz, kullanıcının kimlik oluşturması için "kullanıcının uygulamayı kaydedebilmesi" bayrağının doğru olarak ayarlandığından emin olun. Yukarıdaki izinleri etkinleştirmek için
   1. Azure portalında oturum açın
   1. Azure Active Directory > Kullanıcı ayarlarına gidin
   1. \* * Uygulama kayıtları "," kullanıcılar uygulamaları kaydedebilir "seçeneğinin" Evet "olarak seçilmesi gerekir.

      ![AAD_application_permission](media/vmware-azure-deploy-configuration-server/AAD_application_permission.png)

> [!NOTE]
> Active Directory Federasyon Hizmetleri (AD FS) (ADFS) **desteklenmez**. Lütfen [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)ile yönetilen bir hesap kullanın.

## <a name="capacity-planning"></a>Kapasite planlaması

Yapılandırma sunucusu için boyutlandırma gereksinimleri, olası veri değişim hızına bağlıdır. Bu tabloyu kılavuz olarak kullanın.

| **'SUNA** | **Bellek** | **Önbellek diski boyutu** | **Veri değişim oranı** | **Korumalı makineler** |
| --- | --- | --- | --- | --- |
| 8 vCPU (2 yuva * 4 çekirdek \@ 2,5 GHz) |16 GB |300 GB |500 GB veya daha az |100 'den daha az makine çoğaltın. |
| 12 vCPU (2 yuva * 6 çekirdek \@ 2,5 GHz) |18 GB |600 GB |500 GB ila 1 TB |100-150 makinelerini çoğaltın. |
| 16 vCPU (2 yuva * 8 çekirdek \@ 2,5 GHz) |32 GB |1 TB |1 TB-2 TB |150-200 makinelerini çoğaltın. |

Birden fazla VMware VM 'yi çoğaltıyorsanız, [Kapasite planlama konularını](site-recovery-plan-capacity-vmware.md)okuyun. VMWare çoğaltması için [dağıtım planlayıcısı aracını](site-recovery-deployment-planner.md) çalıştırın.

## <a name="download-the-template"></a>Şablonu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için açık sanallaştırma uygulaması (OVA) şablonunu indirin.

   > [!TIP]
   >Yapılandırma sunucusu şablonunun en son sürümünü doğrudan [Microsoft Yükleme Merkezi](https://aka.ms/asrconfigurationserver)' nden de indirebilirsiniz.

> [!NOTE]
> OVA şablonuyla sunulan lisans, 180 gün için geçerli bir değerlendirme lisanssıdır. Bu dönemde, müşterinin Windows 'u bir temin lisansıyla etkinleştirmesi gerekir.

## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere İstemcisi’ni kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın.

     ![OVF şablonu](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. **Kaynak Seç**' de indirilen ovf 'nin konumunu girin.
4. **Ayrıntıları gözden geçirin**' de **İleri**' yi seçin.
5. **Ad ve klasör seçin** ' de, yapılandırma ' yı **seçin**ve varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin. Ölçülü kaynak sağlama seçeneğinin kullanımı, yapılandırma sunucusunun performansını etkileyebilir.
7. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
8. **Tamamlanmaya hazır** bölümünde:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Ek bir ağ arabirimi eklemek için **dağıtımdan sonra Aç**' ı temizleyin ve ardından **son**' u seçin. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

> [!IMPORTANT]
> Kaynak yapılandırmalarını (bellek/çekirdek/CPU kısıtlaması) değiştirmeyin, dağıtımdan sonra yapılandırma sunucusundaki yüklü Hizmetleri veya dosyaları değiştirin/silin. Bu, Configuration Server 'ın Azure hizmetleri ve yapılandırma sunucusunun performansı ile kaydını etkiler.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

> [!NOTE]
> Yük devretmede kaynak makinelerin IP adreslerini tutmayı planlıyor ve şirket içinde daha sonra yeniden çalışma yapmak istiyorsanız iki NIC gereklidir. Kaynak makinelere bir NIC bağlanacak ve diğer NIC Azure bağlantısı için kullanılacaktır.

Yapılandırma sunucusuna ek bir NIC eklemek istiyorsanız, sunucuyu kasaya kaydetmeden önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin.
4. VM açıldığında sanal NIC 'yi bağlamak için, **oturum açma sırasında bağlan**' ı seçin. Sonra, **ileri** > **son** > **Tamam**' ı seçin.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Yapılandırma sunucusunu Azure Site Recovery hizmetleriyle kaydetme

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk kez oturum açtığınızda, birkaç saniye içinde Azure Site Recovery yapılandırma aracı başlatılır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin.</br>
    a. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.</br>
    b. Seçilen Kullanıcı hesabının Azure 'da uygulama oluşturma izinlerine sahip olduğundan emin olun. Gerekli izinleri etkinleştirmek için [burada](#azure-active-directory-permission-requirements)verilen yönergeleri izleyin.
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu Yönetim Sihirbazı birkaç saniye içinde **otomatik olarak** başlar.

### <a name="configure-settings"></a>Ayarları yapılandırma

1. Yapılandırma sunucusu Yönetim Sihirbazı 'nda, **Kurulum bağlantısı**' nı seçin. Açılan kutudan önce, yerleşik işlem sunucusunun kaynak makinelere Mobility hizmetinin bulunması ve anında yüklenmesi için kullandığı NIC 'i seçin ve ardından yapılandırma sunucusunun Azure ile bağlantı için kullanacağı NIC 'yi seçin. Daha sonra **Kaydet**’e tıklayın. Yapılandırıldıktan sonra bu ayarı değiştiremezsiniz. Bir yapılandırma sunucusunun IP adresini değiştirmemelidir. Yapılandırma sunucusuna atanan IP 'nin DHCP IP 'si olmayan STATIK IP olduğundan emin olun.
2. **Kurtarma Hizmetleri kasasını Seç**bölümünde, **Adım 6** ' da "[yapılandırma sunucusunu Azure Site Recovery hizmetleriyle kaydet](#register-the-configuration-server-with-azure-site-recovery-services)" bölümünde kullanılan kimlik bilgileriyle Microsoft Azure için oturum açın.
3. Oturum açtıktan sonra Azure aboneliğinizi ve ilgili kaynak grubunu ve kasasını seçin.

    > [!NOTE]
    > Kaydolduktan sonra, kurtarma hizmetleri kasasını değiştirme esnekliği yoktur.
    > Kurtarma Hizmetleri kasasının değiştirilmesi, geçerli kasadan yapılandırma sunucusu ilişkilendirmesini zorunlu kılabilir ve yapılandırma sunucusu altındaki tüm korumalı sanal makinelerin çoğaltılması durdurulur. [Daha fazla bilgi](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) edinin.

4. **Üçüncü taraf yazılım yüklerken**,

    |Senaryo   |İzlenecek adımlar  |
    |---------|---------|
    |MySQL 'i el ile yükleyebilir & indirebilir miyim?     |  Evet. MySQL uygulamasını indirin & **C:\temp\asrsetup**klasörüne yerleştirip el ile yükleyin. Artık koşulları kabul ettiğinizde > **indir ve yükle**' ye tıkladıktan sonra portal *zaten yüklüdür*. Sonraki adıma geçebilirsiniz.       |
    |MySQL 'i çevrimiçi indirmeyi önleyebilir miyim?     |   Evet. MySQL yükleyici uygulamanızı **C:\temp\asrsetup**klasörüne yerleştirin. Koşulları kabul edin > **indir ve yükle**' ye tıklayın, Portal sizin tarafınızdan eklenen yükleyiciyi kullanır ve uygulamayı yükler. Sonraki adım yüklemeye devam edebilirsiniz.    |
    |Azure Site Recovery aracılığıyla MySQL yüklemek & indirmek istiyorum     |  Lisans sözleşmesini kabul edin & **indir ve yükle**' ye tıklayın. Sonra bir sonraki adım yüklemeye devam edebilirsiniz.       |

5. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle**' yi ve sonra **devam et**' i seçin. Buraya girilen kimlik bilgileri yerel olarak kaydedilir.
8. **Sanal makine kimlik bilgilerini Yapılandır**' da, çoğaltma sırasında Mobility hizmetini otomatik olarak yüklemek için sanal makinelerin Kullanıcı adını ve parolasını girin. **Windows** makineleri için hesabın, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıklarına ihtiyacı vardır. **Linux**için kök hesabının ayrıntılarını sağlayın.
9. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
10. Kayıt tamamlandıktan sonra, Azure portal açın, yapılandırma sunucusunun ve VMware sunucusunun **Kurtarma Hizmetleri kasasında**listelendiğini doğrulayın  >   > **Site Recovery altyapısını** **yönetme** > **yapılandırma Sunucu**.

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu en son sürüme yükseltmek için aşağıdaki [adımları](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)izleyin. Tüm Site Recovery bileşenlerinin nasıl yükseltileceğiyle ilgili ayrıntılı yönergeler için [buraya](service-updates-how-to.md)tıklayın.

## <a name="manage-the-configuration-server"></a>Yapılandırma sunucusunu yönetme

Devam eden çoğaltmanın kesintiye uğramasını önlemek için yapılandırma sunucusu bir kasaya kaydedildikten sonra yapılandırma sunucusunun IP adresinin değişmediğinden emin olun. Ortak yapılandırma sunucusu yönetim görevleri hakkında [buradan](vmware-azure-manage-configuration-server.md)daha fazla bilgi edinebilirsiniz.

## <a name="faq"></a>SSS

1. OVF aracılığıyla dağıtılan yapılandırma sunucusunda ne kadar süre geçerlidir? Lisansı yeniden etkinleştirediğimde ne olur?

    OVA şablonuyla sunulan lisans, 180 gün için geçerli bir değerlendirme lisanssıdır. Süre dolmadan önce, lisansı etkinleştirmeniz gerekir. Diğer bir deyişle, bu, yapılandırma sunucusunun sıklıkla kapatılmasını sağlayabilir ve böylece çoğaltma etkinliklerine yol açabilir.

2. Yapılandırma sunucusunun yüklü olduğu sanal makineyi farklı amaçlar için kullanabilir miyim?

    **Hayır**, VM 'yi yapılandırma sunucusunun tek amacı için kullanmanızı öneririz. Olağanüstü durum kurtarmayı verimli bir şekilde yönetme [önkoşulları](#prerequisites) bölümünde bahsedilen tüm belirtimleri izlediğinizden emin olun.
3. Yapılandırma sunucusunda zaten kayıtlı olan kasayı yeni oluşturulan bir kasaya değiştirebilir miyim?

    **Hayır**, bir kasa yapılandırma sunucusuna kaydedildikten sonra değiştirilemez.
4. Hem fiziksel hem de sanal makineleri korumak için aynı yapılandırma sunucusunu kullanabilir miyim?

    **Evet**, aynı yapılandırma sunucusu fiziksel ve sanal makineleri çoğaltmak için kullanılabilir. Ancak, fiziksel makine yalnızca bir VMware VM 'sine geri alınabilir.
5. Yapılandırma sunucusunun amacı nedir ve nerede kullanılır?

    Yapılandırma sunucusu ve işlevleri hakkında daha fazla bilgi edinmek için [VMware 'Den Azure 'a çoğaltma mimarisi](vmware-azure-architecture.md) ' ne bakın.
6. Yapılandırma sunucusu 'nun en son sürümünü nereden bulabilirim?

    Yapılandırma sunucusunu Portal üzerinden yükseltme adımları için bkz. [yapılandırma sunucusunu yükseltme](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Tüm Site Recovery bileşenlerinin nasıl yükseltileceğiyle ilgili ayrıntılı yönergeler için [buraya](https://aka.ms/asr_how_to_upgrade)bakın.
7. Yapılandırma sunucusu için parolayı nereden indirebilirim?

    Parolayı indirmek için [Bu makaleye](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) bakın.
8. Parolayı değiştirebilir miyim?

    **Hayır**, yapılandırma sunucusunun **parolasını değiştirmemenizi önemle tavsiye** edilir. Parola değişikliği, korunan makinelerin çoğaltılmasını keser ve kritik sağlık durumuna neden olmaktadır.
9. Kasa kayıt anahtarlarını nereden indirebilirim?

    **Kurtarma Hizmetleri kasasında**,  > **Site Recovery altyapısını** > **yapılandırma sunucularını** **yönetin**. Sunucular ' da, kasa kimlik bilgileri dosyasını indirmek için **kayıt anahtarını indir** ' i seçin.
10. Var olan bir yapılandırma sunucusunu kopyalayabilir ve çoğaltma düzenlemesi için kullanabilir miyim?

    **Hayır**, kopyalanmış bir yapılandırma sunucusu bileşeni kullanımı desteklenmiyor. Genişleme işlem sunucusu 'nun kopyası da desteklenmeyen bir senaryodur. Site Recovery bileşenlerin kopyalanması devam eden çoğaltmaları etkiler.

11. Yapılandırma sunucusunun IP 'sini değiştirebilir miyim?

    **Hayır**, bir yapılandırma sunucusunun IP adresini değiştirmemenizi önemle tavsiye edilir. Yapılandırma sunucusuna atanan tüm IP 'lerin, DHCP IP 'Leri için STATIK IP 'Ler olduğundan emin olun.
12. Azure üzerinde yapılandırma sunucusu ayarlayabilir miyim?

    Şirket içi ortamda, v-Center ile doğrudan görüş ve veri aktarımı gecikme sürelerini en aza indirecek şekilde Configuration Server ayarlamanız önerilir. Yapılandırma sunucusunun zamanlanan yedeklemelerini yeniden [çalışma amacıyla](vmware-azure-manage-configuration-server.md#failback-requirements)gerçekleştirebilirsiniz.

Yapılandırma sunucusu hakkında daha fazla bilgi için, [Configuration Server ortak sorularıyla ilgili belgelerimize](vmware-azure-common-questions.md#configuration-server) bakın.

## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Sonraki adımlar

[VMware VM](vmware-azure-tutorial.md) 'lerinin olağanüstü durum kurtarma 'yi Azure 'a ayarlayın.
