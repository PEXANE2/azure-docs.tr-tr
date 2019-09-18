---
title: Azure HPC önbelleğine depolama ekleme
description: Azure HPC önbelleğinizin, uzun süreli dosya depolaması için şirket içi NFS sisteminizi veya Azure Blob kapsayıcılarını kullanabilmesi için depolama hedeflerini tanımlama
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: ca8e13e322c3e192b697248f1252b65f6cbeda7f
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71037205"
---
# <a name="add-storage"></a>Depolama ekleme

*Depolama hedefleri* , BIR Azure HPC Cache örneğinden erişilen dosyalar için arka uç depolardır. Şirket içi donanım sistemi gibi NFS depolama alanı ekleyebilir veya verileri Azure Blob 'a kaydedebilirsiniz.

Tek bir önbellek için en fazla on farklı depolama hedefi tanımlayabilirsiniz. Önbellek, tüm depolama hedeflerini toplanmış bir ad alanında gösterir.

Depolama dışarı aktarımlarının, önbelleğinizin sanal ağından erişilebilir olması gerektiğini unutmayın. Şirket içi donanım depolaması için NFS depolama erişimi için konak adlarını çözebilen bir DNS sunucusu ayarlamanız gerekebilir. [DNS erişiminde](hpc-cache-prereqs.md#dns-access)daha fazla bilgi edinin.

Azure HPC önbelleğinizi oluştururken veya daha sonra depolama hedefleri ekleyebilirsiniz. Yordam, Azure Blob depolama veya NFS dışarı aktarma eklediğinize bağlı olarak biraz farklılık gösterebilir. Her biri için Ayrıntılar aşağıda verilmiştir.

## <a name="add-storage-targets-while-creating-the-cache"></a>Önbellek oluştururken depolama hedefleri ekleme

Önbellek örneğini oluşturduğunuz anda depolamayı tanımlamak için önbellek oluşturma Sihirbazı 'nın **depolama hedefleri** sekmesini kullanın.

![depolama hedefleri sayfasının ekran görüntüsü](media/hpc-cache-storage-targets-pop.png)

Depolama alanı eklemek için **depolama hedefi Ekle** bağlantısına tıklayın.

## <a name="add-storage-targets-from-the-cache"></a>Önbellekten depolama hedefleri ekleyin

Azure portal, önbellek örneğinizi açın ve sol kenar çubuğundaki **depolama hedefleri** ' ne tıklayın. Depolama hedefi sayfası tüm mevcut hedefleri listeler ve yeni bir bağlantı eklemek için bir bağlantı sağlar.

![Kategori başlıkları ayarları ve Izleme arasındaki başlık yapılandırması altında bulunan, alt çubukta depolama hedefleri bağlantısının ekran görüntüsü](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Yeni bir Azure Blob depolama hedefi ekleme

Yeni bir BLOB depolama hedefi boş bir blob kapsayıcısına veya Azure HPC Cache Cloud FileSystem biçimindeki verilerle doldurulmuş bir kapsayıcıya ihtiyaç duyuyor. [Verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)bölümünde bir blob kapsayıcısını önceden yükleme hakkında daha fazla bilgi edinin.

Bir Azure Blob kapsayıcısı tanımlamak için bu bilgileri girin.

![Yeni bir Azure Blob depolama hedefi için bilgilerle doldurulmuş depolama hedefi ekleme sayfasının ekran görüntüsü](media/hpc-cache-add-blob.png)

* **Depolama hedefi adı** -Azure HPC önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.
* **Hedef türü** - **BLOB**seçin.
* **Depolama hesabı** -başvurulacak kapsayıcıyı içeren hesabı seçin.

  [Erişim rolleri ekleme](#add-the-access-control-roles-to-your-account)bölümünde açıklandığı gibi, depolama hesabına erişmek için önbellek örneğini yetkilendirmeniz gerekecektir.
* **Depolama kapsayıcısı** -bu hedefin blob kapsayıcısını seçin.

* **Sanal ad alanı yolu** -bu depolama hedefi için istemciye yönelik FilePath ayarlayın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanını Yapılandır](hpc-cache-namespace.md) makalesini okuyun.

İşiniz bittiğinde, depolama hedefini eklemek için **Tamam** ' ı tıklatın.

### <a name="add-the-access-control-roles-to-your-account"></a>Erişim denetimi rollerini hesabınıza ekleyin

Azure HPC önbelleği, önbellek uygulamasının Azure Blob depolama hedeflerine yönelik depolama hesabınıza erişmesini yetkilendirmek için [rol tabanlı erişim denetimi 'ni (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index) kullanır.

Depolama hesabı sahibi, "StorageCache kaynak sağlayıcısı" kullanıcısı için, rol [depolama hesabı katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) ve [Depolama Blobu veri katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) ' nı açıkça eklemesi gerekir.

Bunu zaman içinde yapabilir veya bir BLOB depolama hedefi eklediğiniz sayfada bir bağlantıya tıklayarak yapabilirsiniz.

RBAC rolleri ekleme adımları:

1. Depolama hesabı için **erişim denetimi (IAM)** sayfasını açın. ( **Depolama hedefi Ekle** sayfasındaki bağlantı, seçili hesap için otomatik olarak bu sayfayı açar.)

1. Sayfanın üst kısmındaki öğesine tıklayın ve **rol ataması Ekle**' yi seçin. **+**

1. Listeden "depolama hesabı katılımcısı" rolünü seçin.

1. **Erişim ata** alanına, varsayılan değeri seçili bırakın ("Azure AD kullanıcısı, Grup veya hizmet sorumlusu").  

1. **Seç** alanında "storagecache" ifadesini arayın.  Bu dize, "HPC Cache Resource Provider" adlı bir güvenlik sorumlusu ile eşleşmelidir. Bu sorumluyu seçmek için tıklayın.

1. Rol atamasını depolama hesabına eklemek için **Kaydet** düğmesine tıklayın.

1. "Depolama Blobu verileri katılımcısı" rolünü atamak için bu işlemi tekrarlayın.  

![rol ataması Ekle GUI ekran görüntüsü](media/hpc-cache-add-role.png)

## <a name="add-a-new-nfs-storage-target"></a>Yeni bir NFS depolama hedefi ekleme

NFS depolama hedefi, depolama dışarı aktarmaya nasıl ulaşılmayı ve verilerin nasıl etkili bir şekilde önbelleğe alınacağını belirtmek için bazı ek alanlara sahiptir. Ayrıca, birden fazla dışa aktarma işlemi varsa, bir NFS konağından birden çok ad alanı yolu oluşturabilirsiniz.

![NFS hedefi tanımlı depolama hedefi ekleme sayfasının ekran görüntüsü](media/hpc-cache-add-nfs-target.png)

NFS ile desteklenen bir depolama hedefi için şu bilgileri sağlayın:

* **Depolama hedefi adı** -Azure HPC önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.

* **Hedef türü** - **NFS**'yi seçin.

* **Ana bilgisayar** adı-NFS depolama sisteminizin IP adresini veya tam etki alanı adını girin. (Yalnızca önbelleğinizin adı çözebilecek bir DNS sunucusuna erişimi varsa, bir etki alanı adı kullanın.)

* **Kullanım modeli** - [aşağıda bir kullanım modeli seçin](#choose-a-usage-model)bölümünde açıklanan iş akışınızı temel alan veri önbelleğe alma profillerinden birini seçin.

Aynı NFS depolama sisteminde farklı dışarı aktarmaları temsil etmek için birden çok ad alanı yolu oluşturabilirsiniz, ancak bunları bir depolama hedefinden oluşturmanız gerekir.

Her dışarı aktarma için şu değerleri girin:

* **Sanal ad alanı yolu** -bu depolama hedefi için istemciye yönelik FilePath ayarlayın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanını Yapılandır](hpc-cache-namespace.md) makalesini okuyun.

<!--  The virtual path should start with a slash ``/``. -->

* **NFS dışarı aktarma yolu** -NFS dışarı aktarmanın yolunu girin.

* **Alt dizin yolu** -dışarı aktarmanın belirli bir alt dizinini bağlamak istiyorsanız buraya girin. Aksi takdirde, bu alanı boş bırakın. 

İşiniz bittiğinde, depolama hedefini eklemek için **Tamam** ' ı tıklatın.

### <a name="choose-a-usage-model"></a>Kullanım modeli seçin 
<!-- link in GUI to this heading -->

Bir NFS depolama sistemine işaret eden bir depolama hedefi oluşturduğunuzda, bu hedefin *kullanım modelini* seçmeniz gerekir. Bu model, verilerinizin nasıl önbelleğe alınacağını belirler.

* Yoğun okuma-veri okuma erişimini hızlandırmak için genellikle önbelleği kullanıyorsanız bu seçeneği belirleyin. 

* Okuma/yazma-istemciler, okumak ve yazmak için önbellek kullanıyorsa bu seçeneği belirleyin.

* İstemcileri önbelleği atlar-istemcileriniz, önce önbelleğe yazmadan verileri doğrudan depolama sistemine yazarlarınız bu seçeneği belirleyin.

## <a name="next-steps"></a>Sonraki adımlar

Depolama hedefleri oluşturduktan sonra şu görevlerden birini göz önünde bulundurun:

* [Azure HPC önbelleğini bağlama](hpc-cache-mount.md)
* [Verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)