---
title: Azure Site Kurtarma ile Dynamics AX'in olağanüstü kurtarma
description: Azure Site Kurtarma ile Dynamics AX için olağanüstü durum kurtarma yı nasıl ayarlayamanızı öğrenin
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 11/27/2018
ms.openlocfilehash: 0b32f00374aa8ce6c41415e28f319e3e7d5abddb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941584"
---
# <a name="set-up-disaster-recovery-for-a-multitier-dynamics-ax-application"></a>Çok katmanlı Dynamics AX uygulaması için olağanüstü durum kurtarma yı ayarlama   




 Dynamics AX, işletmeler tarafından konumlar daki süreçleri standartlaştırmak, kaynakları yönetmek ve uyumluluğu basitleştirmek için kullanılan en popüler ERP çözümlerinden biridir. Uygulama bir kuruluş için çok önemli olduğundan, bir felaket durumunda uygulamanın en az süre içinde çalışır durumda olması gerekir.

Bugün, Dynamics AX herhangi bir out-of-the-box olağanüstü durum kurtarma yetenekleri sağlamaz. Dynamics AX, Windows Application Object Server, Azure Active Directory, Azure SQL Database, SharePoint Server ve Reporting Services gibi birçok sunucu bileşeninden oluşur. Bu bileşenlerin her birinin olağanüstü durum kurtarma yönetmek için el ile sadece pahalı değil, aynı zamanda hata eğilimli.

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Dynamics AX uygulamanız için nasıl olağanüstü durum kurtarma çözümü oluşturabileceğiniz açıklanmaktadır. Ayrıca, tek tıklamayla kurtarma planı, desteklenen yapılandırmalar ve ön koşullar kullanarak planlanmış/planlanmamış test hatalarını da kapsar.



## <a name="prerequisites"></a>Ön koşullar

Site Kurtarma'yı kullanarak Dynamics AX uygulaması için olağanüstü durum kurtarma uygulaması aşağıdaki ön koşulları gerektirir:

• Şirket içi Dynamics AX dağıtımını ayarlayın.

• Azure aboneliğinde Site Kurtarma kasası oluşturun.

• Azure kurtarma sitenizse, Sanal Makinelere Hazırlık Değerlendirme aracını SANAL M'lerde çalıştırın. Azure Sanal Makineleri ve Site Kurtarma hizmetleriyle uyumlu olmalıdır.

## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makaleyi oluşturmak amacıyla, Windows Server 2012 R2 Enterprise dynamics AX 2012 R3 ile VMware sanal makineleri kullandık. Site Kurtarma çoğaltma uygulama agnostik olduğundan, burada sağlanan önerilerin aşağıdaki senaryolar için tutunmasını bekliyoruz.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil siteye** | **Azure'a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet

## <a name="enable-disaster-recovery-of-the-dynamics-ax-application-by-using-site-recovery"></a>Site Kurtarma'yı kullanarak Dynamics AX uygulamasının olağanüstü durum kurtarmasını etkinleştirin
### <a name="protect-your-dynamics-ax-application"></a>Dynamics AX uygulamanızı koruyun
Uygulama çoğaltma ve kurtarmanın tamamını etkinleştirmek için Dynamics AX'ın her bileşeninin korunması gerekir.

### <a name="1-set-up-active-directory-and-dns-replication"></a>1. Active Directory ve DNS çoğaltma sını ayarlama

Dynamics AX uygulamasının çalışması için olağanüstü durum kurtarma alanında Etkin Dizin gereklidir. Müşterinin şirket içi ortamının karmaşıklığına bağlı olarak aşağıdaki iki seçeneği öneririz.

**Seçenek 1**

