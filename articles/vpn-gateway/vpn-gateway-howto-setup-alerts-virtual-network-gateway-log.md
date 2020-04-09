---
title: 'Azure VPN Ağ Geçidi: Tanılama günlüğü olaylarındaki uyarıları yapılandırma'
description: VPN Ağ Geçidi tanı günlüğü olaylarında uyarıları yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: 49510b26e0b2a9c69dd65faf0f343e86d1a068db
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878910"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>VPN Ağ Geçidi'nden tanılama günlüğü olayları yla ilgili uyarıları ayarlama

Bu makale, Azure Log Analytics'i kullanarak Azure VPN Ağ Geçidi'ndeki tanılama günlüğü olaylarını temel alan uyarılar ayarlamanıza yardımcı olur. 

Aşağıdaki günlükler Azure'da kullanılabilir:

|***Adı*** | ***Açıklama*** |
|---        | ---               |
|Ağ GeçidiDiagnosticLog | Ağ geçidi yapılandırma olayları, birincil değişiklikler ve bakım olayları için tanılama günlükleri içerir |
|TünelDiagnosticLog | Tünel durumu değişikliği olaylarını içerir. Tünel bağlantısı/kesme olayları, varsa durum değişikliğinin özet bir nedeni |
|RouteDiagnosticLog | Ağ geçidinde oluşan statik rotalarda ve BGP olaylarında değişiklikleri günlüğe kaydeder |
|IKEDiagnosticLog | Giriş iKE denetim iletileri ve ağ geçidindeki olayları kaydeder |
|P2SDiagnosticlog | Ağ geçidinde noktadan siteye denetim iletilerini ve olayları günlüğe kaydeder. Bağlantı kaynağı bilgileri yalnızca IKEv2 bağlantıları için sağlanır |

## <a name="set-up-alerts-in-the-azure-portal"></a><a name="setup"></a>Azure portalında uyarılar ayarlama

Aşağıdaki örnek adımlar, siteden siteye VPN tüneli içeren bir bağlantı kopukluğu olayı için bir uyarı oluşturur:


1. Azure portalında, **Tüm hizmetler** altında **Günlük Analitiği'ni** arayın ve **Log Analytics çalışma alanlarını**seçin.

   ![Günlük Analytics çalışma alanlarına gitmek için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "Oluştur")

2. **Log Analytics** sayfasında **Oluştur'u** seçin.

   ![Oluştur düğmesiyle Analytics sayfasına giriş yap](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "Şunu seçin:")

3. **Yeni Oluştur'u** seçin ve ayrıntıları doldurun.

   ![Log Analytics çalışma alanı oluşturmak için ayrıntılar](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "Şunu seçin:")

4. VPN ağ geçidinizi **Monitör** > **Tanılama ayarları** bıçağında bulun.

   ![Tanılama ayarlarında VPN ağ geçidini bulmak için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "Şunu seçin:")

5. Tanılamayı açmak için ağ geçidini çift tıklatın ve ardından **tanılamayı Aç'ı**seçin.

   ![Tanılamayı açmak için seçmeler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "Şunu seçin:")

6. Ayrıntıları doldurun ve Log **Analytics** ve **TunnelDiagnosticLog'a** Gönder'in seçildiğinden emin olun. Adım 3'te oluşturduğunuz Log Analytics Çalışma Alanını seçin.

   ![Seçili onay kutuları](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "Şunu seçin:")

   > [!NOTE]
   > Verilerin başlangıçta gösterilmesi birkaç saat sürebilir.

