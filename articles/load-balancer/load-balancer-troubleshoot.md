---
title: Azure Load Balancer sorunlarını giderme
description: Azure Yük Bakiyesi ile bilinen sorunları nasıl gidereceklerini öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
manager: dcscontentpm
ms.custom: seodoc18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/28/2020
ms.author: allensu
ms.openlocfilehash: ca9b70bd71a618f8e3d5f4fe9504ba66a9f14c6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76935473"
---
# <a name="troubleshoot-azure-load-balancer"></a>Azure Load Balancer sorunlarını giderme

Bu sayfa, Temel ve Standart ortak Azure Yük Dengeleyici sors soruları için sorun giderme bilgileri sağlar. Standart Yük Dengeleyicisi hakkında daha fazla bilgi [için, Standart Yük Dengeleyicisi genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)bakın.

Yük Dengeleyici bağlantısı kullanılamıyorsa, en sık karşılaşılan belirtiler şunlardır: 

- Yük Dengeleyicisi'nin arkasındaki VM'ler sağlık sondalarına yanıt vermiyor 
- Yük Dengeleyicisi'nin arkasındaki VM'ler yapılandırılan bağlantı noktasındaki trafiğe yanıt vermiyor

Arka uçTAKI VM'lerin dış istemcileri yük dengeleyicisinden geçtiğinde, iletişim için istemcilerin IP adresi kullanılır. Müşterilerin IP adresinin NSG izin listesine eklenmiştir. 

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Belirti: Yük Dengeleyicisi'nin arkasındaki VM'ler sağlık sondalarına yanıt vermiyor
Arka uç sunucuların yük dengeleyici kümesine katılabilmek için sonda denetimini geçmeleri gerekir. Sağlık sondaları hakkında daha fazla bilgi için [bkz.](load-balancer-custom-probe-overview.md) 

Yük Dengeleyici arka uç havuzu VM'ler aşağıdaki nedenlerden herhangi biri nedeniyle problara yanıt vermeyebilir: 
- Yük Dengeleyici arka uç havuzu VM sağlıksız 
- Yük Dengeleyici arka uç havuzu VM prob bağlantı noktasını dinlemiyor 
- Güvenlik duvarı veya ağ güvenlik grubu Yük Dengeleyici arka uç havuzu VM'lerde bağlantı noktasını engelliyor 
- Yük Dengeleyicisindeki diğer yanlış yapılandırmalar

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>Neden 1: Yük Dengeleyici arka uç havuzu VM sağlıksız 

**Doğrulama ve çözüm**

Bu sorunu gidermek için, katılımcı VM'lerde oturum açın ve VM durumunun sağlıklı olup olmadığını kontrol edin ve havuzdaki başka bir VM'den **PsPing** veya **TCPing** yanıt verebilir. VM sağlıksızsa veya sondaya yanıt veremiyorsa, yük dengelemesine katlanmadan önce sorunu düzeltmeli ve VM'yi sağlıklı bir duruma geri getirmelisiniz.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>Neden 2: Yük Dengeleyici arka uç havuzu VM prob bağlantı noktasını dinlemiyor
VM sağlıklıysa, ancak sondaya yanıt vermiyorsa, o zaman olası bir neden sonda bağlantı noktasının katılımcı VM'de açık olmaması veya VM'nin o bağlantı noktasında dinlememesi olabilir.

**Doğrulama ve çözüm**

1. Arka uç VM'de oturum açın. 
2. Bir komut istemi açın ve sonda bağlantı noktasında dinleyen bir uygulama olduğunu doğrulamak için aşağıdaki komutu çalıştırın:   
            netstat -an
3. Bağlantı noktası durumu **Listening**olarak listelenmemişse, uygun bağlantı noktasını yapılandırın. 
4. Alternatif olarak, **Listening**olarak listelenen başka bir bağlantı noktası seçin ve buna göre yük dengeleyici yapılandırması güncelleştirin.              

### <a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>Neden 3: Güvenlik duvarı veya ağ güvenlik grubu yük dengeleyici arka uç havuzu VM'lerde bağlantı noktasını engelliyor  
VM'deki güvenlik duvarı sonda bağlantı noktasını engelliyorsa veya alt ağda veya VM'de yapılandırılan bir veya daha fazla ağ güvenlik grubu sondanın bağlantı noktasına ulaşmasına izin vermiyorsa, VM sistem durumu sondasına yanıt veremez.          

**Doğrulama ve çözüm**

