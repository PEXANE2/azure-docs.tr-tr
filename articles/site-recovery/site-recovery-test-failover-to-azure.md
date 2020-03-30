---
title: Azure Site Kurtarma'da Azure'da bir test başarısızlığını (olağanüstü durum kurtarma medeksiyonu) azure'da çalıştırın
description: Azure Site Kurtarma hizmetini kullanarak şirket içi ekrandan Azure'a bir test başarısızlığını çalıştırma hakkında bilgi edinin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: 26c734b7a2e9f5592ee6d51dfee4650a3998ab1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257530"
---
# <a name="run-a-test-failover-disaster-recovery-drill-to-azure"></a>Azure'da bir test başarısızlığını (olağanüstü durum kurtarma medeksiyonu) çalıştırma 


Bu makalede, Bir Site Kurtarma testi başarısız lığını kullanarak Azure'da olağanüstü durum kurtarma matkabının nasıl çalıştırılacak olduğu açıklanmaktadır.  

Çoğaltma ve olağanüstü durum kurtarma stratejinizi doğrulamak için herhangi bir veri kaybı veya kapalı kalma süresi olmadan bir test hatası çalıştırın. Bir test başarısız mı, devam eden çoğaltmayı veya üretim ortamınızı etkilemez. Belirli bir sanal makinede (VM) veya birden çok VM içeren bir [kurtarma planında](site-recovery-create-recovery-plans.md) bir test başarısızlığını çalıştırabilirsiniz.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
Bu yordam, kurtarma planı için bir test başarısızlığını nasıl çalıştırılabildiğini açıklar. Tek bir VM için bir test başarısızlığını çalıştırmak istiyorsanız, [burada](tutorial-dr-drill-azure.md#run-a-test-failover-for-a-single-vm) açıklanan adımları izleyin

![Test Yük Devretmesi](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Azure portalındaki Site Kurtarma'da,**Test Failover'ı**recoveryplan_name Kurtarma*Planları'nı* >  **Recovery Plans** > tıklatın.
2. Başarısız olmak için bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
    - **En son işlenen**: Bu seçenek, Site Kurtarma tarafından işlenen en son kurtarma noktasına plandaki tüm VM'ler üzerinde başarısız olur. Belirli bir VM için en son kurtarma noktasını görmek için VM ayarlarında **En Son Kurtarma Noktaları'na** bakın. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
    - **En son uygulama tutarlılığı**: Bu seçenek, Site Kurtarma tarafından işlenen en son uygulama tutarlı kurtarma noktasına plandaki tüm VM'ler üzerinde başarısız olur. Belirli bir VM için en son kurtarma noktasını görmek için VM ayarlarında **En Son Kurtarma Noktaları'na** bakın.
    - **En Son**: Bu seçenek, her VM için bir kurtarma noktası oluşturmak için önce Site Kurtarma hizmetine gönderilen tüm verileri işler. Bu seçenek en düşük RPO'yu (Kurtarma Noktası Hedefi) sağlar, çünkü başarısız olduktan sonra oluşturulan VM, başarısız lık tetiklendiğinde tüm verileri Site Kurtarma'ya çoğaltılır.
    - **En son çoklu VM işlenmiş**: Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM'li kurtarma planları için kullanılabilir. Ayarı etkin olan VM'ler en son ortak multi-VM tutarlı kurtarma noktasına geçemez. Diğer VM'ler en son işlenmiş kurtarma noktasına geçemez.  
    - **En son multi-VM uygulaması tutarlılığı**: Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM'li kurtarma planları için kullanılabilir. Çoğaltma grubunun bir parçası olan VM'ler en son ortak çoklu VM uygulama tutarlı kurtarma noktasına geçemez. Diğer VM'ler en son uygulama tutarlı kurtarma noktasına geçemedi.
    - **Özel**: Belirli bir VM üzerinde belirli bir kurtarma noktasına başarısız olmak için bu seçeneği kullanın.
3. Test Sanal M'lerinin oluşturulacağı bir Azure sanal ağı seçin.

    - Site Kurtarma, VM'nin **Bilgi İşlem ve Ağ** ayarlarında sağlananla aynı ada ve aynı IP adresine sahip bir alt ağda test VM'leri oluşturmaya çalışır.
    - Test başarısız olmak için kullanılan Azure sanal ağında aynı ada sahip bir alt ağ yoksa, test VM alfabetik olarak ilk alt ağda oluşturulur.
    - Alt ağda aynı IP adresi yoksa, VM alt ağda başka bir kullanılabilir IP adresi alır. [Daha fazla bilgi edinin](#create-a-network-for-test-failover).
4. Azure'da başarısız oluyorsanız ve veri şifreleme etkinse, **Şifreleme Anahtarı'nda**Sağlayıcı yüklemesi sırasında şifrelemeyi etkinleştirdiğinizde verilen sertifikayı seçin. Şifreleme etkin değilse bu adımı yok sayabilirsiniz.
5. **İşler** sekmesinde başarısız olan ilerlemeyi izleyin. Azure portalında test çoğaltma makinesini görebilmelisiniz.
6. Azure VM'ye RDP bağlantısı başlatmak için, Başarısız VM'nin ağ arabirimine [genel bir IP adresi eklemeniz](https://aka.ms/addpublicip) gerekir.
7. Her şey beklendiği gibi çalıştığında, **Temizleme testi başarısız'nı**tıklatın. Bu, test başarısızolması sırasında oluşturulan VM'leri siler.
8. **Notlar**’da, yük devretme testiyle ilişkili gözlemlerinizi kaydedin ve saklayın.


![Test Yük Devretmesi](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Bir test başarısızlık tetiklendiğinde, aşağıdakioluşur:

1. **Önkoşullar**: Başarısız olmak için gerekli tüm koşulların yerine getirildiğinden emin olmak için ön koşullar kontrol çalışır.
2. **Failover**: Bir Azure VM oluşturulabilmesi için veri işlemleri ve hazırladı.
3. **Son**: En son kurtarma noktasını seçtiyseniz, hizmete gönderilen verilerden bir kurtarma noktası oluşturulur.
4. **Başlangıç**: Bu adım, önceki adımda işlenen verileri kullanarak bir Azure sanal makinesi oluşturur.

### <a name="failover-timing"></a>Başarısız zamanlama

Aşağıdaki senaryolarda, failover genellikle tamamlamak için yaklaşık 8 ila 10 dakika sürer ekstra bir ara adım gerektirir:

* 9,8 yaşından büyük Mobilite hizmetinin bir sürümünü çalıştıran VMware VM'ler
* Fiziksel sunucular
* VMware Linux VM'ler
* Hyper-V VM fiziksel sunucular olarak korunuyor
* Aşağıdaki sürücülerin önyükleme sürücüleri olmadığı VMware VM:
    * storvsc
    * vmbus
    * storflt
    * intelide
    * Atapı
* DHCP veya statik IP adresleri kullanıp kullanmadığına bakılmaksızın, DHCP etkin olmayan VMware VM.

Diğer tüm durumlarda, hiçbir ara adım gerekli değildir ve failover önemli ölçüde daha az zaman alır.


## <a name="create-a-network-for-test-failover"></a>Yük devretme testi için ağ oluşturma

Yük devretme testi için her VM'nin **İşlem ve Ağ** ayarlarından üretim kurtarma sitesi ağınızdan yalıtılmış bir ağ seçmenizi öneririz. Varsayılan olarak yeni oluşturduğunuz Azure sanal ağları diğer ağlardan yalıtılmış olur. Test ağı üretim ağınıza benzer olmalıdır:

- Test ağında üretim ağınızdaki sayıda alt ağ olmalıdır. Alt ağların adı aynı olmalıdır.
- Test ağında aynı IP adresi aralığı kullanılmalıdır.
- Test ağının DNS sunucusunu **İşlem ve Ağ** ayarlarında DNS VM için belirtilen IP adresiyle güncelleştirin. Daha fazla ayrıntı için [Active Directory için yük devretme testi ile ilgili dikkat edilmesi gerekenler](site-recovery-active-directory.md#test-failover-considerations) sayfasını okuyun.


## <a name="test-failover-to-a-production-network-in-the-recovery-site"></a>Kurtarma sitesindeki bir üretim ağına test etme

Üretim ağınızdan ayrı bir test ağı kullanmanızı önermiş olsak da, üretim ağınızda bir olağanüstü durum kurtarma demlemesi test etmek istiyorsanız aşağıdakilere dikkat edin:

- Test başarısızlığını çalıştırdığınızda birincil VM'nin kapatıldığından emin olun. Aksi takdirde aynı kimliğe sahip iki VM aynı anda aynı ağda çalışan olacaktır. Bu beklenmedik sonuçlara yol açabilir.
- Test başarısızmı için oluşturulan VM'lerde yapılan değişiklikler, başarısız olmayı temizledikçe kaybolur. Bu değişiklikler birincil VM'ye geri çoğaltılamaz.
- Üretim ortamınızda test etmek, üretim uygulamanızın kapalı kalma süresine yol açar. Kullanıcılar, test başarısızlığını niçin devam ederken VM'lerde çalışan uygulamaları kullanmamalıdır.  



## <a name="prepare-active-directory-and-dns"></a>Aktif Dizin ve DNS Hazırla

Uygulama testi için bir test başarısızlığını çalıştırmak için, test ortamınızdaki üretim Active Directory ortamınızın bir kopyasına ihtiyacınız vardır. Active Directory'nin daha fazla bilgi edinmesi [için test başarısızlarını](site-recovery-active-directory.md#test-failover-considerations) okuyun.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Başarısız olduktan sonra RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız, tabloda özetlenen gereksinimleri izleyin.

**Başarısızlık** | **Konum** | **Eylemler**
--- | --- | ---
**Windows çalıştıran Azure VM** | Arıza dan önce şirket içi makine | Azure VM'ye Internet üzerinden erişmek için RDP'yi etkinleştirin ve **TCP**ve UDP kurallarının Herkese Açık olarak eklenmediğinden ve **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalardaki**tüm profiller için RDP'ye izin verildiğinden emin olun.<br/><br/> Azure VM'ye sitebağlantısı üzerinden erişmek için makinede RDP'yi etkinleştirin ve **Etki Alanı ve Özel** ağlar için Windows Güvenlik **Duvarı** -> **İzin Verilen uygulamalarda ve özelliklerde**RDP'ye izin verildiğini sağlayın.<br/><br/>  İşletim sistemi SAN ilkesinin **OnlineAll**olarak ayarlandıklarına emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Bir başarısızlığı tetiklediğinde VM'de bekleyen Windows güncelleştirmeleri olmadığından emin olun. Windows güncelleştirmesi, başarısız olduğunuzda başlayabilir ve güncelleştirme tamamlanana kadar VM'de oturum açamazsınız.
**Windows çalıştıran Azure VM** | Azure VM başarısız olduktan sonra |  VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> RDP bağlantı noktasına gelen bağlantılara izin vermek için VM üzerinden başarısız olan ağ güvenlik grubu kurallarının (ve bağlı olduğu Azure alt ağının) olması gerekir.<br/><br/> VM'nin ekran görüntüsünü doğrulamak için **Önyükleme tanılamalarını** denetleyin.<br/><br/> Bağlanamıyorsanız, VM'nin çalıştığını denetleyin ve bu [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)gözden geçirin.
**Linux çalıştıran Azure VM** | Arıza dan önce şirket içi makine | VM'deki Secure Shell hizmetinin sistem önyüklemede otomatik olarak başlatılıdığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.
**Linux çalıştıran Azure VM** | Azure VM başarısız olduktan sonra | VM üzerinden başarısız olan ağ güvenlik grubu kurallarının (ve bağlı olduğu Azure alt ağının) SSH bağlantı noktasına gelen bağlantılara izin vermeleri gerekir.<br/><br/> VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> VM'nin ekran görüntüsü için **Önyükleme tanılamalarını** kontrol edin.<br/><br/>

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bir olağanüstü durum kurtarma matkabını tamamladıktan sonra, diğer [başarısızlık](site-recovery-failover.md)türleri hakkında daha fazla bilgi edinin.
