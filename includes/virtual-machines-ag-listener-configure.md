---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 73ba78eca710f0b98b2a209494519cb8003e554b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75468854"
---
Kullanılabilirlik grubu dinleyicisi, SQL Server kullanılabilirlik grubunun dinlediği bir IP adresi ve ağ adıdır. Kullanılabilirlik grubu dinleyicisini oluşturmak için aşağıdakileri yapın:

1. <a name="getnet"></a>Küme ağ kaynağının adını alın.

    a. Birincil yinelemeyi barındıran Azure sanal makinesine bağlanmak için RDP'yi kullanın. 

    b. Failover Cluster Yöneticisi'ni açın.

    c. **Ağlar** düğümünü seçin ve küme ağ adını not edin. PowerShell komut `$ClusterNetworkName` dosyasındaki değişkende bu adı kullanın. Aşağıdaki resimde küme ağ adı **Küme Ağı 1:**

   ![Küme Ağ Adı](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

1. <a name="addcap"></a>İstemci erişim noktasını ekleyin.  
    İstemci erişim noktası, uygulamaların kullanılabilirlik grubundaki veritabanlarına bağlanmak için kullandığı ağ adıdır. Failover Cluster Manager'da istemci erişim noktasını oluşturun.

    a. Küme adını genişletin ve ardından **Görevler'i**tıklatın.

    b. **Roller** bölmesinde, kullanılabilirlik grubu adını sağ tıklatın ve sonra Kaynak > **İstemci Erişim Noktası** **Ekle'yi**seçin.

   ![İstemci Erişim Noktası](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. **Ad** kutusunda, bu yeni dinleyici için bir ad oluşturun. 
   Yeni dinleyicinin adı, uygulamaların SQL Server kullanılabilirlik grubundaki veritabanlarına bağlanmak için kullandığı ağ adıdır.

    d. Dinleyiciyi oluşturmayı bitirmek için **İleri'yi** iki kez tıklatın ve sonra **Bitir'i**tıklatın. Bu noktada dinleyiciyi veya kaynağı çevrimiçi duruma getirmeyin.

1. Kullanılabilirlik grubu küme rolünü çevrimdışına alın. **Roller**altındaki **Failover Cluster Manager'da** rolü sağ tıklatın ve **Rolü Durdur'u**seçin.

1. <a name="congroup"></a>Kullanılabilirlik grubu için IP kaynağını yapılandırın.

    a. **Kaynaklar** sekmesini tıklatın ve ardından oluşturduğunuz istemci erişim noktasını genişletin.  
    İstemci erişim noktası çevrimdışı.

   ![İstemci Erişim Noktası](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. IP kaynağını sağ tıklatın ve ardından özellikleri tıklatın. IP adresinin adını not alın ve `$IPResourceName` PowerShell komut dosyasındaki değişkende kullanın.

    c. **IP Adresi**altında Statik **IP Adresi'ni**tıklatın. IP adresini Azure portalında yük dengeleyici adresini ayarlarken kullandığınız adres olarak ayarlayın.

   ![IP Kaynağı](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

1. <a name = "dependencyGroup"></a>SQL Server kullanılabilirlik grubu kaynağını istemci erişim noktasına bağımlı hale getirin.

    a. Failover Cluster Manager'da **Roller'i**tıklatın ve ardından kullanılabilirlik grubunuzun düğmesini tıklatın.

    b. **Kaynaklar** sekmesinde, **Diğer Kaynaklar**altında, kullanılabilirlik kaynak grubunu sağ tıklatın ve ardından **Özellikler'i**tıklatın. 

    c. Bağımlılıklar sekmesinde, istemci erişim noktası (dinleyici) kaynağının adını ekleyin.

   ![IP Kaynağı](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. **Tamam**'a tıklayın.

1. <a name="listname"></a>İstemci erişim noktası kaynağını IP adresine bağımlı hale getirin.

    a. Failover Cluster Manager'da **Roller'i**tıklatın ve ardından kullanılabilirlik grubunuzun düğmesini tıklatın. 

    b. **Kaynaklar** sekmesinde, **Sunucu Adı**altındaki istemci erişim noktası kaynağını sağ tıklatın ve ardından **Özellikler'i**tıklatın. 

   ![IP Kaynağı](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. **Bağımlılıklar** sekmesini tıklatın. IP adresinin bağımlılık olduğunu doğrulayın. Değilse, IP adresine bir bağımlılık ayarlayın. Listelenen birden çok kaynak varsa, IP adreslerinde OR, AND değil, bağımlılıklar olduğunu doğrulayın. **Tamam**'a tıklayın. 

   ![IP Kaynağı](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    >[!TIP]
    >Bağımlılıkların doğru şekilde yapılandırıldığından doğrulayabilirsiniz. Failover Cluster Manager'da Roller'e gidin, kullanılabilirlik grubuna sağ tıklayın, **Daha Fazla Eylem'i**tıklatın ve ardından **Bağımlılık Raporunu Göster'i**tıklatın. Bağımlılıklar doğru şekilde yapılandırıldığında, kullanılabilirlik grubu ağ adına ve ağ adı IP adresine bağlıdır. 


1. <a name="setparam"></a>PowerShell'de küme parametrelerini ayarlayın.

   a. Aşağıdaki PowerShell komut dosyasını SQL Server örneklerinden birine kopyalayın. Ortamınız için değişkenleri güncelleştirin.

   - `$ListenerILBIP`kullanılabilirlik grubu dinleyicisi için Azure yük dengeleyicisi üzerinde oluşturduğunuz IP adresidir.
    
   - `$ListenerProbePort`kullanılabilirlik grubu dinleyicisi için Azure yük dengeleyicisi üzerinde yapılandırdığınız bağlantı noktasıdır.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<IPResourceName>" # the IP Address resource name
   $ListenerILBIP = "<n.n.n.n>" # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ListenerProbePort = <nnnnn>
  
   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ListenerILBIP";"ProbePort"=$ListenerProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. PowerShell komut dosyasını küme düğümlerinden birinde çalıştırarak küme parametrelerini ayarlayın.  

   > [!NOTE]
   > SQL Server örnekleriniz ayrı bölgelerdeyse, PowerShell komut dosyasını iki kez çalıştırmanız gerekir. İlk kez, ilk `$ListenerILBIP` `$ListenerProbePort` bölgeden ve kullanın. İkinci kez, ikinci `$ListenerILBIP` `$ListenerProbePort` bölgeden ve kullanın. Küme ağ adı ve küme IP kaynak adı da her bölge için farklıdır.

1. Kullanılabilirlik grubu küme rolünü çevrimiçi duruma getirin. **Roller**altındaki **Failover Cluster** Manager'da, rolü sağ tıklatın ve **Rolü Başlat'ı**seçin.

Gerekirse, WSFC küme IP adresi için küme parametrelerini ayarlamak için yukarıdaki adımları yineleyin.

1. WSFC Cluster IP adresinin IP adresinin adını alın. **Küme Çekirdek Kaynakları**altında **Failover Cluster Manager,** **Sunucu Adı**bulun.

1. **IP Adresi'ni**sağ tıklatın ve **Özellikler'i**seçin.

1. IP adresinin **adını** kopyalayın. `Cluster IP Address`Olabilir. 

1. <a name="setwsfcparam"></a>PowerShell'de küme parametrelerini ayarlayın.
  
   a. Aşağıdaki PowerShell komut dosyasını SQL Server örneklerinden birine kopyalayın. Ortamınız için değişkenleri güncelleştirin.

   - `$ClusterCoreIP`WSFC çekirdek küme kaynağı için Azure yük bakiyesi üzerinde oluşturduğunuz IP adresidir. Kullanılabilirlik grubu dinleyicisi için IP adresinden farklıdır.

   - `$ClusterProbePort`WSFC sistem durumu sondası için Azure yük dengeleyicisi üzerinde yapılandırdığınız bağlantı noktasıdır. Kullanılabilirlik grubu dinleyicisi için sondadan farklıdır.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<ClusterIPResourceName>" # the IP Address resource name
   $ClusterCoreIP = "<n.n.n.n>" # the IP Address of the Cluster IP resource. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ClusterProbePort = <nnnnn> # The probe port from the WSFCEndPointprobe in the Azure portal. This port must be different from the probe port for the availability group listener probe port.
  
   Import-Module FailoverClusters
  
   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ClusterCoreIP";"ProbePort"=$ClusterProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. PowerShell komut dosyasını küme düğümlerinden birinde çalıştırarak küme parametrelerini ayarlayın.  

>[!WARNING]
>Kullanılabilirlik grubu dinleyici saglik sondası bağlantı noktası, küme çekirdeği IP adresi saglik sondası bağlantı noktasından farkli olmali. Bu örneklerde dinleyici bağlantı noktası 59999, küme çekirdeği IP adresi sağlık sondası bağlantı noktası 58888'dir. Her iki bağlantı noktası da gelen güvenlik duvarı kuralına izin verme gerektirir.
