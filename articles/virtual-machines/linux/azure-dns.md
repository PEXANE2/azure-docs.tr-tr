---
title: Linux VM'ler için DNS Ad çözümleme seçenekleri
description: Azure IaaS'deki Linux sanal makineleri için, sağlanan DNS hizmetleri, karma harici DNS ve Bring Own DNS sunucunuzu içeren Ad Çözümü senaryoları.
author: RicksterCDN
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: 0910b31685aa408c319b40ea23782b11724b6237
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641721"
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Azure'daki Linux sanal makineleri için DNS Ad Çözümleme seçenekleri
Azure, tek bir sanal ağdaki tüm sanal makineler için varsayılan olarak DNS ad çözümlemesi sağlar. Azure'un barındırdığı sanal makinelerinizde kendi DNS hizmetlerinizi yapılandırarak kendi DNS ad çözümünüzü uygulayabilirsiniz. Aşağıdaki senaryolar, durumunuziçin uygun olanı seçmenize yardımcı olur.

* [Azure'un sağladığı ad çözünürlüğü](#name-resolution-that-azure-provides)
* [Kendi DNS sunucunuzu kullanarak ad çözünürlüğü](#name-resolution-using-your-own-dns-server)

Kullandığınız ad çözümleme türü, sanal makinelerinizin ve rol örneklerinizin birbiriyle nasıl iletişim kurması gerektiğine bağlıdır.

Aşağıdaki tablosenaryoları ve karşılık gelen ad çözümçözümlerini göstermektedir:

| **Senaryo** | **Çözüm** | **Soneki** |
| --- | --- | --- |
| Aynı sanal ağdaki rol örnekleri veya sanal makineler arasındaki ad çözümlemesi |Azure'un sağladığı ad çözünürlüğü |hostname veya tam nitelikli alan adı (FQDN) |
| Farklı sanal ağlardaki rol örnekleri veya sanal makineler arasındaki ad çözümlemesi |Azure (DNS proxy) tarafından çözümiçin sanal ağlar arasında sorguları iletir müşteri tarafından yönetilen DNS sunucuları. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-using-your-own-dns-server)bakın. |Yalnızca FQDN |
| Azure'daki rol örneklerinden veya sanal makinelerden şirket içi bilgisayarların ve hizmet adlarının çözümü |Müşteri tarafından yönetilen DNS sunucuları (örneğin, şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya bölge aktarımları kullanılarak senkronize edilmiş bir DNS ikincil). [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-using-your-own-dns-server)bakın. |Yalnızca FQDN |
| Azure ana bilgisayar adlarının şirket içi bilgisayarlardan çözümü |Sorguları ilgili sanal ağdaki müşteri tarafından yönetilen bir DNS proxy sunucusuna iletin. Proxy sunucusu sorguları çözüm için Azure'a ileter. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-using-your-own-dns-server)bakın. |Yalnızca FQDN |
| İç IP'ler için ters DNS |[Kendi DNS sunucunuzu kullanarak ad çözünürlüğü](#name-resolution-using-your-own-dns-server) |yok |

## <a name="name-resolution-that-azure-provides"></a>Azure'un sağladığı ad çözünürlüğü
Azure, herkese açık DNS adlarının çözünürlüğünün yanı sıra, sanal makineler ve aynı sanal ağdaki rol örnekleri için dahili ad çözünürlüğü sağlar. Azure Kaynak Yöneticisi'ni temel alan sanal ağlarda, DNS soneki sanal ağ da tutarlıdır; FQDN gerekli değildir. DNS adları hem ağ arabirim kartlarına (NIC' ler) hem de sanal makinelere atanabilir. Azure'un sağladığı ad çözümlemesi herhangi bir yapılandırma gerektirmese de, önceki tabloda görüldüğü gibi tüm dağıtım senaryoları için uygun bir seçim değildir.

### <a name="features-and-considerations"></a>Özellikler ve dikkat edilecek hususlar
**Özellik:**

* Azure'un sağladığı ad çözümlemesi için yapılandırma gerekmez.
* Azure'un sağladığı ad çözümleme hizmeti son derece kullanılabilir. Kendi DNS sunucularınızın kümelerini oluşturmanız ve yönetmeniz gerekmez.
* Azure'un sağladığı ad çözümleme hizmeti, hem şirket içi hem de Azure ana bilgisayar adlarını çözmek için kendi DNS sunucularınızla birlikte kullanılabilir.
* FQDN'ye gerek kalmadan sanal ağlardaki sanal makineler arasında ad çözümlemesi sağlanır.
* Otomatik oluşturulan adlarla çalışmak yerine dağıtımlarınızı en iyi açıklayan ana bilgisayar adlarını kullanabilirsiniz.

**Husus -lar:**

