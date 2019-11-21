---
title: 'Tutorial: Load balance internet traffic to VMs - Azure portal'
titleSuffix: Azure Load Balancer
description: Bu öğreticide, Azure portalını kullanarak Standard Load Balancer'ın nasıl oluşturulacağı ve yönetileceği gösterilir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create and Standard Load Balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 4d4703ccb4ee96eb69a780f91eae1eb6da9e1578
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225191"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Tutorial: Load balance internet traffic to VMs using the Azure portal

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. In this tutorial, you learn about the different components of the Azure Standard Load Balancer that distribute internet traffic to VMs and provide high availability. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * Create an Azure Load Balancer
> * Create Load Balancer resources
> * Sanal makineler oluşturma ve IIS sunucusunu yükleme
> * View Load Balancer in action
> * Add and remove VMs from a Load Balancer

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma

In this section, you create a Standard Load Balancer that helps load balance virtual machines. Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Standard Load Balancer oluşturduğunuzda, Standard Load Balancer için ön uç (varsayılan olarak *LoadBalancerFrontend* adını alır) olarak yapılandırılmış yeni bir Standart Genel IP adresi de oluşturmanız gerekir. 

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. In the **Basics** tab of the **Create load balancer** page, enter or select the following information, accept the defaults for the remaining settings, and then select **Review + create**:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Select **Create new** and type *myResourceGroupSLB* in the text box.|
    | Adı                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | Select **Public**.                                        |
    | SKU           | Select **Standard**.                          |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Public IP address name              | Type *myPublicIP* in the text box.   |
    |Availability zone| Select **Zone redundant**.    |

3. In the **Review + create** tab, click **Create**.

   ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Create Load Balancer resources

In this section, you configure Load Balancer settings for a backend address pool, a health probe, and specify a balancer rule.

### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

To distribute traffic to the VMs, a backend address pool contains the IP addresses of the virtual (NICs) connected to the Load Balancer. Create the backend address pool *myBackendPool* to include virtual machines for load-balancing internet traffic.

1. Select **All services** in the left-hand menu, select **All resources**, and then click **myLoadBalancer** from the resources list.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. On the **Add a backend pool** page, for name, type *myBackendPool*, as the name for your backend pool, and then select **Add**.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

To allow the Load Balancer to monitor the status of your app, you use a health probe. The health probe dynamically adds or removes VMs from the Load Balancer rotation based on their response to health checks. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Select **All services** in the left-hand menu, select **All resources**, and then click **myLoadBalancer** from the resources list.
2. **Ayarlar** bölümünde **Durum araştırmaları**’na ve sonra **Ekle**’ye tıklayın.
3. Durum araştırması oluşturmak için şu değerleri kullanın:
     
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | Enter *myHealthProbe*. |
    | Protokol | Select **HTTP**. |
    | Bağlantı noktası | Enter *80*.|
    | Interval | Enter *15* for number of **Interval** in seconds between probe attempts. |
    | Unhealthy threshold | Select *2* for number of **Unhealthy threshold** or consecutive probe failures that must occur before a VM is considered unhealthy.|
    
4. **Tamam**’ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma

Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Create a Load Balancer rule *myLoadBalancerRuleWeb* for listening to port 80 in the frontend *FrontendLoadBalancer* and sending load-balanced network traffic to the backend address pool *myBackEndPool* also using port 80.

1. Select **All services** in the left-hand menu, select **All resources**, and then click **myLoadBalancer** from the resources list.
2. **Ayarlar** bölümünde **Yük dengeleme kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. Use these values to configure the load-balancing rule:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | Enter *myHTTPRule*. |
    | Protokol | Select **TCP**. |
    | Bağlantı noktası | Enter *80*.|
    | Backend port | Enter *80*. |
    | Backend pool | Select *myBackendPool*.|
    | Durum yoklaması | Select *myHealthProbe*. |
    
4. Diğer varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

