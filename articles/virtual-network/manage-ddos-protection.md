---
title: Azure portalını kullanarak Azure DDoS Koruma Standardını yönetme
titlesuffix: Azure Virtual Network
description: Bir saldırıyı azaltmak için Azure Monitor'da Azure DDoS Koruma Standardı telemetrisini nasıl kullanacağınızı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/17/2019
ms.author: kumud
ms.openlocfilehash: 95a70a1d0c4367adb1dd276bff1b1eb20caafc59
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473369"
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Azure portalını kullanarak Azure DDoS Koruma Standardını yönetme

Dağıtılmış hizmet reddi (DDoS) korumasını nasıl etkinleştirip devre dışı kullanabilirsiniz ve Azure DDoS Koruma Standardı ile bir DDoS saldırısını azaltmak için telemetriyi nasıl kullanacağınızı öğrenin. DDoS Koruma Standardı, sanal makineler, yük dengeleyicileri ve kendisine atanmış bir Azure [genel IP adresine](virtual-network-public-ip-address.md) sahip uygulama ağ geçitleri gibi Azure kaynaklarını korur. DDoS Koruma Standardı ve yetenekleri hakkında daha fazla bilgi edinmek için [DDoS Koruma Standardına genel bakış](ddos-protection-overview.md)alameti dir.

Bu öğreticideki herhangi bir adımı tamamlamadan önce, ağ https://portal.azure.com [katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne atanan bir hesapla veya [İzinler'de](#permissions)listelenen uygun eylemlere atanan özel bir [rolde](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure portalında oturum açın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-ddos-protection-plan"></a>Bir DDoS koruma planı oluşturma

DDoS koruma planı, abonelikler arasında DDoS koruma standardını etkinleştiren bir sanal ağ kümesitanımlar. Kuruluşunuz için bir DDoS koruma planı yapılandırabilir ve sanal ağları birden çok abonelikten aynı plana bağlayabilirsiniz. DDoS Koruma Planı'nın kendisi, planın oluşturulması sırasında seçtiğiniz bir abonelikle de ilişkilidir. DDoS Koruma Planı bölgeler ve abonelikler arasında çalışır. Örnek -Doğu-ABD bölgesinde planı oluşturabilir ve kiracınızdaki abonelik #1 bağlantı kurabilirsiniz. Aynı plan, kiracınız genelinde farklı bölgelerdeki diğer aboneliklerden gelen sanal ağlara bağlanabilir. Planın abonelik, korunan genel IP adreslerinin sayısının 100'ü aşması durumunda, planın aylık yinelenen faturasının yanı sıra fazla ücretlendirmesi ile ilişkilidir. DDoS fiyatlandırması hakkında daha fazla bilgi için [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/ddos-protection/)bakın.

