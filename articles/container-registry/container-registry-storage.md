---
title: Kapsayıcı görüntüsü depolaması
description: Docker kapsayıcı görüntülerinizin güvenlik, artıklık ve kapasite dahil olmak üzere Azure Container Registry nasıl depolandığı hakkında ayrıntılar.
ms.topic: article
ms.date: 06/18/2020
ms.openlocfilehash: d51014e9e0769091aba42682cce3a6a01cfa19de
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85214069"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Azure Container Registry kapsayıcı görüntüsü depolaması

Her [temel, standart ve Premium](container-registry-skus.md) Azure Container Registry, görüntü verileri güvenliği için bekleyen şifreleme ve görüntü verileri koruması için coğrafi yedeklilik gibi gelişmiş Azure depolama özelliklerinden yararlanır. Aşağıdaki bölümlerde, Azure Container Registry (ACR) ' de görüntü depolamanın özellikleri ve sınırları açıklanır.

## <a name="encryption-at-rest"></a>Bekleyen şifreleme

Kayıt defterinizde bulunan tüm kapsayıcı görüntüleri bekleyen olarak şifrelenir. Azure, bir görüntüyü depolamadan önce otomatik olarak şifreler ve siz veya Uygulamalarınız ve hizmetleriniz görüntüyü çektiğinizde BT 'nin şifresini çözer. İsteğe bağlı olarak, [müşteri tarafından yönetilen bir anahtarla](container-registry-customer-managed-keys.md)ek bir şifreleme katmanı uygulayın.

## <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

Azure, kapsayıcı görüntülerinin kaybedilmesine karşı koruma sağlamak için coğrafi olarak yedekli bir depolama şeması kullanır. Azure Container Registry, kapsayıcı görüntülerinizi otomatik olarak birden çok coğrafi olarak uzak veri merkezine çoğaltarak bölgesel bir depolama arızası durumunda kaybedilmesini önler.

## <a name="geo-replication"></a>Coğrafi çoğaltma

Daha da yüksek kullanılabilirlik güvencesi gerektiren senaryolar için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanmayı düşünün. Coğrafi çoğaltma, yalnızca bir depolama hatası değil, *Toplam* bölgesel bir hata durumunda kayıt defterinize erişimi kaybetmekten korunmaya yardımcı olur. Coğrafi çoğaltma, dağıtılmış geliştirme veya dağıtım senaryolarında daha hızlı gönderim ve çekmelerde diğer avantajlar sağlar.

## <a name="scalable-storage"></a>Ölçeklenebilir depolama

Azure Container Registry, [kayıt defteri depolama sınırına](container-registry-skus.md#service-tier-features-and-limits)kadar, ihtiyacınız olan sayıda depo, resim, katman veya etiket oluşturmanızı sağlar. 

Çok yüksek sayıda depolar ve Etiketler, kayıt defterinizin performansını etkileyebilir. Kullanılmayan depoları, etiketleri ve görüntüleri kayıt defteri bakım yordamlarınızın bir parçası olarak düzenli olarak silin ve etiketlenmemiş bildirimler için isteğe bağlı olarak bir [bekletme ilkesi](container-registry-retention-policy.md) ayarlayın. Depolar, görüntüler *ve Etiketler gibi* silinen kayıt defteri kaynakları silinmeden sonra kurtarılamaz. Kayıt defteri kaynaklarını silme hakkında daha fazla bilgi için bkz. [Azure Container Registry kapsayıcı görüntülerini silme](container-registry-delete.md).

## <a name="storage-cost"></a>Depolama maliyeti

Fiyatlandırma hakkında tam Ayrıntılar için bkz. [Azure Container Registry fiyatlandırması][pricing].

## <a name="next-steps"></a>Sonraki adımlar

Temel, standart ve Premium kapsayıcı kayıt defterleri hakkında daha fazla bilgi için bkz. [Azure Container Registry hizmet katmanları](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
