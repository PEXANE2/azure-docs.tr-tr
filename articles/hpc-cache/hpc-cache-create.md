---
title: Azure HPC önbelleği oluşturma
description: Azure HPC önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 479adf9419cdd6b04e50fa479d47b56762b2bdc6
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70774689"
---
# <a name="create-an-azure-hpc-cache"></a>Azure HPC önbelleği oluşturma

Önbelleğinizi oluşturmak için Azure portal kullanın.

![En altta oluştur düğmesi ile Azure portal önbellekte genel bakış ekran görüntüsü](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Temel ayrıntıları tanımla

![Azure portal içindeki proje ayrıntıları sayfasının ekran görüntüsü](media/create-1.png)

**Proje ayrıntıları**' nda Azure HPC önbelleğini barındıracak aboneliği ve kaynak grubunu seçin. Aboneliğin [Önizleme erişim](hpc-cache-prereqs.md#azure-subscription) listesinde olduğundan emin olun.

**Hizmet Ayrıntıları**' nda, önbellek adını ve bu diğer öznitelikleri ayarlayın:

* Konum- [desteklenen bölgelerden](hpc-cache-overview.md#region-availability)birini seçin.
* Sanal ağ-mevcut bir tane seçebilirsiniz veya yeni bir sanal ağ oluşturabilirsiniz.
* Alt ağ-yalnızca Azure HPC önbelleği için kullanılacak en az 64 IP adresi (/24) olan bir alt ağ seçin veya oluşturun.

## <a name="set-cache-capacity"></a>Önbellek kapasitesini ayarla
<!-- change link in GUI -->

**Önbellek** sayfasında, Azure HPC önbelleğinizin kapasitesini ayarlamanız gerekir. Bu değer, önbelleğinizin ne kadar veri tutabildiğini ve istemci isteklerine ne kadar hızlı bir şekilde hizmet sunamayacağını belirler. Genel Önizleme süresinden sonra kapasite, önbelleğin maliyetini de etkiler.

Önbellek kapasitesi, saniye başına giriş/çıkış işlemi (ıOPS) cinsinden ölçülür. Şu iki değeri belirleyerek kapasiteyi seçin:

* Önbellek (aktarım hızı) için GB/saniye cinsinden maksimum veri aktarımı oranı
* Önbelleğe alınmış veriler için ayrılan depolama miktarı (TB)

Kullanılabilir aktarım hızı değerlerinden birini ve önbellek depolama boyutunu seçin. IOPS kapasitesi hesaplanır ve değer seçicilerin altında gösterilir.

Gerçek veri aktarımı hızının iş yüküne, ağ hızına ve depolama hedeflerinin türüne bağlı olduğunu unutmayın. Bir dosya önbellekte değilse veya eski olarak işaretlenmişse, hizmet, arka uç depolamadan getirmek için bazı aktarım hızını kullanır. Seçtiğiniz değer tüm önbelleğin en fazla aktarım hızını belirler ve bunların hepsi istemci istekleri için kullanılabilir değildir.

Önbellek depolama için Azure HPC Cache, önbellek isabet oranlarını en üst düzeye çıkarmak için hangi dosyaların önbelleğe alınacağını ve ön sürümlerini yönetir. Önbellek içeriği sürekli değerlendirilir ve dosyalar daha az sıklıkla erişildiği zaman uzun süreli depolamaya taşınır. Etkin çalışma dosyaları kümesini, meta veriler ve diğer ek yük için ek alanla rahatça tutabilecek bir önbellek depolama boyutu seçin.

![önbellek boyutlandırma sayfasının ekran görüntüsü](media/create-cache-iops.png)

## <a name="add-storage-targets"></a>Depolama hedefleri ekleme

Depolama hedefleri, önbelleğinizin içeriği için arka uç, uzun süreli depolardır.

Önbellek oluştururken depolama hedeflerini tanımlayabilirsiniz, ancak bunları daha sonra portalda önbelleğiniz sayfasının **Yapılandır** bölümünde bulunan bağlantıyla birlikte ekleyebilirsiniz.

![depolama hedefleri sayfasının ekran görüntüsü](media/create-targets.png)

Arka uç depolama sistemlerinizi tanımlamak için **depolama hedefi Ekle bağlantısına** tıklayın. Depolama alanı, Azure Blob kapsayıcıları veya şirket içi NFS sistemleri olabilir.

En fazla on farklı depolama hedefi tanımlayabilirsiniz.

Depolama hedefi ekleme hakkında adım adım yönergeler için, [depolama ekleme](hpc-cache-add-storage.md)' yi okuyun. Yordam, BLOB depolama veya NFS dışarı aktarmaları için farklıdır.

Her iki depolama türü için, arka uç depolama sisteminin (bir NFS adresi veya bir blob kapsayıcı adı) ve istemciye yönelik ad alanı yolunun nasıl bulunacağını belirtmeniz gerekir.

BLOB depolama hedefi oluştururken, [erişim denetimi rollerini ekleme](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account)bölümünde açıklandığı gibi, önbelleğin depolama hesabına erişim izinlerine sahip olduğundan emin olun. Rol yapılandırmasının başarılı olduğundan emin değilseniz, önce önbelleği oluşturun ve ardından BLOB depolama alanını daha sonra ekleyin.

NFS depolama hedefi oluştururken bir [kullanım modeli](hpc-cache-add-storage.md#choose-a-usage-model)belirtin. Kullanım modeli ayarı önbelleğin iş akışınızı iyileştirmenize yardımcı olur.

## <a name="add-resource-tags-optional"></a>Kaynak etiketleri ekleme (isteğe bağlı)

**Etiketler** sayfası, Azure HPC önbelleğinize [kaynak etiketleri](https://go.microsoft.com/fwlink/?linkid=873112) eklemenize olanak tanır. 

## <a name="finish-creating-the-cache"></a>Önbellek oluşturmayı tamamlama

Yeni önbelleği yapılandırdıktan sonra, **gözden geçir + oluştur** sekmesine tıklayın. Portal seçimlerinizi doğrular ve seçimlerinizi incelemenizi sağlar. Her şey doğruysa, **Oluştur**' a tıklayın. 

Önbellek oluşturma yaklaşık 10 dakika sürer. İlerlemeyi Azure portal Bildirimler panelinde izleyebilirsiniz. Tamamlandığında, yeni Azure HPC önbellek örneğinin bağlantısını içeren bir bildirim görüntülenir. 

Önbellek Ayrıca aboneliğinizin **kaynaklar** listesinde görünür. 

![Azure portal 'de Azure HPC önbellek örneğinin ekran görüntüsü](media/finished-hpc-cache.png)

## <a name="next-steps"></a>Sonraki adımlar

Önbelleğiniz **kaynaklar** listesinde görüntülendikten sonra, bunu istemci erişimi için bağlayabilir, çalışma kümesi verilerinizi yeni bir Azure Blob depolama hedefine taşımak veya ek veri kaynakları tanımlamak için kullanabilirsiniz.

* [Azure HPC önbelleğini bağlama](hpc-cache-mount.md)
* [Azure HPC önbelleği için verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)
* [Depolama hedefleri ekleme](hpc-cache-add-storage.md)
