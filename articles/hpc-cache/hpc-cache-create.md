---
title: Azure HPC önbelleği oluşturma
description: Azure HPC önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 04/23/2020
ms.author: v-erkel
ms.openlocfilehash: 4ff31ca6a171beece1672802367f08768676efbc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195018"
---
# <a name="create-an-azure-hpc-cache"></a>Azure HPC önbelleği oluşturma

Önbelleğinizi oluşturmak için Azure portal kullanın.

![En altta oluştur düğmesi ile Azure portal önbellekte genel bakış ekran görüntüsü](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Temel ayrıntıları tanımla

![Azure portal içindeki proje ayrıntıları sayfasının ekran görüntüsü](media/hpc-cache-create-basics.png)

**Proje ayrıntıları**' nda, önbelleği barındıracak aboneliği ve kaynak grubunu seçin. Aboneliğin [erişim](hpc-cache-prereqs.md#azure-subscription) listesinde olduğundan emin olun.

**Hizmet Ayrıntıları**' nda, önbellek adını ve bu diğer öznitelikleri ayarlayın:

* Konum- [desteklenen bölgelerden](hpc-cache-overview.md#region-availability)birini seçin.
* Sanal ağ-mevcut bir tane seçebilirsiniz veya yeni bir sanal ağ oluşturabilirsiniz.
* Alt ağ-en az 64 IP adresi olan bir alt ağ seçin veya oluşturun (/24). Bu alt ağ yalnızca bu Azure HPC önbellek örneği için kullanılmalıdır.

## <a name="set-cache-capacity"></a>Önbellek kapasitesini ayarla
<!-- referenced from GUI - update aka.ms link if you change this header text -->

**Önbellek** sayfasında önbelleğinizin kapasitesini ayarlamanız gerekir. Burada ayarlanan değerler, önbelleğinizin ne kadar veri tutabildiğini ve ne kadar hızlı istemci isteklerine hizmet kullanabileceğini belirlemektir.

Kapasite ayrıca önbelleğin maliyetini de etkiler.

Şu iki değeri belirleyerek kapasiteyi seçin:

* Önbellek (aktarım hızı) için GB/saniye cinsinden maksimum veri aktarımı oranı
* Önbelleğe alınmış veriler için ayrılan depolama miktarı (TB)

Kullanılabilir aktarım hızı değerlerinden birini ve önbellek depolama boyutunu seçin.

Gerçek veri aktarımı hızının iş yüküne, ağ hızına ve depolama hedeflerinin türüne bağlı olduğunu unutmayın. Seçtiğiniz değerler tüm önbellek sistemi için en fazla aktarım hızını ayarlar, ancak bazıları ek yük görevleri için kullanılır. Örneğin, bir istemci önbellekte zaten depolanmayan bir dosya isterse veya dosya eski olarak işaretlenmişse önbelleğiniz, arka uç depolamadan getirmek için aktarım hızını kullanır.

Azure HPC Cache, önbellek isabet oranlarını en üst düzeye çıkarmak için hangi dosyaların önbelleğe alınacağını ve ön yüklenmiş olduğunu yönetir Önbellek içerikleri sürekli değerlendirilir ve dosyalar daha az sıklıkta erişildiğinde uzun süreli depolamaya taşınır. Etkin çalışma dosyaları kümesini rahatça tutabilecek bir önbellek depolama boyutu, ayrıca meta veriler ve diğer ek yük için ek alan seçin.

![önbellek boyutlandırma sayfasının ekran görüntüsü](media/hpc-cache-create-capacity.png)

## <a name="enable-azure-key-vault-encryption-optional"></a>Azure Key Vault şifrelemeyi etkinleştir (isteğe bağlı)

Önbelleğiniz, müşteri tarafından yönetilen şifreleme anahtarlarını destekleyen bir bölgedeyse, **önbellek** ve **Etiketler** sekmeleri arasında **disk şifreleme anahtarları** sayfası görüntülenir. Yayın zamanında, bu seçenek Doğu ABD, Orta Güney ABD ve Batı ABD 2 desteklenir.

Önbellek depolamayla kullanılan şifreleme anahtarlarını yönetmek istiyorsanız **disk şifreleme anahtarları** sayfasında Azure Key Vault bilgilerinizi sağlayın. Anahtar Kasası, önbellek ile aynı bölgede ve aynı abonelikte olmalıdır.

Müşteri tarafından yönetilen anahtarlar gerekmiyorsa, bu bölümü atlayabilirsiniz. Azure, verileri varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifreler. Daha fazla bilgi edinmek için [Azure depolama şifrelemesini](../storage/common/storage-service-encryption.md) okuyun.

> [!NOTE]
>
> * Önbelleği oluşturduktan sonra Microsoft tarafından yönetilen anahtarlar ve müşteri tarafından yönetilen anahtarlar arasında geçiş yapılamaz.
> * Önbellek oluşturulduktan sonra anahtar kasasına erişmek için yetkilendirmelisiniz. Şifrelemeyi açmak için önbelleğin **genel bakış** sayfasındaki **şifrelemeyi etkinleştir** düğmesine tıklayın. Bu adımı, önbelleğin oluşturulması 90 dakika içinde gerçekleştirin.
> * Önbellek diskleri bu yetkilendirmeden sonra oluşturulur. Bu, ilk önbellek oluşturma zamanının kısa olduğu, ancak erişim yetkilendirdikten sonra önbelleğin on dakika veya daha fazlası için kullanılmak üzere hazırlanmayacağı anlamına gelir.

Müşteri tarafından yönetilen anahtar şifreleme süreci hakkında tam bir açıklama için, [Azure HPC Cache için müşteri tarafından yönetilen şifreleme anahtarlarını kullanın](customer-keys.md)makalesini okuyun.

!["müşteri tarafından yönetilen" seçiliyken ve Anahtar Kasası alanlarının gösterildiği şifreleme anahtarları sayfasının ekran görüntüsü](media/create-encryption.png)

Müşteri tarafından yönetilen anahtar şifrelemesini seçmek için **Müşteri** tarafından yönetilen ' ı seçin. Anahtar Kasası belirtim alanları görüntülenir. Kullanılacak Azure Key Vault seçin, ardından bu önbellekte kullanılacak anahtarı ve sürümü seçin. Anahtar, 2048 bitlik bir RSA anahtarı olmalıdır. Bu sayfadan yeni bir Anahtar Kasası, anahtar veya anahtar sürümü oluşturabilirsiniz.

Önbelleği oluşturduktan sonra Anahtar Kasası hizmetini kullanmak için onu yetkilendirmelisiniz. Ayrıntılar için [önbellekten Azure Key Vault şifreleme kimliğini](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache) okuyun.

## <a name="add-resource-tags-optional"></a>Kaynak etiketleri ekleme (isteğe bağlı)

**Etiketler** sayfası, Azure HPC Cache örneğinizi [kaynak etiketleri](https://go.microsoft.com/fwlink/?linkid=873112) eklemenize olanak tanır.

## <a name="finish-creating-the-cache"></a>Önbellek oluşturmayı tamamlama

Yeni önbelleği yapılandırdıktan sonra, **gözden geçir + oluştur** sekmesine tıklayın. Portal seçimlerinizi doğrular ve seçimlerinizi incelemenizi sağlar. Her şey doğruysa, **Oluştur**' a tıklayın.

Önbellek oluşturma yaklaşık 10 dakika sürer. İlerlemeyi Azure portal Bildirimler panelinde izleyebilirsiniz.

!["dağıtım çalışma" ve "Bildirimler" sayfalarındaki önbellek oluşturma ekranının ekran görüntüsü](media/hpc-cache-deploy-status.png)

Oluşturma tamamlandığında, yeni Azure HPC önbellek örneğinin bağlantısını içeren bir bildirim görünür ve önbellek, aboneliğinizin **kaynaklar** listesinde görünür.

![Azure portal 'de Azure HPC önbellek örneğinin ekran görüntüsü](media/hpc-cache-new-overview.png)

> [!NOTE]
> Önbelleğiniz müşteri tarafından yönetilen şifreleme anahtarlarını kullanıyorsa, dağıtım durumu tamamlanmadan önce önbellek kaynaklar listesinde görünebilir. Önbelleğin durumu **anahtar beklerken** , anahtar kasasını kullanmak için [bunu yetkilendirebilirsiniz](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache) .

## <a name="next-steps"></a>Sonraki adımlar

Önbelleğiniz **kaynaklar** listesinde görüntülendikten sonra bir sonraki adıma geçebilirsiniz.

* Veri kaynaklarınıza önbelleğinizi erişiminizi sağlamak için [depolama hedeflerini tanımlayın](hpc-cache-add-storage.md) .
* Müşteri tarafından yönetilen şifreleme anahtarları kullanıyorsanız, önbellek kurulumunuzu gerçekleştirmek için önbelleğin genel bakış sayfasından [Azure Key Vault şifrelemeyi yetkilendirmeniz](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache) gerekir. Depolama ekleyebilmek için önce bu adımı yapmanız gerekir. Ayrıntılar için [müşteri tarafından yönetilen şifreleme anahtarlarını kullanın](customer-keys.md) ' i okuyun.
