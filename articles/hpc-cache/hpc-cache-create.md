---
title: Azure HPC Önbelleği Oluşturma
description: Azure HPC Önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 04/15/2020
ms.author: v-erkel
ms.openlocfilehash: befbe2435a518b82cf5a3ab12e6129aa3ce5c22b
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537992"
---
# <a name="create-an-azure-hpc-cache"></a>Azure HPC Önbelleği Oluşturma

Önbelleğinizi oluşturmak için Azure portalını kullanın.

![alt kısmında oluştur düğmesi ile Azure portalında önbelleğe genel bakış ekran görüntüsü](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Temel ayrıntıları tanımlama

![Azure portalında proje ayrıntıları sayfasının ekran görüntüsü](media/hpc-cache-create-basics.png)

**Project Details'da**önbelleği barındıracak abonelik ve kaynak grubunu seçin.

**Hizmet Ayrıntıları'nda**önbellek adını ve diğer öznitelikleri ayarlayın:

* Konum - Desteklenen [bölgelerden](hpc-cache-overview.md#region-availability)birini seçin.
* Sanal ağ - Varolan bir ağ seçebilir veya yeni bir sanal ağ oluşturabilirsiniz.
* Alt ağ - Yalnızca bu Azure HPC Önbelleği örneği için kullanılacak en az 64 IP adresine (/24) sahip bir alt ağ seçin veya oluşturun.

## <a name="set-cache-capacity"></a>Önbellek kapasitesini ayarlama
<!-- referenced from GUI - update aka.ms link if you change this header text -->

**Önbellek** sayfasında önbelleğinizin kapasitesini ayarlamanız gerekir. Burada ayarlanan değerler, önbelleğinizin ne kadar veri tutabileceğini ve istemci isteklerine ne kadar hızlı hizmet verebileceğini belirler.

Kapasite önbelleğin maliyetini de etkiler.

Bu iki değeri ayarlayarak kapasiteyi seçin:

* ÖNbellek (iş verimi) için GB/saniye'de maksimum veri aktarım hızı
* Önbelleğe alınan veriler için ayrılan depolama miktarı, TB'de

Kullanılabilir iş yapma değerlerinden ve önbellek depolama boyutlarından birini seçin.

Gerçek veri aktarım hızının iş yüküne, ağ hızlarına ve depolama hedeflerinin türüne bağlı olduğunu unutmayın. Seçtiğiniz değerler, tüm önbellek sistemi için maksimum iş kısmını ayarlar, ancak bunların bir kısmı genel ek görev için kullanılır. Örneğin, bir istemci önbellekte zaten depolanmış olmayan bir dosya isterse veya dosya eski olarak işaretlenmişse, önbelleğiniz arka uç depolamaalanından almak için bazı iş bilgisini kullanır.

Azure HPC Önbelleği, önbellek isabet oranlarını en üst düzeye çıkarmak için hangi dosyaların önbelleğe alındığını ve önceden yüklendiğini yönetir. Önbellek içeriği sürekli olarak değerlendirilir ve dosyalara daha az sıklıkta erişildiğinde uzun süreli depolama alanına taşınır. Meta veriler ve diğer ek bilgiler için ek alan içeren etkin çalışma dosyaları kümesini rahatça tutabilen bir önbellek depolama boyutu seçin.

![önbellek boyutlandırma sayfasının ekran görüntüsü](media/hpc-cache-create-capacity.png)

## <a name="enable-azure-key-vault-encryption-optional"></a>Azure Anahtar Kasası şifrelemesini etkinleştirme (isteğe bağlı)

Önbelleğiniz müşteri tarafından yönetilen şifreleme anahtarlarını destekleyen bir bölgedeyse, **Önbellek** ve **Etiketler** sekmeleri arasında **Disk şifreleme anahtarları** sayfası görüntülenir. Yayın saati itibariyle bu seçenek Doğu ABD, Güney Orta ABD ve Batı ABD 2'de desteklenir.

Önbellek depolamanızda kullanılan şifreleme anahtarlarını yönetmek istiyorsanız, Azure Anahtar Kasası bilgilerinizi **Disk şifreleme anahtarları** sayfasında niçin sağlayabilirsiniz. Anahtar kasası aynı bölgede ve önbellekle aynı abonelikte olmalıdır.

Müşteri tarafından yönetilen anahtarlara ihtiyacınız yoksa bu bölümü atlayabilirsiniz. Azure, verileri Varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifreler. Daha fazla bilgi edinmek için [Azure depolama şifrelemesini](../storage/common/storage-service-encryption.md) okuyun.

> [!NOTE]
>
> * Önbelleği oluşturduktan sonra Microsoft tarafından yönetilen anahtarlar ile müşteri tarafından yönetilen anahtarlar arasında geçiş yapamazsınız.
> * Önbellek oluşturulduktan sonra, anahtar kasasına erişmek için yetki vermelisiniz. Şifrelemeyi açmak için önbelleğin **Genel Bakış** sayfasında **şifrelemeyi etkinleştir** düğmesini tıklatın. Önbelleği oluşturduktan sonra 90 dakika içinde bu adımı atın.
> * Önbellek diskleri bu yetkilendirmeden sonra oluşturulur. Bu, ilk önbellek oluşturma süresinin kısa olduğu, ancak önbelleğin erişimi yetkilendirmeden sonra on dakika veya daha uzun süre kullanılmaya hazır olmadığı anlamına gelir.

Müşteri tarafından yönetilen anahtar şifreleme işleminin tam açıklaması [için Azure HPC Önbelleği için müşteri tarafından yönetilen şifreleme anahtarlarını kullan'ı](customer-keys.md)okuyun.

!["Müşteri yönetilen" seçili ve anahtar kasa alanları gösteren şifreleme anahtarları sayfasının ekran görüntüsü](media/create-encryption.png)

**Müşteri'yi** seçin müşteri tarafından yönetilen anahtar şifrelemesini seçti. Anahtar kasa belirtimi alanları görüntülenir. Kullanmak üzere Azure Anahtar Kasası'nı seçin, ardından bu önbellek için kullanılacak anahtarı ve sürümü seçin. Anahtar 2048 bit RSA anahtarı olmalıdır. Bu sayfadan yeni bir anahtar kasası, anahtar veya anahtar sürümü oluşturabilirsiniz.

Önbelleği oluşturduktan sonra, anahtar kasa hizmetini kullanmak için yetkilendirmeniz gerekir. Ayrıntılar için [önbellekten Azure Anahtar Kasası Şifrelemesini Yetkilendirme'yi](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache) okuyun.

## <a name="add-resource-tags-optional"></a>Kaynak etiketleri ekleme (isteğe bağlı)

**Etiketler** sayfası, Azure HPC Önbellek örneğinize [kaynak etiketleri](https://go.microsoft.com/fwlink/?linkid=873112) eklemenize olanak tanır.

## <a name="finish-creating-the-cache"></a>Önbelleği oluşturmayı bitirme

Yeni önbelleği yapılandırıldıktan sonra **Gözden Geçir + oluştur** sekmesini tıklatın. Portal seçimlerinizi doğrular ve seçimlerinizi gözden geçirmenizi sağlar. Her şey doğruysa, **Oluştur'u**tıklatın.

Önbellek oluşturma yaklaşık 10 dakika sürer. Azure portalının bildirimler panelinde ilerlemeyi izleyebilirsiniz.

![portalda önbellek oluşturma "dağıtım devam ediyor" ve "bildirimler" sayfalarının ekran görüntüsü](media/hpc-cache-deploy-status.png)

Oluşturma sona erdiğinde, yeni Azure HPC Önbelleği örneğine bağlantı yla bir bildirim görüntülenir ve önbellek aboneliğinizin **Kaynaklar** listesinde görünür.

![Azure portalındaki Azure HPC Önbelleği örneğinin ekran görüntüsü](media/hpc-cache-new-overview.png)

> [!NOTE]
> Önbelleğiniz müşteri tarafından yönetilen şifreleme anahtarlarını kullanıyorsa, önbellek dağıtım durumu tamamlanmadan önce kaynaklar listesinde görünebilir. Önbelleğin durumu **anahtar için bekler** beklemez, anahtar kasasını kullanmak için [yetki](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache) verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Önbelleğiniz **Kaynaklar** listesinde göründükten sonra bir sonraki adıma geçebilirsiniz.

* Önbelleğinizin veri kaynaklarına erişmesini sağlamak için [depolama hedeflerini tanımlayın.](hpc-cache-add-storage.md)
* Müşteri tarafından yönetilen şifreleme anahtarlarını kullanıyorsanız, önbellek kurulumunuzu tamamlamak için önbelleğin genel bakış sayfasından [Azure Key Vault şifrelemesini yetkilendirmeniz](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache) gerekir. Depolama alanı eklemeden önce bu adımı yapmanız gerekir. Ayrıntılar için [müşteri tarafından yönetilen şifreleme anahtarlarını kullan'ı](customer-keys.md) okuyun.