In this section, you create a virtual network, create three virtual machines for the backend pool of the Load Balancer, and then install IIS on the virtual machines to help test the Load Balancer.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturun

1. On the upper-left side of the screen, select **Create a resource** > **Networking** > **Virtual network**.
2. In **Create virtual network**, enter or select this information:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *myVNet* yazın. |
    | Adres alanı | Enter *10.1.0.0/16*. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | Select existing resource - *myResourceGroupSLB*. |
    | Konum | **Batı Avrupa**'yı seçin.|
    | Subnet - Name | *myBackendSubnet* yazın. |
    | Alt Ağ - Adres aralığı | Enter *10.1.0.0/24*. |
    
3. Leave the rest of the defaults and select **Create**.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Standard Load Balancer only supports VMs with Standard IP addresses in the backend pool. In this section, you will create three VMs (*myVM1*, *myVM2*, and *myVM3*) with a Standard public IP address in three different zones (*Zone 1*, *Zone 2*, and *Zone 3*) that are added to the backend pool of the Standard Load Balancer that was created earlier.

1. On the upper-left side of the portal, select **Create a resource** > **Compute** > **Windows Server 2016 Datacenter**. 
   
1. In **Create a virtual machine**, type or select the following values in the **Basics** tab:
   - **Subscription** > **Resource Group**: Select **myResourceGroupSLB**.
   - **Instance Details** > **Virtual machine name**: Type *myVM1*.
   - **Instance Details** > **Region** > select **West Europe**.
   - **Instance Details** > **Availability Options** > Select **Availability zones**. 
   - **Instance Details** > **Availability zone** > Select **1**.
  
1. Select the **Networking** tab, or select **Next: Disks**, then **Next: Networking**. 
   
   - Make sure the following are selected:
       - **Virtual network**: **myVnet**
       - **Subnet**: **myBackendSubnet**
       - **Public IP** > select **Create new**, and in the **Create public IP address** window, for **SKU**, select **Standard**, and for **Availability zone**, select **Zone-redundant**
      
   - To create a new network security group (NSG), a type of firewall, under **Network Security Group**, select **Advanced**. 
       1. In the **Configure network security group** field, select **Create new**. 
       1. Type *myNetworkSecurityGroup*, and select **OK**.

   - To make the VM a part of the Load Balancer's backend pool, complete the following steps:
        - In **Load Balancing**, for **Place this virtual machine behind an existing load balancing solution?** , select **Yes**.
        - In **Load balancing settings**, for **Load balancing options**, select **Azure load balancer**.
        - For **Select a load balancer**, *myLoadBalancer*. 
1. Select the **Management** tab, or select **Next** > **Management**. Under **Monitoring**, set **Boot diagnostics** to **Off**. 
1. **İncele ve oluştur**’u seçin.   
1. Review the settings, and then select **Create**.
1. Follow the steps to create two additional VMs - *myVM2* and *myVM3*, with a Standard SKU public IP address in **Availability zone** **2** and **3** respectively, and all the other settings the same as *myVM1*.  

### <a name="create-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

In this section, you create a network security group rule to allow inbound connections using HTTP.

1. Select **All services** in the left-hand menu, select **All resources**, and then from the resources list click **myNetworkSecurityGroup** that is located in the **myResourceGroupSLB** resource group.
2. **Ayarlar**’ın altında **Gelen güvenlik kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. 80 numaralı bağlantı noktasını kullanarak gelen HTTP bağlantılarına izin vermek için *myHTTPRule* adlı gelen güvenlik kuralı için şu değerleri girin:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *80*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *100*
    - Ad için *myHTTPRule*
    - Açıklama için *HTTP’ye İzin Ver*
4. **Add (Ekle)** seçeneğini belirleyin.

### <a name="install-iis-on-vms"></a>VM’lere IIS yükleme

