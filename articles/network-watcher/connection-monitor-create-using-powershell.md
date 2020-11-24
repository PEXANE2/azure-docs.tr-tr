---
title: Bağlantı Izleyicisi oluşturma-PowerShell
titleSuffix: Azure Network Watcher
description: PowerShell kullanarak bağlantı Izleyicisi oluşturmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/23/2020
ms.author: vinigam
ms.openlocfilehash: 1a554177bf7084b9a7f4c413dbe82271b3ab6b3a
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95545542"
---
# <a name="create-a-connection-monitor-using-powershell"></a>PowerShell kullanarak bağlantı Izleyicisi oluşturma

PowerShell kullanarak kaynaklarınız arasındaki iletişimi izlemek için bağlantı Izleyicisi oluşturmayı öğrenin.


## <a name="before-you-begin"></a>Başlamadan önce 

Bağlantı Izleyicisinde oluşturduğunuz bağlantı izleyicilerinde, hem şirket içi makineleri hem de Azure VM 'lerini kaynak olarak ekleyebilirsiniz. Bu bağlantı izleyicileri, uç noktalara bağlantıyı da izleyebilir. Uç noktalar Azure veya başka bir URL veya IP üzerinde olabilir.

Bağlantı Izleyicisi aşağıdaki varlıkları içerir:

* **Bağlantı İzleyicisi kaynağı** : bölgeye özgü bir Azure kaynağı. Aşağıdaki varlıkların hepsi bir bağlantı İzleyicisi kaynağının özellikleridir.
* **Uç nokta** : bağlantı denetimlerine katılan kaynak veya hedef. Uç noktalara örnek olarak Azure VM 'Leri, şirket içi aracılar, URL 'Ler ve IP 'Ler verilebilir.
* **Test yapılandırması** – bir test için protokole özgü bir yapılandırma. Seçtiğiniz protokole bağlı olarak, bağlantı noktası, eşikler, sınama sıklığı ve diğer parametreleri tanımlayabilirsiniz.
* **Test grubu** : kaynak uç noktaları, hedef uç noktaları ve test yapılandırmalarının bulunduğu grup. Bir bağlantı İzleyicisi birden fazla test grubu içerebilir.
* **Test** : kaynak uç noktası, hedef uç noktası ve test yapılandırmasının birleşimi. Test, izleme verilerinin kullanılabildiği en ayrıntılı düzeydir. İzleme verileri, başarısız olan denetim yüzdesini ve gidiş dönüş süresini (RTT) içerir.

    ![Test grupları ve testler arasındaki ilişkiyi tanımlayan bir bağlantı izleyicisini gösteren diyagram](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create-with-powershell"></a>PowerShell ile oluşturma adımları

PowerShell kullanarak bir bağlantı İzleyicisi oluşturmak için aşağıdaki komutları kullanın.

```powershell

//Connect to your Azure account with the subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionId <your-subscription>
//Select region
$nw = "NetworkWatcher_centraluseuap"
//Declare endpoints like Azure VM below. You can also give VNET,Subnet,Log Analytics workspace
$sourcevmid1 = New-AzNetworkWatcherConnectionMonitorEndpointObject -Name MyAzureVm -ResourceID /subscriptions/<your-subscription>/resourceGroups/<your resourceGroup>/providers/Microsoft.Compute/virtualMachines/<vm-name>
//Declare endpoints like URL, IPs
$bingEndpoint = New-AzNetworkWatcherConnectionMonitorEndpointObject -name Bing -Address www.bing.com # Destination URL
//Create test configuration.Choose Protocol and parametersSample configs below.

$IcmpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -IcmpProtocol
$TcpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -TcpProtocol -Port 80
$httpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -HttpProtocol -Port 443 -Method GET -RequestHeader @{Allow = "GET"} -ValidStatusCodeRange 2xx, 300-308 -PreferHTTPS
$httpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name http-tc -TestFrequencySec 60 -ProtocolConfiguration $httpProtocolConfiguration -SuccessThresholdChecksFailedPercent 20 -SuccessThresholdRoundTripTimeMs 30
$icmpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name icmp-tc -TestFrequencySec 30 -ProtocolConfiguration $icmpProtocolConfiguration -SuccessThresholdChecksFailedPercent 5 -SuccessThresholdRoundTripTimeMs 500
$tcpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name tcp-tc -TestFrequencySec 60 -ProtocolConfiguration $TcpProtocolConfiguration -SuccessThresholdChecksFailedPercent 20 -SuccessThresholdRoundTripTimeMs 30
//Create Test Group
$testGroup1 = New-AzNetworkWatcherConnectionMonitorTestGroupObject -Name testGroup1 -TestConfiguration $httpTestConfiguration, $tcpTestConfiguration, $icmpTestConfiguration -Source $sourcevmid1 -Destination $bingEndpoint,
$testname = "cmtest9"
//Create Connection Monitor
New-AzNetworkWatcherConnectionMonitor -NetworkWatcherName $nw -ResourceGroupName NetworkWatcherRG -Name $testname -TestGroup $testGroup1

```

## <a name="description-of-properties"></a>Özelliklerin açıklaması

* connectionMonitorName-Bağlantı İzleyicisi kaynağının adı

* Bağlantı İzleyicisi oluşturmak istediğiniz aboneliğin alt abonelik KIMLIĞI

* NW-ağ Izleyicisi kaynak KIMLIĞI, CM 'nin oluşturulacağı 

* Bağlantı İzleyicisi 'nin oluşturulacağı konum bölgesi

* Uç Noktalar
    * ad – her uç nokta için benzersiz ad
    * RESOURCEID – Azure uç noktaları Için kaynak KIMLIĞI, sanal makineler için Azure Resource Manager kaynak KIMLIĞINE başvurur. Azure dışı uç noktalar için kaynak KIMLIĞI, Azure olmayan aracılara bağlı Log Analytics çalışma alanının Azure Resource Manager kaynak KIMLIĞINE başvurur.
    * Adres: yalnızca kaynak KIMLIĞI belirtilmediğinde veya kaynak KIMLIĞI Log Analytics çalışma alanı ise geçerlidir. Log Analytics kaynak KIMLIĞIYLE birlikte kullanılırsa, bu, izleme için kullanılabilen aracının FQDN 'sine başvurur. Kaynak KIMLIĞI olmadan kullanılırsa bu, herhangi bir genel bitiş noktasının URL veya IP 'SI olabilir.
    * Filtre: Azure dışı uç noktalar Için, Bağlantı İzleyicisi kaynağında izlemek üzere kullanılacak Log Analytics çalışma alanından aracıları seçmek için filtre kullanın. Filtreler ayarlanmamışsa, Log Analytics çalışma alanına ait olan tüm aracılar izleme için kullanılabilir
        * tür – türü "aracı adresi" olarak ayarla
        * Adres: adresi şirket içi aracınızın FQDN 'SI olarak ayarlayın

* Test grupları
    * ad-test grubunuzu adlandırın.
    * testConfigurations-kaynak uç noktaların hedef uç noktalara bağlandığı test yapılandırması
    * kaynaklar-yukarıda oluşturulan uç noktalardan seçin. Azure tabanlı kaynak uç noktalarında Azure ağ Izleyicisi uzantısının yüklü olması ve Azure tabanlı kaynak uç noktalarında Azure Log Analytics aracısının yüklü olması gerekir. Kaynağınıza yönelik bir aracı yüklemek için bkz. [izleme aracılarını yüklemek](./connection-monitor-overview.md#install-monitoring-agents).
    * hedefler-yukarıda oluşturulan uç noktalardan seçin. Azure VM 'lerine veya herhangi bir uç noktaya (genel IP, URL veya FQDN) yönelik bağlantıyı, bunları hedefler olarak belirterek izleyebilirsiniz. Tek bir test grubunda Azure VM 'leri, Office 365 URL 'Leri, Dynamics 365 URL 'Leri ve özel uç noktaları ekleyebilirsiniz.
    * devre dışı bırak-bu alanı, test grubunun belirttiği tüm kaynaklar ve hedefler için izlemeyi devre dışı bırakmak için kullanın.

* Test yapılandırması
    * ad-test yapılandırmasının adı.
    * testFrequencySec-kaynakların, belirttiğiniz protokol ve bağlantı noktasındaki hedeflere ne sıklıkta ping olarak alınacağını belirtin. 30 saniye, 1 dakika, 5 dakika, 15 dakika veya 30 dakika seçebilirsiniz. Kaynaklar, seçtiğiniz değere göre hedeflere bağlantıyı test edecektir. Örneğin, 30 saniye seçerseniz, kaynaklar en az 30 saniyelik bir dönemde hedefe bağlantıyı kontrol eder.
    * protokol-TCP, ıCMP, HTTP veya HTTPS seçeneklerini belirleyebilirsiniz. Protokolüne bağlı olarak, bazı protokole özgü yapılandırmaları yapabilirsiniz
        * preferHTTPS-HTTP üzerinden HTTPS kullanıp kullanmayacağınızı belirtin
        * bağlantı noktası-tercih ettiğiniz hedef bağlantı noktasını belirtin.
        * Disableizleme Oute-bu, protokolü TCP veya ıCMP olan test grupları için geçerlidir. Topoloji ve atlama-atlama RTT bulma kaynaklarını durdurur.
        * Yöntem-bu, Protokolü HTTP olan test yapılandırmalarına yöneliktir. HTTP istek yöntemini seçin--Al veya postala
        * yol-URL 'ye eklenecek yol parametrelerini belirtin
        * validStatusCodes-geçerli durum kodlarını seçin. Yanıt kodu bu listeyle eşleşmiyorsa, bir tanılama iletisi alacaksınız
        * requestHeaders-hedefe geçirilecek özel istek üst bilgisi dizelerini belirtin
    * Başarılı eşik-aşağıdaki ağ parametrelerinde eşikler ayarlayabilirsiniz:
        * checksFailedPercent-belirttiğiniz ölçütlere göre kaynaklar hedeflere bağlantı denetlediğinde başarısız olan denetim yüzdesini ayarlayın. TCP veya ıCMP protokolü için, başarısız denetimlerin yüzdesi, paket kaybı yüzdesine eşit hale getirilmiş olabilir. HTTP protokolü için, bu alan yanıt olmadan alınan HTTP isteklerinin yüzdesini temsil eder.
        * Roundroundtimems-kaynakların test yapılandırması üzerinden hedefe bağlanması için geçen süreyi milisaniye olarak ayarlayın.

## <a name="scale-limits"></a>Ölçek sınırları

Bağlantı izleyicileri aşağıdaki ölçek sınırlarına sahiptir:

* Her bölge için abonelik başına en fazla bağlantı İzleyicisi: 100
* Bağlantı İzleyicisi başına en fazla test grubu: 20
* Bağlantı İzleyicisi başına en fazla kaynak ve hedef sayısı: 100
* Bağlantı İzleyicisi başına en fazla test yapılandırması: 20

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme verilerini çözümlemeyi ve uyarıları ayarlamayı](./connection-monitor-overview.md#analyze-monitoring-data-and-set-alerts) öğrenin
* [Ağınızdaki sorunları tanılamayı](./connection-monitor-overview.md#diagnose-issues-in-your-network) öğrenin