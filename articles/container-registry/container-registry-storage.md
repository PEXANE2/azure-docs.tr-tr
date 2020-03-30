---
title: Konteyner görüntü depolama
description: Docker kapsayıcı resimlerinizin güvenlik, artıklık ve kapasite dahil olmak üzere Azure Kapsayıcı Kayıt Defteri'nde nasıl depolandırılma konusundaki ayrıntılar.
ms.topic: article
ms.date: 03/21/2018
ms.openlocfilehash: f66c3dd95edfe5035c46857cb6f9aa59d8a6a0e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456213"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defteri'nde kapsayıcı görüntü depolama

Her [Temel, Standart ve Premium](container-registry-skus.md) Azure kapsayıcı kayıt defteri, görüntü veri güvenliği için şifreleme ve görüntü verilerinin korunması için coğrafi artıklık gibi gelişmiş Azure depolama özelliklerinden yararlanır. Aşağıdaki bölümlerde Azure Kapsayıcı Kayıt Defteri'nde (ACR) görüntü depolamanın hem özellikleri hem de sınırları açıklanmaktadır.

## <a name="encryption-at-rest"></a>Şifreleme-at-rest

Kayıt defterinizdeki tüm konteyner görüntüleri istirahatte şifrelenir. Azure, bir görüntüyü depolamadan önce görüntüyü otomatik olarak şifreler ve siz veya uygulamalarınız ve hizmetleriniz görüntüyü çektiğinizde anında şifresini çözer.

## <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

Azure, konteyner görüntülerinizin kaybedilmesine karşı korunmak için coğrafi yedekli bir depolama düzeni kullanır. Azure Kapsayıcı Kayıt Defteri, kapsayıcı görüntülerinizi coğrafi olarak uzak birçok veri merkezine otomatik olarak çoğaltarak bölgesel bir depolama hatası durumunda bunların kaybolmasını önler.

## <a name="geo-replication"></a>Coğrafi çoğaltma

Daha yüksek kullanılabilirlik güvencesi gerektiren senaryolar için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanmayı düşünün. Coğrafi çoğaltma, yalnızca bir depolama hatası değil, *toplam* bölgesel bir hata durumunda kayıt defterinize erişimi kaybetmeye karşı koruma sağlar. Coğrafi çoğaltma, dağıtılmış geliştirme veya dağıtım senaryolarında daha hızlı itme ler ve çekmeler için ağı kapatan görüntü depolama gibi başka avantajlar da sağlar.

## <a name="image-limits"></a>Görüntü sınırları

Aşağıdaki tabloda, Azure kapsayıcı kayıt defterleri için yerinde bulunan kapsayıcı görüntüsü ve depolama sınırları açıklanmaktadır.

| Kaynak | Sınır |
| -------- | :---- |
| Depolar | Sınır yok |
| Görüntüler | Sınır yok |
| Katmanlar | Sınır yok |
| Etiketler | Sınır yok|
| Depolama | 5 TB |

Çok sayıda depo ve etiket kayıt defterinizin performansını etkileyebilir. Kayıt defteri bakım rutininizin bir parçası olarak kullanılmayan depoları, etiketleri ve görüntüleri düzenli aralıklarla silin. Depolar, resimler ve etiketler gibi silinen kayıt defteri kaynakları silindikten sonra *kurtarılamaz.* Kayıt defteri kaynaklarını silme hakkında daha fazla bilgi için Azure [Kapsayıcı Kayıt Defteri'ndeki kapsayıcı görüntülerini sil'e](container-registry-delete.md)bakın.

## <a name="storage-cost"></a>Depolama maliyeti

Fiyatlandırma hakkında tüm ayrıntılar için [Azure Konteyner Kayıt Defteri fiyatlandırması'na][pricing]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Farklı Azure Konteyner Kayıt Defteri SK'leri (Temel, Standart, Premium) hakkında daha fazla bilgi için Azure [Konteyner Kayıt Defteri SK'leri'ne](container-registry-skus.md)bakın.

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