1. Select **All services** in the left-hand menu, select **All resources**, and then from the resources list click **myVM1** that is located in the *myResourceGroupSLB* resource group.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. **Sanal makineye bağlanın** açılır penceresinde **RDP Dosyasını İndir**'i seçin ve indirilen RDP dosyasını açın.
4. **Uzak Masaüstü Bağlantısı** penceresinde **Bağlan**'a tıklayın.
5. VM oluşturma işlemleri sırasında belirlediğiniz kimlik bilgilerini kullanarak VM'de oturum açın. *myVM1* adlı sanal makinede uzak masaüstü oturumu başlatılır.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Windows PowerShell** bölümüne gidin.
7. PowerShell Penceresinde aşağıdaki komutları çalıştırarak IIS sunucusunu yükleyin, varsayılan iisstart.htm dosyasını kaldırın ve ardından VM’nin adını gösteren yeni bir iisstart.htm dosyası ekleyin:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. *myVM1* ile RDP oturumunu kapatın.
7. IIS’yi ve *myVM2* ve *myVM3*’teki güncelleştirilmiş iisstart.htm dosyasını yüklemek için 1 ile 6 arasındaki adımları tekrarlayın.

## <a name="test-the-load-balancer"></a>Test the Load Balancer
1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. Select **All services** in the left-hand menu, select **All resources**, and then click **myPublicIP**.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

      ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

To see the Load Balancer distribute traffic across the three VMs running your app, you can force-refresh your web browser.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Arka uç havuzunda VM'leri kaldırma veya ekleme
Uygulamanızı çalıştıran VM’lerde işletim sistemi güncelleştirmelerini yükleme gibi bakım işlemleri gerçekleştirmeniz gerekebilir. Uygulamanıza gelen trafiği fazla trafiği yönetmek için ek VM’lere ihtiyaç duyabilirsiniz. This section shows you how to remove or add a VM (*myVM1*) from the Load Balancer.

### <a name="remove-vm-from-a-backend-pool"></a>Remove VM from a backend pool
To remove *myVM1* from the backend pool, complete the following steps:

1. Select **All services** in the left-hand menu, select **All resources**, and then click **myLoadBalancer** from the resources list.
2. **Ayarlar**'ın altında **Arka uç havuzları**'na tıklayın, ardından arka uç havuzunun listesinde **myBackendPool**'a tıklayın.
3. On the **myBackendPool** page, to remove *VM1* select the delete icon at the end of the row that displays *myVM1*, and then click **Save**.

Artık *myVM1* arka uç adres havuzunda yer almadığından, *myVM1* üzerinde yazılım güncelleştirmelerini yükleme gibi tüm bakım görevlerini gerçekleştirebilirsiniz. In the absence of *VM1*, the load is now balanced across *myVM2* and *myVM3*. 

### <a name="add-vm-to-a-backend-pool"></a>Add VM to a backend pool
To add *myVM1* back to the backend pool, complete the following steps:

1. Select **All services** in the left-hand menu, select **All resources**, and then select **myVM1** from the resources list.
2. In the **VM1** page, under **Settings**, select **Networking**.
3. In the **Networking** page, select the **Load balancing** tab, and then select **Add load balancing**.
4. In the **Add load balancing** page, do the following:
   1. For **Load balancing options**, select **Azure load balancer**.
   2. For **Select a load balancer**, select *myLoadBalancer*.
   3. For **Select a backend pool**, select *myBackendPool*. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

When they are no longer needed, delete the resource group, Load Balancer, and all related resources. To do so, select the *myResouceGroupSLB* resource group that contains the Load Balancer, and then select  **Delete**.

## <a name="next-steps"></a>Sonraki adımlar

In this tutorial, you created a Standard Load Balancer, attached VMs to it, configured the Load Balancer traffic rule, health probe, and then tested the Load Balancer. Ayrıca, yük dengeli kümeden bir VM kaldırdınız ve VM'yi arka uç adres havuzuna geri eklediniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
