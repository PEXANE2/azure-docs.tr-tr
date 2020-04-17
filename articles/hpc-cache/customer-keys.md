---
title: Azure HPC Önbelleğinde verileri şifrelemek için müşteri destekli anahtarları kullanma
description: Varsayılan Microsoft tarafından yönetilen şifreleme anahtarlarını kullanmak yerine şifreleme anahtarı erişimini denetlemek için Azure HPC Önbelleği ile Azure Key Vault nasıl kullanılır?
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: v-erkel
ms.openlocfilehash: a31979763dd1ab5d8f289deef0e30cce27bb0df4
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538875"
---
# <a name="use-customer-managed-encryption-keys-for-azure-hpc-cache"></a>Azure HPC Önbelleği için müşteri tarafından yönetilen şifreleme anahtarlarını kullanma

Azure HPC Önbelleğinde verilerinizi şifrelemek için kullanılan anahtarların sahipliğini denetlemek için Azure Key Vault'u kullanabilirsiniz. Bu makalede, önbellek veri şifrelemesi için müşteri tarafından yönetilen anahtarların nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Önbellek diskleri de dahil olmak üzere Azure'da depolanan tüm veriler, varsayılan olarak Microsoft tarafından yönetilen anahtarlar kullanılarak istirahatte şifrelenir. Verilerinizi şifrelemek için kullanılan anahtarları yönetmek istiyorsanız, yalnızca bu makaledeki adımları izlemeniz gerekir.

Bu özellik yalnızca bu Azure bölgelerinde kullanılabilir:

* Doğu ABD
* Orta Güney ABD
* Batı ABD 2

Azure HPC Önbelleği için müşteri tarafından yönetilen anahtar şifrelemesini etkinleştirmek için üç adım vardır:

1. Anahtarları depolamak için bir Azure Anahtar Kasası ayarlayın.
1. Azure HPC Önbelleği oluştururken, müşteri tarafından yönetilen anahtar şifrelemesini seçin ve kullanılacak anahtar kasasını ve anahtarı belirtin.
1. Önbellek oluşturulduktan sonra, anahtar kasasına erişme yetkisi ver.

Şifreleme, yeni oluşturulan önbellekten (adım 3) yetkilendirilene kadar tam olarak ayarlanmaz. Bunun nedeni, önbelleğin kimliğini yetkili bir kullanıcı yapmak için anahtar kasasına geçirmeniz gerektiğidir. Önbellek oluşturulana kadar kimlik yok olduğundan, önbelleği oluşturmadan önce bunu yapamazsınız.

