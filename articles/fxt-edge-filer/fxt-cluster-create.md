---
title: 'Öğretici: Azure FXT Edge Filer önbellek kümesini oluşturun'
description: Azure FXT Edge Filer ile karma depolama önbelleği kümesi nasıl oluşturulur?
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.openlocfilehash: bfe1d1aeeac55039acf0c7eb295001277be9cd2e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239213"
---
# <a name="tutorial-create-the-azure-fxt-edge-filer-cluster"></a>Öğretici: Azure FXT Edge Filer kümesini oluşturun

Önbelleğiniz için Azure FXT Edge Filer donanım düğümlerini yükledikten ve kullanıma açtıktan sonra önbellek kümesini oluşturmak için FXT küme yazılımını kullanın. 

Bu öğretici, donanım düğümlerinizi bir küme olarak yapılandırmak için adımlar boyunca size yol eder. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Küme oluşturmaya başlamadan önce hangi bilgiler gereklidir?
> * Kümenin yönetim ağı, küme ağı ve istemciye bakan ağ arasındaki fark
> * Küme düğümüne bağlanma 
> * Bir Azure FXT Edge Filer düğümü kullanarak ilk küme oluşturma
> * Küme ayarlarını yapılandırmak için küme denetim panelinde oturum açma

Bu yordam, IP adreslerini ve ağ kaynaklarını tanımlamak için ne kadar araştırma yapmanız gerektiğine bağlı olarak 15 ila 45 dakika sürer.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce bu ön koşulları tamamlayın:

* Azure FXT Edge Filer donanım sistemlerinizi veri merkezinize yükleyin 

  Kümeoluşturmak için yalnızca bir düğüm gerekir, ancak küme yapılandırmak ve kullanıma hazır hale getirmek için önce [en az iki düğüm daha eklemeniz](fxt-add-nodes.md) gerekir. 

* Uygun güç ve şebeke kablolarını sisteme bağlama  
* En az bir Azure FXT Edge Filer düğümünde güç ve [kök parolasını ayarlayın](fxt-node-password.md)

## <a name="gather-information-for-the-cluster"></a>Küme için bilgi toplama 

Azure FXT Edge Filer kümesini oluşturmak için aşağıdaki bilgilere ihtiyacınız vardır:

* Küme adı

* Küme için ayarlanması gereken yönetim parolası

