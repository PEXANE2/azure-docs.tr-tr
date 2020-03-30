---
title: DNS Bölgeler ve Kayıtlara genel bakış - Azure DNS
description: Microsoft Azure DNS'de DNS bölgelerini ve kayıtlarını barındırmadesteğine genel bakış.
author: rohinkoul
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.topic: conceptual
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: rohink
ms.openlocfilehash: 19189af6424960b8e20be686af745b10f2d8578b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265161"
---
# <a name="overview-of-dns-zones-and-records"></a>DNS bölgeleri ve kayıtlarına genel bakış

Bu sayfada etki alanları, DNS bölgeleri ve DNS kayıtları ve kayıt kümeleri ile ilgili temel kavramlar ve bunların Azure DNS'de nasıl desteklendikleri açıklanmaktadır.

## <a name="domain-names"></a>Etki alanı adları

Etki Alanı Adı Sistemi, bir etki alanları hiyerarşisidir. Hiyerarşi, adı basitçe '**.**'olan 'kök' etki alanından başlar.  Bunun altında "com", "net", "org", "uk" veya "jp" gibi en üst düzey etki alanları bulunur.  Bunların altında "org.uk" veya "co.jp" gibi ikinci düzey etki alanları bulunur. DNS hiyerarşisindeki etki alanları, tüm dünyada DNS ad sunucuları tarafından barındırılan genel olarak dağıtılır.

Etki alanı adı kayıt şirketi, `contoso.com`bir etki alanı adı satın almanızı sağlayan bir kuruluştur.  Bir etki alanı adı satın almak, bu ad altında DNS hiyerarşisini kontrol etme `www.contoso.com` hakkını verir, örneğin adı şirketinizin web sitesine yönlendirmenize olanak sağlar. Kayıt şirketi etki alanını sizin adınıza kendi ad sunucularında barındırabilir veya alternatif ad sunucuları belirtmenize izin verebilir.

Azure DNS, etki alanınızı barındırmak için kullanabileceğiniz, genel olarak dağıtılan, yüksek kullanılabilirlikli bir ad sunucusu altyapısı sağlar. Etki alanlarınızı Azure DNS'de barındırarak, DNS kayıtlarınızı diğer Azure hizmetlerinizle aynı kimlik bilgileri, API'ler, araçlar, faturalandırma ve destekle yönetebilirsiniz.

Azure DNS şu anda alan adlarının satın alımını desteklememektedir. Bir etki alanı adı satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt şirketi kullanmanız gerekir. Kayıt şirketi genellikle küçük bir yıllık ücret alır. Etki alanları daha sonra DNS kayıtlarının yönetimi için Azure DNS'de barındırılabilir. Ayrıntılar için bkz. [Azure DNS'ye bir etki alanı devretme](dns-domain-delegation.md).

## <a name="dns-zones"></a>DNS bölgeleri

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS kayıtları

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Zaman-to-live

Yaşama süresi veya TTL, yeniden sorgulanmadan önce her kaydın istemciler tarafından ne kadar süre önbelleğe alındığını belirtir. Yukarıdaki örnekte, TTL 3600 saniye veya 1 saattir.

Azure DNS'de, TTL her kayıt için değil, kayıt kümesi için belirtilir, bu nedenle bu kayıt kümesindeki tüm kayıtlar için aynı değer kullanılır.  1 ile 2.147.483.647 saniye arasında herhangi bir TTL değeri belirtebilirsiniz.

### <a name="wildcard-records"></a>Joker karakter kayıtları

