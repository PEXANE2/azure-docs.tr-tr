---
title: Azure trafik analitiği | Microsoft Dokümanlar
description: Trafik analitiği yle Azure ağ güvenliği grubu akış günlerini nasıl analiz edebilirsiniz öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2018
ms.author: damendo
ms.reviewer: vinigam
ms.openlocfilehash: 83164a615cacc067e5f1ea6a1dd6ce0f0fd9d540
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298842"
---
# <a name="traffic-analytics"></a>Trafik Analizi

Traffic Analytics, bulut ağlarında kullanıcı ve uygulama etkinliğine görünürlük sağlayan bulut tabanlı bir çözümdür. Trafik analitiği, Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Ağ İzleyiciağ güvenlik grubunu (NSG) akış günlüklerini analiz eder. Trafik analitiği ile şunları yapabilirsiniz:

- Azure aboneliklerinizde ağ etkinliğini görselleştirin ve etkin noktaları belirleyin.
- Açık bağlantı noktaları, internet erişimine çalışan uygulamalar ve sahte ağlara bağlanan sanal makineler (VM) gibi bilgilerle güvenlik tehditlerini belirleyin ve ağınızı güvence altına alayın.
- Ağ dağıtımınızı performans ve kapasite için optimize etmek için Azure bölgeleri ve Internet'teki trafik akışı desenlerini anlayın.
- Ağdaki başarısız bağlantılara yol açan ağ yanlış yapılandırmalarını sapta.

> [!NOTE]
> Trafik Analitiği artık NSG Akış Günlükleri verilerinin 10 dakika daha yüksek bir frekansta topgüvenliğini destekliyor

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="why-traffic-analytics"></a>Neden trafik analitiği?

Ödün vermeyen güvenlik, uyumluluk ve performans için kendi ağınızı izlemek, yönetmek ve bilmek çok önemlidir. Kendi çevrenizi bilmek, çevreyi korumak ve optimize etmek için çok önemlidir. Genellikle ağın geçerli durumunu, kimlerin bağlandığı, nereden bağlandıkları, hangi bağlantı noktalarının internete açık olduğu, beklenen ağ davranışı, düzensiz ağ davranışı ve trafikteki ani artışlar bilmeniz gerekir.

Bulut ağları, ağ arabirimine girerken veya çıkarken IP ağ trafiğini toplama olanağı sağlayan netakış veya eşdeğer protokol özelliğine sahip yönlendiricive anahtarlara sahip olduğunuz şirket içi kurumsal ağlardan farklıdır. Trafik akışı verilerini analiz ederek, ağ trafiği akışı ve hacminin bir analizini oluşturabilirsiniz.

Azure sanal ağları, tek tek ağ arabirimleri, VM'ler veya alt ağlarla ilişkili bir Ağ Güvenlik Grubu aracılığıyla IP trafiğini giriş ve çıkış hakkında bilgi sağlayan NSG akış günlüklerine sahiptir. Ham NSG akış günlüklerini analiz ederek ve güvenlik, topoloji ve coğrafya istihbaratı ekleyerek, trafik analitiği ortamınızdaki trafik akışı hakkında öngörüler sağlayabilir. Trafik Analitiği, çoğu iletişim ana bilgisayarı, çoğu iletişim uygulama protokolleri, çoğu sohbet eden ana bilgisayar çiftleri, izin verilen/engellenen trafik, gelen/giden trafik, açık internet bağlantı noktaları, en engelleme kuralları, Azure veri merkezi başına trafik dağıtımı, sanal ağ, alt ağlar veya sahte ağlar gibi bilgiler sağlar.

## <a name="key-components"></a>Başlıca bileşenler

