---
title: Azure HDInsight için ağ güvenlik grubu (NSG) hizmet etiketleri
description: NSG 'lerinize IP adresleri eklemeden, sistem durumu ve Yönetim Hizmetleri düğümlerinden kümenize gelen trafiğe izin vermek için HDInsight hizmet etiketlerini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: 5608d0cd83e506bc6b30337db5148f344f59f80e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81410866"
---
# <a name="nsg-service-tags-for-azure-hdinsight"></a>Azure HDInsight için NSG hizmet etiketleri

Ağ güvenlik grupları (NSG 'ler) için Azure HDInsight hizmet etiketleri, sistem durumu ve Yönetim Hizmetleri için IP adresi gruplarıdır. Bu gruplar, güvenlik kuralı oluşturma karmaşıklığına en aza indirmenize yardımcı olur. [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags) , NSG 'lerinize her BIR [Yönetim IP adresini](hdinsight-management-ip-addresses.md) girmeden belirli IP 'lerden gelen trafiğe izin verir.

HDInsight hizmeti bu hizmet etiketlerini yönetir. Kendi hizmet etiketinizi oluşturamaz veya var olan bir etiketi değiştiremezsiniz. Microsoft, hizmet etiketiyle eşleşen adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

## <a name="get-started-with-service-tags"></a>Hizmet etiketlerini kullanmaya başlama

Ağ güvenlik gruplarında hizmet etiketleri kullanmak için iki seçeneğiniz vardır:

- **Tek bir Global HDInsight hizmet etiketi kullan**: Bu seçenek, Sanal ağınızı HDInsight hizmetinin tüm bölgelerde kümeleri izlemek için KULLANDıĞı tüm IP adreslerine açar. Bu seçenek en basit yöntemdir, ancak kısıtlayıcı güvenlik gereksinimleriniz varsa uygun olmayabilir.

- **Birden çok bölgesel hizmet etiketi kullan**: Bu seçenek, Sanal ağınızı yalnızca HDInsight 'ın o belirli bölgede kullandığı IP adreslerine açar. Ancak, birden çok bölge kullanıyorsanız, sanal ağınıza birden çok hizmet etiketi eklemeniz gerekir.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Tek bir Global HDInsight hizmet etiketi kullanın

HDInsight kümeniz ile hizmet etiketleri kullanmaya başlamanın en kolay yolu, genel etiketi `HDInsight` bir NSG kuralına eklemektir.

1. [Azure Portal](https://portal.azure.com/)ağ güvenlik grubunuzu seçin.

1. **Ayarlar**altında **gelen güvenlik kuralları**' nı seçin ve **+ Ekle**' yi seçin.

1. **Kaynak** açılan listesinden **hizmet etiketi**' ni seçin.

1. **Kaynak hizmet etiketi** açılan listesinden **HDInsight**' ı seçin.

    ![Azure portal bir hizmet etiketi ekleyin](./media/hdinsight-service-tags/azure-portal-add-service-tag.png)

Bu etiket, HDInsight 'ın kullanılabildiği tüm bölgeler için sistem durumu ve yönetim hizmetlerinin IP adreslerini içerir. Etiketi, kümenizin nerede oluşturulduğuna bakılmaksızın gerekli sistem durumu ve yönetim hizmetleriyle iletişim kurabildiğinden emin olur.

## <a name="use-regional-hdinsight-service-tags"></a>Bölgesel HDInsight hizmet etiketlerini kullanma

Daha kısıtlayıcı izinleriniz olması gerektiğinden genel etiket seçeneği çalışmazsa, yalnızca bölgeniz için geçerli olan hizmet etiketlerine izin verebilirsiniz. Kümenizin oluşturulduğu bölgeye bağlı olarak birden çok hizmet etiketi olabilir.

Bölgeniz için hangi hizmet etiketlerinin ekleneceğini öğrenmek için makalenin aşağıdaki bölümlerini okuyun.

### <a name="use-a-single-regional-service-tag"></a>Tek bir bölgesel hizmet etiketi kullanın

Kümeniz bu tabloda listelenen bir bölgede bulunuyorsa, NSG 'nize yalnızca tek bir bölgesel hizmet etiketi eklemeniz gerekir.

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
| Birleşik Krallık | Güney Birleşik Krallık | HDInsight. UKGüney |
| Azure Kamu | USDoD orta | HDInsight. Usdodorta |
| &nbsp; | USGov Texas | HDInsight. USGovTexas |
| &nbsp; | UsDoD Doğu | HDInsight. USDoDEast |
| &nbsp; | USGov Arizona | HDInsight. USGovArizona |

### <a name="use-multiple-regional-service-tags"></a>Birden çok bölgesel hizmet etiketi kullanma

Kümenizin oluşturulduğu bölge yukarıdaki tabloda listelenmiyorsa, birden çok bölgesel hizmet etiketine izin vermeniz gerekir. Çeşitli bölgelere yönelik kaynak sağlayıcılarının düzenleme farklarından çok daha fazlasını kullanma ihtiyacı vardır.

Kalan bölgeler, kullandıkları bölgesel hizmet etiketlerine göre gruplara ayrılır.

#### <a name="group-1"></a>Grup 1

Kümeniz aşağıdaki tablodaki bölgelerden birinde oluşturulduysa, hizmet etiketlerine `HDInsight.WestUS` izin verin ve. `HDInsight.EastUS` Ayrıca, bölgesel hizmet etiketi de listelenir. Bu bölümdeki bölgeler üç hizmet etiketi gerektirir.

Örneğin, kümeniz `East US 2` bölgede oluşturulduysa, ağ güvenlik grubunuza aşağıdaki hizmet etiketlerini eklemeniz gerekir:

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

*Çin Kuzey* ve *Çin Doğu* bölgelerindeki kümelerin iki hizmet etiketine izin vermek gerekir: `HDInsight.ChinaNorth` ve `HDInsight.ChinaEast`.

#### <a name="group-3"></a>Grup 3

*US gov Iowa* ve *US gov Virginia* bölgelerindeki kümelerin iki hizmet etiketine izin vermek gerekir: `HDInsight.USGovIowa` ve `HDInsight.USGovVirginia`.

#### <a name="group-4"></a>Grup 4

*Almanya Orta* ve *Almanya kuzeydoğu* bölgelerindeki kümelerin iki hizmet etiketine izin vermek gerekir: `HDInsight.GermanyCentral` ve `HDInsight.GermanyNortheast`.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ güvenlik grupları: hizmet etiketleri](../virtual-network/security-overview.md#security-rules)
- [Azure HDInsight kümeleri için sanal ağlar oluşturma](hdinsight-create-virtual-network.md)
