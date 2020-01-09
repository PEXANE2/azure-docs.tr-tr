---
title: Azure Event Hubs-sanal ağlar tümleştirmesini ve güvenlik duvarlarını etkinleştirin
description: Bu öğreticide, güvenli erişimi etkinleştirmek için Event Hubs sanal ağlarla ve güvenlik duvarlarıyla tümleştirmeyi öğreneceksiniz.
services: event-hubs
author: axisc
manager: darosa
ms.author: aschhab
ms.date: 12/20/2019
ms.topic: tutorial
ms.service: event-hubs
ms.custom: mvc
ms.openlocfilehash: f911a1513c6f89180ea51cc0de96dc8a475c7fc8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75437116"
---
# <a name="tutorial-enable-virtual-networks-integration-and-firewalls-on-event-hubs-namespace"></a>Öğretici: Event Hubs ad alanı üzerinde sanal ağlar tümleştirmesini ve güvenlik duvarlarını etkinleştirin

[Sanal ağ (VNet) hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) , sanal ağ özel adres alanınızı ve VNET 'iniz kimliğini doğrudan bir bağlantı üzerinden Azure hizmetlerine genişletir. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır.

Güvenlik duvarları belirli IP adreslerinden (veya IP adres aralıklarından) Event Hubs ad alanına erişimi sınırlamanıza olanak tanır

Bu öğreticide, sanal ağlar hizmet uç noktalarının nasıl tümleştirileceği ve Portal kullanılarak mevcut Azure Event Hubs ad alanınızla güvenlik duvarları (IP filtrelemesi) nasıl ayarlanacağı gösterilmektedir.

Bu öğreticide, aşağıdakileri nasıl yapacağınızı öğreneceksiniz:
> [!div class="checklist"]
> * Sanal ağlar hizmet uç noktalarını Event Hubs ad alanınızla tümleştirme.
> * Event Hubs ad alanınız ile güvenlik duvarı (IP filtrelemesi) kurulumu.

>[!WARNING]
> Sanal ağlar tümleştirmesini uygulamak, diğer Azure hizmetlerinin Event Hubs etkileşimde olmasını engelleyebilir.
>
> Sanal ağlar etkinleştirildiğinde birinci taraf tümleştirmeler desteklenmez.
> Sanal ağlarla çalışmayan yaygın Azure senaryoları-
> * Azure Tanılama ve günlüğe kaydetme
> * Azure Stream Analytics
> * Event Grid tümleştirme
> * Web Apps & Işlevlerinin sanal bir ağda olması gerekir.
> * IoT Hub yolları
> * IoT Device Explorer


> [!IMPORTANT]
> Sanal ağlar desteklenir **standart** ve **adanmış** Event Hubs'ın katmanları. Temel katmanda desteklenmiyor.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz hesap] [] oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Mevcut bir Event Hubs ad alanını kullanacağız, bu nedenle lütfen kullanılabilir bir Event Hubs ad alanı olduğundan emin olun. Bunu yapmazsanız lütfen [Bu öğreticiye](./event-hubs-create.md) başvurun

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

İlk olarak, [Azure Portal][Azure portal] gidin ve Azure aboneliğinizi kullanarak oturum açın.

## <a name="select-event-hubs-namespace"></a>Event Hubs ad alanını seçin

Bu öğreticinin amacı için Event Hubs bir ad alanı oluşturduk ve buna gidecektir.

## <a name="navigate-to-firewalls-and-virtual-networks-experience"></a>Güvenlik duvarları ve sanal ağlar deneyimine gidin

**' Güvenlik duvarları ve sanal ağlar '** seçeneğini seçmek için portaldaki sol bölmedeki gezinti menüsünü kullanın.

  ![Menüye git](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-landing-page.png)

  Bu sayfayı ilk kez ziyaret ettiğinizde **tüm ağlar** radyo düğmesi seçilmelidir. Bu, Event Hubs ad alanının tüm gelen bağlantılara izin verdiğini gösterir.

## <a name="add-virtual-network-service-endpoint"></a>Sanal ağ hizmeti uç noktası ekle

Erişimi sınırlandırmak için, bu Event Hubs ad alanı için sanal ağ hizmet uç noktasını tümleştirmeniz gerekir.

1. Sayfanın en üstündeki **Seçili ağlar** radyo düğmesine tıklayarak sayfanın geri kalanını menü seçenekleriyle etkinleştirin.
  Seçili ağları ![](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-selecting-selected-networks.png)
