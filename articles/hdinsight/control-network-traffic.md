---
title: Azure HDInsight 'ta ağ trafiğini denetleme
description: Azure HDInsight kümelerine gelen ve giden trafiği denetleme tekniklerini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: a33bc5816ded7cdca75737b02add0a6ca8821700
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400203"
---
# <a name="control-network-traffic-in-azure-hdinsight"></a>Azure HDInsight 'ta ağ trafiğini denetleme

Bir Azure sanal ağlarındaki ağ trafiği aşağıdaki yöntemler kullanılarak denetlenebilir:

* **Ağ güvenlik grupları** (NSG) ağa gelen ve giden trafiği filtrelemenize izin verir. Daha fazla bilgi için bkz. ağ [güvenlik grupları ile ağ trafiğini filtreleme](../virtual-network/security-overview.md) belgesi.

* **Ağ sanal** gereçleri (NVA), yalnızca giden trafikle birlikte kullanılabilir. NVA 'lar güvenlik duvarları ve yönlendiriciler gibi cihazların işlevlerini çoğaltın. Daha fazla bilgi için bkz. [ağ araçları](https://azure.microsoft.com/solutions/network-appliances) belgesi.

Yönetilen bir hizmet olarak, HDInsight, VNET 'ten gelen ve giden trafik için HDInsight sistem durumu ve Yönetim Hizmetleri için sınırsız erişim gerektirir. NSG 'leri kullanırken, bu hizmetlerin HDInsight kümesiyle iletişim kurabildiğinden emin olmanız gerekir.

![Azure özel VNET 'te oluşturulan HDInsight varlıklarının diyagramı](./media/control-network-traffic/hdinsight-vnet-diagram.png)

## <a name="hdinsight-with-network-security-groups"></a>Ağ güvenlik gruplarıyla HDInsight

Ağ trafiğini denetlemek için **ağ güvenlik grupları** kullanmayı planlıyorsanız, HDInsight 'ı yüklemeden önce aşağıdaki eylemleri gerçekleştirin:

1. HDInsight için kullanmayı planladığınız Azure bölgesini belirler.

2. Bölgeniz için HDInsight tarafından gereken hizmet etiketlerini belirler. Bu hizmet etiketlerini almanın birden çok yolu vardır:
    1. [Azure HDInsight Için ağ güvenlik grubu (NSG) hizmet etiketlerindeki](hdinsight-service-tags.md)yayımlanmış hizmet etiketleri listesine başvurun. 
    2. Bölgeniz listede yoksa, bölgeniz için bir hizmet etiketi bulmak üzere [hizmet etiketi bulma API](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview) 'sini kullanın.
    3. API 'yi kullandıysanız, [hizmet etıketı json dosyasını](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files) indirin ve istediğiniz bölgeyi arayın.


3. HDInsight 'ı yüklemeyi planladığınız alt ağ için ağ güvenlik grupları oluşturun veya değiştirin.

    * __Ağ güvenlik grupları__: ıp adreslerinden __443__ numaralı bağlantı noktasında __gelen__ trafiğe izin verin. Bu, HDInsight Yönetim Hizmetleri 'nin kümeye sanal ağ dışından ulaşmasını güvence altına alabilir. __Kafka Rest proxy__ etkinleştirilmiş kümeler için bağlantı noktası __9400__ ' de __gelen__ trafiğe de izin verin. Bu, Kafka REST proxy sunucusuna ulaşılacağından emin olur.

Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md).

## <a name="controlling-outbound-traffic-from-hdinsight-clusters"></a>HDInsight kümelerinden giden trafiği denetleme

HDInsight kümelerinden giden trafiği denetleme hakkında daha fazla bilgi için bkz. [Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını yapılandırma](hdinsight-restrict-outbound-traffic.md).

### <a name="forced-tunneling-to-on-premises"></a>Şirket içinde Zorlamalı tünel

Zorlamalı tünel, bir alt ağdan gelen tüm trafiğin, şirket içi ağınız veya güvenlik duvarınız gibi belirli bir ağa veya konuma zorlandığında Kullanıcı tanımlı bir yönlendirme yapılandırmadır. Büyük miktarda veri aktarımı ve olası performans etkisi nedeniyle, şirket içi olarak tüm veri aktarımının zorla tünellemesi _önerilmez._

Zorlamalı tüneli ayarlama ile ilgilenen müşteriler [özel meta](./hdinsight-use-external-metadata-stores.md) veri kullanmalıdır ve küme alt ağından veya şirket içi ağdan bu özel meta veri bağlantısı için uygun bağlantıyı ayarlamanıza olanak sağlar.

Azure Güvenlik Duvarı ile UDR kurulumunun bir örneğini görmek için bkz. [Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını yapılandırma](hdinsight-restrict-outbound-traffic.md).

## <a name="required-ports"></a>Gerekli bağlantı noktaları

Bir **güvenlik duvarı** kullanmayı ve kümeye belirli bağlantı noktalarında dışından erişmeyi planlıyorsanız senaryonuz için gereken bağlantı noktalarında trafiğe izin vermeniz gerekebilir. Varsayılan olarak, önceki bölümde açıklanan Azure Yönetim trafiğinin 443 numaralı bağlantı noktasında kümeye erişmesine izin verildiği sürece bağlantı noktalarının özel bir beyaz listesi gerekli değildir.

Belirli hizmetlere yönelik bağlantı noktalarının listesi için bkz. [HDInsight 'ta Apache Hadoop Services tarafından kullanılan bağlantı noktaları](hdinsight-hadoop-port-settings-for-services.md) .

Sanal gereçler güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [Sanal Gereç senaryo](../virtual-network/virtual-network-scenario-udr-gw-nva.md) belgesi.

## <a name="next-steps"></a>Sonraki adımlar

* Kod örnekleri ve Azure sanal ağları oluşturma örnekleri için bkz. [Azure HDInsight kümeleri için sanal ağlar oluşturma](hdinsight-create-virtual-network.md).
* HDInsight 'ı şirket içi ağa bağlanacak şekilde yapılandırmaya yönelik uçtan uca bir örnek için bkz. HDInsight 'ı Şirket [içi ağa bağlama](./connect-on-premises-network.md).
* Azure sanal ağları hakkında daha fazla bilgi için bkz. [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md).
* Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grupları](../virtual-network/security-overview.md).
* Kullanıcı tanımlı rotalar hakkında daha fazla bilgi için bkz. [Kullanıcı tanımlı rotalar ve IP iletimi](../virtual-network/virtual-networks-udr-overview.md).
* Sanal ağlar hakkında daha fazla bilgi için bkz. [HDInsight Için plan VNET](./hdinsight-plan-virtual-network-deployment.md).
