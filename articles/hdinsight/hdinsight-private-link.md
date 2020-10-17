---
title: Azure HDInsight kümelerini özel bağlantıyla güvenli hale getirme ve yalıtma (Önizleme)
description: Azure özel bağlantısını kullanarak bir sanal ağda Azure HDInsight kümelerini yalıtmak hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: d0ee9680a6b1b7c3e145137c73dda84d1a755b06
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92147908"
---
# <a name="secure-and-isolate-azure-hdinsight-clusters-with-private-link-preview"></a>Azure HDInsight kümelerini özel bağlantıyla güvenli hale getirme ve yalıtma (Önizleme)

Azure HDInsight 'ın [varsayılan sanal ağ mimarisinde](./hdinsight-virtual-network-architecture.md), HDInsight kaynak sağlayıcısı (RP) ortak IP adreslerini kullanarak kümeyle iletişim kurar. Bazı senaryolar, genel IP adresleri kullanmadan tamamen ağ yalıtımı gerektirir. Bu makalede, özel bir HDInsight kümesi oluşturmak için kullanabileceğiniz gelişmiş denetimler hakkında bilgi edineceksiniz. Ağ yalıtımına sahip olmaksızın kümeinizden gelen ve giden trafiği kısıtlama hakkında bilgi için bkz. [Azure HDInsight 'ta ağ trafiğini denetleme](./control-network-traffic.md).

Bir Azure Resource Manager (ARM) şablonunda belirli ağ özelliklerini yapılandırarak özel HDInsight kümeleri oluşturabilirsiniz. Özel HDInsight kümeleri oluşturmak için kullandığınız iki özellik vardır:

* Genel IP adreslerini `resourceProviderConnection` giden olarak ayarlayarak kaldırın.
* Azure özel bağlantısını etkinleştirin ve etkin olarak ayarlayarak [Özel uç noktaları](../private-link/private-endpoint-overview.md) kullanın `privateLink` .

## <a name="remove-public-ip-addresses"></a>Genel IP adreslerini kaldır

Varsayılan olarak, HDInsight RP genel IP 'Leri kullanarak kümeye *gelen* bir bağlantı kullanır. `resourceProviderConnection`Ağ özelliği *giden*olarak ayarlandığında, bağlantıların her zaman RP 'den RP 'ye BAŞLATıLMASı için HDInsight RP bağlantılarını tersine çevirir. Gelen bağlantı olmadan, gelen hizmet etiketlerine veya genel IP adreslerine gerek yoktur.

Varsayılan sanal ağ mimarisinde kullanılan temel yük dengeleyiciler, HDInsight RP gibi gerekli giden bağımlılıklara erişmek için otomatik olarak ortak NAT (ağ adresi çevirisi) sağlar. Genel internet 'e giden bağlantıyı kısıtlamak istiyorsanız [bir güvenlik duvarı yapılandırabilirsiniz](./hdinsight-restrict-outbound-traffic.md), ancak bu bir gereksinim değildir.

Ayrıca, giden olarak yapılandırmak, `resourceProviderConnection` Özel uç noktalar kullanarak Azure Data Lake Storage 2. veya dış meta veri gibi kümeye özgü kaynaklara erişmenize olanak tanır. HDInsight kümesini oluşturmadan önce özel uç noktaları ve DNS girdilerini yapılandırmanız gerekir. Küme oluşturma sırasında Apache Ranger, ambarı, Oozie ve Hive metastores gibi tüm dış SQL veritabanlarını oluşturmanızı ve sağlamanızı öneririz.

Azure Key Vault için özel uç noktalar desteklenmez. Rest 'de CMK şifrelemesi için Azure Key Vault kullanıyorsanız, Azure Key Vault uç noktası, HDInsight alt ağının içinden özel uç nokta olmadan erişilebilir olmalıdır.

Aşağıdaki diyagramda, olası bir HDInsight sanal ağ mimarisinin `resourceProviderConnection` , giden olarak ayarlandığı zaman nasıl görünebileceği gösterilmektedir:

:::image type="content" source="media/hdinsight-private-link/outbound-resource-provider-connection-only.png" alt-text="Giden kaynak sağlayıcısı bağlantısı kullanan HDInsight mimarisi diyagramı":::

Kümenizi oluşturduktan sonra, doğru DNS çözümlemesi ayarlamanız gerekir. Aşağıdaki kurallı ad DNS kaydı (CNAME), Azure tarafından yönetilen genel DNS bölgesinde oluşturulmuştur: `azurehdinsight.net` .

```dns
<clustername>    CNAME    <clustername>-int
```

Küme FQDN 'lerini kullanarak kümeye erişmek için doğrudan iç yük dengeleyici özel IP 'lerini kullanabilir veya kendi Özel DNS bölgenizi kullanarak küme uç noktalarını gereksinimlerinize uygun şekilde geçersiz kılabilirsiniz. Örneğin, bir Özel DNS bölgenize sahip olabilirsiniz `azurehdinsight.net` . ve gereken özel IP 'Leri ekleyin:

```dns
<clustername>        A   10.0.0.1
<clustername-ssh>    A   10.0.0.2
```

## <a name="enable-private-link"></a>Özel bağlantıyı etkinleştir

