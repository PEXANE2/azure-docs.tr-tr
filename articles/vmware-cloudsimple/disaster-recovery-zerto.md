---
title: CloudSimple tarafından Azure VMware Çözümü - Şirket içi iş yükleri için özel bulutu felaket sitesi olarak kullanın
description: CloudSimple Private Cloud'unuzu şirket içi VMware iş yükleri için olağanüstü durum kurtarma sitesi olarak nasıl ayarlayişlerinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0e019a9229b671be2fb73e758bd39f33657bc2d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083139"
---
# <a name="set-up-cloudsimple-private-cloud-as-a-disaster-recovery-site-for-on-premises-vmware-workloads"></a>CloudSimple Private Cloud'u şirket içi VMware iş yükleri için olağanüstü durum kurtarma sitesi olarak ayarlama

CloudSimple Private Cloud'unuzun, bir felaket durumunda iş sürekliliği sağlamak için şirket içi uygulamalar için bir kurtarma sitesi olarak kurulabilir. Kurtarma çözümü çoğaltma ve orkestrasyon platformu olarak Zerto Sanal Çoğaltma dayanmaktadır. Kritik altyapı ve uygulama sanal makineleri, şirket içi vCenter'ınızdan Özel Bulut'unuza sürekli olarak çoğaltılabilir. Özel Bulut'unuzu başarısız test için ve bir felaket sırasında uygulamanızın kullanılabilirliğini sağlamak için kullanabilirsiniz. Özel Bulut'u farklı bir konumdaki bir kurtarma sitesi tarafından korunan birincil site olarak ayarlamak için benzer bir yaklaşım izlenebilir.

