---
title: Azure Site Kurtarma ile ikincil bir siteye NHyper-V olağanüstü durum kurtarma matkabı çalıştırma
description: VMM bulutlarında Hyper-V VM'ler için Azure Site Kurtarma'yı kullanarak ikincil bir şirket içi veri merkezine dr matkabını nasıl çalıştırılayarıştırıyarız öğrenin.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 0363911574a076b13cb72591fb2564364e096c76
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257972"
---
# <a name="run-a-dr-drill-for-hyper-v-vms-to-a-secondary-site"></a>Hyper-V VM'ler için ikincil bir siteye DR matkabı çalıştırma


Bu makalede, Sistem Merkezi Sanal Makine Yöneticisi V(MM) bulutlarında yönetilen Hyper-V VM'ler için Azure [Site Kurtarma](site-recovery-overview.md)kullanarak ikincil bir şirket içi siteye nasıl olağanüstü durum kurtarma (DR) demlemesi yapılacağını açıklar.

Çoğaltma stratejinizi doğrulamak için bir test hatası çalıştırın ve herhangi bir veri kaybı veya kapalı kalma süresi olmadan bir DR delme gerçekleştirin. Bir test başarısız mısrama devam eden çoğaltma veya üretim ortamı üzerinde herhangi bir etkisi yoktur. 

## <a name="how-do-test-failovers-work"></a>Test başarısızları nasıl çalışır?

Birincil siteden ikincil siteye bir test başarısızlığını çalıştırın. Sadece bir VM üzerinde başarısız olup olmadığını kontrol etmek istiyorsanız, ikincil sitede bir şey ayarlamadan bir test failover çalıştırabilirsiniz. Uygulamanın başarısız olduğunu doğrulamak istiyorsanız, ikincil konumda ağ ve altyapı ayarlamanız gerekir.
- Tek bir VM'de veya [kurtarma planında](site-recovery-create-recovery-plans.md)bir test başarısızlığını çalıştırabilirsiniz.
- Bir test başarısızlığını ağ olmadan, varolan bir ağla veya otomatik olarak oluşturulan bir ağla çalıştırabilirsiniz. Bu seçenekler hakkında daha fazla bilgi aşağıdaki tabloda verilmiştir.
    - Bir ağ olmadan bir test failover çalıştırabilirsiniz. Bu seçenek, bir VM'nin başarısız olup olmadığını kontrol etmek istiyorsanız, ancak herhangi bir ağ yapılandırmasını doğrulayamazsınız.
    - Failover'ı varolan bir ağla çalıştırın. Üretim ağı kullanmamanızı öneririz.
    - Başarısız ı çalıştırın ve Site Kurtarma'nın otomatik olarak bir test ağı oluşturmasına izin verin. Bu durumda Site Kurtarma ağı otomatik olarak oluşturur ve test başarısız olduğunda temizler.
- Test başarısız olması için bir kurtarma noktası seçmeniz gerekir: 
    - **En son işlenen**: Bu seçenek, Site Kurtarma tarafından işlenen en son kurtarma noktasına bir VM başarısız olur. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
    - **En son uygulama tutarlılığı**: Bu seçenek, Site Kurtarma tarafından işlenen en son uygulama tutarlı kurtarma noktasına vm üzerinden başarısız. 
    - **En Son**: Bu seçenek, her VM için bir kurtarma noktası oluşturmak için önce Site Kurtarma hizmetine gönderilen tüm verileri işler. Bu seçenek en düşük RPO'yu (Kurtarma Noktası Hedefi) sağlar, çünkü başarısız olduktan sonra oluşturulan VM, başarısız lık tetiklendiğinde tüm verileri Site Kurtarma'ya çoğaltılır.
    - **En son çoklu VM işlenmiş**: Çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM içeren kurtarma planları için kullanılabilir. Ayarı etkin olan VM'ler en son ortak multi-VM tutarlı kurtarma noktasına geçemez. Diğer VM'ler en son işlenmiş kurtarma noktasına geçemez.
    - **En son multi-VM uygulaması tutarlılığı**: Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM'li kurtarma planları için kullanılabilir. Çoğaltma grubunun bir parçası olan VM'ler en son ortak çoklu VM uygulama tutarlı kurtarma noktasına geçemez. Diğer VM'ler en son uygulama tutarlı kurtarma noktasına geçemedi.
    - **Özel**: Belirli bir VM üzerinde belirli bir kurtarma noktasına başarısız olmak için bu seçeneği kullanın.