7. Sanal ağ ağ geçidi kaynağına genel bakışa gidin ve **İzleme** sekmesinden **Uyarılar'ı** seçin. Ardından yeni bir uyarı kuralı oluşturun veya varolan bir uyarı kuralını edin.

   ![Yeni bir uyarı kuralı oluşturmak için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Şunu seçin:")

   ![noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Şunu seçin:")
8. Günlük Analizi çalışma alanını ve kaynağı seçin.

   ![Çalışma alanı ve kaynak için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "Şunu seçin:")

9. **Ekle koşulu**altında sinyal mantığı olarak Özel **günlük aramasını** seçin.

   ![Özel günlük araması için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "Şunu seçin:")

10. **Arama sorgusu** metin kutusuna aşağıdaki sorguyu girin.  <> ve TimeGenerated'daki değerleri uygun şekilde değiştirin.

    ```
    AzureDiagnostics
    | where Category == "TunnelDiagnosticLog"
    | where _ResourceId == tolower("<RESOURCEID OF GATEWAY>")
    | where TimeGenerated > ago(5m) 
    | where remoteIP_s == "<REMOTE IP OF TUNNEL>"
    | where status_s == "Disconnected"
    | project TimeGenerated, OperationName, instance_s, Resource, ResourceGroup, _ResourceId 
    | sort by TimeGenerated asc
    ```

    Eşik değerini 0 olarak ayarlayın ve **Bitti'yi**seçin.

    ![Sorgu girme ve eşik seçme](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "Şunu seçin:")

11. Oluştur **kuralı** sayfasında, **EYLEM GRUPLARI** bölümünde **Yeni Oluştur'u** seçin. Ayrıntıları doldurun ve **Tamam'ı**seçin.

    ![Yeni bir eylem grubu için ayrıntılar](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "Şunu seçin:")

12. Oluşturma **kuralı** sayfasında, **Eylemleri Özelleştir'in** ayrıntılarını doldurun ve **EYLEM GRUBU İsİm** bölümünde doğru adın göründüğünden emin olun. Kuralı oluşturmak için **uyarı kuralı** oluştur'u seçin.

    ![Kural oluşturmak için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "Şunu seçin:")

## <a name="set-up-alerts-by-using-powershell"></a><a name="setuppowershell"></a>PowerShell'i kullanarak uyarıları ayarlama

Aşağıdaki örnek adımlar, siteden siteye VPN tüneli içeren bir bağlantı kopukluğu olayı için bir uyarı oluşturur.

1. Günlük Analizi çalışma alanı oluşturun:

   ```powershell
   $Location           = 'westus2'
   $ResourceGroupName  = 'TestRG1'
   $Sku                = 'pergb2018'
   $WorkspaceName      = 'LogAnalyticsWS123'

   New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku $Sku -ResourceGroupName $ResourceGroupName
   ```

2. VPN ağ geçidi için tanılamayı açın:

   ```powershell
   $ResourceGroupName  = 'TestRG1'
   $VpnGatewayName     = 'VNet1GW'
   $WorkspaceName      = 'LogAnalyticsWS123'

   $VpnGateway         = Get-AzVirtualNetworkGateway -Name $VpnGatewayName -ResourceGroupName $ResourceGroupName
   $Workspace          = Get-AzOperationalInsightsWorkspace -Name $WorkspaceName -ResourceGroupName $ResourceGroupName

   Set-AzDiagnosticSetting `
       -Name 'VPN tunnel' `
       -ResourceId $VpnGateway.Id `
       -WorkspaceId $Workspace.ResourceId `
       -Enabled $true `
       -Category 'TunnelDiagnosticLog'
   ```

3. Bir eylem grubu oluşturun.

   Bu kod, bir uyarı tetiklendiğinde e-posta bildirimi gönderen bir eylem grubu oluşturur:

   ```powershell
   $ActionGroupName            = 'EmailAdmins'   # Max. 60 characters long
   $ActionGroupShortName       = 'EmailAdmins'   # Max. 12 characters long
   $ActionGroupReceiverName    = 'My receiver Name'
   $EmailAddress               = 'xyz@contoso.com'
   $ResourceGroupName          = 'TestRG1'

   $ActionGroupReceiver = New-AzActionGroupReceiver -Name $ActionGroupReceiverName -UseCommonAlertSchema -EmailReceiver -EmailAddress $EmailAddress

   Set-AzActionGroup `
      -ResourceGroupName $ResourceGroupName `
      -Name $ActionGroupName `
      -ShortName $ActionGroupShortName `
      -Receiver @($ActionGroupReceiver)
   ```

4. Özel günlük araması dayalı bir uyarı kuralı oluşturun:

   ```powershell
   $ActionGroupName    = 'EmailAdmins'
   $EmailSubject       = 'Redmond VPN tunnel is disconnected'
   $Location           = 'westus2'
   $RemoteIp           = '104.42.209.46'
   $ResourceGroupName  = 'TestRG1'
   $VpnGatewayName     = 'VNet1GW'
   $WorkspaceName      = 'LogAnalyticsWS123'

   $VpnGateway         = Get-AzVirtualNetworkGateway -Name $VpnGatewayName -ResourceGroupName $ResourceGroupName
   $Workspace          = Get-AzOperationalInsightsWorkspace -Name $WorkspaceName -ResourceGroupName $ResourceGroupName

   $Query = @"
   AzureDiagnostics |
   where Category == "TunnelDiagnosticLog" |
   where TimeGenerated > ago(5m) |
   where _ResourceId == tolower("$($VpnGateway.id)") |
   where remoteIP_s == "$($RemoteIp)" |
   where status_s == "Disconnected" |
   project TimeGenerated, OperationName, instance_s, Resource, ResourceGroup, _ResourceId |
   sort by TimeGenerated asc
   "@

   $Source             = New-AzScheduledQueryRuleSource -Query $Query -DataSourceId $Workspace.ResourceId
   $Schedule           = New-AzScheduledQueryRuleSchedule -FrequencyInMinutes 5 -TimeWindowInMinutes 5
   $TriggerCondition   = New-AzScheduledQueryRuleTriggerCondition -ThresholdOperator 'GreaterThan' -Threshold 0

   $ActionGroup        = Get-AzActionGroup -ResourceGroupName $ResourceGroupName -Name $ActionGroupName
   $AznsActionGroup    = New-AzScheduledQueryRuleAznsActionGroup -ActionGroup $ActionGroup.Id -EmailSubject $EmailSubject
   $AlertingAction     = New-AzScheduledQueryRuleAlertingAction -AznsAction $AznsActionGroup -Severity '1' -Trigger $TriggerCondition

   New-AzScheduledQueryRule `
       -ResourceGroupName $ResourceGroupName `
       -Location $Location `
       -Action $AlertingAction `
       -Enabled $true `
       -Description 'The tunnel between Azure and Redmond with IP address 104.42.209.46 is disconnected' `
       -Schedule $Schedule `
       -Source $Source `
       -Name 'The Azure to Redmond tunnel is disconnected'
   ```

## <a name="next-steps"></a>Sonraki adımlar

Tünel ölçümlerinde uyarıları yapılandırmak için VPN [Ağ Geçidi ölçümlerinde uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md)bölümüne bakın.
