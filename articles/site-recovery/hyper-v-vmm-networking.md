---
title: Azure Site Kurtarma ile ikincil bir siteye geçilemeden IP adresleme sini ayarlama
description: Olağanüstü durum kurtarma ve Azure Site Kurtarma ile başarısız olduktan sonra ikincil bir yerinde VM'lere bağlanmak için IP adresinin nasıl ayarlandığı açıklanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: a61f7ff69e648262eb721eb61a98b09dbbee924c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73961436"
---
# <a name="set-up-ip-addressing-to-connect-to-a-secondary-on-premises-site-after-failover"></a>Başarısız olduktan sonra ikincil bir şirket içi siteye bağlanmak için IP adresini ayarlama

System Center Virtual Machine Manager (VMM) bulutlarında ikinci bir siteye Hyper-V VM'ler üzerinde başarısız olduktan sonra, çoğaltma VM'lerine bağlanabilmeniz gerekir. Bu makale, bunu yapmanıza yardımcı olur. 

## <a name="connection-options"></a>Bağlantı seçenekleri

Başarısız olduktan sonra, çoğaltma VM'ler için IP adresleme işlemek için birkaç yolu vardır: 

- **Başarısız olduktan sonra aynı IP adresini koruyun**: Bu senaryoda, çoğaltılan VM birincil VM ile aynı IP adresine sahiptir. Bu, başarısız olduktan sonra ağla ilgili sorunları basitleştirir, ancak bazı altyapı çalışmaları gerektirir.
- **Başarısız**olduktan sonra farklı bir IP adresi kullanın : Bu senaryoda VM, başarısız olduktan sonra yeni bir IP adresi alır. 
 

## <a name="retain-the-ip-address"></a>IP adresini koruyun

Birincil sitedeki IP adreslerini saklamak istiyorsanız, ikincil siteye başarısız olduktan sonra şunları yapabilirsiniz:

- Birincil ve ikincil siteler arasında uzatılmış bir alt ağ dağıtın.
- Birincil siteden ikincil siteye tam bir alt ağ failover gerçekleştirin. IP adreslerinin yeni konumunu belirtmek için yolları güncelleştirmeniz gerekir.


### <a name="deploy-a-stretched-subnet"></a>Uzatılmış bir alt ağ dağıtma

Genişletilmiş bir yapılandırmada, alt ağ hem birincil hem de ikincil sitelerde aynı anda kullanılabilir. Uzatılmış bir alt ağda, bir makineyi ve IP (Katman 3) adresini ikincil siteye taşıdığınızda, ağ trafiği otomatik olarak yeni konuma yönlendirir. 

- Katman 2 (veri bağlantısı katmanı) perspektifinden, uzatılmış bir VLAN'ı yönetebilen ağ donanımına ihtiyacınız vardır.
- VLAN'ı genişleterek, olası hata etki alanı her iki siteye de uzanır. Bu tek bir başarısızlık noktası haline gelir. Olası olmasa da, böyle bir senaryoda yayın fırtınası gibi bir olayı yalıtmayabilirsiniz. 


### <a name="fail-over-a-subnet"></a>Bir alt ağ üzerinden başarısız

Gerilmiş alt netin faydalarını elde etmek için tüm alt ağ üzerinde, aslında germe olmadan başarısız olabilir. Bu çözümde, kaynak veya hedef sitede bir alt ağ kullanılabilir, ancak her ikisinde de aynı anda bulunmaz.

- Bir arıza durumunda IP adresi alanını korumak için, yönlendirici altyapısının alt ağları bir siteden diğerine taşımasını programlı olarak ayarlayabilirsiniz.
- Bir hata oluştuğunda, alt ağlar ilişkili VM'leriyle birlikte hareket eder.
- Bu yaklaşımın ana dezavantajı, bir hata durumunda, tüm alt ağı taşımak zorunda olmasıdır.

#### <a name="example"></a>Örnek

Burada tam subnet failover bir örnek. 

- Başarısız olmadan önce, birincil sitenin 192.168.1.0/24 alt netinde çalışan uygulamaları vardır.
- Başarısız lık sırasında, bu alt ağdaki tüm VM'ler ikincil siteye geçemedi ve IP adreslerini korur. 
- 192.168.1.0/24 alt netindeki tüm VM'lerin artık ikincil siteye taşındığını yansıtacak şekilde tüm siteler arasındaki yolların değiştirilmesi gerekir.