## <a name="prepare-networking"></a>Ağ hazırlama

Bir test başarısızlığını çalıştırdığınızda, tabloda özetlendiği gibi test çoğaltma makineleri için ağ ayarlarını seçmeniz istenir.

| **Seçeneği** | **Şey** | |
| --- | --- | --- |
| **Yok** | Test VM çoğaltma VM bulunduğu ana bilgisayarüzerinde oluşturulur. Buluta eklenmez ve herhangi bir ağa bağlı değildir.<br/><br/> Makineyi oluşturulduktan sonra bir VM ağına bağlayabilirsiniz.| |
| **Varolan kullanımı** | Test VM çoğaltma VM bulunduğu ana bilgisayarüzerinde oluşturulur. Buluta eklenmez.<br/><br/>Üretim ağınızdan izole edilmiş bir VM ağı oluşturun.<br/><br/>VLAN tabanlı bir ağ kullanıyorsanız, bu amaçla VMM'de ayrı bir mantıksal ağ (üretimde kullanılmaz) oluşturmanızı öneririz. Bu mantıksal ağ, test başarısızları için VM ağları oluşturmak için kullanılır.<br/><br/>Mantıksal ağ, sanal makineler barındıran tüm Hyper-V sunucularının ağ bağdaştırıcılarından en az biriyle ilişkilendirilmelidir.<br/><br/>VLAN mantıksal ağlar için, mantıksal ağa eklediğiniz ağ siteleri yalıtılmalıdır.<br/><br/>Windows Ağı Sanallaştırma tabanlı mantıksal ağ kullanıyorsanız, Azure Site Kurtarma otomatik olarak yalıtılmış VM ağları oluşturur. | |
| **Ağ oluşturma** | **Mantıksal Ağ'da** ve ilgili ağ sitelerinde belirttiğiniz ayara göre otomatik olarak geçici bir test ağı oluşturulur.<br/><br/> Failover, VM'lerin oluşturulduğunu denetler.<br/><br/> Kurtarma planı birden fazla VM ağı kullanıyorsa, bu seçeneği kullanmalısınız.<br/><br/> Windows Ağı Sanallaştırma ağlarını kullanıyorsanız, bu seçenek otomatik olarak çoğaltma sanal makinenin ağında aynı ayarlara (alt ağlar ve IP adresi havuzları) sahip VM ağları oluşturabilir. Bu VM ağları, test başarısızmı tamamlandıktan sonra otomatik olarak temizlenir.<br/><br/> Test VM çoğaltma sanal makine nin bulunduğu ana bilgisayarda oluşturulur. Buluta eklenmez.|

### <a name="best-practices"></a>En iyi uygulamalar

- Üretim ağını sınaması, üretim iş yüklerinin kapalı kalma süresine neden olur. Olağanüstü durum kurtarma tatbikatı devam ederken kullanıcılarınızdan ilgili uygulamaları kullanmamalarını isteyin.

- Test ağının, test hatası için kullanılan VMM mantıksal ağ türüyle eşleşmesi gerekmez. Ancak, bazı kombinasyonlar işe yaramazsa:

     - Yineleme DHCP ve VLAN tabanlı yalıtım kullanıyorsa, yineleme için VM ağı statik bir IP adresi havuzu gerekmez. Bu nedenle, adres havuzu bulunmadığından, test başarısız olması için Windows Ağı Sanallaştırması'nı kullanmak çalışmaz. 
        
     - Yineleme ağı yalıtım kullanmıyorsa ve test ağı Windows Ağı Sanallaştırma'yı kullanıyorsa, sınama başarısızlığı çalışmaz. Bunun nedeni, yalıtımsız ağın Bir Windows Ağı Sanallaştırma ağı oluşturmak için gereken alt ağlara sahip olmamasıdır.
        
