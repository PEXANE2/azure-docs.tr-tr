---
title: Azure'da Linux VM'leri için zaman eşitleme
description: Linux sanal makineleri için zaman senkronizasyonu.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 7c93c1f525713a90abd71c30a21401b9d1cfcb9f
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460911"
---
# <a name="time-sync-for-linux-vms-in-azure"></a>Azure'da Linux VM'leri için zaman eşitleme

Zaman eşitleme güvenlik ve olay korelasyon için önemlidir. Bazen dağıtılmış hareketler uygulaması için kullanılır. Birden çok bilgisayar sistemi arasındaki zaman doğruluğu senkronizasyon yoluyla elde edilir. Eşitleme, zaman kaynağı ile zamanı alan bilgisayar arasındaki yeniden başlatma lar ve ağ trafiği de dahil olmak üzere birden çok şeyden etkilenebilir. 

Azure, Windows Server 2016 çalıştıran altyapı tarafından desteklenen bir altyapıdır. Windows Server 2016, zamanı düzeltmek ve yerel saati UTC ile eşitlemek üzere koşullandırmak için kullanılan algoritmaları geliştirmiştir.  Windows Server 2016 Doğru Zaman özelliği, VM'leri doğru zaman için ana bilgisayarla yöneten VMICTimeSync hizmetinin nasıl geliştirildiğini büyük ölçüde iyileştirmiştir. Geliştirmeler, VM start veya VM geri yüklemeve gecikme düzeltmesi kesme de dahil olmak. 

