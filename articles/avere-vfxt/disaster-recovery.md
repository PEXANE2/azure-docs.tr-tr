---
title: Azure için Avere vFXT için olağanüstü durum kurtarma kılavuzu
description: Azure için Avere vFXT'deki verileri yanlışlıkla silme veya kesintilere karşı nasıl koruyulur?
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: rohogue
ms.openlocfilehash: 28278f76497d6e9d0fee221bb4ef32fe6d369db0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75966660"
---
# <a name="disaster-recovery-guidance-for-avere-vfxt-for-azure"></a>Azure için Avere vFXT için olağanüstü durum kurtarma kılavuzu

Bu makalede, Avere vFXT'nizi Azure iş akışı için koruma stratejileri özetler ve kazalardan veya kesintilerden kurtulabilmeniz için verileri yedeklemek için kılavuz verir.

Azure için Avere vFXT verileri geçici olarak önbelleğinde depolar. Veriler, şirket içi donanım sistemleri, Azure Blob depolama kapları veya her ikisi birden dahilolmak üzere arka uç depolama sistemlerinde uzun süreli olarak depolanır.

Kesintilere ve olası veri kaybına karşı korunmak için şu dört alanı göz önünde bulundurun:

* Azure sistemi için bir Avere vFXT kullanılamıyorsa kapalı kalma sürelerine karşı koruma
* Küme önbelleğindeki verileri koruma
* Arka uç NAS donanım depolamasında verileri koruma
* Arka uç Azure Blob bulut depolamasında verileri koruma

