---
title: Azure Container Registry coğrafi çoğaltma
description: Coğrafi olarak çoğaltılan Azure kapsayıcısı kayıt defterleri oluşturmaya ve yönetmeye başlayın.
services: container-registry
author: stevelas
manager: gwallace
ms.service: container-registry
ms.topic: overview
ms.date: 05/24/2019
ms.author: stevelas
ms.openlocfilehash: 2fffa3b063969cbe68fb9a405f4198f15b3f9809
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845211"
---
# <a name="geo-replication-in-azure-container-registry"></a>Azure Container Registry’de coğrafi çoğaltma

Yerel bir varlık ya da etkin bir yedekleme isteyen şirketler, birden çok Azure bölgesinden Hizmetleri çalıştırmayı seçin. En iyi uygulama olarak, resimlerin çalıştırıldığı her bölgeye bir kapsayıcı kayıt defteri yerleştirmek, ağ kapatma işlemlerine izin vererek, hızlı ve güvenilir görüntü katmanı aktarımlarını etkinleştirir. Coğrafi çoğaltma, Azure Container Registry 'nin tek bir kayıt defteri olarak çalışmasını sağlar ve çoklu ana bölge kayıt defterlerine sahip birden çok bölgeye hizmet verebilir. 

Coğrafi olarak çoğaltılan bir kayıt defteri aşağıdaki avantajları sağlar:

* Tek kayıt defteri/görüntü/etiket adları birden çok bölgede kullanılabilir
* Ağ-bölgesel dağıtımlardan kayıt defteri erişimini kapat
* Başka çıkış ücretleri yoktur, çünkü görüntü, kapsayıcı konağınız ile aynı bölgedeki yerel, çoğaltılan bir kayıt defterinden alınır
* Birden çok bölgede bir kayıt defterinin tek yönetimi

> [!NOTE]
> Birden fazla Azure Container Registry 'de kapsayıcı görüntülerinin kopyalarını korumanız gerekiyorsa Azure Container Registry de [görüntü içeri aktarmayı](container-registry-import-images.md)destekler. Örneğin, bir DevOps iş akışında, Docker komutlarını kullanmaya gerek kalmadan bir geliştirme kayıt defterindeki bir görüntüyü üretim kayıt defterine aktarabilirsiniz.
>

## <a name="example-use-case"></a>Örnek kullanım örneği
Contoso, ABD, Kanada ve Avrupa 'Da bulunan bir genel varlık Web sitesi çalıştırır. Contoso, yerel ve ağ-kapatma içeriğiyle bu pazarlara hizmet vermek için Batı ABD, Doğu ABD, Kanada Orta ve Batı Avrupa [Azure Kubernetes hizmeti](/azure/aks/) (aks) kümelerini çalıştırır. Docker görüntüsü olarak dağıtılan Web sitesi uygulaması, tüm bölgelerde aynı kod ve görüntüyü kullanır. Bu bölgeye yerel olan içerik, her bölgede benzersiz olarak sağlanan bir veritabanından alınır. Her bölgesel dağıtımın, yerel veritabanı gibi kaynaklar için benzersiz yapılandırması vardır.

Geliştirme ekibi, Batı ABD veri merkezini kullanarak Seattle WA 'da bulunur.

