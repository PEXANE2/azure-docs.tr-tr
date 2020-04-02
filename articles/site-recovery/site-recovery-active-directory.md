---
title: Azure Site Kurtarma ile Etkin Dizin/DNS olağanüstü durum kurtarma yı ayarlama
description: Bu makalede, Azure Site Kurtarma ile Active Directory ve DNS için olağanüstü durum kurtarma çözümnasıl uygulanacağı açıklanmaktadır.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: mayg
ms.openlocfilehash: 2cf4f22be2a4407d73fcc7bb340fad647c8aa145
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546511"
---
# <a name="set-up-disaster-recovery-for-active-directory-and-dns"></a>Active Directory ve DNS için olağanüstü durum kurtarma yı ayarlama

SharePoint, Dynamics AX ve SAP gibi kurumsal uygulamalar, doğru çalışması için Active Directory ve DNS altyapısına bağlıdır. Uygulamalar için olağanüstü durum kurtarma yı ayarladığınızda, doğru uygulama işlevselliğini sağlamak için diğer uygulama bileşenlerini kurtarmadan önce genellikle Etkin Dizin ve Etki Alanı Adı Sistemi'ni (DNS) kurtarmanız gerekir.

Etkin Dizin için bir olağanüstü durum kurtarma planı oluşturmak için [Site Kurtarma'yı](site-recovery-overview.md) kullanabilirsiniz. Bir aksaklık meydana geldiğinde, bir hata başlatabilirsiniz. Birkaç dakika içinde Active Directory’niz çalışır duruma gelebilir. Birincil sitenizde (örneğin SharePoint ve SAP için) birden çok uygulama için Active Directory'yi dağıttıysanız, sitenin tamamında başarısız olmak isteyebilirsiniz. Önce Site Kurtarma'yı kullanarak Active Directory üzerinde başarısız olabilirsiniz. Ardından, uygulamaya özgü kurtarma planlarını kullanarak diğer uygulamalarda başarısız olun.

Bu makalede, Active Directory için bir olağanüstü durum kurtarma çözümü oluşturmak için nasıl açıklanmaktadır. Ön koşullar ve başarısız talimatlar içerir. Başlamadan önce Active Directory ve Site Kurtarma'ya aşina olmalısınız.

## <a name="prerequisites"></a>Ön koşullar

- Azure'da çoğalıyorsanız, abonelik, Azure Sanal Ağı, depolama hesabı ve Kurtarma Hizmetleri kasası dahil olmak üzere [Azure kaynakları hazırlayın.](tutorial-prepare-azure.md)
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-azure.md) gözden geçirin.

## <a name="replicate-the-domain-controller"></a>Etki alanı denetleyicisini çoğaltma