Önbelleği oluşturduktan sonra, müşteri tarafından yönetilen anahtarlar ile Microsoft tarafından yönetilen anahtarlar arasında değişiklik yapamazsınız. Ancak, önbelleğiniz müşteri tarafından yönetilen anahtarları kullanıyorsa şifreleme anahtarını, anahtar sürümünü ve gerektiğinde anahtar kasasını [değiştirebilirsiniz.](#update-key-settings)

## <a name="understand-key-vault-and-key-requirements"></a>Anahtar kasası ve anahtar gereksinimlerini anlama

Azure HPC Önbelleği ile çalışmak için anahtar kasası ve anahtarıbu gereksinimleri karşılamalıdır.

Anahtar kasa özellikleri:

* **Abonelik** - Önbellek için kullanılan aynı aboneliği kullanın.
* **Bölge** - Anahtar kasası Azure HPC Önbelleği ile aynı bölgede olmalıdır.
* **Fiyatlandırma katmanı** - Standart katman, Azure HPC Önbelleği ile kullanım için yeterlidir.
* **Yumuşak silme** - Azure HPC Önbelleği, anahtar kasasında zaten yapılandırılmamışsa yumuşak silmeyi etkinleştirir.
* **Temizleme koruması** - Temizleme koruması etkinleştirilmelidir.
* **Erişim ilkesi** - Varsayılan ayarlar yeterlidir.
* **Ağ bağlantısı** - Azure HPC Önbelleği seçtiğiniz uç nokta ayarlarına bakılmaksızın anahtar kasasına erişebilmeli.

Temel özellikler:

* **Anahtar türü** - RSA
* **RSA anahtar boyutu** - 2048
* **Etkin** - Evet

Anahtar kasa erişim izinleri:

* Azure HPC Önbelleğini oluşturan kullanıcının Key Vault [katılımcısı rolüne](../role-based-access-control/built-in-roles.md#key-vault-contributor)eşdeğer izinleri olmalıdır. Azure Key Vault'u kurmak ve yönetmek için aynı izinler gereklidir.

  Daha fazla bilgi [için önemli bir kasaya Güvenli erişim](../key-vault/key-vault-secure-your-key-vault.md) okuyun.

## <a name="1-set-up-azure-key-vault"></a>1. Azure Anahtar Kasası'nı ayarlama

Önbelleği oluşturmadan önce bir anahtar kasası ve anahtarı ayarlayabilir veya önbellek oluşturmanın bir parçası olarak yapabilirsiniz. Bu kaynakların [yukarıda](#understand-key-vault-and-key-requirements)özetlenen gereksinimleri karşıladığından emin olun.

Önbellek oluşturma zamanında önbelleğin şifrelemesi için kullanılacak bir kasa, anahtar ve anahtar sürümünü belirtmeniz gerekir.

Ayrıntılar için [Azure Anahtar Kasası belgelerini](../key-vault/key-vault-overview.md) okuyun.

> [!NOTE]
> Azure Anahtar Kasası aynı aboneliği kullanmalı ve Azure HPC Önbelleği yle aynı bölgede olmalıdır. Bu makalenin başında listelenen desteklenen bölgelerden birini kullanın.

## <a name="2-create-the-cache-with-customer-managed-keys-enabled"></a>2. Müşteri tarafından yönetilen anahtarlar etkinken önbelleği oluşturma

Azure HPC Önbelleğinizi oluştururken şifreleme anahtarı kaynağını belirtmeniz gerekir. [Azure HPC Önbelleği Oluştur'daki](hpc-cache-create.md)yönergeleri izleyin ve Disk **şifreleme anahtarları** sayfasındaki anahtar kasasını ve anahtarı belirtin. Önbellek oluşturma sırasında yeni bir anahtar kasası ve anahtar oluşturabilirsiniz.

> [!TIP]
> Disk **şifreleme anahtarları** sayfası görünmüyorsa, önbelleğinizin desteklenen bölgelerden birinde olduğundan emin olun.

Önbelleği oluşturan [kullanıcının, Key Vault katılımcısı rolüne](../role-based-access-control/built-in-roles.md#key-vault-contributor) eşit veya daha yüksek ayrıcalıklara sahip olması gerekir.

1. Özel olarak yönetilen tuşları etkinleştirmek için düğmeyi tıklatın. Bu ayarı değiştirdikten sonra, anahtar kasa ayarları görüntülenir.

1. Anahtar seçim sayfasını açmak için **bir anahtar kasası seçin'i** tıklatın. Bu önbelleğin disklerinde verileri şifrelemek için anahtar kasasını ve anahtarını seçin veya oluşturun.

   Azure Anahtar Kasanız listede görünmüyorsa, aşağıdaki gereksinimleri kontrol edin:

   * Önbellek anahtar kasası ile aynı abonelikte mi?
   * Önbellek anahtar kasası ile aynı bölgede mi?
   * Azure portalı ile anahtar kasası arasında ağ bağlantısı var mı?

1. Kasa seçtikten sonra, kullanılabilir seçeneklerden tek tek anahtarı seçin veya yeni bir anahtar oluşturun. Anahtar 2048 bit RSA anahtarı olmalıdır.

1. Seçili anahtarın sürümünü belirtin. [Azure Anahtar Kasası belgelerinde](../key-vault/about-keys-secrets-and-certificates.md#objects-identifiers-and-versioning)sürüm hakkında daha fazla bilgi edinin.

Diğer belirtimlerle devam edin ve Azure [HPC Önbelleği Oluştur'da](hpc-cache-create.md)açıklandığı gibi önbelleği oluşturun.

## <a name="3-authorize-azure-key-vault-encryption-from-the-cache"></a>3. Azure Anahtar Kasası şifrelemesini önbellekten yetkilendirme
<!-- header is linked from create article, update if changed -->

Birkaç dakika sonra, yeni Azure HPC Önbelleği Azure portalınızda görünür. Azure Anahtar Kasanıza erişmek ve müşteri tarafından yönetilen anahtar şifrelemesini etkinleştirmek için yetkilendirmek için **Genel Bakış** sayfasına gidin. (Önbellek, "dağıtım devam ediyor" iletileri açık lanmadan önce kaynaklar listesinde görünebilir.)

Bu iki aşamalı işlem gereklidir, çünkü Azure HPC Önbellek örneğinin yetkilendirme için Azure Anahtar Kasası'na geçmesi için bir kimliğe ihtiyacı vardır. Önbellek kimliği, ilk oluşturma adımları tamamlanana kadar yok.

> [!NOTE]
> Önbelleği oluşturduktan sonra 90 dakika içinde şifrelemeyetkisi vermelisiniz. Bu adımı tamamlamazsanız, önbellek zaman dolacak ve başarısız olur. Başarısız bir önbelleğin yeniden oluşturulması gerekir, düzeltilemez.

Önbellek, anahtarı **bekleyen**durumu gösterir. Belirtilen anahtar kasasına erişmek için önbelleği yetkilendirmek için sayfanın üst kısmındaki **şifrelemeyi etkinleştir** düğmesini tıklatın.

![etkinleştir şifreleme düğmesinde vurgulama (üst satır) ve Durum: Tuş bekleniyor](media/waiting-for-key.png)

**Şifrelemeyi Etkinleştir'i** tıklatın ve ardından şifreleme anahtarını kullanmak için önbelleği yetkilendirmek için **Evet** düğmesini tıklatın. Bu eylem, anahtar kasasında yumuşak silme ve temizleme koruması (zaten etkin değilse) de sağlar.

![portaldaki önbelleğe genel bakış sayfasının ekran görüntüsü, üst kısmında kullanıcıdan evet'i tıklatarak şifrelemeyi etkinleştirmesini isteyen bir başlık mesajı](media/enable-keyvault.png)

Önbellek anahtar kasasına erişim istedikten sonra, önbelleğe alınan verileri depolayan diskleri oluşturabilir ve şifreleyebilir.

Şifrelemeyi yetkilendirdikten sonra, Azure HPC Önbelleği, şifrelenmiş diskleri ve ilgili altyapıyı oluşturmak için birkaç dakika daha kurulumdan geçer.

## <a name="update-key-settings"></a>Tuş ayarlarını güncelleştir

Önbelleğiniziçin anahtar kasasını, anahtarı nı veya anahtar sürümünü Azure portalından değiştirebilirsiniz. **Müşteri anahtarı ayarları** sayfasını açmak için önbelleğin **Şifreleme** ayarları bağlantısını tıklatın. (Önbelleği müşteri tarafından yönetilen anahtarlar ve sistem tarafından yönetilen anahtarlar arasında değiştiremezsiniz.)

![Azure portalındaki önbellek sayfasından Ayarlar > Şifreleme'ye tıklayarak ulaşılan "Müşteri anahtarları ayarları" sayfasının ekran görüntüsü](media/change-key-click.png)

Anahtar bağlantısını **değiştir'i** tıklatın ve ardından anahtar seçiciyi açmak için **anahtar kasasını, anahtarı veya sürümü değiştir'i** tıklatın.

![anahtar kasası, anahtar ve sürümü seçmek için üç açılır seçiciyle "Azure Key Vault'tan anahtar seçin" sayfasının ekran görüntüsü](media/select-new-key.png)

Listede bu önbellekle aynı abonelikteki ve aynı bölgedeki anahtar kasaları gösterilir.

Yeni şifreleme anahtarı değerlerini seçtikten sonra **Seç'i**tıklatın. Yeni değerlerle birlikte bir onay sayfası görüntülenir. Seçimi sonuçlandırmak için **Kaydet'i** tıklatın.

![sol üstte Kaydet düğmesi ile onay sayfasının ekran görüntüsü](media/save-key-settings.png)

## <a name="read-more-about-customer-managed-keys-in-azure"></a>Azure'da müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi edinin

Bu makaleler, Azure'daki verileri şifrelemek için Azure Key Vault'u ve müşteri tarafından yönetilen anahtarları kullanma hakkında daha fazla bilgi veaçıklar:

* [Azure depolama şifrelemeye genel bakış](../storage/common/storage-service-encryption.md)
* [Müşteri tarafından yönetilen anahtarlarla disk şifreleme](../virtual-machines/linux/disk-encryption.md#customer-managed-keys) - Azure Anahtar Kasası ve yönetilen diskleri kullanmak için belgeler, Azure HPC Önbelleği ile kullanılan işleme benzer

## <a name="next-steps"></a>Sonraki adımlar

Azure HPC Önbelleği'ni ve yetkili Key Vault tabanlı şifrelemeyi oluşturduktan sonra, veri kaynaklarına erişim sağlayarak önbelleğinizi ayarlamaya devam edin.

* [Depolama hedefleri ekleme](hpc-cache-add-storage.md)