Müşteri, tüm şirket içi site için az sayıda uygulamaya ve tek bir etki alanı denetleyicisine sahiptir ve tüm site boyunca birlikte başarısız olmayı planlamaktadır. Etki alanı denetleyici makinesini ikincil bir siteye çoğaltmak için Site Kurtarma çoğaltmaişlemini kullanmanızı öneririz (hem siteden siteye hem de siteden Azure'a senaryolar için geçerlidir).

**Seçenek 2**

Müşteri çok sayıda uygulamaya sahiptir ve bir Active Directory ormanı çalıştırıyor ve aynı anda birkaç uygulama üzerinde başarısız olmayı planlıyor. Olağanüstü durum kurtarma sitesinde (ikincil bir site veya Azure'da) ek bir etki alanı denetleyicisi ayarlamanızı öneririz.

 Daha fazla bilgi için [bkz.](site-recovery-active-directory.md) Bu belgenin geri kalanı için, olağanüstü durum kurtarma sitesinde bir etki alanı denetleyicisi olduğunu varsayıyoruz.

### <a name="2-set-up-sql-server-replication"></a>2. SQL Server çoğaltma sını ayarlama
SQL katmanını korumak için önerilen seçenek hakkında teknik kılavuz için [bkz.](site-recovery-sql.md)

### <a name="3-enable-protection-for-the-dynamics-ax-client-and-application-object-server-vms"></a>3. Dynamics AX istemcisi ve Uygulama Nesnesi Sunucusu VM'leri için koruma yı etkinleştirin
VM'lerin [Hyper-V](site-recovery-hyper-v-site-to-azure.md) veya [VMware'de](site-recovery-vmware-to-azure.md)dağıtılıp dağıtılmadığına bağlı olarak ilgili Site Kurtarma yapılandırmasını gerçekleştirin.

> [!TIP]
> Kilitlenme tutarlıfrekansını 15 dakikaolarak yapılandırmanızı öneririz.
>

Aşağıdaki anlık görüntü, Bir VMware siteden Azure'a koruma senaryosunda Dinamikbileşeni VM'lerin koruma durumunu gösterir.

![Korumalı öğeler](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Ağ yapılandırma
**VM bilgi işlem ve ağ ayarlarını yapılandırma**

Dynamics AX istemcisi ve Application Object Server VM'ler için, Site Kurtarma'daki ağ ayarlarını yapılandırın, böylece VM ağları başarısız olduktan sonra doğru olağanüstü durum kurtarma ağına iliştirilir. Bu katmanlar için olağanüstü durum kurtarma ağının SQL katmanına uygun olduğundan emin olun.

Aşağıdaki anlık görüntüde gösterildiği gibi, ağ ayarlarını yapılandırmak için çoğaltılan öğelerde VM'yi seçebilirsiniz:

* Application Object Server sunucuları için doğru kullanılabilirlik kümesini seçin.

* Statik bir IP kullanıyorsanız, VM'nin **Hedef IP** metin kutusuna almasını istediğiniz IP'yi belirtin.

    ![Ağ ayarları](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)


### <a name="5-create-a-recovery-plan"></a>5. Kurtarma planı oluşturma

Başarısız lık işlemini otomatikleştirmek için Site Kurtarma'da bir kurtarma planı oluşturabilirsiniz. Kurtarma planına bir uygulama katmanı ve web katmanı ekleyin. Ön uç uygulama katmanından önce kapanacak şekilde farklı gruplar halinde sipariş edin.

1. Aboneliğinizdeki Site Kurtarma kasasını ve **Kurtarma Planları** döşemesini seçin.

2. **+ Kurtarma planını**seçin ve bir ad belirtin.

3. **Kaynak** ve **Hedef'i**seçin. Hedef Azure veya ikincil bir site olabilir. Azure'u seçerseniz, dağıtım modelini belirtmeniz gerekir.

    ![Kurtarma planı oluşturma](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4. Kurtarma planı için Uygulama Nesnesi Sunucusu'nu ve istemci VM'lerini seçin ve ✓'yi seçin.

    ![Öğeleri seçin](./media/site-recovery-dynamics-ax/selectvms.png)

    Kurtarma planı örneği:

    ![Kurtarma planı ayrıntıları](./media/site-recovery-dynamics-ax/recoveryplan.png)

Dynamics AX uygulaması için kurtarma planını aşağıdaki adımları ekleyerek özelleştirebilirsiniz. Önceki anlık görüntü, tüm adımları ekledikten sonra tam kurtarma planını gösterir.


* **SQL Server failover adımları**: SQL sunucusuna özgü kurtarma adımları hakkında bilgi için [SQL Server ve Azure Site Kurtarma ile Çoğaltma uygulamalarına](site-recovery-sql.md)bakın.

* **Failover Grup 1**: Uygulama Nesnesi Sunucusu VM'leri üzerinde başarısız olur.
Seçilen kurtarma noktasının veritabanı PIT'e mümkün olduğunca yakın olduğundan, ancak bunun öncesinde olmadığından emin olun.

* **Komut Dosyası**: Yük dengeleyici (sadece E-A) ekleyin.
Uygulama Nesnesi Sunucusu VM grubu ortaya çıktıktan sonra bir komut dosyası (Azure Otomasyonu üzerinden) ekleyin ve buna bir yük dengeleyici ekleyin. Bu görevi yapmak için bir komut dosyası kullanabilirsiniz. Daha fazla bilgi için, [çok katmanlı uygulama felaket kurtarma için bir yük dengeleyici eklemek için nasıl](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)bakın.

* **Failover Group 2**: Dynamics AX istemcisi VM'leri üzerinde başarısız. Kurtarma planının bir parçası olarak web katmanı VM'ler üzerinde başarısız.


### <a name="perform-a-test-failover"></a>Bir test başarısız yapma

Test başarısız olması sırasında Active Directory'ye özgü daha fazla bilgi için "Active Directory olağanüstü durum kurtarma çözümü" yardımcı kılavuzuna bakın.

Test başarısızolması sırasında SQL sunucusuna özgü daha fazla bilgi için [bkz.](site-recovery-sql.md)

1. Azure portalına gidin ve Site Kurtarma kasanızı seçin.

2. Dynamics AX için oluşturulan kurtarma planını seçin.

3. **Yük Devretme Testi**'ni seçin.

4. Test başarısız etme işlemini başlatmak için sanal ağı seçin.

5. İkincil ortam dolduktan sonra, doğrulamalarınızı gerçekleştirebilirsiniz.

6. Doğrulamalar tamamlandıktan **sonra, Doğrulamalar tamamlandı** ve test başarısız ortamı temizlenir' seçeneğini belirleyin.

Bir test başarısızlığına ilişkin daha fazla bilgi için, [Site Kurtarma'da Azure'a test başarısızlığına](site-recovery-test-failover-to-azure.md)bakın.

### <a name="perform-a-failover"></a>Bir failover gerçekleştirin

1. Azure portalına gidin ve Site Kurtarma kasanızı seçin.

2. Dynamics AX için oluşturulan kurtarma planını seçin.

3. **Failover'ı**seçin ve **Failover'ı**seçin.

4. Hedef ağı seçin ve başarısız lık işlemini başlatmak için **✓'yi** seçin.

Bir failover yapma hakkında daha fazla bilgi için, [Site Kurtarma Failover](site-recovery-failover.md)bakın.

### <a name="perform-a-failback"></a>Yeniden çalışma gerçekleştirme

Failback sırasında SQL Server'a özgü hususlar [için](site-recovery-sql.md)bkz.

1. Azure portalına gidin ve Site Kurtarma kasanızı seçin.

2. Dynamics AX için oluşturulan kurtarma planını seçin.

3. **Failover'ı**seçin ve **Failover'ı**seçin.

4. **Yönü Değiştir'i**seçin.

5. Uygun seçenekleri seçin: veri eşitleme ve VM oluşturma.

6. **Failback** işlemini başlatmak için ✓'yi seçin.


Bir geri ödeme yapma hakkında daha fazla bilgi için [Azure'dan şirket içi failback VMware VM'lere](site-recovery-failback-azure-to-vmware.md)bakın.

## <a name="summary"></a>Özet
Site Kurtarma'yı kullanarak Dynamics AX uygulamanız için tam bir otomatik olağanüstü durum kurtarma planı oluşturabilirsiniz. Bir aksaklık durumunda, herhangi bir yerden saniyeler içinde arıza işlemini başlatabilir ve uygulamayı dakikalar içinde çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Site Kurtarma ile kurumsal iş yüklerini koruma hakkında daha fazla bilgi edinmek için [bkz.](site-recovery-workload.md)
