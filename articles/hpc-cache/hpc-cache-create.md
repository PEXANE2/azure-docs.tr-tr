---
title: Azure HPC önbelleği oluşturma (Önizleme)
description: Azure HPC önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 09/24/2019
ms.author: v-erkell
ms.openlocfilehash: a0590c14032595bea685c69962ef27dca14d1d69
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300009"
---
# <a name="create-an-azure-hpc-cache-preview"></a>Azure HPC önbelleği oluşturma (Önizleme)

Önbelleğinizi oluşturmak için Azure portal kullanın. 

![En altta oluştur düğmesi ile Azure portal önbellekte genel bakış ekran görüntüsü](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Temel ayrıntıları tanımla

![Azure portal içindeki proje ayrıntıları sayfasının ekran görüntüsü](media/hpc-cache-create-basics.png)

**Proje ayrıntıları**' nda, önbelleği barındıracak aboneliği ve kaynak grubunu seçin. Aboneliğin [Önizleme erişim](hpc-cache-prereqs.md#azure-subscription) listesinde olduğundan emin olun.

**Hizmet Ayrıntıları**' nda, önbellek adını ve bu diğer öznitelikleri ayarlayın:

* Konum- [desteklenen bölgelerden](hpc-cache-overview.md#region-availability)birini seçin.
* Sanal ağ-mevcut bir tane seçebilirsiniz veya yeni bir sanal ağ oluşturabilirsiniz.
* Alt ağ-yalnızca bu Azure HPC önbellek örneği için kullanılacak en az 64 IP adresi (/24) olan bir alt ağ seçin veya oluşturun.

## <a name="set-cache-capacity"></a>Önbellek kapasitesini ayarla
<!-- referenced from GUI - update aka.ms link if you change this header text -->

**Önbellek** sayfasında önbelleğinizin kapasitesini ayarlamanız gerekir. Bu değer, önbelleğinizin ne kadar veri tutabildiğini ve istemci isteklerine ne kadar hızlı bir şekilde hizmet sunamayacağını belirler. 

Genel Önizleme süresinden sonra kapasite, önbelleğin maliyetini de etkiler.

Önbellek kapasitesi, saniye başına giriş/çıkış işlemi (ıOPS) cinsinden ölçülür. Şu iki değeri belirleyerek kapasiteyi seçin:

* Önbellek (aktarım hızı) için GB/saniye cinsinden maksimum veri aktarımı oranı
* Önbelleğe alınmış veriler için ayrılan depolama miktarı (TB)

Kullanılabilir aktarım hızı değerlerinden birini ve önbellek depolama boyutunu seçin. IOPS kapasitesi hesaplanır ve değer seçicilerin altında gösterilir.

Gerçek veri aktarımı hızının iş yüküne, ağ hızına ve depolama hedeflerinin türüne bağlı olduğunu unutmayın. Seçtiğiniz değer tüm önbelleğin en fazla aktarım hızını belirler ve bunların hepsi istemci istekleri için kullanılabilir değildir. Örneğin, bir istemci önbellekte zaten depolanmayan bir dosya isterse veya dosya eski olarak işaretlenmişse önbelleğiniz, arka uç depolamadan getirmek için aktarım hızını kullanır.

Azure HPC Cache, önbellek isabet oranlarını en üst düzeye çıkarmak için hangi dosyaların önbelleğe alınacağını ve ön yüklenmiş olduğunu yönetir Önbellek içeriği sürekli değerlendirilir ve dosyalar daha az sıklıkla erişildiği zaman uzun süreli depolamaya taşınır. Etkin çalışma dosyaları kümesini, meta veriler ve diğer ek yük için ek alanla rahatça tutabilecek bir önbellek depolama boyutu seçin.

![önbellek boyutlandırma sayfasının ekran görüntüsü](media/hpc-cache-create-iops.png)

## <a name="add-storage-targets"></a>Depolama hedefleri ekleme

Depolama hedefleri, önbelleğinizin içeriği için arka uç, uzun süreli depolardır.

Önbellek oluştururken depolama hedeflerini tanımlayabilirsiniz, ancak bunları daha sonra portalda önbelleğiniz sayfasının **Yapılandır** bölümünde bulunan bağlantıyla birlikte ekleyebilirsiniz.

![depolama hedefleri sayfasının ekran görüntüsü](media/hpc-cache-storage-targets-pop.png)

Arka uç depolama sistemlerinizi tanımlamak için **depolama hedefi Ekle bağlantısına** tıklayın. Depolama alanı, Azure Blob kapsayıcıları veya şirket içi NFS sistemleri olabilir.

En fazla on farklı depolama hedefi tanımlayabilirsiniz.

Depolama hedefi eklemek için adım adım yönergeler, [depolama hedefleri ekleme](hpc-cache-add-storage.md)' ye dahildir. Yordam, BLOB depolama veya NFS dışarı aktarmaları için farklıdır.

İşte bazı ipuçları:

* Her iki depolama türü için, arka uç depolama sisteminin (bir NFS adresi veya bir blob kapsayıcı adı) ve istemciye yönelik ad alanı yolunun nasıl bulunacağını belirtmeniz gerekir.

* BLOB depolama hedefi oluştururken, [erişim denetimi rollerini ekleme](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account)bölümünde açıklandığı gibi, önbelleğin depolama hesabına erişim izinlerine sahip olduğundan emin olun. Rol yapılandırmasının başarılı olduğundan emin değilseniz, önce önbelleği oluşturun ve ardından BLOB depolama alanını daha sonra ekleyin.

* NFS depolama hedefi oluştururken bir [kullanım modeli](hpc-cache-add-storage.md#choose-a-usage-model)belirtin. Kullanım modeli ayarı önbelleğin iş akışınızı iyileştirmenize yardımcı olur.

## <a name="add-resource-tags-optional"></a>Kaynak etiketleri ekleme (isteğe bağlı)

**Etiketler** sayfası, Azure HPC Cache örneğinizi [kaynak etiketleri](https://go.microsoft.com/fwlink/?linkid=873112) eklemenize olanak tanır.

## <a name="finish-creating-the-cache"></a>Önbellek oluşturmayı tamamlama

Yeni önbelleği yapılandırdıktan sonra, **gözden geçir + oluştur** sekmesine tıklayın. Portal seçimlerinizi doğrular ve seçimlerinizi incelemenizi sağlar. Her şey doğruysa, **Oluştur**' a tıklayın. 

Önbellek oluşturma yaklaşık 10 dakika sürer. İlerlemeyi Azure portal Bildirimler panelinde izleyebilirsiniz. 

!["dağıtım çalışma" ve "Bildirimler" sayfalarındaki önbellek oluşturma ekranının ekran görüntüsü](media/hpc-cache-deploy-status.png)

Oluşturma tamamlandığında, yeni Azure HPC önbellek örneğinin bağlantısını içeren bir bildirim görünür ve önbellek, aboneliğinizin **kaynaklar** listesinde görünür. 

![Azure portal 'de Azure HPC önbellek örneğinin ekran görüntüsü](media/hpc-cache-new-overview.png)

## <a name="next-steps"></a>Sonraki adımlar

Önbelleğiniz **kaynaklar** listesinde görüntülendikten sonra, bunu istemci erişimi için bağlayabilir, çalışma kümesi verilerinizi yeni bir Azure Blob depolama hedefine taşımak veya ek veri kaynakları tanımlamak için kullanabilirsiniz.

* [Azure HPC önbelleğini bağlama](hpc-cache-mount.md)
* [Azure HPC önbelleği için verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)
* [Depolama hedefleri ekleme](hpc-cache-add-storage.md)
