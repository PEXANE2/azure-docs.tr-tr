---
title: Azure sanal ağlarındaki kaynaklar için ad çözümlemesi
titlesuffix: Azure Virtual Network
description: Azure IaaS için ad çözümleme senaryoları, karma çözümler, farklı bulut hizmetleri, Active Directory ve kendi DNS sunucunuzu kullanma arasında.
services: virtual-network
documentationcenter: na
author: rohinkoul
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/2/2020
ms.author: rohink
ms.custom: fasttrack-edit
ms.openlocfilehash: d7d0699718642a7eb9f85b2e8a86623092c34365
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010571"
---
# <a name="name-resolution-for-resources-in-azure-virtual-networks"></a>Azure sanal ağlarındaki kaynaklar için ad çözümlemesi

IaaS, PaaS ve karma çözümleri barındırmak için Azure'u nasıl kullandığınıza bağlı olarak, sanal makinelere (VM) ve sanal ağda dağıtılan diğer kaynakların birbirleriyle iletişim kurmasına izin verebilirsiniz. IP adreslerini kullanarak iletişimi etkinleştirebilmenize rağmen, kolayca hatırlanabilecek ve değişmeyebileceğiniz adları kullanmak çok daha kolaydır. 

Sanal ağlarda dağıtılan kaynakların alan adlarını dahili IP adreslerine çözmesi gerektiğinde, üç yöntemden birini kullanabilirler:

