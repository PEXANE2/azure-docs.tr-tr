---
title: Azure HPC Önbelleğine depolama alanı ekleme
description: Azure HPC Önbelleğinizin şirket içi NFS sisteminizi veya Azure Blob kapsayıcılarınızı uzun süreli dosya depolama için kullanabilmesi için depolama hedeflerini nasıl tanımlarsınız?
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: rohogue
ms.openlocfilehash: a68bf06bad995f71bedf6a5bdedcb676737a8c61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271895"
---
# <a name="add-storage-targets"></a>Depolama hedefleri ekleme

*Depolama hedefleri,* Azure HPC Önbelleği örneği aracılığıyla erişilen dosyalar için arka uç depolama alanıdır. NFS depolama (şirket içi donanım sistemi gibi) ekleyebilir veya verileri Azure Blob'da depolayabilirsiniz.

Bir önbellek için en fazla on farklı depolama hedefi tanımlayabilirsiniz. Önbellek, tüm depolama hedeflerini tek bir toplu ad alanında sunar.

Depolama dışa aktarımlarına önbelleğinizin sanal ağından erişilmesi gerektiğini unutmayın. Şirket içi donanım depolama için, NFS depolama erişimi için ana bilgisayar adlarını çözebilecek bir DNS sunucusu ayarlamanız gerekebilir. [DNS erişiminde](hpc-cache-prereqs.md#dns-access)daha fazla bilgi edinin.

Önbelleğinizi oluşturduktan sonra depolama hedefleri ekleyin. Yordam, Azure Blob depolama alanı mı yoksa NFS dışa aktarma ekleyip eklemediğinize bağlı olarak biraz farklıdır. Her biri için ayrıntılar aşağıdadır.

## <a name="open-the-storage-targets-page"></a>Depolama hedefleri sayfasını açma

Azure portalından önbellek örneğini açın ve sol kenar çubuğundaki **Depolama hedeflerini** tıklatın. Depolama hedefleri sayfası varolan tüm hedefleri listeler ve yeni bir hedef eklemek için bir bağlantı verir.

![depolama hedeflerinin ekran görüntüsü kenar çubuğunda, kategori başlıkları Ayarlar ve İzleme arasında olan Yapılandırma başlığı altında](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Yeni bir Azure Blob depolama hedefi ekleme

Yeni bir Blob depolama hedefinin boş bir Blob kapsayıcısına veya Azure HPC Önbellek bulut dosyası sistem biçiminde ki verilerle dolu bir kapsayıcıya ihtiyacı vardır. [Verileri Azure Blob depolamasına taşı'nda](hpc-cache-ingest.md)Blob kapsayıcısını önceden yükleme hakkında daha fazla bilgi edinin.

Eklemeden önce bu sayfadan yeni bir kapsayıcı oluşturabilirsiniz.

Azure Blob kapsayıcısı tanımlamak için bu bilgileri girin.

![yeni bir Azure Blob depolama hedefi için bilgilerle doldurulan depolama alanı hedef sayfasının ekran görüntüsü](media/hpc-cache-add-blob.png)

* **Depolama hedef adı** - Azure HPC Önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.
* **Hedef türü** - **Blob**seçin.
* **Depolama hesabı** - Kullanmak istediğiniz hesabı seçin.

  Erişim rollerini ekle'de açıklandığı gibi depolama hesabına erişmek için önbellek [örneğini yetkilendirmeniz](#add-the-access-control-roles-to-your-account)gerekir.

  Kullanabileceğiniz depolama hesabı türü hakkında bilgi için [Blob depolama gereksinimleri'ni](hpc-cache-prereqs.md#blob-storage-requirements)okuyun.

* **Depolama kapsayıcısı** - Bu hedef için Blob kapsayıcısını seçin veya **yeni oluştur'u**tıklatın.

  ![yeni kapsayıcı için ad ve erişim düzeyini (özel) belirtmek için iletişim ekranı](media/add-blob-new-container.png)

* **Sanal ad alanı yolu** - Bu depolama hedefi için istemciye bakan dosya yolunu ayarlayın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplu ad alanını yapılandır'ı](hpc-cache-namespace.md) okuyun.

Tamamlandığında, depolama hedefini eklemek için **Tamam'ı** tıklatın.

> [!NOTE]
> Depolama hesabı güvenlik duvarınız yalnızca "seçili ağlarla" erişimi kısıtlamak üzere ayarlanmışsa, [Blob depolama hesabı güvenlik duvarı ayarlarında belgelenen](hpc-cache-blob-firewall-fix.md)geçici geçici geçici çözüm'ü kullanın.

### <a name="add-the-access-control-roles-to-your-account"></a>Hesabınıza erişim denetimi rollerini ekleme

Azure HPC Önbelleği, Azure Blob depolama hedefleri için depolama hesabınıza erişmek için önbellek hizmetini yetkilendirmek için [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index) kullanır.

Depolama hesabı sahibi, kullanıcı "HPC Önbellek Kaynak Sağlayıcısı" için [Depolama Hesabı Katkıda Bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) ve Depolama [Blob Veri Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) rollerini açıkça eklemelidir.

Bunu önceden veya Blob depolama hedefi eklediğiniz sayfadaki bir bağlantıyı tıklatarak yapabilirsiniz. Rol ayarlarının Azure ortamında yayılmasının beş dakika kadar sürebileceğini unutmayın, bu nedenle bir depolama hedefi oluşturmadan önce rolleri ekledikten sonra birkaç dakika beklemeniz gerekir.

RBAC rollerini eklemek için adımlar:

1. Depolama hesabı için **Access denetimi (IAM)** sayfasını açın. (Depolama ekle **hedef** sayfasındaki bağlantı, seçilen hesap için bu sayfayı otomatik olarak açar.)

1. Sayfanın **+** üst kısmındaki neleri tıklatın ve **rol ataması ekle'yi**seçin.

1. Listeden "Depolama Hesabı Katılımcısı" rolünü seçin.

1. Alana **Erişim Ata'da,** seçili varsayılan değeri ("Azure AD kullanıcısı, grup veya hizmet ilkesi") bırakın.  

1. **Select** alanında "hpc" araması yapın.  Bu dize, "HPC Önbellek Kaynak Sağlayıcısı" adlı bir hizmet ilkesiyle eşleşmelidir. Seçmek için bu anapara'yı tıklatın.

   > [!NOTE]
   > "hpc" için bir arama işe yaramazsa, bunun yerine "storagecache" dizesini kullanmayı deneyin. Önizlemelere katılan kullanıcıların (GA'dan önce) hizmet sorumlusu için eski adı kullanmaları gerekebilir.

1. Alttaki **Kaydet** düğmesini tıklatın.

1. "Depolama Blob Veri Katkıda Bulunan" rolünü atamak için bu işlemi yineleyin.  

![rol atama GUI ekle ekran görüntüsü](media/hpc-cache-add-role.png)

## <a name="add-a-new-nfs-storage-target"></a>Yeni bir NFS depolama hedefi ekleme

Bir NFS depolama hedefi, Blob depolama hedefinden daha fazla alana sahiptir. Bu alanlar, depolama dışa nasıl erişeceğini ve verilerini verimli bir şekilde nasıl önbelleceğini belirtir. Ayrıca, NFS depolama hedefi, NFS ana bilgisayarının birden fazla dış agünüş leri varsa birden çok ad alanı yolu oluşturmanıza olanak tanır.

![NFS hedefi tanımlanmış depolama hedef sayfası ekle ekran görüntüsü](media/hpc-cache-add-nfs-target.png)

NFS destekli bir depolama hedefi için bu bilgileri sağlayın:

* **Depolama hedef adı** - Azure HPC Önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.

* **Hedef türü** - **NFS'yi**seçin.

* **Hostname** - NFS depolama sisteminiziçin IP adresini veya tam nitelikli alan adını girin. (Yalnızca önbelleğiniz adı çözebilecek bir DNS sunucusuna erişebiliyorsa bir etki alanı adı kullanın.)

* **Kullanım modeli** - Aşağıda [bir kullanım modeli seçin'de](#choose-a-usage-model)açıklanan iş akışınıza göre veri önbelleğe alma profillerinden birini seçin.

### <a name="nfs-namespace-paths"></a>NFS ad alanı yolları

Her yol aynı depolama sisteminde farklı bir dışa aktarma veya alt dizini temsil ettiği sürece, bir NFS depolama hedefibirden çok sanal yola sahip olabilir.

Tüm yolları tek bir depolama hedefinden oluşturun.

İstediğiniz zaman depolama hedefine [ad alanı yolları ekleyebilir ve edinebilirsiniz.](hpc-cache-edit-storage.md)

Her ad alanı yolu için bu değerleri doldurun:

* **Sanal ad alanı yolu** - Bu depolama hedefi için istemciye bakan dosya yolunu ayarlayın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplu ad alanını yapılandır'ı](hpc-cache-namespace.md) okuyun.

<!--  The virtual path should start with a slash ``/``. -->

* **NFS dışa aktarma yolu** - NFS dışa aktarma yolunu girin.

* **Subdirectory yolu** - Dışa aktarmanın belirli bir alt dizini takmak istiyorsanız, buraya girin. Değilse, bu alanı boş bırakın.

Tamamlandığında, depolama hedefini eklemek için **Tamam'ı** tıklatın.

### <a name="choose-a-usage-model"></a>Bir kullanım modeli seçin
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Bir NFS depolama sistemine işaret eden bir depolama hedefi oluşturduğunuzda, bu hedef için *kullanım modelini* seçmeniz gerekir. Bu model, verilerinizin nasıl önbelleğe alınır belirler.

Üç seçenek vardır:

* **Ağır ve seyrek yazmaları okuyun** - Statik veya nadiren değiştirilen dosyalara okuma erişimini hızlandırmak istiyorsanız bu seçeneği kullanın.

  Bu seçenek, istemcilerin okuduğu dosyaları önbelleğe alın, ancak geçer yazmaları hemen arka uç depolama alanına geçer. Önbellekte depolanan dosyalar hiçbir zaman NFS depolama birimindeki dosyalarla karşılaştırılmayız.

  Bir dosyanın önbelleğe yazmadan doğrudan depolama sisteminde değiştirilme riski varsa bu seçeneği kullanmayın. Bu durumda, dosyanın önbelleğe alınmış sürümü hiçbir zaman arka uçtaki değişikliklerle güncelleştirilmez ve veri kümesi tutarsız hale gelebilir.

* **%15'ten fazla yazar** - Bu seçenek hem okuma hem de yazma performansını hızlandırUr. Bu seçeneği kullanırken, tüm istemcilerin arka uç depolama alanını doğrudan monte etmek yerine Azure HPC Önbelleği aracılığıyla dosyalara erişmesi gerekir. Önbelleğe alınan dosyaların arka uçta depolanamayan son değişiklikleri olur.

  Bu kullanım modelinde, önbellekteki dosyalar arka uç depolamadaki dosyalarla karşılaştırılmez. Dosyanın önbelleğe alınmış sürümünün daha geçerli olduğu varsayılır. Önbellekteki değiştirilmiş bir dosya, ek değişiklik olmadan yalnızca bir saat boyunca önbellekte kaldıktan sonra arka uç depolama sistemine yazılır.

* **İstemciler önbelleği atlayarak NFS hedefine yazın** - İş akışınızdaki herhangi bir istemci önbelleğe yazmadan verileri doğrudan depolama sistemine yazarsa bu seçeneği belirleyin. İstenilen dosyaların istediği önbelleğe alınır, ancak istemciden gelen dosyalarda yapılan değişiklikler hemen arka uç depolama sistemine aktarılır.

  Bu kullanım modeliyle, önbellekteki dosyalar güncelleştirmeler için arka uç sürümlerine karşı sık sık denetlenir. Bu doğrulama, veri tutarlılığı korunurken dosyaların önbellek dışında değiştirilmesine olanak tanır.

Bu tablo, kullanım modeli farklılıklarını özetley:

| Kullanım modeli | Önbelleğe alma modu | Arka uç doğrulama | Maksimum geri yazma gecikmesi |
| ---- | ---- | ---- | ---- |
| Ağır, seyrek yazmaları okuyun | Okuma | Hiçbir zaman | None |
| %15'ten büyük yazıyor | Okuma/yazma | Hiçbir zaman | 1 saat |
| İstemciler önbelleği atlatır | Okuma | 30 saniye | None |

## <a name="next-steps"></a>Sonraki adımlar

Depolama hedefleri oluşturduktan sonra şu görevlerden birini göz önünde bulundurun:

* [Azure HPC Önbelleğini Dağın](hpc-cache-mount.md)
* [Verileri Azure Blob depolama alanına taşıma](hpc-cache-ingest.md)

Herhangi bir ayarı güncelleştirmeniz gerekiyorsa, [bir depolama hedefini değiştirebilirsiniz.](hpc-cache-edit-storage.md)