- Ağ eşleme için seçtiğiniz ağı test başarısız olmak için kullanmamanızı öneririz.

- Çoğaltma sanal makinelerinin failover'dan sonra eşlenen VM ağlarına nasıl bağlandığını VM ağının VMM konsolunda nasıl yapılandırıldığına bağlıdır.


### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>VM ağı yalıtım veya VLAN yalıtımı olmadan yapılandırıldı

VM ağı VMM'de yalıtım veya VLAN yalıtımı olmadan yapılandırılırsa, aşağıdakileri unutmayın:

- VM ağı için DHCP tanımlanırsa, yineleme sanal makine ilişkili mantıksal ağdaki ağ sitesi için belirtilen ayarlar aracılığıyla VLAN Kimliği'ne bağlanır. Sanal makine IP adresini kullanılabilir DHCP sunucusundan alır.
- Hedef VM ağı için statik bir IP adresi havuzu tanımlamanız gerekmez. VM ağı için statik bir IP adresi havuzu kullanılıyorsa, yineleme sanal makinesi ilişkili mantıksal ağdaki ağ sitesi için belirtilen ayarlar aracılığıyla VLAN Kimliği'ne bağlanır.
- Sanal makine IP adresini VM ağı için tanımlanan havuzdan alır. Hedef VM ağında statik bir IP adresi havuzu tanımlanmamışsa, IP adresi ayırma başarısız olur. Koruma ve kurtarma için kullanacağınız kaynak ve hedef VMM sunucularında IP adresi havuzunu oluşturun.

### <a name="vm-network-with-windows-network-virtualization"></a>Windows Network Sanallaştırma ile VM ağı

VM ağı VMM'de Windows Network Virtualization ile yapılandırılırsa, aşağıdakileri unutmayın:

- Kaynak VM ağının DHCP veya statik IP adresi havuzu kullanacak şekilde yapılandırılıp yapılandırılmadığına bakılmaksızın, hedef VM ağı için statik bir havuz tanımlamalısınız. 
- DHCP'yi tanımlarsanız, hedef VMM sunucusu BIR DHCP sunucusu gibi davranır ve hedef VM ağı için tanımlanan havuzdan bir IP adresi sağlar.
- Kaynak sunucu için statik BIR IP adresi havuzu kullanılıyorsa, hedef VMM sunucusu havuzdan bir IP adresi ayırır. Statik IP adresi havuzu tanımlanmamışsa, her iki durumda da IP adresi ayırma başarısız olur.



## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama

Bir VM'nin başarısız olup olmadığını kontrol etmek istiyorsanız, altyapı olmadan bir test hatası çalıştırabilirsiniz. Uygulamanın başarısız olmasını test etmek için tam bir DR tatbikatı yapmak istiyorsanız, ikincil sitede altyapıyı hazırlamanız gerekir:

- Varolan bir ağı kullanarak bir test başarısızlığı çalıştırıyorsanız, bu ağda Active Directory, DHCP ve DNS'yi hazırlayın.
- Otomatik olarak bir VM ağı oluşturma seçeneğiyle bir test başarısızlığını çalıştırAcaksanız, test başarısızlığını çalıştırmadan önce otomatik olarak oluşturulan ağa altyapı kaynakları eklemeniz gerekir. Kurtarma planında, test başarısız olması için kullanacağınız kurtarma planında Grup-1'den önce el ile bir adım ekleyerek bunu kolaylaştırabilirsiniz. Ardından, test başarısızlığını çalıştırmadan önce otomatik olarak oluşturulan ağa altyapı kaynaklarını ekleyin.


### <a name="prepare-dhcp"></a>DHCP'yi hazırla
Test başarısızlığı ile ilgili sanal makineler DHCP kullanıyorsa, test başarısızlığı amacıyla yalıtılmış ağ içinde bir test DHCP sunucusu oluşturun.