* [Azure DNS özel bölgeleri](../dns/private-dns-overview.md)
* [Azure tarafından sağlanan ad çözümü](#azure-provided-name-resolution)
* [Kendi DNS sunucunuzu kullanan ad çözünürlüğü](#name-resolution-that-uses-your-own-dns-server) (sorguları Azure sağlanan DNS sunucularına iletebilir)

Kullandığınız ad çözümlemesi türü, kaynaklarınızın birbirleriyle nasıl iletişim kurmaları gerektiğine bağlıdır. Aşağıdaki tablosenaryoları ve karşılık gelen ad çözümçözümlerini göstermektedir:

> [!NOTE]
> Azure DNS özel bölgeleri tercih edilen çözümdür ve Size DNS bölgelerinizi ve kayıtlarınızı yönetmede esneklik sağlar. Daha fazla bilgi için bkz. [Azure DNS'yi özel etki alanları için kullanma](../dns/private-dns-overview.md).

> [!NOTE]
> Azure Sağlanan DNS kullanıyorsanız, sanal makinelerinize otomatik olarak uygun DNS soneki uygulanır. Diğer tüm seçenekler için Tam Nitelikli Alan Adları (FQDN) kullanmanız veya sanal makinelerinize uygun DNS sonekini el ile uygulamanız gerekir.

| **Senaryo** | **Çözüm** | **DNS Son Ek** |
| --- | --- | --- |
| Aynı sanal ağda bulunan VM'ler arasındaki ad çözümlemesi veya aynı bulut hizmetinde Azure Bulut Hizmetleri rol örnekleri. | [Azure DNS özel bölgeleri](../dns/private-dns-overview.md) veya [Azure tarafından sağlanan ad çözümü](#azure-provided-name-resolution) |Hostname veya FQDN |
| Farklı sanal ağlardaki VM'ler arasında ad çözümlemesi veya farklı bulut hizmetlerinde rol örnekleri. |[Azure DNS özel bölgeleri](../dns/private-dns-overview.md) veya Müşteri tarafından yönetilen DNS sunucuları, Azure tarafından çözüme kavuşturuiçin sanal ağlar arasında sorgular iletir (DNS proxy). [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-that-uses-your-own-dns-server)bakın. |Yalnızca FQDN |
| Aynı sanal ağdaki rol örnekleri veya VM'ler için sanal ağ tümleştirmesini kullanan bir Azure Uygulama Hizmetinin (Web Uygulaması, İşlev veya Bot) ad çözünürlüğü. |Müşteri tarafından yönetilen DNS sunucuları, Azure (DNS proxy) tarafından çözüme kavuşturuluz için sanal ağlar arasında sorgular iletir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-that-uses-your-own-dns-server)bakın. |Yalnızca FQDN |
| App Service Web Apps'tan aynı sanal ağdaki VM'lere ad çözünürlüğü. |Müşteri tarafından yönetilen DNS sunucuları, Azure (DNS proxy) tarafından çözüme kavuşturuluz için sanal ağlar arasında sorgular iletir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-that-uses-your-own-dns-server)bakın. |Yalnızca FQDN |
| Bir sanal ağdaki App Service Web Apps'tan farklı bir sanal ağdaki VM'lere ad çözünürlüğü. |Müşteri tarafından yönetilen DNS sunucuları, Azure (DNS proxy) tarafından çözüme kavuşturuluz için sanal ağlar arasında sorgular iletir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-that-uses-your-own-dns-server)bakın. |Yalnızca FQDN |
| Azure'daki VM'lerden veya rol örneklerinden şirket içi bilgisayar ve hizmet adlarının çözümü. |Müşteri tarafından yönetilen DNS sunucuları (şirket içi etki alanı denetleyicisi, yerel salt okunur etki alanı denetleyicisi veya örneğin bölge aktarımları kullanılarak senkronize edilmiş bir DNS ikincil bilgileri). [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-that-uses-your-own-dns-server)bakın. |Yalnızca FQDN |
| Azure ana bilgisayar adlarının şirket içi bilgisayarlardan çözünürlüğü. |Sorguları ilgili sanal ağdaki müşteri tarafından yönetilen bir DNS proxy sunucusuna iletir, proxy sunucusu sorguları çözüm için Azure'a iletir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](#name-resolution-that-uses-your-own-dns-server)bakın. |Yalnızca FQDN |
| Dahili IP'ler için Ters DNS. |[Azure DNS özel bölgeleri](../dns/private-dns-overview.md) veya [Azure tarafından sağlanan ad çözünürlüğü](#azure-provided-name-resolution) veya [Ad çözümlemesi, kendi DNS sunucunuzu kullanır.](#name-resolution-that-uses-your-own-dns-server) |Uygulanamaz |
| Sanal ağda değil, farklı bulut hizmetlerinde bulunan VM'ler veya rol örnekleri arasındaki ad çözümlemesi. |Geçerli değildir. Farklı bulut hizmetlerindeki VM'ler ve rol örnekleri arasındaki bağlantı sanal ağ dışında desteklenmez. |Uygulanamaz|

## <a name="azure-provided-name-resolution"></a>Azure tarafından sağlanan ad çözümü

Azure sağlanan ad çözümlemesi yalnızca temel yetkili DNS özellikleri sağlar. Bu seçeneği kullanırsanız, DNS bölge adları ve kayıtları otomatik olarak Azure tarafından yönetilir ve DNS bölge adlarını veya DNS kayıtlarının yaşam döngüsünü denetleyemeyeceksiniz. Sanal ağlarınız için tam özellikli bir DNS çözümüne ihtiyacınız varsa [Azure DNS özel bölgeleri](../dns/private-dns-overview.md) veya Müşteri tarafından yönetilen [DNS sunucularını](#name-resolution-that-uses-your-own-dns-server)kullanmanız gerekir.

Azure, ortak DNS adlarının çözünürlüğünün yanı sıra, aynı sanal ağ veya bulut hizmetinde bulunan VM'ler ve rol örnekleri için dahili ad çözümlemesi sağlar. Bulut hizmetindeki VM'ler ve örnekler aynı DNS sonekini paylaşır, bu nedenle ana bilgisayar adı tek başına yeterlidir. Ancak klasik dağıtım modeli kullanılarak dağıtılan sanal ağlarda, farklı bulut hizmetlerinin farklı DNS sonekleri vardır. Bu durumda, farklı bulut hizmetleri arasındaki adları çözmek için FQDN gerekir. Azure Kaynak Yöneticisi dağıtım modeli kullanılarak dağıtılan sanal ağlarda, DNS soneki sanal ağdaki tüm sanal makinelerde tutarlıdır, bu nedenle FQDN'ye gerek yoktur. DNS adları hem VM'lere hem de ağ arabirimlerine atanabilir. Azure tarafından sağlanan ad çözümlemesi herhangi bir yapılandırma gerektirmese de, önceki tabloda açıklandığı gibi tüm dağıtım senaryoları için uygun bir seçim değildir.

> [!NOTE]
> Bulut hizmetleri web'ini ve çalışan rollerini kullanırken, Azure Hizmet Yönetimi REST API'sini kullanarak rol örneklerinin dahili IP adreslerine de erişebilirsiniz. Daha fazla bilgi için [Service Management REST API Başvurusu'na](https://msdn.microsoft.com/library/azure/ee460799.aspx)bakın. Adres rol adı ve örnek numarası temel alınr. 
>

### <a name="features"></a>Özellikler

Azure tarafından sağlanan ad çözünürlüğü aşağıdaki özellikleri içerir:
* Kullanım kolaylığı. Yapılandırma gerekmez.
* Yüksek kullanılabilirlik. Kendi DNS sunucularınızın kümelerini oluşturmanız ve yönetmeniz gerekmez.
* Hem şirket içi hem de Azure ana bilgisayar adlarını çözmek için hizmeti kendi DNS sunucunuzla birlikte kullanabilirsiniz.
* FQDN'ye gerek kalmadan, aynı bulut hizmetinde VM'ler ve rol örnekleri arasında ad çözümlemesi kullanabilirsiniz.
* Azure Kaynak Yöneticisi dağıtım modelini kullanan sanal ağlarda, FQDN'ye gerek kalmadan SANAL Ağlar'da Sanal Ağlar arasında ad çözümlemesi kullanabilirsiniz. Klasik dağıtım modelindeki sanal ağlar, farklı bulut hizmetlerinde adları çözerken bir FQDN gerektirir. 
* Otomatik oluşturulan adlarla çalışmak yerine, dağıtımlarınızı en iyi açıklayan ana bilgisayar adlarını kullanabilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure tarafından sağlanan ad çözümlemesi kullanırken göz önünde bulundurulması gereken noktalar:
* Azure tarafından oluşturulan DNS soneki değiştirilemez.
* DNS araması sanal bir ağa doğru kapsamlıdır. Bir sanal ağ için oluşturulan DNS adları diğer sanal ağlardan çözülemez.
* Kendi kayıtlarınızı el ile kaydedemezsiniz.
* WINS ve NetBIOS desteklenmez. Windows Gezgini'nde VM'lerinizi göremezsiniz.
* Ana bilgisayar adları DNS uyumlu olmalıdır. Adlar yalnızca 0-9, a-z ve '-' kullanmalıdır ve '-' ile başlayamaz veya bitiremez.
* DNS sorgu trafiği her VM için daraltılır. Azaltma çoğu uygulamayı etkilememelidir. İstek azaltma gözlenirse, istemci tarafı önbelleğe alma nın etkin olduğundan emin olun. Daha fazla bilgi için [Bkz. DNS istemci yapılandırması.](#dns-client-configuration)
* Klasik bir dağıtım modelinde her sanal ağ için yalnızca ilk 180 bulut hizmetindeki VM'ler kaydedilir. Bu sınır Azure Kaynak Yöneticisi'ndeki sanal ağlar için geçerli değildir.
* Azure DNS IP adresi 168.63.129.16'dır. Bu statik bir IP adresidir ve değişmez.

### <a name="reverse-dns-considerations"></a>Ters DNS Hususlar
Ters DNS tüm ARM tabanlı sanal ağlarda desteklenir. Sanal makinelerin IP adreslerini sanal makinelerin FQDN'lerine eşlemek için ters DNS sorguları (PTR sorguları) verebilirsiniz.
* Sanal makinelerin IP adresleri için tüm PTR sorguları form \[vmname\].internal.cloudapp.net FQDN'leri döndürecek
* Form \[vmname\].internal.cloudapp.net FQDNs ileri arama sanal makineye atanan IP adresine çözülecektir.
* Sanal ağ bir kayıt sanal ağı olarak Azure [DNS özel bölgelere](../dns/private-dns-overview.md) bağlıysa, ters DNS sorguları iki kayıt döndürer. Bir kayıt formu \[vmname\]olacaktır . [priatednszonename] ve diğer form \[vmname\]olacaktır .internal.cloudapp.net
* Ters DNS araması, diğer sanal ağlara bakSa bile belirli bir sanal ağa kapsamlıdır. Eşli sanal ağlarda bulunan sanal makinelerin IP adresleri için Ters DNS sorguları (PTR sorguları) NXDOMAIN döndürecek.

> [!NOTE]
> Ters DNS araması yapmak istiyorsanız, sanal ağ üzerinden bir ters arama bölgesi (in-addr.arpa) [Azure DNS özel bölgeleri](../dns/private-dns-overview.md) oluşturabilir ve birden çok sanal ağa bağlayabilirsiniz. Ancak sanal makineler için ters DNS kayıtlarını el ile yönetmeniz gerekir.
>


## <a name="dns-client-configuration"></a>DNS istemci yapılandırması

Bu bölüm istemci tarafı önbelleğe alma ve istemci tarafı yeniden denemelerini kapsar.

### <a name="client-side-caching"></a>İstemci tarafı önbelleğe alma

Her DNS sorgusunun ağ üzerinden gönderilmesi gerekmez. İstemci tarafı önbelleğe alma, yinelenen DNS sorgularını yerel bir önbellekten çözerek gecikme süresini azaltmaya ve ağ hatalarına karşı dayanıklılığı artırmaya yardımcı olur. DNS kayıtları, önbelleğin kayıt tazeliğini etkilemeden kaydı mümkün olduğunca uzun süre saklamasına olanak tanıyan, canlı yayın (TTL) mekanizması içerir. Bu nedenle, istemci tarafı önbelleğe alma çoğu durum için uygundur.

Varsayılan Windows DNS istemcisinde yerleşik bir DNS önbelleği vardır. Bazı Linux dağıtımları varsayılan olarak önbelleğe alma içermez. Zaten yerel bir önbellek olmadığını fark ederseniz, her Linux VM'sine bir DNS önbelleği ekleyin.

Farklı DNS önbelleğe alma paketleri (dnsmasq gibi) bir dizi vardır. En yaygın dağılımlara dnsmasq nasıl yüklenir:

* **Ubuntu (resolvconf kullanır)**:
  * Dnsmasq paketini `sudo apt-get install dnsmasq`yükleyin.
* **SUSE (netconf kullanır)**:
  * Dnsmasq paketini `sudo zypper install dnsmasq`yükleyin.
  * Dnsmasq hizmetini ' `systemctl enable dnsmasq.service`ile etkinleştirin. 
  * Dnsmasq hizmetini ' `systemctl start dnsmasq.service`ile başlatın. 
  * **/etc/sysconfig/network/config'i**edin ve *NETCONFIG_DNS_FORWARDER=""* ile *dnsmasq'ı*değiştirin.
  * Önbelleği yerel DNS çözümleyicisi olarak ayarlamak için resolv.conf ile `netconfig update`güncelleştirin.
* **CentOS (NetworkManager kullanır)**:
  * Dnsmasq paketini `sudo yum install dnsmasq`yükleyin.
  * Dnsmasq hizmetini ' `systemctl enable dnsmasq.service`ile etkinleştirin.
  * Dnsmasq hizmetini ' `systemctl start dnsmasq.service`ile başlatın.
  * *Prepend etki alanı adı-sunucuları 127.0.0.1 ekleyin;* **/etc/dhclient-eth0.conf**.
  * Önbelleği yerel `service network restart`DNS çözümleyicisi olarak ayarlamak için ağ hizmetini yeniden başlatın.

> [!NOTE]
> Dnsmasq paketi Linux için kullanılabilen birçok DNS önbelleklerinden sadece biridir. Kullanmadan önce, özel gereksinimlerinize uygunluğunu kontrol edin ve başka bir önbellek yüklenmeden kontrol edin.

    
### <a name="client-side-retries"></a>İstemci tarafı yeniden denemeleri

DNS öncelikle bir UDP protokolüdür. UDP protokolü ileti teslimini garanti etmediğinden, yeniden deneme mantığı DNS protokolünde işlenir. Her DNS istemcisi (işletim sistemi), oluşturucunun tercihine bağlı olarak farklı yeniden deneme mantığı sergileyebilir:

* Windows işletim sistemleri bir saniye sonra yeniden dener ve iki saniye, dört saniye ve dört saniye sonra tekrar dener. 
* Varsayılan Linux kurulumu beş saniye sonra yeniden çalışır. Yeniden deneme belirtimlerini bir saniyelik aralıklarla beş kez değiştirmenizi öneririz.

Linux VM'deki geçerli ayarları `cat /etc/resolv.conf`'' ile kontrol edin. *Seçenekler* satırına bakın, örneğin:

```bash
options timeout:1 attempts:5
```

resolv.conf dosyası genellikle otomatik olarak oluşturulur ve düzenlenmemelidir. *Seçenekler* satırını eklemek için belirli adımlar dağılıma göre değişir:

* **Ubuntu** (resolvconf kullanır):
  1. **/etc/resolvconf/resolv.conf.d/tail** *seçenekleri* satırını ekleyin.
  2. Güncellemek için çalıştırın. `resolvconf -u`
* **SUSE** (netconf kullanır):
  1. **/etc/sysconfig/network/config'deki** **NETCONFIG_DNS_RESOLVER_OPTIONS=""** parametresine *zaman aşım:1 girişimleri:5* ekleyin.
  2. Güncellemek için çalıştırın. `netconfig update`
* **CentOS** (NetworkManager kullanır):
  1. **/etc/NetworkManager/dispatcher.d/11-dhclient'a** *yankı "seçenekleri zaman ayarı:1 girişimleri:5"* ekleyin.
  2. 'ile `service network restart`güncelleştirin.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>Kendi DNS sunucunuzu kullanan ad çözünürlüğü

Bu bölüm VM'leri, rol örneklerini ve web uygulamalarını kapsar.

### <a name="vms-and-role-instances"></a>VM'ler ve rol örnekleri

Ad çözümleme gereksinimleriniz Azure tarafından sağlanan özelliklerin ötesine geçebilir. Örneğin, Sanal ağlar arasında DNS adlarını çözmek için Microsoft Windows Server Active Directory etki alanlarını kullanmanız gerekebilir. Azure, bu senaryoları kapsayacak şekilde kendi DNS sunucularınızı kullanma olanağı sağlar.

Sanal ağ daki DNS sunucuları, DNS sorgularını Azure'daki özyinelemeli çözümleyicilere iletebilir. Bu, bu sanal ağdaki ana bilgisayar adlarını çözümlemenizi sağlar. Örneğin, Azure'da çalışan bir etki alanı denetleyicisi (DC), etki alanları için DNS sorgularına yanıt verebilir ve diğer tüm sorguları Azure'a iletebilir. İletme sorguları, VM'lerin hem şirket içi kaynaklarınızı (DC üzerinden) hem de Azure tarafından sağlanan ana bilgisayar adlarınızı (iletme yoluyla) görmelerine olanak tanır. Azure'daki özyinelemeli çözümleyicilere erişim sanal IP 168.63.129.16 üzerinden sağlanır.

DNS iletme, sanal ağlar arasında DNS çözünürlüğü sağlar ve şirket içi makinelerinizin Azure tarafından sağlanan ana bilgisayar adlarını çözmesine olanak tanır. Bir VM'nin ana bilgisayar adını çözmek için, DNS sunucusu VM'nin aynı sanal ağda ikamet etmesi ve ana bilgisayar ad sorgularını Azure'a iletecek şekilde yapılandırılması gerekir. DNS soneki her sanal ağda farklı olduğundan, DNS sorgularını çözüm için doğru sanal ağa göndermek için koşullu iletme kurallarını kullanabilirsiniz. Aşağıdaki resimde, bu yöntemi kullanarak sanal ağlar arasında DNS çözünürlüğü yapan iki sanal ağ ve şirket içi ağ gösterilmektedir. Örnek bir DNS iletme, [Azure Quickstart Şablonları galerisinde](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ve [GitHub'da](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder)kullanılabilir.

> [!NOTE]
> Bir rol örneği, aynı sanal ağ içinde VM'lerin ad çözümlemesi gerçekleştirebilir. Bunu, VM'nin ana bilgisayar adı ve **internal.cloudapp.net** DNS sonekinden oluşan FQDN'yi kullanarak yapar. Ancak, bu durumda, ad çözümlemesi yalnızca rol örneğinin Rol [Şeması'nda (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/jj156212.aspx)tanımlanan VM adı varsa başarılı olur.
> `<Role name="<role-name>" vmName="<vm-name>">`
>
> Başka bir sanal ağdaki VM'lerin ad çözümünü gerçekleştirmesi gereken rol örnekleri **(internal.cloudapp.net** soneki kullanarak FQDN) bunu bu bölümde açıklanan yöntemi kullanarak yapmak zorunda (iki sanal ağ arasında iletileyen özel DNS sunucuları).
>

![Sanal ağlar arasındaki DNS diyagramı](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Azure tarafından sağlanan ad çözümlemesi kullanırken, Azure Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP), her VM'ye dahili bir DNS soneki **(.internal.cloudapp.net)** sağlar. Ana bilgisayar adı kayıtları **internal.cloudapp.net** bölgesinde olduğundan, bu sonek ana bilgisayar adı çözümlemesini sağlar. Kendi ad çözümünüzü kullandığınızda, diğer DNS mimarilerine (etki alanı birleştirilmiş senaryolar gibi) müdahale ettiğinden, bu sonek VM'lere sağlanmaz. Bunun yerine, Azure çalışmayan bir yer tutucu *(reddog.microsoft.com)* sağlar.

Gerekirse, PowerShell veya API kullanarak dahili DNS soneki belirleyebilirsiniz:

* Azure Kaynak Yöneticisi dağıtım modellerindeki sanal ağlar için sonek, [ağ arabirimi REST API,](https://docs.microsoft.com/rest/api/virtualnetwork/networkinterfaces) [Get-AzNetworkInterface](/powershell/module/az.network/get-aznetworkinterface) PowerShell cmdlet ve [az network nic show](/cli/azure/network/nic#az-network-nic-show) Azure CLI komutu üzerinden kullanılabilir.
* Klasik dağıtım modellerinde, sonek Dağıtım [API Al](https://msdn.microsoft.com/library/azure/ee460804.aspx) çağrısı veya [Get-AzureVM -Hata Ayıklama](/powershell/module/servicemanagement/azure/get-azurevm) cmdlet üzerinden kullanılabilir.

Sorguları Azure'a iletmek gereksinimlerinize uygun değilse, kendi DNS çözümünüzü sağlamanız gerekir. DNS çözümünüzün şu anlama olması gerekir:

* Örneğin, [DDNS](virtual-networks-name-resolution-ddns.md)aracılığıyla uygun ana bilgisayar ad çözünürlüğü sağlayın. DDNS kullanıyorsanız, DNS kayıt atma devre dışı kalmanız gerekebilir. Azure DHCP kiralamaları uzundur ve atma işlemi DNS kayıtlarını zamanından önce kaldırabilir. 
* Dış etki alanı adlarının çözülmesine izin vermek için uygun özyinelemeli çözüm sağlayın.
* Hizmet veren müşterilerden erişilebilir (TCP ve UDP bağlantı noktası 53) ve internete erişebilme.
* Dış ajanların oluşturduğu tehditleri azaltmak için internetten erişime karşı güvence altına alın.

> [!NOTE]
> En iyi performans için, Azure VM'lerini DNS sunucusu olarak kullanıyorsanız, IPv6 devre dışı bırakılmalı.

### <a name="web-apps"></a>Web uygulamaları
Sanal ağa bağlı App Service'i kullanarak aynı sanal ağdaki VM'lere oluşturulmuş web uygulamanızdan ad çözümlemesi yapmanız gerektiğini varsayalım. Sorguları Azure'a (sanal IP 168.63.129.16) ileten bir DNS iletmecisi olan özel bir DNS sunucusu ayarlamaya ek olarak, aşağıdaki adımları gerçekleştirin:
1. [Uygulamanızı sanal ağla tümleştir'de](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)açıklandığı gibi, web uygulamanız için sanal ağ tümleştirmesini etkinleştirin.
2. Azure portalında, web uygulamasına ev sahipliği yapan Uygulama Hizmeti planı için **Ağ**, **Sanal Ağ Entegrasyonu**altında **Eşitleme Ağı'nı** seçin.

    ![Sanal ağ adı çözünürlüğünün ekran görüntüsü](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Sanal ağa bağlı App Service'i kullanarak, farklı bir sanal ağdaki VM'lere sahip olan web uygulamanızdan ad çözümlemesi gerçekleştirmeniz gerekiyorsa, her iki sanal ağda da özel DNS sunucularını aşağıdaki gibi kullanmanız gerekir:

* Hedef sanal ağınızda, sorguları Azure'daki özyinelemeli çözümleyiciye (sanal IP 168.63.129.16) iletebilen bir VM'de bir DNS sunucusu ayarlayın. Örnek bir DNS iletme, [Azure Quickstart Şablonları galerisinde](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) ve [GitHub'da](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder)kullanılabilir. 
* VM'de kaynak sanal ağda bir DNS iletmesi ayarlayın. Bu DNS iletmesini hedef sanal ağınızdaki DNS sunucusuna sorguları iletmek için yapılandırın.
* Kaynak DNS sunucunuzu kaynak sanal ağınızın ayarlarında yapılandırın.
* [Uygulamanızı sanal ağla tümleştir'deki](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)yönergeleri izleyerek, web uygulamanızın kaynak sanal ağa bağlanması için sanal ağ tümleştirmesini etkinleştirin.
* Azure portalında, web uygulamasına ev sahipliği yapan Uygulama Hizmeti planı için **Ağ**, **Sanal Ağ Entegrasyonu**altında **Eşitleme Ağı'nı** seçin.

## <a name="specify-dns-servers"></a>DNS sunucularını belirtin
Kendi DNS sunucularınızı kullanırken Azure, sanal ağ başına birden çok DNS sunucusu belirtme olanağı sağlar. Ayrıca ağ arabirimi başına (Azure Resource Manager için) veya bulut hizmeti başına (klasik dağıtım modeli için) birden çok DNS sunucusu da belirtebilirsiniz. Bir ağ arabirimi veya bulut hizmeti için belirtilen DNS sunucuları, sanal ağ için belirtilen DNS sunucularından önce gelir.

> [!NOTE]
> DNS sunucu IP'leri gibi ağ bağlantısı özellikleri doğrudan VM'ler içinde düzenlenmemelidir. Bunun nedeni, sanal ağ bağdaştırıcısı değiştirildiğinde hizmet iyileşmesırasında silinebilirler. Bu, hem Windows hem de Linux VM'leri için geçerlidir.

Azure Kaynak Yöneticisi dağıtım modelini kullanırken, sanal ağ ve ağ arabirimi için DNS sunucularını belirtebilirsiniz. Ayrıntılar için [sanal ağı yönet](manage-virtual-network.md) ve [ağ arabirimini yönet'](virtual-network-network-interface.md)e bakın.

> [!NOTE]
> Sanal ağınız için özel DNS sunucusu tercih ederseniz, en az bir DNS sunucu IP adresi belirtmeniz gerekir; aksi takdirde, sanal ağ yapılandırmayı yoklar ve bunun yerine Azure tarafından sağlanan DNS'yi kullanır.

Klasik dağıtım modelini kullanırken, Azure portalında veya [Ağ Yapılandırma dosyasında](https://msdn.microsoft.com/library/azure/jj157100)sanal ağ için DNS sunucularını belirtebilirsiniz. Bulut hizmetleri için, DNS sunucularını [Hizmet Yapılandırması dosyası](https://msdn.microsoft.com/library/azure/ee758710) üzerinden veya PowerShell kullanarak Yeni [AzureVM](/powershell/module/servicemanagement/azure/new-azurevm)ile belirtebilirsiniz.

> [!NOTE]
> Yeni DNS ayarlarının etkili olması için, zaten dağıtılan bir sanal ağ veya sanal makinenin DNS ayarlarını değiştirirseniz, sanal ağdaki etkilenen tüm VM'lerde DHCP kira yenilemesi gerçekleştirmeniz gerekir. Windows işletim sistemi çalıştıran VM'ler `ipconfig /renew` için bunu doğrudan VM'ye yazarak yapabilirsiniz. Adımlar işletim sistemi bağlı olarak değişir. İşletim sistemi türünüz için ilgili belgelere bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kaynak Yöneticisi dağıtım modeli:

* [Sanal ağı yönetme](manage-virtual-network.md)
* [Ağ arabirimini yönetme](virtual-network-network-interface.md)

Klasik dağıtım modeli:

* [Azure Hizmet Yapılandırma Şeması](https://msdn.microsoft.com/library/azure/ee758710)
* [Sanal Ağ Yapılandırma Şeması](https://msdn.microsoft.com/library/azure/jj157100)
* [Ağ yapılandırma dosyası kullanarak Sanal Ağ'ı yapılandırma](virtual-networks-using-network-configuration-file.md)