Çoğu kuruluş için birden fazla plan oluşturulması gerekmez. Bir plan abonelikler arasında taşınamaz. Bir planın içinde olduğu aboneliği değiştirmek istiyorsanız, [varolan planı silmeniz](#work-with-ddos-protection-plans) ve yeni bir plan oluşturmanız gerekir.

1. Azure portalının sol üst köşesinde **kaynak oluştur'u** seçin.
2. *DDoS*için arama . Arama sonuçlarında **DDos koruma planı** göründüğünde, planı seçin.
3. **Oluştur'u**seçin.
4. Kendi değerlerinizi girin veya seçin veya aşağıdaki örnek değerleri girin ve sonra **Oluştur'u**seçin:

    |Ayar        |Değer                                              |
    |---------      |---------                                          |
    |Adı           | myDdosProtectionPlan                              |
    |Abonelik   | Aboneliğinizi seçin.                         |
    |Kaynak grubu | **Yeni Oluştur'u** seçin ve *myResourceGroup'a* girin |
    |Konum       | Doğu ABD                                           |

## <a name="enable-ddos-for-a-new-virtual-network"></a>Yeni bir sanal ağ için DDoS'u etkinleştirme

1. Azure portalının sol üst köşesinde **kaynak oluştur'u** seçin.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. Girin veya kendi değerlerinizi seçin, aşağıdaki örnek değerleri girin veya seçin, kalan varsayılanları kabul edin ve sonra **Oluştur'u**seçin:

    | Ayar         | Değer                                                        |
    | ---------       | ---------                                                    |
    | Adı            | myVirtualNetwork                                             |
    | Abonelik    | Aboneliğinizi seçin.                                    |
    | Kaynak grubu  | **Var olanı kullan**’ı seçin ve sonra **myResourceGroup** seçeneğini belirleyin |
    | Konum        | Doğu ABD                                                      |
    | DDos koruması | **Standart'ı** seçin ve ardından **DDoS koruması**altında **myDdosProtectionPlan'ı**seçin. Seçtiğiniz plan sanal ağla aynı veya farklı abonelikte olabilir, ancak her iki abonelik de aynı Azure Etkin Dizin kiracıyla ilişkilendirilmelidir.|

Sanal ağ için DDoS Standardı etkinleştirildiğinde sanal ağı başka bir kaynak grubuna veya aboneye taşıyamazsınız. DDoS Standard etkinleştirilmiş bir sanal ağı taşımanız gerekiyorsa, önce DDoS Standard'ı devre dışı bırakıp sanal ağı taşıyın ve ardından DDoS standardını etkinleştirin. Taşımadan sonra, sanal ağdaki tüm korunan genel IP adresleriiçin otomatik ayarlı ilke eşikleri sıfırlanır.

## <a name="enable-ddos-for-an-existing-virtual-network"></a>Varolan bir sanal ağ için DDoS'u etkinleştirme

1. Varolan [bir DDoS koruma planınız](#create-a-ddos-protection-plan)yoksa, DDoS koruma planı oluşturma'daki adımları tamamlayarak bir DDoS koruma planı oluşturun.
2. Azure portalının sol üst köşesinde **kaynak oluştur'u** seçin.
3. Portalın üst kısmındaki **Arama kaynakları, hizmetleri ve dokümanlar kutusuna** DDoS Koruma Standardı'nı etkinleştirmek istediğiniz sanal ağın adını girin. Sanal ağın adı arama sonuçlarında göründüğünde, bu ağı seçin.
4. **AYARLAR**altında **DDoS korumayı**seçin.
5. **Standart'ı**seçin. **DDoS koruma planı**altında, varolan bir DDoS koruma planı veya adım 1'de oluşturduğunuz planı seçin ve sonra **Kaydet'i**seçin. Seçtiğiniz plan sanal ağla aynı veya farklı abonelikte olabilir, ancak her iki abonelik de aynı Azure Etkin Dizin kiracıyla ilişkilendirilmelidir.

**Komutlar** 
- Azure CLI: [az ağ ddos koruması oluşturma](https://docs.microsoft.com/cli/azure/network/ddos-protection?view=azure-cli-latest#az-network-ddos-protection-create)
- Powershell: [Yeni-AzDdosProtectionPlan](https://docs.microsoft.com/powershell/module/Az.Network/New-AzDdosProtectionPlan?view=azps-2.8.0)
 

## <a name="disable-ddos-for-a-virtual-network"></a>Sanal ağ için DDoS'u devre dışı

1. Portalın üst kısmındaki **Arama kaynakları, hizmetleri ve dokümanlar kutusuna** DDoS koruma standardını devre dışı kakmak istediğiniz sanal ağın adını girin. Sanal ağın adı arama sonuçlarında göründüğünde, bu ağı seçin.
2. **AYARLAR**altında **DDoS korumayı**seçin.
3. **DDoS koruma planı** altında **Temel'i** seçin ve ardından **Kaydet'i**seçin.

**Komutlar** 
- Azure CLI: [az ağ ddos koruma silme](https://docs.microsoft.com/cli/azure/network/ddos-protection?view=azure-cli-latest#az-network-ddos-protection-delete)
- Powershell: [Kaldır-AzDdosProtectionPlan](https://docs.microsoft.com/powershell/module/az.network/remove-azddosprotectionplan?view=azps-3.2.0)

## <a name="work-with-ddos-protection-plans"></a>DDoS koruma planları ile çalışma

1. Portalın solundaki **tüm hizmetleri** seçin.
2. **Filtre** kutusuna *DDoS* girin. Sonuçlarda **DDoS koruma planları** göründüğünde, bunu seçin.
3. Listeden görüntülemek istediğiniz koruma planını seçin.
4. Planla ilişkili tüm sanal ağlar listelenir.
5. Bir planı silmek istiyorsanız, öncelikle tüm sanal ağları ondan ayırmanız gerekir. Bir planı sanal ağdan ayrıştırmak için sanal [ağ için DDoS'u devre dışı dışı etme](#disable-ddos-for-a-virtual-network)'ye bakın.

## <a name="configure-alerts-for-ddos-protection-metrics"></a>DDoS koruma ölçümleri için uyarıları yapılandırma

Azure Monitor uyarı yapılandırmasını kullanarak, bir saldırı sırasında etkin bir azaltma olduğunda sizi uyarmak için kullanılabilir DDoS koruma ölçümlerinden herhangi birini seçebilirsiniz. Koşullar yerine getirildiğinde, belirtilen adres bir uyarı e-postası alır:

1. Portalın solundaki **tüm hizmetleri** seçin.
2. **Monitör** kutusuna *Monitör'ü* girin. **Monitör** sonuçlarda göründüğünde, onu seçin.
3. **PAYLAŞıLAN HİzMETLER**altında **Ölçümler'i** seçin.
4. Girin veya kendi değerlerinizi seçin veya aşağıdaki örnek değerleri girin, kalan varsayılanları kabul edin ve sonra **Tamam'ı**seçin:

    |Ayar                  |Değer                                                                                               |
    |---------                |---------                                                                                           |
    |Adı                     | myDdosAlert                                                                                        |
    |Abonelik             | Uyarı almak istediğiniz genel IP adresini içeren aboneliği seçin.        |
    |Kaynak grubu           | Uyarı almak istediğiniz genel IP adresini içeren kaynak grubunu seçin.      |
    |Kaynak                 | Uyarı almak istediğiniz genel IP adresini içeren genel IP adresini seçin. DDoS, sanal ağdaki kaynaklara atanan ortak IP adreslerini izler. Sanal ağda genel IP adreslerine sahip herhangi bir kaynağınız yoksa, öncelikle ortak IP adresine sahip bir kaynak oluşturmanız gerekir. Azure Uygulama Hizmet Ortamları ve Azure VPN Ağ Geçidi dışında [Azure hizmetleri için Sanal ağda](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network)listelenen Kaynak Yöneticisi (klasik değil) aracılığıyla dağıtılan tüm kaynakların genel IP adresini izleyebilirsiniz. Bu öğreticiye devam etmek için, hızlı bir şekilde bir [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine oluşturabilirsiniz.                   |
    |Ölçüm                   | DDoS saldırısı altında veya değil                                                                            |
    |Eşik                | 1 - **1** saldırı altında olduğunuz anlamına gelir. **0,** saldırı altında olmadığınız anlamına gelir.                         |
    |Dönem                   | Seçtiğiniz değeri seçin.                                                                   |
    |E-posta ile bildirin         | Onay kutusunu işaretleyin                                                                                  |
    |Ek yönetici | Abonelik için e-posta sahibi, katkıda bulunan veya okuyucu değilseniz e-posta adresinizi girin. |

    Saldırı algılamasından birkaç dakika sonra, Azure Monitor ölçümlerinden aşağıdaki resme benzer bir e-posta alırsınız:

    ![Saldırı uyarısı](./media/manage-ddos-protection/ddos-alert.png)


Uyarınızı doğrulamak için bir DDoS saldırısını simüle etmek için Bkz. [DDoS algılamasını doğrulayın.](#validate-ddos-detection)

Ayrıca, uyarılar oluşturmak için [web hooks](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [mantıksal uygulamaları](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="use-ddos-protection-telemetry"></a>DDoS koruma telemetrisini kullanma

Bir saldırı için telemetri, Azure Monitor üzerinden gerçek zamanlı olarak sağlanır. Telemetri yalnızca genel bir IP adresinin azaltma altında olduğu süre boyunca kullanılabilir. Bir saldırıdan önce veya sonra telemetri göremezsin.

1. Portalın solundaki **tüm hizmetleri** seçin.
2. **Monitör** kutusuna *Monitör'ü* girin. **Monitör** sonuçlarda göründüğünde, onu seçin.
3. **PAYLAŞıLAN HİzMETLER**altında **Ölçümler'i**seçin.
4. Telemetri istediğiniz genel IP adresini içeren **Abonelik** ve **Kaynak grubunu** seçin.
5. Kaynak türü için **Genel IP Adresi'ni** seçin ve ardından telemetri için istediğiniz belirli genel IP adresini seçin. **Resource type**
6. Ekranın sol tarafında bir dizi **Kullanılabilir Ölçüm** görünür. Bu ölçümler, seçildiğinde, genel bakış ekranındaki **Azure Monitör Ölçümleri Grafiği'nde** grafiklenir.
7. **Toplama** türünü **Max** olarak seçin

Metrik adlar farklı paket türleri sunar ve baytlar ve paketler, her metrikte etiket adlarının temel yapısı aşağıdaki gibidir:

- **Bırakılan etiket adı** (örneğin, **Gelen Paketler DDoS**Düştü ): DDoS koruma sistemi tarafından bırakılan/silinen paket sayısı.
- **İletilen etiket adı** (örneğin **Gelen Paketler İletilen DDoS**): DDoS sistemi tarafından hedef VIP'ye iletilen paket sayısı – filtrelenmemiş trafik.
- **Etiket adı yok** (örneğin **Gelen Paketler DDoS**): Temizleme sistemine gelen toplam paket sayısı – bırakılan ve iletilen paketlerin toplamını temsil eder.

Telemetriyi doğrulamak için bir DDoS saldırısını simüle etmek için [bkz.](#validate-ddos-detection)

## <a name="view-ddos-mitigation-policies"></a>DDoS azaltma ilkelerini görüntüleyin

DDoS Koruma Standardı, DDoS etkinleştirilmiş sanal ağda, korunan kaynağın her genel IP adresi için üç otomatik ayarlı azaltma ilkesi (TCP SYN, TCP & UDP) uygular. Aşağıdaki resimde gösterildiği gibi, 'Max' olarak **toplama** türüne sahip DDoS azaltma ölçümlerini **tetiklemek için Gelen TCP paketlerini** ve **Gelen UDP paketlerini seçerek** ilke eşlemlerini görüntüleyebilirsiniz:

![Azaltma ilkelerini görüntüleme](./media/manage-ddos-protection/view-mitigation-policies.png)

İlke eşikleri, Azure makine öğrenimi tabanlı ağ trafiği profiloluşturma yoluyla otomatik olarak yapılandırılır. Yalnızca ilke eşiği ihlal edildiğinde, saldırı altındaki IP adresi için DDoS azaltma oluşur.

## <a name="configure-ddos-attack-analytics"></a>DDoS saldırı analizini yapılandırma
Azure DDoS Protection standardı, DDoS Attack Analytics ile ayrıntılı saldırı öngörüleri ve görselleştirme sağlar. Sanal ağlarını DDoS saldırılarına karşı koruyan müşteriler, saldırı trafiğine ve saldırı azaltma raporları & azaltma akış günlükleri aracılığıyla saldırıyı azaltmak için alınan eylemleri ayrıntılı olarak görünürlüğe sahiptir. 

## <a name="configure-ddos-attack-mitigation-reports"></a>DDoS saldırı azaltma raporlarını yapılandırma
Saldırı azaltma raporları, kaynağınıza yapılan saldırı hakkında ayrıntılı bilgi sağlamak için toplanan Netflow protokolü verilerini kullanır. Bir genel IP kaynağı saldırı altında olduğunda, azaltma başlar başlamaz rapor oluşturma başlar. Her 5 dakikada bir artımlı bir rapor ve tüm azaltma dönemi için bir azaltma sonrası rapor oluşturulacaktır. Bu, DDoS saldırısının daha uzun bir süre devam etmesi durumunda, azaltma raporunun en güncel anlık görüntüsünü her 5 dakikada bir ve saldırı azaltma sona erdiğinde tam bir özetini görebilmesini sağlamaktır. 

1. Portalın solundaki **tüm hizmetleri** seçin.
2. **Monitör** kutusuna *Monitör'ü* girin. **Monitör** sonuçlarda göründüğünde, onu seçin.
3. **AYARLAR** **altında, Tanılama Ayarları'nı**seçin.
4. Günlüğe kaydetmek istediğiniz genel IP adresini içeren **Abonelik** ve **Kaynak grubunu** seçin.
5. Kaynak türü için **Genel IP Adresi'ni** seçin, ardından ölçümleri günlüğe kaydetmek istediğiniz belirli genel IP adresini seçin. **Resource type**
6. **DDoSMitigationReports günlüğünü toplamak için tanılamayı aç'ı** seçin ve ardından istediğiniz kadar aşağıdaki seçenekleri seçin:

    - **Arşiv bir depolama hesabına**: Veriler bir Azure Depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi edinmek için [Arşiv tanı günlüklerine](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
    - **Etkinlik hub'ına akış**: Günlük alıcının Azure Etkinlik Hub'ını kullanarak günlükleri almasını sağlar. Olay hub'ları Splunk veya diğer SIEM sistemleriyle tümleştirmeyi sağlar. Bu seçenek hakkında daha fazla bilgi edinmek için, [bir olay hub'ına Akış tanılama günlükleri](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
    - **Log Analytics'e gönder**: Günlükleri Azure Monitor hizmetine yazar. Bu seçenek hakkında daha fazla bilgi edinmek [için Azure Monitor günlüklerinde kullanılmak üzere günlükleri topla'ya](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

Her iki artımlı & saldırı sonrası azaltma raporları aşağıdaki alanları içerir
- Saldırı vektörleri
- Trafik istatistikleri
- Bırakılan paketlerin nedeni
- İlgili protokoller
- En iyi 10 kaynak ülke veya bölge
- En iyi 10 kaynak ASNs

## <a name="configure-ddos-attack-mitigation-flow-logs"></a>DDoS saldırı azaltma akış günlüklerini yapılandırma
Saldırı Azaltma Akış Günlükleri, etkin bir DDoS saldırısı sırasında bırakılan trafiği, iletilen trafiği ve diğer ilginç veri noktalarını neredeyse gerçek zamanlı olarak gözden geçirmenize olanak sağlar. Bu verilerin sürekli akışını, neredeyse gerçek zamanlı izleme için olay merkezi aracılığıyla SIEM sistemlerinize aktarabilir, olası eylemleri gerçekleştirebilir ve savunma operasyonlarınızın ihtiyacını karşılayabilirsiniz. 

1. Portalın solundaki **tüm hizmetleri** seçin.
2. **Monitör** kutusuna *Monitör'ü* girin. **Monitör** sonuçlarda göründüğünde, onu seçin.
3. **AYARLAR** **altında, Tanılama Ayarları'nı**seçin.
4. Günlüğe kaydetmek istediğiniz genel IP adresini içeren **Abonelik** ve **Kaynak grubunu** seçin.
5. Kaynak türü için **Genel IP Adresi'ni** seçin, ardından ölçümleri günlüğe kaydetmek istediğiniz belirli genel IP adresini seçin. **Resource type**
6. **DDoSMitigationFlowLogs günlüğünü toplamak için tanılamayı aç'ı** seçin ve ardından istediğiniz kadar aşağıdaki seçenekleri seçin:

    - **Arşiv bir depolama hesabına**: Veriler bir Azure Depolama hesabına yazılır. Bu seçenek hakkında daha fazla bilgi edinmek için [Arşiv tanı günlüklerine](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
    - **Etkinlik hub'ına akış**: Günlük alıcının Azure Etkinlik Hub'ını kullanarak günlükleri almasını sağlar. Olay hub'ları Splunk veya diğer SIEM sistemleriyle tümleştirmeyi sağlar. Bu seçenek hakkında daha fazla bilgi edinmek için, [bir olay hub'ına Akış tanılama günlükleri](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
    - **Log Analytics'e gönder**: Günlükleri Azure Monitor hizmetine yazar. Bu seçenek hakkında daha fazla bilgi edinmek [için Azure Monitor günlüklerinde kullanılmak üzere günlükleri topla'ya](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
1. Azure analiz panosundaki akış günlükleri verilerini görüntülemek için örnek panohttps://github.com/Anupamvi/Azure-DDoS-Protection/raw/master/flowlogsbyip.zip

Akış günlükleri aşağıdaki alanlara sahip olacaktır: 
- Kaynak IP
- Hedef IP
- Kaynak Bağlantı Noktası 
- Hedef bağlantı noktası 
- Protokol türü 
- Azaltma sırasında alınan eylem

Saldırı analizi yalnızca genel IP adresinin sanal ağında DDoS Koruma Standardı etkinleştirildiğinde çalışır. 

## <a name="validate-ddos-detection"></a>DDoS algılamayı doğrulayın

Microsoft, simülasyonlar için DDoS Koruması etkin leştirilmiş genel IP adreslerine karşı trafik oluşturabileceğiniz bir arayüz oluşturmak için [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) ile ortaklık yaptı. BreakPoint Cloud simülasyonu şunları yapmanızı sağlar:

- Microsoft Azure DDoS Koruması'nın Azure kaynaklarınızı DDoS saldırılarına karşı nasıl koruduğunun doğrulanması
- DDoS saldırısı altındayken olay yanıt sürecinizi optimize edin
- Belge DDoS uyumluluğu
- Ağ güvenlik ekiplerinizi eğitin

## <a name="view-ddos-protection-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde DDoS koruma uyarılarını görüntüleme

Azure Güvenlik Merkezi, sorunları araştırmaya ve düzeltmeye yardımcı olacak bilgileriçeren [bir güvenlik uyarıları](/azure/security-center/security-center-managing-and-responding-alerts)listesi sağlar. Bu özellik sayesinde, DDoS saldırısıyla ilgili uyarılar ve saldırıyı yakın zamanda azaltmak için yapılan eylemler de dahil olmak üzere uyarıların birleşik bir görünümünü elde elabilirsiniz.
Herhangi bir DDoS saldırı algılama ve azaltma için göreceğiniz iki özel uyarı vardır:

- **Genel IP için algılanan DDoS Saldırısı**: DDoS koruma hizmeti, ortak IP adreslerinizden birinin bir DDoS saldırısının hedefi olduğunu algıladığında bu uyarı oluşturulur.
- **Genel IP için azaltılan DDoS Saldırısı**: Genel IP adresine yapılan bir saldırı azaltıldığında bu uyarı oluşturulur.
Uyarıları görüntülemek için Azure portalında **Güvenlik Merkezi'ni** açın. **Tehdit Koruması**altında, **Güvenlik uyarılarını**seçin. Aşağıdaki ekran görüntüsü, DDoS saldırı uyarılarının bir örneğini gösterir.

![Azure Güvenlik Merkezi'nde DDoS Uyarısı](./media/manage-ddos-protection/ddos-alert-asc.png)

Uyarılar, saldırı altında olan genel IP adresi, coğrafi ve tehdit istihbaratı bilgileri ve düzeltme adımları hakkında genel bilgileri içerir.

## <a name="permissions"></a>İzinler

DDoS koruma planlarıyla çalışmak için, hesabınızın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tabloda listelenen uygun eylemlere atanan [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir role atanması gerekir:

| Eylem                                            | Adı                                     |
| ---------                                         | -------------                            |
| Microsoft.Network/ddosProtectionPlans/oku        | DDoS koruma planını okuyun              |
| Microsoft.Network/ddosProtectionPlans/yazma       | DDoS koruma planı oluşturma veya güncelleştirme  |
| Microsoft.Network/ddosProtectionPlans/silme      | DDoS koruma planını silme            |
| Microsoft.Network/ddosProtectionPlans/join/action | Bir DDoS koruma planına katılın              |

Sanal ağ için DDoS korumasını etkinleştirmek [için,](manage-virtual-network.md#permissions)hesabınıza sanal ağlar için uygun eylemler atanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama