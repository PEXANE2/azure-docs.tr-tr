---
title: 'Azure VPN Gateway: tanılama günlüğü olayları üzerinde uyarı yapılandırma'
description: VPN Gateway tanılama günlüğü olaylarında uyarı yapılandırma adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: 732f4b460a3a97f84dd79a37715aff972e8482ed
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934541"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>VPN Gateway tanılama günlüğü olayları üzerinde uyarı ayarlama

Bu makale Azure Log Analytics kullanarak Azure VPN Gateway tanılama günlüğü olaylarını temel alan uyarılar ayarlamanıza yardımcı olur. 

Aşağıdaki Günlükler Azure 'da kullanılabilir:

|***Adı*** | ***Açıklama*** |
|---        | ---               |
|GatewayDiagnosticLog | Ağ Geçidi yapılandırma olayları, birincil değişiklikler ve bakım olayları için tanılama günlüklerini içerir |
|Tüneldiagnosticlog | Tünel durumu değişikliği olaylarını içerir. Tünel bağlantısı/bağlantı kesme olayları, uygunsa durum değişikliği için özetlenen bir nedene sahiptir |
|RouteDiagnosticLog | Ağ geçidinde gerçekleşen statik rotalara ve BGP olaylarına yapılan değişiklikleri günlüğe kaydeder |
|Ikediagnosticlog | Ağ geçidinde ıKE denetim iletilerini ve olaylarını günlüğe kaydeder |
|P2SDiagnosticLog | Ağ geçidinde Noktadan siteye denetim iletilerini ve olaylarını günlüğe kaydeder |

## <a name="setup"></a>Uyarıları ayarlama

Aşağıdaki örnek adımlar, siteden siteye VPN tünelini içeren bir bağlantı kesme olayı için uyarı oluşturur:


1. Azure portal, **tüm hizmetler** altında **Log Analytics** araması yapın ve **Log Analytics çalışma alanları**' nı seçin.

   ![Log Analytics çalışma alanlarına gitmek için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "Create")

2. **Log Analytics** sayfasında **Oluştur** ' u seçin.

   ![Oluştur düğmesi içeren Log Analytics sayfası](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "Seçin")

3. **Yeni oluştur** ' u seçin ve ayrıntıları girin.

   ![Log Analytics çalışma alanı oluşturma ayrıntıları](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "Seçin")

4. VPN ağ geçidinizi **izleyici** > **Tanılama ayarları** dikey penceresinde bulun.

   ![Tanılama ayarlarında VPN ağ geçidini bulma seçimleri](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "Seçin")

5. Tanılamayı açmak için, ağ geçidine çift tıklayın ve ardından **tanılamayı aç**' ı seçin.

   ![Tanılamayı açmak için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "Seçin")

6. Ayrıntıları girin ve Log Analytics ve **Tunneldiagnosticlog** ' a **Gönder** ' in seçildiğinden emin olun. Adım 3 ' te oluşturduğunuz Log Analytics çalışma alanını seçin.

   ![Seçili onay kutuları](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "Seçin")
   
> [!NOTE]
> Verilerin başlangıçta gösterilmesi birkaç saat sürebilir.
>

7. Sanal ağ geçidi kaynağına genel bakış ' a gidin ve **izleme** sekmesinden **Uyarılar** ' ı seçin. Sonra yeni bir uyarı kuralı oluşturun veya var olan bir uyarı kuralını düzenleyin.

   ![Yeni bir uyarı kuralı oluşturmaya yönelik seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Seçin")

   ![Noktadan siteye](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Seçin")
8. Log Analytics çalışma alanını ve kaynağı seçin.

   ![Çalışma alanı ve kaynak seçimleri](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "Seçin")

9. **Koşul Ekle**altında, sinyal mantığı olarak **özel günlük araması** ' nı seçin.

   ![Özel günlük araması için seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "Seçin")

10. **Arama sorgusu** metin kutusuna aşağıdaki sorguyu girin. < > Ve TimeGenerated değerlerini uygun şekilde değiştirin.

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

    Eşik değerini 0 olarak ayarlayın ve **bitti**' yi seçin.

    ![Bir sorgu girme ve eşik seçme](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "Seçin")

11. **Kural oluştur** sayfasında, **eylem grupları** bölümünde **Yeni oluştur** ' u seçin. Ayrıntıları girin ve **Tamam**' ı seçin.

    ![Yeni bir eylem grubu için Ayrıntılar](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "Seçin")

12. **Kural oluştur** sayfasında, **eylem grubu adı** bölümünde doğru adın göründüğünden emin olmak için, ayrıntıları **Özelleştir** ' in ayrıntılarını girin. Kuralı oluşturmak için **Uyarı kuralı oluştur** ' u seçin.

    ![Kural oluşturmaya yönelik seçimler](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "Seçin")

## <a name="next-steps"></a>Sonraki adımlar

Tünel ölçümlerinde uyarıları yapılandırmak için bkz. [VPN Gateway ölçümleri üzerinde uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
