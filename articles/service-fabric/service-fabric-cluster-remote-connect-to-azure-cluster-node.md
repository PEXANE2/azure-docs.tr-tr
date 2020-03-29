---
title: Azure Hizmet Kumaş küme düğümüne uzaktan bağlanma
description: Ölçek kümesi örneğine (Service Fabric cluster düğümü) uzaktan nasıl bağlanış bekleyeceğini öğrenin.
ms.topic: conceptual
ms.date: 03/23/2018
ms.openlocfilehash: c7ca4f0d5dce1b19837a44d5c9749f3e1293c6b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458324"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Sanal makine ölçeği kümesi örneğine veya küme düğümüne uzaktan bağlanma
Azure'da çalışan bir Hizmet Kumaşı kümesinde, tanımladığınız her küme düğümü türü [sanal bir makine ayrı ölçek ayarlar.](service-fabric-cluster-nodetypes.md)  Belirli ölçek kümesi örneklerine (küme düğümleri) uzaktan bağlanabilirsiniz.  Tek örnekli Sanal M'lerin aksine, ölçek kümesi örneklerinin kendi sanal IP adresleri yoktur. Belirli bir örne uzaktan bağlanmak için kullanabileceğiniz bir IP adresi ve bağlantı noktası ararken bu zor olabilir.

Belirli bir örne uzaktan bağlanmak için kullanabileceğiniz bir IP adresi ve bağlantı noktası bulmak için aşağıdaki adımları tamamlayın.

1. Uzak Masaüstü Protokolü (RDP) için gelen NAT kurallarını alın.

    Genellikle, kümenizde tanımlanan her düğüm türünün kendi sanal IP adresi ve özel bir yük dengeleyicisi vardır. Varsayılan olarak, bir düğüm türü için yük dengeleyicisi aşağıdaki biçimle adlandırılır: *LB-{cluster-name}-{node-type}*; örneğin, *LB-mycluster-FrontEnd*. 
    
    Azure portalındaki yük bakiyecinizin sayfasında > **Gelen NAT** **Ayarlarını**seçin: 

    ![Yük bakiyesi Gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    Aşağıdaki ekran görüntüsü FrontEnd adlı bir düğüm türü için gelen NAT kurallarını gösterir: 

    ![Yük bakiyesi Gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    Her düğüm için IP adresi **HEDEF** sütununda görünür, **HEDEF** sütunölçek kümesi örneğini verir ve **SERVICE** sütunu bağlantı noktası numarasını sağlar. Uzak bağlantı için bağlantı noktaları, bağlantı noktası 3389 ile başlayan artan sırada her düğüme ayrılır.

    Ayrıca, kümeniz için Kaynak Yöneticisi `Microsoft.Network/loadBalancers` şablonu bölümünde Gelen NAT kurallarını da bulabilirsiniz.
    
2. Bir düğüm için hedef bağlantı noktası eşleneme gelen bağlantı noktası onaylamak için, kuralını tıklatın ve **Hedef bağlantı noktası** değerine bakabilirsiniz. Aşağıdaki ekran görüntüsü, önceki adımda **FrontEnd (Örnek 1)** düğümü için gelen NAT kuralını gösterir. (Gelen) bağlantı noktası numarası 3390 olmasına rağmen, hedef bağlantı noktası 3389 bağlantı noktasına eşlenir, hedefteki RDP hizmetinin bağlantı noktası.  

    ![Hedef bağlantı noktası eşleme](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    Varsayılan olarak, Windows kümeleri için hedef bağlantı noktası, hedef düğümdeki RDP hizmetiyle eşleyen 3389 numaralı bağlantı noktasıdır. Linux kümeleri için hedef bağlantı noktası, Secure Shell (SSH) hizmetiyle eşalan 22 bağlantı noktasıdır.

3. Belirli düğüme (ölçek kümesi örneği) uzaktan bağlanın. Kümeyi veya yapılandırdığınız diğer kimlik bilgilerini oluşturduğunuzda ayarladığınız kullanıcı adını ve parolayı kullanabilirsiniz. 

    Aşağıdaki ekran görüntüsü, Windows kümesindeki **FrontEnd (Örnek 1)** düğümüne bağlanmak için Uzak Masaüstü Bağlantısı'nı kullanarak gösterilmektedir:
    
    ![Uzak Masaüstü Bağlantısı](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Linux düğümlerinde, SSH'ye bağlanabilirsiniz (aşağıdaki örnekte kısalık için aynı IP adresini ve bağlantı noktasını yeniden kullanır):

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Sonraki adımlar için aşağıdaki makaleleri okuyun:
* ["Herhangi bir yerde dağıt" özelliğine genel bakışı ve Azure tarafından yönetilen kümelerle karşılaştırmaya](service-fabric-deploy-anywhere.md)bakın.
* Küme [güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* Dağıtımdan sonra küme [VM'lerde RDP bağlantı noktası aralığı değerlerini güncelleştirme](./scripts/service-fabric-powershell-change-rdp-port-range.md)
* Küme VM'leri için [yönetici kullanıcı adını ve parolasını değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md)