* IP adresleri:

  * Küme yönetimi için tek bir IP adresi ve yönetim ağı için kullanılacak netmaske ve yönlendirici
  * Küme (düğümden düğüme) iletişim için bitişik bir IP adresi aralığındaki ilk ve son IP adresleri. Ayrıntılar için [ip adresi dağıtımına](#ip-address-distribution)aşağıdan bakınız. 
  * (İstemcilere bakan IP adresleri küme oluşturulduktan sonra ayarlanır.)

* Ağ altyapısı bilgileri:

  * Küme için bir DNS sunucusunun IP adresi
  * Küme için DNS etki alanının adı
  * Küme NTP sunucuları için ad veya IP adresi (bir sunucu veya üç veya daha fazla) 
  * Kümenin arabirimlerinde IEEE 802.1AX-2008 bağlantı toplamayı etkinleştirmek isteyip istemediğiniz
  * IEEE 802.3ad (LACP) dinamik toplama kullanıp kullanmayacağınız, bağlantı toplamayı etkinleştiriyseniz

Kümeyi oluşturduktan sonra bu ağ altyapısı öğelerini yapılandırabilirsiniz, ancak oluşturma zamanında yapmak daha iyidir. 

### <a name="ip-address-distribution"></a>IP adresi dağıtımı

Azure FXT Edge Filer karma depolama önbelleği kümesi IP adreslerini üç kategoride kullanır:

* Yönetim IP: Küme yönetimi için tek bir IP adresi

  Bu adres küme yapılandırma yardımcı programlarına (web tabanlı denetim paneli veya XML-RPC API) erişmek için giriş noktası olarak hizmet vermektedir. Bu adres kümedeki birincil düğüme otomatik olarak atanır ve birincil düğüm değişirse otomatik olarak hareket eder.

  Diğer IP adresleri denetim paneline erişmek için kullanılabilir, ancak yönetim IP adresi tek tek düğümler üzerinde başarısız olsa bile erişim sağlamak için tasarlanmıştır.

* Küme Ağı: Küme iletişimi için bir ip adresi aralığı

  Küme ağı küme düğümleri arasında iletişim ve arka uç depolama (çekirdek filers) dosyaları almak için kullanılır.

  **En iyi uygulama:** Her Azure FXT Edge Filer düğümünde küme iletişimi için kullanılan fiziksel bağlantı noktası başına bir IP adresi ayırın. Küme, belirtilen aralıktaki adresleri otomatik olarak tek tek düğümlere atar.

* İstemcilere bakan ağ: İsteyerek ve dosya yazmak için kullandıkları IP adresleri aralığı

  İstemci ağ adresleri küme üzerinden çekirdek filer verilerine erişmek için istemciler tarafından kullanılır. Örneğin, bir NFS istemcisi bu adreslerden birini monte edebilir.

  **En iyi uygulama:** Her FXT Serisi düğümünde istemci iletişimi için kullanılan fiziksel bağlantı noktası başına bir IP adresi ayırın.

  Küme, istemciye bakan IP adreslerini kurucu düğümlerine mümkün olduğunca eşit olarak dağıtır.

  Basitlik için, birçok yönetici, istemci isteklerini adres aralığında dağıtmayı kolaylaştırmak için tek bir DNS adını round-robin DNS (RRDNS) yapılandırmasıyla yapılandırır. Bu kurulum, kümeye erişmek için tüm istemcilerin aynı montaj komutunu kullanmasına da olanak tanır. Daha fazla bilgi için [DNS'yi Yapılandır'ı](fxt-configure-network.md#configure-dns-for-load-balancing) okuyun.

Yeni bir küme oluşturmak için yönetim IP adresi ve küme ağ adresleri aralığı belirtilmelidir. İstemcilere bakan adresler küme oluşturulduktan sonra belirtilir.

## <a name="connect-to-the-first-node"></a>İlk düğüme bağlanma

Yüklü FXT düğümlerinden herhangi birisine bağlanabilir ve kümeyi ayarlamak için işletim sistemi yazılımını kullanabilirsiniz.

Bunu zaten yapmadıysanız, kümeniz için FXT düğümlerinden en az birini güçlendirin ve bir ağ bağlantısı ve IP adresi olduğundan emin olun. Düğümü etkinleştirmek için yeni bir kök parola ayarlamanız gerekir, bu nedenle daha önce yapmadıysanız [donanım parolalarını](fxt-node-password.md) ayarla'daki adımları izleyin.

Ağ bağlantısını denetlemek için düğümün ağ bağlantısı LED'lerinin yandığından (ve gerekirse bağlı olduğu ağ anahtarındaki göstergelerden) emin olun. Gösterge LED'leri [Monitör Azure FXT Edge Filer donanım durumunda](fxt-monitor.md)açıklanmıştır.

Düğüm önden döndüğünde, bir IP adresi isteğinde bulunacaktır. Bir DHCP sunucusuna bağlıysa, DHCP tarafından sağlanan IP adresini kabul eder. (Bu IP adresi geçicidir. Küme oluşturduğunuzda değişecektir.)

Bir DHCP sunucusuna bağlı değilse veya yanıt almazsa, düğüm 169.254 formunda kendi kendine atanmış bir IP adresi ayarlamak için Bonjour yazılımını kullanır. \*. \*. Ancak, küme oluşturmak için kullanmadan önce düğümün ağ kartlarından birine geçici statik bir IP adresi ayarlamanız gerekir. Talimatlar bu eski belgede yer alıyor; Güncelleştirilmiş bilgiler için Microsoft Service ve Support adresine başvurun: [Ek A: FXT Düğümünde Statik IP Adresi Ayarlama.](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/static_ip.html)

### <a name="find-the-ip-address"></a>IP adresini bulma

IP adresini bulmak için Azure FXT Edge Filer düğümüne bağlanın. Seri kablo, USB ve VGA bağlantı noktalarına doğrudan bağlantı kullanabilir veya KVM anahtarı yla bağlanabilirsiniz. (Bağlantı noktası bağlantı ayrıntıları [için](fxt-node-password.md)bkz.

Bağlandıktan sonra, düğümü ilk `root` kez önyüklemeyaptığınızda kullanıcı adı ve ayarladığınız parolayla oturum açın.  

Oturum açmadan sonra düğümün IP adresini belirlemeniz gerekir.

Bu sisteme `ifconfig` atanan adresleri görmek için komutu kullanın.

Örneğin, komut `ifconfig | grep -B5 inet` internet adresleri olan bağlantı noktalarını arar ve bağlantı noktası tanımlayıcısını göstermek için beş bağlam satırı verir.

Ifconfig raporunda gösterilen ip adresini yazın. E0a veya e0b gibi bağlantı noktası adlarıyla listelenen adresler iyi seçeneklerdir. Bu adlar yalnızca iDRAC/IPMI servis bağlantı noktaları için kullanıldığından, e7* adlarıyla listelenen IP adreslerini kullanmayın.  

## <a name="load-the-cluster-configuration-wizard"></a>Küme yapılandırma sihirbazını yükleyin

Kümeoluşturmak için tarayıcı tabanlı küme yapılandırma aracını kullanın. 

Düğümün IP adresini bir web tarayıcısında girin. Tarayıcı siteye güvenilmeden ilgili bir mesaj veriyorsa, yine de siteye ilerleyin. (Tek tek Azure FXT Edge Filer düğümlerinde CA sağlanan güvenlik sertifikaları yoktur.)

![Tarayıcı penceresinde panel oturum açma sayfasını denetleme](media/fxt-cluster-create/unconfigured-node-gui.png)

Kullanıcı **Adı** ve **Parola** alanlarını boş bırakın. Küme oluşturma sayfasını yüklemek için **Giriş'i** tıklatın.

![Tarayıcı tabanlı GUI denetim panelinde yapılandırılmamış bir düğüm için ilk kurulum ekranı. Yazılımı güncelleştirme, kümeyi el ile yapılandırma veya bir kurulum dosyasından küme yapılandırma seçenekleri gösterir.](media/fxt-cluster-create/setup-first-screen.png)

## <a name="create-the-cluster"></a>Kümeyi oluşturma

Küme yapılandırma aracı, Azure FXT Edge Filer kümesini oluşturmak için bir ekran kümesinde size yol gösteriyor. Başlamadan önce [gerekli bilgilerin](#gather-information-for-the-cluster) hazır olduğundan emin olun. 

### <a name="creation-options"></a>Oluşturma seçenekleri

İlk ekran üç seçenek sunar. Destek personelinden özel talimatlar istemiyorsanız manuel yapılandırma seçeneğini kullanın.

Yeni küme yapılandırma seçenekleri ekranını yüklemek için **kümeyi el ile yapılandıracağım** atıklayın. 

Diğer seçenekler nadiren kullanılır:

* "Sistem görüntüsünü güncelleştir" kümeoluşturmadan önce yeni işletim sistemi yazılımı yüklemenizi ister. (Şu anda yüklü olan yazılım sürümü ekranın üst kısmında listelenmiştir.) Yazılım paketi dosyasını -url ve kullanıcı adı/parola) veya bilgisayarınızdan bir dosya yükleyerek sağlamanız gerekir. 

* Küme kurulum dosyası seçeneği bazen Microsoft Müşteri Hizmetleri ve Desteği tarafından kullanılır. 

## <a name="cluster-options"></a>Kümeleme seçenekleri

Bir sonraki ekran, yeni küme için seçenekleri yapılandırmanızı ister.

Sayfa iki ana bölüme ayrılmıştır: **Temel Yapılandırma** ve **Ağ Yapılandırması.** Ağ yapılandırma bölümünde ayrıca alt bölümler de vardır: biri **Yönetim** ağı için, diğeri **de Cluster** ağı için.

### <a name="basic-configuration"></a>Temel yapılandırma

Üst bölümde, yeni küme için temel bilgileri doldurun.

![Tarayıcı GUI sayfasında "Temel yapılandırma" bölümünün ayrıntıları. Üç alan gösterir (küme adı, yönetici parolası, parolayı onaylama)](media/fxt-cluster-create/basic-configuration.png) 

* **Küme Adı** - Küme için benzersiz bir ad girin.

  Küme adı şu ölçütleri karşılamalıdır:
  
  * 1 ila 16 karakter uzunluğu
  * Harfleri, sayıları ve tireyi (-) ve alt çizgi (_) karakterlerini içerebilir 
  * Diğer noktalama işaretlerini veya özel karakterleri içermemelidir
  
  Bu adı daha sonra **Cluster** > **General Setup** yapılandırma sayfasında değiştirebilirsiniz. (Küme ayarları hakkında daha fazla bilgi için, bu belge kümesinin bir parçası olmayan [Küme Yapılandırma Kılavuzu'nu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html)okuyun.)

  > [!NOTE] 
  > Küme adınız, izleme veya sorun giderme için desteklenen sistem bilgilerini tanımlamak için kullanılır, bu nedenle şirket adınızı eklemeniz yararlı olur.

* **Yönetici parolası** - Varsayılan yönetim kullanıcısının parolasını ayarlayın, `admin`.
  
  Kümeyi yöneten her kişi için ayrı ayrı kullanıcı hesapları ayarlamanız `admin`gerekir, ancak kullanıcıyı kaldıramazsınız. Oturum aç, ek kullanıcılar oluşturmanız gerekiyormuş gibi `admin` oturum açın.
 
  `admin` Küme Denetim Masası'ndaki **Yönetim** > **Kullanıcıları** ayarları sayfasında parolayı değiştirebilirsiniz. Ayrıntılar [için, Küme Yapılandırma Kılavuzu'ndaki](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_users.html) **Kullanıcılar** belgelerini okuyun.

<!-- to do: update "legacy" URLs when docs are ported to Microsoft site -->

### <a name="network-configuration"></a>Ağ yapılandırması

**Ağ** bölümü, kümenin kullanacağı ağ altyapısını belirtmenizi ister. 

Yapılandırmak için iki ayrı ağ vardır:

* *Yönetim ağı* yapılandırma ve izleme için kümeye yönetici erişimi sağlar. Burada belirtilen IP adresi, Kontrol Paneli'ne bağlanırken veya SSH erişimi için kullanılır. 

  Çoğu küme yalnızca tek bir yönetim IP adresi kullanır, ancak arabirim eklemek istiyorsanız, kümeyi oluşturduktan sonra bunu yapabilirsiniz.

* *Küme ağı* küme düğümleri arasında ve küme düğümleri ve arka uç depolama (çekirdek filers) arasında iletişim için kullanılır.

İstemcilere bakan ağ, küme oluşturulduktan sonra daha sonra yapılandırılır.

Bu bölümde, her iki ağ tarafından da kullanılan DNS ve NTP sunucuları için yapılandırma da yer almaktadır.

### <a name="configure-the-management-network"></a>Yönetim ağını yapılandırma

**Yönetim** bölümündeki ayarlar, yöneticinin kümeye erişmesini sağlayan ağ içindir.

![5 seçenekli alanlar ve 1Gb yönetim ağı için bir onay kutusu ile "Yönetim" bölümünün ayrıntıları](media/fxt-cluster-create/management-network-filled.png)

* **Yönetim IP** - Küme Denetim Masası'na erişmek için kullanacağınız IP adresini belirtin. Bu adres kümenin birincil düğümü tarafından talep edilir, ancak özgün birincil düğüm kullanılamazsa otomatik olarak sağlıklı bir düğüme taşınır.

  Çoğu küme yalnızca bir yönetim IP adresi kullanır. Birden fazla istiyorsanız, **kümeyi** > oluşturduktan sonra Küme**Yönetim Ağı** ayarları sayfasını kullanarak bunları ekleyebilirsiniz. Küme Yapılandırma [Kılavuzu'nda](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html)daha fazla bilgi edinin.

* **Netmask** - Yönetim ağı için netmask belirtin.

* **Yönlendirici** - Yönetim ağı tarafından kullanılan varsayılan ağ geçidi adresini girin.

* **VLAN etiketi (isteğe bağlı)** - Kümeniz VLAN etiketleri kullanıyorsa, yönetim ağı için etiketi belirtin.

  Ek VLAN ayarları **Küme** > **VLAN** ayarları sayfasında yapılandırılır. Daha fazla bilgi edinmek için Küme Yapılandırma Kılavuzu'nda [VLAN'larla çalışma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html#vlan-overview) ve [Küme > VLAN'ı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vlan.html) okuyun.

* **MTU** - Gerekirse, kümenizin yönetim ağı için maksimum iletim birimini (MTU) ayarlayın.

* **1Gb mgmt ağını kullanın** - FXT düğümlerinizdeki iki 1GbE ağ bağlantı noktasını yalnızca yönetim ağına atamak istiyorsanız bu kutuyu işaretleyin. (Diğer tüm trafik için 25GbE/10GbE bağlantı noktaları bulunmalıdır.) Bu kutuyu denetlemezseniz, yönetim ağı kullanılabilir en yüksek hız bağlantı noktasını kullanır. 

### <a name="configure-the-cluster-network"></a>Küme ağını yapılandırma 

Küme ağ ayarları küme düğümleri arasındaki trafiğe ve küme düğümleri ile çekirdek filörleri arasındaki trafiğe uygulanır.

![altı değer girmek için alanları ile "Küme" bölümünün detay](media/fxt-cluster-create/cluster-network-filled.png)

* **İlk IP** ve **Son IP** - Dahili küme iletişimi için kullanılacak aralığı tanımlayan IP adreslerini girin. Burada kullanılan IP adresleri bitişik olmalı ve DHCP tarafından atanmamış olmalıdır.

  Kümeyi oluşturduktan sonra daha fazla IP adresi ekleyebilirsiniz. Küme **Küme** > **Ağları** ayarları sayfasını[(Küme Yapılandırma Kılavuzu belgeleri)](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cluster_networks.html#gui-cluster-networks)kullanın.

  **Aralıktaki IP Sayısı'ndaki** değer hesaplanır ve otomatik olarak gösterilir.

* **Mgmt olmayan netmask (isteğe bağlı)** - Küme ağı için netmask belirtin. 

  Sistem, yönetim ağı için girdiğiniz netmask değerini otomatik olarak önerir; gerekirse değiştirin.

* **Küme yönlendiricisi (isteğe bağlı)** - Küme ağı tarafından kullanılan varsayılan ağ geçidi adresini belirtin. 

  Sistem, yönetim ağı için sağladığınız aynı ağ geçidi adresini otomatik olarak önerir.

* **VLAN kümesi etiketi (isteğe bağlı)** - Kümeniz VLAN etiketleri kullanıyorsa, küme ağı için etiketi belirtin.

* **MGMT Dışı MTU (isteğe bağlı)** - Gerekirse küme ağınız için maksimum iletim ünitesini (MTU) ayarlayın.

### <a name="configure-cluster-dns-and-ntp"></a>DNS ve NTP kümesini yapılandırma 

**Kümele** bölümünün altında DNS ve NTP sunucularını belirtmek ve bağlantı toplamayı etkinleştirmek için alanlar vardır. Bu ayarlar kümenin kullandığı tüm ağlar için geçerlidir.

![DNS sunucuları için üç alan, DNS etki alanı ve DNS araması için alanlar, NTP sunucuları için üç alan ve bağlantı toplama seçenekleri için açılır menü içeren DNS/NTP yapılandırması için bölümün ayrıntıları](media/fxt-cluster-create/dns-ntp-filled.png)

* **DNS server(lar)** - Bir veya daha fazla etki alanı adı sistemi (DNS) sunucusunun IP adresini girin.

  SMB, AD veya Kerberos kullanmak istiyorsanız, tüm kümeler için DNS önerilir. 
  
  En iyi performans için, [Azure FXT Edge Filer kümesi için DNS'yi Yapılandırın'da](fxt-configure-network.md#configure-dns-for-load-balancing)açıklandığı gibi kümenin DNS sunucusunu round-robin yük dengelemesi için yapılandırın.

* **DNS etki alanı** - Kümenin kullanacağı ağ etki alanı adını girin.

* **DNS arama** - İsteğe bağlı olarak, sistemin DNS sorgularını çözmek için arama yapması gereken ek alan adları girin. Boşluklara ayrılmış en fazla altı alan adı ekleyebilirsiniz.

* **NTP server(lar)** - Sağlanan alanlarda bir veya üç ağ zaman protokolü (NTP) sunucularını belirtin. Ana bilgisayar adlarını veya IP adreslerini kullanabilirsiniz.

* **Bağlantı toplama** - Bağlantı toplama, küme FXT düğümleri üzerindeki ethernet bağlantı noktalarının çeşitli trafik türlerini nasıl işleyeceğini özelleştirmenize olanak tanır. Daha fazla bilgi edinmek için Küme Yapılandırma Kılavuzu'nda [Bağlantı Toplama'yı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cluster_general_setup.html#link-aggregation) okuyun.

### <a name="click-the-create-button"></a>Oluştur düğmesini tıklatın

Formda gerekli tüm ayarları yaptıktan sonra **Cluster Oluştur** düğmesini tıklatın.

![sağ alttaki oluştur düğmesi üzerinde imleç ile tamamlanmış formun alt](media/fxt-cluster-create/create-cluster.png)

Sistem küme oluştururken bir ileti görüntüler.

![tarayıcıda küme yapılandırma durum iletisi: "FXT düğümü şimdi küme oluşturuyor. Bu işlem birkaç dakika sürer. Küme oluşturulduğunda, yapılandırmayı tamamlamak için bu bağlantıyı ziyaret edin." "bu bağlantıyı ziyaret et" üzerine köprü ile](media/fxt-cluster-create/creating-message.png)

Birkaç dakika sonra, küme Denetim Masası'na gitmek için iletideki bağlantıyı tıklatabilirsiniz. (Bu bağlantı sizi **Yönetim IP'sinde**belirttiğiniz IP adresine götürür.) Oluştur düğmesini tıklattıktan sonra bağlantının etkin hale gelmesi 15 saniye ile bir dakika arasında sürer. Web arabirimi yüklenmiyorsa, birkaç saniye daha bekleyin ve ardından bağlantıyı yeniden tıklatın. 

Küme oluşturma işlemi bir dakika veya daha uzun sürer, ancak işlem devam ederken Denetim Masası'nda oturum açabilirsiniz. Küme oluşturma işlemi bitene kadar denetim panelinin pano sayfasının uyarıları göstermesi normaldir. 

## <a name="open-the-settings-pages"></a>Ayarlar sayfalarını açma 

Kümeyi oluşturduktan sonra, ağınızdaki yapılandırmayı ve iş akışınız için yapılandırmasını özelleştirmeniz gerekir. 

Yeni kümenizi ayarlamak için Denetim Masası web arabirimini kullanın. Küme oluşturma durum ekranınızdaki bağlantıyı izleyin veya kümede ayarladığınız yönetim IP adresine göz atın.

Küme oluşturulurken belirlediğiniz kullanıcı `admin` adı ve parolayla web arabiriminde oturum açın.

![kontrol paneli giriş alanlarını gösteren web tarayıcısı](media/fxt-cluster-create/admin-login.png)

Denetim Masası **Pano** sayfasını açar ve gösterir. Küme oluşturma sona ererken, herhangi bir uyarı iletisi ekrandan netolmalıdır.

Kümeyi yapılandırmak için **Ayarlar** sekmesini tıklatın.

**Ayarlar** sekmesinde, sol kenar çubuğu yapılandırma sayfalarından oluşan bir menü gösterir. Sayfalar kategoriye göre düzenlenir. Sayfaları tek tek genişletmek veya gizlemek için kategori adının üst kısmındaki + veya - denetimini tıklatın.

![Cluster > Genel Kurulum sayfası yüklü denetim panelinin (tarayıcıda) ayarlar sekmesi](media/fxt-cluster-create/settings-tab-populated.png)

## <a name="cluster-setup-steps"></a>Küme kurulum adımları

İşlemin bu noktasında, kümeniz var, ancak yalnızca bir düğüm, istemciye bakan IP adresleri ve arka uç depolama yok. Yeni oluşturulan kümeden iş akışınızı işlemeye hazır bir önbellek sistemine geçmek için ek kurulum adımları gereklidir.

### <a name="required-configuration"></a>Gerekli yapılandırma

Bu adımlar, kümelerin çoğu veya tümü için gereklidir. 

* Kümeye düğüm ekleme 

  Üç düğüm standarttır, ancak birçok üretim kümelerinde en fazla 24 düğüm vardır.

  Kümenize diğer Azure FXT Edge Filer birimlerini nasıl ekleyeceğinizi öğrenmek ve Yüksek Kullanılabilirlik'i etkinleştirmek için [küme düğümleri](fxt-add-nodes.md) ekle'yi okuyun.

* Arka uç depolamayı belirtin

  Kümenin kullanacağı her arka uç depolama sistemi için *çekirdek filer* tanımları ekleyin. Oku Arka uç depolama ekle ve daha fazla bilgi edinmek için [sanal ad alanını yapılandırın.](fxt-add-storage.md#about-back-end-storage)

* İstemci erişimini ve sanal ad alanını ayarlama 

  En az bir sanal sunucu (vserver) oluşturun ve istemci makinelerinin kullanması için bir IP adresi aralığı atayın. Ayrıca, arka uç depolama dışaaklarını sanal yollara eşlemenize olanak tanıyan sanal bir dosya sistemi özelliği olan küme ad alanını (bazen Global Ad Alanı veya GNS olarak da adlandırılır) yapılandırmanız gerekir. Küme ad alanı, arka uç depolama ortamını değiştirseniz bile istemcilere tutarlı ve erişilebilir bir dosya sistemi yapısı sağlar. Ad alanı ayrıca Azure Blob kapsayıcıları veya desteklenen diğer bulut nesnesi depolaması için kullanıcı dostu bir sanal depolama hiyerarşisi de sağlayabilir.

  Ayrıntılar için [ad alanını yapılandır'ı](fxt-add-storage.md#configure-the-namespace) okuyun. Bu adım şunları içerir:
  * Vservers oluşturma
  * İstemci ağ görünümü ile arka uç depolama arasında bağlantı kurma 
  * Her vserver tarafından hangi istemci IP adreslerinin sunulduğunun tanımlanması

  > [!Note] 
  > Kümenin GNS'sini ayarlamaya başlamadan önce önemli planlama önerilir. Genel [Ad Alanı Kullanma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html) ve Küme Yapılandırma Kılavuzu'nda [VServers](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#creating-and-working-with-vservers) bölümleri oluşturma ve çalışma bölümünü okuyun.

* [Ağ ayarlarını ayarlama](fxt-configure-network.md)

  Yeni bir küme için doğrulanmalı veya özelleştirilmelidir ağla ilgili birkaç ayar vardır. Bu öğelerle ilgili ayrıntılar için [ağ ayarlarını ayarlama'yı](fxt-configure-network.md) okuyun:

  * DNS ve NTP yapılandırmalarını doğrulama 
  * Gerekirse dizin hizmetlerini yapılandırma 
  * V'n'ler ayarlama
  * Proxy sunucularını yapılandırma
  * Küme ağına IP adresleri ekleme
  * Şifreleme sertifikalarını depolama

* [Destek izleme yi ayarlama](#enable-support)

  Yapılandırma aracının gizlilik ilkesini kabul etmeli ve destek yükleme ayarlarınızı aynı anda yapılandırmanız gerekir.

  Küme, istatistikler ve hata ayıklama dosyaları da dahil olmak üzere kümeniz hakkındaki sorun giderme verilerini otomatik olarak yükleyebilir. Bu yüklemeler, Microsoft Müşteri Hizmetleri ve Destek'in mümkün olan en iyi hizmeti sunmasına olanak sağlar. İzlenenleri özelleştirebilir ve isteğe bağlı olarak proaktif destek ve uzaktan sorun giderme hizmetini etkinleştirebilirsiniz.  

### <a name="optional-configuration"></a>İsteğe bağlı yapılandırma

Bu adımlar tüm kümeler için gerekli değildir. Bazı iş akışları türleri veya belirli küme yönetim stilleri için gereklidirler. 

* Düğüm ayarlarını özelleştirme

  Düğüm adlarını ayarlayabilir ve düğüm IPMI bağlantı noktalarını küme genelinde veya tek tek yapılandırabilirsiniz. Kümeye düğüm eklemeden önce bu ayarları yapılandırıyorsanız, yeni düğümler birleştirdiklerinde ayarları otomatik olarak alabilir. Seçenekler, eski küme oluşturma belgesinde Düğüm [Ayarlarını Özelleştirme](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_node.html)de açıklanmıştır.

  > [!TIP]
  > Bu ürüne ait bazı belgeler henüz Microsoft Azure dokümantasyon sitesinde bulunmamaktadır. [Küme Yapılandırma Kılavuzu'na](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html) bağlantılar ve [Küme Oluşturma Kılavuzu'nun](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/create_index.html) eski sürümü sizi GitHub tarafından barındırılan ayrı bir web sitesine götürür. 

* SMB'yi yapılandır

  SMB'nin kümenize ve NFS'nize erişmesine izin vermek istiyorsanız, SMB'yi yapılandırmanız ve açmanız gerekir. SMB (bazen CIFS olarak da adlandırılır), genellikle Microsoft Windows istemcilerini desteklemek için kullanılır.

  SMB için planlama ve yapılandırma, Denetim Masası'ndaki birkaç düğmeyi tıklatmaktan daha fazlasını içerir. Sisteminizin gereksinimlerine bağlı olarak, SMB çekirdek dosyalayıcıları nasıl tanımladığınızı, kaç vservers oluşturduğunuzu, bağlantılarınızı ve ad alanınızı nasıl yapılandırdığınızı, erişim izinlerini ve diğer ayarları etkileyebilir.

  Daha fazla bilgi [için, Kobİ Erişimini Yapılandırma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/smb_overview.html) KümeLeme Kılavuzu bölümünü okuyun.

* Ek lisanslar yükleme

  Azure Blob dışında bulut depolama alanı kullanmak istiyorsanız, ek bir özellik lisansı yüklemeniz gerekir. FlashCloud<sup>TM</sup> lisansı satın alma yla ilgili ayrıntılar için Microsoft temsilcinize başvurun. Ayrıntılar arka [uç depolama ekle ve sanal ad alanı yapılandırmaaçıklanır.](fxt-add-storage.md#about-back-end-storage)


### <a name="enable-support"></a>Desteği etkinleştirme

Azure FXT Edge Filer kümesi, kümenizle ilgili destek verilerini otomatik olarak yükleyebilir. Bu yüklemeler, personelin mümkün olan en iyi müşteri hizmetini sağlamasına olanak sağlar.

Destek yüklemeleri ayarlamak için aşağıdaki adımları izleyin.

1. **Küme** > **Desteği** ayarları sayfasına gidin. Gizlilik politikasını kabul edin. 

   ![Gizlilik politikasını kabul etmek için Denetim Masası'nı ve açılır pencereyi Onayla düğmesiyle gösteren ekran görüntüsü](media/fxt-cluster-create/fxt-privacy-policy.png)

1. Bölümü genişletmek için **Müşteri Bilgileri'nin** solundaki üçgeni tıklatın.
1. Yükleme **bilgilerini yeniden ertele** düğmesini tıklatın.
1. Kümenin destek adını **Benzersiz Küme Adı'nda** ayarlayın - personeli desteklemek için kümenizi benzersiz olarak tanımladığından emin olun.
1. **İstatistik İzleme,** **Genel Bilgi Yükleme**ve **Crash Information Upload**için kutuları işaretleyin.
1. **Gönder'i**tıklatın.  

   ![Destek ayarları sayfasının tamamlanmış müşteri bilgileri bölümünü içeren ekran görüntüsü](media/fxt-cluster-create/fxt-support-info.png)

1. Bölümü genişletmek için **Güvenli Proaktif Destek'in (SPS)** solundaki üçgeni tıklatın.
1. **SPS Bağlantısını Etkinleştir**için kutuyu işaretleyin.
1. **Gönder'i**tıklatın.

   ![Destek ayarları sayfasında tamamlanmış Güvenli Proaktif Destek bölümünü içeren ekran görüntüsü](media/fxt-cluster-create/fxt-support-sps.png)

## <a name="next-steps"></a>Sonraki adımlar

Temel kümeyi oluşturduktan ve gizlilik ilkesini kabul ettikten sonra, küme düğümlerinin geri kalanını ekleyin. 

> [!div class="nextstepaction"]
> [Küme düğümleri ekleme](fxt-add-nodes.md)
