---
title: Yapılandırma sunucusunu Azure Site Kurtarma'da dağıtma
description: Bu makalede, Azure Site Kurtarma ile VMware olağanüstü durum kurtarma için bir yapılandırma sunucusu nasıl dağıtılanın açıklanmaktadır
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/15/2019
ms.author: ramamill
ms.openlocfilehash: 5209dab5e0934cc98bb1334a1565cc13998a7d2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257387"
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusunu dağıtma

VMware VM'lerin ve fiziksel sunucuların Azure'da olağanüstü kurtarma durumu için [Azure Site Kurtarma'yı](site-recovery-overview.md) kullandığınızda şirket içi yapılandırma sunucusu dağıtırsınız. Yapılandırma sunucusu şirket içi VMware ve Azure arasındaki iletişimi koordine eder. Ayrıca veri çoğaltma yönetir. Bu makale, VMware VM'leri Azure'a çoğaltırken yapılandırma sunucusunu dağıtmak için gereken adımlarda size yol kat eden bir makaledir. Fiziksel sunucu çoğaltma için bir yapılandırma sunucusu ayarlamanız gerekiyorsa, [bkz.](physical-azure-set-up-source.md)

> [!TIP]
> Azure Site Kurtarma mimarisinin bir parçası olarak yapılandırma sunucusunun rolü hakkında bilgi edinmek [için VMware to Azure olağanüstü durum kurtarma mimarisine](vmware-azure-architecture.md)bakın.

## <a name="deploy-a-configuration-server-through-an-ova-template"></a>BIR YAPıLANDıRMA sunucusuna OVA şablonu aracılığıyla dağıtma

Yapılandırma sunucusu, belirli minimum donanım ve boyutlandırma gereksinimleriyle son derece kullanılabilir bir VMware VM olarak ayarlanmalıdır. Kolay ve kolay dağıtım için Site Kurtarma, burada listelenen tüm zorunlu gereksinimlere uygun yapılandırma sunucusunu kurmak için indirilebilir bir Açık Sanallaştırma Uygulaması (OVA) şablonu sağlar.

## <a name="prerequisites"></a>Ön koşullar

Yapılandırma sunucusu için minimum donanım gereksinimleri aşağıdaki bölümlerde özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="azure-active-directory-permission-requirements"></a>Azure Etkin Dizin izni gereksinimleri

Yapılandırma sunucusunu Azure Site Kurtarma hizmetlerine kaydettirmek için Azure Active Directory 'de (Azure AD) ayarlanmış aşağıdaki izinlerden birine sahip bir kullanıcınız olmalıdır.

1. Kullanıcı, bir uygulama oluşturmak için bir uygulama geliştiricisi rolüne sahip olmalıdır.
    - Doğrulamak için Azure portalında oturum açın.</br>
    - Azure **Etkin Dizin** > **Rolleri'ne ve yöneticilere**gidin.</br>
    - Uygulama geliştiricisi rolünün kullanıcıya atandığını doğrulayın. Değilse, bu izni olan bir kullanıcıyı kullanın veya izni etkinleştirmek için bir [yöneticiye başvurun.](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal#assign-roles)
    
2. Uygulama geliştiricisi rolü atanamazsa, **Kullanıcıların kullanıcının** kimlik oluşturması için uygulamalar bayrağının **doğru** ayarlandığından emin olun. Bu izinleri etkinleştirmek için:
    - Azure Portal’da oturum açın.
    - Azure **Etkin Dizin** > **Kullanıcı ayarlarına**gidin.
    - **Uygulama kayıtları**altında, Kullanıcılar **uygulamaları kaydedebilirsiniz,** **Evet**seçin .

      ![Azure AD_application_permission](media/vmware-azure-deploy-configuration-server/AAD_application_permission.png)

> [!NOTE]
> Active Directory Federation Services *desteklenmiyor.* [Azure Etkin Dizin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)içinde yönetilen bir hesap kullanım.

## <a name="download-the-template"></a>Şablonu indirme

1. Kasada, Altyapı > **Kaynak** **Hazırla'ya**gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için OVA şablonuna indirin.

   > [!TIP]
   >Yapılandırma sunucusu şablonunun en son sürümünü doğrudan [Microsoft Download Center'dan](https://aka.ms/asrconfigurationserver)da indirebilirsiniz.

> [!NOTE]
> OVA şablonuyla sağlanan lisans, 180 gün boyunca geçerli olan bir değerlendirme lisansıdır. Bu süreden sonra, bir lisans satın almanız gerekir.

## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere İstemcisi’ni kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde, **OVF Şablonu Dağıt** sihirbazını başlatmak için **OVF Şablonu** Dağıt'ı'nı seçin.

     ![OVF Şablonu Dağıt](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. **Kaynak seçin** bölümünde, indirilen OVF’nin konumunu girin.
4. **Değerlendirme ayrıntıları** bölümünde **İleri** seçeneğini belirleyin.
5. **Ad ve klasör seçin** ve **Yapılandırma seçin** bölümünde varsayılan ayarları kabul edin.
6. **Select storage** bölümünde, en iyi performans için **Select virtual disk format** bölümünden **Thick Provision Eager Zeroed** seçeneğini belirleyin. İnce sağlama seçeneğinin kullanılması yapılandırma sunucusunun performansını etkileyebilir.
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. **Tamamlanmaya hazır** olduğunda:

    * VM'yi varsayılan ayarlarla ayarlamak için Dağıtım > dan sonra**Güç'ü bitir'i**seçin. **Power on after deployment**
    * Ek bir ağ arabirimi eklemek için **dağıtımdan sonra Güç'ün açık olduğunu**ve ardından **Finish'i**seçin. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

> [!IMPORTANT]
> Bellek, çekirdek ve CPU kısıtlaması gibi kaynak yapılandırmalarını değiştirmeyin veya dağıtımdan sonra yapılandırma sunucusundaki yüklü hizmetleri veya dosyaları değiştirmeyin veya silmeyin. Bu tür değişiklikler, yapılandırma sunucusunun Azure hizmetlerine kaydedilmesi ve yapılandırma sunucusunun performansını etkiler.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

> [!NOTE]
> Kaynak makinelerin IP adreslerini başarısız bir şekilde tutmayı planlıyorsanız ve daha sonra şirket içinde başarısız olmak istiyorsanız iki NIC gerekir. Bir NIC kaynak makinelere bağlı, diğeri NIC ise Azure bağlantısı için kullanılır.

Yapılandırma sunucusuna ek bir NIC eklemek istiyorsanız, sunucuyu kasaya kaydettirmeden önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Ardından **İleri'yi**seçin.
3. Bir bağdaştırıcı türü ve ağ seçin.
4. VM açıkken sanal NIC'yi bağlamak için **power-on'da Bağlan'ı**seçin. Sonra **Sonraki** > Bitiş**Tamam'ı****Finish** > seçin.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Yapılandırma sunucusunu Azure Site Kurtarma hizmetleriyle kaydetme

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk oturum açtığınızda, birkaç saniye içinde Azure Site Kurtarma Yapılandırması aracı başlar.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Ardından **İleri'yi**seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin.</br>
    a. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.</br>
    b. Seçilen kullanıcı hesabının Azure'da bir uygulama oluşturma iznine sahip olduğundan emin olun. Gerekli izinleri etkinleştirmek için [Azure Active Directory izin gereksinimleri](#azure-active-directory-permission-requirements)bölümündeki yönergeleri izleyin.
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetimi sihirbazı birkaç saniye içinde otomatik olarak başlar.

### <a name="configure-settings"></a>Ayarları yapılandırma

1. Yapılandırma sunucusu yönetimi sihirbazında **Kurulum bağlantısı'nı**seçin. Açılan kutulardan, öncelikle dahili işlem sunucusunun kaynak makinelerde mobilite hizmetinin keşfi ve itme yüklemesi için kullandığı NIC'i seçin. Ardından yapılandırma sunucusunun Azure ile bağlantı kurmak için kullandığı NIC'yi seçin. **Kaydet'i**seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez. Yapılandırma sunucusunun IP adresini değiştirmeyin. Yapılandırma sunucusuna atanan IP'nin DHCP IP değil statik bir IP olduğundan emin olun.
2. **Kurtarma Hizmetlerini Seç kasasında,** yapılandırma [sunucusunu Azure Site Kurtarma hizmetlerine kaydedin'in](#register-the-configuration-server-with-azure-site-recovery-services)adım 6'sında kullanılan kimlik bilgileriyle Microsoft Azure'da oturum açın.
3. Oturum açmadan sonra Azure aboneliğinizi ve ilgili kaynak grubunu ve kasasını seçin.

    > [!NOTE]
    > Kayıt tan sonra, kurtarma hizmetleri kasasını değiştirme esnekliği yoktur.
    > Kurtarma hizmetleri kasasının değiştirilmesi, yapılandırma sunucusunun geçerli kasadan ayrıştırılmasını gerektirir ve yapılandırma sunucusunun altındaki tüm korumalı sanal makinelerin çoğaltılması durdurulur. Daha fazla bilgi için [VMware VM olağanüstü durum kurtarma yapılandırma sunucusunu yönet'e](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault)bakın.

4. **Üçüncü taraf yazılım yükleme:**

    |Senaryo   |İzlenen adımlar  |
    |---------|---------|
    |MySQL'i el ile indirip yükleyebilir miyim?     |  Evet. MySQL uygulamasını indirin, **C:\Temp\ASRSetup**klasörüne yerleştirin ve el ile yükleyin. Şartları kabul ettikten ve **İndir ve yükle'yi**seçtikten sonra, portal *zaten yüklü*olduğunu söylüyor. Bir sonraki adıma geçebilirsiniz.       |
    |MySQL'in çevrimiçi olarak indirilmesini önleyebilir miyim?     |   Evet. MySQL yükleyici uygulamanızı **C:\Temp\ASRSetup**klasörüne yerleştirin. Koşulları kabul edin, **İndir ve yükle'yi**seçin ve portal uygulamayı yüklemek için eklediğiniz yükleyiciyi kullanır. Yükleme bittikten sonra bir sonraki adıma geçin.    |
    |Azure Site Kurtarma üzerinden MySQL'i indirip yüklemek istiyorum.    |  Lisans sözleşmesini kabul edin ve **İndir ve yükle'yi**seçin. Yükleme bittikten sonra bir sonraki adıma geçin.       |

5. **Cihaz yapılandırmasına onaylayın,** devam etmeden önce ön koşullar doğrulanır.
6. **VCenter Server/vSphere ESXi sunucusunu yapılandırın,** çoğaltmak istediğiniz VM'lerin bulunduğu vCenter sunucusunun FQDN veya IP adresini veya vSphere ana bilgisayarını girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Devam Ekle'yi** > **Continue**seçin. Burada girilen kimlik bilgileri yerel olarak kaydedilir.
8. **Sanal makine kimlik bilgilerini yapılandırıldığında,** çoğaltma sırasında mobilite hizmetini otomatik olarak yüklemek için sanal makinelerin kullanıcı adını ve parolasını girin. **Windows** makineleri için, hesabın çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıklarına ihtiyacı vardır. **Linux**için, kök hesap için ayrıntıları sağlar.
9. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
10. Kayıt bittikten sonra Azure portalını açın ve yapılandırma sunucusunun ve VMware sunucusunun **Recovery Services Vault** > **Manage** > **Site Recovery Infrastructure** > **Configuration Servers'da**listeli olduğunu doğrulayın.

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu en son sürüme yükseltmek [için VMware VM olağanüstü durum kurtarma yapılandırma sunucusunu yönet'e](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)bakın. Tüm Site Kurtarma bileşenlerinin nasıl yükseltilenhakkında talimatlar için Site [Kurtarma'daki Hizmet güncelleştirmelerine](service-updates-how-to.md)bakın.

## <a name="manage-the-configuration-server"></a>Yapılandırma sunucusunu yönetme

Devam eden çoğaltmakesintilerini önlemek için, yapılandırma sunucusu nun kasasına kaydedildikten sonra yapılandırma sunucusunun IP adresinin değişmediğinden emin olun. Yaygın yapılandırma sunucusu yönetimi görevleri hakkında daha fazla bilgi edinmek [için VMware VM olağanüstü durum kurtarma yapılandırma sunucusunu yönet'e](vmware-azure-manage-configuration-server.md)bakın.

## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

Dağıtım & bağlantı sorunlarını çözmek için [sorun giderme makalemize](vmware-azure-troubleshoot-configuration-server.md) bakın.

## <a name="faqs"></a>SSS

* OVF aracılığıyla dağıtılan bir yapılandırma sunucusunda sağlanan lisans ne kadar süreyle geçerlidir? Lisansı yeniden etkinleştirmezsem ne olur?

    OVA şablonuyla sağlanan lisans, 180 gün için geçerli bir değerlendirme lisansıdır. Son kullanma tarihinden önce lisansı etkinleştirmeniz gerekir. Aksi takdirde, yapılandırma sunucusunun sık sık kapanmasına neden olabilir ve çoğaltma etkinliklerinin engelolmasına neden olabilir. Daha fazla bilgi için [VMware VM olağanüstü durum kurtarma yapılandırma sunucusunu yönet'e](vmware-azure-manage-configuration-server.md#update-windows-license)bakın.

* Yapılandırma sunucusunun farklı amaçlariçin yüklendiği VM'yi kullanabilir miyim?

    Hayır. VM'yi yapılandırma sunucusunun tek amacı için kullanın. Olağanüstü durum kurtarmanın etkin yönetimi için [Önkoşullar'da](#prerequisites) belirtilen tüm özellikleri uyguladığınıza emin olun.
* Yapılandırma sunucusunda kayıtlı olan kasayı yeni oluşturulan bir kasayla değiştirebilir miyim?

    Hayır. Bir kasa yapılandırma sunucusuna kaydedildikten sonra değiştirilemez.
* Hem fiziksel hem de sanal makineleri korumak için aynı yapılandırma sunucusunu kullanabilir miyim?

    Evet. Aynı yapılandırma sunucusu fiziksel ve sanal makineleri çoğaltmak için kullanılabilir. Ancak, fiziksel makine yalnızca vmware VM'ye geri başarısız olabilir.
* Yapılandırma sunucusunun amacı nedir ve nerede kullanılır?

    Yapılandırma sunucusu ve işlevleri hakkında daha fazla bilgi edinmek [için VMware to Azure çoğaltma mimarisine](vmware-azure-architecture.md)bakın.
* Yapılandırma sunucusunun en son sürümünü nerede bulabilirim?

    Yapılandırma sunucusunu portal üzerinden yükseltme adımları için [bkz.](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) Tüm Site Kurtarma bileşenlerinin nasıl yükseltilenhakkında talimatlar için Site [Kurtarma'daki Hizmet güncelleştirmelerine](https://aka.ms/asr_how_to_upgrade)bakın.
* Yapılandırma sunucusu için parolayı nerede indirebilirim?

    Parolayı indirmek için [vmware VM olağanüstü durum kurtarma yapılandırma sunucusunu yönet'e](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase)bakın.
* Şifreyi değiştirebilir miyim?

    Hayır. Yapılandırma sunucusunun parolasını değiştirmeyin. Paroladaki bir değişiklik, korumalı makinelerin çoğaltılmasını bozar ve kritik bir sağlık durumuna yol açar.
* Kasa kayıt anahtarlarını nereden indirebilirim?

    **Kurtarma Hizmetleri Vault'ta**Site Kurtarma**Altyapı** > **Yapılandırma Sunucularını** **Yönet'i** > seçin. **Sunucularda,** kasa kimlik bilgilerini indirmek için **kayıt anahtarını** indir'i seçin.
* Varolan bir yapılandırma sunucusunun klonlanıp çoğaltma düzenlemesi için kullanabilir miyim?

    Hayır. Klonlanmış yapılandırma sunucusu bileşeninin kullanımı desteklenmez. Ölçeklendirme işlem sunucusunun klonlanması da desteklenmeyen bir senaryodur. Klonlama Site Kurtarma bileşenleri devam eden çoğaltmaları etkiler.

* Bir yapılandırma sunucusunun IP'sini değiştirebilir miyim?

    Hayır. Yapılandırma sunucusunun IP adresini değiştirmeyin. Yapılandırma sunucusuna atanan tüm IP'lerin DHCP IP'leri değil, statik IP'ler olduğundan emin olun.
* Azure'da bir yapılandırma sunucusu ayarlayabilir miyim?

    V-Center ile doğrudan görüş hattı yla şirket içi bir ortamda bir yapılandırma sunucusu ayarlayın ve veri aktarım gecikmelerini en aza indirin. Hata [geri alma amacıyla](vmware-azure-manage-configuration-server.md#failback-requirements)yapılandırma sunucusunun zamanlanmış yedeklerini alabilirsiniz.

* Bir yapılandırma sunucusunda veya ölçeklendirme işlem sunucusunda önbellek sürücüsünü değiştirebilir miyim?

    Hayır, Önbellek sürücüsü ayarlandıktan sonra değiştirilemez.

Yapılandırma sunucularında daha fazla SSS için [Configuration server sık sorulan sorulara](vmware-azure-common-questions.md#configuration-server)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM'lerin](vmware-azure-tutorial.md) Azure'a olağanüstü durum kurtarmasını ayarlayın.