>[!NOTE]
>Windows Time hizmetine hızlı bir genel bakış için, bu [üst düzey genel bakış videosuna](https://aka.ms/WS2016TimeVideo)bir göz atın.
>
> Daha fazla bilgi [için Windows Server 2016 için doğru zaman](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)anına bakın. 

## <a name="overview"></a>Genel Bakış

Bilgisayar saatinin doğruluğu, bilgisayar saatinin Eşgüdümlü Evrensel Saat (UTC) zaman standardına ne kadar yakın olduğu ölçülür. UTC, 300 yıl içinde sadece bir saniye kapalı olabilir hassas atom saatleri çokuluslu bir örnek tarafından tanımlanır. Ancak, UTC okuma doğrudan özel donanım gerektirir. Bunun yerine, zaman sunucuları UTC ile senkronize edilir ve ölçeklenebilirlik ve sağlamlık sağlamak için diğer bilgisayarlardan erişilir. Her bilgisayarın, sunucuların ne zaman kullanılacağını bilen ve bilgisayar saatinin düzeltilmesi gerekip gerekmediğini periyodik olarak kontrol eden ve gerekirse zamanı ayarlayan zaman eşitleme hizmeti vardır. 

Azure ana bilgisayarları, Microsoft'a ait Stratum 1 aygıtlarından GPS antenleri ile zaman ayıran dahili Microsoft zaman sunucularıyla senkronize edilir. Azure'daki sanal makineler, doğru zamanı *(ana bilgisayar zamanı)* VM'ye geçirmek için ana bilgisayarlarına güvenebilir veya VM doğrudan bir zaman sunucusundan veya her ikisinin birleşiminden zaman alabilir. 

Bağımsız donanımda, Linux işletim sistemi yalnızca önyüklemede ana bilgisayar donanım saatini okur. Bundan sonra, saat Linux çekirdeğindeki kesme zamanlayıcısı kullanılarak korunur. Bu yapılandırmada, saat zaman içinde sürüklenecektir. Azure'daki yeni Linux dağıtımlarında, VM'ler ana bilgisayardan saat güncelleştirmelerini daha sık sorgulamak için Linux tümleştirme hizmetlerine (LIS) dahil olan VMICTimeSync sağlayıcısını kullanabilir.

Ana bilgisayarla sanal makine etkileşimleri de saati etkileyebilir. [Bellek koruma bakımı](../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot)sırasında, VM'ler 30 saniyeye kadar duraklatıldı. Örneğin, bakım başlamadan önce VM saati 10:00:00'ı gösterir ve 28 saniye sürer. VM devam ettikten sonra, VM'deki saat hala 28 saniye kapalı olacak 10:00:00'ı gösterir. Bunu düzeltmek için, VMICTimeSync hizmeti ana bilgisayarda neler olduğunu izler ve vm'lerde değişikliklerin telafi edilmesini ister.

Zaman eşitleme çalışması olmadan, VM'deki saat hata biriktirir. Yalnızca bir VM olduğunda, iş yükü son derece doğru zaman tutma gerektirmedikçe etki önemli olmayabilir. Ancak çoğu durumda, hareketleri izlemek için zaman kullanan ve tüm dağıtım boyunca tutarlı olması gereken birden çok, birbirine bağlı VM'miz vardır. VM'ler arasındaki süre farklı olduğunda, aşağıdaki efektleri görebilirsiniz:

- Kimlik doğrulama başarısız olur. Kerberos veya sertifikaya bağımlı teknoloji gibi güvenlik protokolleri, sistemler arasında tutarlı olan zamana dayanır.
- Günlükler (veya diğer veriler) zamanında kabul etmiyorsa, bir sistemde neler olduğunu anlamak çok zordur. Aynı olay farklı zamanlarda meydana geldi gibi görünecek, korelasyon zor hale.
- Saat kapalıysa, faturalandırma yanlış hesaplanabilir.



## <a name="configuration-options"></a>Yapılandırma seçenekleri

Azure'da barındırılan Linux VM'leriniz için zaman eşitlemesini yapılandırmanın genellikle üç yolu vardır:

- Azure Marketi görüntüleri için varsayılan yapılandırma, hem NTP süresini hem de VMICTimeSync ana bilgisayar zamanını kullanır. 
- Yalnızca VMICTimeSync kullanılarak ana bilgisayar.
- VMICTimeSync ana bilgisayar kullanma süresi olan veya kullanmadan başka bir harici zaman sunucusu kullanın.


### <a name="use-the-default"></a>Varsayılanı kullanma

Varsayılan olarak, Linux için Azure Marketi görüntülerinin çoğu iki kaynaktan eşitolacak şekilde yapılandırılır: 

- Birincil olarak NTP, bir NTP sunucusundan zaman alır. Örneğin, Ubuntu 16.04 LTS Marketplace görüntüleri **ntp.ubuntu.com.**
- İkincil olarak VMICTimeSync hizmeti, ana bilgisayar süresini VM'lere iletmek ve VM bakım için duraklatıldıktan sonra düzeltmeler yapmak için kullanılır. Azure ana bilgisayarları, doğru zamanı korumak için Microsoft'a ait Stratum 1 aygıtlarını kullanır.

Yeni Linux dağıtımlarında, VMICTimeSync hizmeti hassas zaman protokolünü (PTP) kullanır, ancak önceki dağıtımlar PTP'yi desteklemeyebilir ve ana bilgisayardan zaman almak için NTP'ye geri döner.

NTP'nin doğru eşitleme olduğunu `ntpq -p` doğrulamak için komutu çalıştırın.

### <a name="host-only"></a>Yalnızca ev sahibi 

time.windows.com ve ntp.ubuntu.com gibi NTP sunucuları herkese açık olduğundan, zamanlarını onlarla eşitleme için internet üzerinden trafik gönderilmesi gerekir. Değişen paket gecikmeleri zaman eşitleme kalitesini olumsuz etkileyebilir. Yalnızca ana bilgisayar eşitlemesine geçerek NTP'yi kaldırmak bazen zaman eşitleme sonuçlarınızı artırabilir.

Varsayılan yapılandırmayı kullanarak zaman eşitleme sorunlarıyla karşılaşırsanız, yalnızca ana bilgisayara geçiş yapmak mantıklıdır. VM'nizde zaman eşitlemeyi iyileştirip iyileştirmeyeceğini görmek için yalnızca ana bilgisayar eşitlemeyi deneyin. 

### <a name="external-time-server"></a>Dış zaman sunucusu

Belirli zaman eşitleme gereksinimleriniz varsa, dış zaman sunucularını kullanma seçeneğiniz de vardır. Dış zaman sunucuları, Microsoft dışındaki veri merkezlerinde barındırılan makinelerle zaman tekdüzeliği sağlayarak veya artık saniyeleri özel bir şekilde işleme sağlayarak test senaryoları için yararlı olabilecek belirli bir zaman sağlayabilir.

Varsayılan yapılandırmaya benzer sonuçlar sağlamak için harici bir zaman sunucusunu VMICTimeSync hizmetiyle birleştirebilirsiniz. Harici bir zaman sunucusunu VMICTimeSync ile birleştirmek, VM'ler bakım için duraklatıldığında neden olabilecek sorunlarla başa çıkmak için en iyi seçenektir. 

## <a name="tools-and-resources"></a>Araçlar ve kaynaklar

Zaman eşitleme yapılandırmanızı denetlemek için bazı temel komutlar vardır. Linux dağıtımı için dokümantasyon, bu dağıtım için zaman eşitleme yapılandırmak için en iyi yolu hakkında daha fazla ayrıntıya sahip olacaktır.

### <a name="integration-services"></a>Tümleştirme hizmetleri

Tümleştirme hizmetinin (hv_utils) yüklenip yüklenmeip yüklenmeip yüklenmeyip yüklenmeyip yüklenmeyip yüklenmeyip yüklenmeyip yüklenmeyip yük

```bash
lsmod | grep hv_utils
```
Buna benzer bir şey görmeniz gerekir:

```
hv_utils               24418  0
hv_vmbus              397185  7 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
```

Hyper-V entegrasyon hizmetleri daemon çalışıyor olup olmadığını görmek.

```bash
ps -ef | grep hv
```

Buna benzer bir şey görmeniz gerekir:

```
root        229      2  0 17:52 ?        00:00:00 [hv_vmbus_con]
root        391      2  0 17:52 ?        00:00:00 [hv_balloon]
```


### <a name="check-for-ptp"></a>PTP için kontrol edin

Linux'un yeni sürümleriyle, VMICTimeSync sağlayıcısının bir parçası olarak Hassas Zaman Protokolü (PTP) saat kaynağı mevcuttur. Red Hat Enterprise Linux veya CentOS 7.x'in eski sürümlerinde [Linux Entegrasyon Hizmetleri](https://github.com/LIS/lis-next) indirilebilir ve güncelleştirilmiş sürücüyü yüklemek için kullanılabilir. PTP kullanırken, Linux cihazı form /dev/ptp*x*olacaktır. 

Hangi PTP saat kaynaklarının kullanılabildiğine bakın.

```bash
ls /sys/class/ptp
```

Bu örnekte, döndürülen değer *ptp0'dır,* bu nedenle saat adını denetlemek için bunu kullanırız. Aygıtı doğrulamak için saat adını kontrol edin.

```bash
cat /sys/class/ptp/ptp0/clock_name
```

Bu **hyperv**dönmelidir.

### <a name="chrony"></a>chrony

Ubuntu 19.10 ve sonraki sürümlerinde, Red Hat Enterprise Linux ve CentOS 7.x, [chrony](https://chrony.tuxfamily.org/) ptp kaynak saat kullanmak üzere yapılandırılmıştır. Chrony yerine, eski Linux sürümleri PTP kaynaklarını desteklemeyen Network Time Protocol daemon (ntpd) kullanır. Bu sürümlerde PTP'yi etkinleştirmek için, chrony'nin aşağıdaki kod kullanılarak el ile yüklenmesi ve (chrony.conf'ta) yapılandırılması gerekir:

```bash
refclock PHC /dev/ptp0 poll 3 dpoll -2 offset 0
```

Ubuntu ve NTP hakkında daha fazla bilgi için [Bkz. Zaman Eşitleme.](https://help.ubuntu.com/lts/serverguide/NTP.html)

Red Hat ve NTP hakkında daha fazla bilgi için [NTP'yi Yapılandır'a](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-configure_ntp)bakın. 

Chrony hakkında daha fazla bilgi için [bkz.](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-using_chrony)

Hem chrony hem de TimeSync kaynakları aynı anda etkinleştirilmişse, birini diğer kaynağı yedek olarak ayarlayan **tercih**olarak işaretleyebilirsiniz. NTP hizmetleri uzun bir süre dışında büyük eğrilikler için saati güncelleştirmediği için, VMICTimeSync yalnızca NTP tabanlı araçlardan çok daha hızlı bir şekilde duraklatılmış VM olaylarından saati kurtarır.

Varsayılan olarak, chronyd herhangi bir zaman kayması düzeltmek için sistem saatini hızlandırır veya yavaşlatır. Sürüklenme çok büyük olursa, chrony sürüklenme düzeltmek için başarısız olur. Bunun üstesinden gelmek `makestep` için, **/etc/chrony.conf'taki** parametre, sürüklenme belirtilen eşiği aşarsa zaman eşitini zorlayacak şekilde değiştirilebilir.

 ```bash
makestep 1.0 -1
```

Burada, sürüklenme 1 saniyeden büyükse, chrony bir zaman güncelleştirmesi zorlar. Değişiklikleri uygulamak için chronyd hizmetini yeniden başlatın:

```bash
systemctl restart chronyd
```

### <a name="systemd"></a>sistemli 

SUSE ve Ubuntu bültenleri 19.10 önce, zaman eşitleme [systemd](https://www.freedesktop.org/wiki/Software/systemd/)kullanılarak yapılandırılır. Ubuntu hakkında daha fazla bilgi için [Bkz. Zaman Eşitleme.](https://help.ubuntu.com/lts/serverguide/NTP.html) SUSE hakkında daha fazla bilgi için, [SUSE Linux Enterprise Server 12 SP3 Sürüm Notları](https://www.suse.com/releasenotes/x86_64/SUSE-SLES/12-SP3/#InfraPackArch.ArchIndependent.SystemsManagement)Bölüm 4.5.8 bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi [için Windows Server 2016 için doğru zaman](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)anına bakın.


