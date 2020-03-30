---
title: Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri
description: Ağ güvenlik gruplarınıza açıkça IP adresleri eklemeden HDInsight sistem ve yönetim hizmetleri düğümlerinden kümenize gelen trafiğin izin vermek için HDInsight hizmet etiketlerini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/10/2020
ms.openlocfilehash: a72753d5553e79a8ed28c3afcc7e54af6c2d230c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79117244"
---
# <a name="network-security-group-nsg-service-tags-for-azure-hdinsight"></a>Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri

Ağ güvenliği grupları (NSG'ler) için HDInsight hizmet etiketleri, sağlık ve yönetim hizmetleri için IP adresleri gruplarıdır. Bu gruplar, güvenlik kuralı oluşturma karmaşıklığını en aza indirmeye yardımcı olur. [Hizmet etiketleri,](../virtual-network/security-overview.md#service-tags) ağ güvenlik gruplarınızdaki [yönetim IP adreslerinin](hdinsight-management-ip-addresses.md) her birini girmeden belirli IP adreslerinden gelen trafiğe izin vermek için alternatif bir yöntem sağlar.

Bu hizmet etiketleri HDInsight hizmeti tarafından oluşturulur ve yönetilir. Kendi hizmet etiketinizi oluşturamaz veya varolan bir etiketi değiştiremezsiniz. Microsoft, hizmet etiketiyle eşleşen adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

## <a name="getting-started-with-service-tags"></a>Hizmet etiketleri ile başlarken

Ağ güvenlik gruplarınızda hizmet etiketlerini kullanmak için iki seçeneğiniz vardır:

1. Tek bir HDInsight hizmet etiketi kullanın - bu seçenek, sanal ağınızı HDInsight hizmetinin tüm bölgelerdeki kümeleri izlemek için kullandığı tüm IP Adreslerine açar. Bu seçenek en basit yöntemdir, ancak kısıtlayıcı güvenlik gereksinimleriniz varsa uygun olmayabilir.

1. Birden çok bölgesel hizmet etiketi kullanın - bu seçenek sanal ağınızı yalnızca HDInsight'ın söz konusu bölgede kullandığı IP Adreslerine açar. Ancak, birden çok bölge kullanıyorsanız, sanal ağınıza birden çok hizmet etiketi eklemeniz gerekir.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Tek bir global HDInsight hizmet etiketi kullanma

HDInsight kümenizde hizmet etiketlerini kullanmaya başlamanın en kolay `HDInsight` yolu, genel etiketi ağ güvenliği grubu kuralına eklemektir.

1. Azure [portalından](https://portal.azure.com/)ağ güvenlik grubunuzu seçin.

1. **Ayarlar**altında **Gelen güvenlik kurallarını**seçin ve sonra **+ Ekle'yi**seçin.

1. **Kaynak** açılır listesinden Hizmet **Etiketi'ni**seçin.

1. Kaynak **hizmet etiketi** açılır listesinden **HDInsight'ı**seçin.

    ![Azure portalı hizmet etiketi ekle](./media/hdinsight-service-tags/azure-portal-add-service-tag.png)

Bu etiket, HDInsight'ın mevcut olduğu tüm bölgelerin sağlık ve yönetim hizmetlerinin IP adreslerini içerir ve kümenizin nerede oluşturulurse oluşturulsun gerekli sağlık ve yönetim hizmetleriyle iletişim kurmasını sağlar.

## <a name="use-regional-hdinsight-service-tags"></a>Bölgesel HDInsight hizmet etiketlerini kullanma

Daha kısıtlayıcı izinlere ihtiyacınız olduğu için birinci seçenek işe yaramazsa, yalnızca bölgeniz için geçerli olan hizmet etiketlerine izin verebilirsiniz. İlgili hizmet etiketleri, kümenizin oluşturulduğu bölgeye bağlı olarak bir, iki veya üç hizmet etiketi olabilir.

Bölgeniz için hangi hizmet etiketlerini ekleyeceğinizi öğrenmek için belgenin aşağıdaki bölümlerini okuyun.

### <a name="use-a-single-regional-service-tag"></a>Tek bir bölgesel hizmet etiketi kullanma

Hizmet etiketi seçeneği iki'yi tercih ederseniz ve kümeniz bu tabloda listelenen bölgelerden birinde bulunuyorsa, ağ güvenlik grubunuza yalnızca tek bir bölgesel hizmet etiketi eklemeniz gerekir.

| Ülke | Bölge | Hizmet etiketi |
| ---- | ---- | ---- |
| Avustralya | Doğu Avustralya | HDInsight.AustraliaDoğu |
| &nbsp; | Güneydoğu Avustralya | HDInsight.AustraliaGüneydoğu |
| &nbsp; | Orta Avustralya | HDInsight.AustraliaCentral |
| Çin | Çin Doğu 2 | HDInsight.ChinaEast2 |
| &nbsp; | Çin Kuzey 2 | HDInsight.ChinaNorth2 |
| Amerika Birleşik Devletleri | Orta Kuzey ABD | HDInsight.NorthCentralUS |
| &nbsp; | Batı ABD 2 | HDInsight.WestUS2 |
| &nbsp; | Orta Batı ABD | HDInsight.WestCentralUS |
| Kanada | Doğu Kanada | HDInsight.CanadaDoğu |
| Brezilya | Güney Brezilya | HDInsight.BrazilSouth |
| Güney Kore | Güney Kore - Orta | HDInsight.KoreaCentral |
| &nbsp; | Güney Kore - Güney | HDInsight.KoreaSouth |
| Hindistan | Orta Hindistan | HDInsight.CentralHindistan |
| &nbsp; | Güney Hindistan | HDInsight.SouthIndia |
| Japonya | Batı Japonya | HDInsight.JapanWest |
| Fransa | Orta Fransa| HDInsight.FranceCentral |
| Birleşik Krallık | Güney Birleşik Krallık | HDInsight.UKSouth |
| Azure Kamu | USDoD Merkez   | HDInsight.USDoDCentral |
| &nbsp; | USGov Texas | HDInsight.USGovTexas |
| &nbsp; | Usdod Doğu | HDInsight.USDoDEast |
| &nbsp; | USGov Arizona | HDInsight.USGovArizona |

### <a name="use-multiple-regional-service-tags"></a>Birden çok bölgesel hizmet etiketi kullanma

Hizmet etiketi seçeneği iki'yi tercih ederseniz ve kümenizin oluşturulduğu bölge yukarıda listelenmediyse, birden çok bölgesel hizmet etiketine izin vermeniz gerekir. Birden fazla kullanma gereksinimi, çeşitli bölgeler için kaynak sağlayıcılarının düzenlenmesindeki farklılıklardan kaynaklanmaktadır.

Geri kalan bölgeler, kullandıkları bölgesel hizmet etiketlerine göre gruplara ayrılır.

#### <a name="group-1"></a>Grup 1

Kümeniz aşağıdaki tablodaki bölgelerden birinde oluşturulmuşsa, hizmet `HDInsight.WestUS` `HDInsight.EastUS` etiketlerine ve listelenen bölgesel hizmet etiketine ek olarak izin verin. Bu bölümdeki bölgeler üç hizmet etiketi gerektirir.

Örneğin, kümeniz `East US 2` bölgede oluşturulduysa, ağ güvenlik grubunuza aşağıdaki hizmet etiketlerini eklemeniz gerekir:

- `HDInsight.EastUS2`
- `HDInsight.WestUS`
- `HDInsight.EastUS`

| Ülke | Bölge | Hizmet etiketi |
| ---- | ---- | ---- |
| Amerika Birleşik Devletleri | Doğu ABD 2 | HDInsight.EastUS2 |
| &nbsp; | Orta ABD | HDInsight.CentralUS |
| &nbsp; | KuzeyOrta ABD | HDInsight. KuzeyCentralUS |
| &nbsp; | Orta Güney ABD | HDInsight.SouthCentralUS |
| &nbsp; | Doğu ABD | HDInsight.EastUS |
| &nbsp; | Batı ABD | HDInsight.WestUS |
| Japonya | Doğu Japonya | HDInsight.JapanDoğu |
| Avrupa | Kuzey Avrupa | HDInsight.NorthEurope |
| &nbsp; | Batı Avrupa| HDInsight.WestEurope |
| Asya | Doğu Asya | HDInsight.EastAsia |
| &nbsp; | Güneydoğu Asya | HDInsight.SoutheastAsia |
| Avustralya | Doğu Avustralya | HDInsight.AustraliaDoğu |

#### <a name="group-2"></a>Grup 2

**Çin Kuzey** ve **Çin Doğu**bölgelerinde Kümeler, iki hizmet `HDInsight.ChinaNorth` `HDInsight.ChinaEast`etiketleri izin vermek gerekir: ve .

#### <a name="group-3"></a>Grup 3

**ABD Gov Iowa** ve ABD **Gov Virginia**bölgelerinde Kümeler, iki `HDInsight.USGovIowa` `HDInsight.USGovVirginia`hizmet etiketleri izin vermek gerekir: ve .

#### <a name="group-4"></a>Grup 4

**Almanya Orta** ve **Almanya Kuzeydoğu**bölgelerinde kümeler, iki hizmet `HDInsight.GermanyCentral` `HDInsight.GermanyNorthEast`etiketleri izin vermek gerekir: ve .

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ güvenlik grupları - hizmet etiketleri](../virtual-network/security-overview.md#security-rules)
- [Azure HDInsight kümeleri için sanal ağlar oluşturma](hdinsight-create-virtual-network.md)
