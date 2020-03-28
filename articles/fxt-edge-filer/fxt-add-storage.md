---
title: 'Öğretici: Azure FXT Edge Filer kümesine depolama alanı ekleme'
description: Azure FXT Edge Filer için arka uç depolama ve istemciye bakan sözde ad alanı nasıl yapılandırılabilen
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.openlocfilehash: 3f736942627d088e3a639f89bef5438714c2608b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239220"
---
# <a name="tutorial-add-back-end-storage-and-configure-the-virtual-namespace"></a>Öğretici: Arka uç depolama ekle ve sanal ad alanını yapılandırın 

Bu öğretici, önbelleğiniz için arka kenar depolamasını nasıl ekleyeceğiniz ve istemciye bakan sanal dosya sistemini nasıl ayarleyeceğiniz açıklanmaktadır. 

Küme, istemcilerin istediği verilere erişmek ve değişiklikleri önbellektekinden daha kalıcı olarak depolamak için arka uç depolama sistemlerine bağlanır. 

Ad alanı, istemci tarafındaki iş akışlarını değiştirmeden arka uç depolamaalanını değiştirmenize olanak tanıyan istemciye dönük sözde dosya sistemidir. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Azure FXT Edge Filer kümesine arka uç depolama alanı ekleme 
> * Depolama için istemciye bakan yol nasıl tanımlanır?

## <a name="about-back-end-storage"></a>Arka uç depolama hakkında

Azure FXT Edge Filer kümesi, arka uç depolama sistemini FXT kümesine bağlamak için *çekirdek filer* tanımı nı kullanır.

Azure FXT Edge Filer birkaç popüler NAS donanım sistemiyle uyumludur ve Azure Blob veya diğer bulut depolama adresinden boş kapsayıcılar kullanabilir. 

FXT işletim sisteminin bulut depolama hacmindeki tüm verileri tamamen yönetebilmeleri için bulut depolama kapsayıcıları eklendiğinde boş olmalıdır. Kapsayıcıyı kümeye çekirdek filer olarak ekledikten sonra varolan verilerinizi bulut kapsayıcısına taşıyabilirsiniz.

Sisteminize bir çekirdek filer eklemek için Denetim Masası'nı kullanın.