- **Ağ güvenliği grubu (NSG)**: Bir Azure Sanal Ağı'na bağlı kaynaklara ağ trafiğine izin veren veya reddeden güvenlik kurallarının listesini içerir. Ağ güvenlik grupları (NSG’ler), alt ağlarla, ayrı ayrı VM’lerle (klasik) veya VM’lere bağlı ağ arabirimleri ile ilişkilendirilebilir (Resource Manager). Daha fazla bilgi için [Bkz. Ağ güvenlik grubuna genel bakış.](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)
- **Ağ güvenlik grubu (NSG) akış günlükleri**: Bir ağ güvenlik grubu aracılığıyla giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemenize olanak tanır. NSG akış günlükleri json biçiminde yazılır ve kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında beş tuple bilgi (kaynak/hedef IP adresi, kaynak/hedef bağlantı noktası ve protokol) ve trafiğe izin verilip verilmediğini veya reddedilmişse. NSG akış günlükleri hakkında daha fazla bilgi için [NSG akış günlüklerine](network-watcher-nsg-flow-logging-overview.md)bakın.
- **Günlük Analizi**: İzleme verilerini toplayan ve verileri merkezi bir depoda depolayan bir Azure hizmetidir. Bu veriler, Azure API'si aracılığıyla sağlanan olayları, performans verilerini veya özel verileri içerebilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. Ağ performans monitörü ve trafik analizi gibi izleme uygulamaları, temel olarak Azure Monitor günlükleri kullanılarak oluşturulur. Daha fazla bilgi için [Azure Monitor günlüklerine](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)bakın.
- **Günlük Analizi çalışma alanı**: Azure Hesabıyla ilgili verilerin depolandığı Azure Monitor günlüklerinin bir örneği. Günlük Analizi çalışma alanları hakkında daha fazla bilgi için [bkz.](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)
- **Ağ İzleyicisi**: Azure'da ağ senaryosu düzeyindeki koşulları izlemenize ve tanılamanıza olanak tanıyan bölgesel bir hizmettir. Network Watcher ile NSG akış günlüklerini açıp kapatabilirsiniz. Daha fazla bilgi için [Ağ İzleyicisi'ne](network-watcher-monitoring-overview.md)bakın.

## <a name="how-traffic-analytics-works"></a>Trafik analitiği nasıl çalışır?

Trafik analitiği ham NSG akış günlüklerini inceler ve aynı kaynak IP adresi, hedef IP adresi, hedef bağlantı noktası ve protokol arasında ortak akışları bir araya toplayarak azaltılmış günlükleri yakalar. Örneğin, Ana Bilgisayar 1 (IP adresi: 10.10.10.10) Ana Bilgisayar 2'ye (IP adresi: 10.10.20.10), bağlantı noktasını kullanarak 1 saatlik bir süre içinde 100 kez (örneğin, 80) ve protokol (örneğin, http) iletişim kurar. Azaltılmış günlük bir girişi vardır, Bu Host 1 & Host 2 1 saatlik bir süre içinde *100* kez bağlantı noktası 80 ve protokol *HTTP*kullanarak 100 kez iletişim , yerine 100 girişleri sahip. Azaltılmış günlükler coğrafya, güvenlik ve topoloji bilgileriyle geliştirilir ve daha sonra Log Analytics çalışma alanında depolanır. Aşağıdaki resim veri akışını gösterir:

![NSG akış günlükleri işleme için veri akışı](./media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions-nsg"></a>Desteklenen bölgeler: NSG 

NSG'ler için trafik analitiğini aşağıdaki desteklenen bölgelerden herhangi birinde kullanabilirsiniz:

* Orta Kanada
* Orta Batı ABD
* Doğu ABD
* Doğu ABD 2
* Orta Kuzey ABD
* Orta Güney ABD
* Orta ABD
* Batı ABD
* Batı ABD 2
* Orta Fransa
* Batı Avrupa
* Kuzey Avrupa
* Güney Brezilya
* Batı Birleşik Krallık
* Güney Birleşik Krallık
* Doğu Avustralya
* Güneydoğu Avustralya
* Doğu Asya
* Güneydoğu Asya
* Güney Kore - Orta
* Orta Hindistan
* Güney Hindistan
* Doğu Japonya 
* Batı Japonya
* US Gov Virginia
* Çin Doğu 2

## <a name="supported-regions-log-analytics-workspaces"></a>Desteklenen bölgeler: Log Analytics Çalışma Alanları

Günlük Analizi çalışma alanı aşağıdaki bölgelerde bulunmalıdır:
* Orta Kanada
* Orta Batı ABD
* Doğu ABD
* Doğu ABD 2
* Orta Kuzey ABD
* Orta Güney ABD
* Orta ABD
* Batı ABD
* Batı ABD 2
* Orta ABD
* Orta Fransa
* Batı Avrupa
* Kuzey Avrupa
* Güney Brezilya
* Batı Birleşik Krallık
* Güney Birleşik Krallık
* Doğu Avustralya
* Güneydoğu Avustralya
* Doğu Asya
* Güneydoğu Asya
* Güney Kore - Orta
* Orta Hindistan
* Doğu Japonya
* US Gov Virginia
* Çin Doğu 2

## <a name="prerequisites"></a>Ön koşullar

### <a name="user-access-requirements"></a>Kullanıcı erişim gereksinimleri

Hesabınız aşağıdaki Azure [yerleşik rollerinden](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)birinin üyesi olmalıdır:

|Dağıtım modeli   | Rol                   |
|---------          |---------               |
|Resource Manager   | Sahip                  |
|                   | Katılımcı            |
|                   | Okuyucu                 |
|                   | Ağ Katılımcısı    |

Hesabınız yerleşik rollerden birine atanmamışsa, abonelik düzeyinde aşağıdaki eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) atanması gerekir:

- "Microsoft.Network/applicationGateways/read"
- "Microsoft.Network/connections/read"
- "Microsoft.Network/loadBalancers/read"
- "Microsoft.Network/localNetworkGateways/read"
- "Microsoft.Network/networkInterfaces/read"
- "Microsoft.Network/networkSecurityGroups/read"
- "Microsoft.Network/publicIPAddresses/read"
- "Microsoft.Network/routeTables/read"
- "Microsoft.Network/virtualNetworkGateways/oku"
- "Microsoft.Network/virtualNetworks/read"
- "Microsoft.Network/expressRouteCircuits/oku"

Kullanıcı erişim izinlerini nasıl kontrol edinen hakkında bilgi için [Trafik analizi SSS](traffic-analytics-faq.md)bölümüne bakın.

### <a name="enable-network-watcher"></a>Ağ İzleyicisini etkinleştirme

Trafiği çözümlemek için varolan bir ağ izleyicisine sahip olmanız veya trafiği çözümlemek istediğiniz NSG'lerin olduğu her bölgede [bir ağ izleyicisine sahip](network-watcher-create.md) olmanız gerekir. [Desteklenen bölgelerden](#supported-regions-nsg)herhangi birinde barındırılan NSG'ler için trafik analitiği etkinleştirilebilir.

### <a name="select-a-network-security-group"></a>Ağ güvenlik grubu seçme

