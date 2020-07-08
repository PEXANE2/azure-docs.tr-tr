---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 73ba78eca710f0b98b2a209494519cb8003e554b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75468854"
---
Kullanılabilirlik grubu dinleyicisi, SQL Server kullanılabilirlik grubunun dinlediği bir IP adresi ve ağ adıdır. Kullanılabilirlik grubu dinleyicisini oluşturmak için aşağıdakileri yapın:

1. <a name="getnet"></a>Küme ağ kaynağının adını alın.

    a. Birincil çoğaltmayı barındıran Azure sanal makinesine bağlanmak için RDP 'yi kullanın. 

    b. Yük Devretme Kümesi Yöneticisi açın.

    c. **Ağlar** düğümünü seçin ve küme ağı adını aklınızda edin. `$ClusterNetworkName`PowerShell betiğindeki değişkende bu adı kullanın. Aşağıdaki görüntüde küme ağı adı **küme ağı 1**' dir:

   ![Küme ağ adı](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

1. <a name="addcap"></a>İstemci erişim noktasını ekleyin.  
    İstemci erişim noktası, uygulamaların bir kullanılabilirlik grubundaki veritabanlarına bağlanmak için kullandığı ağ adıdır. Yük Devretme Kümesi Yöneticisi ' de istemci erişim noktası oluşturun.

    a. Küme adını genişletin ve ardından **Roller**' e tıklayın.

    b. **Roller** bölmesinde, kullanılabilirlik grubu adına sağ tıklayın ve ardından **kaynak**  >  **istemci erişim noktası**Ekle ' yi seçin.

   ![İstemci erişim noktası](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. **Ad** kutusunda, bu yeni dinleyici için bir ad oluşturun. 
   Yeni dinleyicinin adı, uygulamaların SQL Server kullanılabilirlik grubundaki veritabanlarına bağlanmak için kullandığı ağ adıdır.

    d. Dinleyiciyi oluşturmayı tamamlaması için iki kez **İleri** ' ye ve ardından **son**' a tıklayın. Bu noktada dinleyiciyi veya kaynağı çevrimiçi duruma getirmeyin.

1. Kullanılabilirlik grubu kümesi rolünü çevrimdışına alın. **Roller**altında **Yük devretme kümesi Yöneticisi** , role sağ tıklayın ve **rolü durdur**' u seçin.

1. <a name="congroup"></a>Kullanılabilirlik grubu için IP kaynağını yapılandırın.

    a. **Kaynaklar** sekmesine tıklayın ve ardından oluşturduğunuz istemci erişim noktasını genişletin.  
    İstemci erişim noktası çevrimdışı.

   ![İstemci erişim noktası](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. IP kaynağına sağ tıklayın ve ardından Özellikler ' e tıklayın. IP adresinin adını ve `$IPResourceName` PowerShell betiğindeki değişkende kullanın.

    c. **IP adresi**altında **statik IP adresi**' ne tıklayın. IP adresini, Azure portal yük dengeleyici adresini ayarladığınızda kullandığınız adresle aynı olacak şekilde ayarlayın.

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

1. <a name = "dependencyGroup"></a>SQL Server kullanılabilirlik grubu kaynağını istemci erişim noktasına bağımlı hale getirin.

    a. Yük Devretme Kümesi Yöneticisi ' de **Roller**' e ve ardından kullanılabilirlik grubunuza tıklayın.

    b. **Kaynaklar** sekmesinde, **diğer kaynaklar**altında, kullanılabilirlik kaynak grubu ' na sağ tıklayın ve ardından **Özellikler**' e tıklayın. 

    c. Bağımlılıklar sekmesinde, istemci erişim noktası (dinleyici) kaynağının adını ekleyin.

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. **Tamam**'a tıklayın.

1. <a name="listname"></a>İstemci erişim noktası kaynağını IP adresine bağımlı hale getirin.

    a. Yük Devretme Kümesi Yöneticisi ' de **Roller**' e ve ardından kullanılabilirlik grubunuza tıklayın. 

    b. **Kaynaklar** sekmesinde, **sunucu adı**altında istemci erişim noktası kaynağına sağ tıklayın ve ardından **Özellikler**' e tıklayın. 

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. **Bağımlılıklar** sekmesine tıklayın. IP adresinin bir bağımlılık olduğunu doğrulayın. Değilse, IP adresi için bir bağımlılık ayarlayın. Listelenen birden fazla kaynak varsa, IP adreslerinin, ve bağımlılıkları olduğunu doğrulayın. **Tamam**'a tıklayın. 

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    >[!TIP]
    >Bağımlılıkların doğru yapılandırıldığını doğrulayabilirsiniz. Yük Devretme Kümesi Yöneticisi, roller ' e gidin, kullanılabilirlik grubuna sağ tıklayın, **diğer eylemler**' i ve ardından **bağımlılık raporunu göster**' e tıklayın. Bağımlılıklar doğru yapılandırıldığında, kullanılabilirlik grubu ağ adına bağımlıdır ve ağ adı IP adresine bağımlıdır. 


1. <a name="setparam"></a>PowerShell 'de küme parametrelerini ayarlayın.

   a. Aşağıdaki PowerShell betiğini SQL Server örneklerinizin birine kopyalayın. Ortamınızın değişkenlerini güncelleştirin.

   - `$ListenerILBIP`, kullanılabilirlik grubu dinleyicisi için Azure Yük dengeleyicisinde oluşturduğunuz IP adresidir.
    
   - `$ListenerProbePort`, kullanılabilirlik grubu dinleyicisi için Azure Yük dengeleyicisinde yapılandırdığınız bağlantı noktasıdır.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<IPResourceName>" # the IP Address resource name
   $ListenerILBIP = "<n.n.n.n>" # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ListenerProbePort = <nnnnn>
  
   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ListenerILBIP";"ProbePort"=$ListenerProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. Küme düğümlerinin birinde PowerShell betiğini çalıştırarak küme parametrelerini ayarlayın.  

   > [!NOTE]
   > SQL Server örneklerinizin ayrı bölgelerde olması durumunda PowerShell betiğini iki kez çalıştırmanız gerekir. İlk kez, `$ListenerILBIP` ilk bölgeden ve kullanın `$ListenerProbePort` . İkinci kez, `$ListenerILBIP` ikinci bölgeden ve ' ı kullanın `$ListenerProbePort` . Küme ağ adı ve küme IP kaynak adı her bölge için de farklıdır.

1. Kullanılabilirlik grubu kümesi rolünü çevrimiçi duruma getirin. **Roller**altında **Yük devretme kümesi Yöneticisi** , role sağ tıklayın ve **rolü Başlat**' ı seçin.

Gerekirse, WSFC kümesi IP adresinin küme parametrelerini ayarlamak için yukarıdaki adımları tekrarlayın.

1. WSFC kümesi IP adresinin IP adresi adını alın. **Küme çekirdeği kaynakları**altında **Yük devretme kümesi Yöneticisi** **sunucu adı**' nı bulun.

1. **IP adresi**' ne sağ tıklayın ve **Özellikler**' i seçin.

1. IP adresinin **adını** kopyalayın. Olabilir `Cluster IP Address` . 

1. <a name="setwsfcparam"></a>PowerShell 'de küme parametrelerini ayarlayın.
  
   a. Aşağıdaki PowerShell betiğini SQL Server örneklerinizin birine kopyalayın. Ortamınızın değişkenlerini güncelleştirin.

   - `$ClusterCoreIP`, WSFC çekirdek kümesi kaynağı için Azure Yük dengeleyicisinde oluşturduğunuz IP adresidir. Kullanılabilirlik grubu dinleyicisinin IP adresinden farklıdır.

   - `$ClusterProbePort`, WSFC sistem durumu araştırması için Azure Yük dengeleyicide yapılandırdığınız bağlantı noktasıdır. Kullanılabilirlik grubu dinleyicisinin araştırmasıyla farklıdır.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<ClusterIPResourceName>" # the IP Address resource name
   $ClusterCoreIP = "<n.n.n.n>" # the IP Address of the Cluster IP resource. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ClusterProbePort = <nnnnn> # The probe port from the WSFCEndPointprobe in the Azure portal. This port must be different from the probe port for the availability group listener probe port.
  
   Import-Module FailoverClusters
  
   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ClusterCoreIP";"ProbePort"=$ClusterProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. Küme düğümlerinin birinde PowerShell betiğini çalıştırarak küme parametrelerini ayarlayın.  

>[!WARNING]
>Kullanılabilirlik grubu dinleyicisi durum araştırma bağlantı noktası, küme çekirdeği IP adresi durum araştırma bağlantı noktasından farklı olmalıdır. Bu örneklerde, dinleyici bağlantı noktası 59999 ve küme çekirdek IP adresi durum araştırma bağlantı noktası 58888 ' dir. Her iki bağlantı noktası da bir gelen güvenlik duvarı kuralına izin gerektirir.
