---
title: Azure trafik analitiği sık sorulan sorular | Microsoft Dokümanlar
description: Trafik analitiği hakkında en sık sorulan sorulardan bazılarına yanıt alın.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: damendo
ms.openlocfilehash: 5e31ed905f05070c8715a63ef3386b0006df0a75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840630"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Trafik Analitiği sık sorulan sorular

Bu makale, Azure Ağ İzleyicisi'nde trafik analitiği hakkında en sık sorulan soruların çoğunu tek bir yerde toplar.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Trafik analitiğini kullanmanın ön koşulları nelerdir?

Trafik Analitiği aşağıdaki ön koşulları gerektirir:

- Ağ İzleyicisi aboneliği etkinleştirildi.
- İzlemek istediğiniz NSG'ler için Ağ Güvenlik Grubu (NSG) akış günlükleri etkinleştirildi.
- Ham akış günlüklerini depolamak için bir Azure Depolama hesabı.
- Okuma ve yazma erişimine sahip bir Azure Log Analytics çalışma alanı.

Hesabınız trafik analizini etkinleştirmek için aşağıdakilerden birini karşılamalıdır:

- Hesabınızda abonelik kapsamında aşağıdaki rol tabanlı erişim denetimi (RBAC) rollerinden herhangi birine sahip olmalıdır: sahibi, katılımcısı, okuyucu veya ağ katkıda bulunan kişi.
- Hesabınız daha önce listelenen rollerden birine atanmamışsa, abonelik düzeyinde aşağıdaki eylemlere atanan özel bir role atanması gerekir.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/bağlantılar/okuma
    - Microsoft.Network/loadBalancers/oku 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/oku 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/okuma
    - Microsoft.Network/virtualNetworkGateways/oku 
    - Microsoft.Network/virtualNetworks/read
        
Abonelik için kullanıcıya atanan rolleri denetlemek için:

1. **Giriş-AzAccount'ı kullanarak Azure'da**oturum açın. 

2. **Select-AzSubscription'ı**kullanarak gerekli aboneliği seçin. 

3. Belirli bir kullanıcıya atanan tüm rolleri listelemek için **Get-AzRoleAssignment -SignInName [kullanıcı e-posta] -IncludeClassicAdministrators**'ı kullanın. 

