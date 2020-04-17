---
title: VMware VM'leri aracısız Azure Geçiş Sunucusu Geçişi'ni geçirin
description: Azure Geçişi ile VMware VM'lerin aracısız geçişini nasıl çalıştırılacak yapılacağını öğrenin.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 4612c9b0ea2ef8d53b0c04f47628f3789705d833
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535324"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>VMware VM'leri Azure'a geçir (aracısız)

Bu makalede, Azure Geçiş Sunucusu Geçişi aracıyla aracısız geçiş kullanarak şirket içi VMware VM'leri Azure'a nasıl geçirilebilirsiniz.

[Azure Geçiş,](migrate-services-overview.md) şirket içi uygulamalarınızın ve iş yüklerinizin ve AWS/GCP VM örneklerinin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Bu öğretici, Azure Geçiş Sunucusu Değerlendirmesi ve Geçişi kullanarak VMware VM'lerinin Azure'a nasıl değerlendirilip Azure'a geçirilenleri gösteren bir serinin üçüncüsüdür. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM'leri geçiş için hazırlayın.
> * Azure Geçiş Sunucusu Geçişi aracını ekleyin.
> * Geçirmek istediğiniz VM'leri keşfedin.
> * VM'leri çoğaltmaya başlayın.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Tam bir VM geçişi çalıştırın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.

## <a name="migration-methods"></a>Geçiş yöntemleri

Azure Geçir Sunucu Geçişi aracını kullanarak VMware VM'leri Azure'a geçirebilirsiniz. Bu araç VMware VM geçişi için birkaç seçenek sunar:

- Aracısız çoğaltma kullanarak geçiş. Üzerlerine bir şey yüklemenize gerek kalmadan VM'leri geçirin.
- Çoğaltma için bir aracı ile geçiş. Çoğaltma için VM'ye bir aracı yükleyin.

Aracısız veya aracı tabanlı geçiş kullanmak isteyip istemediğinize karar vermek için şu makaleleri inceleyin:

- Aracısız geçişin nasıl çalıştığını [öğrenin](server-migrate-overview.md) ve [geçiş yöntemlerini karşılaştırın.](server-migrate-overview.md#compare-migration-methods)
- Aracı tabanlı yöntemi kullanmak istiyorsanız [bu makaleyi okuyun.](tutorial-migrate-vmware-agent.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. Azure ve VMware'i geçiş için ayarlamak için bu [serinin ilk eğitimini tamamlayın.](tutorial-prepare-vmware.md) Özellikle, bu öğretici de gereken:
    - Azure'u geçiş için [hazırlayın.](tutorial-prepare-vmware.md#prepare-azure)
    - Şirket içi ortamı göçe [hazırlayın.](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration)
    
2. Azure'a geçirmeden önce VMware VM'leri Azure'a Geçire Biliyor'la değerlendirmeyi denemenizi öneririz. Değerlendirme yi ayarlamak için, bu [serinin ikinci eğitimini tamamlayın.](tutorial-assess-vmware.md) VM'leri değerlendirmek istemiyorsanız, bu öğreticiyi atlayabilirsiniz. Her ne kadar bir değerlendirme denemenizi öneririz, ancak bir geçiş denemeden önce bir değerlendirme çalıştırmak zorunda değilsiniz.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure Geçir Sunucusu Geçişi aracını ekleme

Azure Geçiş:Sunucu Geçişi aracını ekleyin.

- [VMware VM](/tutorial-assess-vmware.md)değerlendirmek için ikinci öğretici izlediyseniz, devam edin ve aracı ekleyebilirsiniz.
- İkinci öğreticiye uymadıysanız, bir Azure Geçiş projesi ayarlamak için [bu yönergeleri izleyin.](how-to-add-tool-first-time.md)  Projeyi oluştururken Azure Geçiş:Sunucu Geçişi aracını eklersiniz.

Bir proje ayarladıysanız, aracı aşağıdaki gibi ekleyin:

1. Azure Geçir projesinde **Genel Bakış'ı**tıklatın. 
2. **Sunucuları Keşfet, değerlendir ve geçiş te,** **sunucuları değerlendir'i ve geçişini**tıklatın.

     ![Sunucuları değerlendirme ve geçirme](./media/tutorial-migrate-vmware/assess-migrate.png)

3. **Geçiş araçlarında**, **geçiş yapmaya hazır olduğunuzda bir geçiş aracı eklemek için burayı tıklatın'ı**seçin.

    ![Bir araç seçin](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. Araçlar listesinde Azure **Geçiş: Sunucu Geçişi** > **Ekle aracını** seçin

    ![Sunucu Geçişi aracı](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Azure Geçiş cihazını ayarlama

Azure Geçir Sunucusu Geçişi, hafif bir VMware VM cihazı çalıştırın. Cihaz VM keşfi gerçekleştirir ve VM meta verilerini ve performans verilerini Azure Geçiş:Sunucu Geçişi'ne gönderir. Aynı cihaz, VMware VM'lerin aracısız geçişini gerçekleştirmek için Azure Geçir:Sunucu Değerlendirmesi aracı tarafından da kullanılır.

- [VMware VM'leri değerlendirmek için öğreticiyi](tutorial-assess-vmware.md)izlediyseniz, bu eğitim sırasında cihazı zaten kurmuşsunuzdur.
- Bu öğreticiyi izlemediyseniz, şu yöntemlerden birini kullanarak cihazı şimdi ayarlayabilirsiniz:
    - İndirilen BIR OVA şablonu kullanarak VMware VM'de [ayarlayın.](how-to-set-up-appliance-vmware.md)
    - PowerShell yükleyici komut dosyasına sahip bir VMware VM veya fiziksel makineüzerinde ayarlayın. [Bu yöntem,](deploy-appliance-script.md) BIR OVA şablonu kullanarak bir VM ayarlayamıyorsanız veya Azure'da ysanız kullanılmalıdır.

Cihazı oluşturduktan sonra, Azure Geçiş:Sunucu Değerlendirmesi'ne bağlanıp bağlanabildiğinizi, ilk kez yapılandırıp yapılandırıp kaydedilemediğini kontrol edin ve Azure Geçiş projesine kaydettirebilirsiniz.


## <a name="prepare-vms-for-migration"></a>VM'leri geçiş için hazırlama

Azure Geçiş, VM'lerin Azure'a geçirilebilmesini sağlamak için bazı VM değişiklikleri gerektirir.

- Bazı işletim sistemlerinde Azure Geçiş bu değişiklikleri otomatik olarak yapar. [Daha fazlasını öğrenin](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms)
- Bu işletim sistemlerinden birine sahip olmayan bir VM'yi geçirtiyorsanız, VM'yi hazırlamak için yönergeleri izleyin.
- Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM'yi geçiş yaparsanız, VM Azure'da önyükleme yapmayabilir.
- Şirket içi VM'lerde yaptığınız yapılandırma değişiklikleri, VM için çoğaltma etkinleştirildikten sonra Azure'da çoğaltılır. Değişikliklerin çoğaltıldığından emin olmak için, geçiş yaptığınız kurtarma noktasının yapılandırma değişikliklerinin şirket içinde yapıldığı tarihten daha geç olduğundan emin olun.


### <a name="prepare-windows-server-vms"></a>Windows Server VM'leri Hazırlama

**Eylem** | **Şey** | **Yönergeler**
--- | --- | ---
Azure VM'deki Windows birimlerinin şirket içi VM ile aynı sürücü harf atamalarını kullandığından emin olun. | SAN ilkesini Çevrimiçi Tümü olarak yapılandırın. | 1. Yönetici hesabıyla VM'de oturum açın ve bir komut penceresi açın.<br/> 2. **Diskpart** yardımcı programını çalıştırmak için disk parçası yazın.<br/> 3. Tip **SAN POLİtİkASI=OnlineTümü**<br/> 4. Diskpart'ten çıkmak için Çıkış yazın ve komut istemini kapatın.
Azure VM için Azure seri erişim konsoluna olanak sağlama | Bu, sorun giderme ile yardımcı olur. VM'yi yeniden başlatmanız gerekmez. Azure VM disk görüntüsünü kullanarak önyükleme yapacak ve bu yeni VM için yeniden başlatmaya eşdeğerdir. | Etkinleştirmek için [bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console) izleyin.
Hyper-V Konuk Tümleştirmesi Yükle | Windows Server 2003 çalıştıran makineleri geçirin, Hyper-V Guest Integration Services'ı VM işletim sistemine yükleyin. | [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).
Uzak Masaüstü | VM'de Uzak Masaüstü'nü etkinleştirin ve Windows Güvenlik Duvarı'nın herhangi bir ağ profilinde Uzak Masaüstü erişimini engellemediğini denetleyin. | [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).

### <a name="prepare-linux-vms"></a>Linux VM'leri Hazırlayın

**Eylem** | **Şey** 
--- | --- | ---
Hyper-V Linux Entegrasyon Hizmetleri Yükleyin | Linux dağıtımlarının çoğu yeni sürümü varsayılan olarak buna dahildir.
Gerekli Hyper-V sürücülerini içerecek şekilde Linux init görüntüsünü yeniden oluşturma | Bu, VM'nin Azure'da önyükleme olmasını sağlar ve yalnızca bazı dağıtımlarda gereklidir.
Azure seri konsol günlüğe kaydetmeyi etkinleştirme | Bu, sorun giderme ile yardımcı olur. VM'yi yeniden başlatmanız gerekmez. Azure VM disk görüntüsünü kullanarak önyükleme yapacak ve bu yeni VM için yeniden başlatmaya eşdeğerdir.<br/> Etkinleştirmek için [bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) izleyin.
Aygıt haritası dosyalarını güncelleştir | Kalıcı aygıt tanımlayıcıları kullanmak için aygıt adını ses lendirmelerine sahip aygıt eşlemi dosyasını güncelleştirme
fstab girişlerini güncelleştirme | Kalıcı birim tanımlayıcıları kullanmak için girişleri güncelleştirin.
Udev kuralını kaldırma | Mac adresivb dayalı arayüz adları rezervleri herhangi bir udev kuralları kaldırın.
Ağ arabirimlerini güncelleştirme | DHCP'ye dayalı IP adresi almak için ağ arabirimlerini güncelleştirin.
ssh etkinleştirme | SSH'nin etkin olduğundan ve sshd hizmetinin yeniden başlatılmaya otomatik olarak başlatılmak üzere ayarlandığından emin olun.<br/> Gelen ssh bağlantı isteklerinin işletim sistemi güvenlik duvarı veya komut dosyası kuralı tarafından engellenmediğinden emin olun.

Azure'da bir Linux VM çalıştırMak için bu adımları anlatan ve popüler Linux dağıtımlarından bazılarına yönelik yönergeleri içeren [bu makaleyi izleyin.](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)  


## <a name="replicate-vms"></a>Sanal makineleri çoğaltma

Bulma işlemi tamamlandığında VMware VM’lerini Azure’a çoğaltabilirsiniz. 

> [!NOTE]
> En fazla 10 makineyi birlikte çoğaltabilirsiniz. Daha fazla çoğaltmanız gerekiyorsa, bunları aynı anda 10'luk gruplar halinde çoğaltın. Aracısız geçiş için 100 eşzamanlı çoğaltma kadar çalıştırabilirsiniz.

1. Azure Geçir projesinde **> Sunucular,** **Azure Geçir: Sunucu Geçişi**, **Çoğaltma'yı**tıklatın.

    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-vmware/select-replicate.png)

2. **Çoğalt**, > **Kaynak ayarları** > **Makineleriniz sanallaştırıldı mı?** bölümünde **Evet, VMware vSphere** ile seçeneğini belirleyin.
3. **Şirket içi aletler** bölümünde, ayarladığınız Azure Geçişi aletinin adını seçip **Tamam** seçeneğini belirleyin. 

    ![Kaynak ayarları](./media/tutorial-migrate-vmware/source-settings.png)

    - Bu adım, öğreticiyi tamamladığınızda zaten bir cihaz ayarladığınızı varsalar.
    - Bir cihaz ayarlamadıysanız, [bu makaledeki](how-to-set-up-appliance-vmware.md)talimatları uygulayın.

4. **Sanal makineler** bölümünde çoğaltmak istediğiniz makineleri seçin.
    - VM’ler için bir değerlendirme çalıştırırsanız değerlendirme sonuçlarından VM boyutlandırma ve disk türü (premium/standart) önerilerini uygulayabilirsiniz. Bunu yapmak için **Azure Geçişi değerlendirmesinden geçiş ayarları içe aktarılsın mı?** bölümünde **Evet**’i seçin.
    - Bir değerlendirme çalıştırmadıysanız veya değerlendirme ayarlarını kullanmak istemiyorsanız **Hayır**’ı seçin.
    - Değerlendirmeyi kullanmayı seçtiyseniz VM grubunu ve değerlendirme adını belirleyin.

    ![Değerlendirme seçme](./media/tutorial-migrate-vmware/select-assessment.png)

5. VM’leri **Sanal makineler** bölümünde gerektiği şekilde arayın ve geçirmek istediğiniz her bir VM’yi seçin. Sonra **İleri'yi tıklatın: Hedef ayarları.**

    ![VM'leri seçin](./media/tutorial-migrate-vmware/select-vms.png)

6. **Hedef ayarları**’nda aboneliği ve geçiş yapacağınız hedef bölgeyi seçin. Daha sonra Azure VM’lerinin geçişten sonra bulunacağı kaynak grubunu belirtin. **Sanal Ağ**’da Azure VM’lerinin geçişten sonra katılacağı Azure sanal ağını/alt ağını seçin.
7. **Azure Hibrit Avantajı**’nda:

    - Azure Hibrit Avantajı’nı uygulamak istemiyorsanız **Hayır**’ı seçin. Ardından **İleri**’ye tıklayın.
    - Etkin Yazılım Güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve avantajı geçirdiğiniz makinelere uygulamak istiyorsanız **Evet**’i seçin. Ardından **İleri**’ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-vmware/target-settings.png)

8. **İşlem** bölümünde VM adını, boyutunu, İşletim Sistemi disk türünü ve kullanılabilirlik kümesini gözden geçirin. VM’ler [Azure gereksinimleriyle](migrate-support-matrix-vmware-migration.md#azure-vm-requirements)uyumlu olmalıdır.

    - **VM boyutu**: Değerlendirme önerileri kullanıyorsanız, VM boyutu açılır bırakma önerilen boyutu içerir. Aksi takdirde Azure Geçişi, Azure aboneliğindeki en yakın eşleşmeye göre bir boyut seçer. Alternatif olarak **Azure VM boyutu **’nda el ile bir boyut seçin. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. 
    - **Kullanılabilirlik kümesi**: VM geçişten sonra bir Azure kullanılabilirlik kümesinde olması gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![VM bilgi işlem ayarları](./media/tutorial-migrate-vmware/compute-settings.png)

9. **Diskler** bölümünde, VM disklerinin Azure’a çoğaltılıp çoğaltılmayacağını belirtin ve Azure’da disk türünü (Standart SSD/HDD veya premium yönetilen diskler) seçin. Ardından **İleri**’ye tıklayın.
    - Diskleri çoğaltmadan çıkarabilirsiniz.
    - Diskleri çıkarırsanız bu diskler geçişten sonra Azure VM’de bulunmaz. 

    ![Diskler](./media/tutorial-migrate-vmware/disks.png)

10. **Çoğaltmayı gözden geçir ve başlat** bölümünde ayarları gözden geçirin ve sunuculara yönelik ilk çoğaltmayı başlatmak için **Çoğalt** üzerine tıklayın.

> [!NOTE]
> Çoğaltma başlamadan önce çoğaltma ayarlarını çoğaltma ayarlarını çoğaltma**yı** **yönet** > makinelerde istediğiniz zaman güncelleştirebilirsiniz. Çoğaltma başladıktan sonra ayarlar değiştirilemez.

### <a name="provisioning-for-the-first-time"></a>İlk kez sağlanması

Azure Geçir projesinde çoğaltmakta olduğunuz ilk VM buysa, Azure Geçir Sunucusu Geçişi bu kaynakları projeyle aynı kaynak grubunda otomatik olarak karşılar.

- **Servis veri kurumu**: Azure Geçir Sunucusu Geçişi, cihaza çoğaltma düzenleme iletileri göndermek için servis veri veri tobunu kullanır.
- **Ağ geçidi depolama hesabı**: Sunucu Geçişi, çoğaltılan VM'ler hakkındaki durum bilgilerini depolamak için ağ geçidi depolama hesabını kullanır.
- **Günlük depolama hesabı**: Azure Geçir cihazı, VM'ler için çoğaltma günlüklerini bir günlük depolama hesabına yükler. Azure Geçir, çoğaltma bilgilerini çoğaltma yönetilen disklere uygular.
- **Anahtar kasası**: Azure Geçir cihazı, servis veri aracının bağlantı dizelerini yönetmek için anahtar kasasını ve çoğaltmada kullanılan depolama hesaplarının erişim anahtarlarını kullanır. Hazırlanırken anahtar kasasının depolama hesabına erişmesi için gereken izinleri ayarlamanız gerekirdi. [Bu izinleri gözden geçirin.](tutorial-prepare-vmware.md#assign-permissions-to-create-a-key-vault)   


## <a name="track-and-monitor"></a>İzleme ve izleme


- Bir Başlat Çoğaltma işi **çoğaltma'yı** tıklattığınızda iş başlar. 
- Çoğaltmayı Başlat işi başarıyla bittiğinde, makineler ilk çoğaltmalarını Azure'a başlar.
- İlk çoğaltma sırasında bir VM anlık görüntüsü oluşturulur. Anlık görüntüdeki disk verileri Azure'da yönetilen disklerin çoğaltılması için çoğaltılır.
- İlk çoğaltma bittikten sonra, delta çoğaltma başlar. Şirket içi disklerde yapılan artımlı değişiklikler, Azure'daki yineleme disklerine düzenli olarak çoğaltılır.

Portal bildirimlerinde iş durumunu izleyebilirsiniz.

**Azure Geçişi: Sunucu Geçişi'nde** **çoğaltma sunucularını** tıklatarak çoğaltma durumunu izleyebilirsiniz.
![Çoğaltmayı izleme](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Delta çoğaltma başladığında, Azure'a tam bir geçiş çalıştırmadan önce VM'ler için bir test geçişi çalıştırabilirsiniz. Bunu her makine için en az bir kez, geçirmeden önce yapmanızı öneririz.

- Bir test geçişi, çalışmaya devam eden ve çoğaltmaya devam eden şirket içi makineleri etkilemeden, geçişin beklendiği gibi çalışacağını denetler. 
- Test geçişi, çoğaltılan verileri kullanarak (genellikle Azure aboneliğinizde üretim dışı bir VNet'e geçiş yaparak) bir Azure VM oluşturarak geçişi simüle eder.
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce tüm sorunları gidermek için çoğaltılan Azure VM testini kullanabilirsiniz.

Bir test geçişini aşağıdaki gibi yapın:


1. **Geçiş hedefleri** > **Sunucularında** > Azure**Geçir: Sunucu Geçişi**, **geçirilen sunucuları sına'yı**tıklatın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Test edilecek VM’yi sağ tıklayıp **Test geçişi** üzerine tıklayın.

    ![Test geçişi](./media/tutorial-migrate-vmware/test-migrate.png)

3. **Test Geçişi** bölümünde, Azure VM’nin geçişten sonra bulunacağı Azure VNet’i seçin. Üretim dışı bir VNet kullanmanızı öneririz.
4. **Test Geçişi** işlemi başlar. Portal bildirimlerinde işi izleyin.
5. Geçiş bittikten sonra geçirilen Azure VM’yi, Azure portalında **Sanal Makineler** bölümünde görüntüleyin. Makine adında **Testi** şeklinde bir son ek vardır.
6. Test tamamlandıktan sonra **Makineleri çoğaltma** bölümünde Azure VM’yi sağ tıklayıp **Test geçişini temizle** üzerine tıklayın.

    ![Geçişi temizleme](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçişinin beklendiği gibi çalıştığını doğruladıktan sonra, şirket içi makineleri geçirebilirsiniz.

1. Azure Geçir projesinde **> Sunucular** > **Azure Geçir: Sunucu Geçişi**, **sunucuları çoğaltma'yı**tıklatın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-vmware/replicate-servers.png)

2. **Makineleri çoğaltma** bölümünde VM > **Geçir** üzerine sağ tıklayın.
3. **Geçiret** > **Sanal makineleri kapatın ve veri kaybı olmadan planlı geçiş gerçekleştirin**, **Evet** > **Tamam'ı**seçin.
    - Azure Geçişi, varsayılan olarak şirket içi VM’yi kapatır ve son çoğaltmadan bu yana gerçekleşen tüm VM değişikliklerini eşitlemek için bir isteğe bağlı çoğaltma çalıştırır. Bu, veri kaybı olmamasını sağlar.
    - VM’yi kapatmak istemiyorsanız, **Hayır** seçeneğini belirleyin
4. VM için bir geçiş işlemi başlar. Azure bildirimlerinde işlemi izleyin.
5. İşlem bittikten sonra **Sanal Makineler** sayfasında VM’yi görüntüleyebilir ve yönetebilirsiniz.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yapıldıktan sonra VM > **Stop Replication'a**sağ tıklayın. Bu, şirket içi makinenin çoğaltmasını durdurur ve VM için çoğaltma durumu bilgilerini temizler.
2. Azure VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) aracısını geçirilen makinelere yükleyin.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine gelen trafiği azledin.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi ile gelen trafik erişimini kilitleme ve sınırlama [- Tam zamanında yönetim.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
-  [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Bulut Benimseme Çerçevesi'ndeki [bulut geçişi yolculuğunu](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) incele.