Azure DNS [joker kayıtlarını](https://en.wikipedia.org/wiki/Wildcard_DNS_record) destekler. Joker karakter kayıtları eşleşen bir ada sahip herhangi bir sorguya yanıt olarak döndürülür (joker karakter olmayan bir kayıt kümesinden daha yakın bir eşleşme yoksa). Azure DNS, NS ve SOA dışındaki tüm kayıt türleri için joker karakter kayıt kümelerini destekler.

Joker karakter kaydı kümesi oluşturmak için, 'kayıt kümesi adını\*kullanın. Alternatif olarak, '' ' '\*ile bir ad da kullanabilirsiniz\*onun sol en etiketi olarak, örneğin, 'foo'.

### <a name="caa-records"></a>CAA kayıtları

CAA kayıtları, etki alanı sahiplerinin etki alanları için hangi Sertifika Yetkililerinin (CA) sertifika verme yetkisine sahip olduğunu belirtmelerine olanak sağlar. Bu, C'lerin bazı durumlarda yanlış sertifika vermekten kaçınmasına olanak tanır. CAA kayıtlarının üç özelliği vardır:
* **Bayraklar**: [RFC'ye](https://tools.ietf.org/html/rfc6844#section-3) göre özel bir anlamı olan kritik bayrağı temsil etmek için kullanılan 0 ile 255 arasında bir tamsayıdır
* **Tag**: aşağıdakilerden biri olabilecek bir ASCII dizesi:
    * **sorun**: sertifika (tüm türler) verme izni verilen CA'ları belirtmek istiyorsanız bunu kullanın
    * **issuewild**: sertifika verme izni olan CA'ları belirtmek istiyorsanız bunu kullanın (yalnızca joker sertifikalar)
    * **iodef**: CAs'ın yetkisiz cert sorunu isteklerini bildirebileceği bir e-posta adresi veya hostname belirtin
* **Değer**: seçilen belirli Etiket için değer

### <a name="cname-records"></a>CNAME kayıtları

CNAME kaydı kümeleri aynı ada sahip diğer kayıt kümeleriyle birlikte olamaz. Örneğin, göreli adı 'www' ve aynı anda 'www' adı olan bir a kaydı ile ayarlanmış bir CNAME kaydı oluşturamazsınız.

Bölge apeksi (ad =\@' ') her zaman bölge oluşturulduğunda oluşturulan NS ve SOA kayıt kümelerini içerdiğinden, bölge tepe noktasında ayarlanan bir CNAME kaydı oluşturamazsınız.

Bu kısıtlamalar DNS standartlarından kaynaklanmaktadır ve Azure DNS sınırlamaları değildir.

### <a name="ns-records"></a>NS kayıtları

Bölge tepe noktasında ayarlanan NS kaydı\@(adı ' ') her DNS bölgesinde otomatik olarak oluşturulur ve bölge silindiğinde otomatik olarak silinir (ayrı olarak silinemez).

Bu kayıt kümesi, bölgeye atanan Azure DNS ad sunucularının adlarını içerir. Birden fazla DNS sağlayıcısıyla birlikte barındırılabilen etki alanlarını desteklemek için bu NS kayıt kümesine ek ad sunucuları ekleyebilirsiniz. Bu kayıt kümesi için TTL ve meta verilerini de değiştirebilirsiniz. Ancak, önceden doldurulmuş Azure DNS ad sunucularını kaldıramaz veya değiştiremezsiniz. 

Bu yalnızca bölge tepe noktasında ayarlanan NS kaydı için geçerlidir. Bölgenizdeki diğer NS kayıt kümeleri (alt bölgeleri temsilcilendirmek için kullanılır) kısıtolmadan oluşturulabilir, değiştirilebilir ve silinebilir.

### <a name="soa-records"></a>SOA kayıtları

SOA kayıt kümesi her bölgenin tepesinde otomatik olarak oluşturulur (ad = '\@'), ve bölge silindiğinde otomatik olarak silinir.  SOA kayıtları ayrı olarak oluşturulamaz veya silinemez.

Azure DNS tarafından sağlanan birincil ad sunucusu adını ifade edecek şekilde önceden yapılandırılan 'ana bilgisayar' özelliği dışında SOA kaydının tüm özelliklerini değiştirebilirsiniz.

SOA kaydındaki bölge seri numarası, bölge kayıtlarında değişiklik yapıldığında otomatik olarak güncelleştirilmeyecektir. Gerekirse SOA kaydı nı düzenleyerek el ile güncellenebilir.

### <a name="spf-records"></a>SPF kayıtları

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV kayıtları

[SRV kayıtları](https://en.wikipedia.org/wiki/SRV_record) sunucu konumlarını belirtmek için çeşitli hizmetler tarafından kullanılır. Azure DNS'de bir SRV kaydı belirtirken:

* *Hizmet* ve *protokol,* alt çizerlerle önceden belirlenmiş kayıt kümesi adının bir parçası olarak belirtilmelidir.  Örneğin, 'yudum.\_ \_tcp.name' dedi.  Bölge tepe noktasındaki bir kayıt için, kayıt\@adında ' ' belirtmenize gerek yoktur, sadece\_hizmeti ve protokolü kullanın, örneğin ' sip. \_tcp'dir.
* *Öncelik,* *ağırlık,* *bağlantı noktası*ve *hedef,* kayıt kümesindeki her kaydın parametreleri olarak belirtilir.

### <a name="txt-records"></a>TXT kayıtları

TXT kayıtları, alan adlarını rasgele metin dizeleri ile eşlemek için kullanılır. Gönderen [İlkesi Çerçevesi (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) ve [DomainKeys Identified Mail (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail)gibi e-posta yapılandırması ile ilgili olmak üzere birden çok uygulamada kullanılırlar.

DNS standartları, her biri 254 karaktere kadar uzunlukta olabilecek birden çok dize içeren tek bir TXT kaydına izin verir. Birden çok dize kullanıldığında, istemciler tarafından kısıtlanır ve tek bir dize olarak kabul edilir.

Azure DNS REST API'sini ararken, her TXT dizesini ayrı ayrı belirtmeniz gerekir.  Azure portalı, PowerShell veya CLI arabirimlerini kullanırken kayıt başına otomatik olarak gerekirse 254 karakterlik segmentlere bölünmüş tek bir dize belirtmeniz gerekir.

Bir DNS kaydındaki birden çok dize, Bir TXT kayıt kümesindeki birden çok TXT kaydıyla karıştırılmamalıdır.  TXT kayıt kümesi, her *biri* birden çok dize içerebilen birden çok kayıt içerebilir.  Azure DNS, her TXT kayıt kümesinde (tüm kayıtların birarada bulunduğu nda) 1024 karaktere kadar toplam dize uzunluğunu destekler.

## <a name="tags-and-metadata"></a>Etiketler ve meta veriler

### <a name="tags"></a>Etiketler

Etiketler ad değeri çiftleri listesidir ve Azure Kaynak Yöneticisi tarafından kaynakları etiketlemek için kullanılır.  Azure Kaynak Yöneticisi, Azure faturanızın filtrelenmiş görünümlerini etkinleştirmek için etiketleri kullanır ve ayrıca hangi etiketlerin gerekli olduğuna ilişkin bir ilke belirlemenize olanak tanır. Etiketler hakkında daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/management/tag-resources.md).

Azure DNS, DNS bölge kaynaklarında Azure Kaynak Yöneticisi etiketlerini kullanmayı destekler.  DNS kayıt kümelerinde etiketleri desteklemez, ancak alternatif olarak 'meta veri' aşağıda açıklandığı gibi DNS kayıt kümelerinde desteklenir.

### <a name="metadata"></a>Meta Veriler

Azure DNS, ayarlanmış etiketlere alternatif olarak ,'meta veriler' kullanarak açıklama lı kayıt kümelerini destekler.  Etiketlere benzer şekilde, meta veriler ad değeri çiftleri ile her kayıt kümesini ilişkilendirmenizi sağlar.  Bu, örneğin her kayıt kümesinin amacını kaydetmek için yararlı olabilir.  Etiketlerin aksine, meta veriler Azure faturanızın filtrelenmiş görünümünü sağlamak için kullanılamaz ve Azure Kaynak Yöneticisi ilkesinde belirtilemez.

## <a name="etags"></a>Etags

İki kişinin veya iki işlemin aynı anda bir DNS kaydını değiştirmeye çalıştığını varsayalım. Hangisi kazanır? Kazanan, başka biri tarafından oluşturulan değişiklikleri üzerine yazdıklarını biliyor mu?

Azure DNS, aynı kaynakta eşzamanlı değişiklikleri güvenli bir şekilde işlemek için Etags kullanır. E-etiketler [Azure Kaynak Yöneticisi 'Etiketler'](#tags)ile ayrıdır. Her DNS kaynağının (bölge veya kayıt kümesi) bununla ilişkili bir Etag'ı vardır. Bir kaynak her zaman alınır, Etag'ı da alınır. Bir kaynağı güncellerken, Azure DNS'nin sunucudaki Etag'ın eşleştiğini doğrulayabilmesi için Etag'ı geri geçirmeyi seçebilirsiniz. Etag'ın yeniden oluşturulmasıyla sonuçlanan bir kaynak güncelleştirmesi için yapılan her güncelleştirme, eşzamanlı bir değişikliğin gerçekleştiğini gösterir. Etags, kaynağın zaten var olmadığından emin olmak için yeni bir kaynak oluştururken de kullanılabilir.

Varsayılan olarak, Azure DNS PowerShell bölgeler ve kayıt kümelerinde eşzamanlı değişiklikleri engellemek için Etags kullanır. İsteğe bağlı *-Overwrite* anahtarı, oluşan eşzamanlı değişikliklerin üzerine yazıldığı Etag denetimlerini bastırmak için kullanılabilir.

Azure DNS REST API düzeyinde, E-etiketler http üstbilgi kullanılarak belirtilir.  Davranışları aşağıdaki tabloda verilmiştir:

| Üst bilgi | Davranış |
| --- | --- |
| None |PUT her zaman başarılı olur (Etag denetimi yok) |
| Eğer-maç \<etag> |PUT yalnızca kaynak varsa ve Etag eşleşirse başarılı olur |
| If-match * |PUT yalnızca kaynak varsa başarılı olur |
| If-none-match * |PUT yalnızca kaynak yoksa başarılı olur |


## <a name="limits"></a>Sınırlar

Azure DNS kullanırken aşağıdaki varsayılan sınırlar geçerlidir:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Azure DNS'yi kullanmaya başlamak [için, bir DNS bölgesi oluşturmayı](dns-getstarted-create-dnszone-portal.md) ve [DNS kayıtları oluşturmayı](dns-getstarted-create-recordset-portal.md)öğrenin.
* Varolan bir DNS bölgesini geçirmek için, [bir DNS bölge dosyasını nasıl içe aktarıp dışa aktarılanmayı](dns-import-export.md)öğrenin.