> [!NOTE]
> 
> Amazon AWS veya Google Cloud depolama alanını kullanmak istiyorsanız, bir FlashCloud<sup>TM</sup> özellik lisansı yüklemeniz gerekir. Lisans anahtarı için Microsoft temsilcinize başvurun ve ardından özellik [lisansları ekleme veya kaldırma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/install_licenses.html#install-licenses)için eski yapılandırma kılavuzundaki yönergeleri izleyin.
> 
> Azure Blob depolama desteği, Azure FXT Edge Filer yazılım lisansına dahildir. 

Çekirdek dosyalayıcıları ekleme hakkında daha ayrıntılı bilgi için Küme Yapılandırma Kılavuzu'nun şu bölümlerini okuyun:

* Seçme ve bir çekirdek filer eklemek için hazırlanıyor hakkında daha fazla bilgi için, [Core Filers ile Çalışma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/core_filer_overview.html#core-filer-overview)okuyun.
* Ayrıntılı önkoşullar ve adım adım talimatlar için aşağıdaki makaleleri okuyun:

  * [Yeni NAS Çekirdek Filer Ekleme](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html#create-core-filer-nas)
  * [Yeni Bulut Çekirdek Filer Ekleme](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html#create-core-filer-cloud)

Çekirdek filer ekledikten sonra, Core Filer Ayrıntıları ayarları sayfasında ayarlarını güncelleştirebilirsiniz.

## <a name="add-a-core-filer"></a>Çekirdek filer ekle

**Core Filer** > **Yönet Çekirdek Filers** ayarları sayfasındaki **Oluştur** düğmesini tıklatarak bir çekirdek filer tanımlayın.

![Çekirdek Filers'ı Yönet sayfasındaki çekirdek dosyalayıcılar listesinin üzerindeki oluştur düğmesini tıklatma](media/fxt-cluster-config/create-core-filer-button.png)

**Yeni Çekirdek Filer Ekle** sihirbazı, arka uç depolamanıza bağlantı veren bir çekirdek filer oluşturma sürecinde size yol sunar. Küme Yapılandırma Kılavuzu, NFS/NAS depolama ve bulut depolama için farklı olan işlemin adım adım açıklamalarına sahiptir (bağlantılar yukarıdadır). 

Alt görevler şunlardır:

* Çekirdek filer (NAS veya bulut) türünü belirtin

  ![Donanım NAS yeni çekirdek filer sihirbazı ilk sayfası. "Bulut çekirdeği filer" seçeneği devre dışı bırakılır ve eksik lisansla ilgili bir hata iletisi gösterir.](media/fxt-cluster-config/new-nas-1.png)

* Çekirdek filer adını ayarlayın. Küme yöneticilerinin hangi depolama sistemini temsil ettiğini anlamasına yardımcı olacak bir ad seçin.

* NAS çekirdek dosyalayıcıları için tam nitelikli alan adı (FQDN) veya IP adresi sağlayın. FQDN tüm çekirdek filers için tavsiye edilir ve SMB erişimi için gereklidir.

* Önbellek ilkesi seçin - Sihirbazın ikinci sayfası, yeni çekirdek filer için kullanılabilir önbellek ilkelerini listeler. Ayrıntılar için [Küme Yapılandırma Kılavuzu'nun önbellek ilkeleri bölümünü](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_manage_cache_policies.html)okuyun. 

  ![Bir donanım NAS yeni çekirdek filer sihirbazı ikinci sayfa; Önbellek İlkesi açılır menüsü açıktır ve birkaç devre dışı bırakArak birkaç devre dışı bırakma seçeneği ve üç geçerli önbellek ilkesi seçeneği (atlama, önbelleğe alma ve önbelleğe alma/yazma) gösterilir.](media/fxt-cluster-config/new-nas-choose-cache-policy.png)

* Bulut depolama için, diğer parametrelerin yanı sıra bulut hizmetini ve erişim kimlik bilgilerini belirtmeniz gerekir. Ayrıntılar için Cluster Configuration Guide'da [Bulut hizmeti ve protokolü](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html#cloud-service-and-protocol) okuyun.

  ![Yeni Çekirdek Filer sihirbazında bulut çekirdek filer bilgileri](media/fxt-cluster-config/new-core-filer-cloud3.png) 
  
  Bu küme için zaten bulut erişim kimlik bilgileri eklediyseniz, bunlar listede görünür. **Küme** > **Bulutu Kimlik Bilgileri** ayarları sayfasında kimlik bilgilerini güncelleştirin ve ekleyin. 

Sihirbazda gerekli tüm ayarları doldurduktan sonra, değişikliği göndermek için **Filer Ekle** düğmesini tıklatın.

Birkaç dakika sonra, depolama sistemi **Pano** çekirdek dosyalayıcıları listesinde görünür ve çekirdek filer ayarları sayfalarından erişilebilir.

![çekirdek filer "Flurry-NAS" Çekirdek Filers ayarları yönet sayfasında, filer ayrıntıları görünümü genişletilmiş](media/fxt-cluster-config/core-filer-in-manage-page.png)

Bu ekran görüntüsündeki çekirdek filer bir vserver eksik. Çekirdek filer'ı bir vserver'a bağlamalı ve istemcilerin depolama alanına erişebilmeleri için bir bağlantı oluşturmanız gerekir. Bu [adımlar, ad alanını yapılandır'da](#configure-the-namespace)aşağıda açıklanmıştır.

## <a name="configure-the-namespace"></a>Ad alanını yapılandırma

Azure FXT Edge Filer kümesi, istemcinin farklı arka uç sistemlerinde depolanan verilere erişimini kolaylaştıran *küme ad alanı* adı verilen sanal bir dosya sistemi oluşturur. İstemciler sanal bir yol kullanarak dosya istediğinden, depolama sistemleri istemci iş akışını değiştirmek zorunda kalmadan eklenebilir veya değiştirilebilir. 

Küme ad alanı, bulut ve NAS depolama sistemlerini benzer bir dosya yapısında sunmanıza da olanak tanır. 

Kümenin vservers ad alanı korumak ve istemcilere içerik hizmet vermektedir. Küme ad alanını oluşturmak için iki adım vardır: 

1. Vserver oluşturma 
1. Arka uç depolama sistemleri ile istemciye bakan dosya sistemi yolları arasındaki bağlantıları ayarlama 

### <a name="create-a-vserver"></a>Vserver oluşturma

VServers, istemci ve kümenin temel dosyalayıcıları arasında verilerin nasıl aktığını kontrol eden sanal dosya sunucularıdır:

* VServers istemciye bakan IP adreslerini barındırıyor
* VServers ad alanı oluşturmak ve arka uç depolama dışa aktarımistemciye bakan sanal dizin yapısını eşleyen kavşakları tanımlar
* VServers çekirdek filer dışa aktarma ilkeleri ve kullanıcı kimlik doğrulama sistemleri de dahil olmak üzere dosya erişim denetimleri, uygulamak
* VServers SMB altyapı sağlar

Küme vserver'ı yapılandırmaya başlamadan önce, bağlı belgeleri okuyun ve ad alanı ve vservers'ı anlamanıza yardımcı olmak için Microsoft temsilcinize danışın. VN'leri kullanıyorsanız, vserver'ı oluşturmadan önce [bunları oluşturun.](fxt-configure-network.md#adjust-network-settings) 

Küme Yapılandırma Kılavuzu'nun bu bölümleri, FXT vserver ve genel ad alanı özelliklerini tanımanıza yardımcı olur:

* [VServers Oluşturma ve Çalışma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#creating-and-working-with-vservers)
* [Genel Ad Alanı kullanma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html)
* [VServer Oluşturma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vserver_manage.html#creating-a-vserver)

Kümeniz için en az bir vserver gerekir. 

Yeni bir vserver oluşturmak için aşağıdaki bilgilere ihtiyacınız var:

* vserver için ayarlanan ad

* Vserver'ın işleyeceğini istemciye bakan IP adresleri aralığı

  Vserver'ı oluştururken tek bir bitişik IP adresi aralığı sağlamanız gerekir. Daha sonra **İstemci Karşı Karşıya Ağ** ayarları sayfasını kullanarak daha fazla adres ekleyebilirsiniz.

* Ağınızda VLAN'lar varsa, bu vserver için hangi VLAN'ı kullanacak

Yeni bir **vserver** > oluşturmak için**VServer Yönet VServers** ayarları sayfasını kullanın. Ayrıntılar için Cluster Configuration Guide'da [VServer oluşturma'yı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vserver_manage.html#creating-a-vserver) okuyun. 

![yeni bir vserver oluşturmak için açılır pencere](media/fxt-cluster-config/new-vserver.png)

### <a name="create-a-junction"></a>Bir kavşak oluşturma

*Birbağlantı,* istemci tarafından görülebilen ad alanıyla arka uç depolama yolunu eşler.

Bu sistemi istemci montaj noktalarında kullanılan yolu basitleştirmek ve kapasiteyi sorunsuz bir şekilde ölçeklendirmek için kullanabilirsiniz, çünkü bir sanal yol birden çok çekirdek dosyalayıcıdan depolama yı barındırabilir.

![Ayarları dolu Yeni Bağlantı sihirbazı sayfası ekle](media/fxt-cluster-config/add-junction-full.png)

Ad alanı bağlantısı oluşturma yla ilgili tüm ayrıntılar için Küme Yapılandırma Kılavuzu'ndaki [ **VServer** > Ad Alanına](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html) bakın.

![VServer > İsim Alanı ayarları sayfası bir kavşak için ayrıntıları gösteren](media/fxt-cluster-config/namespace-populated.png)

## <a name="configure-export-rules"></a>Dışa aktarma kurallarını yapılandırma

Hem vserver hem de core filer'e sahip olduktan sonra, istemcilerin çekirdek filer dışa aktarımlarında dosyalara nasıl erişebileceğini denetleyen dışa aktarma kurallarını ve dışa aktarma ilkelerini özelleştirmeniz gerekir.

İlk olarak, yeni kurallar eklemek, varsayılan ilkeyi değiştirmek veya kendi özel dışa aktarma ilkenizi oluşturmak için **VServer** > **Dışa Aktarma Kuralları** sayfasını kullanın.

İkinci olarak, özelleştirilmiş ilkeyi bu vserver üzerinden erişildiğinde çekirdek filer'inizin dışa aktarımlarına uygulamak için **VServer** > **Dışa Aktarma İlkeleri** sayfasını kullanın.

Ayrıntılar için Çekirdek [Filer Dışa Aktarmalarına Erişimi Kontrol Eden](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/export_rules_overview.html) Küme Yapılandırma Kılavuzu makalesini okuyun.


## <a name="next-steps"></a>Sonraki adımlar

Depolama alanı ekledikten ve istemciye bakan ad alanını yapılandırdıktan sonra, kümenizin ilk kurulumunu tamamlayın: 

> [!div class="nextstepaction"]
> [Kümenin ağ ayarlarını yapılandırma](fxt-configure-network.md)