> [!NOTE]
> Afet kurtarma ortamınızı boyutlandırma yönergeleri için [Zerto](https://s3.amazonaws.com/zertodownload_docs/5.5U3/Zerto%20Virtual%20Replication%20Sizing.pdf) belgesinde Dikkat Edilmesi Gerekenler'a bakın.

CloudSimple çözümü:

* Olağanüstü durum kurtarma (DR) için özel olarak bir veri merkezi kurma gereksinimini ortadan kaldırır.
* CloudSimple'ın dünya çapında coğrafi esneklik için dağıtıldığı Azure konumları'ndan yararlanmanızı sağlar.
* Dr için dağıtım maliyetlerini ve toplam sahip olma maliyetini azaltma seçeneği sunar.

Çözüm şunları gerektirir:

* Özel Bulut'unuzda Zerto'ya yükleyin, yapılandırın ve yönetin.
* Özel Bulut korunan site olduğunda Zerto için kendi lisanslarınızı sağlayın. CloudSimple sitesinde çalışan Zerto'yu lisans lama için şirket içi sitenizle eşleştirebilirsiniz.

Aşağıdaki şekil Zerto çözeltisinin mimarisini göstermektedir.

![Mimari](media/cloudsimple-zerto-architecture.png)

## <a name="how-to-deploy-the-solution"></a>Çözüm nasıl dağıtılır?

Aşağıdaki bölümlerde, Özel Bulut'unuzda Zerto Sanal Çoğaltma'yı kullanarak bir DR çözümünüzün nasıl dağıtılanılabildiğini açıklayınız.

1. [Ön koşullar](#prerequisites)
2. [CloudSimple Private Cloud'da isteğe bağlı yapılandırma](#optional-configuration-on-your-private-cloud)
3. [CloudSimple Private Cloud'da ZVM ve VRA'yı ayarlama](#set-up-zvm-and-vra-on-your-private-cloud)
4. [Zerto Sanal Koruma Grubu'nun kurulumu](#set-up-zerto-virtual-protection-group)

### <a name="prerequisites"></a>Ön koşullar

Zerto Sanal Çoğaltma'yı şirket içi ortamınızdan Özel Bulut'unuza sağlamak için aşağıdaki ön koşulları tamamlayın.

1. [Şirket içi ağınızla CloudSimple Private Cloud'unuz arasında Siteden Siteye VPN bağlantısı kurun.](set-up-vpn.md)
2. [Özel Bulut yönetimi bileşenlerinizin Özel Bulut DNS sunucularına iletilmesi için DNS aramasını ayarlayın.](on-premises-dns-setup.md)  DNS aramasının iletilmesini etkinleştirmek için, `*.cloudsimple.io` şirket içi DNS sunucunuzda CloudSimple DNS sunucularına bir yönlendirme bölgesi girişi oluşturun.
3. Şirket içi vCenter bileşenlerinin şirket içi DNS sunucularına iletilmesi için DNS aramasını ayarlayın.  DNS sunucularına Siteden Siteye VPN üzerinden CloudSimple Private Cloud'unuzdan erişilebilmelidir. Yardım için, aşağıdaki bilgileri sağlayarak bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderin.  

    * Şirket içi DNS etki alanı adı
    * Şirket içi DNS sunucu IP adresleri

4. Özel Bulut'unuza bir Windows sunucusu yükleyin. Sunucu Zerto Virtual Manager yüklemek için kullanılır.
5. [CloudSimple ayrıcalıklarınızı artırın.](escalate-private-cloud-privileges.md)
6. Zerto Virtual Manager'ın hizmet hesabı olarak kullanılacak yönetim rolüyle Private Cloud vCenter'ınızda yeni bir kullanıcı oluşturun.

### <a name="optional-configuration-on-your-private-cloud"></a>Özel Bulut'unuzda isteğe bağlı yapılandırma

1. Özel Bulut vCenter'ınızda, şirket içi ortamınızdan VM'ler için hedef kaynak havuzları olarak kullanmak üzere bir veya daha fazla kaynak havuzu oluşturun.
2. Özel Bulut vCenter'ınızda, şirket içi ortamınızdan VM'ler için hedef klasör olarak kullanmak üzere bir veya daha fazla klasör oluşturun.
3. Failover ağı için VN'ler oluşturun ve güvenlik duvarı kuralları ayarlayın. Yardım için bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) açın.
4. Başarısız ağ ve Test ağı için dağıtılmış bağlantı noktası grupları oluşturun ve VM'lerin başarısız lığı test edin.
5. [DHCP ve DNS sunucularını](dns-dhcp-setup.md) yükleyin veya Özel Bulut ortamınızda bir Active Directory etki alanı denetleyicisi kullanın.

### <a name="set-up-zvm-and-vra-on-your-private-cloud"></a>Özel Bulutunuzda ZVM ve VRA'yı ayarlama

1. Özel Bulut'unuzdaki Windows sunucusuna Zerto Virtual Manager'ı (ZVM) yükleyin.
2. Önceki adımlarda oluşturulan hizmet hesabını kullanarak ZVM'de oturum açın.
3. Zerto Virtual Manager için lisans ayarlayın.
4. Zerto Sanal Çoğaltma Cihazı'nı (VRA) Özel Bulut'unuzun ESXi ana bilgisayarlarına yükleyin.
5. Özel Bulut ZVM'nizi şirket içi ZVM'nizle eşleştirin.

### <a name="set-up-zerto-virtual-protection-group"></a>Zerto Sanal Koruma Grubu'nun kurulumu

1. Yeni bir Sanal Koruma Grubu (VPG) oluşturun ve VPG için önceliği belirtin.
2. İş sürekliliği için koruma gerektiren sanal makineleri seçin ve gerekirse önyükleme sırasını özelleştirin.
3. Kurtarma sitesini Özel Bulut'unuz, varsayılan kurtarma sunucusunu da Özel Bulut kümesi veya oluşturduğunuz kaynak grubu olarak seçin. Özel Bulut'unuzdaki kurtarma veri deposu için **vsanDatastore'u** seçin.

    ![VPG](media/cloudsimple-zerto-vpg.png)

    > [!NOTE]
    > VM Ayarları seçeneği altında tek tek VM'ler için ana bilgisayar seçeneğini özelleştirebilirsiniz.

4. Depolama seçeneklerini gerektiği gibi özelleştirin.
5. Daha önce oluşturulan dağıtılmış bağlantı noktası grupları olarak başarısız ağ ve başarısız test ağı için kullanılacak kurtarma ağlarını belirtin ve kurtarma komut dosyalarını gerektiği gibi özelleştirin.
6. Gerekirse tek tek VM'ler için ağ ayarlarını özelleştirin ve VPG'yi oluşturun.
7. Çoğaltma tamamlandıktan sonra test başarısız.

## <a name="reference"></a>Başvuru

[Zerto Dokümantasyon](https://www.zerto.com/myzerto/technical-documentation/)