Aşağıdaki grafikler, başarısız olmadan önce ve sonra alt ağları gösterir.


**Başarısız olmadan önce**

![Başarısız olmadan önce](./media/hyper-v-vmm-networking/network-design2.png)

**Başarısız olduktan sonra**

![Başarısız olduktan sonra](./media/hyper-v-vmm-networking/network-design3.png)

Başarısız olduktan sonra, Site Kurtarma VM'deki her ağ arabirimi için bir IP adresi ayırır. Adres, her VM örneği için ilgili ağdaki statik IP adresi havuzundan ayrılır.

- İkincil sitedeki IP adresi havuzu kaynak sitedekiyle aynıysa, Site Kurtarma aynı IP adresini (kaynak VM'nin) yineleme VM'sine ayırır. IP adresi VMM'de ayrılmıştır, ancak Hyper-V ana bilgisayardaki başarısız IP adresi olarak ayarlı değildir. Hyper-v ana bilgisayardaki failover IP adresi, hatadan hemen önce ayarlanır.
- Aynı IP adresi kullanılamıyorsa, Site Kurtarma havuzdan başka bir kullanılabilir IP adresi ayırır.
- VM'ler DHCP kullanıyorsa, Site Kurtarma IP adreslerini yönetmez. İkincil sitedeki DHCP sunucusunun adresleri kaynak siteyle aynı aralıktan ayırabiliyor mu kontrol etmeniz gerekir.

### <a name="validate-the-ip-address"></a>IP adresini doğrulama

Bir VM için korumayı etkinleştirdikten sonra, VM'ye atanan adresi doğrulamak için aşağıdaki örnek komut dosyasını kullanabilirsiniz. Bu IP adresi failover IP adresi olarak ayarlanır ve failover sırasında VM'ye atanır:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="use-a-different-ip-address"></a>Farklı bir IP adresi kullanma

Bu senaryoda, üzerinde başarısız vm'lerin IP adresleri değiştirilir. Bu çözümün dezavantajı gerekli bakımdır.  DNS ve önbellek girişlerinin güncelleştirilmesi gerekebilir. Bu, aşağıdaki gibi azaltılabilir kapalı kalma süresi, neden olabilir:

- Intranet uygulamaları için düşük TTL değerlerini kullanın.
- DNS sunucusunun zamanında güncellemesi için site kurtarma kurtarma planında aşağıdaki komut dosyasını kullanın. Dinamik DNS kaydı kullanıyorsanız komut dosyasına ihtiyacınız yoktur.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Örnek 

Bu örnekte, birincil ve ikincil siteler arasında farklı IP adresleri vardır ve birincil veya kurtarma sitesinde barındırılan uygulamalara erişilebilen üçüncü bir site vardır.

- Başarısız olmadan önce, uygulamalar birincil sitede 192.168.1.0/24 alt ağı barındırılır.
- Başarısız olduktan sonra, uygulamalar ikincil sitede 172.16.1.0/24 alt netinde yapılandırılır.
- Üç site de birbirine erişebilir.
- Başarısız olduktan sonra, kurtarma alt ağına uygulamalar geri yüklenir.
- Bu senaryoda, tüm alt ağ üzerinde başarısız olmaya gerek yoktur ve VPN veya ağ yollarını yeniden yapılandırmak için herhangi bir değişiklik gerekmez. Başarısız ve bazı DNS güncelleştirmeleri, uygulamaların erişilebilir kalmasını sağlar.
- DNS dinamik güncelleştirmelere izin verecek şekilde yapılandırılırsa, VM'ler başarısız olduktan sonra başladıklarında yeni IP adresini kullanarak kendilerini kaydederler.

**Başarısız olmadan önce**

![Farklı IP adresi - başarısız olmadan önce](./media/hyper-v-vmm-networking/network-design10.png)

**Başarısız olduktan sonra**

![Farklı IP adresi - başarısız olduktan sonra](./media/hyper-v-vmm-networking/network-design11.png)


## <a name="next-steps"></a>Sonraki adımlar

[Yük devretme çalıştırma](hyper-v-vmm-failover-failback.md)