* Güvenlik duvarı etkinse, sonda bağlantı noktasına izin verecek şekilde yapılandırıp yapılandırılmadı denetleyin. Değilse, sonda bağlantı noktasında ki trafiğe izin verecek şekilde güvenlik duvarını yapılandırın ve yeniden sınama. 
* Ağ güvenlik grupları listesinden, sonda bağlantı noktasındaki gelen veya giden trafiğin parazit olup olmadığını kontrol edin. 
* Ayrıca, Bir **Reddet Tüm** ağ güvenlik gruplarının VM'nin NIC'ine veya LB sondalarının trafiğine & izin veren varsayılan kuraldan daha yüksek bir önceliğe sahip alt ağ üzerinde hükmedip yönetilmediğini denetleyin (ağ güvenlik grupları 168.63.129.16'lık Load Balancer IP'sine izin vermelidir). 
* Bu kurallardan herhangi biri sonda trafiğini engelliyorsa, sonda trafiğine izin verecek şekilde kuralları kaldırın ve yeniden yapılandırın.  
* VM'nin sağlık sondalarına yanıt vermeye başedip başlamadığı test edin. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>Neden 4: Yük Dengeleyicisindeki diğer yanlış yapılandırmalar
Önceki tüm nedenler doğru doğrulanmış ve çözülmüş gibi görünüyorsa ve arka uç VM hala sistem durumu sondasına yanıt vermiyorsa, bağlantı için el ile test edin ve bağlantıyı anlamak için bazı izleri toplayın.

**Doğrulama ve çözüm**

* Sonda bağlantı noktası yanıtını test etmek için VNet'teki diğer VM'lerden birinden **Psping'i** kullanın (örnek: .\psping.exe -t 10.0.0.4:3389) ve sonuçları kaydedin. 
* Sonda bağlantı noktası yanıtını (örneğin: .\tcping.exe 10.0.0.4 3389) ve kayıt sonuçlarını test etmek için VNet içindeki diğer VM'lerden birinden **TCPing'i** kullanın. 
* Bu ping testlerinde yanıt alınmazsa,
    - Hedef arka uç havuzu VM'de eşzamanlı netsh izi ve aynı VNet'ten başka bir test VM çalıştırın. Şimdi, bir süre için bir PsPing testi çalıştırın, bazı ağ izleri toplamak ve sonra testi durdurmak. 
    - Ağ yakalamayı analiz edin ve ping sorgusuyla ilgili hem gelen hem de giden paketler olup olmadığını görün. 
        - Arka uç havuzu VM'de gelen paketler gözlenmezse, trafiği engelleyen bir ağ güvenlik grupları veya UDR yanlış yapılandırması olabilir. 
        - Arka uç vm havuzunda giden paketler gözlenmezse, VM'nin ilgisiz sorunlar (örneğin, sonda bağlantı noktasını engelleyen uygulama) için denetmesi gerekir. 
    - Sonda paketlerinin yük bakiyesine ulaşmadan önce başka bir hedefe (muhtemelen UDR ayarları yla) zorlandığını doğrulayın. Bu, trafiğin arka uç VM'ye asla ulaşmama neden olabilir. 
* Sonda türünü (örneğin, HTTP'den TCP'ye) değiştirin ve sorunun sonda yanıtı yapılandırmasında olup olmadığını doğrulamak için ağ güvenlik grupları AK'larda ve güvenlik duvarında ilgili bağlantı noktasını yapılandırın. Sistem durumu sondası yapılandırması hakkında daha fazla bilgi için [bkz.](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/)

## <a name="symptom-vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>Belirti: Yük Dengeleyicisi'nin arkasındaki VM'ler yapılandırılan veri bağlantı noktasındaki trafiğe yanıt vermiyor

Bir arka uç havuzu VM sağlıklı olarak listelenir ve sağlık sondalarına yanıt veriyorsa, ancak yük dengelemesine hala katılmuyorsa veya veri trafiğine yanıt vermiyorsa, bunun nedeni aşağıdaki nedenlerden olabilir: 
* Yük Dengeleyici Backend havuzu VM veri bağlantı noktasında dinlemiyor 
* Ağ güvenlik grubu Yük Dengeleyici arka uç havuzu VM üzerinde bağlantı noktası engelliyor  
* Yük Dengeleyiciye aynı VM ve NIC'den erişim 
* Katılımcı Yük Dengeleyici arka uç havuzu VM'den Internet Yük Dengeleyici ön ucuna erişim 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>Neden 1: Yük Dengeleyici arka uç havuzu VM veri bağlantı noktasında dinlemiyor 
Bir VM veri trafiğine yanıt vermiyorsa, bunun nedeni katılımcı VM'de hedef bağlantı noktasının açık olmaması veya VM'nin o bağlantı noktasında niçin dinliyordu. 

**Doğrulama ve çözüm**

1. Arka uç VM'de oturum açın. 
2. Bir komut istemi açın ve veri bağlantı noktasında dinleyen bir  uygulama olduğunu doğrulamak için aşağıdaki komutu çalıştırın: netstat -an 
3. Bağlantı noktası Devlet "DİnLEME" ile listelenmemişse, uygun dinleyici bağlantı noktasını yapılandırın 
4. Bağlantı noktası Dinleme olarak işaretlenmişse, olası sorunlar için bu bağlantı noktasındaki hedef uygulamayı denetleyin.

### <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>Neden 2: Ağ güvenlik grubu Yük Dengeleyici arka uç havuzu VM bağlantı noktasını engelliyor  

Alt ağda veya VM'de yapılandırılan bir veya daha fazla ağ güvenlik grubu kaynak IP'yi veya bağlantı noktasını engelliyorsa, VM yanıt veremiyor demektir.

