---
title: Kayıt defterini coğrafi olarak çoğaltma
description: Kayıt defterinin çok büyük bölgesel yinelemelerle birden çok bölgeye hizmet etmesini sağlayan coğrafi olarak çoğaltılmış bir Azure kapsayıcı kayıt defteri oluşturmaya ve yönetmeye başlayın.
author: stevelas
ms.topic: article
ms.date: 08/16/2019
ms.author: stevelas
ms.openlocfilehash: d238de30e458261a11c941c03ac127c732ca8d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456450"
---
# <a name="geo-replication-in-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defteri'nde coğrafi çoğaltma

Yerel bir varlık veya sıcak yedekleme isteyen şirketler, birden çok Azure bölgesinden hizmetleri çalıştırmayı seçer. En iyi yöntem olarak görüntülerin çalıştığı her bölgeye bir kapsayıcı kayıt defteri yerleştirmek, hızlı ve güvenilir görüntü katmanı aktarımları sağlayan ağa yakın işlemlere olanak tanır. Azure kapsayıcı kayıt defterinin, çoklu ana kaynağa sahip bölgesel kayıt defterleri ile birden çok bölgeyi sunan tek bir kayıt defteri olarak işlev görmesini sağlar. 

Coğrafi olarak çoğaltılmış bir kayıt defteri aşağıdaki avantajları sağlar:

* Tek bir kayıt defteri/görüntü/etiket adları birden çok bölgede kullanılabilir
* Bölgesel dağıtımlardan ağa yakın kayıt defteri erişimi
* Görüntüler, kapsayıcı ana bilgisayarınızla aynı bölgede bulunan çoğaltılmış yerel bir kayıt defterinden çekildiğinden ek çıkış ücretleri uygulanmaz
* Kayıt defterinin birden çok bölgede tek yönetimi

