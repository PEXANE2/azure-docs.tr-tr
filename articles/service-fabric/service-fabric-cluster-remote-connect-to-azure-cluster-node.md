---
title: Azure Service Fabric küme düğümüne uzaktan bağlanma
description: Bir ölçek kümesi örneğine (Service Fabric küme düğümü) uzaktan nasıl bağlanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 03/23/2018
ms.openlocfilehash: c7ca4f0d5dce1b19837a44d5c9749f3e1293c6b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75458324"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Sanal makine ölçek kümesi örneğine veya bir küme düğümüne uzaktan Bağlan
Azure 'da çalışan bir Service Fabric kümesinde, tanımladığınız her küme düğüm türü [bir sanal makine ayrı ölçeğini ayarlar](service-fabric-cluster-nodetypes.md).  Belirli ölçek kümesi örneklerine (küme düğümleri) uzaktan bağlanabilirsiniz.  Tek örnekli VM 'lerden farklı olarak, ölçek kümesi örneklerinin kendi sanal IP adresleri yoktur. Bu, belirli bir örneğe uzaktan bağlanmak için kullanabileceğiniz bir IP adresi ve bağlantı noktası ararken zor olabilir.

Belirli bir örneğe uzaktan bağlanmak için kullanabileceğiniz bir IP adresi ve bağlantı noktası bulmak için aşağıdaki adımları izleyin.

1. Uzak Masaüstü Protokolü (RDP) için gelen NAT kurallarını alın.

    Genellikle, kümenizde tanımlanan her düğüm türünün kendi sanal IP adresi ve ayrılmış yük dengeleyici vardır. Varsayılan olarak, bir düğüm türü için yük dengeleyici şu biçimde adlandırılır: *lb-{Cluster-Name}-{Node-Type}*; Örneğin, *lb-MyCluster-ön uç*. 
    
    Yük dengeleyicinizin Azure Portal sayfasında, **Ayarlar** > **gelen NAT kuralları**' nı seçin: 

    ![Yük dengeleyici gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    Aşağıdaki ekran görüntüsünde, ön uç adlı düğüm türü için gelen NAT kuralları gösterilmektedir: 

    ![Yük dengeleyici gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    Her düğüm için, IP adresi **hedef** sütununda görünür, **hedef** sütunu ölçek kümesi örneğine verir ve **hizmet** sütunu bağlantı noktası numarasını sağlar. Uzak bağlantı için bağlantı noktaları, bağlantı noktası 3389 ' den başlayarak her bir düğüme artan düzende ayrılır.

    Ayrıca, gelen NAT kurallarını kümeniz için Kaynak Yöneticisi şablonunun `Microsoft.Network/loadBalancers` bölümünde bulabilirsiniz.
    
2. Bir düğüm için bağlantı noktası eşlemeyi hedeflemek üzere gelen bağlantı noktasını onaylamak için, kuralına tıklayıp **hedef bağlantı noktası** değerine bakabilirsiniz. Aşağıdaki ekran görüntüsünde, önceki adımda **ön uç (örnek 1)** düğümü IÇIN gelen NAT kuralı gösterilmektedir. (Gelen) bağlantı noktası numarası 3390 olmasına karşın hedef bağlantı noktasının hedefteki RDP hizmetinin bağlantı noktası 3389 ' e eşlendiğine dikkat edin.  

    ![Hedef bağlantı noktası eşlemesi](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    Varsayılan olarak, Windows kümeleri için hedef bağlantı noktası, hedef düğümdeki RDP hizmetiyle eşlenen bağlantı noktası 3389 ' dir. Linux kümelerinde hedef bağlantı noktası, Secure Shell (SSH) hizmetiyle eşlenen bağlantı noktası 22 ' dir.

3. Belirli düğüme uzaktan bağlanın (ölçek kümesi örneği). Kümeyi oluştururken veya yapılandırdığınız diğer kimlik bilgileriyle ayarladığınız Kullanıcı adını ve parolayı kullanabilirsiniz. 

    Aşağıdaki ekran görüntüsünde, bir Windows kümesindeki **ön uç (örnek 1)** düğümüne bağlanmak üzere Uzak Masaüstü bağlantısı kullanımı gösterilmektedir:
    
    ![Uzak Masaüstü Bağlantısı](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Linux düğümlerinde, SSH ile bağlanabilirsiniz (Aşağıdaki örnek, breçekimi için aynı IP adresini ve bağlantı noktasını yeniden kullanır):

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Sonraki adımlar için aşağıdaki makaleleri okuyun:
* ["Her yerde dağıtma" özelliğine genel bakış ve Azure tarafından yönetilen kümelerle karşılaştırma](service-fabric-deploy-anywhere.md)konusuna bakın.
* [Küme güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* Dağıtımdan sonra küme VM 'lerinde [RDP bağlantı noktası aralığı değerlerini Güncelleştir](./scripts/service-fabric-powershell-change-rdp-port-range.md)
* Küme VM 'Leri için [Yönetici Kullanıcı adını ve parolasını değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md)

