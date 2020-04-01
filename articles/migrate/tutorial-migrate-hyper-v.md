---
title: Azure Geçir Sunucu Geçişi ile Hyper-V VM'leri Azure'a geçirin
description: Azure Geçiş Sunucusu Geçişi ile şirket içi Hyper-V V MM'leri Azure'a nasıl geçirebilirsiniz öğrenin
ms.topic: tutorial
ms.date: 11/18/2019
ms.custom:
- MVC
- fasttrack-edit
ms.openlocfilehash: b5d37da7ea0c53a7e8cbb5b579d529dd4a799fed
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422691"
---
# <a name="migrate-hyper-v-vms-to-azure"></a>Hyper-V VM’lerini Azure’a geçirme 

Bu makalede, Azure Geçişi: Sunucu Geçişi aracıyla aracısız geçiş kullanarak şirket içi Hyper-V V VM'leri Azure'a nasıl geçirilebilirsiniz.

[Azure Geçiş,](migrate-services-overview.md) şirket içi uygulamalarınızın ve iş yüklerinizin ve özel/genel bulut VM'lerinizin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Bu öğretici, Azure Geçiş Sunucusu Değerlendirmesi ve Geçişi kullanarak Hyper-V'nin Azure'a nasıl değerlendirilip Azure'a geçirilen bir serinin üçüncüsüdür. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:


> [!div class="checklist"]
> * Azure'u ve şirket içi Hyper-V ortamınızı hazırlayın
> * Kaynak ortamını ayarlayın ve bir çoğaltma cihazı dağıtın.
> * Hedef ortamı ayarlayın.
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Azure'a tam bir geçiş çalıştırın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. Hyper-V geçiş mimarisini [gözden geçirin.](hyper-v-migration-architecture.md)
2. Geçiş için Azure ve Hyper-V'yi ayarlamak için bu [serinin ilk eğitimini tamamlayın.](tutorial-prepare-hyper-v.md) İlk öğreticide, siz:
    - Azure'u geçiş için [hazırlayın.](tutorial-prepare-hyper-v.md#prepare-azure)
    - Şirket içi ortamı göçe [hazırlayın.](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-migration)
3. Azure'a geçirmeden önce Azure Geçir: Sunucu Değerlendirmesi'ni kullanarak Hyper-V V VM'leri değerlendirmeyi denemenizi öneririz. Bunu yapmak için, bu [serinin ikinci öğretici tamamlayın.](tutorial-assess-hyper-v.md) Bir değerlendirme denemenizi öneririz, ancak VM'leri geçirmeden önce bir değerlendirme çalıştırmanız gerekmez.
4. Azure hesabınıza Sanal Makine Katılımcısı rolü atandığından emin olun, böylece aşağıdakilere izin verebilirsiniz:

    - Seçilen kaynak grubunda sanal makine oluşturma.
    - Seçilen sanal ağda sanal makine oluşturma.
    - Azure yönetilen bir diske yazın.
