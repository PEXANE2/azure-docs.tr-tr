---
title: Birden çok sesleri Azure Logic Apps için erişim ayarlama-
description: Birden çok tümleştirme hizmeti ortamında (sesleri), Azure Logic Apps 'ten dış sistemlere erişmek için tek bir genel giden IP adresi ayarlayabilirsiniz
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: f35b6836012df7ac3879070f7a85fbfb21b456c0
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74549251"
---
# <a name="set-up-access-for-multiple-integration-service-environments-in-azure-logic-apps"></a>Azure Logic Apps çoklu tümleştirme hizmeti ortamları için erişim ayarlayın

Azure Logic Apps ile çalışırken, bir [Azure sanal ağındaki](../virtual-network/virtual-networks-overview.md)kaynaklara erişmesi gereken mantıksal uygulamaları barındırmak için bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ayarlayabilirsiniz. IP kısıtlamalarına sahip diğer uç noktalara erişmesi gereken birden çok ıSE örneğiniz varsa, sanal ağınıza bir [Azure Güvenlik Duvarı](../firewall/overview.md) veya [ağ sanal](../virtual-network/virtual-networks-overview.md#filter-network-traffic) gereci dağıtın ve giden trafiği bu güvenlik duvarı veya ağ sanal gereci üzerinden yönlendirin. Daha sonra sanal ağınızdaki tüm ıSE örneklerinin hedef sistemlerle iletişim kurmak için tek, öngörülebilir ve genel bir IP adresi kullanmasını sağlayabilirsiniz. Bu şekilde, her bir ıSE için hedef sistemlerde ek güvenlik duvarı yer larını ayarlamanız gerekmez. Bu konu başlığı altında, giden trafiğin bir Azure Güvenlik Duvarı üzerinden nasıl yönlendirileceği gösterilmektedir, ancak Azure Marketi 'nden üçüncü taraf güvenlik duvarı gibi bir sanal ağ sanal gerecine de benzer kavramlar uygulayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* ISE ile aynı sanal ağda çalışan bir Azure Güvenlik Duvarı. Güvenlik duvarınız yoksa, önce sanal ağınıza `AzureFirewallSubnet` adlı [bir alt ağ ekleyin](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet) . Daha sonra, sanal ağınızda [bir güvenlik duvarı oluşturabilir ve dağıtabilirsiniz](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall) .

* Bir Azure [yol tablosu](../virtual-network/manage-route-table.md). Bir tane yoksa, önce [bir rota tablosu oluşturun](../virtual-network/manage-route-table.md#create-a-route-table). Yönlendirme hakkında daha fazla bilgi için bkz. [sanal ağ trafiği yönlendirme](../virtual-network/virtual-networks-udr-overview.md).

## <a name="set-up-route-table"></a>Rota tablosu ayarlama

1. [Azure Portal](https://portal.azure.com), yol tablosunu seçin, örneğin:

   ![Giden trafiği yönlendiren kuralla rota tablosu seçin](./media/connect-virtual-network-vnet-set-up-single-ip-address/select-route-table-for-virtual-network.png)

1. [Yeni bir yol eklemek](../virtual-network/manage-route-table.md#create-a-route)için yol tablosu menüsünde **rotalar** > **Ekle**' yi seçin.

   ![Giden trafiği yönlendirmek için rota ekleyin](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-route-to-route-table.png)

1. **Yol Ekle** bölmesinde, hedef sisteme giden tüm trafiğin bu davranışı takip eden bir kuralla [yeni yolu ayarlayın](../virtual-network/manage-route-table.md#create-a-route) :

   * Sonraki atlama türü olarak [**Sanal Gereç**](../virtual-network/virtual-networks-udr-overview.md#user-defined) kullanır.

   * Sonraki atlama adresi olarak güvenlik duvarı örneği için özel IP adresine gider.

     Bu IP adresini bulmak için, güvenlik duvarı menünüzde **genel bakış**' ı seçin, **özel IP adresi**altında adresi bulun, örneğin:

     ![Güvenlik Duvarı özel IP adresi bulma](./media/connect-virtual-network-vnet-set-up-single-ip-address/find-firewall-private-ip-address.png)

   Bu tür bir kuralın nasıl görünebileceğini gösteren bir örnek aşağıda verilmiştir:

   ![Giden trafiği yönlendirmek için kural ayarla](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-rule-to-route-table.png)

   | Özellik | Value | Açıklama |
   |----------|-------|-------------|
   | **Yol adı** | <*benzersiz-yol adı*> | Yol tablosundaki yol için benzersiz bir ad |
   | **Adres ön eki** | <*hedef adresi*> | Trafiğin gitmesini istediğiniz hedef sistemin adresi. Bu adres için [sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) kullandığınızdan emin olun. |
   | **Sonraki atlama türü** | **Sanal Gereç** | Giden trafik tarafından kullanılan [atlama türü](../virtual-network/virtual-networks-udr-overview.md#next-hop-types-across-azure-tools) |
   | **Sonraki atlama adresi** | <*Güvenlik Duvarı-özel-IP-adres*> | Güvenlik duvarınızın özel IP adresi |
   |||

## <a name="set-up-network-rule"></a>Ağ kuralını ayarlama

1. Azure portal, güvenlik duvarınızı bulun ve seçin. Güvenlik Duvarı menüsünde, **Ayarlar**' ın altında **kurallar**' ı seçin. Kurallar bölmesinde, ağ kuralı **koleksiyonu** > **ağ kuralı koleksiyonu Ekle**' yi seçin.

   ![Güvenlik duvarına ağ kuralı koleksiyonu Ekle](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-network-rule-collection.png)

1. Koleksiyonda, hedef sisteme giden trafiğe izin veren bir kural ekleyin.

   Örneğin, bir ıSE 'de çalışan ve bir SFTP sistemiyle iletişim kurması gereken bir mantıksal uygulamanız olduğunu varsayalım. `ISE_SFTP_Outbound`adlı bir ağ kuralı içeren `LogicApp_ISE_SFTP_Outbound`adlı bir ağ kuralı koleksiyonu oluşturursunuz. Bu kural, güvenlik duvarınızın özel IP adresini kullanarak, ıSE 'nin sanal ağınızda hedef SFTP sistemine çalıştığı herhangi bir alt ağın IP adresinden gelen trafiğe izin verir.

   ![Güvenlik Duvarı için ağ kuralı ayarlama](./media/connect-virtual-network-vnet-set-up-single-ip-address/set-up-network-rule-for-firewall.png)

   **Ağ kuralı koleksiyonu özellikleri**

   | Özellik | Value | Açıklama |
   |----------|-------|-------------|
   | **Adı** | <*ağ kuralı-koleksiyon-adı*> | Ağ kuralı koleksiyonunuzun adı |
   | **Öncelik** | <*öncelik düzeyi*> | Kural koleksiyonunu çalıştırmak için kullanılacak öncelik sırası. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı kavramları nelerdir](../firewall/firewall-faq.md#what-are-some-azure-firewall-concepts)? |
   | **Eylem** | **İzin ver** | Bu kural için gerçekleştirilecek eylem türü |
   |||

   **Ağ kuralı özellikleri**

   | Özellik | Value | Açıklama |
   |----------|-------|-------------|
   | **Adı** | <*ağ kuralı-adı*> | Ağ kuralınız için ad |
   | **Protokol** | <*bağlantı-protokoller*> | Kullanılacak bağlantı protokolleri. Örneğin, NSG kurallarını kullanıyorsanız **, yalnızca TCP**değil **TCP** ve **UDP**' yi seçin. |
   | **Kaynak adresler** | <*Ise-alt ağ-adresler*> | ISE 'nin çalıştığı ve mantıksal uygulamanızdan gelen trafiğin kaynaklandığı alt ağ IP adresleri |
   | **Hedef adresler** | <*hedef-IP adresi*> | Trafiğin gitmesini istediğiniz hedef sisteminizin IP adresi |
   | **Hedef bağlantı noktaları** | <*hedef bağlantı noktaları*> | Hedef sisteminizin gelen iletişim için kullandığı bağlantı noktaları |
   |||

   Ağ kuralları hakkında daha fazla bilgi için şu makalelere bakın:

   * [Ağ kuralı yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#configure-a-network-rule)
   * [Azure Güvenlik Duvarı kural işleme mantığı](../firewall/rule-processing.md#network-rules-and-applications-rules)
   * [Azure Güvenlik Duvarı SSS](../firewall/firewall-faq.md)
   * [Azure PowerShell: New-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule)
   * [Azure CLı: az Network Firewall Network-Rule](https://docs.microsoft.com/cli/azure/ext/azure-firewall/network/firewall/network-rule?view=azure-cli-latest#ext-azure-firewall-az-network-firewall-network-rule-create)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)