NSG akış günlüğe kaydetmeyi etkinleştirmeden önce, akışlarını günlüğe kaydetmek için bir ağ güvenlik grubuna sahip olmalısınız. Ağ güvenlik grubunuz yoksa, oluşturmak için [ağ güvenlik grubu oluşturma'ya](../virtual-network/manage-network-security-group.md#create-a-network-security-group) bakın.

Azure portalında **Ağ izleyicisine**gidin ve ardından **NSG akış günlüklerini**seçin. Aşağıdaki resimde gösterildiği gibi, nsg akış günlüğünü etkinleştirmek istediğiniz ağ güvenlik grubunu seçin:

![NSG akış günlüğünün etkinleştirilmesini gerektiren NSG'lerin seçimi](./media/traffic-analytics/selection-of-nsgs-that-require-enablement-of-nsg-flow-logging.png)

[Desteklenen bölgeler](#supported-regions-nsg)dışında herhangi bir bölgede barındırılan bir NSG için trafik analizini etkinleştirmeye çalışırsanız, "Bulunamadı" hatası alırsınız.

## <a name="enable-flow-log-settings"></a>Akış günlüğü ayarlarını etkinleştirme

Akış günlüğü ayarlarını etkinleştirmeden önce aşağıdaki görevleri tamamlamanız gerekir:

Aboneliğiniz için zaten kayıtlı değilse Azure Öngörüleri sağlayıcısına kaydolun:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

NSG akış günlüğünü depolamak için zaten bir Azure Depolama hesabınız yoksa, bir depolama hesabı oluşturmanız gerekir. Aşağıdaki komutla bir depolama hesabı oluşturabilirsiniz. Komutu çalıştırmadan `<replace-with-your-unique-storage-account-name>` önce, yalnızca sayılar ve küçük harfler kullanarak, 3-24 karakter arasında tüm Azure konumlarında benzersiz bir ad la değiştirin. Gerekirse kaynak grubu adını da değiştirebilirsiniz.

```azurepowershell-interactive
New-AzStorageAccount `
  -Location westcentralus `
  -Name <replace-with-your-unique-storage-account-name> `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Resimde gösterildiği gibi aşağıdaki seçenekleri seçin:

1. **Durum** için *A'yı* Seçin
2. **Akış Günlükleri sürümü**için Sürüm *2'yi* seçin. Sürüm 2 akış oturumu istatistiklerini (Baytlar ve Paketler) içerir
3. Akış günlüğünü depolamak için varolan bir depolama hesabı seçin. Verileri sonsuza kadar depolamak istiyorsanız, değeri *0*olarak ayarlayın. Depolama hesabı için Azure Depolama ücretlerine tabisiniz. Depolama nızın "Veri Gölü Depolama Gen2 Hiyerarşik Ad Alanı Etkin" olarak ayarlanmış olmadığından emin olun.
4. **Bekletme'yi,** verileri depolamak istediğiniz gün sayısına ayarlayın.
5. **Trafik Analitiği Durumu**için *A'yı* seçin.
6. İşlem aralığını seçin. Seçtiğiniz temel ekibe göre akış günlükleri depolama hesabından toplanır ve Traffic Analytics tarafından işlenir. Her 1 saat veya her 10 dakikada bir işlem aralığı seçebilirsiniz. 
7. Varolan bir Günlük Analizi (OMS) Çalışma Alanı'nı seçin veya yeni bir çalışma alanı oluşturmak için **Yeni Çalışma Alanı Oluştur'u** seçin. Bir Log Analytics çalışma alanı, Trafik Analitiği tarafından daha sonra analitiği oluşturmak için kullanılan toplu ve dizine eklenmiş verileri depolamak için kullanılır. Varolan bir çalışma alanı seçerseniz, desteklenen [bölgelerden](#supported-regions-log-analytics-workspaces) birinde bulunması ve yeni sorgu diline yükseltilmiş olması gerekir. Varolan bir çalışma alanını yükseltmek istemiyorsanız veya desteklenen bir bölgede çalışma alanı nız yoksa, yeni bir çalışma alanı oluşturun. Sorgu dilleri hakkında daha fazla bilgi için Azure [Günlük Analizi yükseltmesi ile ilgili yeni günlük araması bölümüne](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)bakın.

> [!NOTE]
>Trafik analitiği çözümlerini barındıran günlük analitiği çalışma alanı ve NSG'lerin aynı bölgede olması gerekmez. Örneğin, Batı Avrupa bölgesindeki bir çalışma alanında trafik analitiğine sahip olabilirken, Doğu ABD ve Batı ABD'de NSG'lerin olabilir. Aynı çalışma alanında birden çok NSG yapılandırılabilir.

8. **Kaydet'i**seçin.

    ![Depolama hesabı seçimi, Log Analytics çalışma alanı ve Trafik Analitiği etkinleştirme](./media/traffic-analytics/ta-customprocessinginterval.png)

Trafik analizini etkinleştirmek istediğiniz diğer NSG'ler için önceki adımları yineleyin. Akış günlüklerinden elde edilen veriler çalışma alanına gönderilir, bu nedenle ülkenizdeki yerel yasa ve yönetmeliklerin çalışma alanının bulunduğu bölgede veri depolamasına izin verdiğinden emin olun. Farklı NSG'ler için farklı işlem aralıkları belirlediyseniz, veriler farklı aralıklarla toplanır. Örneğin: Kritik VNET'ler için 10 dakika, kritik olmayan VNET'ler için 1 saat işlem aralığını etkinleştirmeyi seçebilirsiniz.

Ayrıca Azure PowerShell'deki [Set-AzNetworkWatcherConfigFlowLog](/powershell/module/az.network/set-aznetworkwatcherconfigflowlog) PowerShell cmdlet'i kullanarak trafik analitiğini yapılandırabilirsiniz. Yüklü `Get-Module -ListAvailable Az` sürümünüzü bulmak için çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps).

## <a name="view-traffic-analytics"></a>Trafik analitiğini görüntüleme

Trafik Analizi'ni görüntülemek için portal arama çubuğunda **Ağ İzleyicisini** arayın. Network Watcher'a girdikten sonra, trafik analitiğini ve yeteneklerini keşfetmek için sol menüden **Trafik Analitiği'ni** seçin. 

![Trafik Analizi panosuna erişim](./media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

Trafik Analitiği'nin herhangi bir rapor oluşturabilmesi için önce anlamlı öngörüler elde edebilmek için yeterli veriyi toplaması gerektiğinden, panonun ilk kez görünmesi 30 dakika kadar sürebilir.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Trafik Analitiği tamamen yapılandırıldıktan sonra elde etmek isteyebileceğin öngörülerden bazıları aşağıdaki gibidir:

### <a name="find-traffic-hotspots"></a>Trafik etkin noktalarını bulma

**Aramak**

- Hangi ana bilgisayarlar, alt ağlar ve sanal ağlar en fazla trafiği gönderiyor veya alıyor, maksimum kötü amaçlı trafiği geçiyor ve önemli akışları engelliyor?
    - Ana bilgisayar, alt ağ ve sanal ağ için karşılaştırmalı grafiği denetleyin. En fazla trafiği hangi ana bilgisayarların, alt ağların ve sanal ağların gönderdiğini veya aldığını anlamak, en çok trafiği işleyen ana bilgisayarları ve trafik dağıtımının düzgün şekilde yapılıp yapılmadığını belirlemenize yardımcı olabilir.
    - Trafik hacminin bir ana bilgisayar için uygun olup olmadığını değerlendirebilirsiniz. Trafik hacmi normal davranış mı, yoksa daha fazla araştırma hak ediyor mu?
- Ne kadar gelen/giden trafik var?
    -   Ana bilgisayarın giden den daha fazla gelen trafik alması mı bekleniyor, yoksa tam tersi mi?
- Engellenen trafiğin istatistikleri.
    - Neden bir ana bilgisayar önemli miktarda iyi huylu trafiği engelliyor? Bu davranış, yapılandırmanın daha fazla araştırılmasını ve büyük olasılıkla optimizasyonunu gerektirir
- Kötü amaçlı izin verilen/engellenen trafiğin istatistikleri
  - Neden kötü amaçlı trafik alan bir ana bilgisayar ve neden kötü amaçlı kaynaktan akar izin verilir? Bu davranış, yapılandırmanın daha fazla araştırılmasını ve büyük olasılıkla optimizasyonunu gerektirir.

    Aşağıdaki resimde gösterildiği gibi **Ana Bilgisayar**altında **Tümünü**Gör'üni seçin:

    ![En çok trafik ayrıntılarıyla birlikte ana bilgisayarı gösteren pano](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- Aşağıdaki resim, bir ana bilgisayar için ilk beş konuşan ana bilgisayar için eğilim gösteren zamanı ve akışla ilgili ayrıntıları (izin verilen – gelen/giden ve reddedilen – gelen/giden akışlar) gösterir:

    ![En çok konuşan ilk beş ev sahibi trendi](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Aramak**

- En çok sohbet eden ev sahibi çiftleri hangileridir?
    - Ön uç veya arka uç iletişimi veya arka uç internet trafiği gibi düzensiz davranışlar gibi beklenen davranışlar.
- İzin verilen/engellenen trafiğin istatistikleri
    - Ana bilgisayar neden önemli trafik hacmine izin vermiyor veya engelliyor?
- En sık kullanılan uygulama protokolü en çok sohbet eden ana bilgisayar çiftleri arasında:
    - Bu uygulamalara bu ağda izin veriliyor mu?
    - Uygulamalar düzgün yapılandırıldı mı? İletişim için uygun protokolü kullanıyorlar mı? Aşağıdaki resimde gösterilecek gibi **Sık Konuşma**altında **Tümünü** Gör'üni seçin:

        ![En sık yapılan konuşmayı gösteren pano](./media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- Aşağıdaki resim, en iyi beş konuşma için eğilim gösteren zamanı ve bir konuşma çifti için izin verilen ve reddedilen gelen ve giden akışlar gibi akışla ilgili ayrıntıları gösterir:

    ![En iyi beş geveze konuşma ayrıntıları ve eğilim](./media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Aramak**

- Ortamınızda en çok hangi uygulama protokolü kullanılır ve uygulama protokolünü en çok hangi komşuanahtar çiftleri kullanıyor?
    - Bu uygulamalara bu ağda izin veriliyor mu?
    - Uygulamalar düzgün yapılandırıldı mı? İletişim için uygun protokolü kullanıyorlar mı? Beklenen davranış 80 ve 443 gibi ortak bağlantı noktalarıdır. Standart iletişim için, olağandışı bağlantı noktaları görüntülenirse, yapılandırma değişikliği gerektirebilir. Aşağıdaki resimde **Uygulama bağlantı noktası**altında **tümünü** gör'üni seçin:

        ![Üst uygulama protokollerini gösteren pano](./media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- Aşağıdaki resimler, bir L7 protokolü için en iyi beş L7 protokolü ve akışla ilgili ayrıntıları (örneğin, izin verilen ve reddedilen akışlar) için eğilim gösteren zamanı gösterir:

    ![En iyi beş katman 7 protokolleri ayrıntıları ve eğilim](./media/traffic-analytics/top-five-layer-seven-protocols-details-and-trend.png)

    ![Günlük aramasında uygulama protokolü için akış ayrıntıları](./media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Aramak**

- Ortamınızdaki bir VPN ağ geçidinin kapasite kullanım eğilimleri.
    - Her VPN SKU bant genişliği belirli bir miktar sağlar. VPN ağ geçitleri yeterince kullanılmamış mı?
    - Ağ geçitleriniz kapasiteye ulaşıyor mu? Bir sonraki yüksek SKU yükseltmeniz gerekir mi?
- Hangi en sohbet ana bilgisayarlar, hangi VPN ağ geçidi üzerinden, hangi bağlantı noktası üzerinde?
    - Bu desen normal mi? Aşağıdaki resimde gösterildiği gibi **VPN ağ geçidi**altında **tümünü** gör'üni seçin:

        ![Üst aktif VPN bağlantılarını gösteren pano](./media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- Aşağıdaki resim, bir Azure VPN Ağ Geçidi'nin kapasite kullanımı için eğilim gösteren zamanı ve akışla ilgili ayrıntıları (izin verilen akışlar ve bağlantı noktaları gibi) gösterir:

    ![VPN ağ geçidi kullanım eğilimi ve akış ayrıntıları](./media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Coğrafi coğrafyaya göre trafik dağılımını görselleştirin

**Aramak**

- Veri merkezine en iyi trafik kaynakları, veri merkeziyle sohbet eden en iyi haydut ağlar ve en iyi sohbet uygulama protokolleri gibi veri merkezi başına trafik dağılımı.
  - Bir veri merkezinde daha fazla yük gözlemlerseniz, verimli trafik dağıtımı için plan yapabilirsiniz.
  - Sahte ağlar veri merkezinde sohbet ediyorsa, bunları engellemek için NSG kurallarını düzeltin.

    Aşağıdaki resimde gösterildiği gibi **ortamınızın**altındaki **Haritayı** Görüntüle'yi seçin:

    ![Trafik dağılımını gösteren pano](./media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- Coğrafi harita, veri merkezleri (Dağıtımsız/Dağıtımsız/Etkin Olmayan/Trafik Analizi Etkinleştirilmiş/Trafik Analizi Etkin Değil) gibi parametrelerin seçimi için üst şeridi ve etkin dağıtıma İyi Huylu/Kötü Amaçlı Trafiğine katkıda bulunan ülkeleri/bölgeleri gösterir:

    ![Etkin dağıtımı gösteren coğrafi harita görünümü](./media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- Coğrafi harita, mavi (Benign trafik) ve kırmızı (kötü niyetli trafik) renkli çizgilerle iletişim kuran ülkelerden/bölgelerden ve kıtalardan bir veri merkezine trafik dağılımını gösterir:

    ![Ülkelere/bölgelere ve kıtalara trafik dağılımını gösteren coğrafi harita görünümü](./media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Günlük aramasında trafik dağıtımı için akış ayrıntıları](./media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Sanal ağlarla trafik dağılımını görselleştirin

**Aramak**

- Sanal ağ başına trafik dağılımı, topoloji, sanal ağa en iyi trafik kaynakları, sanal ağa konuşan en iyi haydut ağlar ve en iyi sohbet uygulama protokolleri.
  - Hangi sanal ağın hangi sanal ağa yakınsamasını bilmek. Konuşma beklenmiyorsa, düzeltilebilir.
  - Sahte ağlar sanal bir ağla sohbet ediyorsa, sahte ağları engellemek için NSG kurallarını düzeltebilirsiniz.
 
    Aşağıdaki resimde gösterildiği gibi **ortamınızın**altındaki **VNet'leri** Görüntüle'yi seçin:

    ![Sanal ağ dağıtımLarını gösteren pano](./media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- Sanal Ağ Topolojisi, sanal ağın (Inter sanal ağ Bağlantıları/Etkin/Etkin Olmayan), Dış Bağlantılar, Etkin Akışlar ve sanal ağın kötü amaçlı akışları gibi parametrelerin seçimi için üst şeridi gösterir.
- Sanal Ağ Topolojisini aboneliklere, çalışma alanlarına, kaynak gruplarına ve zaman aralığına göre filtreleyebilirsiniz. Akışı anlamanıza yardımcı olan ek filtreler şunlardır: Akış Türü (InterVNet, IntraVNET, vb.), Akış Yönü (Gelen, Giden), Akış Durumu (İzin Verilen, Engellenen), VNET'ler (Hedeflenen ve Bağlı), Bağlantı Türü (Peering veya Gateway - P2S ve S2S) ve NSG. Ayrıntılı olarak incelemek istediğiniz VNet'lere odaklanmak için bu filtreleri kullanın.
- Sanal Ağ Topolojisi, akışlarla (İzin Verilen/Engellenen/Giden/Giden/İyi Huylu/Kötü Amaçlı) ile ilgili olarak sanal ağa trafik dağılımını gösterir, uygulama protokolü ve ağ güvenlik grupları, örneğin:

    ![Trafik dağılımı ve akış ayrıntılarını gösteren sanal ağ topolojisi](./media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)
    
    ![Üst düzey ve daha fazla filtre yi gösteren sanal ağ topolojisi](./media/traffic-analytics/virtual-network-filters.png)

    ![Günlük aramasında sanal ağ trafiği dağıtımı için akış ayrıntıları](./media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Aramak**

- Alt ağ başına trafik dağılımı, topoloji, alt ağa en iyi trafik kaynakları, alt ağa konuşan üst haydut ağlar ve en üst teçhiz uygulama protokolleri.
    - Hangi alt ağla sohbet ettiğini bilmek. Beklenmeyen konuşmalar görürseniz, yapılandırmanızı düzeltebilirsiniz.
    - Sahte ağlar bir alt ağla sohbet ediyorsa, sahte ağları engellemek için NSG kurallarını yapılandırarak bunu düzeltebilirsiniz.
- Subnets Topolojisi, Etkin/Etkin Olmayan alt ağ, Dış Bağlantılar, Etkin Akışlar ve alt ağın Kötü Amaçlı Akışları gibi parametrelerin seçimi için üst şeridi gösterir.
- Subnet Topolojisi, akışlar (İzin Verilen/Engellenen/Giden/Giden/İyi Huylu/Kötü Amaçlı), uygulama protokolü ve NSG'ler gibi akışlarla ilgili olarak sanal ağa trafik dağılımını gösterir:

    ![Trafik dağılımını akışlarla ilgili sanal ağ alt ağı olarak gösteren subnet topolojisi](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

**Aramak**

Yük Dengeleyici, topoloji, üst trafik kaynakları, Yük Dengeleyicisi & Uygulama ağ geçidine konuşan üst haydut ağlar ve üst sohbet uygulama protokolleri & Uygulama ağ geçidi ne göre trafik dağılımı. 
    
 - Hangi alt ağ hangi Uygulama ağ geçidi veya Yük Dengeleyici susuyor bilmek. Beklenmeyen konuşmaları gözlemlerseniz, yapılandırmanızı düzeltebilirsiniz.
 - Sahte ağlar bir Uygulama ağ geçidi veya Yük Dengeleyicisi ile sohbet ediyorsa, sahte ağları engellemek için NSG kurallarını yapılandırarak bunu düzeltebilirsiniz. 

    ![subnet-topoloji-vitrin-trafik-dağılım-a-uygulama-ağ geçidi-subnet-regards-to-flows](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-application-gateway-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Internet'ten trafik alan bağlantı noktalarını ve sanal makineleri görüntüleyin

**Aramak**

- Hangi açık bağlantı noktaları internet üzerinden sohbet ediyor?
  - Beklenmeyen bağlantı noktaları açık bulunursa, yapılandırmanızı düzeltebilirsiniz:

    ![Trafik alan ve internete gönderen bağlantı noktalarını gösteren pano](./media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

    ![Azure hedef bağlantı noktaları ve ana bilgisayarların ayrıntıları](./media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Aramak**

Çevrenizde kötü amaçlı trafik var mı? Nereden geliyor? Nereye var?

![Kötü amaçlı trafik günlük aramasında ayrıntılı olarak akar](./media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsgnsg-rules-hits"></a>NSG/NSG kurallarındaki eğilimleri görselleştirin

**Aramak**

- Akış dağılımı ile karşılaştırmalı grafikte en çok hangi NSG/NSG kuralları en çok isabet alan hangi stor?
- NSG/NSG kurallarına göre en iyi kaynak ve hedef konuşma çiftleri nelerdir?

    ![NSG'yi gösteren pano istatistikleri vurdu](./media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- Aşağıdaki resimler, bir ağ güvenlik grubu için NSG kurallarının isabetleri için eğilim gösteren zamanı ve kaynak hedef akış ayrıntılarını gösterir:

  - Hangi NSG'lerin ve NSG kurallarının kötü amaçlı akışlarda geçiş yaptığını ve bulut ortamınıza erişen en iyi kötü amaçlı IP adresleri olduğunu hızla algılayın
  - Hangi NSG/NSG kurallarının önemli ağ trafiğine izin ver/engellediğini belirleme
  - NSG veya NSG kurallarının parçalı denetimi için üst filtreleri seçin

    ![NSG kural isabetleri ve en iyi NSG kuralları için eğilim gösteren zamanı gösterme](./media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Günlük aramasında en iyi NSG kuralları istatistik ayrıntıları](./media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sık sorulan soruların yanıtlarını almak için [Trafik analizi SSS](traffic-analytics-faq.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Akış günlüklerini etkinleştirmeyi öğrenmek için [bkz.](network-watcher-nsg-flow-logging-portal.md)
- Trafik Analitiği'nin şemasını ve işleme ayrıntılarını anlamak için [Trafik analitiği şemasına](traffic-analytics-schema.md)bakın.