Herhangi bir çıktı görmüyorsanız, komutları çalıştırmak için erişim elde etmek için ilgili abonelik yöneticisine başvurun. Daha fazla ayrıntı için Azure [PowerShell ile rol tabanlı erişim denetimini yönet'e](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell)bakın.


## <a name="in-which-azure-regions-is-traffic-analytics-available"></a>Trafik Analitiği hangi Azure bölgelerinde kullanılabilir?

NSG'ler için trafik analitiğini aşağıdaki desteklenen bölgelerden herhangi birinde kullanabilirsiniz:
- Orta Kanada
- Orta Batı ABD
- Doğu ABD
- Doğu ABD 2
- Orta Kuzey ABD
- Orta Güney ABD
- Orta ABD
- Batı ABD
- Batı ABD 2
- Orta Fransa
- Batı Avrupa
- Kuzey Avrupa
- Güney Brezilya
- Batı Birleşik Krallık
- Güney Birleşik Krallık
- Doğu Avustralya
- Güneydoğu Avustralya 
- Doğu Asya
- Güneydoğu Asya
- Güney Kore - Orta
- Orta Hindistan
- Güney Hindistan
- Doğu Japonya
- Batı Japonya
- US Gov Virginia
- Çin Doğu 2

Günlük Analizi çalışma alanı aşağıdaki bölgelerde bulunmalıdır:
- Orta Kanada
- Orta Batı ABD
- Doğu ABD
- Doğu ABD 2
- Orta Kuzey ABD
- Orta Güney ABD
- Orta ABD
- Batı ABD
- Batı ABD 2
- Orta Fransa
- Batı Avrupa
- Kuzey Avrupa
- Batı Birleşik Krallık
- Güney Birleşik Krallık
- Doğu Avustralya
- Güneydoğu Avustralya
- Doğu Asya
- Güneydoğu Asya 
- Güney Kore - Orta
- Orta Hindistan
- Doğu Japonya
- US Gov Virginia
- Çin Doğu 2

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Akış günlüklerini etkinleştirdiğim NSG'ler çalışma alanımdan farklı bölgelerde olabilir mi?

Evet, bu NSG'ler Log Analytics çalışma alanınızdan farklı bölgelerde olabilir.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Birden çok NSG tek bir çalışma alanı içinde yapılandırılabilir mi?

Evet.

## <a name="can-i-use-an-existing-workspace"></a>Varolan bir çalışma alanını kullanabilir miyim?

Evet. Varolan bir çalışma alanı seçerseniz, yeni sorgu diline geçirildiğini unutmayın. Çalışma alanını yükseltmek istemiyorsanız, yeni bir çalışma oluşturmanız gerekir. Yeni sorgu dili hakkında daha fazla bilgi için Azure [Monitor günlüklerinin yeni günlük aramasına yükseltinme](../log-analytics/log-analytics-log-search-upgrade.md)sine bakın.

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-log-analytics-workspace-be-in-a-different-subscription"></a>Azure Depolama Hesabım tek bir abonelikte, Log Analytics çalışma alanım farklı bir abonelikte olabilir mi?

Evet, Azure Depolama hesabınız tek bir abonelikte olabilir ve Log Analytics çalışma alanınız farklı bir abonelikte olabilir.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Ham günlükleri farklı bir abonelikte depolayabilir miyim?

Hayır. Akış günlükleri için NSG'nin etkin olduğu herhangi bir depolama hesabında ham günlükleri depolayabilirsiniz. Ancak, hem depolama hesabı hem de ham günlükler aynı abonelik te ve bölgede olmalıdır.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>"Bulunamadı" hatası nedeniyle trafik analitiği için NSG yapılandıramıyorsam ne olur?

Desteklenen bir bölge seçin. Desteklenmeyen bir bölge seçerseniz, "Bulunamadı" hatası alırsınız. Desteklenen bölgeler bu makalede daha önce listelenmiştir.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>NSG akış günlükleri sayfasının altında "Yüklenemedi" durumunu alıyorsam ne olur?

Akış günlüğe kaydetmenin düzgün çalışması için Microsoft.Insights sağlayıcısının kayıtlı olması gerekir. Microsoft.Insights sağlayıcısının aboneliğiniz için kayıtlı olup olmadığından emin değilseniz, aşağıdaki komutla *xxxxx-xxxxx-xxxxxx-xxxx'i* değiştirin ve PowerShell'den aşağıdaki komutları çalıştırın:

```powershell-interactive
**Select-AzSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Çözümü yapılandırdım. Neden panoda bir şey göremiyorum?

Panonun ilk kez görünmesi 30 dakika kadar sürebilir. Çözüm, anlamlı öngörüler elde etmek için öncelikle yeterli veriyi toplamalıdır. Sonra raporlar oluşturur. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>Bu mesajı alırsam ne olur: "Bu çalışma alanında seçili zaman aralığı için herhangi bir veri bulamadık. Zaman aralığını değiştirmeyi deneyin veya farklı bir çalışma alanı seçin."?

Aşağıdaki seçenekleri deneyin:
- Üst çubuktaki zaman aralığını değiştirin.
- Üst çubukta farklı bir Log Analytics çalışma alanı seçin.
- Yakın zamanda etkinleştirilmişse, 30 dakika sonra trafik analitiğine erişmeyi deneyin.
    
Sorunlar devam ederse, [Kullanıcı ses forumunda](https://feedback.azure.com/forums/217313-networking?category_id=195844)endişelere yol alabilen.

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>Ya bu mesajı alırsam: "NSG akış günlüklerinizi ilk kez analiz etmek. Bu işlemin tamamlanması 20-30 dakika sürebilir. Bir süre sonra tekrar kontrol edin. 2) Yukarıdaki adım işe yaramazsa ve çalışma alanınız ücretsiz SKU altındaysa, kota üzerinden doğrulamak için çalışma alanı kullanımınızı buradan kontrol edin, başka bir bilgi için SSS'ye bakın."?

Bu iletiyi görebilirsiniz, çünkü:
- Trafik Analitiği yakın zamanda etkinleştirildi ve anlamlı öngörüler elde etmek için henüz yeterli veri toplamamış olabilir.
- Log Analytics çalışma alanının ücretsiz sürümünü kullanıyorsunuz ve bu sürüm kota sınırlarını aştı. Daha büyük kapasiteye sahip bir çalışma alanı kullanmanız gerekebilir.
    
Sorunlar devam ederse, [Kullanıcı ses forumunda](https://feedback.azure.com/forums/217313-networking?category_id=195844)endişelere yol alabilen.
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>Ya bu mesajı alırsam: "Görünüşe göre kaynak verimiz (Topoloji) ve akış bilgisi yok. Bu arada, kaynak verilerini görmek ve daha fazla bilgi için SSS'ye başvurmak için burayı tıklatın."?

Panodaki kaynak bilgilerini görüyorsunuz; ancak akışla ilgili istatistik ler bulunmamaktadır. Kaynaklar arasında iletişim akışı olmadığı için veriler mevcut olmayabilir. 60 dakika bekleyin ve durumu yeniden kontrol edin. Sorun devam ederse ve kaynaklar arasında iletişim akışının olduğundan eminseniz, [Kullanıcı ses forumunda](https://feedback.azure.com/forums/217313-networking?category_id=195844)endişelere yol alabilirsiniz.

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>PowerShell veya Azure Kaynak Yöneticisi şablonu veya istemci kullanarak trafik analitiğini yapılandırabilir miyim?

6.2.1 sürümünden itibaren Windows PowerShell'i kullanarak trafik analitiğini yapılandırabilirsiniz. Belirli bir NSG için akış günlüğü ve trafik analitiğini Set cmdlet'i kullanarak yapılandırmak için [Set-AzNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/az.network/set-aznetworkwatcherconfigflowlog)bölümüne bakın. Belirli bir NSG için akış günlüğü ve trafik analizi durumunu almak için [Get-AzNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkwatcherflowlogstatus)bölümüne bakın.

Şu anda, trafik analizini yapılandırmak için Bir Azure Kaynak Yöneticisi şablonu kullanamazsınız.

Bir Azure Kaynak Yöneticisi istemcisi kullanarak trafik analizini yapılandırmak için aşağıdaki örneklere bakın.

**Cmdlet örnek ver:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**cmdlet örneğini alın:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```


## <a name="how-is-traffic-analytics-priced"></a>Trafik Analitiği nasıl fiyatlandırılır?

Trafik Analitiği ölçülüdür. Ölçüm, akış günlüğü verilerinin hizmet tarafından işlenmesine ve elde edilen gelişmiş günlüklerin bir Log Analytics çalışma alanında depolanmasına dayanır. 

Örneğin, [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/network-watcher/)göre , Batı Orta ABD bölgesi göz önüne alındığında, Trafik Analitiği tarafından işlenen bir depolama hesabında depolanan akış günlükleri 10 GB ise ve Log Analytics çalışma alanında alınan gelişmiş günlükler 1 GB ise geçerli ücretler şunlardır: 10 x 2,3$ + 1 x 2,76$ = 25,76$

## <a name="how-frequently-does-traffic-analytics-process-data"></a>Trafik Analitiği verileri ne sıklıkta işliyor?

Trafik Analizi Şeması ve Veri Toplama Belgesi'ndeki [veri toplama bölümüne](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-schema#data-aggregation) bakın

## <a name="how-does-traffic-analytics-decide-that-an-ip-is-malicious"></a>Traffic Analytics, BIR IP'nin kötü amaçlı olduğuna nasıl karar verir? 

Traffic Analytics, BIR IP'yi kötü amaçlı olarak kabul etmek için Microsoft dahili tehdit istihbarat sistemlerine güvenir. Bu sistemler, Microsoft ürünleri ve hizmetleri, Microsoft Dijital Suçlar Birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve harici akışlar gibi çeşitli telemetri kaynaklarından yararlanır ve bunun üzerine çok fazla zeka oluşturur. Bu verilerin bazıları Microsoft Internal'dir. Bilinen bir IP kötü amaçlı olarak işaretleniyorsa, ayrıntıları öğrenmek için lütfen bir destek bileti kaldırın.

## <a name="how-can-i-set-alerts-on-traffic-analytics-data"></a>Trafik Analizi verilerinde nasıl uyarıları ayarlayabilirim?

Traffic Analytics'te uyarılar için dahili destek yoktur. Ancak, Trafik Analitiği verileri Log Analytics'te depolandığından, özel sorgular yazabilir ve bunlara uyarılar ayarlayabilirsiniz. Adım -ları:
- Trafik Analitiği'nde Log Analytics için kısa bağlantıyı kullanabilirsiniz. 
- Sorgularınızı yazmak için [burada belgelenen şemayı](traffic-analytics-schema.md) kullanın 
- Uyarıyı oluşturmak için "Yeni uyarı kuralı"nı tıklatın
- Uyarıyı oluşturmak için [günlük uyarıları belgelerine](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) bakın

## <a name="how-do-i-check-which-vms-are-receiving-most-on-premise-traffic"></a>Hangi VM'lerin şirket içi en çok trafik aldığını nasıl kontrol edebilirim?

            AzureNetworkAnalytics_CL
            | where SubType_s == "FlowLog" and FlowType_s == "S2S" 
            | where <Scoping condition>
            | mvexpand vm = pack_array(VM1_s, VM2_s) to typeof(string)
            | where isnotempty(vm) 
             | extend traffic = AllowedInFlows_d + DeniedInFlows_d + AllowedOutFlows_d + DeniedOutFlows_d // For bytes use: | extend traffic = InboundBytes_d + OutboundBytes_d 
            | make-series TotalTraffic = sum(traffic) default = 0 on FlowStartTime_t from datetime(<time>) to datetime(<time>) step 1m by vm
            | render timechart

  IP'ler için:

            AzureNetworkAnalytics_CL
            | where SubType_s == "FlowLog" and FlowType_s == "S2S" 
            //| where <Scoping condition>
            | mvexpand IP = pack_array(SrcIP_s, DestIP_s) to typeof(string)
            | where isnotempty(IP) 
            | extend traffic = AllowedInFlows_d + DeniedInFlows_d + AllowedOutFlows_d + DeniedOutFlows_d // For bytes use: | extend traffic = InboundBytes_d + OutboundBytes_d 
            | make-series TotalTraffic = sum(traffic) default = 0 on FlowStartTime_t from datetime(<time>) to datetime(<time>) step 1m by IP
            | render timechart

Zaman için, kullanım formatı : yyyy-mm-dd 00:00:00

## <a name="how-do-i-check-standard-deviation-in-traffic-recieved-by-my-vms-from-on-premise-machines"></a>Şirket içi makinelerden VM'lerim tarafından alınan trafikteki standart sapmayı nasıl kontrol edebilirim?

            AzureNetworkAnalytics_CL
            | where SubType_s == "FlowLog" and FlowType_s == "S2S" 
            //| where <Scoping condition>
            | mvexpand vm = pack_array(VM1_s, VM2_s) to typeof(string)
            | where isnotempty(vm) 
            | extend traffic = AllowedInFlows_d + DeniedInFlows_d + AllowedOutFlows_d + DeniedOutFlows_d // For bytes use: | extend traffic = InboundBytes_d + OutboundBytes_d
            | summarize deviation = stdev(traffic)  by vm


IP'ler için:

            AzureNetworkAnalytics_CL
            | where SubType_s == "FlowLog" and FlowType_s == "S2S" 
            //| where <Scoping condition>
            | mvexpand IP = pack_array(SrcIP_s, DestIP_s) to typeof(string)
            | where isnotempty(IP) 
            | extend traffic = AllowedInFlows_d + DeniedInFlows_d + AllowedOutFlows_d + DeniedOutFlows_d // For bytes use: | extend traffic = InboundBytes_d + OutboundBytes_d
            | summarize deviation = stdev(traffic)  by IP
            
## <a name="how-do-i-check-which-ports-are-reachable-or-bocked-between-ip-pairs-with-nsg-rules"></a>NSG kuralları yla IP çiftleri arasında hangi bağlantı noktalarına ulaşılabilir (veya bocked) olduğunu nasıl kontrol edebilirim?

            AzureNetworkAnalytics_CL
            | where SubType_s == "FlowLog" and TimeGenerated between (startTime .. endTime)
            | extend sourceIPs = iif(isempty(SrcIP_s), split(SrcPublicIPs_s, " ") , pack_array(SrcIP_s)),
            destIPs = iif(isempty(DestIP_s), split(DestPublicIPs_s," ") , pack_array(DestIP_s))
            | mvexpand SourceIp = sourceIPs to typeof(string)
            | mvexpand DestIp = destIPs to typeof(string)
            | project SourceIp = tostring(split(SourceIp, "|")[0]), DestIp = tostring(split(DestIp, "|")[0]), NSGList_s, NSGRule_s, DestPort_d, L4Protocol_s, FlowStatus_s 
            | summarize DestPorts= makeset(DestPort_d) by SourceIp, DestIp, NSGList_s, NSGRule_s, L4Protocol_s, FlowStatus_s

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Coğrafi harita görünümünde klavyeyi kullanarak nasıl gezinebilirim?

Coğrafi harita sayfası iki ana bölümden oluşmaktadır:
    
- **Banner**: Coğrafi haritanın üst kısmındaki başlık, trafik dağıtım filtrelerini seçmek için düğmeler sağlar (örneğin, Dağıtım, ülkelerden/bölgelerden gelen trafik ve Kötü Amaçlı). Bir düğme seçtiğinizde, ilgili filtre haritaya uygulanır. Örneğin, Etkin düğmesini seçerseniz, harita dağıtımınızdaki etkin veri merkezlerini vurgular.
- **Harita**: Harita bölümünün altındaki harita bölümünde Azure veri merkezleri ve ülkeler/bölgeler arasındaki trafik dağılımı gösterilmektedir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Banner'da klavye gezintisi
    
- Varsayılan olarak, afişin coğrafi harita sayfasındaki seçim "Azure DCs" filtresidir.
- Başka bir filtreye geçmek `Tab` için `Right arrow` anahtarı veya anahtarı kullanın. Geriye doğru hareket etmek `Shift+Tab` `Left arrow` için, anahtarı veya anahtarı kullanın. İleri navigasyon soldan sağa, yukarıdan aşağıya takip.
- Seçili `Down` filtreyi uygulamak için ok tuşuna basın. `Enter` Filtre seçimi ve dağıtımına bağlı olarak, harita bölümünün altındaki bir veya birden çok düğüm vurgulanır.
- Banner ve harita arasında `Ctrl+F6`geçiş yapmak için .
        
### <a name="keyboard-navigation-on-the-map"></a>Haritada klavye navigasyonu
    
- Banner'daki herhangi bir filtreyi `Ctrl+F6`seçip bastıktan sonra odak, harita görünümünde vurgulanan düğümlerden birine **(Azure veri merkezi** veya **Ülke/Bölge)** taşınır.
- Haritadaki diğer vurgulanmış düğümlere geçmek için, `Tab` ileri `Right arrow` hareketi için anahtarı veya anahtarı kullanın. Kullanın `Shift+Tab` veya `Left arrow` geriye doğru hareket için anahtar.
- Haritada vurgulanan düğümlerden birini seçmek `Enter` için, veya `Down arrow` anahtarı kullanın.
- Bu tür düğümlerin seçiminde, odak düğüm için **Bilgi Aracı Kutusu'na** taşınır. Varsayılan olarak, odak Bilgi Aracı **Kutusu'ndaki**kapalı düğmeye taşınır. **Kutu** görünümü içinde daha fazla `Right arrow` `Left arrow` hareket etmek için, sırasıyla ileri ve geri taşımak için tuşları kullanın. Basıldığında `Enter` **Bilgi Aracı Kutusu'nda**odaklanmış düğmeyi seçmekle aynı etkiye sahiptir.
- Odak Bilgi `Tab` **Aracı Kutusu'nda**yken bastığınızda, odak seçili düğümle aynı kıtadaki bitiş noktalarına taşınır. Bu `Right arrow` uç `Left arrow` noktalararasında hareket etmek için ve tuşları kullanın.
- Diğer akış uç noktalarına veya kıta `Tab` kümelerine geçmek `Shift+Tab` için ileri hareket ve geri hareket için kullanın.
- Odak **Kıta kümeleri**olduğunda, kıta `Enter` `Down` kümesi içindeki uç noktaları vurgulamak için veya ok tuşlarını kullanın. Uç noktalardan ve kıta kümesinin bilgi kutusundaki kapat düğmesinden `Right arrow` `Left arrow` geçmek için sırasıyla ileri ve geri hareket için veya tuşunu kullanın. Herhangi bir bitiş noktasında, `Shift+L` seçili düğümden bitiş noktasına bağlantı satırına geçmek için kullanabilirsiniz. Seçili `Shift+L` bitiş noktasına geçmek için yeniden basabilirsiniz.
        
### <a name="keyboard-navigation-at-any-stage"></a>Herhangi bir aşamada klavye navigasyonu
    
- `Esc`genişletilmiş seçimi daraltıyor.
- Anahtar `Up arrow` aynı eylemi `Esc`gerçekleştirir. Anahtar `Down arrow` aynı eylemi `Enter`gerçekleştirir.
- Yakınlaştırmak ve `Shift+Plus` `Shift+Minus` uzaklaştırmak için kullanın.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Sanal ağ topolojisi görünümünde klavyeyi kullanarak nasıl gezinebilirim?

Sanal ağlar topoloji sayfası iki ana bölümden oluşur:
    
- **Banner**: Sanal ağların üst kısmındaki başlık topolojisi trafik dağıtım filtrelerini seçmek için düğmeler sağlar (örneğin, Bağlı sanal ağlar, Bağlantısı kesilen sanal ağlar ve Genel IP'ler). Bir düğme seçtiğinizde, ilgili filtre topolojiye uygulanır. Örneğin, Etkin düğmesini seçerseniz, topoloji dağıtımınızdaki etkin sanal ağları vurgular.
- **Topoloji**: Başlığın altında, topoloji bölümü sanal ağlar arasındaki trafik dağılımını gösterir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Banner'da klavye gezintisi
    
- Varsayılan olarak, başlık için sanal ağlar topoloji sayfasında seçim "Bağlı VNets" filtresidir.
- Başka bir filtreye geçmek `Tab` için, ilerlemek için anahtarı kullanın. Geriye doğru hareket `Shift+Tab` etmek için anahtarı kullanın. İleri navigasyon soldan sağa, yukarıdan aşağıya takip.
- Seçili filtreyi uygulamak için basın. `Enter` Filtre seçimi ve dağıtımına bağlı olarak, topoloji bölümünün altındaki bir veya birden çok düğüm (sanal ağ) vurgulanır.
- Afiş ve topoloji arasında geçiş `Ctrl+F6`yapmak için.
        
### <a name="keyboard-navigation-on-the-topology"></a>Topolojide klavye gezintisi
    
- Banner'daki herhangi bir filtreyi `Ctrl+F6`seçip bastıktan sonra odak, topoloji görünümünde vurgulanan düğümlerden birine **(VNet)** taşınır.
- Topoloji görünümünde diğer vurgulanan düğümlere geçmek için, ileri hareket için `Shift+Right arrow` anahtarı kullanın. 
- Vurgulanan düğümlerde, odak düğüm için **Bilgi Aracı Kutusu'na** taşınır. Varsayılan olarak, odak **Bilgi Aracı Kutusu'ndaki** **Daha Fazla ayrıntı** düğmesine taşınır. **Kutu** görünümü içinde daha fazla `Right arrow` hareket `Left arrow` etmek için, sırasıyla ileri ve geri hareket etmek için tuşları kullanın. Basıldığında `Enter` **Bilgi Aracı Kutusu'nda**odaklanmış düğmeyi seçmekle aynı etkiye sahiptir.
- Bu tür düğümlerin seçiminde, `Shift+Left arrow` tuşa basarak tüm bağlantılarını tek tek ziyaret edebilirsiniz. Odak, bu bağlantının **Bilgi Aracı Kutusu'na** taşınır. Herhangi bir noktada, odak tekrar basarak `Shift+Right arrow` düğüme geri kaydırılabilir.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Subnet topoloji görünümünde klavyeyi kullanarak nasıl gezinebilirim?

Sanal alt ağlar topoloji sayfası iki ana bölümden oluşur:
    
- **Banner**: Sanal alt ağların üst kısmındaki başlık, trafik dağıtım filtrelerini seçmek için düğmeler sağlar (örneğin, Etkin, Orta ve Ağ Geçidi alt ağları). Bir düğme seçtiğinizde, ilgili filtre topolojiye uygulanır. Örneğin, Etkin düğmesini seçerseniz, topoloji dağıtımınızdaki etkin sanal alt ağı vurgular.
- **Topoloji**: Başlığın altında, topoloji bölümü sanal alt ağlar arasında trafik dağılımını gösterir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Banner'da klavye gezintisi
    
- Varsayılan olarak, başlık için sanal alt ağlar topoloji sayfasında seçim "Alt ağlar" filtresidir.
- Başka bir filtreye geçmek `Tab` için, ilerlemek için anahtarı kullanın. Geriye doğru hareket `Shift+Tab` etmek için anahtarı kullanın. İleri navigasyon soldan sağa, yukarıdan aşağıya takip.
- Seçili filtreyi uygulamak için basın. `Enter` Filtre seçimi ve dağıtımına bağlı olarak, topoloji bölümünün altındaki bir veya birden çok düğüm (Subnet) vurgulanır.
- Afiş ve topoloji arasında geçiş `Ctrl+F6`yapmak için.
        
### <a name="keyboard-navigation-on-the-topology"></a>Topolojide klavye gezintisi
    
- Banner'daki herhangi bir filtreyi `Ctrl+F6`seçip bastıktan sonra odak, topoloji görünümünde vurgulanan düğümlerden birine **(Subnet)** taşınır.
- Topoloji görünümünde diğer vurgulanan düğümlere geçmek için, ileri hareket için `Shift+Right arrow` anahtarı kullanın. 
- Vurgulanan düğümlerde, odak düğüm için **Bilgi Aracı Kutusu'na** taşınır. Varsayılan olarak, odak **Bilgi Aracı Kutusu'ndaki** **Daha Fazla ayrıntı** düğmesine taşınır. **Kutu** görünümü içinde daha fazla `Right arrow` `Left arrow` hareket etmek için, sırasıyla ileri ve geri taşımak için tuşları kullanın. Basıldığında `Enter` **Bilgi Aracı Kutusu'nda**odaklanmış düğmeyi seçmekle aynı etkiye sahiptir.
- Bu tür düğümlerin seçiminde, tuşa basarak `Shift+Left arrow` tüm bağlantılarını tek tek ziyaret edebilirsiniz. Odak, bu bağlantının **Bilgi Aracı Kutusu'na** taşınır. Herhangi bir noktada, odak tekrar basarak `Shift+Right arrow` düğüme geri kaydırılabilir.    