* Azure'un oluşturduğu DNS soneki değiştirilemez.
* Kendi kayıtlarınızı el ile kaydedemezsiniz.
* WINS ve NetBIOS desteklenmez.
* Ana bilgisayar adları DNS uyumlu olmalıdır.
    Adlar yalnızca 0-9, a-z ve '-' kullanmalıdır ve '-' ile başlayamaz veya bitiremez. Bkz. RFC 3696 Bölüm 2.
* DNS sorgu trafiği her sanal makine için daraltılır. Azaltma çoğu uygulamayı etkilememelidir.  İstek azaltma gözlenirse, istemci tarafı önbelleğe alma nın etkin olduğundan emin olun.  Daha fazla bilgi için [azure'un sağladığı ad çözümlemesi için en iyi şekilde alma](#getting-the-most-from-name-resolution-that-azure-provides)'ya bakın.

### <a name="getting-the-most-from-name-resolution-that-azure-provides"></a>Azure'un sağladığı ad çözümlemesi en iyi şekilde elde
**İstemci tarafı önbelleğe alma:**

Bazı DNS sorguları ağ üzerinden gönderilmez. İstemci tarafı önbelleğe alma, yinelenen DNS sorgularını yerel bir önbellekten çözerek gecikme süresini azaltmaya ve ağ tutarsızlıklarına karşı dayanıklılığı artırmaya yardımcı olur. DNS kayıtları, önbelleğin kayıt tazeliğini etkilemeden kaydı mümkün olduğunca uzun süre saklamasını sağlayan bir Zaman-To-Live (TTL) içerir. Sonuç olarak, istemci tarafı önbelleğe alma çoğu durum için uygundur.

Bazı Linux dağıtımları varsayılan olarak önbelleğe alma içermez. Zaten yerel bir önbellek olup olmadığını kontrol ettikten sonra her Linux sanal makinesine bir önbellek eklemenizi öneririz.

Dnsmasq gibi birkaç farklı DNS önbelleğe alma paketi mevcuttur. Burada en yaygın dağılımları dnsmasq yüklemek için adımlar şunlardır:

**Ubuntu (resolvconf kullanır)**
  * dnsmasq paketini ("sudo apt-get install dnsmasq") yükleyin.

**SUSE (netconf kullanır)**:
1. dnsmasq paketini ("sudo zypper install dnsmasq") yükleyin.
2. Dnsmasq hizmetini etkinleştirin ("systemctl etkinleştirdin dnsmasq.service").
3. Dnsmasq hizmetini başlatın ("systemctl başlangıç dnsmasq.service").
4. "/etc/sysconfig/network/config" adlı şeyi edin ve NETCONFIG_DNS_FORWARDER="" ile "dnsmasq" olarak değiştirin.
5. Önbelleği yerel DNS çözümleyicisi olarak ayarlamak için resolv.conf 'ı ("netconfig update") güncelleştirin.

**Rogue Wave Software tarafından CentOS (eski OpenLogic; NetworkManager kullanır)**
1. Dnsmasq paketini ("sudo yum install dnsmasq") yükleyin.
2. Dnsmasq hizmetini etkinleştirin ("systemctl etkinleştirdin dnsmasq.service").
3. Dnsmasq hizmetini başlatın ("systemctl başlangıç dnsmasq.service").
4. "Prepend etki alanı adı-sunucuları 127.0.0.1;" için "/etc/dhclient-eth0.conf" ekleyin.
5. Önbelleği yerel DNS çözümleyicisi olarak ayarlamak için ağ hizmetini yeniden başlatma ("hizmet ağı yeniden başlatma")

> [!NOTE]
> : 'dnsmasq' paketi Linux için kullanılabilen birçok DNS önbelleklerinden yalnızca biridir. Kullanmadan önce, gereksinimlerinize uygunluğunu ve başka bir önbelleğin yüklenmediğini kontrol edin.
>
>

**İstemci tarafı yeniden denemeleri**

DNS öncelikle bir UDP protokolüdür. UDP protokolü ileti teslimini garanti etmediğinden, DNS protokolü yeniden deneme mantığını kendisi işler. Her DNS istemcisi (işletim sistemi) oluşturucunun tercihine bağlı olarak farklı yeniden deneme mantığı gösterebilir:

* Windows işletim sistemleri bir saniye sonra yeniden deneyin ve sonra tekrar başka bir iki, dört ve başka bir dört saniye sonra.
* Varsayılan Linux kurulumu beş saniye sonra yeniden çalışır.  Bunu, bir saniyelik aralıklarla beş kez yeniden denemek için değiştirmelisiniz.  

Bir Linux sanal makinesindeki geçerli ayarları kontrol etmek için , 'cat /etc/resolv.conf', ve örneğin 'seçenekler' satırına bakın:

    options timeout:1 attempts:5

resolv.conf dosyası otomatik olarak oluşturulur ve düzenlenmemelidir. 'Seçenekler' satırını ekleyen belirli adımlar dağılıma göre değişir:

**Ubuntu** (resolvconf kullanır)
1. Seçenekler satırını '/etc/resolvconf/resolv.conf.d/head'e ekleyin.
2. Güncellemek için 'resolvconf -u' çalıştırın.

**SUSE** (netconf kullanır)
1. '/etc/sysconfig/network/config' parametresine NETCONFIG_DNS_RESOLVER_OPTIONS="" parametresine 'zaman aşınması:1 deneme:5' ekleyin.
2. Güncelleştirmek için 'netconfig güncelleştirmesini' çalıştırın.

**Rogue Wave Software tarafından CentOS (eski OpenLogic)** (NetworkManager kullanır)
1. 'RES_OPTIONS="zaman aşıntIsı:1 deneme:5"' '/etc/sysconfig/network'e ekleyin.
2. Güncelleştirmek için 'servis ağı yeniden başlat' çalıştırın.

## <a name="name-resolution-using-your-own-dns-server"></a>Kendi DNS sunucunuzu kullanarak ad çözünürlüğü
Ad çözümleme gereksinimleriniz Azure'un sağladığı özelliklerin ötesine geçebilir. Örneğin, sanal ağlar arasında DNS çözünürlüğü gerekebilir. Bu senaryoyu kapsayacak şekilde kendi DNS sunucularınızı kullanabilirsiniz.  

Sanal ağdaki DNS sunucuları, aynı sanal ağdaki ana bilgisayar adlarını çözmek için Azure'un özyinelemeli çözümleyicilerine DNS sorgularını iletebilir. Örneğin, Azure'da çalışan bir DNS sunucusu, kendi DNS bölge dosyaları için DNS sorgularına yanıt verebilir ve diğer tüm sorguları Azure'a iletebilir. Bu işlev, sanal makinelerin hem bölge dosyalarınızdaki girişlerinizi hem de Azure'un sağladığı ana bilgisayar adlarını (iletme aracı aracılığıyla) görmesini sağlar. Azure'un özyinelemeli çözümleyicilerine erişim sanal IP 168.63.129.16 üzerinden sağlanır.

DNS iletme, sanal ağlar arasında DNS çözünürlüğü sağlar ve şirket içi makinelerinizin Azure'un sağladığı ana bilgisayar adlarını çözmesini sağlar. Sanal makinenin ana bilgisayar adını çözümlemek için, DNS sunucu sanal makinesinin aynı sanal ağda bulunması ve ana bilgisayar sorgusunu Azure'a iletecek şekilde yapılandırılması gerekir. DNS soneki her sanal ağda farklı olduğundan, DNS sorgularını çözüm için doğru sanal ağa göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki resimde, bu yöntemi kullanarak sanal ağlar arasında DNS çözünürlüğü yapan iki sanal ağ ve şirket içi ağ gösterilmektedir:

![Sanal ağlar arasında DNS çözünürlüğü](./media/azure-dns/inter-vnet-dns.png)

Azure'un sağladığı ad çözümlemesi kullandığınızda, her sanal makineye DHCP kullanılarak dahili DNS soneki sağlanır. Kendi ad çözümlemesi çözümünüzü kullandığınızda, sonek diğer DNS mimarilerine engel olduğundan bu sonek sanal makinelere sağlanmaz. FQDN ile makinelere başvurmak veya sanal makinelerinizdeki sonekyapılandırmak için PowerShell veya API'yi kullanarak soneki belirleyebilirsiniz:

* Azure Kaynak Yöneticisi tarafından yönetilen sanal ağlar için [sonek, ağ arabirimi kartı](https://msdn.microsoft.com/library/azure/mt163668.aspx) kaynağı üzerinden kullanılabilir. Nic'in `azure network public-ip show <resource group> <pip name>` FQDN'sini içeren genel IP'nizin ayrıntılarını görüntülemek için komutu da çalıştırabilirsiniz.

Sorguları Azure'a iletmek gereksinimlerinize uygun değilse, kendi DNS çözümünüzü sağlamanız gerekir.  DNS çözümünüzün şu anlama olması gerekir:

* Örneğin [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md)üzerinden uygun ana bilgisayar adı çözünürlüğü sağlayın. DDNS kullanıyorsanız, DNS kayıt atma devre dışı kalmanız gerekebilir. Azure'un DHCP kiralamaları çok uzundur ve atma işlemi DNS kayıtlarını zamanından önce kaldırabilir.
* Dış etki alanı adlarının çözülmesine izin vermek için uygun özyinelemeli çözüm sağlayın.
* Hizmet sunduğu istemcilerden (Bağlantı noktası 53'teki TCP ve UDP) erişilebilir olun ve Internet'e erişebilirsiniz.
* Harici aracıların oluşturduğu tehditleri azaltmak için Internet'ten erişime karşı güvence altına alın.

> [!NOTE]
> En iyi performans için, Azure DNS sunucularında sanal makineler kullandığınızda, IPv6'yı devre dışı bırakıp her DNS sunucusu sanal makinesine [Örnek Düzeyinde Genel IP](../../virtual-network/virtual-networks-instance-level-public-ip.md) atayın.  
>
>
