---
title: Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri
description: Ağ güvenlik gruplarınızı IP adreslerini açıkça eklemeden, HDInsight sistem durumu ve Yönetim Hizmetleri düğümlerinden kümenize gelen trafiğe izin vermek için HDInsight hizmet etiketlerini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/19/2019
ms.openlocfilehash: 7e3ce33bdf0773ababe5eb190877a9288c094c5c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74187091"
---
# <a name="network-security-group-nsg-service-tags-for-azure-hdinsight"></a>Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri

Ağ güvenlik grupları (NSG 'ler) için HDInsight hizmet etiketleri, sistem durumu ve Yönetim Hizmetleri için IP adresi gruplarıdır. Bu gruplar, güvenlik kuralı oluşturma karmaşıklığına en aza indirmenize yardımcı olur. [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags) , ağ güvenlik gruplarınızı her BIR [Yönetim IP ADRESINI](hdinsight-management-ip-addresses.md) girmeden belirli IP adreslerinden gelen trafiğe izin vermek için alternatif bir yöntem sağlar.

Bu hizmet etiketleri HDInsight hizmeti tarafından oluşturulur ve yönetilir. Kendi hizmet etiketinizi oluşturamaz veya var olan bir etiketi değiştiremezsiniz. Microsoft, hizmet etiketiyle eşleşen adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

## <a name="getting-started-with-service-tags"></a>Service Tags ile çalışmaya başlama

Ağ güvenlik gruplarında hizmet etiketleri kullanmak için iki seçeneğiniz vardır:

1. Tek bir HDInsight hizmet etiketi kullan-bu seçenek, Sanal ağınızı tüm bölgelerdeki kümeleri izlemek için HDInsight hizmetinin kullandığı tüm IP adreslerine açar. Bu seçenek en basit yöntemdir, ancak kısıtlayıcı güvenlik gereksinimleriniz varsa uygun olmayabilir.

1. Birden çok bölgesel hizmet etiketi kullan-bu seçenek, Sanal ağınızı yalnızca HDInsight 'ın o belirli bölgede kullandığı IP adreslerine açar. Ancak, birden çok bölge kullanıyorsanız, sanal ağınıza birden çok hizmet etiketi eklemeniz gerekir.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Tek bir Global HDInsight hizmet etiketi kullanın

HDInsight kümeniz ile hizmet etiketleri kullanmaya başlamanın en kolay yolu, `HDInsight` genel etiketi bir ağ güvenlik grubu kuralına eklemektir. Ağ güvenlik grubunuza hizmet etiketleri ekleme hakkında yönergeler için bkz. [güvenlik grupları: hizmet etiketleri](../virtual-network/security-overview.md#service-tags).

Bu etiket, HDInsight 'ın kullanılabildiği tüm bölgeler için sistem durumu ve yönetim hizmetlerinin IP adreslerini içerir ve kümenizin nerede oluşturulduğuna bakılmaksızın gerekli sistem durumu ve yönetim hizmetleriyle iletişim kurabildiğinden emin olur.

## <a name="use-regional-hdinsight-service-tags"></a>Bölgesel HDInsight hizmet etiketlerini kullanma

Daha kısıtlayıcı izinleriniz olması gerektiğinden bir seçenek işe amıyorsa, yalnızca bölgeniz için geçerli olan hizmet etiketlerine izin verebilirsiniz. Geçerli hizmet etiketleri, kümenizin oluşturulduğu bölgeye bağlı olarak bir, iki veya üç hizmet etiketi olabilir.

Bölgeniz için hangi hizmet etiketlerinin ekleneceğini öğrenmek için, belgenin aşağıdaki bölümlerini okuyun.

### <a name="use-a-single-regional-service-tag"></a>Tek bir bölgesel hizmet etiketi kullanın

İki hizmet etiketi seçeneğini tercih ediyorsanız ve kümeniz bu tabloda listelenen bölgelerden birinde bulunuyorsa, ağ güvenlik grubunuza yalnızca tek bir bölgesel hizmet etiketi eklemeniz gerekir.

| Ülke | Bölge | Hizmet etiketi |
| ---- | ---- | ---- |
| Avustralya | Avustralya Doğu | HDInsight. AustraliaEast |
| &nbsp; | Avustralya Güneydoğu | HDInsight. AustraliaSoutheast |
| &nbsp; | Avustralya Orta | HDInsight. AustraliaCentral |
| Çin | Çin Doğu 2 | HDInsight. ChinaEast2 |
| &nbsp; | Çin Kuzey 2 | HDInsight. ChinaNorth2 |
| Amerika Birleşik Devletleri | Orta Kuzey ABD | HDInsight. Kuzeydoğu ABD |
| &nbsp; | Batı ABD 2 | HDInsight. WestUS2 |
| &nbsp; | Orta Batı ABD | HDInsight. WestCentralUS |
| Kanada | Doğu Kanada | HDInsight. Canadadoğu |
| Brezilya | Güney Brezilya | HDInsight. BrazilSouth |
| Güney Kore | Kore Orta | HDInsight. KoreaCentral |
| &nbsp; | Kore Güney | HDInsight. Koreagüney |
| Hindistan | Orta Hindistan | HDInsight. merkezde Hindistan |
| &nbsp; | Güney Hindistan | HDInsight. Güneydoğu |
| Japonya | Japonya Batı | HDInsight. JapanWest |
| Fransa | Fransa Orta| HDInsight. Francecna al |
| UK | Birleşik Krallık Güney | HDInsight. UKGüney |
| Azure Kamu (Fairfax) | USDoD orta   | HDInsight. Usdodorta |
| &nbsp; | USGov Texas | HDInsight. USGovTexas |
| &nbsp; | UsDoD Doğu | HDInsight. USDoDEast |

### <a name="use-multiple-regional-service-tags"></a>Birden çok bölgesel hizmet etiketi kullanma

İki hizmet etiketi seçeneğini tercih ediyorsanız ve kümenizin oluşturulduğu bölge yukarıda listelenmediyse, birden çok bölgesel hizmet etiketine izin vermeniz gerekir. Farklı bölgelere yönelik kaynak sağlayıcılarının düzenleme farklılıklarından çok daha fazla kullanım olması gerekir.

Kalan bölgeler, kullandıkları bölgesel hizmet etiketlerine göre gruplara ayrılır.

#### <a name="group-1"></a>1\. Grup

Kümeniz aşağıdaki tablodaki bölgelerden birinde oluşturulduysa, listelenen bölge hizmeti etiketinin yanı sıra hizmet etiketlerine `HDInsight.WestUS` ve `HDInsight.EastUS` izin verin. Bu bölümdeki bölgeler üç hizmet etiketi gerektirir.

Örneğin, kümeniz `East US 2` bölgesinde oluşturulduysa, ağ güvenlik grubunuza aşağıdaki hizmet etiketlerini eklemeniz gerekir:

- `HDInsight.EastUS2`
- `HDInsight.WestUS`
- `HDInsight.EastUS`

| Ülke | Bölge | Hizmet etiketi |
| ---- | ---- | ---- |
| Amerika Birleşik Devletleri | Doğu ABD 2 | HDInsight. EastUS2 |
| &nbsp; | Orta ABD | HDInsight. merkezde ABD |
| &nbsp; | Kuzey Orta ABD | 'Tan. Kuzeydoğu ABD |
| &nbsp; | Orta Güney ABD | HDInsight. Güneydoğu ABD |
| &nbsp; | Doğu ABD | HDInsight. EastUS |
| &nbsp; | Batı ABD | HDInsight. WestUS |
| Japonya | Japonya Doğu | HDInsight. JapanEast |
| Avrupa | Kuzey Avrupa | HDInsight. NorthEurope |
| &nbsp; | Batı Avrupa| HDInsight. WestEurope |
| Asya | Doğu Asya | HDInsight. Eastasıya |
| &nbsp; | Güneydoğu Asya | HDInsight. Güneydoğu |
| Avustralya | Avustralya Doğu | HDInsight. AustraliaEast |

#### <a name="group-2"></a>Grup 2

**Çin Kuzey** ve **Çin Doğu**bölgelerinde bulunan kümeler iki hizmet etiketine izin vermeniz gerekir: `HDInsight.ChinaNorth` ve `HDInsight.ChinaEast`.

#### <a name="group-3"></a>Grup 3

**US gov Iowa** ve **US gov Virginia**bölgelerinde bulunan kümeler iki hizmet etiketine izin vermeniz gerekir: `HDInsight.USGovIowa` ve `HDInsight.USGovVirginia`.

#### <a name="group-4"></a>Grup 4

**Almanya Orta** ve **Almanya kuzeydoğu**bölgelerinde bulunan kümeler iki hizmet etiketine izin vermeniz gerekir: `HDInsight.GermanyCentral` ve `HDInsight.GermanyNorthEast`.

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ güvenlik grupları-hizmet etiketleri](../virtual-network/security-overview.md#security-rules)
* [Azure HDInsight kümeleri için sanal ağlar oluşturma](hdinsight-create-virtual-network.md)
