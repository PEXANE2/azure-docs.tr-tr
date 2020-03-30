---
title: Azure Site Kurtarma ile olağanüstü durum kurtarma sırasında bir hata çalıştırma
description: Azure Site Kurtarma ile Azure'a vm'ler/fiziksel sunucular üzerinde nasıl başarısız olunacağı.
ms.service: site-recovery
ms.topic: article
ms.date: 12/10/2019
ms.openlocfilehash: 99a197e8f5ebac8a3b0be1b567ee41b43a2c4476
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471277"
---
# <a name="run-a-failover-from-on-premises-to-azure"></a>Şirket içinde Azure'a bir arıza çalıştırma

Bu makalede, [Azure Site Kurtarma'da](site-recovery-overview.md) şirket içi makinelerden Azure'a nasıl geçilen

## <a name="before-you-start"></a>Başlamadan önce

- Olağanüstü durum kurtarma daki başarısızlık süreci hakkında [bilgi edinin.](failover-failback-overview.md)
- Birden çok makinede başarısız olmak istiyorsanız, kurtarma planında makineleri nasıl bir araya topladığınızı [öğrenin.](recovery-plan-overview.md)
- Tam bir arıza yapmadan önce, her şeyin beklendiği gibi çalıştığından emin olmak için bir [olağanüstü durum kurtarma tatbikatı](site-recovery-test-failover-to-azure.md) çalıştırın.

## <a name="prepare-to-connect-after-failover"></a>Başarısız olduktan sonra bağlanmaya hazırlanın

Başarısız olduktan sonra oluşturulan Azure VM'lerine bağlanabildiğinizden emin olmak için, başarısız olmadan önce şirket içinde yapmanız gereken birkaç şey aşağıda verebilirsiniz.


### <a name="prepare-on-premises-to-connect-after-failover"></a>Başarısız olduktan sonra bağlanmak için şirket içinde hazırlanın

Başarısız olduktan sonra RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız, başarısız olmadan önce şirket içinde yapmanız gereken birkaç şey vardır.

**Başarısız olduktan sonra** | **Konum** | **Eylemler**
--- | --- | ---
**Windows çalıştıran Azure VM** | Arıza dan önce şirket içi makine | Azure VM'ye Internet üzerinden erişmek için RDP'yi etkinleştirin ve **TCP**ve UDP kurallarının Herkese Açık olarak eklenmediğinden ve **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalardaki**tüm profiller için RDP'ye izin verildiğinden emin olun.<br/><br/> Azure VM'ye sitebağlantısı üzerinden erişmek için makinede RDP'yi etkinleştirin ve **Etki Alanı ve Özel** ağlar için Windows Güvenlik **Duvarı** -> **İzin Verilen uygulamalarda ve özelliklerde**RDP'ye izin verildiğini sağlayın.<br/><br/> <br/><br/> Statik kalıcı yolları ve WinHTTP proxy'yi kaldırın. İşletim sistemi SAN ilkesinin **OnlineAll**olarak ayarlandıklarına emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Bir başarısızlığı tetiklediğinde VM'de bekleyen Windows güncelleştirmeleri olmadığından emin olun. Windows güncelleştirmesi, başarısız olduğunuzda başlayabilir ve güncelleştirme tamamlanana kadar VM'de oturum açamazsınız.
**Linux çalıştıran Azure VM** | Arıza dan önce şirket içi makine | VM'deki Secure Shell hizmetinin sistem önyüklemede otomatik olarak başlatılıdığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.


## <a name="run-a-failover"></a>Yük devretme çalıştırma

Bu yordam, bir [kurtarma planı](site-recovery-create-recovery-plans.md)için bir failover çalıştırmak için nasıl açıklanır. Eğer tek bir VM için bir failover çalıştırmak istiyorsanız, bir [VMware VM](vmware-azure-tutorial-failover-failback.md)için yönergeleri izleyin , fiziksel bir [sunucu,](physical-to-azure-failover-failback.md)veya [Hyper-V VM](hyper-v-azure-failover-failback-tutorial.md).


Kurtarma planı başarısızlığını aşağıdaki gibi çalıştırın:

1. Site Kurtarma kasasında, **Kurtarma Planları** > *recoveryplan_name'yi*seçin.
2. **Failover'ı**tıklatın.

    ![Yük devretme](./media/site-recovery-failover/Failover.png)

3. **Failover** > **Failover yönünde,** Azure'a çoğalıyorsanız varsayılanı bırakın.
4. **Failover'da,** başarısız olmak için bir **Kurtarma Noktası** seçin.

    - **Son**: En son noktayı kullanın. Bu, Site Kurtarma hizmetine gönderilen tüm verileri işler ve her makine için bir kurtarma noktası oluşturur. Bu seçenek en düşük RPO'yu (Kurtarma Noktası Hedefi) sağlar, çünkü failover'dan sonra oluşturulan VM, başarısız olduğunda Site Kurtarma'ya çoğaltılan tüm verilere sahiptir.
   - **En son işlenen**: Bu seçeneği, Site Kurtarma tarafından zaten işlenmiş olan en son kurtarma noktasına VM'ler üzerinde başarısız olmak için kullanın. VM **Son Kurtarma Noktaları'nda**en son işlenmiş kurtarma noktasını görebilirsiniz. İşlenmemiş verilerin işlenmesi için zaman harcanması nedeniyle bu seçenek düşük bir RTO sağlar
   - **En son uygulama tutarlılığı**: Site Kurtarma tarafından işlenen en son uygulama tutarlı kurtarma noktasına VM'leri başarısız olmak için bu seçeneği kullanın.
   - **En son çoklu VM işlenmiş**: Bu seçenekle, çoğaltma grubunun bir parçası olan VM'ler ile en son ortak multi-VM tutarlı kurtarma noktasına başarısız olur. Diğer sanal makineler en son işlenmiş kurtarma noktasına kadar başarısız. Bu seçenek yalnızca çoklu VM tutarlılığı etkin leştirilmiş en az bir VM'si olan kurtarma planları içindir.
   - **En son multi-VM uygulama tutarlılığı**: Çoğaltma grubunun bir parçası olan bu seçenekle VM'ler en son yaygın çoklu VM uygulama tutarlı kurtarma noktasına geçemez. Diğer sanal makineler en son uygulama tutarlı kurtarma noktasına başarısız. Yalnızca multi-VM tutarlılığı etkin en az bir VM olan kurtarma planları için.
   - **Özel**: Kurtarma planları için kullanılamaz. Bu seçenek yalnızca tek tek VM'lerin başarısız olması içindir.

5. Site Kurtarma'nın başarısız olmadan önce kaynak VM'leri kapatmasını **istiyorsanız, kapatma makinesini** seçin. Kapatma işlemi başarısız olsa bile yük devretme devam eder.  

    > [!NOTE]
    > Hyper-V VM'ler üzerinde başarısız olursanız, kapatma, başarısız olmayı tetiklemeden önce, henüz servise gönderilmemiş şirket içi verileri eşitlemeye ve çoğaltmaya çalışır. 

6. **İşler** sayfasında başarısız ilerlemeyi izleyin. Hatalar oluşsa bile, kurtarma planı tamamlanana kadar çalışır.
7. Başarısız olduktan sonra, doğrulamak için VM'de oturum açın. 
8. Başarısız olmak için kullanmak üzere farklı kurtarma noktasına geçmek istiyorsanız, **Kurtarma noktasını değiştir'i**kullanın.
9. Hazır olduğunda, başarısızlık taahhüdünde bulunabilirsin. **Commit** eylemi, hizmette kullanılabilen tüm kurtarma noktalarını siler. **Kurtarma noktasını değiştir** seçeneği artık kullanılamıyor.

## <a name="run-a-planned-failover-hyper-v"></a>Planlı bir başarısızlık çalıştırma (Hyper-V)

Hyper-V VM'ler için planlanmış bir başarısızlık çalıştırabilirsiniz.

- Planlı bir hata, sıfır veri kaybı failover seçeneğidir.
- Planlı bir hata tetiklendiğinde, önce kaynak sanal makineler kapatılır, en son veriler eşitlenir ve ardından bir hata tetiklenir.
- **Planlı başarısızlık** seçeneğini kullanarak planlı bir hata çalıştırın. Bu düzenli bir failover benzer bir şekilde çalışır.
 
## <a name="track-failovers"></a>Arızaları izleme

Failover ile ilişkili işler vardır.

![Yük devretme](./media/site-recovery-failover/FailoverJob.png)

- **Önkoşullar kontrolü**: Başarısız olmak için gerekli tüm koşulların yerine getirilmesini sağlar.
- **Failover**: Verileri, azure vm'si oluşturulabilecek şekilde işler. **En Son** kurtarma noktasını seçtiyseniz, hizmete gönderilen verilerden bir kurtarma noktası oluşturulur.
- **Başlangıç**: Önceki adımda işlenen verileri kullanarak bir Azure VM oluşturur.

> [!WARNING]
> **Devam eden bir başarısızlık iptal etmeyin**: Failover başlamadan önce, VM için çoğaltma durdurulur. Devam eden bir işi iptal ederseniz, başarısızlık durur, ancak VM çoğalmaya başlamaz. Çoğaltma yeniden başlatılamaz.


### <a name="extra-failover-time"></a>Ekstra zaman fazla

Bazı durumlarda, VM failover genellikle tamamlamak için yaklaşık sekiz ila 10 dakika sürer ara adım gerektirir. Bu ek adım/zaman etkilenen makineler şunlardır:

* 9,8'den eski bir Mobilite hizmet sürümü çalıştıran VMware sanal makineleri.
* Fiziksel sunucular ve Hyper-V VM'ler fiziksel sunucular olarak korunur.
* VMware Linux VM'ler.
* Bu sürücülerin önyükleme sürücüsü olarak bulunmadığı VMware VM'ler:
    * storvsc
    * vmbus
    * storflt
    * intelide
    * Atapı
* DHCP veya statik IP adresleri kullanıp kullanmadıklarına bakılmaksızın DHCP etkin olmayan VMware VM'ler.


## <a name="automate-actions-during-failover"></a>Başarısız olurken eylemleri otomatikleştirin

Başarısız lık sırasında eylemleri otomatikleştirmek isteyebilirsiniz. Bunu yapmak için, kurtarma planlarında komut dosyalarını veya Azure otomasyon runbook'larını kullanabilirsiniz.

- Komut dosyaları ekleme de dahil olmak üzere kurtarma planları oluşturma ve özelleştirme hakkında [bilgi edinin.](site-recovery-create-recovery-plans.md)
- Kurtarma planlarına Azure Otomasyon runbook'ları ekleme hakkında [bilgi edinin.](site-recovery-runbook-automation.md)


## <a name="configure-settings-after-failover"></a>Başarısız olduktan sonra ayarları yapılandır

### <a name="retain-drive-letters-after-failover"></a>Başarısız olduktan sonra sürücü harflerini koruyun

Site Kurtarma, sürücü harflerinin bekletinme işlemlerini işler. VM çoğaltma sırasında diskleri dışlıyorsanız, bunun nasıl çalıştığına [bir örnek gözden geçirin.](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk)

### <a name="prepare-in-azure-to-connect-after-failover"></a>Azure'da başarısız olduktan sonra bağlanmak için hazırlanın

RDP veya SSH kullanarak başarısız olduktan sonra oluşturulan Azure VM'lerine bağlanmak istiyorsanız, tabloda özetlenen gereksinimleri izleyin.

**Başarısızlık** | **Konum** | **Eylemler**
--- | --- | ---
**Windows çalıştıran Azure VM** | Azure VM başarısız olduktan sonra |  VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> RDP bağlantı noktasına gelen bağlantılara izin vermek için VM üzerinden başarısız olan ağ güvenlik grubu kurallarının (ve bağlı olduğu Azure alt ağının) olması gerekir.<br/><br/> VM'nin ekran görüntüsünü doğrulamak için **Önyükleme tanılamalarını** denetleyin.<br/><br/> Bağlanamıyorsanız, VM'nin çalıştığını denetleyin ve bu [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)gözden geçirin.
**Linux çalıştıran Azure VM** | Azure VM başarısız olduktan sonra | VM üzerinden başarısız olan ağ güvenlik grubu kurallarının (ve bağlı olduğu Azure alt ağının) SSH bağlantı noktasına gelen bağlantılara izin vermeleri gerekir.<br/><br/> VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> VM'nin ekran görüntüsü için **Önyükleme tanılamalarını** kontrol edin.<br/><br/>

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.

## <a name="set-up-ip-addressing"></a>IP adresleme ayarlama

- **Dahili IP adresleri**: Bir Azure VM'nin dahili IP adresini başarısız olduktan sonra ayarlamak için birkaç seçeneğiniz vardır:
    - Aynı IP adresini koruyun: Azure VM'de şirket içi makineye ayrılan IP adresiyle aynı IP adresini kullanabilirsiniz.
    - Farklı IP adresi kullanın: Azure VM için farklı bir IP adresi kullanabilirsiniz.
    - Dahili IP adreslerini ayarlama hakkında [daha fazla bilgi edinin.](concepts-on-premises-to-azure-networking.md#assign-an-internal-address)
- **Harici IP adresleri**: Genel IP adreslerini başarısız lıkta saklayabilirsiniz. Başarısız olma işleminin bir parçası olarak oluşturulan Azure VM'lere Azure bölgesinde kullanılabilen bir Azure genel IP adresi atanmalıdır. Ortak bir IP adresi, el ile veya kurtarma planıyla işlemi otomatikleştirerek atayabilirsiniz. [Daha fazla bilgi edinin](concepts-public-ip-address-with-site-recovery.md).


## <a name="next-steps"></a>Sonraki adımlar

Başarısız olduktan sonra, Azure VM'lerini şirket içi siteye çoğaltmaya başlamak için yeniden korumanız gerekir. Çoğaltma çalışmaya başladıktan sonra, hazır olduğunuzda şirket içinde başarısız olabilirsiniz.

- Yeniden koruma ve geri dönüş hakkında [daha fazla bilgi edinin.](failover-failback-overview.md#reprotectionfailback)
- VMware yeniden koruma ve geri başarısız için [hazırlanın.](vmware-azure-reprotect.md)
- [Geri başarısız](hyper-v-azure-failback.md) Hiper-V VM' ler.
- Fiziksel sunucular için failover ve failback işlemi [hakkında bilgi edinin.](physical-to-azure-failover-failback.md)

