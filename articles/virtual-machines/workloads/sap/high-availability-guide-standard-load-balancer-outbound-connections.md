---
title: SAP HA senaryolarında Azure VM'ler&Standart ILB için genel uç nokta bağlantısı
description: SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisi kullanan Sanal Makineler için genel uç nokta bağlantısı
services: virtual-machines-windows,virtual-network,storage,
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/07/2020
ms.author: radeltch
ms.openlocfilehash: 4fd01764c183098a8bd78d502eea7ab173fa22cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80293908"
---
# <a name="public-endpoint-connectivity-for-virtual-machines-using-azure-standard-load-balancer-in-sap-high-availability-scenarios"></a>SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisi kullanan Sanal Makineler için genel uç nokta bağlantısı

Bu makalenin kapsamı, genel bitiş noktası(lar) giden bağlantı sağlayacak yapılandırmaları açıklamaktır. Yapılandırmaları SUSE / RHEL için Pacemaker ile Yüksek Kullanılabilirlik bağlamında esas olarak bulunmaktadır.  

Yüksek kullanılabilirlik çözümünde Pacemaker'ı Azure çit aracısıyla kullanıyorsanız, VM'lerin Azure yönetim API'sine giden bağlantıya sahip olması gerekir.  
Makale, senaryonuz için en uygun seçeneği seçmenize olanak tanıyan çeşitli seçenekler sunar.  

## <a name="overview"></a>Genel Bakış

Kümeleme yoluyla SAP çözümleri için yüksek kullanılabilirlik uygularken, gerekli bileşenlerden biri [Azure Yük Dengeleyici'dir.](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Azure, standart ve temel olmak üzüntü olmak ü

Standart Azure yük dengeleyicisi, Temel yük dengeleyicisi üzerinde bazı avantajlar sunar. Örneğin, Azure Kullanılabilirlik bölgelerinde çalışır, daha kolay sorun giderme, gecikme süresini azaltmak için daha iyi izleme ve günlüğe kaydetme özelliklerine sahiptir. "HA bağlantı noktaları" özelliği tüm bağlantı noktalarını kapsar, yani artık tüm bağımsız bağlantı noktalarını listelemek gerekmez.  

Azure yük dengeleyicisinin temel ve standart SKU'su arasında bazı önemli farklar vardır. Bunlardan biri, giden trafiğin halka açık bitiş noktasına taşınmasıdır. Tam Basic karşı Standart SKU yük dengeleyici karşılaştırması için [Yük Dengeleyici SKU karşılaştırması'na](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)bakın.  
 
Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ek yapılandırma yapılmadığı sürece ortak uç noktalara giden bağlantı yoktur.  

Bir VM'ye genel BIR IP adresi atanmışsa veya VM,genel IP adresine sahip bir yük dengeleyicisinin arka uç havuzundaysa, ortak bitiş noktalarına giden bağlantıya sahip olur.  

SAP sistemleri genellikle hassas iş verileri içerir. SAP sistemlerini barındıran VM'lerin genel IP adreslerine sahip olması nadiren kabul edilebilir. Aynı zamanda, VM'den ortak bitiş noktalarına giden bağlantı gerektiren senaryolar da vardır.  

Azure ortak bitiş noktasına erişim gerektiren senaryoörnekleri şunlardır:  
- Pacemaker kümelerinde Eskrim mekanizması olarak Azure Çit Aracısı kullanma
- Azure Backup
- Azure Site Recovery  
- İşletim sistemini yamaiçin ortak depo kullanma
- SAP uygulama veri akışı, genel bitiş noktasına giden bağlantı gerektirebilir

SAP dağıtımınız ortak bitiş noktalarına giden bağlantı gerektirmiyorsa, ek yapılandırmayı uygulamanız gerekmez. Genel uç noktalardan gelen bağlantıya da gerek olmadığını varsayarak, yüksek kullanılabilirlik senaryonuz için dahili standart SKU Azure Yük Dengeleyicisi oluşturmak yeterlidir.  

> [!Note]
> Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ortak bitiş noktalarına yönlendirmeye izin verecek ek yapılandırma yapılmadığı sürece giden internet bağlantısı olmaz.  
>VM'lerin ortak IP adresleri varsa veya azure yük bakiyeleyicisinin arka uç havuzunda genel IP adresi varsa, VM'nin genel bitiş noktalarına giden bağlantısı zaten olacaktır.