Azure müşterisi için her Avere vFXT, bu öğeleriçin planlar içeren kendi kapsamlı olağanüstü durum kurtarma planını oluşturmalıdır. Ayrıca vFXT kümesi ile kullandığınız uygulamalara esneklik oluşturabilirsiniz. Yardım için [Sonraki adımlardaki](#next-steps) bağlantıları okuyun.

## <a name="protect-against-downtime"></a>Kapalı kalma sürelerine karşı koruyun

Artıklık, Azure ürünü için Avere vFXT'de yerleşiktir:

* Küme yüksek kullanılabilir ve tek tek küme düğümleri en az kesinti ile üzerinde başarısız olabilir.
* Önbellekte değiştirilen veriler, uzun süreli depolama için arka uç çekirdek dosyalayıcılara (donanım NAS veya Azure Blob) düzenli olarak yazılır.

Azure kümesi için her Avere vFXT tek bir kullanılabilirlik bölgesinde bulunmalıdır, ancak bölgesel bir kesinti durumunda hızlı bir şekilde erişim sağlamak için farklı bölgelerde veya farklı bölgelerde bulunan gereksiz kümeleri kullanabilirsiniz.

Verilere erişimi kaybetme konusunda endişeleriniz varsa, depolama kapsayıcılarını birden çok bölgede konumlandırabilirsiniz. Ancak, bölgeler arasındaki işlemlerin bir bölge içinde kalan işlemlerden daha yüksek gecikme süreye ve daha yüksek maliyete sahip olduğunu unutmayın.

## <a name="protect-data-in-the-cluster-cache"></a>Küme önbelleğindeki verileri koruma

Önbelleğe alınan veriler her zaman normal bir kapatmadan önce çekirdek dosyalayıcılara yazılır, ancak denetimsiz kapatmada, önbellekteki değiştirilen veriler kaybedilebilir.

Kümeyi yalnızca dosya okumalarını en iyi duruma getirmek için kullanırsanız, kaybedecek değişiklik yoktur. Kümeyi dosya değişikliklerini önbelleğe almak için de kullanıyorsanız (yazıyor), çekirdek [dosyalayıcıların önbellek ilkelerini](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_manage_cache_policies.html) ayarlayıp ayarlamayacağınızı düşünün<!-- link to legacy doc --> verilerin uzun süreli depolama alanına ne sıklıkta yazıldığını özelleştirmek için kullanılır.

Genel olarak, kurtarma planınız, daha fazla veri tutan ve genellikle bir hatadan sonra iş akışınızı yeniden oluşturmak için daha önemli olan arka uç depolama sistemlerini yedeklemeye odaklanmalıdır.

## <a name="protect-data-in-nas-core-filers"></a>NAS çekirdek dosyalayıcılarında verileri koruma

NAS sağlayıcısı tarafından önerilen anlık görüntüler ve tam yedeklemeler de dahil olmak üzere şirket içi NAS donanım çekirdeği filer'inde depolanan verileri korumak için kabul edilen yöntemleri kullanın. Bu çekirdek filers için olağanüstü durum kurtarma bu makalenin kapsamı dışındadır.

## <a name="protect-data-in-azure-blob-storage"></a>Azure Blob depolama alanında verileri koruma

Azure için Avere vFXT, Azure Blob çekirdek dosyalayıcıları için yerel olarak yedekli depolama (LRS) kullanır. Bu, Blob kapsayıcılarınızdaki verilerin veri merkezi içindeki geçici donanım hatalarına karşı korunmak için otomatik olarak kopyalandığında anlamına gelir.

Bu bölümde, Blob depolamasundaki verilerinizi bölge genelindeki nadir kesintilere veya yanlışlıkla silmelere karşı nasıl daha fazla koruyacağınıza ilişkin ipuçları verilmiştir.

Azure Blob depolama alanında verileri korumak için en iyi uygulamalar şunlardır:

* Kritik verilerinizi sık sık başka bir bölgedeki başka bir depolama hesabına kopyalayın (genellikle olağanüstü durum kurtarma planınız tarafından belirlendiği gibi).
* Yanlışlıkla silinmesini veya bozulmasını önlemek için tüm hedef sistemlerdeki verilere erişimi denetleyin. Veri depolamada [kaynak kilitleri](../azure-resource-manager/management/lock-resources.md) kullanmayı düşünün.
* Blob çekirdek dosyalarınız için Azure [bulut anlık görüntü](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_snapshot_policies.html>) özelliği için Avere vFXT özelliğini etkinleştirin.

### <a name="copy-avere-vfxt-core-filer-data-to-a-backup-account"></a>Avere vFXT çekirdek filer verilerini yedek bir hesaba kopyalama

Başka bir hesapta veri yedeklemesi oluşturmak için aşağıdaki adımları izleyin.

1. Gerekirse, yeni bir şifreleme anahtarı oluşturun ve etkilenen sistemlerin dışında güvenli bir şekilde saklayın.

   Verileriniz Azure kümesi için Avere vFXT tarafından şifrelenmişse, verileri başka bir depolama hesabına kopyalamadan önce yeni bir şifreleme anahtarı oluşturmanız gerekir. Bu anahtarı ve parolayı güvenli ve bölgesel bir hatadan etkilenmeyecek bir tesiste güvenli bir şekilde saklayın.

   Kapsayıcıyı özgün kümesine yeniden ekliyor olsanız bile, bu anahtarı bir kümeye eklerken sağlamanız gerekir.

   [Bulut şifreleme ayarlarını](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_encryption_settings.html>) okuyun<!-- link to legacy doc site --> ayrıntılı bilgi için.

   Kapsayıcınız yalnızca Azure'un yerleşik şifrelemesi kullanıyorsa, bu adımı atlayabilirsiniz.

1. Çekirdek filer'ı sistemden çıkarın. Bu, kümeyi değiştirilen tüm verileri arka uç depolamasına yazmaya zorlar.

   Yedeklemeden sonra çekirdek filer'i yeniden eklemeniz gerekse de, tüm verilerin tamamen arka uca yazıldığını garanti etmenin en iyi yoludur. ("Askıya alma" seçeneği bazen değiştirilen verileri önbellekte bırakabilir.) <!-- xxx true? or just metadata? -->

   Yedekten sonra kapsayıcıyı yeniden eklediğinizde çoğaltmak için çekirdek filer'In adını ve bağlantı bilgilerini (denetim panelindeki **Ad Alanı** sayfasında listelenen) not edin.

   Çekirdek filer kaldırmak için küme denetim paneli kullanın. [Küme denetim panelini açın](avere-vfxt-cluster-gui.md) ve **Core filer** > **Manage core filers'ı**seçin. Yedeklemek istediğiniz depolama sistemini bulun ve kümeden silmek için **Kaldır** düğmesini kullanın.

1. Başka bir bölgedeki başka bir depolama hesabında yeni, boş bir Blob depolama kapsayıcısı oluşturun.

1. Çekirdek filer'daki verileri yeni kapsayıcıya kopyalamak için uygun bir kopyalama aracı kullanın. Kopya, verileri değişiklik yapmadan ve Azure için Avere vFXT tarafından kullanılan özel bulut dosya sistemi biçimini bozmadan çoğaltmalıdır. Azure tabanlı araçlar arasında [AzCopy,](../storage/common/storage-use-azcopy-v10.md) [Azure PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)ve [Azure Veri Fabrikası](../data-factory/connector-azure-data-lake-store.md)bulunmaktadır.

1. Verileri yedekleme kapsayıcısına kopyaladıktan sonra, [yapı depolama](avere-vfxt-add-storage.md)alanında açıklandığı gibi özgün kapsayıcıyı kümeye geri ekleyin.

   * İstemci iş akışlarının değişmesi gerekmesin diye aynı çekirdek filer adını ve bağlantı bilgilerini kullanın.
   * Kova **içeriği** değerini varolan veri seçeneğine ayarlayın.
   * Kapsayıcı küme tarafından şifrelenmişse, içeriği için geçerli şifreleme anahtarını girmeniz gerekir. (Bu, birinci adımda güncellediğiniz anahtardır.)

İlkinden sonraki yedeklemeler için yeni bir depolama kapsayıcısı oluşturmanız gerekmez. Ancak, geçerli anahtarın hatırladığınız bir yerde depolandığınızdan emin olmak için yedeklemeyi her yaptığınızda yeni bir şifreleme anahtarı oluşturmayı düşünün.

### <a name="access-a-backup-data-source-during-an-outage"></a>Kesinti sırasında yedek bir veri kaynağına erişin

Yedekleme kapsayıcısına Azure kümesi için Bir Avere vFXT'den erişmek için aşağıdaki işlemi izleyin:

1. Gerekirse, etkilenmemiş bir bölgede Azure kümesi için yeni bir Avere vFXT oluşturun.

   > [!TIP]
   > Azure kümesi için bir Avere vFXT oluşturduğunuzda, oluşturma şablonunun ve parametrelerinin bir kopyasını kaydedebilirsiniz. Birincil kümenizi oluştururken bu bilgileri kaydederseniz, aynı özelliklere sahip yedek bir küme oluşturmak için kullanabilirsiniz. [Özet](avere-vfxt-deploy.md#validation-and-purchase) sayfasında, İndir **meylesini ve parametrelerbağlantısını** tıklatın. Kümeyi oluşturmadan önce bilgileri bir dosyaya kaydedin.

1. Yinelenen Blob kapsayıcısını işaret eden yeni bir bulut çekirdeği filer ekleyin.

   Hedef kapsayıcının çekirdek filer oluşturma sihirbazının **Kova içeriği** ayarında zaten veri içerdiğini belirtmeyi unutmayın. (Yanlışlıkla bu kümeyi **Boş**olarak bırakırsanız sistem sizi uyarmalıdır.)  <!-- you can't add a populated volume at cluster creation time via template, only create a fresh one -->

1. Gerekirse, istemcileri orijinal yerine yeni kümeye veya yeni çekirdek filer'e monte edebilecekleri şekilde güncelleştirin. (Değiştirme çekirdek filer orijinal kapsayıcı olarak aynı ad ve bağlantı yolu ile eklerseniz, yeni bir IP adresine yeni küme monte etmek gerekmedikçe istemci işlemlerini güncelleştirmek gerekmez.)

## <a name="next-steps"></a>Sonraki adımlar

* Azure için Avere vFXT ayarlarını özelleştirme hakkında daha fazla bilgi için [Cluster ayarlarını](avere-vfxt-tuning.md)okuyun.
* Azure'da olağanüstü durum kurtarma ve esnek uygulamalar oluşturma hakkında daha fazla bilgi edinin:

  * [Azure dayanıklılık teknik kılavuzu](https://docs.microsoft.com/azure/architecture/framework/resiliency/overview)
  * [Bölge genelindeki hizmet kesintisinden kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region)
  * [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](<https://docs.microsoft.com/azure/resiliency/resiliency-disaster-recovery-high-availability-azure-applications>)
  <!-- can't find these in the source tree to use relative links -->