5. [Azure ağı ayarlayın.](../virtual-network/manage-virtual-network.md#create-a-virtual-network) Azure'a geçiş yaptığınızda, oluşturulan Azure VM'leri, geçiş ayarlarken belirttiğiniz bir Azure ağına katılır.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure Geçir Sunucusu Geçişi aracını ekleme

Hyper-V VM'leri değerlendirmek için ikinci öğreticiyi izlemediyseniz, bir Azure Geçir projesi ayarlamak ve projeye Azure Geçir Sunucusu Değerlendirmesi aracını eklemek için [bu yönergeleri izlemeniz](how-to-add-tool-first-time.md) gerekir.

İkinci öğreticiyi izlediyseniz ve zaten bir Azure Geçiş projeniz varsa, Azure Geçiş: Sunucu Geçişi aracını aşağıdaki gibi ekleyin:

1. Azure Geçir projesinde **Genel Bakış'ı**tıklatın. 
2. **Sunucuları Keşfet, değerlendir ve geçiş te,** **sunucuları değerlendir'i ve geçişini**tıklatın.
3. **Geçiş araçlarında**, **geçiş yapmaya hazır olduğunuzda bir geçiş aracı eklemek için burayı tıklatın'ı**seçin.

    ![Bir araç seçin](./media/tutorial-migrate-hyper-v/select-migration-tool.png)

4. Araçlar listesinde Azure **Geçiş: Sunucu Geçişi** > **Ekle aracını** seçin

    ![Sunucu Geçişi aracı](./media/tutorial-migrate-hyper-v/server-migration-tool.png)


## <a name="set-up-the-azure-migrate-appliance"></a>Azure Geçiş cihazını ayarlama

Azure Geçir Sunucusu Geçişi, verileri Azure Geçişi'nde düzenlemek ve çoğaltmak için Hyper-V Ana Bilgisayarlarında veya küme düğümlerinde bir yazılım aracısı çalıştırır ve geçiş için özel bir cihaz gerektirmez.

- Azure Geçir : Sunucu Değerlendirme cihazı VM keşfi gerçekleştirir ve VM meta verilerini ve performans verilerini Azure Geçiş Sunucusu Geçişi'ne gönderir.
- Geçiş düzenleme ve veri çoğaltma Microsoft Azure Site Kurtarma sağlayıcısı ve Microsoft Azure Kurtarma Hizmeti aracısı tarafından işlenir.

Cihazı kurmak için:
- Hyper-V VM'leri değerlendirmek için ikinci öğreticiyi izlediyseniz, bu eğitim sırasında cihazı zaten kurmuşsunuzdur ve tekrar yapmanız gerekmez.
- Bu öğreticitakip etmediyseniz, şimdi cihaz kurmak gerekir. Bunu yapmak için, siz: 

    - Azure portalından sıkıştırılmış hyper-v vhd indirin.
    - Cihazı oluşturun ve Azure Geçiş Sunucusu Değerlendirmesi'ne bağlanıp bağlanabıp bağlanabıp bağlanamayalı kontrol edin. 
    - Cihazı ilk kez yapılandırın ve Azure Geçiş projesine kaydedin.

    Cihazı kurmak için [bu makaledeki](how-to-set-up-appliance-hyper-v.md) ayrıntılı talimatları uygulayın.

## <a name="prepare-hyper-v-hosts"></a>Hyper-V ana bilgisayarlarını hazırlayın

1. Azure > **Sunucular**projesinde , **Azure Geçir: Sunucu Geçişinde,** **Keşfet'i**tıklatın.
2. **Discover makinelerinde** >  **Yes, with Hyper-V****makineleriniz sanallaştırıldı mı?**
3. **Hedef bölgesinde,** makineleri geçirmek istediğiniz Azure bölgesini seçin.
6. **Geçiş için hedef bölgenin bölge adı olduğunu onayla'yı**seçin.
7. **Kaynak Oluştur'u**tıklatın. Bu, arka planda bir Azure Site Kurtarma kasası oluşturur.
    - Azure Geçiş Sunucusu Geçişi ile geçiş ayarlamayı zaten ayarladıysanız, kaynaklar daha önce ayarlandığı için bu seçenek görünmez.
    - Bu düğmeyi tıklattıktan sonra bu projenin hedef bölgesini değiştiremezsiniz.
    - Sonraki tüm göçler bu bölgeye dir.
    
8. **Hyper-V ana bilgisayar sunucularını hazırla'da**Hyper-V Çoğaltma sağlayıcısını ve kayıt anahtarı dosyasını indirin.
    - Hyper-V ana bilgisayarını Azure Geçiş Sunucusu Geçişi'ne kaydetmek için kayıt anahtarı gereklidir.
    - Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![İndirme sağlayıcısı ve anahtarı](./media/tutorial-migrate-hyper-v/download-provider-hyper-v.png)

4. Sağlayıcı kurulum dosyasını ve kayıt anahtarı dosyasını çoğaltmak istediğiniz VM'leri çalıştıran her Hyper-V ana bilgisayarına (veya küme düğümüne) kopyalayın.
5. Sonraki yordamda açıklandığı gibi, her ana bilgisayarda sağlayıcı kurulum dosyasını çalıştırın.
6. Sağlayıcıyı ana bilgisayarlara yükledikten sonra, **Discover makinelerinde,** **Kaydı Kesinleştir'i**tıklatın.

    ![Kaydı sonuçlandırın](./media/tutorial-migrate-hyper-v/finalize-registration.png)

Keşfedilen VM'ler Azure Geçiş Sunucusu Geçişi'nde görünene kadar kaydı niçin sonlandırıldıktan sonra 15 dakika kadar sürebilir. VM'ler keşfedildikçe, **Keşfedilen sunucular** yükselir.

![Keşfedilen sunucular](./media/tutorial-migrate-hyper-v/discovered-servers.png)

### <a name="register-hyper-v-hosts"></a>Hyper-V ana bilgisayarlarını kaydedin

İndirilen kurulum dosyasını (AzureSiteRecoveryProvider.exe) ilgili her Hyper-V ana bilgisayara yükleyin.

1. Sağlayıcı kurulum dosyasını her ana bilgisayar veya küme düğümünde çalıştırın.
2. **Microsoft Update'>** Sağlayıcı Kurulumu sihirbazında, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update'i kullanmayı tercih edin.
3. **Yükleme'de,** Sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve **Yükle'yi**seçin.
4. Yüklemeden sonra, Kayıt Sihirbazı > **Vault Ayarları'nda** **Gözat'ı**seçin ve **Anahtar Dosyası'nda**indirdiğiniz kasa anahtar dosyasını seçin.
5. **Proxy**Ayarları'nda, ana bilgisayarda çalışan sağlayıcının internete nasıl bağlandığını belirtin.
    - Cihaz bir proxy sunucusunun arkasında bulunuyorsa, proxy ayarlarını belirtmeniz gerekir.
    - Proxy adını , **http://ip-address**veya **http://FQDN**. olarak belirtin HTTPS proxy sunucuları desteklenmez.
   

6. Sağlayıcının [gerekli URL'lere](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts)erişebileceğinden emin olun.
7. **Kayıt'ta,** ana bilgisayar kaydedildikten sonra **Finish'i**tıklatın.

## <a name="replicate-hyper-v-vms"></a>Hyper-V VM'leri Çoğaltma

Keşif tamamlandıktan sonra Hyper-V V M'lerin Azure'a çoğaltılmasını başlatabilirsiniz.

> [!NOTE]
> En fazla 10 makineyi birlikte çoğaltabilirsiniz. Daha fazla çoğaltmanız gerekiyorsa, bunları aynı anda 10'luk gruplar halinde çoğaltın.

1. Azure Geçir projesinde **> Sunucular,** **Azure Geçir: Sunucu Geçişi**, **Çoğaltma'yı**tıklatın.
2. **Çoğaltma**, > **Kaynak ayarları** > **Makineleriniz sanallaştırılmış mı?** **Yes, with Hyper-V** Sonra **İleri'yi tıklatın: Sanal makineler.**
3. **Sanal makineler** bölümünde çoğaltmak istediğiniz makineleri seçin.
    - VM’ler için bir değerlendirme çalıştırırsanız değerlendirme sonuçlarından VM boyutlandırma ve disk türü (premium/standart) önerilerini uygulayabilirsiniz. Bunu yapmak için **Azure Geçişi değerlendirmesinden geçiş ayarları içe aktarılsın mı?** bölümünde **Evet**’i seçin.
    - Bir değerlendirme çalıştırmadıysanız veya değerlendirme ayarlarını kullanmak istemiyorsanız **Hayır**’ı seçin.
    - Değerlendirmeyi kullanmayı seçtiyseniz VM grubunu ve değerlendirme adını belirleyin.

        ![Değerlendirme seçme](./media/tutorial-migrate-hyper-v/select-assessment.png)

4. VM’leri **Sanal makineler** bölümünde gerektiği şekilde arayın ve geçirmek istediğiniz her bir VM’yi seçin. Ardından, **İleri'yi tıklatın: Hedef ayarları.**

    ![VM'leri seçin](./media/tutorial-migrate-hyper-v/select-vms.png)

5. **Hedef ayarlarında,** geçiş yaptığınız hedef bölgeyi, aboneliği ve geçişten sonra Azure VM'lerinin kalacağı kaynak grubunu seçin.
7. **Çoğaltma Depolama Hesabı'nda,** çoğaltılan verilerin Azure'da depolandığı Azure Depolama hesabını seçin.
8. **Sanal Ağ,** geçişten sonra Azure VM'lerinin birleşeceği Azure VNet/alt net'ini seçin.
9. **Azure Hibrit Avantajı**’nda:

    - Azure Hibrit Avantajı’nı uygulamak istemiyorsanız **Hayır**’ı seçin. Ardından **İleri'yi**tıklatın.
    - Etkin Yazılım Güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve avantajı geçirdiğiniz makinelere uygulamak istiyorsanız **Evet**’i seçin. Ardından **İleri**’ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-hyper-v/target-settings.png)

10. **İşlem** bölümünde VM adını, boyutunu, İşletim Sistemi disk türünü ve kullanılabilirlik kümesini gözden geçirin. VM’ler [Azure gereksinimleriyle](migrate-support-matrix-hyper-v-migration.md#azure-vm-requirements)uyumlu olmalıdır.

    - **VM boyutu**: Değerlendirme önerileri kullanıyorsanız, VM boyutu açılır bırakma önerilen boyutu içerir. Aksi takdirde Azure Geçişi, Azure aboneliğindeki en yakın eşleşmeye göre bir boyut seçer. Alternatif olarak **Azure VM boyutu **’nda el ile bir boyut seçin. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. 
    - **Kullanılabilirlik kümesi**: VM geçişten sonra bir Azure kullanılabilirlik kümesinde olması gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![VM bilgi işlem ayarları](./media/tutorial-migrate-hyper-v/compute-settings.png)

11. **Diskler** bölümünde, VM disklerinin Azure’a çoğaltılıp çoğaltılmayacağını belirtin ve Azure’da disk türünü (Standart SSD/HDD veya premium yönetilen diskler) seçin. Ardından **İleri**’ye tıklayın.
    - Diskleri çoğaltmadan çıkarabilirsiniz.
    - Diskleri çıkarırsanız bu diskler geçişten sonra Azure VM’de bulunmaz. 

    ![Diskler](./media/tutorial-migrate-hyper-v/disks.png)

10. **Çoğaltmayı gözden geçir ve başlat** bölümünde ayarları gözden geçirin ve sunuculara yönelik ilk çoğaltmayı başlatmak için **Çoğalt** üzerine tıklayın.

> [!NOTE]
> Çoğaltma başlamadan önce çoğaltma ayarlarını çoğaltma ayarlarını çoğaltma**yı** **yönet** > makinelerde istediğiniz zaman güncelleştirebilirsiniz. Çoğaltma başladıktan sonra ayarlar değiştirilemez.

## <a name="provisioning-for-the-first-time"></a>İlk kez sağlanması

Azure Geçiş projesinde çoğalttığınız ilk VM buysa, Azure Geçiş: Sunucu Geçişi bu kaynakları projeyle aynı kaynak grubunda otomatik olarak karşılar.

- **Servis veri kurumu**: Azure Geçiş: Sunucu Geçişi, cihaza çoğaltma düzenleme iletileri göndermek için Servis Veri Gönderi'ni kullanır.
- **Ağ geçidi depolama hesabı**: Azure Geçişi: Sunucu Geçişi, çoğaltılan VM'ler hakkındaki durum bilgilerini depolamak için ağ geçidi depolama hesabını kullanır.
- **Günlük depolama hesabı**: Azure Geçir cihazı, VM'ler için çoğaltma günlüklerini bir günlük depolama hesabına yükler. Azure Geçir, çoğaltma bilgilerini çoğaltma yönetilen disklere uygular.
- **Anahtar kasası**: Azure Geçir cihazı, servis veri aracının bağlantı dizelerini yönetmek için anahtar kasasını ve çoğaltmada kullanılan depolama hesaplarının erişim anahtarlarını kullanır. Azure'u Hyper-V VM değerlendirmesi ve geçişi için [hazırladığınız](tutorial-prepare-hyper-v.md#prepare-azure) zaman, anahtar kasanın depolama hesabına erişmek için ihtiyaç duyduğu izinleri ayarlamanız gerekirdi. 


## <a name="track-and-monitor"></a>İzleme ve izleme


- Bir Başlat Çoğaltma işi **çoğaltma'yı** tıklattığınızda iş başlar. 
- Çoğaltmayı Başlat işi başarıyla bittiğinde, makineler ilk çoğaltmalarını Azure'a başlar.
- İlk çoğaltma bittikten sonra, delta çoğaltma başlar. Şirket içi disklerde yapılan artımlı değişiklikler düzenli olarak Azure'da çoğaltılır.

Portal bildirimlerinde iş durumunu izleyebilirsiniz.

**Azure Geçişi: Sunucu Geçişi'nde** **çoğaltma sunucularını** tıklatarak çoğaltma durumunu izleyebilirsiniz.
![Çoğaltmayı izleme](./media/tutorial-migrate-hyper-v/replicating-servers.png)



## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Delta çoğaltma başladığında, Azure'a tam bir geçiş çalıştırmadan önce VM'ler için bir test geçişi çalıştırabilirsiniz. Bunu her makine için en az bir kez, geçirmeden önce yapmanızı öneririz.

- Bir test geçişi, çalışmaya devam eden ve çoğaltmaya devam eden şirket içi makineleri etkilemeden, geçişin beklendiği gibi çalışacağını denetler. 
- Test geçişi, çoğaltılan verileri kullanarak (genellikle Azure aboneliğinizde üretim dışı bir Azure VNet'e geçiş yaparak) bir Azure VM oluşturarak geçişi simüle eder.
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce tüm sorunları gidermek için çoğaltılan Azure VM testini kullanabilirsiniz.

Bir test geçişini aşağıdaki gibi yapın:


1. **Geçiş hedefleri** > **Sunucularında** > Azure**Geçir: Sunucu Geçişi**, **geçirilen sunucuları sına'yı**tıklatın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-hyper-v/test-migrated-servers.png)

2. Test edilecek VM’yi sağ tıklayıp **Test geçişi** üzerine tıklayın.

    ![Test geçişi](./media/tutorial-migrate-hyper-v/test-migrate.png)

3. **Sına'dan**Geçiş'te, geçişten sonra Azure VM'nin bulunacağı Azure sanal ağını seçin. Üretim dışı bir sanal ağ kullanmanızı öneririz.
4. **Test Geçişi** işlemi başlar. Portal bildirimlerinde işi izleyin.
5. Geçiş bittikten sonra geçirilen Azure VM’yi, Azure portalında **Sanal Makineler** bölümünde görüntüleyin. Makine adında **Testi** şeklinde bir son ek vardır.
6. Test tamamlandıktan sonra **Makineleri çoğaltma** bölümünde Azure VM’yi sağ tıklayıp **Test geçişini temizle** üzerine tıklayın.

    ![Geçişi temizleme](./media/tutorial-migrate-hyper-v/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçişinin beklendiği gibi çalıştığını doğruladıktan sonra, şirket içi makineleri geçirebilirsiniz.

1. Azure Geçir projesinde **> Sunucular** > **Azure Geçir: Sunucu Geçişi**, **sunucuları çoğaltma'yı**tıklatın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-hyper-v/replicate-servers.png)

2. **Makineleri çoğaltma** bölümünde VM > **Geçir** üzerine sağ tıklayın.
3. **Geçiret** > **Sanal makineleri kapatın ve veri kaybı olmadan planlı geçiş gerçekleştirin**, **Evet** > **Tamam'ı**seçin.
    - Azure Geçişi, varsayılan olarak şirket içi VM’yi kapatır ve son çoğaltmadan bu yana gerçekleşen tüm VM değişikliklerini eşitlemek için bir isteğe bağlı çoğaltma çalıştırır. Bu, veri kaybı olmamasını sağlar.
    - VM’yi kapatmak istemiyorsanız, **Hayır** seçeneğini belirleyin
4. VM için bir geçiş işlemi başlar. Azure bildirimlerinde işlemi izleyin.
5. İşlem bittikten sonra **Sanal Makineler** sayfasında VM’yi görüntüleyebilir ve yönetebilirsiniz.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yapıldıktan sonra VM > **Stop geçişine**sağ tıklayın. Bu aşağıdakileri yapar:
    - Şirket içi makinenin çoğaltmasını durdurur.
    - Makineyi Azure Geçiş: Sunucu Geçişi'nde **çoğaltma sunucuları** sayısından kaldırır.
    - VM için çoğaltma durumu bilgilerini temizler.
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
