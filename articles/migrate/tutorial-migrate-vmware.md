---
title: Aracısız Azure geçişi sunucu geçişine sahip şirket içi VMware VM 'lerini Azure 'a geçirme | Microsoft Docs
description: Azure geçişi kullanılarak şirket içi VMware VM 'lerinin Azure 'a nasıl gerçekleştirileceğini ve aracısız geçirilmesini açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 885c877f219f59ab5049cf7b8e01243077d6d3eb
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348392"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>VMware VM 'lerini Azure 'a geçirme (aracısız)

Bu makalede, Azure geçişi sunucu geçiş aracı ile aracısız geçiş kullanarak şirket içi VMware VM 'lerini Azure 'a nasıl geçireceğiniz gösterilmektedir.

[Azure geçişi](migrate-services-overview.md) , şirket içi uygulamalarınızı ve iş yüklerinizi ve AWS/GCP sanal makine örneklerini Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için bir merkezi Merkez sağlar. Hub, değerlendirme ve geçiş için Azure geçiş araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Bu öğretici, Azure geçiş sunucusu değerlendirmesini ve geçişini kullanarak VMware VM 'lerini Azure 'a nasıl değerlendirileceğini ve geçireceğinizi gösteren bir serinin üçüncü bir dizisidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM 'Leri geçiş için hazırlayın.
> * Azure geçiş sunucusu geçiş aracı 'nı ekleyin.
> * Geçirmek istediğiniz VM 'Leri bulun.
> * VM 'Leri çoğaltmaya başlayın.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Tam VM geçişini çalıştırın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="migration-methods"></a>Geçiş yöntemleri

Azure geçişi sunucu geçiş aracını kullanarak VMware VM 'lerini Azure 'a geçirebilirsiniz. Bu araç, VMware VM geçişi için birkaç seçenek sunar:

- Aracısız çoğaltma kullanılarak geçiş. VM 'Leri bunlara hiçbir şey yüklemeye gerek kalmadan geçirin.
- Çoğaltma için bir aracı ile geçiş. Çoğaltma için VM 'ye bir aracı yükler.

Aracısız veya aracı tabanlı geçiş kullanmak isteyip istemediğinize karar vermek için şu makaleleri gözden geçirin:

