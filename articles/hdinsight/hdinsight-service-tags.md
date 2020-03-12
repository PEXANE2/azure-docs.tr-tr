---
title: Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri
description: Ağ güvenlik gruplarınızı IP adreslerini açıkça eklemeden, HDInsight sistem durumu ve Yönetim Hizmetleri düğümlerinden kümenize gelen trafiğe izin vermek için HDInsight hizmet etiketlerini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/10/2020
ms.openlocfilehash: a72753d5553e79a8ed28c3afcc7e54af6c2d230c
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79117244"
---
# <a name="network-security-group-nsg-service-tags-for-azure-hdinsight"></a>Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri

Ağ güvenlik grupları (NSG 'ler) için HDInsight hizmet etiketleri, sistem durumu ve Yönetim Hizmetleri için IP adresi gruplarıdır. Bu gruplar, güvenlik kuralı oluşturma karmaşıklığına en aza indirmenize yardımcı olur. [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags) , ağ güvenlik gruplarınızı her BIR [Yönetim IP ADRESINI](hdinsight-management-ip-addresses.md) girmeden belirli IP adreslerinden gelen trafiğe izin vermek için alternatif bir yöntem sağlar.

Bu hizmet etiketleri HDInsight hizmeti tarafından oluşturulur ve yönetilir. Kendi hizmet etiketinizi oluşturamaz veya var olan bir etiketi değiştiremezsiniz. Microsoft, hizmet etiketiyle eşleşen adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

## <a name="getting-started-with-service-tags"></a>Service Tags ile çalışmaya başlama

Ağ güvenlik gruplarında hizmet etiketleri kullanmak için iki seçeneğiniz vardır:

1. Tek bir HDInsight hizmet etiketi kullan-bu seçenek, Sanal ağınızı tüm bölgelerdeki kümeleri izlemek için HDInsight hizmetinin kullandığı tüm IP adreslerine açar. Bu seçenek en basit yöntemdir, ancak kısıtlayıcı güvenlik gereksinimleriniz varsa uygun olmayabilir.

1. Birden çok bölgesel hizmet etiketi kullan-bu seçenek, Sanal ağınızı yalnızca HDInsight 'ın o belirli bölgede kullandığı IP adreslerine açar. Ancak, birden çok bölge kullanıyorsanız, sanal ağınıza birden çok hizmet etiketi eklemeniz gerekir.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Tek bir Global HDInsight hizmet etiketi kullanın

HDInsight kümeniz ile hizmet etiketleri kullanmaya başlamanın en kolay yolu, `HDInsight` genel etiketi bir ağ güvenlik grubu kuralına eklemektir.

1. [Azure Portal](https://portal.azure.com/)ağ güvenlik grubunuzu seçin.

1. **Ayarlar**altında **gelen güvenlik kuralları**' nı seçin ve **+ Ekle**' yi seçin.

1. **Kaynak** açılan listesinden **hizmet etiketi**' ni seçin.

1. **Kaynak hizmet etiketi** açılan listesinden **HDInsight**' ı seçin.

    ![Azure portal hizmet etiketi ekle](./media/hdinsight-service-tags/azure-portal-add-service-tag.png)

Bu etiket, HDInsight 'ın kullanılabildiği tüm bölgeler için sistem durumu ve yönetim hizmetlerinin IP adreslerini içerir ve kümenizin nerede oluşturulduğuna bakılmaksızın gerekli sistem durumu ve yönetim hizmetleriyle iletişim kurabildiğinden emin olur.

## <a name="use-regional-hdinsight-service-tags"></a>Bölgesel HDInsight hizmet etiketlerini kullanma

Daha kısıtlayıcı izinleriniz olması gerektiğinden bir seçenek işe amıyorsa, yalnızca bölgeniz için geçerli olan hizmet etiketlerine izin verebilirsiniz. Geçerli hizmet etiketleri, kümenizin oluşturulduğu bölgeye bağlı olarak bir, iki veya üç hizmet etiketi olabilir.

Bölgeniz için hangi hizmet etiketlerinin ekleneceğini öğrenmek için, belgenin aşağıdaki bölümlerini okuyun.

### <a name="use-a-single-regional-service-tag"></a>Tek bir bölgesel hizmet etiketi kullanın

İki hizmet etiketi seçeneğini tercih ediyorsanız ve kümeniz bu tabloda listelenen bölgelerden birinde bulunuyorsa, ağ güvenlik grubunuza yalnızca tek bir bölgesel hizmet etiketi eklemeniz gerekir.

| Ülke | Bölge | Hizmet etiketi |
| ---- | ---- | ---- |
| Avustralya | Doğu Avustralya | HDInsight. AustraliaEast |
| &nbsp; | Güneydoğu Avustralya | HDInsight. AustraliaSoutheast |
| &nbsp; | Orta Avustralya | HDInsight. AustraliaCentral |
| Çin | Çin Doğu 2 | HDInsight. ChinaEast2 |
| &nbsp; | Çin Kuzey 2 | HDInsight. ChinaNorth2 |
| Amerika Birleşik Devletleri | Orta Kuzey ABD | HDInsight. Kuzeydoğu ABD |
| &nbsp; | Batı ABD 2 | HDInsight. WestUS2 |
| &nbsp; | Orta Batı ABD | HDInsight. WestCentralUS |
| Kanada | Doğu Kanada | HDInsight. Canadadoğu |
| Brezilya | Güney Brezilya | HDInsight. BrazilSouth |
| Güney Kore | Güney Kore - Orta | HDInsight. KoreaCentral |
| &nbsp; | Güney Kore - Güney | HDInsight. Koreagüney |
| Hindistan | Orta Hindistan | HDInsight. merkezde Hindistan |
| &nbsp; | Güney Hindistan | HDInsight. Güneydoğu |
| Japonya | Batı Japonya | HDInsight. JapanWest |
| Fransa | Orta Fransa| HDInsight. Francecna al |
| UK | Güney Birleşik Krallık | HDInsight. UKGüney |
| Azure Kamu | USDoD orta   | HDInsight. Usdodorta |
| &nbsp; | USGov Texas | HDInsight. USGovTexas |
| &nbsp; | UsDoD Doğu | HDInsight. USDoDEast |
| &nbsp; | USGov Arizona | HDInsight. USGovArizona |

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
| Japonya | Doğu Japonya | HDInsight. JapanEast |
| Avrupa | Kuzey Avrupa | HDInsight. NorthEurope |
| &nbsp; | Batı Avrupa| HDInsight. WestEurope |
| Asya | Doğu Asya | HDInsight. Eastasıya |
| &nbsp; | Güneydoğu Asya | HDInsight. Güneydoğu |
| Avustralya | Doğu Avustralya | HDInsight. AustraliaEast |

#### <a name="group-2"></a>Grup 2

**Çin Kuzey** ve **Çin Doğu**bölgelerinde bulunan kümeler iki hizmet etiketine izin vermeniz gerekir: `HDInsight.ChinaNorth` ve `HDInsight.ChinaEast`.

#### <a name="group-3"></a>Grup 3

**US gov Iowa** ve **US gov Virginia**bölgelerinde bulunan kümeler iki hizmet etiketine izin vermeniz gerekir: `HDInsight.USGovIowa` ve `HDInsight.USGovVirginia`.

#### <a name="group-4"></a>Grup 4

**Almanya Orta** ve **Almanya kuzeydoğu**bölgelerinde bulunan kümeler iki hizmet etiketine izin vermeniz gerekir: `HDInsight.GermanyCentral` ve `HDInsight.GermanyNorthEast`.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ güvenlik grupları-hizmet etiketleri](../virtual-network/security-overview.md#security-rules)
- [Azure HDInsight kümeleri için sanal ağlar oluşturma](hdinsight-create-virtual-network.md)
