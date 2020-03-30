---
title: Azure Site Kurtarma'yı kullanarak bir IIS web uygulaması için olağanüstü durum kurtarma ayarlama
description: Azure Site Kurtarma'yı kullanarak IIS web çiftliği sanal makinelerini nasıl kopyalayacağınız hakkında bilgi edinin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 513a0f28fc03cbf24e35112245c9756d5ce00783
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954662"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-iis-based-web-application"></a>Çok katmanlı IIS tabanlı bir web uygulaması için olağanüstü durum kurtarma yı ayarlama

Uygulama yazılımı bir kuruluşta iş verimliliğinin lokomotifidir. Çeşitli web uygulamaları bir kuruluşta farklı amaçlara hizmet edebilir. Bordro işleme, finansal uygulamalar ve müşteriye yönelik web siteleri için kullanılan uygulamalar gibi bazı uygulamalar kuruluş için kritik olabilir. Verimlilik kaybını önlemek için, kuruluşun bu uygulamaları sürekli olarak çalışır hale getirmek önemlidir. Daha da önemlisi, bu uygulamaların tutarlı bir şekilde kullanılabilir olması, kuruluşun marka veya imajına zarar görmesini önlemeye yardımcı olabilir.

Kritik web uygulamaları genellikle çok katmanlı uygulamalar olarak ayarlanır: web, veritabanı ve uygulama farklı katmanlarda dır. Uygulamalar, çeşitli katmanlara yayılmasının yanı sıra, trafiği dengelemek için her katmanda birden çok sunucu kullanabilir. Ayrıca, çeşitli katmanlar ve web sunucusu arasındaki eşlemeler statik IP adreslerine dayalı olabilir. Bu eşlemelerin bazılarının, özellikle web sunucusunda birden çok web sitesi yapılandırıldığında güncellenmesi gerekir. Web uygulamaları SSL kullanıyorsa, sertifika bağlamalarını güncelleştirmeniz gerekir.

Çoğaltmayı temel almama geleneksel kurtarma yöntemleri, çeşitli yapılandırma dosyalarını, kayıt defteri ayarlarını, ciltleri, özel bileşenleri (COM veya .NET), içeriği ve sertifikaları yedeklemeyi içerir. Dosyalar bir dizi el ile adımla kurtarılır. Dosyaları yedekleme ve el ile kurtarma nın geleneksel kurtarma yöntemleri hantal, hataya açık ve ölçeklenebilir değildir. Örneğin, sertifikaları yedeklemeyi kolayca unutabilirsiniz. Başarısız olduktan sonra, sunucu için yeni sertifikalar satın almaktan başka seçeneğiniz kalmaz.

İyi bir olağanüstü durum kurtarma çözümü, karmaşık uygulama mimarileri için kurtarma planlarını modellemeyi destekler. Ayrıca, katmanlar arasındaki uygulama eşlemelerini işlemek için kurtarma planına özelleştirilmiş adımlar ekleyebilmelisin. Bir felaket varsa, uygulama eşlemeleri daha düşük bir RTO'ya yol açmanıza yardımcı olan tek tıklamalı, kesin çekim çözümü sağlar.

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Internet Bilgi Hizmetleri'ni (IIS) temel alan bir web uygulamasının nasıl korunulacak açıklanmaktadır. Makale, üç katmanlı, IIS tabanlı bir web uygulamasını Azure'a çoğaltmak için en iyi uygulamaları, olağanüstü durum kurtarma demlemeyi nasıl yapılacağını ve Azure'a uygulama üzerinde nasıl başarısız olunmasını kapsar.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki görevleri nasıl yapacağınızı bildiğinizden emin olun:

* [Sanal bir makineyi Azure'a çoğaltma](vmware-azure-tutorial.md)
* [Kurtarma ağı tasarlama](site-recovery-network-design.md)
* [Azure'da bir test başarısızmı yapma](site-recovery-test-failover-to-azure.md)
* [Azure'da başarısız olun](site-recovery-failover.md)
* [Etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
* [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım desenleri
IIS tabanlı bir web uygulaması genellikle aşağıdaki dağıtım desenlerinden birini izler:

**Dağıtım deseni 1**

IIS sunucusu ve SQL Server ile IIS tabanlı bir web çiftliği, Uygulama İsteği Yönlendirme (ARR) ile.

![Üç katmanlı IIS tabanlı bir web çiftliğinin diyagramı](./media/site-recovery-iis/deployment-pattern1.png)

**Dağıtım deseni 2**

ARR, IIS sunucusu, uygulama sunucusu ve SQL Server içeren IIS tabanlı bir web çiftliği.

![Dört katmanlı IIS tabanlı bir web çiftliğinin diyagramı](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makaledeki örnekler için, Windows Server 2012 R2 Enterprise'da IIS 7.5'e sahip VMware sanal makinelerini kullanıyoruz. Site Kurtarma çoğaltma uygulamaya özgü olmadığından, bu makaledeki önerilerin aşağıdaki tabloda listelenen senaryolarda ve IIS'nin farklı sürümlerinde uygulanması beklenmektedir.

### <a name="source-and-target"></a>Kaynak ve hedef

Senaryo | İkincil siteye | Azure’a
--- | --- | ---
Hyper-V | Evet | Evet
VMware | Evet | Evet
Fiziksel sunucu | Hayır | Evet
Azure|NA|Evet

## <a name="replicate-virtual-machines"></a>Sanal makineleri çoğaltma

Tüm IIS web çiftliği sanal makinelerini Azure'a çoğaltmaya başlamak [için, Site Kurtarma'da Azure'a test etme 'deki](site-recovery-test-failover-to-azure.md)kılavuzu izleyin.

Statik bir IP adresi kullanıyorsanız, sanal makinenin almasını istediğiniz IP adresini belirtebilirsiniz. IP adresini ayarlamak **için, İşlem ve Ağ ayarları** > **HEDEF IP'ye**gidin.

![Site Kurtarma İşlemi ve Ağ bölmesinde hedef IP'nin nasıl ayarlanacağını gösteren ekran görüntüsü](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
Kurtarma planı, bir hata sırasında çok katmanlı bir uygulamada çeşitli katmanların sırasını destekler. Sıralama, uygulama tutarlılığını korumaya yardımcı olur. Çok katmanlı bir web uygulaması için bir kurtarma planı oluşturduğunuzda, [Site Kurtarma'yı kullanarak kurtarma planı oluştur'da](site-recovery-create-recovery-plans.md)açıklanan adımları tamamlayın.

### <a name="add-virtual-machines-to-failover-groups"></a>Başarısız gruplara sanal makineler ekleme
Tipik bir çok katmanlı IIS web uygulaması aşağıdaki bileşenlerden oluşur:
* SQL sanal makineleri olan bir veritabanı katmanı.
* Bir IIS sunucusu ve uygulama katmanından oluşan web katmanı. 

Katmana göre farklı gruplara sanal makineler ekleyin:

1. Bir kurtarma planı oluşturun. Grup 1'in altına veritabanı katmanı sanal makineleri ekleyin. Bu, veritabanı katmanı sanal makinelerin en son kapatılmasını ve önce gündeme getirilmesini sağlar.
1. Grup 2'nin altındaki uygulama katmanı sanal makinelerini ekleyin. Bu, uygulama katmanı sanal makinelerin veritabanı katmanı getirildikten sonra gündeme getirilmesini sağlar.
1. Grup 3'te web katmanı sanal makineleri ekleyin. Bu, web katmanı sanal makinelerin uygulama katmanı gündeme getirildikten sonra gündeme getirilmesini sağlar.
1. Grup 4'te yük dengesi sanal makineleri ekleyin. Bu, yük dengesi sanal makinelerin web katmanı gündeme getirildikten sonra gündeme getirilmesini sağlar.

Daha fazla bilgi için [bkz.](site-recovery-runbook-automation.md#customize-the-recovery-plan)


### <a name="add-a-script-to-the-recovery-plan"></a>Kurtarma planına komut dosyası ekleme
IIS web çiftliğinin düzgün çalışması için, Azure sanal makinelerinde başarısız lık sonrası veya bir test başarısız olması sırasında bazı işlemler yapmanız gerekebilir. Bazı başarısız lık sonrası işlemleri otomatikleştirebilirsiniz. Örneğin, kurtarma planına karşılık gelen komut dosyaları ekleyerek DNS girişini güncelleyebilir, site bağlamayı değiştirebilir veya bağlantı dizesini değiştirebilirsiniz. [Kurtarma planına bir VMM komut dosyası ekleme,](site-recovery-how-to-add-vmmscript.md) komut dosyası kullanarak otomatik görevlerin nasıl ayarlanır olduğunu açıklar.

#### <a name="dns-update"></a>DNS güncelleştirmesi
Dinamik DNS güncelleştirmesi için DNS yapılandırılırsa, sanal makineler genellikle başladıklarında DNS'yi yeni IP adresiyle günceller. DNS'yi sanal makinelerin yeni IP adresleriyle güncelleştirmek için açık bir adım eklemek istiyorsanız, Kurtarma planı gruplarında başarısız lık sonrası eylem olarak [DNS'deki IP'yi güncelleştirmek için](https://aka.ms/asr-dns-update) bir komut dosyası ekleyin.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Bir uygulamanın web.config'inde bağlantı dizesi
Bağlantı dizesi, web sitesinin iletişim kurduğu veritabanını belirtir. Bağlantı dizesi veritabanı sanal makinenin adını taşıyorsa, başarısız lık sonrası başka adımlar aranmadı. Uygulama veritabanı ile otomatik olarak iletişim kurabilir. Ayrıca, veritabanı sanal makine için IP adresi korunursa, bağlantı dizesini güncelleştirmek gerekmez. 

Bağlantı dizesi bir IP adresi kullanarak veritabanı sanal makineye başvuruyorsa, hata sonrası güncelleştirilmelidir. Örneğin, aşağıdaki bağlantı dizesi IP adresi 127.0.1.2 ile veritabanına işaret ediyor:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Web katmanındaki bağlantı dizesini güncelleştirmek için kurtarma planında Grup 3'ten sonra bir [IIS bağlantı güncelleştirme komut dosyası](https://gallery.technet.microsoft.com/Update-IIS-connection-2579aadc) ekleyin.

#### <a name="site-bindings-for-the-application"></a>Uygulama için site bağlamaları
Her site bağlayıcı bilgilerden oluşur. Bağlama bilgileri, bağlama türünü, IIS sunucusunun site isteklerini dinlediği IP adresini, bağlantı noktası numarasını ve sitenin ana bilgisayar adlarını içerir. Bu hata sırasında, IP adresinde bunlarla ilişkili bir değişiklik varsa, bu bağlamaları güncelleştirmeniz gerekebilir.

> [!NOTE]
>
> Siteyi **tüm atanmamışlara**bağlamayı ayarlarsanız, bu bağlama sonrası başarısızlığı güncelleştirmeniz gerekmez. Ayrıca, bir siteyle ilişkili IP adresi başarısız lık sonrası değiştirilmezse, site bağlamayı güncelleştirmeniz gerekmez. (IP adresinin tutulması, birincil ve kurtarma sitelerine atanan ağ mimarisine ve alt ağlara bağlıdır. Bunları güncelleştirmek kuruluşunuz için uygun olmayabilir.)

![SSL bağlamayı ayarladığını gösteren ekran görüntüsü](./media/site-recovery-iis/sslbinding.png)

IP adresini bir siteyle ilişkilendirdiyseniz, tüm site bağlamalarını yeni IP adresiyle güncelleştirin. Site bağlamalarını değiştirmek için, kurtarma planında Grup 3'ten sonra bir [IIS web katmanı güncelleştirme komut dosyası](https://aka.ms/asr-web-tier-update-runbook-classic) ekleyin.

#### <a name="update-the-load-balancer-ip-address"></a>Yük bakiyesi IP adresini güncelleştirme
IP adresini güncelleştirmek için bir ARR sanal makineniz varsa, Grup 4'ten sonra bir [IIS ARR failover komut dosyası](https://aka.ms/asr-iis-arrtier-failover-script-classic) ekleyin.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>HTTPS bağlantısı için SSL sertifikası bağlama
Bir web sitesinin, web sunucusu ve kullanıcının tarayıcısı arasında güvenli bir iletişim sağlamaya yardımcı olan ilişkili bir SSL sertifikası olabilir. Web sitesinin bir HTTPS bağlantısı varsa ve ayrıca SSL sertifikası bağlayıcısı olan IIS sunucusunun IP adresine bağlanan ilişkili bir HTTPS sitesi varsa, sertifika için IIS sanal makinenin ip adresine sahip yeni bir site eklemeniz gerekir.

SSL sertifikası bu bileşenlere karşı verilebilir:

* Web sitesinin tam nitelikli etki alanı adı.
* Sunucunun adı.
* Etki alanı adı için joker karakter sertifikası.  
* Bir IP adresi. SSL sertifikası IIS sunucusunun IP adresine karşı verilmişse, Azure sitesindeki IIS sunucusunun IP adresine karşı başka bir SSL sertifikası verilmesi gerekir. Bu sertifika için ek bir SSL bağlama oluşturulması gerekir. Bu nedenle, IP adresine karşı verilen bir SSL sertifikası kullanmamanızı öneririz. Bu seçenek daha az yaygın olarak kullanılır ve yakında yeni sertifika yetkilisi/tarayıcı forumu değişikliklerine uygun olarak amortismana alınır.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>Web katmanı ve uygulama katmanı arasındaki bağımlılığı güncelleştirme
Sanal makinelerin IP adresini temel alan uygulamaya özgü bir bağımlılığınız varsa, bu bağımlılık sonrası başarısızlığı güncelleştirmeniz gerekir.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. Azure portalında Kurtarma Hizmetleri kasanızı seçin.
2. IIS web çiftliği için oluşturduğunuz kurtarma planını seçin.
3. **Yük Devretme Testi**'ni seçin.
4. Test başarısız lık işlemini başlatmak için kurtarma noktasını ve Azure sanal ağını seçin.
5. İkincil ortam dolduğunda, doğrulamalar gerçekleştirebilirsiniz.
6. Doğrulamalar tamamlandığında, test başarısız ortamını temizlemek için **Doğrulamaları tamamla'yı**seçin.

Daha fazla bilgi için [bkz.](site-recovery-test-failover-to-azure.md)

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1. Azure portalında Kurtarma Hizmetleri kasanızı seçin.
1. IIS web çiftliği için oluşturduğunuz kurtarma planını seçin.
1. **Yük devretme**'yi seçin.
1. Başarısız işlemi başlatmak için kurtarma noktasını seçin.

Daha fazla bilgi için Site [Kurtarma'da Failover'a](site-recovery-failover.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Site Kurtarma'yı kullanarak [diğer uygulamaları çoğaltma](site-recovery-workload.md) hakkında daha fazla bilgi edinin.
