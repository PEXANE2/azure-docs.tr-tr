---
title: Azure Site Kurtarma ile StorSimple dosya paylaşımı DR'sini otomatikleştirin | Microsoft Dokümanlar
description: Microsoft Azure StorSimple depolama sunda barındırılan dosya paylaşımları için olağanüstü durum kurtarma çözümü oluşturma adımlarını ve en iyi uygulamaları açıklar.
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: alkohli
ms.openlocfilehash: 650798fdb884e6494990efb533335a1dd8b4d89f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67875387"
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>StorSimple'da barındırılan dosya paylaşımları için Azure Site Kurtarma'yı kullanarak Otomatik Olağanüstü Durum Kurtarma çözümü

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple, dosya paylaşımlarıyla sık ilişkili yapılandırılmamış verilerin karmaşıklığını gideren karma bir bulut depolama çözümüdür. StorSimple, şirket içi çözümün bir uzantısı olarak bulut depolamayı kullanır ve verileri şirket içi depolama ve bulut depolama da otomatik olarak katmanlar. Yerel ve bulut anlık görüntüleriyle tümleşik veri koruması, genişleyen bir depolama altyapısına olan ihtiyacı ortadan kaldırır.

[Azure Site Kurtarma,](../site-recovery/site-recovery-overview.md) sanal makinelerin çoğaltma, başarısız olma ve kurtarma işlemini düzenleyerek olağanüstü durum kurtarma (DR) özellikleri sağlayan Azure tabanlı bir hizmettir. Azure Site Kurtarma, sanal makineler ve uygulamalar üzerinde özel/genel veya barındırılan bulutları sürekli olarak çoğaltmak, korumak ve sorunsuz bir şekilde başarısız olmak için bir dizi çoğaltma teknolojisini destekler.

Azure Site Kurtarma, sanal makine çoğaltma ve StorBasit bulut anlık görüntü özelliklerini kullanarak dosya sunucusu ortamının tamamını koruyabilirsiniz. Bir aksaklık durumunda, dosya paylaşımlarınızı Azure'da yalnızca birkaç dakika içinde çevrimiçi duruma getirmek için tek bir tıklama kullanabilirsiniz.

Bu belge, StorSimple depolama alanında barındırılan dosya paylaşımlarınız için nasıl bir olağanüstü durum kurtarma çözümü oluşturabileceğinizi ve tek tıklamayla kurtarma planı kullanarak planlı, planlanmamış ve test hatalarını nasıl gerçekleştirebileceğinizi ayrıntılı olarak açıklar. Özetle, olağanüstü durum senaryoları sırasında StorSimple hatalarını etkinleştirmek için Azure Site Kurtarma kasanızdaki Kurtarma Planı'nı nasıl değiştirebileceğinizi gösterir. Buna ek olarak, desteklenen yapılandırmaları ve ön koşulları açıklar. Bu belge, Azure Site Kurtarma ve StorBasit mimarilerinin temellerini bildiğinizi varsayar.

## <a name="supported-azure-site-recovery-deployment-options"></a>Desteklenen Azure Site Kurtarma dağıtım seçenekleri
Müşteriler dosya sunucularını Hyper-V veya VMware'de çalışan fiziksel sunucular veya sanal makineler (VM' ler) olarak dağıtabilir ve storSimple depolamasundan oyulmuş birimlerden dosya paylaşımları oluşturabilir. Azure Site Kurtarma, hem fiziksel hem de sanal dağıtımları ikincil bir siteye veya Azure'a koruyabilir. Bu belge, Hyper-V'de barındırılan bir dosya sunucusu VM'nin kurtarma sitesi olarak azure ile birlikte bir DR çözümünün ayrıntılarını ve StorSimple depolama alanında dosya paylaşımlarını kapsar. Dosya sunucusu VM'nin VM veya fiziksel bir makinede olduğu diğer senaryolar da benzer şekilde uygulanabilir.

## <a name="prerequisites"></a>Ön koşullar
StorSimple depolama alanında barındırılan dosya paylaşımları için Azure Site Kurtarma'yı kullanan tek tıklamayla olağanüstü durum kurtarma çözümü uygulamak aşağıdaki ön koşullara sahiptir:

   - Şirket içi Windows Server 2012 R2 Dosya sunucusu VM Hyper-V veya VMware veya fiziksel bir makinede barındırılan
   - Azure StorSimple yöneticisine kayıtlı şirket içi StorSimple depolama aygıtı
   - Azure StorSimple yöneticisinde oluşturulan StorSimple Cloud Appliance. Cihaz kapalı bir durumda tutulabilir.
   - StorSimple depolama aygıtında yapılandırılan birimlerde barındırılan dosya paylaşımları
   - Microsoft Azure aboneliğinde oluşturulan [Azure Site Kurtarma hizmetleri kasası](../site-recovery/site-recovery-vmm-to-vmm.md)

