---
title: Azure için avere vFXT için olağanüstü durum kurtarma Kılavuzu
description: Azure için avere vFXT içindeki verileri yanlışlıkla silme veya kesintilerden koruma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: rohogue
ms.openlocfilehash: 4a8dbe9d1c66955e585f7b2cbf842193c965252e
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74998777"
---
# <a name="disaster-recovery-guidance-for-avere-vfxt-for-azure"></a>Azure için avere vFXT için olağanüstü durum kurtarma Kılavuzu

Bu makalede, Azure iş akışı için avere vFXT 'nizi korumanıza yönelik stratejiler özetlenmektedir ve kazalardan dolayı veya kesintiler arasından kurtarma yapabilmeniz için verileri yedeklemeye yönelik rehberlik sunulmaktadır.

Azure için avere vFXT, verileri önbellekte geçici olarak depolar. Veriler, arka uç depolama sistemlerinde şirket içi donanım sistemleri, Azure Blob depolama kapsayıcıları veya her ikisi de uzun süreli depolanır.

Kesintilere ve olası veri kaybına karşı koruma sağlamak için şu dört alanı göz önünde bulundurun:

* Azure sistemi için bir avere vFXT kullanılamaz hale gelirse kapalı kalma süresine karşı koruma
* Küme önbelleğindeki verileri koruma
* Arka uç NAS donanım depolamadaki verileri koruma
* Arka uç Azure Blob bulut depolamadaki verileri koruma