- Aracısız geçişin [nasıl çalıştığını öğrenin](server-migrate-overview.md) ve [sınırlamaları gözden geçirin](server-migrate-overview.md#agentless-migration-limitations).
- Aracı tabanlı yöntemi kullanmak istiyorsanız [Bu makaleyi okuyun](tutorial-migrate-vmware-agent.md) .

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. VMware geçiş mimarisini [anlayın](migrate-architecture.md) .
2. Geçiş için Azure ve VMware 'yi ayarlamak üzere bu serideki [ilk öğreticiyi doldurun](tutorial-prepare-vmware.md) . Özellikle, bu öğreticide şunları yapmanız gerekir:
    - [Azure](tutorial-prepare-vmware.md#prepare-azure) 'u geçiş için hazırlayın.
    - [Şirket içi ortamı](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration) geçiş için hazırlayın.
    
3. VMware VM 'lerini Azure 'a geçirmeden önce Azure geçişi sunucu değerlendirmesi ile değerlendirmeyi denemeniz önerilir. Değerlendirmeyi ayarlamak için bu serideki [ikinci öğreticiyi doldurun](tutorial-assess-vmware.md) . VM 'Leri değerlendirmek istemiyorsanız bu öğreticiyi atlayabilirsiniz. Bir değerlendirme denemeniz önerilir, ancak geçiş yapmayı denemeden önce bir değerlendirme çalıştırmanız gerekmez.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure geçiş sunucusu geçiş aracını ekleme

VMware VM 'lerini değerlendirmek için ikinci öğreticiyi izmediyseniz, bir Azure geçişi projesi ayarlama ve Azure geçişi sunucusu geçiş aracını seçme [yönergelerini izlemeniz](how-to-add-tool-first-time.md) gerekir. 

İkinci öğreticiyi takip ediyorsanız ve zaten bir Azure geçişi projesi ayarlandıysa, Azure geçiş sunucusu geçiş aracını şu şekilde ekleyin:

1. Azure geçişi projesinde **Genel Bakış ' a**tıklayın. 
2. **Bul, değerlendir ve geçiş sunucuları**' nda, **sunucuları değerlendir ve geçir**' e tıklayın.

     ![Sunucuları değerlendirin ve geçirin](./media/tutorial-migrate-vmware/assess-migrate.png)

3. **Geçiş araçları**' nda, geçişe **hazır olduğunuzda bir geçiş aracı eklemek Için buraya tıklayın ' ı**seçin.

    ![Araç seçin](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. Araçlar listesinde Azure geçişi ' ni **seçin: Sunucu geçişi** > **ekleme aracı**

    ![Sunucu geçiş aracı](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Azure geçişi gereç 'yi ayarlama

Azure geçişi sunucu geçişi, hafif bir VMware VM gereci çalıştırır. Gereç, VM bulma işlemini gerçekleştirir ve VM meta verilerini ve performans verilerini Azure geçişi sunucu geçişine iletir. Aynı gereç, Azure geçişi sunucu değerlendirmesi aracı tarafından da kullanılır.

VMware VM 'lerini değerlendirmek için ikinci öğreticiyi izlediyseniz, bu öğreticide zaten gereci ayarlamış olursunuz. Bu öğreticiyi izlemeden, gereci şimdi ayarlamanız gerekir. Bunu yapmak için şunları yapın: 

- Bir OVA şablon dosyasını indirip vCenter Server içe aktarın.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun. 
- Gereci ilk kez yapılandırın ve Azure geçişi projesi ile kaydedin.

Gereci ayarlamak için [Bu makaledeki](how-to-set-up-appliance-vmware.md) yönergeleri izleyin.


## <a name="prepare-vms-for-migration"></a>VM 'Leri geçiş için hazırlama

Azure geçişi, VM 'Lerin Azure 'a geçirilmesini sağlamak için bazı VM değişiklikleri gerektirir.

- Bazı [işletim sistemleri](server-migrate-overview.md#agentless-migration-limitations)Için Azure geçişi bu değişiklikleri otomatik olarak yapar.
- Bu işletim sistemlerinden birine sahip olmayan bir VM 'yi geçiriyorsanız, sanal makineyi hazırlamak için yönergeleri izleyin.
- Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir.
- VM için çoğaltma etkinleştirildikten sonra şirket içi VM 'lerde yaptığınız yapılandırma değişiklikleri Azure 'a çoğaltılır. Değişikliklerin çoğaltılmasını sağlamak için, geçiş yaptığınız kurtarma noktasının şirket içinde yapılandırma değişikliklerinin gerçekleştirildiği zamandan daha geç olduğundan emin olun.


### <a name="prepare-windows-server-vms"></a>Windows Server VM 'lerini hazırlama

**Eylem** | **Ayrıntılar** | **Yönergelerin**
--- | --- | ---
Azure VM 'deki Windows birimlerinin, şirket içi VM ile aynı sürücü harfi atamalarını kullandığından emin olun. | SAN ilkesini çevrimiçi olarak yapılandırın. | 1. VM 'de bir yönetici hesabıyla oturum açın ve bir komut penceresi açın.<br/> 2. DiskPart yardımcı programını çalıştırmak için **DiskPart** yazın.<br/> 3. **San ilkesi = OnlineAll** yazın<br/> 4. DiskPart 'Tan çıkmak için exit yazın ve komut istemi ' ni kapatın.
Azure VM için Azure seri erişim konsolunu etkinleştirme | Bu sorun gidermeye yardımcı olur. VM 'yi yeniden başlatmanız gerekmez. Azure VM, disk görüntüsü kullanılarak önyüklenir ve bu, yeni VM için bir yeniden başlatmaya eşdeğerdir. | ' İ etkinleştirmek için [Bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console) izleyin.
Hyper-V Konuk tümleştirmesi 'ni yükler | Windows Server 2003 çalıştıran makineleri geçiriyorsanız, Hyper-V Konuk Tümleştirme Hizmetleri 'ni VM işletim sistemine yükleyebilirsiniz. | [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).
Uzak Masaüstü | SANAL makinede Uzak Masaüstü 'Nü etkinleştirin ve Windows Güvenlik Duvarı 'nın herhangi bir ağ profili üzerinde uzak masaüstü erişimini engellemediğinden emin olun. | [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).

### <a name="prepare-linux-vms"></a>Linux VM 'lerini hazırlama

**Eylem** | **Ayrıntılar** 
--- | --- | ---
Hyper-V Linux Tümleştirme Hizmetleri 'ni yükler | Linux dağıtımların çoğu yeni sürümü bu varsayılan olarak eklenmiştir.
Linux init görüntüsünü, gerekli Hyper-V sürücülerini içerecek şekilde yeniden oluşturun | Bu, VM 'nin Azure 'da önyüklenmesini sağlar ve yalnızca bazı dağıtımlarda gerekli olur.
Azure seri konsol günlüğünü etkinleştirme | Bu sorun gidermeye yardımcı olur. VM 'yi yeniden başlatmanız gerekmez. Azure VM, disk görüntüsü kullanılarak önyüklenir ve bu, yeni VM için bir yeniden başlatmaya eşdeğerdir.<br/> ' İ etkinleştirmek için [Bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) izleyin.
Cihaz eşleme dosyasını güncelleştir | Kalıcı cihaz tanımlayıcılarını kullanmak için cihaz adı olan cihaz eşleme dosyasını birim ilişkilendirmeleriyle Güncelleştir
Fstab girdilerini Güncelleştir | Kalıcı birim tanımlayıcılarını kullanmak için girişleri güncelleştirin.
Uıdev kuralını kaldır | Arabirim adlarını ayrılmış olan tüm udev kurallarını MAC adresine göre kaldırın vb.
Ağ arabirimlerini Güncelleştir | Ağ arabirimlerini DHCP tabanlı IP adresini alacak şekilde güncelleştirin.
SSH 'yi etkinleştirme | SSH 'nin etkinleştirildiğinden ve SSHD hizmetinin yeniden başlatma sırasında otomatik olarak başlayacak şekilde ayarlandığından emin olun.<br/> Gelen SSH bağlantı isteklerinin işletim sistemi güvenlik duvarı veya komut dosyası kuralları tarafından engellenmediğinden emin olun.

Azure 'da bir Linux sanal makinesi çalıştırmaya yönelik bu adımları açıklayan [Bu makaleyi izleyin](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic) ve popüler Linux dağıtımlarından bazılarına yönelik yönergeleri dahil edin.  


## <a name="replicate-vms"></a>VM 'Leri çoğaltma

Bulma işlemi tamamlandığında, VMware VM 'lerinin Azure 'a çoğaltılmasını başlatabilirsiniz.

1. Azure geçişi Proje > **sunucularında** **Azure geçişi: Sunucu geçişi**, **Çoğalt**' a tıklayın.

    ![VM 'Leri çoğaltma](./media/tutorial-migrate-vmware/select-replicate.png)

2. > **Kaynak ayarları** >  **Çoğalt**,**makineleriniz sanallaştırılmış mı?** , **VMware vSphere ile Evet '** i seçin.
3. **Şirket içi gereç**içinde, > **Tamam**' ı ayarladığınız Azure geçiş gerecinin adını seçin. 

    ![Kaynak ayarları](./media/tutorial-migrate-vmware/source-settings.png)

    - Bu adım, öğreticiyi tamamladığınızda daha önce bir gereç ayarlamış olduğunuz varsayılır.
    - Bir gereç ayarlamadıysanız, [Bu makaledeki](how-to-set-up-appliance-vmware.md)yönergeleri izleyin.

4. **Sanal makineler**' de çoğaltmak istediğiniz makineleri seçin.
    - VM 'Ler için bir değerlendirme çalıştırırsanız, değerlendirme sonuçlarından VM boyutlandırma ve disk türü (Premium/standart) önerilerini uygulayabilirsiniz. Bunu yapmak için, **geçiş ayarlarını bir Azure geçiş değerlendirmesiyle Içeri aktarma**bölümünde **Evet** seçeneğini belirleyin.
    - Bir değerlendirme çalıştırmadıysanız veya değerlendirme ayarlarını kullanmak istemiyorsanız, **Hayır** seçeneğini belirleyin.
    - Değerlendirmeyi kullanmayı seçtiyseniz, VM grubunu ve değerlendirme adını seçin.

    ![Değerlendirme seçin](./media/tutorial-migrate-vmware/select-assessment.png)

5. **Sanal makinelerde**, gerektiğinde VM 'leri arayın ve geçirmek istediğiniz her VM 'yi denetleyin. Ardından İleri **' ye tıklayın: Hedef ayarlar**.

    ![VM 'Leri seçin](./media/tutorial-migrate-vmware/select-vms.png)

6. **Hedef ayarlar**' da, geçirilecek abonelik ve hedef bölge ' yi seçin ve Azure VM 'lerinin geçişten sonra bulunacağı kaynak grubunu belirtin. **Sanal ağ**' da, Azure VM 'lerinin geçişten sonra katılacağını Azure VNET/subnet ' i seçin.
7. **Azure hibrit avantajı**:

    - Azure Hibrit Avantajı uygulamak istemiyorsanız **Hayır** ' ı seçin. Ardından **İleri**'ye tıklayın.
    - Etkin yazılım güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve geçirdiğiniz makinelere avantajı uygulamak istiyorsanız **Evet** ' i seçin. Ardından **İleri**'ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-vmware/target-settings.png)

8. **İşlem**bölümünde VM adı, boyutu, işletim sistemi disk türü ve kullanılabilirlik kümesini gözden geçirin. VM 'Ler [Azure gereksinimleriyle](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements)uyumlu olmalıdır.

    - **VM boyutu**: Değerlendirme önerilerini kullanıyorsanız, VM boyutu açılan listesi önerilen boyutu içerir. Aksi takdirde Azure geçişi, Azure aboneliğindeki en yakın eşleşmeye göre bir boyut seçer. Alternatif olarak, **Azure VM boyutu**' nda el ile bir boyut seçin. 
    - **Işletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim sistemi diski, işletim sistemi önyükleme yükleyicisine ve yükleyiciye sahip disktir. 
    - **Kullanılabilirlik kümesi**: Sanal makinenin geçişten sonra bir Azure kullanılabilirlik kümesinde olması gerekiyorsa, kümeyi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![VM işlem ayarları](./media/tutorial-migrate-vmware/compute-settings.png)

9. **Diskler**' de, VM disklerinin Azure 'da çoğaltılıp çoğaltılmayacağını belirtin ve Azure 'da disk türünü (Standart SSD/HDD veya Premium yönetilen diskler) seçin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmanın dışında bırakabilirsiniz.
    - Diskleri hariç tutdıysanız geçişten sonra Azure VM 'de mevcut olmayacaktır. 

    ![Diskler](./media/tutorial-migrate-vmware/disks.png)

10. **İnceleme ve çoğaltmayı başlatma**bölümünde ayarları gözden geçirin ve sunucular için ilk çoğaltmayı başlatmak üzere **Çoğalt** ' a tıklayın.

> [!NOTE]
> Çoğaltma ayarlarını, çoğaltma başlamadan önce istediğiniz zaman güncelleştirebilirsiniz, bu da çoğaltmayı **yönetme** > **bölümünde.** Çoğaltma başladıktan sonra ayarlar değiştirilemez.

### <a name="provisioning-for-the-first-time"></a>İlk kez sağlama

Azure geçişi projesinde çoğaltmakta olduğunuz ilk sanal makine ise, Azure geçişi sunucu geçişi, bu kaynakları projeyle aynı kaynak grubunda otomatik olarak sağlar.

- **Hizmet veri yolu**: Azure geçişi sunucu geçişi, hizmet veri yolunu kullanarak, yüklemeye çoğaltma düzenleme iletileri gönderir.
- **Ağ geçidi depolama hesabı**: Sunucu geçişi, çoğaltılan VM 'Ler hakkında durum bilgilerini depolamak için ağ geçidi depolama hesabını kullanır.
- **Günlük depolama hesabı**: Azure geçişi gereci, VM 'Ler için çoğaltma günlüklerini bir günlük depolama hesabına yükler. Azure geçişi, çoğaltma bilgilerini çoğaltma tarafından yönetilen disklere uygular.
- **Anahtar Kasası**: Azure geçişi gereci, hizmet veri yolu için bağlantı dizelerini yönetmek için anahtar kasasını ve Çoğaltmada kullanılan depolama hesapları için erişim anahtarlarını kullanır. Hazırlandığınızda, anahtar kasasının depolama hesabına erişmesi için gereken izinleri ayarlamış olmanız gerekir. [Bu Izinleri gözden geçirin](tutorial-prepare-vmware.md#assign-role-assignment-permissions).   


## <a name="track-and-monitor"></a>İzleme ve izleme


- **Çoğalt** ' a tıkladığınızda çoğaltma Başlat işi başlar. 
- Çoğaltma Başlat işi başarıyla tamamlandığında, makineler ilk çoğaltmasını Azure 'a başlatır.
- İlk çoğaltma sırasında VM anlık görüntüsü oluşturulur. Anlık görüntüdeki disk verileri, Azure 'da çoğaltma ile yönetilen disklere çoğaltılır.
- İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Şirket içi disklerde artımlı değişiklikler düzenli aralıklarla Azure 'daki çoğaltma disklerine çoğaltılır.

Portal bildirimlerinde iş durumunu izleyebilirsiniz.

Çoğaltma durumunu, Azure geçişi 'nde  ****sunucuları çoğaltma** ' ya tıklayarak izleyebilirsiniz: Sunucu geçişi**.
![Çoğaltmayı izleme](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Delta çoğaltma başladığında, Azure 'a tam geçiş çalıştırmadan önce VM 'Ler için bir test geçişi çalıştırabilirsiniz. Bunu geçirmeden önce her makine için en az bir kez yapmanızı öneririz.

- Bir test geçişi çalıştırmak, geçiş işleminin beklendiği gibi çalıştığını denetler ve bu işlem, çalışır durumda olan şirket içi makineleri etkilemeden, çoğaltmaya devam eder. 
- Test geçişi, çoğaltılan verileri kullanarak bir Azure VM oluşturarak geçişe benzetir (genellikle Azure aboneliğinizdeki bir üretim dışı VNet 'e geçiş yapar).
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce herhangi bir sorunu gidermek için çoğaltılan test Azure VM 'yi kullanabilirsiniz.

Test geçişini aşağıdaki şekilde yapın:


1. **Geçiş hedefleri** > sunucularındaAzuregeçişi > : **Sunucu geçişi**, **geçirilen sunucuları test et**' e tıklayın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Sınanacak sanal makineye sağ tıklayın ve **test geçişi**' ne tıklayın.

    ![Test geçişi](./media/tutorial-migrate-vmware/test-migrate.png)

3. **Test geçişi**' nde, geçiş SONRASıNDA Azure VM 'Nin bulunduğu Azure VNET ' i seçin. Üretim dışı bir VNet kullanmanızı öneririz.
4. **Test geçiş** işi başlar. Portalı bildirimlerde işi izleyin.
5. Geçiş bittikten sonra, Azure portal **sanal makinelerde** GEÇIRILMIŞ Azure VM 'yi görüntüleyin. Makine adının bir sonek **testi**vardır.
6. Sınama tamamlandıktan sonra, **makineler çoğaltılırken**Azure VM 'ye sağ tıklayın ve **Test geçişini temizle**' ye tıklayın.

    ![Geçişi temizle](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçişinin beklendiği gibi çalışıp çalışmadığını doğruladıktan sonra şirket içi makineleri geçirebilirsiniz.

1. Azure geçişi Proje > **sunucuları** > **Azure geçişi: Sunucu geçişi**, **sunucuları çoğaltma**' ya tıklayın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-vmware/replicate-servers.png)

2. **Makineleri çoğaltmak**IÇIN, sanal makineye sağ tıklayın > **geçirin**.
3. Sanal makineleri Kapat ' a **geçiş** > yapın**ve veri kaybı olmadan planlı bir geçiş gerçekleştirin**, **Evet** > **Tamam**' ı seçin.
    - Varsayılan olarak Azure geçişi, şirket içi sanal makineyi kapatır ve son çoğaltmadan bu yana gerçekleşen tüm VM değişikliklerini eşitleme için isteğe bağlı bir çoğaltma çalıştırır. Bu, veri kaybı olmamasını sağlar.
    - VM 'yi kapatmak istemiyorsanız, **Hayır** ' ı seçin.
4. VM için bir geçiş işi başlatılır. Azure bildirimlerinde işi izleyin.
5. İş bittikten sonra **sanal makineler** sayfasından VM 'yi görüntüleyebilir ve yönetebilirsiniz.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yapıldıktan sonra, **geçişi durdurmak**> VM 'ye sağ tıklayın. Bu, şirket içi makine için çoğaltmayı durduruyor ve VM için çoğaltma durumu bilgilerini temizler.
2. Geçirilen makinelere Azure VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Aracısı 'nı yükler.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine giden trafiği kesin.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi ile gelen trafik erişimini kilitleme ve sınırlayın [-tam zamanında yönetim](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
-  [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Azure bulut benimseme çerçevesindeki [bulut geçiş yolculuğunu](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) araştırın.