Ayrıca, Azure kurtarma sitenizse, Azure Sanal MM'ler ve Azure Site Kurtarma hizmetleriyle uyumlu olduğundan emin olmak için Sanal Makinelere [Hazırlık Değerlendirme aracını](https://azure.microsoft.com/downloads/vm-readiness-assessment/) VM'lerde çalıştırın.

Gecikme sorunları (daha yüksek maliyetlere neden olabilir) önlemek için, StorSimple Cloud Appliance, otomasyon hesabı ve depolama hesabınızı(lar) aynı bölgede oluşturduğunuzdan emin olun.

## <a name="enable-dr-for-storsimple-file-shares"></a>StorSimple dosya paylaşımları için DR'yi etkinleştirme
Tam çoğaltma ve kurtarma sağlamak için şirket içi ortamın her bileşeninin korunması gerekir. Bu bölümde nasıl açıklanır:
    
   - Etkin Dizin ve DNS çoğaltmasını ayarlama (isteğe bağlı)
   - Dosya sunucusu VM'nin korunmasını sağlamak için Azure Site Kurtarma'yı kullanma
   - StorSimple birimlerinin korunmasını etkinleştirin
   - Ağı yapılandırma

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Etkin Dizin ve DNS çoğaltmasını ayarlama (isteğe bağlı)
Active Directory ve DNS çalıştıran makineleri DR sitesinde kullanılabilmeleri için korumak istiyorsanız, bunları açıkça korumanız gerekir (böylece dosya sunucularına kimlik doğrulama ile başarısız olduktan sonra erişilebilir). Müşterinin şirket içi ortamının karmaşıklığına bağlı olarak önerilen iki seçenek vardır.