Ortak yük dengeleyicisi için, Internet istemcilerinin IP adresi istemciler ve yük dengeleyici backend VM'ler arasındaki iletişim için kullanılacaktır. Arka uç VM'nin ağ güvenlik grubunda istemcilerin IP adresine izin verildiğinden emin olun.

1. Arka uç VM'de yapılandırılan ağ güvenlik gruplarını listele. Daha fazla bilgi için [bkz.](../virtual-network/manage-network-security-group.md)
1. Ağ güvenlik grupları listesinden:
    - veri bağlantı noktasındaki gelen veya giden trafiğin paraziti vardır. 
    - VM'nin NIC'inde veya Yük Dengeleyici problarına ve trafiğine izin veren varsayılan kuralın daha yüksek bir önceliğe sahip alt netindeki Tüm ağ güvenlik grubu kuralını **reddet** (ağ güvenlik grupları 168.63.129,16 yük dengeleyiciIP,yani prob bağlantı noktası)
1. Kurallardan herhangi biri trafiği engelliyorsa, veri trafiğine izin verecek şekilde bu kuralları kaldırın ve yeniden yapılandırın.  
1. VM'nin sağlık sondalarına yanıt vermeye başedip başlamadığı test edin.

### <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>Neden 3: Yük Dengeleyicisi'ne aynı VM ve Ağ arabiriminden erişim 

Bir Yük Dengeleyicisinin arka uç VM'inde barındırılan uygulamanız, aynı Ağ Arabirimi üzerinden aynı arka uç VM'de barındırılan başka bir uygulamaya erişmeye çalışıyorsa, bu desteklenmeyen bir senaryodur ve başarısız olur. 

**Çözünürlük** Bu sorunu aşağıdaki yöntemlerden biriyle çözebilirsiniz:
* Uygulama başına ayrı arka uç havuzu VM'lerini yapılandırın. 
* Uygulamayı çift NIC VM'lerde yapılandırın, böylece her uygulama kendi Ağ arabirimini ve IP adresini kullanıyordu. 

### <a name="cause-4-accessing-the-internal-load-balancer-frontend-from-the-participating-load-balancer-backend-pool-vm"></a>Neden 4: Katılımcı Yük Dengeleyici arka uç havuzu VM'den dahili Yük Dengeleyici ön ucuna erişim

Bir dahili Yük Dengeleyici si, bir VNet içinde yapılandırılırsa ve katılımcı arka uç VM'lerden biri dahili Yük Dengeleyici ön ucuna erişmeye çalışıyorsa, akış kaynak VM'ye eşlendiğinde hatalar oluşabilir. Bu senaryo desteklenmez. Ayrıntılı bir tartışma için [sınırlamaları](concepts-limitations.md#limitations) gözden geçirin.

**Çözünürlük** Proxy kullanmak da dahil olmak üzere bu senaryonun engelini kaldırmanın çeşitli yolları vardır. Uygulama Ağ Geçidi'ni veya diğer üçüncü taraf vekillerini (örneğin, nginx veya haproxy) değerlendirin. Uygulama Ağ Geçidi hakkında daha fazla bilgi için [bkz.](../application-gateway/application-gateway-introduction.md)

## <a name="symptom-cannot-change-backend-port-for-existing-lb-rule-of-a-load-balancer-which-has-vm-scale-set-deployed-in-the-backend-pool"></a>Belirti: Arka uç havuzunda VM Ölçek Seti olan bir yük dengeleyicisinin mevcut LB kuralı için arka uç bağlantı noktasını değiştiremez. 
### <a name="cause--the-backend-port-cannot-be-modified-for-a-load-balancing-rule-thats-used-by-a-health-probe-for-load-balancer-referenced-by-vm-scale-set"></a>Neden : Arka uç bağlantı noktası, VM Scale Set tarafından başvurulan yük dengeleyicisi için bir sistem durumu sondası tarafından kullanılan bir yük dengeleme kuralı için değiştirilemez.
**Çözünürlük** Bağlantı noktasını değiştirmek için, VM Ölçeği Kümesini güncelleştirerek sistem durumu sondasını kaldırabilir, bağlantı noktasını güncelleyebilir ve sistem durumu sondasını yeniden yapılandırabilirsiniz.

## <a name="additional-network-captures"></a>Ek ağ yakalamaları
Bir destek servis talebi açmaya karar verirseniz, daha hızlı bir çözüm için aşağıdaki bilgileri toplayın. Aşağıdaki testleri gerçekleştirmek için tek bir arka uç VM seçin:
- Sonda bağlantı noktası yanıtını test etmek için VNet içindeki arka uç VM'lerden birinden Psping'i kullanın (örneğin: 10.0.0.4:3389'u psping) ve sonuçları kaydedin. 
- Bu ping testlerinde yanıt alınmazsa, PsPing'i çalıştırırken arka uç VM'de ve VNet testi VM'de eşzamanlı netsh izlemesini çalıştırın ve Netsh izini durdurun. 
  
## <a name="next-steps"></a>Sonraki adımlar

Önceki adımlar sorunu çözmezse, bir [destek bileti](https://azure.microsoft.com/support/options/)açın.

