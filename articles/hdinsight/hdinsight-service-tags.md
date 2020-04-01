---
title: Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri
description: NSG'lerinize IP adresleri eklemeden, sağlık ve yönetim hizmetleri düğümlerinden kümenize gelen trafiğin izin vermek için HDInsight hizmet etiketlerini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/10/2020
ms.openlocfilehash: 34ec05a8362f5947cb61924b19c6b1a52e5d91a4
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437668"
---
# <a name="nsg-service-tags-for-azure-hdinsight"></a>Azure HDInsight için NSG hizmet etiketleri

Ağ güvenliği grupları (NSG'ler) için Azure HDInsight hizmet etiketleri, sağlık ve yönetim hizmetleri için IP adresleri gruplarıdır. Bu gruplar, güvenlik kuralı oluşturma karmaşıklığını en aza indirmeye yardımcı olur. [Hizmet etiketleri,](../virtual-network/security-overview.md#service-tags) NSG'lerinizdeki [yönetim IP adreslerinin](hdinsight-management-ip-addresses.md) her birini girmeden belirli IP adreslerinden gelen trafiğe izin vermek için alternatif bir yöntem sağlar.

HDInsight hizmeti bu hizmet etiketlerini yönetir. Kendi hizmet etiketinizi oluşturamaz veya varolan bir etiketi değiştiremezsiniz. Microsoft, hizmet etiketiyle eşleşen adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

## <a name="get-started-with-service-tags"></a>Hizmet etiketleri ile başlayın

Ağ güvenlik gruplarınızda hizmet etiketlerini kullanmak için iki seçeneğiniz vardır:

- **Tek bir global HDInsight hizmet etiketi kullanın**: Bu seçenek, sanal ağınızı HDInsight hizmetinin tüm bölgelerdeki kümeleri izlemek için kullandığı tüm IP adreslerine açar. Bu seçenek en basit yöntemdir, ancak kısıtlayıcı güvenlik gereksinimleriniz varsa uygun olmayabilir.

- **Birden çok bölgesel hizmet etiketi kullanın**: Bu seçenek sanal ağınızı yalnızca HDInsight'ın söz konusu bölgede kullandığı IP adreslerine açar. Ancak, birden çok bölge kullanıyorsanız, sanal ağınıza birden çok hizmet etiketi eklemeniz gerekir.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Tek bir global HDInsight hizmet etiketi kullanma

HDInsight kümenizde hizmet etiketlerini kullanmaya başlamanın en kolay `HDInsight` yolu, genel etiketi bir NSG kuralına eklemektir.

1. Azure [portalından](https://portal.azure.com/)ağ güvenlik grubunuzu seçin.

1. **Ayarlar**altında **Gelen güvenlik kurallarını**seçin ve sonra **+ Ekle'yi**seçin.

1. **Kaynak** açılır listesinden Hizmet **Etiketi'ni**seçin.

1. Kaynak **hizmet etiketi** açılır listesinden **HDInsight'ı**seçin.

    ![Azure portalından hizmet etiketi ekleme](./media/hdinsight-service-tags/azure-portal-add-service-tag.png)

Bu etiket, HDInsight'ın kullanılabildiği tüm bölgeler için sağlık ve yönetim hizmetlerinin IP adreslerini içerir. Etiket, kümenizin nerede oluşturulursa oluşturulsun gerekli sağlık ve yönetim hizmetleriyle iletişim kurmasını sağlar.

## <a name="use-regional-hdinsight-service-tags"></a>Bölgesel HDInsight hizmet etiketlerini kullanma

Daha kısıtlayıcı izinlere ihtiyacınız olduğundan genel etiket seçeneği çalışmazsa, yalnızca bölgeniz için geçerli olan hizmet etiketlerine izin verebilirsiniz. Kümenizin oluşturulduğu bölgeye bağlı olarak bir, iki veya üç geçerli hizmet etiketi olabilir.

Bölgeniz için hangi hizmet etiketlerini ekleyeceğinizi öğrenmek için makalenin aşağıdaki bölümlerini okuyun.

### <a name="use-a-single-regional-service-tag"></a>Tek bir bölgesel hizmet etiketi kullanma

Bölgesel hizmet etiketleri kullanmayı tercih ediyorsanız ve kümeniz bu tabloda listelenen bölgelerden birinde bulunuyorsa, ağ güvenlik grubunuza yalnızca tek bir bölgesel hizmet etiketi eklemeniz gerekir.

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
| Azure Kamu | USDoD Merkez | HDInsight.USDoDCentral |
| &nbsp; | USGov Texas | HDInsight.USGovTexas |
| &nbsp; | Usdod Doğu | HDInsight.USDoDEast |
| &nbsp; | USGov Arizona | HDInsight.USGovArizona |

### <a name="use-multiple-regional-service-tags"></a>Birden çok bölgesel hizmet etiketi kullanma

Bölgesel hizmet etiketleri kullanmayı tercih ediyorsanız, ancak kümenizin oluşturulduğu bölge önceki tabloda listelenmediyse, birden çok bölgesel hizmet etiketine izin vermeniz gerekir. Birden fazla kullanma gereksinimi, çeşitli bölgeler için kaynak sağlayıcılarının düzenlenmesindeki farklılıklardan kaynaklanmaktadır.

Geri kalan bölgeler, kullandıkları bölgesel hizmet etiketlerine göre gruplara ayrılır.

#### <a name="group-1"></a>Grup 1

Kümeniz aşağıdaki tablodaki bölgelerden birinde oluşturulduysa, hizmet `HDInsight.WestUS` etiketlerine ve `HDInsight.EastUS` listelenen bölgesel hizmet etiketine ek olarak izin verin. Bu bölümdeki bölgeler üç hizmet etiketi gerektirir.

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

*Çin Kuzey* ve *Çin Doğu* bölgelerinde kümeler iki hizmet `HDInsight.ChinaNorth` `HDInsight.ChinaEast`etiketleri izin vermek gerekir: ve .

#### <a name="group-3"></a>Grup 3

*ABD Gov Iowa* ve ABD *Gov Virginia* bölgelerinde kümeler iki `HDInsight.USGovIowa` `HDInsight.USGovVirginia`hizmet etiketleri izin vermek gerekir: ve .

#### <a name="group-4"></a>Grup 4

*Almanya Orta* ve *Almanya Kuzeydoğu* bölgelerinde kümeler iki hizmet `HDInsight.GermanyCentral` `HDInsight.GermanyNortheast`etiketleri izin vermek gerekir: ve .

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ güvenlik grupları: hizmet etiketleri](../virtual-network/security-overview.md#security-rules)
- [Azure HDInsight kümeleri için sanal ağlar oluşturma](hdinsight-create-virtual-network.md)