#### <a name="option-1"></a>Seçenek 1
Müşterinin az sayıda uygulaması, şirket içi sitenin tamamı için tek bir etki alanı denetleyicisi varsa ve tüm site üzerinde başarısız olacaksa, etki alanı denetleyici makinesini ikincil bir şekilde çoğaltmak için Azure Site Kurtarma çoğaltmasını kullanmanızı öneririz site (bu hem siteden siteye hem de siteden azure'a uygulanabilir).

#### <a name="option-2"></a>2. Seçenek
Müşterinin çok sayıda uygulaması varsa, Active Directory ormanı çalıştırıyorsa ve aynı anda birkaç uygulama da başarısız oluyorsa, DR sitesinde (ikincil bir site veya Azure'da) ek bir etki alanı denetleyicisi ayarlamanızı öneririz.

Dr sitesinde bir etki alanı denetleyicisini kullanıma sunarken, talimatlar için [Azure Site Kurtarma'yı kullanarak Active Directory ve DNS için Otomatik DR çözümüne](../site-recovery/site-recovery-active-directory.md) bakın. Bu belgenin geri kalanı için, DR sitesinde bir etki alanı denetleyicisi olduğunu varsayıyoruz.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Dosya sunucusu VM'nin korunmasını sağlamak için Azure Site Kurtarma'yı kullanma
Bu adım, şirket içi dosya sunucusu ortamını hazırlamanızı, bir Azure Site Kurtarma kasası oluşturmanızı ve hazırlamanızı ve VM'nin dosya korumasını etkinleştirmenizi gerektirir.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Şirket içi dosya sunucusu ortamını hazırlamak için
1. Kullanıcı **Hesabı Denetimini** **Asla Bildirmeyecek**şekilde ayarlayın. Azure Site Kurtarma tarafından başarısız olduktan sonra iSCSI hedeflerini bağlamak için Azure otomasyon komut dosyalarını kullanabilmeniz için bu gereklidir.
   
   1. Windows +Q tuşuna basın ve **UAC'yi**arayın.  
   1. **Kullanıcı Hesabı Denetimayarlarını Değiştir'i**seçin.  
   1. **Çubuğu Asla Bildirmeye**doğru alta sürükleyin.  
   1. **Tamam'ı** tıklatın ve istendiğinde **Evet'i** seçin.  
   
      ![Kullanıcı Hesabı Kontrol Ayarları](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png) 

1. VM Aracısını her dosya sunucusu VM'ye yükleyin. Bu, Azure otomasyon komut dosyalarını VM'ler üzerinde başarısız olduğunda çalıştırabilmeniz için gereklidir.
   
   1. [Aracıyı](https://aka.ms/vmagentwin) ' `C:\\Users\\<username>\\Downloads`ya indirin.
   1. Windows PowerShell'i Yönetici modunda açın (Yönetici olarak çalıştırın) ve ardından indirme konumuna gitmek için aşağıdaki komutu girin:  
         `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`
         
         > [!NOTE]
         > Dosya adı sürüme bağlı olarak değişebilir.
      
1. **İleri**'ye tıklayın.
1. Anlaşma Koşullarını kabul **edin** ve **Sonra İleri'yi**tıklatın.
1. **Son**'a tıklayın.
1. StorSimple depolama alanına oyulmuş birimleri kullanarak dosya paylaşımları oluşturun. Daha fazla bilgi için [bkz.](storsimple-manage-volumes.md)
   
   1. Şirket içi VM'lerinizde Windows +Q tuşuna basın ve **iSCSI'yi**arayın.
   1. **iSCSI başlatıcısı**seçin.
   1. **Yapılandırma** sekmesini seçin ve başlatıcı adını kopyalayın.
   1. [Azure portalına](https://portal.azure.com/)giriş yapın.
   1. **StorSimple** sekmesini seçin ve ardından fiziksel aygıtı içeren StorSimple Manager Service'i seçin.
   1. Birim kapsayıcı(lar) oluşturun ve sonra birim(ler) oluşturun. (Bu birimler dosya sunucusu VMs dosya paylaşımı(lar) içindir. Başlatıcı adını kopyalayın ve birimleri oluştururken Access Control Records için uygun bir ad verin.
   1. **Yapılaşı** sekmesini seçin ve aygıtın IP adresini not edin.
   1. Şirket içi VM'lerinizde **iSCSI başlatıcısına** tekrar gidin ve Hızlı Bağlantı bölümündeIP girin. **Hızlı Bağlan'ı** tıklatın (aygıt artık bağlı olmalıdır).
   1. Azure portalını açın ve **Birimler ve Aygıtlar** sekmesini seçin. **Otomatik Yapılandırma'yı**tıklatın. Oluşturduğunuz birim görünmelidir.
   1. **Portalda, Aygıtlar** sekmesini seçin ve ardından **Yeni Sanal Aygıt Oluştur'u seçin.** (Bu sanal aygıt bir arıza oluşursa kullanılır). Bu yeni sanal aygıt, ek maliyetlerden kaçınmak için çevrimdışı durumda tutulabilir. Sanal aygıtı çevrimdışı naalmak için Portal'daki **Sanal Makineler** bölümüne gidin ve kapatın.
   1. Şirket içi VM'lere geri dön ve Disk Yönetimini aç (Windows tuşu + X tuşuna basın ve **Disk Yönetimini**seçin).
   1. Bazı ekstra diskler fark edeceksiniz (oluşturduğunuz birim sayısına bağlı olarak). İlkini sağ tıklatın, **Diski Başlatma'yı**seçin ve **Tamam'ı**seçin. **Ayrılmamış** bölümü sağ tıklatın, **Yeni Basit Birim'i**seçin, bir sürücü harfi atayın ve sihirbazı bitirin.
   1. Tüm diskler için adım l'yi tekrarlayın. Artık **Bu bilgisayardaki** tüm diskleri Windows Gezgini'nde görebilirsiniz.
   1. Bu birimlerde dosya paylaşımları oluşturmak için Dosya ve Depolama Hizmetleri rolünü kullanın.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Azure Site Kurtarma kasası oluşturmak ve hazırlamak için
Dosya sunucusu VM'yi korumadan önce Azure Site Kurtarma'ya başlamak için [Azure Site Kurtarma belgelerine](../site-recovery/site-recovery-hyper-v-site-to-azure.md) bakın.

#### <a name="to-enable-protection"></a>Koruma sağlamak için
1. iSCSI hedefini(ler) Azure Site Kurtarma yoluyla korumak istediğiniz şirket içi VM'lerden çıkarın:
   
   1. Windows + Q tuşuna basın ve **iSCSI'yi**arayın.
   1. **iSCSI başlatıcısını ayarla'yı**seçin.
   1. Daha önce bağladığınız StorSimple aygıtının bağlantısını kesin. Alternatif olarak, korumayı etkinleştirirken dosya sunucusunu birkaç dakika lığına kapatabilirsiniz.
      
   > [!NOTE]
   > Bu, dosya paylaşımların geçici olarak kullanılamamasına neden olur.
   
1. Azure Site Kurtarma portalından dosya sunucusu VM'nin [sanal makine korumasını etkinleştirin.](../site-recovery/site-recovery-hyper-v-site-to-azure.md)
1. İlk eşitleme başladığında, hedefi yeniden bağlayabilirsiniz. iSCSI başlatıcısına gidin, StorSimple aygıtını seçin ve **Bağlan'ı**tıklatın.
1. Eşitleme tamamlandığında ve VM'nin durumu **korunduğunda,** VM'yi seçin, **Yapılandırma** sekmesini seçin ve VM'nin ağını buna göre güncelleştirin (bu, VM(ler) üzerinde başarısız olanın parçası olacağı ağdır). Ağ görünmüyorsa, eşitleme nin devam ettiği anlamına gelir.

### <a name="enable-protection-of-storsimple-volumes"></a>StorSimple birimlerinin korunmasını etkinleştirin
StorSimple birimleri için bu birim seçeneği **için varsayılan yedeklemeyi etkinleştir'i** seçmediyseniz, StorSimple Manager hizmetinde **Yedekleme İlkeleri'ne** gidin ve tüm birimler için uygun bir yedekleme ilkesi oluşturun. Yedekleme sıklığını uygulama için görmek istediğiniz kurtarma noktası hedefine (RPO) ayarlamanızı öneririz.

### <a name="configure-the-network"></a>Ağı yapılandırma
Dosya sunucusu VM için, Azure Site Kurtarma'daki ağ ayarlarını, başarısız olduktan sonra Doğru DR ağına bağlanacak şekilde yapılandırın.

Aşağıdaki resimde gösterildiği gibi ağ ayarlarını yapılandırmak için **Çoğaltılan öğeler** sekmesinde VM'yi seçebilirsiniz.

![İşlem ve Ağ](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
Dosya paylaşımlarının başarısızlık işlemini otomatikleştirmek için ASR'de bir kurtarma planı oluşturabilirsiniz. Bir aksaklık oluşursa, dosya paylaşımlarını tek bir tıklamayla birkaç dakika içinde yukarı taşıyabilirsiniz. Bu otomasyonu etkinleştirmek için bir Azure otomasyon hesabına ihtiyacınız olacaktır.

#### <a name="to-create-an-automation-account"></a>Otomasyon hesabı oluşturmak için
1. Azure portalı &gt; **Otomasyonu** bölümüne gidin.
1. **+ Ekle** düğmesine tıklayın, bıçağın altında açılır.
   
   ![Automation Hesabı ekleme](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)
   
   - Ad - Yeni bir otomasyon hesabı girin
   - Abonelik - Abonelik seçin
   - Kaynak grubu - Yeni oluşturma/varolan kaynak grubunu seç
   - Konum - Konumu seçin, StorSimple Cloud Appliance ve Depolama Hesaplarının oluşturulduğu aynı coğrafi/bölgede saklayın.
   - Azure Çalıştır Hesabı oluştur - **Evet** seçeneğini belirleyin.
   
1. Otomasyon hesabına gidin, gerekli tüm runbook'ları otomasyon hesabına almak için **RunBook'lara** &gt; **Gözat Galerisi'ni** tıklatın.
1. Galeride Olağanüstü Durum **Kurtarma** etiketini bularak aşağıdaki runbook'ları ekleyin:
   
   - Test Failover (TFO) sonra StorSimple hacimleri temizleme
   - Failover StorBasit hacim kapları
   - Arıza dan sonra StorSimple cihazına montaj hacimleri
   - Azure VM'de özel komut dosyası uzantısını kaldırma
   - StorSimple Sanal Cihaz'ı Başlat
   
      ![Galeriye Gözat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)
   
1. Otomasyon hesabındaki runbook'u seçerek tüm komut dosyalarını yayımlayın ve **Yayımla'yı** **Ve** &gt; ardından doğrulama iletisine **Evet'i** tıklatın. Bu adımdan **sonra, Runbook'lar** sekmesi aşağıdaki gibi görünür:
   
   ![Runbook'lar](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)
   
1. Otomasyon **hesabında, Değişkenler** &gt; **Değişkenleri ekleyin** ve aşağıdaki değişkenleri ekleyin'i tıklatın. Bu varlıkları şifrelemeyi seçebilirsiniz. Bu değişkenler kurtarma planına özgütür. Bir sonraki adımda oluşturacağınız kurtarma planınız TestPlan ise, değişkenleriniz TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName ve benzeri olmalıdır.

   - **BaseUrl**: Azure bulutu için Kaynak Yöneticisi url'si. **Get-AzEnvironment |'yi kullanmaya alın | Select-Object Adı, ResourceManagerUrl** cmdlet.
   - _RecoveryPlanName_**-ResourceGroupName**: StorSimple kaynağına sahip Kaynak Yöneticisi grubu.
   - _RecoveryPlanName_**-ManagerName**: StorSimple aygıtına sahip StorSimple kaynağı.
   - _RecoveryPlanName_**-DeviceName**: Üzerinde başarısız olması gereken StorSimple Cihazı.
   - _RecoveryPlanName_**-DeviceIpAddress**: Cihazın IP adresi (Bu StorSimple Device Manager **bölümü** &gt; &gt; ayarları **Ağ** &gt; **DNS Ayarları** grubu altında **Cihazlar** sekmesinde bulunabilir).
   - _RecoveryPlanName_**-VolumeContainers**: Cihazda üzerinde başarısız olması gereken virgülden ayrılmış hacim kapları dizesi; örneğin: volcon1, volcon2, volcon3.
   - _RecoveryPlanName_**-TargetDeviceName**: Konteynerlerin üzerinde başarısız olunması gereken StorSimple Cloud Appliance.
   - _RecoveryPlanName_**-TargetDeviceIpAddress**: Hedef aygıtın IP adresi (Bu Sanal &gt; **Makine** bölümü **Ayarlar** grup &gt; **Ağ** sekmesinde bulunabilir).
   - _RecoveryPlanName_**-StorageAccountName**: Komut dosyasının (VM üzerinden başarısız olduğu üzerinde çalışması gereken) depolanacak depolama hesabı adıdır. Bu, komut dosyasını geçici olarak depolamak için biraz alanı olan herhangi bir depolama hesabı olabilir.
   - _RecoveryPlanName_**-StorageAccountKey**: Yukarıdaki depolama hesabının erişim anahtarı.
   - _RecoveryPlanName_**-VMGUIDS**: Bir VM'yi koruduktan sonra, Azure Site Kurtarma her VM'ye VM üzerinden başarısız olanın ayrıntılarını veren benzersiz bir kimlik atar. VMGUID'i elde etmek için **Kurtarma Hizmetleri** sekmesini seçin ve **Korumalı Madde** &gt; **Koruma Grupları** &gt; Makineleri **Özellikleri'ni** **Machines** &gt; tıklatın. Birden çok VM'niz varsa, GUID'leri virgülle ayrılmış dize olarak ekleyin.

     Örneğin, kurtarma planının adı fileServerpredayRP ise, tüm varlıkları ekledikten sonra **Değişkenler,** **Bağlantılar** ve **Sertifikalar** sekmeniz aşağıdaki gibi görünmelidir.

      ![Varlıklar](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

1. StorSimple 8000 serisi Runbook modüllerini Otomasyon hesabınıza yükleyin. Modül eklemek için aşağıdaki adımları kullanın:
   
   1. Powershell'i açın, klasöre dizin değiştirmek & yeni bir klasör oluşturun.
      
      ```
            mkdir C:\scripts\StorSimpleSDKTools
            cd C:\scripts\StorSimpleSDKTools
      ```
   1. Nuget CLI'yi Adım1'de aynı klasörün altına indirin.
      nuget.exe çeşitli sürümleri [nuget indirme](https://www.nuget.org/downloads)mevcuttur. Her indirme bağlantısı doğrudan bir .exe dosyasına işaret edin, bu nedenle dosyayı tarayıcıdan çalıştırmak yerine sağ tıklayıp bilgisayarınıza kaydettiğinizden emin olun.
      
      ```
            wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
      ```
      
   1. Bağımlı SDK'yı indirin
      
      ```
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
      ```
      
   1. StorSimple 8000 Serisi cihaz yönetimi için bir Azure Otomasyon Runbook Modülü oluşturun. Bir Otomasyon modülü zip dosyası oluşturmak için aşağıdaki komutları kullanın.
         
      ```powershell
            # set path variables
            $downloadDir = "C:\scripts\StorSimpleSDKTools"
            $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

            #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
            mkdir "$moduleDir"

            copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
            copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

            #Don't change the name of the Archive
            compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
      ```
         
   1. Yukarıdaki adımda oluşturulan Azure Otomasyon modülü zip dosyasını (Microsoft.Azure.Management.StorSimple8000Series.zip) aktarın. Bu, Otomasyon Hesabı'nı seçerek yapılabilir, PAYLAŞıLAN KAYNAKLAR altında **Modüller'i** tıklatın ve ardından **modül ekle'yi**tıklatın.
   
   StorSimple 8000 serisi modüllerini aldıktan sonra **Modüller** sekmesi aşağıdaki gibi görünmelidir:
   
      ![Modules](./media/storsimple-disaster-recovery-using-azure-site-recovery/image12.png)

1. **Kurtarma Hizmetleri** bölümüne gidin ve daha önce oluşturduğunuz Azure Site Kurtarma kasasını seçin.
1. Kurtarma **Planları (Site Kurtarma)** seçeneğini **Yönet** grubundan seçin ve aşağıdaki gibi yeni bir kurtarma planı oluşturun:
   
   - **+ Plan Recover** butonuna tıklayın, bıçağın altında açılır.
      
      ![Kurtarma planı oluşturma](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)
      
   - Kurtarma planı adı girin, Kaynak, Hedef & Dağıtım modeli değerlerini seçin.
   
   - Kurtarma planına eklemek istediğiniz koruma grubundan VM'leri seçin ve **Tamam** düğmesini tıklatın.
   
   - Daha önce oluşturduğunuz Kurtarma planı'nı seçin, Kurtarma planı özelleştirme görünümünü açmak için **Özelleştir** düğmesini tıklatın.
   
   - **Tüm grupların kapatılmasına** sağ tıklayın ve **ön eylem ekle'yi**tıklatın.
   
   - Eylem kılıcı ekle, bir ad girin, Çalıştırılan Yerde **Birincil yan** seçeneğini seçin, Otomasyon hesabını (runbook'ları eklediğiniz) seçin ve ardından **Failover-StorSimple-Volume-Containers** runbook'u seçin.
   
   - Grup 1'e sağ tıklayın: **Korumalı öğeleri ekle** seçeneğini **başlatın** ve tıklatın, ardından kurtarma planında korunacak VM'leri seçin ve **Tamam** düğmesini tıklatın. İsteğe bağlı, zaten seçili VM'ler.
   
   - Grup 1'e sağ tıklayın: **Eylem gönder** seçeneğini **başlatın** ve tıklatın ve ardından aşağıdaki tüm komut dosyalarını ekleyin:  
      
      - Başlangıç-StorBasit-Sanal-Cihaz runbook  
      - Başarısız aşırı StorBasit hacim-kapsayıcılar runbook  
      - Montaj hacimleri-sonra-failover runbook  
      - Kaldırma-özel komut dosyası uzantısı runbook  
        
   - Aynı **Grup 1: Adımlar sonrası** bölümünde yukarıdaki 4 komut dosyasından sonra el ile eylem ekleyin. Bu eylem, her şeyin doğru çalıştığını doğrulayabilmeniz indiği noktadır. Bu eylemin yalnızca Test başarısızlığı bir parçası olarak eklenmesi gerekir (bu nedenle yalnızca **Test Failover** onay kutusunu seçin).
    
   - El ile eylemden sonra, diğer runbook'lar için kullandığınız yordamı kullanarak **Temizleme** komut dosyasını ekleyin. Kurtarma planını **kaydedin.**
    
   > [!NOTE]
   > Bir test başarısızlığını çalıştırırken, el ile eylem adımında her şeyi doğrulamanız gerekir, çünkü hedef aygıtta klonlanmış olan StorSimple birimleri, el ile eylem tamamlandıktan sonra temizlemenin bir parçası olarak silinir.
       
      ![Kurtarma planı](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Bir test başarısız yapma
Test başarısızolması sırasında Active Directory'ye özgü hususlar için [Active Directory DR Solution](../site-recovery/site-recovery-active-directory.md) çözüm kılavuzuna bakın. Test başarısız olduğunda şirket içi kurulum hiç rahatsız edilmez. Şirket içi VM'ye eklenen StorSimple birimleri Azure'daki StorSimple Cloud Appliance'a klonlanır. Azure'da test amaçlı bir VM getirilir ve klonlanan birimler VM'ye eklenir.

#### <a name="to-perform-the-test-failover"></a>Test başarısızını gerçekleştirmek için
1. Azure portalında Site Kurtarma kasanızı seçin.
1. Dosya sunucusu VM için oluşturulan kurtarma planını tıklatın.
1. **Test Failover'ı**tıklatın.
1. Başarısız olduktan sonra Azure Sanal M'lerinin bağlanacağı Azure sanal ağını seçin.
   
   ![Failover'ı Başlat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
   
1. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Özelliklerini açmak için VM'ye tıklayarak veya kasa adı **Test failover job** &gt; **İşler** &gt; Sitesi Kurtarma **işlerinde**test başarısız işi yle ilerlemeyi izleyebilirsiniz.
1. Failover tamamlandıktan sonra, azure makinesinin Azure portalı &gt; Sanal **Makineler'de**görüntülenebildiğini de görebilmelisin. Doğrulamalarınızı gerçekleştirebilirsiniz.
1. Doğrulamalar tamamlandıktan sonra **Doğrulamaları Tamamla'yı**tıklatın. Bu, StorSimple Ciltlerini kaldırır ve StorSimple Cloud Appliance'ı kapatır.
1. İşi bittikten sonra, kurtarma planında **Temizleme testi başarısız** olduğunu tıklatın. Notlar olarak test başarısızile ilgili tüm gözlemleri kaydedin ve kaydedin. Bu, test başarısızolurken oluşturulan sanal makineyi siler.

## <a name="perform-a-planned-failover"></a>Planlı bir hata yapma
   Planlanan bir hata sırasında, şirket içi dosya sunucusu VM incelikle kapatılır ve StorSimple aygıtındaki birimlerin bulut yedekleme anlık görüntüsü alınır. StorSimple birimleri sanal aygıtta başarısız oldu, Azure'da bir yineleme VM'si getirilir ve birimler VM'ye eklenir.

#### <a name="to-perform-a-planned-failover"></a>Planlı bir başarısızlımı gerçekleştirmek için
1. Azure portalında, dosya sunucusu VM için oluşturulan kurtarma **hizmetleri** &gt; **kasakurtarma planlarını (Site Kurtarma)** &gt; **recoveryplan_name** seçin.
1. Kurtarma planı bıçak, Tıklayın **Daha** &gt; **Planlı failover**.  

   ![Kurtarma planı](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
1. Planlı **Failover'ı Onayla** bıçağında, kaynak ve hedef konumları seçin ve hedef ağı seçin ve başarısız süreci başlatmak için ✓ denetim simgesine tıklayın.
1. Çoğaltma sanal makineler oluşturulduktan sonra, bekleyen bir durumda olurlar. Başarısız olmak için **Commit'i** tıklatın.
1. Çoğaltma tamamlandıktan sonra, sanal makineler ikincil konumda başlar.

## <a name="perform-a-failover"></a>Bir failover gerçekleştirin
Planlanmamış bir hata sırasında, StorSimple birimleri sanal aygıtta başarısız olur, Azure'da bir yineleme VM'si açılır ve birimler VM'ye eklenir.

#### <a name="to-perform-a-failover"></a>Bir failover gerçekleştirmek için
1. Azure portalında, dosya sunucusu VM için oluşturulan kurtarma **hizmetleri** &gt; **kasakurtarma planlarını (Site Kurtarma)** &gt; **recoveryplan_name** seçin.
1. Kurtarma planı bıçak, Tıklayın **Daha Failover** &gt; **Failover**.  
1. **Failover'ı Onayla** bıçağında kaynak ve hedef konumlarını seçin.
1. **Sanal makineleri kapat'ı** seçin ve Site Kurtarma'nın korunan sanal makineyi kapatmaya çalışması ve verilerin en son sürümünün üzerinde başarısız olması için verileri eşitlemesi gerektiğini belirtmek için en son verileri eşitle.
1. Başarısız olduktan sonra, sanal makineler bekleyen bir durumdadır. Başarısız olmak için **Commit'i** tıklatın.


## <a name="perform-a-failback"></a>Yeniden çalışma gerçekleştirme
Bir geri dönüş sırasında, StorSimple birim kapları yedekleme alındıktan sonra fiziksel aygıta geri başarısız oldu.

#### <a name="to-perform-a-failback"></a>Bir geri dönüş gerçekleştirmek için
1. Azure portalında, dosya sunucusu VM için oluşturulan kurtarma **hizmetleri** &gt; **kasakurtarma planlarını (Site Kurtarma)** &gt; **recoveryplan_name** seçin.
1. Kurtarma planı bıçak, Tıklayın **Daha** &gt; **Planlı Failover**.  
1. Kaynak ve hedef konumları seçin, uygun Veri eşitleme ve VM oluşturma seçeneklerini seçin.
1. Failback işlemini başlatmak için **Tamam** düğmesini tıklatın.
   
   ![Failback'i başlat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>En İyi Uygulamalar
### <a name="capacity-planning-and-readiness-assessment"></a>Kapasite planlama ve hazırlık değerlendirmesi
#### <a name="hyper-v-site"></a>Hyper-V sitesi
Hyper-V çoğaltma ortamınız için sunucu, depolama ve ağ altyapısını tasarlamak için [Kullanıcı Kapasitesi planlayıcısı aracını](https://www.microsoft.com/download/details.aspx?id=39057) kullanın.

#### <a name="azure"></a>Azure
Azure Sanal Sanal M'ler ve Azure Site Kurtarma Hizmetleri ile uyumlu olduğundan emin olmak için Sanal Araçlar'da [Azure Sanal Makine Hazırlık Değerlendirme aracını](https://azure.microsoft.com/downloads/vm-readiness-assessment/) çalıştırabilirsiniz. Hazır Değerlendirme Aracı VM yapılandırmalarını denetler ve yapılandırmalar Azure ile uyumsuz olduğunda uyarır. Örneğin, Bir C: sürücü 127 GB'dan büyükse bir uyarı yayınlar.

Kapasite planlaması en az iki önemli süreçten oluşur:

   - Şirket içi Hyper-V VM'leri Azure VM boyutlarıyla (A6, A7, A8 ve A9 gibi) eşleme.
   - Gerekli Internet bant genişliğini belirleme.

## <a name="limitations"></a>Sınırlamalar
- Şu anda, yalnızca 1 StorSimple cihazı (tek bir StorSimple Cloud Appliance) üzerinden başarısız olabilir. Birkaç StorSimple aygıtına yayılan bir dosya sunucusunun senaryosu henüz desteklenmemektedir.
- VM için koruma sağlarken bir hata alırsanız, iSCSI hedeflerinin bağlantısını kestiğinden emin olun.
- Birim kapsayıcılara yayılan yedekleme ilkeleri nedeniyle birlikte gruplanan tüm birim kapsayıcıları birlikte başarısız olur.
- Seçtiğiniz birim kapsayıcılarında tüm birimler üzerinde başarısız olacaktır.
- Tek bir StorSimple Cloud Appliance'ın maksimum kapasitesi 64 TB olduğundan, 64 TB'den fazla olan birimler üzerinde başarısız olamaz.
- Planlanan/planlanmamış başarısız olursa ve VM'ler Azure'da oluşturulursa, VM'leri temizlemeyin. Bunun yerine, bir geri dönüş yapın. VM'leri silerseniz, şirket içi VM'ler yeniden açılamaz.
- Bir başarısızlıktan sonra, birimleri göremiyorsanız, VM'lere gidin, Disk Yönetimi'ni açın, diskleri taranan ve çevrimiçi duruma getirin.
- Bazı durumlarda, DR sitesindeki sürücü harfleri şirket içi harflerden farklı olabilir. Bu durumda, failover tamamlandıktan sonra sorunu el ile düzeltmeniz gerekir.
- Başarısız iş zaman ayarı: Birim kapsayıcılarının başarısızlığı komut dosyası başına Azure Site Kurtarma sınırından (şu anda 120 dakika) daha fazla zaman alıyorsa, StorSimple komut dosyası zaman ları sona erecektir.
- Yedekleme iş zaman ları: Birimlerin yedeklemesi komut dosyası başına Azure Site Kurtarma sınırından (şu anda 120 dakika) daha fazla zaman alıyorsa, StorSimple komut dosyası devre dışı kalır.
   
  > [!IMPORTANT]
  > Yedeklemeyi Azure portalından el ile çalıştırın ve kurtarma planını yeniden çalıştırın.
   
- İş zaman larını klonlama: Ciltlerin klonlanması, komut dosyası başına Azure Site Kurtarma sınırından (şu anda 120 dakika) daha fazla zaman alıyorsa, StorSimple komut dosyası devre dışı kalır.
- Zaman eşitleme hatası: StorSimple komut dosyaları, yedekleme portalında başarılı olsa bile yedeklemelerin başarısız olduğunu söyleyerek hata yapar. Bunun olası bir nedeni, StorSimple cihazının saat insaat dilimindeki geçerli saatle eşitlenmemiş olması olabilir.
   
  > [!IMPORTANT]
  > Cihaz saatini saat dilimindeki geçerli saatle senkronize edin.
   
- Cihaz arıza hatası: Kurtarma planı çalışırken bir cihaz arızası olduğunda StorSimple komut dosyası başarısız olabilir.
   
  > [!IMPORTANT]
  > Cihaz arızası tamamlandıktan sonra geri kazanım planını yeniden çalıştırın.


## <a name="summary"></a>Özet
Azure Site Kurtarma'yı kullanarak, StorSimple depolama alanında dosya paylaşımları barındırılan bir dosya sunucusu VM için tam bir otomatik durum kurtarma planı oluşturabilirsiniz. Bir aksaklık durumunda herhangi bir yerden saniyeler içinde arıza işlemini başlatabilir ve uygulamayı birkaç dakika içinde çalışır hale getirebilirsiniz.