2. Sayfanın sanal ağ bölümünde, ***var olan sanal ağı ekle***seçeneğini belirleyin. Bu, önceden oluşturulmuş bir sanal ağ seçmenizi sağlayacak olan bölmeye eklenecektir.
  ![var olan sanal ağ](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane.png) Ekle
3. Listeden sanal ağı seçin ve alt ağı seçin.
   alt ağ](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-with-subnet-query.png) ![seçin
4. Sanal ağı listeye eklemeden önce hizmet uç noktasını etkinleştirmeniz gerekir. Hizmet uç noktası etkinleştirilmemişse, Portal bunu etkinleştirmenizi ister.
  ![alt ağ seçin ve uç noktayı etkinleştirin](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-after-enabling.png)
    > [!NOTE]
    > Hizmet uç noktasını etkinleştiremeyebilirsiniz, ARM şablonunu kullanarak eksik sanal ağ hizmeti uç noktasını yoksayabilirsiniz. Portalda bu işlev kullanılamaz.

5. Seçili alt ağ üzerinde hizmet uç noktasını etkinleştirdikten sonra, izin verilen sanal ağlar listesine ekleme işlemine devam edebilirsiniz.
  uç nokta etkinleştirildikten sonra alt ağ ekleme ![](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-after-adding.png)

6. Sanal ağ yapılandırmasını hizmete kaydetmek için üst şeritte **Kaydet** düğmesine basın. Onayın Portal bildirimlerinde gösterilmesi için lütfen birkaç dakika bekleyin.

## <a name="add-firewall-for-specified-ip"></a>Belirtilen IP için güvenlik duvarı Ekle

Güvenlik duvarı kurallarını kullanarak, sınırlı bir IP adresi aralığı veya belirli bir IP adresi için Event Hubs ad alanına erişimi sınırlayabiliriz.

1. Sayfanın en üstündeki **Seçili ağlar** radyo düğmesine tıklayarak sayfanın geri kalanını menü seçenekleriyle etkinleştirin.
  Seçili ağları ![](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-selecting-selected-networks.png)
2. **Güvenlik duvarı** bölümünde, ***adres aralığı*** Kılavuzu ' nun altında, bir veya daha fazla belırlı IP adresi veya IP adresi aralığı ekleyebilirsiniz.
  IP adresi eklemek ![](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-firewall.png)
3. Birden çok IP adresini (veya IP adresi aralıklarını) ekledikten sonra, yapılandırmanın hizmet tarafında kaydedildiğinden emin olmak için üst şeritte **Kaydet** ' e basın. Onayın Portal bildirimlerinde gösterilmesi için lütfen birkaç dakika bekleyin.
  ![IP adresleri Ekle ve Kaydet](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-firewall-hitting-save.png)

## <a name="adding-your-current-ip-address-to-the-firewall-rules"></a>Geçerli IP adresinizi güvenlik duvarı kurallarına ekleme

1. ***Adres aralığı*** kılavuzunun hemen ÜSTÜNDEKI ***istemci IP ADRESINI (geçerli IP adresiniz)*** onay kutusunu işaretleyerek geçerli IP adresinizi hızlıca de ekleyebilirsiniz.
  geçerli IP adresi ekleme ![](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-current-ip-hitting-save.png)
2. Geçerli IP adresinizi güvenlik duvarı kurallarına ekledikten sonra, yapılandırmanın hizmet tarafında kaydedildiğinden emin olmak için üst şeritte **Kaydet** ' e basın. Onayın Portal bildirimlerinde gösterilmesi için lütfen birkaç dakika bekleyin.
  ![geçerli IP adresi ekle ve Kaydet](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-current-ip-hitting-save-after-saving.png)

## <a name="conclusion"></a>Sonuç

Bu öğreticide, sanal ağ uç noktalarını ve güvenlik duvarı kurallarını mevcut bir Event Hubs ad alanıyla tümleştirmiş olursunuz. Şunları öğrenirsiniz:
> [!div class="checklist"]
> * Sanal ağlar hizmet uç noktalarını Event Hubs ad alanınızla tümleştirme.
> * Event Hubs ad alanınız ile güvenlik duvarı (IP filtrelemesi) kurulumu.


[Azure portal]: https://portal.azure.com/