> [!NOTE]
> Birden fazla Azure kapsayıcı kayıt defterinde kapsayıcı görüntülerinin kopyalarını korumanız gerekiyorsa, Azure Kapsayıcı Kayıt Defteri [görüntü alma'yı](container-registry-import-images.md)da destekler. Örneğin, DevOps iş akışında, Docker komutlarını kullanmanıza gerek kalmadan bir resmi geliştirme kayıt defterinden üretim kayıt defterine aktarabilirsiniz.
>

## <a name="example-use-case"></a>Örnek kullanım örneği
Contoso, ABD, Kanada ve Avrupa genelinde bulunan bir kamu varlığı web sitesi çalışır. Contoso, bu pazarlara yerel ve ağ yakın içeriğiyle hizmet vermek için Batı ABD, Doğu ABD, Kanada Orta ve Batı Avrupa'da [Azure Kubernetes Service](/azure/aks/) (AKS) kümeleri çalıştırır. Docker görüntüsü olarak dağıtılan web sitesi uygulaması, tüm bölgelerde aynı kodu ve görüntüyü kullanır. Bu bölgeye yerel olan içerik, her bölgede benzersiz olarak sağlanan bir veritabanından alınır. Her bölgesel dağıtım, yerel veritabanı gibi kaynaklar için benzersiz bir yapılandırmaya sahiptir.

Geliştirme ekibi Seattle WA' da, Batı ABD veri merkezini kullanmaktadır.

![Birden çok kayıt defterine bastırma](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Birden çok kayıt defterine bastırma*

Jeo-çoğaltma özelliklerini kullanmadan önce, Contoso'nun Batı Abd'de ABD merkezli bir kayıt defteri ve Batı Avrupa'da ek bir kayıt defteri vardı. Geliştirme ekibi, bu farklı bölgelere hizmet vermek için görüntüleri iki farklı kayıt defterine taşıdı.

```bash
docker push contoso.azurecr.io/public/products/web:1.2
docker push contosowesteu.azurecr.io/public/products/web:1.2
```
![Birden çok kayıt defterinden çekme](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Birden çok kayıt defterinden çekme*

Birden çok kayıt defterinin tipik zorlukları şunlardır:

* Doğu ABD, Batı ABD ve Kanada Merkez kümeleri, bu uzak konteyner ana bilgisayarların her biri Batı ABD veri merkezlerinden görüntüleri çekmek gibi çıkış ücretleri tahakkuk, Batı ABD kayıt tüm çekin.
* Geliştirme ekibi görüntüleri Batı ABD ve Batı Avrupa kayıtlarına itmelidir.
* Geliştirme ekibi, her bölgesel dağıtımı yerel kayıt defterine atıfta bulunan görüntü adlarıyla yapılandırmalı ve sürdürmelidir.
* Kayıt defteri erişimi her bölge için yapılandırılmalıdır.

## <a name="benefits-of-geo-replication"></a>Coğrafi çoğaltmanın faydaları

![Coğrafi olarak çoğaltılmış bir kayıt defterinden çekme](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Azure Kapsayıcı Kayıt Defteri'nin coğrafi çoğaltma özelliği kullanılarak aşağıdaki avantajlar gerçekleştirilir:

* Tüm bölgelerde tek bir kayıt defterini yönetin:`contoso.azurecr.io`
* Tüm bölgeler aynı görüntü URL'sini kullandığından, görüntü dağıtımlarının tek bir yapılandırmasını yönetin:`contoso.azurecr.io/public/products/web:1.2`
* ACR coğrafi çoğaltmayı yönetirken tek bir kayıt defterine itin. Belirli yinelemelerde olayları size bildirmek için bölgesel [webhooks](container-registry-webhook.md) yapılandırabilirsiniz.

## <a name="configure-geo-replication"></a>Coğrafi çoğaltmayı yapılandırma

Coğrafi çoğaltmayı yapılandırmak, bir haritadaki bölgeleri tıklatmak kadar kolaydır. Ayrıca Azure CLI'deki [az acr çoğaltma](/cli/azure/acr/replication) komutları da dahil olmak üzere araçları kullanarak coğrafi çoğaltmayı yönetebilir veya Azure [Kaynak Yöneticisi şablonuyla](https://github.com/Azure/azure-quickstart-templates/tree/master/101-container-registry-geo-replication)coğrafi çoğaltma için etkin leştirilmiş bir kayıt defteri dağıtabilirsiniz.

Coğrafi çoğaltma yalnızca Premium [kayıt defterlerinin](container-registry-skus.md) bir özelliğidir. Kayıt defteriniz henüz Premium değilse, [Azure portalında](https://portal.azure.com)Temel ve Standart'tan Premium'a geçiş yapabilirsiniz:

![Azure portalında SK'leri değiştirme](media/container-registry-skus/update-registry-sku.png)

Premium kayıt defteriniz için coğrafi çoğaltmayı yapılandırmak için Azure https://portal.azure.comportalına 'da giriş yapın.

Azure Kapsayıcı Kayıt Defterinize gidin ve **Çoğaltmaları**seçin:

![Azure portalı kapsayıcı kayıt defteri kullanıcı arabirimindeki Çoğaltmalar](media/container-registry-geo-replication/registry-services.png)

Tüm geçerli Azure Bölgelerini gösteren bir harita görüntülenir:

 ![Azure portalındaki bölge haritası](media/container-registry-geo-replication/registry-geo-map.png)

* Mavi altıgenler geçerli yinelemeleri temsil eder
* Yeşil altıgenler olası çoğaltma bölgelerini temsil eder
* Gri altıgenler, çoğaltma için henüz kullanılamayan Azure bölgelerini temsil eder

Yinelemeyi yapılandırmak için yeşil altıgen seçin ve ardından **Oluştur'u**seçin:

 ![Azure portalında çoğaltma oluşturmaya yönelik kullanıcı arabirimi](media/container-registry-geo-replication/create-replication.png)

Ek yinelemeleri yapılandırmak için, diğer bölgeler için yeşil altıgenleri seçin ve ardından **Oluştur'u**tıklatın.

ACR, görselleri yapılandırılan yinelemeler arasında eşitlemeye başlar. Tamamlandıktan sonra, portal *Hazır*yansıtır. Portaldaki yineleme durumu otomatik olarak güncellenmez. Güncelleştirilmiş durumu görmek için yenileme düğmesini kullanın.

## <a name="considerations-for-using-a-geo-replicated-registry"></a>Coğrafi olarak çoğaltılmış bir kayıt defteri kullanmak için dikkat edilmesi gerekenler

* Coğrafi olarak çoğaltılmış bir kayıt defterindeki her bölge kurulduktan sonra bağımsızdır. Azure Konteyner Kayıt Defteri SLA'ları, coğrafi olarak çoğaltılan her bölge için geçerlidir.
* Coğrafi olarak çoğaltılan bir kayıt defterinden görüntüleri itdiğinizde veya çektiğinde, arka plandaki Azure Trafik Yöneticisi isteği size en yakın bölgede bulunan kayıt defterine gönderir.
* Bir resmi veya etiket güncelleştirmesini en yakın bölgeye ittikten sonra, Azure Kapsayıcı Kayıt Defteri'nin bildirimleri ve katmanları tercih ettiğiniz kalan bölgelere çoğaltması biraz zaman alır. Daha büyük görüntülerin çoğaltılması daha küçük görüntülere göre daha uzun sürer. Görüntüler ve etiketler, çoğaltma bölgeleri arasında nihai tutarlılık modeliyle eşitlenir.
* Coğrafi olarak çoğaltılan push güncellemelerine bağlı iş akışlarını yönetmek için, itme olaylarına yanıt verecek [şekilde web hooks'ları](container-registry-webhook.md) yapılandırmanızı öneririz. Coğrafi olarak çoğaltılan bölgelerde tamamlanan itme olaylarını izlemek için coğrafi olarak çoğaltılan bir kayıt defteri içinde bölgesel web hook'lar ayarlayabilirsiniz.

## <a name="delete-a-replica"></a>Bir çoğaltmayı sil

Kayıt defteriniz için bir yineleme yapılandırıldıktan sonra, artık ihtiyaç duyulmamışsa istediğiniz zaman silebilirsiniz. Azure portalını veya Azure CLI'deki [az acr çoğaltma silme](/cli/azure/acr/replication#az-acr-replication-delete) komutunu kullanarak bir yinelemeyi silin.

Azure portalında bir yinelemeyi silmek için:

1. Azure Kapsayıcı Kayıt Defteri'nize gidin ve **Çoğaltmaları**seçin.
1. Yinelemenin adını seçin ve **Sil'i**seçin. Yinelemeyi silmek istediğinizi onaylayın.

> [!NOTE]
> Kayıt defteri yinelemesini, yani kayıt *home region* defterini oluşturduğunuz konumu silemezsiniz. Yalnızca kayıt defterinin kendisini silerek ev yinelemesini silebilirsiniz.

## <a name="geo-replication-pricing"></a>Coğrafi çoğaltma fiyatlandırması

Coğrafi çoğaltma, Azure Konteyner Kayıt Defteri'nin [Premium SKU](container-registry-skus.md) özelliğidir. Bir kayıt defterini istediğiniz bölgelere çoğalttırken, her bölge için Premium kayıt ücreti alırsınız.

Önceki örnekte, Contoso doğu ABD, Kanada Orta ve Batı Avrupa'ya kopyaları ekleyerek iki kayıt defterini bire kadar birleştirmiştir. Contoso, ek yapılandırma veya yönetim olmadan ayda dört kat Premium ödeyecekti. Her bölge artık görüntülerini yerel olarak çekerek, Batı ABD'den Kanada'ya ve Doğu ABD'ye ağ çıkış ücretleri olmadan performansı ve güvenilirliği artırıyor.

## <a name="troubleshoot-push-operations-with-geo-replicated-registries"></a>Coğrafi olarak çoğaltılan kayıt defterleriyle sorun giderme itme işlemleri
 
Görüntüyü coğrafi olarak çoğaltılan bir kayıt defterine iten bir Docker istemcisi, tüm görüntü katmanlarını ve manifestounu tek bir yinelenen bölgeye itmeyebilir. Azure Trafik Yöneticisi kayıt defteri isteklerini ağa en yakın yinelenen kayıt defterine yönlendirir, bu durum oluşabilir. Kayıt defterinde *yakındaki* iki çoğaltma bölgesi varsa, görüntü katmanları ve bildirim iki siteye dağıtılabilir ve bildirim doğrulandığında itme işlemi başarısız olur. Bu sorun, kayıt defterinin DNS adının bazı Linux ana bilgisayarlarında çözülme şeklinden oluşur. Bu sorun, istemci tarafı DNS önbelleği sağlayan Windows'da oluşmaz.
 
Bu sorun oluşursa, bir çözüm Linux ana bilgisayar gibi `dnsmasq` istemci tarafı DNS önbelleği uygulamaktır. Bu, kayıt defterinin adının tutarlı bir şekilde çözülmesini sağlamaya yardımcı olur. Bir kayıt defterine gitmek için Azure'da bir Linux VM kullanıyorsanız, [Azure'daki Linux sanal makineleri için DNS Ad Çözümü seçeneklerindeki seçeneklere](../virtual-machines/linux/azure-dns.md)bakın.

Görüntüleri iterken DNS çözünürlüğünü en yakın yinelemeye en iyi duruma getirmek için, itme işlemlerinin kaynağıyla aynı Azure bölgelerinde veya Azure dışında çalışırken en yakın bölgede coğrafi olarak çoğaltılan bir kayıt defterini yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Konteyner Kayıt Defteri'nde üç](container-registry-tutorial-prepare-registry.md)bölümlük öğretici serisine, Coğrafi çoğaltmaya göz atın. Coğrafi olarak çoğaltılmış bir kayıt defteri oluşturma, bir kapsayıcı oluşturma ve `docker push` ardından tek bir komutla konteynerler için birden çok bölgesel Web Apps örneğine dağıtma konusunda yürüyün.

> [!div class="nextstepaction"]
> [Azure Kapsayıcı Kayıt Defteri'nde coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md)