Önce aşağıdaki kağıtları okuyun:

* Azure Standart Yük Dengeleyicisi
  * [Azure Standart Yük Dengeleyicisi'ne genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) - Azure Standart Yük dengeleyicisi, önemli ilkeler, kavramlar ve öğreticilere kapsamlı genel bakış 
  * [Azure'da giden bağlantılar](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#scenarios) - Azure'da giden bağlantının nasıl elde edilene ilişkin senaryolar
  * [Yük dengeleyici giden kuralları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)- yük dengeleyici giden kuralları kavramları açıklar ve nasıl giden kurallar oluşturmak için
* Azure Güvenlik Duvarı
  * [Azure Güvenlik Duvarı'na Genel Bakış](https://docs.microsoft.com/azure/firewall/overview)- Azure Güvenlik Duvarı'na genel bakış
  * [Öğretici: Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) - Azure Güvenlik Duvarı'nı Azure portalı üzerinden nasıl yapılandıracağınız la ilgili talimatlar
* [Sanal Ağlar -Kullanıcı tanımlı kurallar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) - Azure yönlendirme kavramları ve kuralları  
* [Güvenlik Grupları Hizmet Etiketleri](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) - Hizmet etiketleri ile Ağ Güvenlik Grupları nızı ve Güvenlik Duvarı yapılandırmanızı nasıl basitleştirebilirsiniz?

## <a name="additional-external-azure-standard-load-balancer-for-outbound-connections-to-internet"></a>İnternete giden bağlantılar için ek harici Azure Standart Yük Dengeleyicisi

Ortak bitiş noktalarında giden bağlantı elde etmek için bir seçenek, ortak bitiş noktasından VM gelen bağlantı izin vermeden, kamu IP adresi ile ikinci bir yük dengeleyici oluşturmak için, ikinci yük dengeleyici arka uç havuzuna VM'ler eklemek ve yalnızca [giden kuralları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)tanımlamak.  
VM'den gelen giden aramalar için erişilebilen ortak uç noktalarını denetlemek için [Ağ Güvenlik Grupları'nı](https://docs.microsoft.com/azure/virtual-network/security-overview) kullanın.  
Daha fazla bilgi için, belge [Giden bağlantılarda](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#scenarios)Senaryo 2'ye bakın.  
Yapılandırma aşağıdaki gibi görünür:  

![Ağ Güvenlik Grupları ile ortak uç noktalara bağlantı denetimi](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-public.png)

### <a name="important-considerations"></a>Önemli noktalar

- Aynı alt ağdaki birden fazla VM için, genel bitiş noktasına giden bağlantı sağlamak ve maliyeti optimize etmek için bir ek Toplu Yük Dengeleyicisi kullanabilirsiniz  
- VM'lerden hangi ortak bitiş noktalarına erişilebilenleri denetlemek için [Ağ Güvenlik Grupları'nı](https://docs.microsoft.com/azure/virtual-network/security-overview) kullanın. Ağ Güvenlik Grubu'nu alt ağa veya her VM'ye atayabilirsiniz. Mümkün olduğunda, güvenlik kurallarının karmaşıklığını azaltmak için [Hizmet etiketlerini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanın.  
- Ortak IP adresi ve giden kuralları olan Azure standart Yük dengeleyicisi, genel bitiş noktasına doğrudan erişim sağlar. Denetim ve günlüğe kaydetme için merkezi kurumsal çözüm üzerinden tüm giden trafiğin geçmesi için kurumsal güvenlik gereksinimleriniz varsa, bu senaryoyla gereksinimi yerine getiremeyebilirsiniz.  

>[!TIP]
>Mümkün olduğunda, Ağ Güvenlik Grubu'nun karmaşıklığını azaltmak için [Hizmet etiketlerini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanın. 

### <a name="deployment-steps"></a>Dağıtım adımları

1. Yük Dengeleyicisi Oluştur  
   1. Azure [portalında](https://portal.azure.com) Tüm kaynakları tıklatın, Ekle'yi tıklatın ve ardından **Yük Bakiyesi'ni** arayın  
   1. **Oluştur'u** tıklatın 
   1. Yük Dengeleyici Adı **MyPublicILB**  
   1. **Genel** **Türü,** Standart sku olarak seçin  
   1. **Ortak IP adresi oluştur'u** seçin ve **MyPublicILBFrondEndIP** adı olarak belirtin  
   1. Kullanılabilirlik bölgesi olarak **Bölge Yedekini** seçin  
   1. Gözden Geçir ve Oluştur'u tıklatın, ardından Oluştur'u tıklatın  
2. **MyBackendPoolOfPublicILB backend** havuzu oluşturun ve VM'ler ekleyin.  
   1. Sanal ağı seçin  
   1. VM'leri ve IP adreslerini seçin ve arka uç havuzuna ekleyin  
3. [Giden kurallar oluşturun.](https://docs.microsoft.com/azure/load-balancer/configure-load-balancer-outbound-cli#create-outbound-rule) Şu anda Azure portalından giden kurallar oluşturmak mümkün değildir. [Azure CLI](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)ile giden kurallar oluşturabilirsiniz.  

   ```azurecli
    az network lb outbound-rule create --address-pool MyBackendPoolOfPublicILB --frontend-ip-configs MyPublicILBFrondEndIP --idle-timeout 30 --lb-name MyPublicILB --name MyOutBoundRules  --outbound-ports 10000 --enable-tcp-reset true --protocol All --resource-group MyResourceGroup
   ```

4. Belirli Ortak Uç Noktalara erişimi kısıtlamak için Ağ Güvenliği grubu kuralları oluşturun. Varolan Ağ Güvenlik Grubu varsa, bunu ayarlayabilirsiniz. Aşağıdaki örnekte, Azure yönetim API'sine erişimin nasıl etkinleştirilen gösterilmektedir: 
   1. Ağ Güvenlik Grubuna Gidin
   1. Giden Güvenlik Kuralları'nı tıklatın
   1. Tüm giden **Internet**Erişimini **Reddetmek** için bir kural ekleyin.
   1. Tüm Internet erişimini reddetmek için kuralın önceliğinden daha düşük olan **AzureCloud'a**erişime **izin** vermek için bir kural ekleyin.


   Giden güvenlik kuralları aşağıdaki gibi görünür: 

   ![Genel IP ile İkinci Yük Dengeleyicisi ile giden bağlantı](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-network-security-groups.png)

   Azure Ağı güvenlik grupları hakkında daha fazla bilgi için [Güvenlik Grupları'na ](https://docs.microsoft.com/azure/virtual-network/security-overview)bakın. 

## <a name="azure-firewall-for-outbound-connections-to-internet"></a>Internet'e giden bağlantılar için Azure Güvenlik Duvarı

Ortak uç noktalardan VM'ye gelen bağlantıya izin vermeden, genel uç noktalara giden bağlantı elde etmek için başka bir seçenek de Azure Güvenlik Duvarı'dır. Azure Güvenlik Duvarı, yerleşik Yüksek Kullanılabilirlik ile yönetilen bir hizmettir ve birden çok Kullanılabilirlik Bölgesi'ni kapsayabilir.  
Ayrıca, Trafiği Azure Güvenlik Duvarı'ndan yönlendirmek için VM'lerin ve Azure yük dengeleyicisinin dağıtıldığı alt ağla ilişkili [kullanıcı tanımlı rotayı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes)da dağıtmanız gerekir.  
Azure Güvenlik Duvarı'nı nasıl dağıtılayabilirsiniz hakkında ayrıntılı bilgi [için](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)Bkz.  

Mimari gibi görünecek:

![Azure Güvenlik Duvarı ile giden bağlantı](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-firewall.png)

### <a name="important-considerations"></a>Önemli noktalar

- Azure güvenlik duvarı, yerleşik Yüksek Kullanılabilirlik ile bulut aladili hizmetidir ve yer değiştirmeyi destekler.
- AzureFirewallSubnet adında ek alt ağ gerektirir. 
- Büyük veri kümelerinin SAP VM'lerin bulunduğu sanal ağdan başka bir sanal ağdaki bir VM'ye veya genel bitiş noktasına aktarılması uygun maliyetli bir çözüm olmayabilir. Bu tür bir örnek, büyük yedeklemeleri sanal ağlar arasında kopyalamaktır. Ayrıntılar için Azure Güvenlik Duvarı fiyatlandırması için bkz.  
- Kurumsal Güvenlik Duvarı çözümü Azure Güvenlik Duvarı değilse ve merkezi kurumsal çözüm olsa da tüm giden trafik geçişine sahip olmak için güvenlik gereksinimleriniz varsa, bu çözüm pratik olmayabilir.  

>[!TIP]
>Mümkün olduğunda, Azure Güvenlik Duvarı kurallarının karmaşıklığını azaltmak için [Hizmet etiketlerini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanın.  

### <a name="deployment-steps"></a>Dağıtım adımları

1. Dağıtım adımları, Sanal Ağlarınız ve Alt ağınız vm'leriniz için zaten tanımlandığını varsayar.  
2. VMS ve Standart Yük Dengeleyicisinin dağıtıldığı aynı Sanal Ağ'da Subnet **AzureFirewallSubnet** oluşturun.  
   1. Azure portalında, Sanal Ağa Gidin: Tüm Kaynakları Tıklatın, Sanal Ağı Arayın, Sanal Ağ'ı Tıklatın, Alt Ağları Seçin.  
   1. Alt Net Ekle'yi tıklatın. Ad olarak **AzureFirewallSubnet** girin. Uygun Adres Aralığını girin. Kaydedin.  
3. Azure Güvenlik Duvarı oluşturun.  
   1. Azure portalında Tüm kaynakları seçin, Ekle, Güvenlik Duvarı, Oluştur'u tıklatın. Kaynak grubunu seçin (Sanal Ağ'ın olduğu aynı kaynak grubunu seçin).  
   1. Azure Güvenlik Duvarı kaynağı için ad girin. Örneğin, **MyAzureFirewall**.  
   1. Bölge'yi seçin ve VM'lerinizin dağıtıldığı Kullanılabilirlik bölgeleriyle uyumlu en az iki Kullanılabilirlik bölgesi seçin.  
   1. SAP VM'lerin ve Azure Standart Yük bakiyesinin dağıtıldığı Sanal Ağınızı seçin.  
   1. Genel IP Adresi: Oluştur'u tıklatın ve bir ad girin. Örneğin **MyFirewallPublicIP**için.  
4. Belirtilen ortak uç noktalara giden bağlantıya izin vermek için Azure Güvenlik Duvarı Kuralı oluşturun. Örnek, Azure Yönetimi API ortak bitiş noktasına erişime nasıl izin verilebildiğini gösterir.  
   1. Kurallar, Ağ Kuralı Koleksiyonu'nı seçin ve ardından Ağ Kuralı Koleksiyonu Ekle'yi tıklatın.  
   1. Adı: **MyOutboundRule**, Öncelik girin, Eylem **İzin**seçin .  
   1. Hizmet: Adı **ToAzureAPI**.  Protokol: **Herhangi bir**seçin. Kaynak Adresi: VM'lerin ve Standart Yük Dengeleyici'nin dağıtıldığı alt ağınızın aralığını girin: **11.97.0.0/24.** Hedef bağlantı <b>*</b>noktaları: girin.  
   1. Kaydet
   1. Azure Güvenlik Duvarı'nda konumlandırıldığınızdan genel bakış ı seçin. Azure Güvenlik Duvarı'nın Özel IP Adresini not edin.  
5. Azure Güvenlik Duvarı'na rota oluşturma  
   1. Azure portalında Tüm kaynakları seçin ve ardından Ekle, Rota Tablosu, Oluştur'u tıklatın.  
   1. Ad MyRouteTable girin, Abonelik, Kaynak grubu ve Konum (Sanal ağınızın ve Güvenlik Duvarınızın konumunu eşleştirin) seçin.  
   1. Kaydet  

   Güvenlik duvarı kuralı şu ![şekilde görünür: Azure Güvenlik Duvarı ile giden bağlantı](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-firewall-rule.png)

6. VM'lerinizin alt netinden **MyAzureFirewall'ın**özel IP'sine Kullanıcı Tanımlı Rota oluşturun.
   1. Rota Tablosu'nda konumlandırıldığından, Rotalar'ı tıklatın. Ekle'yi seçin. 
   1. Rota adı: ToMyAzureFirewall, Adres öneki: **0.0.0.0/0**. Sonraki hop türü: Sanal Cihaz'ı seçin. Sonraki atlama adresi: yapılandırılan güvenlik duvarının özel IP adresini girin: **11.97.1.4**.  
   1. Kaydet

## <a name="using-proxy-for-pacemaker-calls-to-azure-management-api"></a>Pacemaker için Proxy'yi Azure Yönetimi API'sine kullanma çağrıları

Pacemaker'ın Azure yönetimi API genel bitiş noktasına çağrılarına izin vermek için proxy'yi kullanabilirsiniz.  

### <a name="important-considerations"></a>Önemli noktalar

  - Zaten kurumsal proxy varsa, giden çağrıları genel bitiş noktalarına yönlendirebilirsiniz. Halka açık uç noktalara yapılan aramalar kurumsal kontrol noktasından geçecektir.  
  - Proxy yapılandırmasının Azure yönetim API'sine giden bağlantıya izin verdiğinden emin olun:`https://management.azure.com`  
  - VM'lerden Proxy'ye giden bir rota olduğundan emin olun  
  - Proxy yalnızca HTTP/HTTPS aramalarını işleyecek. Farklı protokoller (RFC gibi) üzerinden genel bitiş noktasına giden çağrıları yapmak için ek bir ihtiyaç varsa, alternatif çözüm gerekir  
  - Proxy çözümü, Pacemaker kümesinde kararsızlığı önlemek için son derece kullanılabilir olmalıdır  
  - Proxy'nin konumuna bağlı olarak, Azure Çit Aracısı'ndan Azure Yönetimi API'sine yapılan aramalarda ek gecikme gecikmesi sağlayabilir. Kurumsal proxy'niz hala şirketteyse, Kalp Pili kümeniz Azure'dayken gecikme süresini ölçün ve bu çözüm sizin için uygunsa düşünün  
  - Zaten yüksek kullanılabilir kurumsal proxy yerinde yoksa, müşteri ekstra maliyet ve karmaşıklık tahakkuk olacağını gibi biz bu seçeneği önermiyoruz. Bununla birlikte, Pacemaker'dan Azure Yönetimi genel API'sine giden bağlantıya izin vermek amacıyla ek proxy çözümü dağıtmaya karar verirseniz, proxy'nin yüksek oranda kullanılabilir olduğundan ve VM'lerden proxy'ye gecikme sinin düşük olduğundan emin olun.  

### <a name="pacemaker-configuration-with-proxy"></a>Proxy ile kalp pili yapılandırması 

Birçok farklı Proxy seçenekleri sektöründe mevcuttur. Proxy dağıtımı için adım adım yönergeleri bu belgenin kapsamı dışındadır. Aşağıdaki örnekte, proxy **MyProxyService** yanıt ve port **MyProxyPort**dinlerken varsayıyoruz.  
Kalp pilinin Azure yönetim API'si ile iletişim kurmasına izin vermek için, tüm küme düğümlerinde aşağıdaki adımları gerçekleştirin:  

1. Kalp pili yapılandırma dosyasını /etc/sysconfig/pacemaker'ı düzenleme ve aşağıdaki satırları (tüm küme düğümleri) ekleyin:

   ```console
   sudo vi /etc/sysconfig/pacemaker
   # Add the following lines
   http_proxy=http://MyProxyService:MyProxyPort
   https_proxy=http://MyProxyService:MyProxyPort
   ```

2. **Tüm** küme düğümlerinde kalp pili hizmetini yeniden başlatın.  
  - SUSE
 
     ```console
     # Place the cluster in maintenance mode
     sudo crm configure property maintenance-mode=true
     #Restart on all nodes
     sudo systemctl restart pacemaker
     # Take the cluster out of maintenance mode
     sudo crm configure property maintenance-mode=true
     ```

  - Red Hat  

     ```console
     # Place the cluster in maintenance mode
     sudo pcs property set maintenance-mode=true
     #Restart on all nodes
     sudo systemctl restart pacemaker
     # Take the cluster out of maintenance mode
     sudo pcs property set maintenance-mode=false
     ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da SUSE'da Pacemaker'ı nasıl yapılandırılasınız öğrenin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)
* [Azure'da Red Hat'te Pacemaker'ı nasıl yapılandırılatırın öğrenin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker)