### <a name="prepare-active-directory"></a>Aktif Dizin Hazırla
Uygulama testi için bir test başarısızlığını çalıştırmak için, test ortamınızdaki üretim Active Directory ortamının bir kopyasına ihtiyacınız vardır. Daha fazla bilgi için, [Active Directory için test failover hususlar gözden geçirin.](site-recovery-active-directory.md#test-failover-considerations)

### <a name="prepare-dns"></a>DNS Hazırla
Aşağıdaki gibi test failover için bir DNS sunucusu hazırlayın:

* **DHCP**: Sanal makineler DHCP kullanıyorsa, test DNS'nin IP adresi test DHCP sunucusunda güncelleştirilmelidir. Windows Network Sanallaştırma ağ türünü kullanıyorsanız, VMM sunucusu DHCP sunucusu gibi davranır. Bu nedenle, DNS IP adresi test failover ağında güncelleştirilmelidir. Bu durumda, sanal makineler kendilerini ilgili DNS sunucusuna kaydeder.
* **Statik adres**: Sanal makineler statik bir IP adresi kullanıyorsa, test DNS sunucusunun IP adresi test failover ağında güncelleştirilmelidir. DNS'yi test sanal makinelerinin IP adresiyle güncelleştirmeniz gerekebilir. Bu amaçla aşağıdaki örnek komut dosyasını kullanabilirsiniz:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Bu yordam, kurtarma planı için bir test başarısızlığını nasıl çalıştırılabildiğini açıklar. Alternatif olarak, **Sanal Makineler** sekmesinde tek bir sanal makine için failover çalıştırabilirsiniz.

1. **Kurtarma Planları** > *recoveryplan_name'ni*seçin. **Failover** > **Test Failover'ı**tıklatın.
2. Test **Failover** bıçağında, yineleme VM'lerinin test başarısız olduktan sonra ağlara nasıl bağlanması gerektiğini belirtin.
3. **İşler** sekmesinde başarısız olan ilerlemeyi izleyin.
4. Başarısız lık tamamlandıktan sonra, VM'lerin başarılı bir şekilde başlatılanın.
5. İşi bittiğinde, kurtarma planında **Temizleme testi başarısız** olduğunu tıklatın. **Notlar**’da, yük devretme testiyle ilişkili gözlemlerinizi kaydedin ve saklayın. Bu adım, test başarısız olması sırasında Site Kurtarma tarafından oluşturulan tüm VM'leri ve ağları siler. 

![Yük devretme testi](./media/hyper-v-vmm-test-failover/TestFailover.png)
 


> [!TIP]
> Test başarısızmı sırasında sanal bir makineye verilen IP adresi, sanal makinenin planlı veya planlanmamış bir hata için alacağı IP adresidir (IP adresinin test başarısız ağında mevcut olduğu varsayılarak). Aynı IP adresi test başarısız ağda yoksa, sanal makine test failover ağında bulunan başka bir IP adresi alır.



### <a name="run-a-test-failover-to-a-production-network"></a>Üretim ağına test başarısızlığını çalıştırma

Ağ eşleme sırasında belirttiğiniz üretim kurtarma sitesi ağınızda bir test başarısızlığını çalıştırmamanızı öneririz. Ancak, başarısız bir VM'de uçtan uca ağ bağlantısını doğrulamanız gerekiyorsa, aşağıdaki noktalara dikkat edin:

* Test başarısızlığını yaparken birincil VM'nin kapatıldığından emin olun. Bunu yapmazsanız, aynı kimliğe sahip iki sanal makine aynı anda aynı ağda çalışıyor olur. Bu durum istenmeyen sonuçlara yol açabilir.
* Sanal makineler üzerinden test başarısız temizlenir zaman VM'ler üzerinde test başarısız yaptığınız herhangi bir değişiklik kaybolur. Bu değişiklikler birincil VM'lere çoğaltılamaz.
* Bu gibi testler, üretim uygulamanız için kapalı kalma süresine yol açar. DR matkabı devam ederken uygulamanın kullanıcılarından uygulamayı kullanmamalarını isteyin.  


## <a name="next-steps"></a>Sonraki adımlar
Başarılı bir DR matkap çalıştırdıktan sonra, [tam bir failover çalıştırabilirsiniz.](site-recovery-failover.md)