![Birden çok kayıt defterlerine iletme](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Birden çok kayıt defterlerine iletme*

Contoso, coğrafi çoğaltma özelliklerini kullanmadan önce Batı Avrupa ek bir kayıt defteriyle Batı ABD içinde ABD tabanlı kayıt defterine sahipti. Bu farklı bölgelere hizmeti sağlamak için, geliştirme ekibi görüntüleri iki farklı kayıt defterine gönderdi.

```bash
docker push contoso.azurecr.io/public/products/web:1.2
docker push contosowesteu.azurecr.io/public/products/web:1.2
```
![Birden çok kayıt defterlerinden çekme](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Birden çok kayıt defterlerinden çekme*

Birden çok kayıt defterlerinin tipik sorunları şunlardır:

* Doğu ABD, Batı ABD ve Kanada Orta kümeleri, bu uzak kapsayıcının her biri için çıkış ücretlerini, Batı ABD veri merkezlerinden çekme görüntüleri barındırarak, tüm Batı ABD kayıt defterinden çeker.
* Geliştirme ekibinin görüntüleri Batı ABD ve Batı Avrupa kayıt defterlerine itmesi gerekir.
* Geliştirme ekibinin her bölgesel dağıtımı, yerel kayıt defterine başvuran görüntü adlarıyla yapılandırması ve koruması gerekir.
* Her bölge için kayıt defteri erişiminin yapılandırılması gerekir.

## <a name="benefits-of-geo-replication"></a>Coğrafi çoğaltmanın avantajları

![Coğrafi olarak çoğaltılan bir kayıt defterinden çekme](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Azure Container Registry coğrafi çoğaltma özelliğini kullanarak bu avantajlar gerçekleştirilir:

* Tüm bölgelerde tek bir kayıt defterini yönetme:`contoso.azurecr.io`
* Tüm bölgeler aynı görüntü URL 'sini kullandığı için tek bir görüntü dağıtımı yapılandırmasını yönetin:`contoso.azurecr.io/public/products/web:1.2`
* Tek bir kayıt defterine göndererek ACR, Coğrafi çoğaltmayı yönetir. Bölgesel [Web kancalarını](container-registry-webhook.md) belirli çoğaltmalarda olayları bilgilendirmek üzere yapılandırabilirsiniz.

## <a name="configure-geo-replication"></a>Coğrafi çoğaltmayı yapılandırma

Coğrafi çoğaltmanın yapılandırılması, bir haritadaki bölgelere tıklanması kadar kolaydır. Azure CLı 'de [az ACR çoğaltma](/cli/azure/acr/replication) komutları dahil olmak üzere Coğrafi çoğaltmayı da yönetebilirsiniz.

Coğrafi çoğaltma yalnızca [Premium kayıt defterlerinin](container-registry-skus.md) bir özelliğidir. Kayıt defteriniz henüz Premium değilse, [Azure Portal](https://portal.azure.com)temel ve standart 'den Premium 'a geçiş yapabilirsiniz:

![Azure portal SKU 'Ları değiştirme](media/container-registry-skus/update-registry-sku.png)

Premium kayıt defteriniz için Coğrafi çoğaltmayı yapılandırmak üzere, konumundaki https://portal.azure.com Azure Portal oturum açın.

Azure Container Registry gidin ve **çoğaltmalar**' ı seçin:

![Azure portalı kapsayıcı kayıt defteri kullanıcı arabirimindeki Çoğaltmalar](media/container-registry-geo-replication/registry-services.png)

Geçerli tüm Azure bölgelerini gösteren bir harita görüntülenir:

 ![Azure portalındaki bölge haritası](media/container-registry-geo-replication/registry-geo-map.png)

* Mavi altıgenler geçerli çoğaltmaları temsil eder
* Yeşil altıgenler olası çoğaltma bölgelerini temsil eder
* Gri altıgenler, henüz çoğaltma için kullanılamayan Azure bölgelerini temsil eder

Bir çoğaltma yapılandırmak için yeşil bir altıon seçin, sonra **Oluştur**' u seçin.

 ![Azure portalında çoğaltma oluşturmaya yönelik kullanıcı arabirimi](media/container-registry-geo-replication/create-replication.png)

Ek çoğaltmalar yapılandırmak için, diğer bölgelerin yeşil altılarını seçin ve ardından **Oluştur**' a tıklayın.

ACR, yapılandırılmış çoğaltmalar genelinde görüntüleri eşitlemeye başlar. Tamamlandıktan sonra Portal, *Ready*olarak yansıtır. Portaldaki çoğaltma durumu otomatik olarak güncelleştirmez. Güncelleştirilmiş durumu görmek için Yenile düğmesini kullanın.

## <a name="considerations-for-using-a-geo-replicated-registry"></a>Coğrafi olarak çoğaltılan kayıt defteri kullanma konuları

* Coğrafi olarak çoğaltılan bir kayıt defterindeki her bölge, ayarlandıktan sonra bağımsızdır. Azure Container Registry SLA 'Lar, coğrafi olarak çoğaltılan her bölge için geçerlidir.
* Coğrafi olarak çoğaltılan bir kayıt defterinden görüntü gönderdiğinizde veya çektiğinizde, arka planda Azure Traffic Manager, isteği size en yakın bölgede bulunan kayıt defterine gönderir.
* En yakın bölgeye bir görüntü veya etiket güncelleştirmesi gönderdikten sonra, Azure Container Registry bildirimlerin ve katmanların seçtiğiniz kalan bölgelere çoğaltılması biraz zaman alır. Daha büyük resimler daha küçük olanlara çoğaltılmak için daha uzun sürer. Görüntüler ve Etiketler, son tutarlılık modeliyle çoğaltma bölgeleri arasında eşitlenir.
* Coğrafi olarak çoğaltılan bir kayıt defterine gönderim güncelleştirmelerine bağlı olan iş akışlarını yönetmek için, [Web kancalarını](container-registry-webhook.md) anında iletme olaylarına yanıt verecek şekilde yapılandırmanızı öneririz. Coğrafi olarak çoğaltılan bölgelerde gerçekleştirilen anında iletme olaylarını izlemek için coğrafi olarak çoğaltılan bir kayıt defteri içinde bölgesel Web kancaları oluşturabilirsiniz.


## <a name="geo-replication-pricing"></a>Coğrafi çoğaltma fiyatlandırması

Coğrafi çoğaltma, Azure Container Registry [PREMIUM SKU](container-registry-skus.md) 'sunun bir özelliğidir. İstediğiniz bölgelere bir kayıt defteri çoğalttığınızda, her bölge için Premium kayıt defteri ücretlerine tabi olursunuz.

Yukarıdaki örnekte, contoso iki kayıt, Doğu ABD, Kanada Orta ve Batı Avrupa çoğaltmaları ekleyerek bir yukarı Birleşik olarak birleştirilmiş. Contoso, ek yapılandırma veya yönetim olmadan ayda dört kat Premium ödemenizi ister. Her bölge artık görüntülerini yerel olarak çeker, performansı, Batı ABD ağ çıkış ücretleri olmadan Kanada ve Doğu ABD artırır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Container Registry coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md)olmak üzere üç parçalı öğretici serisine göz atın. Coğrafi olarak çoğaltılan bir kayıt defteri oluşturma, kapsayıcı oluşturma ve daha sonra kapsayıcı örnekleri için birden çok bölgesel Web Apps `docker push` tek bir komutla dağıtma adımları.

> [!div class="nextstepaction"]
> [Azure Container Registry coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md)