Azure müşterisi için her avere vFXT, bu öğeler için planlar içeren kendi kapsamlı olağanüstü durum kurtarma planını oluşturmalıdır. Ayrıca, vFXT kümesiyle kullandığınız uygulamalara dayanıklılık de oluşturabilirsiniz. Yardım için [sonraki adımlarda](#next-steps) bulunan bağlantıları okuyun.

## <a name="protect-against-downtime"></a>Kapalı kalma süresine karşı koruma

Yedeklilik, Azure ürünü için avere vFXT 'de yerleşik olarak bulunur:

* Küme yüksek oranda kullanılabilir ve tek küme düğümleri, en az kesintiyle yük devredebilirler.
* Önbellekte değiştirilen veriler, uzun vadeli depolama için arka uç çekirdek filtrelerin (donanım NAS veya Azure blob) için düzenli olarak yazılmıştır.

Azure kümesi için her avere vFXT tek bir kullanılabilirlik alanında bulunmalıdır, ancak bölgesel bir kesinti olması durumunda hızlıca erişim sağlamak için farklı bölgelerde veya farklı bölgelerde bulunan yedekli kümeleri de kullanabilirsiniz.

Ayrıca, verilere erişimi kaybetmekten endişe ediyorsanız, depolama kapsayıcılarını birden çok bölgeye yerleştirebilirsiniz. Bununla birlikte, bölgeler arasındaki işlemlerin, bir bölgede yer alan işlemlerden daha yüksek gecikme süresine ve daha yüksek maliyetli olduğunu aklınızda bulundurun.

## <a name="protect-data-in-the-cluster-cache"></a>Küme önbelleğindeki verileri koruma

Önbelleğe alınan veriler, düzenli kapatmadan önce her zaman çekirdek filtrelerin 'ye yazılır, ancak denetlenmeyen bir kapatmayla, önbellekteki değiştirilen veriler kaybolabilir.

Yalnızca dosya okumalarını iyileştirmek için kümeyi kullanırsanız, kaybetmek herhangi bir değişiklik yoktur. Ayrıca, dosya değişikliklerini (yazma) önbelleğe almak için kümeyi kullanıyorsanız, çekirdek filers [önbellek ilkelerinin](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_manage_cache_policies.html) uygulanıp ayarlanmayacağını düşünün<!-- link to legacy doc --> verilerin uzun süreli depolamaya ne sıklıkta yazıldığını özelleştirmek için.

Genel olarak, kurtarma planınız, daha fazla veri tutan ve genellikle bir hatadan sonra iş akışınızı yeniden oluşturmak için daha önemli olan arka uç depolama sistemlerini yedeklemeye odaklanmalıdır.

## <a name="protect-data-in-nas-core-filers"></a>NAS Core filtrelerin 'teki verileri koruma

Şirket içi bir NAS donanım çekirdeği filklerinde depolanan verileri, anlık görüntüler ve tam yedeklemeler de dahil olmak üzere, NAS sağlayıcısı tarafından önerildiği şekilde korumak için kabul edilen yöntemleri kullanın. Bu çekirdek dosyasıları için olağanüstü durum kurtarma, bu makalenin kapsamı dışındadır.

## <a name="protect-data-in-azure-blob-storage"></a>Azure Blob depolamada verileri koruma

Azure için avere vFXT, Azure Blob çekirdek dosyasıları için yerel olarak yedekli depolama (LRS) kullanır. Bu, blob kapsayıcılarınızdaki verilerin veri merkezindeki geçici donanım arızalarına karşı koruma için otomatik olarak kopyalandığı anlamına gelir.

Bu bölümde, seyrek bölge genelinde kesintiler veya yanlışlıkla silinmelerden BLOB depolama alanındaki verilerinizi daha fazla korumaya yönelik ipuçları verilmektedir.

Azure Blob depolamada verileri korumaya yönelik en iyi uygulamalar şunlardır:

* Kritik verilerinizi başka bir bölgedeki başka bir depolama hesabına (genellikle olağanüstü durum kurtarma planınız tarafından belirlenen sıklıkta) kopyalayın.
* Yanlışlıkla silme veya bozulmayı engellemek için tüm hedef sistemlerdeki verilere erişimi denetleyin. Veri depolamada [kaynak kilitleri](../azure-resource-manager/resource-group-lock-resources.md) kullanmayı göz önünde bulundurun.
* Blob Core filers için avere vFXT for Azure [Cloud Snapshot](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_snapshot_policies.html>) özelliğini etkinleştirin.

### <a name="copy-avere-vfxt-core-filer-data-to-a-backup-account"></a>Avere vfxt Core dosyalayıcı verilerini bir yedekleme hesabına Kopyala

Başka bir hesapta veri yedeklemesi oluşturmak için bu adımları izleyin.

1. Gerekirse, yeni bir şifreleme anahtarı oluşturun ve etkilenen sistemlerin dışında güvenli bir şekilde depolayın.

   Verileriniz Azure kümesi için avere vFXT tarafından şifrelendiyse, verileri başka bir depolama hesabına kopyalamadan önce yeni bir şifreleme anahtarı oluşturmanız gerekir. Bu anahtar ve parolayı güvenli ve bölgesel bir hatadan etkilenmeyecek bir tesisde güvenli bir şekilde depolayın.

   Bu anahtarı, özgün kümesine yeniden ekleseniz bile, kapsayıcıyı bir kümeye eklerken sağlamanız gerekir.

   [Bulut şifreleme ayarlarını](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_encryption_settings.html>) okuma<!-- link to legacy doc site --> ayrıntılı bilgi edinmek için.

   Kapsayıcınız yalnızca Azure yerleşik şifrelemesini kullanıyorsa, bu adımı atlayabilirsiniz.

1. Çekirdek filleyici 'yi sistemden kaldırın. Bu, kümeyi değiştirilen tüm verileri arka uç depolamaya yazmaya zorlar.

   Yedeklemeden sonra çekirdek filleyici 'yi yeniden eklemek zorunda olsanız da, tüm verilerin arka uca tamamen yazıldığından emin olmanın en iyi yoludur. ("Askıya al" seçeneği bazen önbellekte değiştirilen verileri bırakabilir.) <!-- xxx true? or just metadata? -->

   Yedeklemeden sonra kapsayıcıyı yeniden eklediğinizde, çoğaltmayı çoğaltabilmeniz için, çekirdek filinin adını ve birleşim bilgilerini (Denetim Masası 'nda **ad alanı** sayfasında listelenir) not edin.

   Çekirdek filni kaldırmak için küme Denetim Masası 'nı kullanın. [Küme Denetim Masası 'nı açın](avere-vfxt-cluster-gui.md) ve çekirdek **dosyalayıcı** > , **çekirdek filtrelerin 'yi yönetin**. Yedeklemek istediğiniz depolama sistemini bulun ve **Kaldır** düğmesini kullanarak kümeden silin.

1. Başka bir bölgedeki başka bir depolama hesabında yeni, boş bir BLOB depolama kapsayıcısı oluşturun.

1. Çekirdek fildekdeki verileri yeni kapsayıcıya kopyalamak için herhangi bir uygun kopyalama aracını kullanın. Kopyalama, verileri değişiklik olmadan çoğaltmalıdır ve Azure için avere vFXT tarafından kullanılan özel bulut dosya biçimini kesintiye uğratmadan. Azure tabanlı araçlar [AzCopy](../storage/common/storage-use-azcopy-v10.md), [Azure PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)ve [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)içerir.

1. Verileri yedekleme kapsayıcısına kopyaladıktan sonra, [depolama birimi yapılandırma](avere-vfxt-add-storage.md)bölümünde açıklandığı gibi özgün kapsayıcıyı kümeye geri ekleyin.

   * İstemci iş akışlarının değiştirilmesine gerek kalmaması için aynı çekirdek dosyalayıcı adını ve birleşim bilgilerini kullanın.
   * **Demet içeriği** değerini var olan veri seçeneğine ayarlayın.
   * Kapsayıcı küme tarafından şifrelendiyse, içeriği için geçerli şifreleme anahtarını girmeniz gerekir. (Birinci adımda güncelleştirdiğiniz anahtardır.)

İlk bunlardan sonra yedeklemeler için yeni bir depolama kapsayıcısı oluşturmanız gerekmez. Ancak, geçerli anahtarı hatırlamanız gereken bir yerde depolandığından emin olmak için her yedekleme yaptığınızda yeni bir şifreleme anahtarı oluşturmayı düşünün.

### <a name="access-a-backup-data-source-during-an-outage"></a>Kesinti sırasında yedekleme veri kaynağına erişme

Azure kümesi için bir avere vFXT 'den yedekleme kapsayıcısına erişmek için şu işlemi izleyin:

1. Gerekirse, etkilenmeyen bir bölgede Azure kümesi için yeni bir avere vFXT oluşturun.

   > [!TIP]
   > Azure kümesi için bir avere vFXT oluşturduğunuzda, oluşturma şablonu ve parametrelerinin bir kopyasını kaydedebilirsiniz. Birincil kümenizi oluştururken bu bilgileri kaydederseniz, aynı özelliklere sahip bir değiştirme kümesi oluşturmak için kullanabilirsiniz. [Doğrulama ve Özet](avere-vfxt-deploy.md#validation-and-purchase) sayfasında, **şablon ve parametreleri indir** bağlantısına tıklayın. Kümeyi oluşturmak için **Tamam** düğmesine tıklamadan önce bu bilgileri bir dosyaya kaydedin.

1. Yinelenen blob kapsayıcısına işaret eden yeni bir bulut çekirdeği ekleyin.

   Hedef kapsayıcının, çekirdek filme oluşturma sihirbazının **demet içeriği** ayarında verileri zaten içerdiğini belirttiğinizden emin olun. (Bu kümeyi yanlışlıkla **boş**bırakırsanız, sistem sizi uyarır.)  <!-- you can't add a populated volume at cluster creation time via template, only create a fresh one -->

1. Gerekirse, istemcileri yeni kümeyi veya orijinal yerine yeni çekirdek fili taktikleri şekilde güncelleştirin. (Yeni kümeyi yeni bir IP adresine bağlamanız gerekmiyorsa, önceki çekirdek Filini özgün kapsayıcının aynı adı ve birleşim yoluyla eklerseniz, istemci süreçlerini güncelleştirmeniz gerekmez.)

## <a name="next-steps"></a>Sonraki adımlar

* Azure için avere vFXT ayarlarını özelleştirme hakkında daha fazla bilgi için, [küme ayarlamayı](avere-vfxt-tuning.md)okuyun.
* Azure 'da olağanüstü durum kurtarma ve dayanıklı uygulamalar oluşturma hakkında daha fazla bilgi edinin:

  * [Azure dayanıklılık teknik kılavuzu](https://docs.microsoft.com/azure/architecture/framework/resiliency/overview)
  * [Bölge genelinde hizmet kesintiinden kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region)
  * [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](<https://docs.microsoft.com/azure/resiliency/resiliency-disaster-recovery-high-availability-azure-applications>)
  <!-- can't find these in the source tree to use relative links -->