- Etki alanı denetleyicisi veya DNS barındıran en az bir sanal makinede (VM) Site Kurtarma çoğaltmasını ayarlamanız gerekir.
- Ortamınızda birden çok etki alanı denetleyicisi varsa, hedef sitede ek bir etki alanı denetleyicisi ayarlamanız gerekir. Ek etki alanı denetleyicisi Azure'da veya ikincil bir şirket içi veri merkezinde olabilir.
- Yalnızca birkaç uygulamanız ve bir etki alanı denetleyiciniz varsa, tüm site üzerinde birlikte başarısız olmak isteyebilirsiniz. Bu durumda, etki alanı denetleyicisini Azure'da veya ikincil bir şirket içi veri merkezinde hedef siteye çoğaltmak için Site Kurtarma'yı kullanmanızı öneririz. Test [başarısız](#test-failover-considerations)olmak için aynı çoğaltılan etki alanı denetleyicisini veya DNS sanal makinesini kullanabilirsiniz.
- Ortamınızda çok sayıda uygulama ve birden fazla etki alanı denetleyicisi varsa veya aynı anda birkaç uygulama üzerinde başarısız olmayı planlıyorsanız, etki alanı denetleyicisini Site Kurtarma ile çoğaltmanın yanı sıra, hedef sitede (Azure'da veya ikincil bir şirket içi veri merkezinde) ek bir denetleyici kurmanızı öneririz. [Test başarısız olması](#test-failover-considerations)için, Site Kurtarma tarafından çoğaltılan bir etki alanı denetleyicisi kullanabilirsiniz. Başarısız olmak için, hedef sitedeki ek etki alanı denetleyicisini kullanabilirsiniz.

## <a name="enable-protection-with-site-recovery"></a>Site Kurtarma ile koruma sağlama

Etki alanı denetleyicisini veya DNS'yi barındıran sanal makineyi korumak için Site Kurtarma'yı kullanabilirsiniz.

### <a name="protect-the-vm"></a>VM'yi koruyun

Site Kurtarma kullanılarak çoğaltılan etki alanı [denetleyicisi, test başarısız olmak](#test-failover-considerations)için kullanılır. Aşağıdaki gereksinimleri karşıladığından emin olun:

1. Etki alanı denetleyicisi küresel bir katalog sunucusudur.
1. Etki alanı denetleyicisi, bir test başarısızlığı sırasında gereken roller için Esnek Tek Ana İşlemler (FSMO) rol sahibi olmalıdır. Aksi takdirde, bu rollerin başarısız olduktan sonra [ele geçirilmesi](https://support.microsoft.com/help/255504/using-ntdsutil-exe-to-transfer-or-seize-fsmo-roles-to-a-domain-control) gerekecektir.

### <a name="configure-vm-network-settings"></a>VM ağ ayarlarını yapılandırma

Site Kurtarma'da etki alanı denetleyicisini veya DNS'yi barındıran sanal makine için, yinelenen sanal makinenin **Bilgi İşlem ve Ağ** ayarları altında ağ ayarlarını yapılandırın. Bu, sanal makinenin başarısız olduktan sonra doğru ağa bağlı olmasını sağlar.

## <a name="protect-active-directory"></a>Aktif Dizini Koru

### <a name="site-to-site-protection"></a>Siteden siteye koruma

İkincil sitede bir etki alanı denetleyicisi oluşturun. Sunucuyu etki alanı denetleyicirolüne terfi ettirdiğinizde, birincil sitede kullanılan aynı etki alanının adını belirtin. Sitelerin eklendiği site bağlantı nesnesindeki ayarları yapılandırmak için **Active Directory Sites and Services** snap-in'i kullanabilirsiniz. Bir site bağlantısındaki ayarları yapılandırarak, çoğaltmanın iki veya daha fazla site arasında ne zaman oluştuğunu ve ne sıklıkta oluştuğunu denetleyebilirsiniz. Daha fazla bilgi için [bkz.](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731862(v=ws.11))

### <a name="site-to-azure-protection"></a>Siteden Azure'a koruma

İlk olarak, Azure sanal ağında bir etki alanı denetleyicisi oluşturun. Sunucuyu etki alanı denetleyicirolüne terfi ettirdiğinizde, birincil sitede kullanılan etki alanı adını belirtin.

Ardından, Sanal ağ için DNS sunucusunu Azure'daki DNS sunucusunu kullanacak şekilde yeniden yapılandırın.

:::image type="content" source="./media/site-recovery-active-directory/azure-network.png" alt-text="Azure Ağı":::

### <a name="azure-to-azure-protection"></a>Azure'dan Azure'a koruma

İlk olarak, Azure sanal ağında bir etki alanı denetleyicisi oluşturun. Sunucuyu etki alanı denetleyicirolüne terfi ettirdiğinizde, birincil sitede kullanılan etki alanı adını belirtin.

Ardından, Sanal ağ için DNS sunucusunu Azure'daki DNS sunucusunu kullanacak şekilde yeniden yapılandırın.

## <a name="test-failover-considerations"></a>Test başarısız değerlendirmeleri

Üretim iş yüklerini etkilemeyi önlemek için, test başarısız olması üretim ağından yalıtılmış bir ağda gerçekleşir.

Çoğu uygulama, bir etki alanı denetleyicisi veya DNS sunucusunun bulunmasını gerektirir. Bu nedenle, uygulama üzerinde başarısız olmadan önce, yalıtılmış ağda test failover için kullanılacak bir etki alanı denetleyicisi oluşturmanız gerekir. Bunu yapmanın en kolay yolu, etki alanı denetleyicisi veya DNS barındıran sanal bir makineyi çoğaltmak için Site Kurtarma'yı kullanmaktır. Ardından, uygulama için kurtarma planının bir test başarısızlığını çalıştırmadan önce etki alanı denetleyicisanal makinesinin bir test başarısızlığını çalıştırın.

1. Etki alanı denetleyicisini veya DNS'yi barındıran sanal makineyi [çoğaltmak](vmware-azure-tutorial.md) için Site Kurtarma'yı kullanın.
1. Yalıtılmış bir ağ oluşturun. Azure'da oluşturduğunuz tüm sanal ağlar varsayılan olarak diğer ağlardan yalıtılır. Üretim ağınızda kullandığınız bu ağ için aynı IP adresi aralığını kullanmanızı öneririz. Bu ağda siteden siteye bağlantı sağlamayı etkinleştirme.
1. Yalıtılmış ağda bir DNS IP adresi sağlayın. DNS sanal makinenin olmasını beklediğiniz IP adresini kullanın. Azure'da çoğalıyorsanız, başarısız lıkta kullanılan sanal makinenin IP adresini sağlayın. IP adresini girmek için, çoğaltılan sanal makineye, **Bilgi İşlem ve Ağ** ayarlarına Hedef **IP** ayarlarını seçin.

   :::image type="content" source="./media/site-recovery-active-directory/azure-test-network.png" alt-text="Azure test ağı":::

   > [!TIP]
   > Site Kurtarma, aynı adı taşıyan bir alt ağda ve sanal makinenin **Bilgi İşlem ve Ağ** ayarlarında sağlanan aynı IP adresini kullanarak test sanal makineleri oluşturmaya çalışır. Test başarısızlığı için sağlanan Azure sanal ağında aynı adı taşıyan bir alt ağ kullanılamıyorsa, test sanal makinesi alfabetik olarak ilk alt ağda oluşturulur.
   >
   > Hedef IP adresi seçili alt ağın bir parçasıysa, Site Kurtarma hedef IP adresini kullanarak sanal makine üzerinden test başarısızlığı oluşturmaya çalışır. Hedef IP seçili alt ağın bir parçası değilse, sanal makine üzerinden test başarısız seçili alt netteki bir sonraki kullanılabilir IP kullanılarak oluşturulur.

### <a name="test-failover-to-a-secondary-site"></a>İkincil bir siteye test başarısız

1. Başka bir şirket içi siteye çoğalıyorsanız ve DHCP kullanıyorsanız, [test başarısızlığı için DNS ve DHCP'yi ayarlayın.](hyper-v-vmm-test-failover.md#prepare-dhcp)
1. Yalıtılmış ağda çalışan etki alanı denetleyicisanal makinenin bir test başarısızlığı yapın. Test başarısızlığı yapmak için etki alanı denetleyicisanal makinenin en son kullanılabilir _uygulama tutarlı_ kurtarma noktasını kullanın.
1. Uygulamanın çalıştığı sanal makineleri içeren kurtarma planı için bir test başarısızlığını çalıştırın.
1. Sınama tamamlandığında, etki alanı denetleyicisanal makineüzerinde _test failover temizleyin._ Bu adım, test başarısız olması için oluşturulan etki alanı denetleyicisini siler.

### <a name="remove-references-to-other-domain-controllers"></a>Diğer etki alanı denetleyicilerine yapılan başvuruları kaldırma

Bir test başarısızı başlattığınızda, test ağına tüm etki alanı denetleyicilerini eklemeyin. Üretim ortamınızda bulunan diğer etki alanı denetleyicilerine yapılan başvuruları kaldırmak için [FSMO Active Directory rollerini ele geçirmeniz](https://support.microsoft.com/help/255504/using-ntdsutil-exe-to-transfer-or-seize-fsmo-roles-to-a-domain-control) ve eksik etki alanı denetleyicileri için [meta veri temizleme](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816907(v=ws.10)) yapmanız gerekebilir.

### <a name="issues-caused-by-virtualization-safeguards"></a>Sanallaştırma güvenlik önlemlerinin neden olduğu sorunlar

> [!IMPORTANT]
> Bu bölümde açıklanan yapılandırmaların bazıları standart veya varsayılan etki alanı denetleyicisi yapılandırmaları değildir. Bu değişiklikleri bir üretim etki alanı denetleyicisi olarak yapmak istemiyorsanız, test başarısızolmak için kullanmak üzere Site Kurtarma için ayrılmış bir etki alanı denetleyicisi oluşturabilirsiniz. Bu değişiklikleri yalnızca bu etki alanı denetleyicisi için yapın.

Windows Server 2012'den başlayarak, [Active Directory Domain Services (AD DS) içine ek güvenlik önlemleri oluşturulur.](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) Bu korumalar, temel hipervizör platformu **VM-GenerationID'yi**destekliyorsa, sanallaştırılmış etki alanı denetleyicilerinin güncelleştirme sıra numarası (USN) geri almalarına karşı korunmasına yardımcı olur. Azure **VM-GenerationID'yi**destekler. Bu nedenle, Windows Server 2012 veya daha sonra Azure sanal makinelerinde çalıştıran etki alanı denetleyicileri bu ek güvenlik önlemlerine sahiptir.

**VM-GenerationID** sıfırlandığında, AD DS veritabanının **InvocationID** değeri de sıfırlanır. Ayrıca, göreli kimlik (RID) havuzu atılır `SYSVOL` ve klasör yetkili olmayan olarak işaretlenir. Daha fazla bilgi için bkz: [Active Directory Etki Alanı Hizmetlerine Giriş sanallaştırma](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) ve [Dağıtılmış Dosya Sistemi Çoğaltma (DFSR) sanallaştırma.](https://techcommunity.microsoft.com/t5/storage-at-microsoft/safely-virtualizing-dfsr/ba-p/424671)

Azure'un üzerinde başarısız olmak **VM-GenerationID'nin** sıfırlanmasına neden olabilir. **VM-GenerationID'yi** sıfırlamak, etki alanı denetleyicisanal makinesi Azure'da başlatıldığında ek güvenlik önlemleri tetikler. Bu, etki alanı denetleyicisanal makinede oturum açabilmekte önemli bir gecikmeye neden olabilir.

Bu etki alanı denetleyicisi yalnızca bir test başarısız olduğunda kullanıldığından, sanallaştırma korumaları gerekli değildir. Etki alanı denetleyicisanal makine için **VM-GenerationID** değerinin değişmemesini sağlamak için, şirket `DWORD` içi etki alanı denetleyicisinde aşağıdakilerin değerini **4** olarak değiştirebilirsiniz:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start`

#### <a name="symptoms-of-virtualization-safeguards"></a>Sanallaştırma korumalarının belirtileri

Sanallaştırma korumaları bir test başarısızlığından sonra tetiklenirse, aşağıdaki belirtilerden birini veya birkaçını görebilirsiniz:

- **GenerationID** değeri değişir:

  :::image type="content" source="./media/site-recovery-active-directory/Event2170.png" alt-text="Nesil Kimlik Değişikliği":::

- **InvocationID** değeri değişir:

  :::image type="content" source="./media/site-recovery-active-directory/Event1109.png" alt-text="İptal Kimlik Değişikliği":::

- `SYSVOL`klasör `NETLOGON` ve paylaşımlar kullanılamaz.

  :::image type="content" source="./media/site-recovery-active-directory/sysvolshare.png" alt-text="SYSVOL klasör paylaşımı":::

  :::image type="content" source="./media/site-recovery-active-directory/Event13565.png" alt-text="NtFrs SYSVOL klasörü":::

- DFSR veritabanları silinir.

  :::image type="content" source="./media/site-recovery-active-directory/Event2208.png" alt-text="DFSR veritabanları silinir":::

### <a name="troubleshoot-domain-controller-issues-during-test-failover"></a>Test başarısızmı sırasında etki alanı denetleyicisi sorunlarını giderme

> [!IMPORTANT]
> Bu bölümde açıklanan yapılandırmaların bazıları standart veya varsayılan etki alanı denetleyiciyapılandırmaları değildir. Bu değişiklikleri bir üretim etki alanı denetleyicisi için yapmak istemiyorsanız, Site Kurtarma testi nin başarısız olması için ayrılmış bir etki alanı denetleyicisi oluşturabilirsiniz. Değişiklikleri yalnızca bu özel etki alanı denetleyicisi için yapın.

1. Komut isteminde, klasör ve `SYSVOL` `NETLOGON` klasörün paylaşılıp paylaşılmadığını denetlemek için aşağıdaki komutu çalıştırın:

    `NET SHARE`

1. Komut isteminde, etki alanı denetleyicisinin düzgün çalıştığından emin olmak için aşağıdaki komutu çalıştırın:

    `dcdiag /v > dcdiag.txt`

1. Çıktı günlüğünde aşağıdaki metni arayın. Metin, etki alanı denetleyicisinin doğru çalıştığını doğrular.

    - `passed test Connectivity`
    - `passed test Advertising`
    - `passed test MachineAccount`

Önceki koşullar karşılanırsa, etki alanı denetleyicisinin düzgün çalışması olasıdır. Değilse, aşağıdaki adımları tamamlayın:

1. Etki alanı denetleyicisinin yetkili bir geri yüklemesini yapın. Aşağıdaki bilgileri aklınızda bulundurun:

    - [Dosya Çoğaltma Hizmeti 'ni (FRS)](https://techcommunity.microsoft.com/t5/storage-at-microsoft/the-end-is-nigh-for-frs-8211-updated-for-ws2016/ba-p/425379)kullanarak çoğaltma önermesek de, FRS çoğaltma kullanıyorsanız, yetkili geri yükleme için adımları izleyin. İşlem, Dosya [Çoğaltma Hizmeti'ni yeniden başlatmayı sağlamak için BurFlags kayıt defteri anahtarını kullanarak](https://support.microsoft.com/kb/290762)açıklanmıştır.

      BurFlags hakkında daha fazla bilgi için, blog yazısı [D2 ve D4 bakın: Ne için?](/archive/blogs/janelewis/d2-and-d4-what-is-it-for).

    - DFSR çoğaltma kullanıyorsanız, yetkili bir geri yükleme için adımları tamamlayın. Bu [işlem, DFSR tarafından çoğaltılan SYSVOL klasörü için yetkili ve yetkili olmayan bir senkronizasyon (FRS için "D4/D2" gibi) Kuvvet'te](https://support.microsoft.com/kb/2218556)açıklanmıştır.

      PowerShell işlevlerini de kullanabilirsiniz. Daha fazla bilgi için [dfsr-SYSVOL yetkili/yetkili olmayan geri yükleme PowerShell işlevlerine](/archive/blogs/thbouche/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions)bakın.

1. Şirket içi etki alanı denetleyicisinde aşağıdaki kayıt defteri anahtarını **0** olarak ayarlayarak ilk eşitleme gereksinimini atla. `DWORD` Yoksa, **Parametreler** düğümü altında oluşturabilirsiniz.

   `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations`

   Daha fazla bilgi için [Bkz. Sorun Giderme DNS Olay Kimliği 4013: DNS sunucusu AD tümleşik DNS bölgelerini yükleyemedi.](https://support.microsoft.com/kb/2001093)

1. Kullanıcı oturum açmayı doğrulamak için genel katalog sunucusunun kullanılabilir olması gereksinimini devre dışı kaldırın. Bunu yapmak için, şirket içi etki alanı denetleyicisinde, aşağıdaki kayıt defteri anahtarını **1**olarak ayarlayın. `DWORD` Yoksa, **Lsa** düğümü altında oluşturabilirsiniz.

   `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures`

   Daha fazla bilgi [için, Genel Katalog'un Nasıl Çalıştığını](/previous-versions/windows/it-pro/windows-server-2003/cc737410(v=ws.10))görün.

### <a name="dns-and-domain-controller-on-different-machines"></a>Farklı makinelerde DNS ve etki alanı denetleyicisi

Etki alanı denetleyicisini ve DN'leri aynı VM üzerinde çalıştırıyorsanız, bu yordamı atlayabilirsiniz.

DNS etki alanı denetleyicisi ile aynı VM'de değilse, test başarısız olması için bir DNS VM oluşturmanız gerekir. Yeni bir DNS sunucusu kullanabilir ve gerekli tüm bölgeleri oluşturabilirsiniz. Örneğin, Etkin Dizin etki `contoso.com`alanınız , adı `contoso.com`olan bir DNS bölgesi oluşturabilirsiniz. Etkin Dizin'e karşılık gelen girişler DNS'de aşağıdaki gibi güncelleştirilmelidir:

1. Kurtarma planındaki diğer sanal makineler başlamadan önce bu ayarların yerinde olduğundan emin olun:

   - Bölge, orman kökü adından sonra adlandırılmalıdır.
   - Bölge dosya destekli olmalıdır.
   - Güvenli ve güvenli olmayan güncelleştirmeler için bölge etkinleştirilmelidir.
   - Etki alanı denetleyicisini barındıran sanal makinenin çözümleyicisi, DNS sanal makinenin IP adresini işaret etmelidir.

1. Etki alanı denetleyicisini barındıran VM'de aşağıdaki komutu çalıştırın:

   `nltest /dsregdns`

1. DNS sunucusuna bölge eklemek, güvenli olmayan güncelleştirmelere izin vermek ve bölge için DNS'ye bir giriş eklemek için aşağıdaki komutları çalıştırın:

   ```cmd
   dnscmd /zoneadd contoso.com  /Primary

   dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1

   dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>

   dnscmd /config contoso.com /allowupdate 1
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure Site Kurtarma ile kurumsal iş yüklerini koruma hakkında [daha fazla bilgi edinin.](site-recovery-workload.md)