Varsayılan olarak devre dışı bırakılan özel bağlantı, bir kümeyi oluşturmadan önce Kullanıcı tanımlı yollar (UDR) ve güvenlik duvarı kurallarını düzgün bir şekilde ayarlamak için kapsamlı ağ bilgisi gerektirir. Kümeye özel bağlantı erişimi yalnızca, `resourceProviderConnection` önceki bölümde açıklandığı gibi ağ özelliği *giden* olarak ayarlandığında kullanılabilir.

`privateLink` *Etkin*olarak ayarlandığında, iç [Standart yük dengeleyiciler](../load-balancer/load-balancer-overview.md) (SLB) oluşturulur ve her SLB Için bir Azure özel bağlantı hizmeti sağlanır. Özel bağlantı hizmeti, HDInsight kümesine özel uç noktalardan erişmenizi sağlar.

Standart yük dengeleyiciler, temel yük dengeleyiciler gibi [genel gıden NAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) 'yi otomatik olarak sağlamaz. Giden bağımlılıklar için [sanal ağ NAT](../virtual-network/nat-overview.md) veya bir [güvenlik DUVARı](./hdinsight-restrict-outbound-traffic.md)gibi kendi NAT çözümünüzü sağlamanız gerekir. HDInsight kümenizin giden bağımlılıklarına hala erişmesi gerekiyor. Bu giden bağımlılıklara izin verilmiyorsa, küme oluşturma başarısız olabilir.

### <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Aşağıdaki diyagramda bir küme oluşturmadan önce gereken ağ yapılandırması örneği gösterilmektedir. Bu örnekte, tüm giden trafik UDR kullanılarak Azure Güvenlik Duvarı 'na [zorlanır](../firewall/forced-tunneling.md) ve bir küme oluşturmadan önce güvenlik duvarında gerekli giden bağımlılıkların "izin verildi" olması gerekir. Kurumsal Güvenlik Paketi kümeler için, Azure Active Directory Domain Services ağ bağlantısı VNet eşlemesi tarafından sağlanarak yapılabilir.

:::image type="content" source="media/hdinsight-private-link/before-cluster-creation.png" alt-text="Giden kaynak sağlayıcısı bağlantısı kullanan HDInsight mimarisi diyagramı":::

Ağı ayarladıktan sonra, aşağıdaki şekilde gösterildiği gibi giden kaynak sağlayıcısı bağlantısı ve özel bağlantı etkin bir küme oluşturabilirsiniz. Bu yapılandırmada, her standart yük dengeleyici için genel IP 'Ler ve özel bağlantı hizmeti sağlanmamıştır.

:::image type="content" source="media/hdinsight-private-link/after-cluster-creation.png" alt-text="Giden kaynak sağlayıcısı bağlantısı kullanan HDInsight mimarisi diyagramı":::

### <a name="access-a-private-cluster"></a>Özel bir kümeye erişme

Özel kümelere erişmek için, doğrudan iç yük dengeleyici özel IP 'Leri veya özel bağlantı DNS uzantıları ve özel uç noktaları kullanabilirsiniz. `privateLink`Ayar etkin olarak ayarlandığında, kendi özel uç noktalarınızı oluşturabilir ve özel DNS bölgeleri aracılığıyla DNS çözümlemesini yapılandırabilirsiniz.

Azure tarafından yönetilen genel DNS bölgesinde oluşturulan özel bağlantı girdileri `azurehdinsight.net` aşağıdaki gibidir:

```dns
<clustername>        CNAME    <clustername>.privatelink
<clustername>-int    CNAME    <clustername>-int.privatelink
<clustername>-ssh    CNAME    <clustername>-ssh.privatelink
```

Aşağıdaki görüntüde, küme yük dengeleyicilerine doğrudan bir bakış hattı olmayan veya bir sanal ağdan kümeye erişmek için gereken özel DNS girişlerinin bir örneği gösterilmektedir. FQDN 'leri geçersiz kılmak `*.privatelink.azurehdinsight.net` ve kendi özel uç noktalarınız IP adreslerinizi çözümlemek Için Azure özel bölgesini kullanabilirsiniz.

:::image type="content" source="media/hdinsight-private-link/access-private-clusters.png" alt-text="Giden kaynak sağlayıcısı bağlantısı kullanan HDInsight mimarisi diyagramı":::

## <a name="arm-template-properties"></a>ARM şablon özellikleri

Aşağıdaki JSON kod parçacığı, özel bir HDInsight kümesi oluşturmak için ARM şablonunuzda yapılandırmanız gereken iki ağ özelliğini içerir.

```json
networkProperties: {
    "resourceProviderConnection": "Inbound" | "Outbound",
    "privateLink": "Enabled" | "Disabled"
}
```

Özel bağlantı dahil olmak üzere HDInsight kurumsal güvenlik özelliklerinin çoğunu içeren tüm bir şablon için bkz. [HDInsight Enterprise Security Template](https://github.com/Azure-Samples/hdinsight-enterprise-security/tree/main/ESP-HIB-PL-Template).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight için Kurumsal Güvenlik Paketi](enterprise-security-package.md)
* [Azure HDInsight 'ta kurumsal güvenlik genel bilgileri ve yönergeleri](./domain-joined/general-guidelines.md)
