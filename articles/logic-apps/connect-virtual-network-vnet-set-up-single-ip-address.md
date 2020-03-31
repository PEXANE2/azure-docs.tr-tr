---
title: ISE'ler için genel giden IP adresi ayarlama
description: Azure Mantık Uygulamaları'nda tümleştirme hizmeti ortamları (ISEs) için tek bir genel giden IP adresi nin nasıl ayarlandığını öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 619c68b84291bc35b8216194ac4534393fde454c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191473"
---
# <a name="set-up-a-single-ip-address-for-one-or-more-integration-service-environments-in-azure-logic-apps"></a>Azure Logic Apps'ta bir veya daha fazla tümleştirme hizmeti ortamı için tek bir IP adresi ayarlama

Azure Logic Apps ile çalışırken, bir [Azure sanal ağındaki](../virtual-network/virtual-networks-overview.md)kaynaklara erişmeniz gereken mantık uygulamalarını barındırmak için bir [ *entegrasyon hizmeti ortamı* (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ayarlayabilirsiniz. IP kısıtlamaları olan diğer uç noktalara erişmeniz gereken birden fazla İmKB örneğiniz varsa, sanal ağınıza bir [Azure Güvenlik Duvarı](../firewall/overview.md) veya ağ sanal [cihazı](../virtual-network/virtual-networks-overview.md#filter-network-traffic) dağıtın ve giden trafiği bu güvenlik duvarı veya ağ sanal cihazı üzerinden yönlendirin. Daha sonra sanal ağınızdaki tüm Ise örneklerinin hedef sistemlerle iletişim kurmak için tek, genel, statik ve öngörülebilir bir IP adresi kullanmasını sağlayabilirsiniz. Bu şekilde, her ise için bu hedef sistemlerde ek güvenlik duvarı açıklıkları ayarlamanız gerekmez.

Bu konu, giden trafiği bir Azure Güvenlik Duvarı üzerinden nasıl yönlendirebileceğinizi gösterir, ancak benzer kavramları Azure Marketi'nden üçüncü taraf güvenlik duvarı gibi bir ağ sanal cihazına uygulayabilirsiniz. Bu konu birden çok İmKB örneğinin kurulumuna odaklansa da, senaryonuz erişim gerektiren IP adreslerinin sayısını sınırlamayı gerektirdiğinde bu yaklaşımı tek bir İmKB için de kullanabilirsiniz. Güvenlik duvarı veya sanal ağ cihazı için ek maliyetlerin senaryonuz için anlamlı olup olmadığını göz önünde bulundurun. [Azure Güvenlik Duvarı fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-firewall/)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* İmKB'inizle aynı sanal ağda çalışan bir Azure güvenlik duvarı. Güvenlik duvarınız yoksa, önce sanal ağınıza adlandırılmış `AzureFirewallSubnet` bir alt ağ [ekleyin.](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet) Daha sonra sanal ağınızda [bir güvenlik duvarı oluşturabilir ve dağıtabilirsiniz.](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall)

* Azure [rota tablosu.](../virtual-network/manage-route-table.md) Yoksa, önce bir rota [tablosu oluşturun.](../virtual-network/manage-route-table.md#create-a-route-table) Yönlendirme hakkında daha fazla bilgi için Sanal ağ trafiği yönlendirmesi'ne bakın. [Virtual network traffic routing](../virtual-network/virtual-networks-udr-overview.md)

## <a name="set-up-route-table"></a>Rota tablosunu ayarlama

1. Azure [portalında,](https://portal.azure.com)örneğin rota tablosunu seçin:

   ![Giden trafiği yönlendirmek için kurallı rota tablosunu seçin](./media/connect-virtual-network-vnet-set-up-single-ip-address/select-route-table-for-virtual-network.png)

1. Rota tablosu menüsünde [yeni bir rota eklemek](../virtual-network/manage-route-table.md#create-a-route)için**Rotaekle'yi** **Routes** > seçin.

   ![Giden trafiği yönlendirmek için rota ekleme](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-route-to-route-table.png)

1. Rota **Ekle** [bölmesine, yeni rotayı](../virtual-network/manage-route-table.md#create-a-route) hedef sisteme giden tüm trafiğin aşağıdaki davranışı izlediğini belirten bir kuralla ayarlayın:

   * Sanal [**cihazı**](../virtual-network/virtual-networks-udr-overview.md#user-defined) bir sonraki hop türü olarak kullanır.

   * Bir sonraki atlama adresi olarak güvenlik duvarı örneğinin özel IP adresine gider.

     Bu IP adresini bulmak için, güvenlik duvarı menüsünde **Genel Bakış'ı**seçin, **özel IP adresi**altındaki adresi bulun , örneğin:

     ![Güvenlik duvarı özel IP adresini bulma](./media/connect-virtual-network-vnet-set-up-single-ip-address/find-firewall-private-ip-address.png)

   Böyle bir kuralın nasıl görünebileceğini gösteren bir örnek aşağıda verilmiştir:

   ![Giden trafiği yönlendirmek için kural ayarlama](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-rule-to-route-table.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Yol adı** | <*benzersiz rota adı*> | Rota tablosundaki rota için benzersiz bir ad |
   | **Adres ön eki** | <*hedef adresi*> | Trafiğin gitmesini istediğiniz hedef sistemin adresi. Bu adres için [Sınıfsız Etki Alanları Yönlendirme (CIDR) gösterimi](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) kullandığınızdan emin olun. |
   | **Sonraki atlama türü** | **Sanal gereç** | Giden trafik tarafından kullanılan [atlama türü](../virtual-network/virtual-networks-udr-overview.md#next-hop-types-across-azure-tools) |
   | **Sonraki atlama adresi** | <*güvenlik duvarı-özel-IP adresi*> | Güvenlik duvarınızın özel IP adresi |
   |||

## <a name="set-up-network-rule"></a>Ağ kuralını ayarlama

1. Azure portalında güvenlik duvarınızı bulun ve seçin. Güvenlik duvarı menüsünde Ayarlar **altında** **Kurallar'ı**seçin. Kurallar bölmesinde Ağ **kural koleksiyonunu** > seç,**ağ kuralı koleksiyonu nu ekleyin.**

   ![Güvenlik duvarına ağ kuralı koleksiyonu ekleme](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-network-rule-collection.png)

1. Koleksiyonda, hedef sisteme trafiğe izin veren bir kural ekleyin.

   Örneğin, Bir İmKB'de çalışan ve bir SFTP sistemiyle iletişim kurması gereken bir mantık uygulamanız olduğunu varsayalım. Adlı bir ağ kuralı koleksiyonu `LogicApp_ISE_SFTP_Outbound` `ISE_SFTP_Outbound`oluşturursunuz. Bu kural, Güvenlik duvarınızın özel IP adresini kullanarak İmKB'nizin sanal ağınızda çalıştığı herhangi bir alt netin IP adresinden hedef SFTP sistemine kadar olan trafiğe izin verir.

   ![Güvenlik duvarı için ağ kuralını ayarlama](./media/connect-virtual-network-vnet-set-up-single-ip-address/set-up-network-rule-for-firewall.png)

   **Ağ kuralı toplama özellikleri**

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Adı** | <*ağ-kural-toplama-adı*> | Ağ kuralı koleksiyonunuziçin ad |
   | **Öncelik** | <*öncelik düzeyi*> | Kural koleksiyonunu çalıştırmak için kullanılacak öncelik sırası. Daha fazla bilgi için bkz: [Bazı Azure Güvenlik Duvarı kavramları nelerdir?](../firewall/firewall-faq.md#what-are-some-azure-firewall-concepts) |
   | **Eylem** | **Izin** | Bu kural için gerçekleşecek eylem türü |
   |||

   **Ağ kuralı özellikleri**

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Adı** | <*ağ kural adı*> | Ağ kuralınızın adı |
   | **Protokolü** | <*bağlantı protokolleri*> | Kullanılacak bağlantı protokolleri. Örneğin, NSG kurallarını kullanıyorsanız, hem **TCP** hem de **UDP'yi**seçin, sadece **TCP'yi**değil. |
   | **Kaynak adresleri** | <*İmKB-subnet adresleri*> | İmKB'nizin çalıştığı ve mantık uygulamanızdan kaynaklanan trafiğin kaynaklandığı alt ağ IP adresleri |
   | **Hedef adresleri** | <*hedef-IP adresi*> | Trafiğin gitmesini istediğiniz hedef sisteminizin IP adresi |
   | **Hedef bağlantı noktaları** | <*hedef limanlar*> | Hedef sisteminizin gelen iletişim için kullandığı tüm bağlantı noktaları |
   |||

   Ağ kuralları hakkında daha fazla bilgi için şu makalelere bakın:

   * [Ağ kuralını yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#configure-a-network-rule)
   * [Azure Güvenlik Duvarı kural işleme mantığı](../firewall/rule-processing.md#network-rules-and-applications-rules)
   * [Azure Güvenlik Duvarı SSS](../firewall/firewall-faq.md)
   * [Azure PowerShell: Yeni-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule)
   * [Azure CLI: az ağ güvenlik duvarı ağ kuralı](https://docs.microsoft.com/cli/azure/ext/azure-firewall/network/firewall/network-rule?view=azure-cli-latest#ext-azure-firewall-az-network-firewall-network-rule-create)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps'tan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)