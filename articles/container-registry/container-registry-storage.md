---
title: Kapsayıcı görüntüsü depolaması
description: Kapsayıcı görüntülerinizin ve diğer yapılarınızın güvenlik, artıklık ve kapasite dahil Azure Container Registry nasıl depolandığı hakkında ayrıntılar.
ms.topic: article
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: 4bdffd111273e00b796e45f4e09bfac9ba6713e4
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102036019"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Azure Container Registry kapsayıcı görüntüsü depolaması

Her [temel, standart ve Premium](container-registry-skus.md) Azure Container Registry, görüntü verileri güvenliği için bekleyen şifreleme ve görüntü verileri koruması için coğrafi yedeklilik gibi gelişmiş Azure depolama özelliklerinden yararlanır. Aşağıdaki bölümlerde, Azure Container Registry (ACR) ' de görüntü depolamanın özellikleri ve sınırları açıklanır.

## <a name="encryption-at-rest"></a>Bekleyen şifreleme

Kayıt defterinizde bulunan tüm kapsayıcı görüntüleri ve diğer yapıtlar Rest 'de şifrelenir. Azure, bir görüntüyü depolamadan önce otomatik olarak şifreler ve siz veya Uygulamalarınız ve hizmetleriniz görüntüyü çektiğinizde BT 'nin şifresini çözer. İsteğe bağlı olarak, [müşteri tarafından yönetilen bir anahtarla](container-registry-customer-managed-keys.md)ek bir şifreleme katmanı uygulayın.

## <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

Azure, çoğu bölgede dağıtılan kapsayıcı kayıt defterleri için, kapsayıcı görüntülerinin ve diğer yapıtlarınızın kaybedilmesine karşı koruma sağlamaya yardımcı olması için coğrafi olarak yedekli bir depolama şeması kullanır. Azure Container Registry, kapsayıcı görüntülerinizi otomatik olarak birden çok coğrafi olarak uzak veri merkezine çoğaltarak bölgesel bir depolama hatası oluşursa kaybedilmesini önler.

> [!IMPORTANT]
> * Bölgesel bir depolama hatası oluşursa, kayıt defteri verileri yalnızca Azure desteğiyle iletişim kurarak kurtarılabilir. 
> * Brezilya Güney ve Güneydoğu Asya 'daki veri fazlalığı nedeniyle, bu bölgelerdeki Azure Container Registry veriler [yalnızca yerel coğrafi bölgede](https://azure.microsoft.com/global-infrastructure/geographies/)depolanır. Güneydoğu Asya için tüm veriler Singapur 'da depolanır. Brezilya Güney için tüm veriler Brezilya 'da depolanır. Bölgenin önemli bir olağanüstü durum nedeniyle kaybolması durumunda Microsoft Azure Container Registry verilerinizi kurtaramayacak.

## <a name="geo-replication"></a>Coğrafi çoğaltma

Daha da yüksek kullanılabilirlik güvencesi gerektiren senaryolar için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanmayı düşünün. Coğrafi çoğaltma, yalnızca bir depolama hatası değil, *Toplam* bölgesel bir hata durumunda kayıt defterinize erişimi kaybetmekten korunmaya yardımcı olur. Coğrafi çoğaltma, dağıtılmış geliştirme veya dağıtım senaryolarında daha hızlı gönderim ve çekmelerde diğer avantajlar sağlar.

## <a name="zone-redundancy"></a>Bölge yedekliliği

Esnek ve yüksek kullanılabilirliğe sahip bir Azure Kapsayıcı kayıt defteri oluşturmak için isteğe bağlı olarak Azure bölgelerinde [bölge yedekliliği](zone-redundancy.md) etkinleştirin. Premium hizmet katmanının bir özelliği olan bölge artıklığı, kayıt defterinizi her bir etkin bölgedeki en az üç ayrı bölgeye çoğaltmak için Azure [kullanılabilirlik bölgelerini](../availability-zones/az-overview.md) kullanır. Bir kayıt defterinin güvenilirliğini ve performansını geliştirmek için coğrafi çoğaltma ve bölge yedekliliği birleştirin. 

## <a name="scalable-storage"></a>Ölçeklenebilir depolama

Azure Container Registry, [kayıt defteri depolama sınırına](container-registry-skus.md#service-tier-features-and-limits)kadar, ihtiyacınız olan sayıda depo, resim, katman veya etiket oluşturmanızı sağlar. 

Yüksek sayıda depolar ve Etiketler, kayıt defterinizin performansını etkileyebilir. Kullanılmayan depoları, etiketleri ve görüntüleri kayıt defteri bakım yordamlarınızın bir parçası olarak düzenli olarak silin ve etiketlenmemiş bildirimler için isteğe bağlı olarak bir [bekletme ilkesi](container-registry-retention-policy.md) ayarlayın. Depolar, görüntüler *ve Etiketler gibi* silinen kayıt defteri kaynakları silinmeden sonra kurtarılamaz. Kayıt defteri kaynaklarını silme hakkında daha fazla bilgi için bkz. [Azure Container Registry kapsayıcı görüntülerini silme](container-registry-delete.md).

## <a name="storage-cost"></a>Depolama maliyeti

Fiyatlandırma hakkında tam Ayrıntılar için bkz. [Azure Container Registry fiyatlandırması][pricing].

## <a name="next-steps"></a>Sonraki adımlar

Temel, standart ve Premium kapsayıcı kayıt defterleri hakkında daha fazla bilgi için bkz. [Azure Container Registry hizmet katmanları](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
