---
title: Tutorial - Configure subnet traffic routing with Azure Traffic Manager
description: This tutorial explains how to configure Traffic Manager to route traffic from user subnets to specific endpoints.
services: traffic-manager
documentationcenter: ''
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: allensu
ms.openlocfilehash: 00bc453ebb0e467f48bd886fc7c6b6c422693864
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74420267"
---
# <a name="tutorial-direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Tutorial: Direct traffic to specific endpoints based on user subnet using Traffic Manager

Bu makalede alt ağ trafiği yönlendirme yöntemini yapılandırma adımları anlatılmaktadır. **Alt ağ** trafik yönlendirme yöntemi, bir IP adresi kümesini belirli uç noktalarla eşlemenizi sağlar. Traffic Manager'a bir istek geldiğinde kaynak IP adresi incelenir ve bu adresle ilişkilendirilmiş uç nokta döndürülür.

Bu öğreticide alt ağ yönlendirme ile kullanıcı sorgusunun IP adresine bağlı olarak trafik iç web sitesine veya üretim web sitesine yönlendirilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma
> * IIS çalıştıran VM'lerin DNS adını yapılandırma
> * Trafiği kullanıcının alt ağına göre yönlendirmek için bir Traffic Manager profili oluşturma
> * Traffic Manager profiline VM uç noktaları ekleme
> * Traffic Manager'ın nasıl çalıştığını görün

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide Traffic Manager'ın çalışmasını uygulamalı olarak görmek için şu sistemleri dağıtmanız gerekir:

- farklı Azure bölgelerinde çalışan iki basit web sitesi: **Doğu ABD** (iç web sitesi olarak görev yapar) ve **Batı Avrupa** (üretim web sitesi olarak görev yapar).
- Traffic Manager'ı test etmek için iki test amaçlı VM: bir VM **Doğu ABD** bölgesinde, ikinci VM ise **Batı Avrupa** bölgesinde olmalıdır.

Test amaçlı VM'ler Traffic Manager'ın kullanıcı sorgusunun geldiği alt ağa göre kullanıcı trafiğini iç web sitesine veya üretim web sitesine nasıl yönlendirdiğini göstermek için kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesi oluşturmak için aşağıdaki adımları izleyin:

1. Biri **Doğu ABD** diğeri **Batı Avrupa** bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'de de IIS sunucusu yükleyin ve varsayılan web sitesi sayfasını web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma

In this section, you create two VMs *myIISVMEastUS* and *myIISVMWestEurope* in the **East US** and **West Europe** Azure regions.

1. On the upper, left corner of the Azure portal, select **Create a resource** > **Compute** > **Windows Server 2019 Datacenter**.
2. In **Create a virtual machine**, type or select the following values in the **Basics** tab:

   - **Subscription** > **Resource Group**: Select **Create new** and then type **myResourceGroupTM1**.
   - **Instance Details** > **Virtual machine name**: Type *myIISVMEastUS*.
   - **Instance Details** > **Region**:  Select **East US**.
   - **Administrator Account** > **Username**:  Enter a user name of your choosing.
   - **Administrator Account** > **Password**:  Enter a password of your choosing. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
   - **Inbound Port Rules** > **Public inbound ports**: Select **Allow selected ports**.
   - **Inbound Port Rules** > **Select inbound ports**: Select **RDP** and **HTTP** in the pull down box.

3. Select the **Management** tab, or select **Next: Disks**, then **Next: Networking**, then **Next: Management**. Under **Monitoring**, set **Boot diagnostics** to **Off**.
4. **İncele ve oluştur**’u seçin.
5. Review the settings, and then click **Create**.  
6. Follow the steps to create a second VM named *myIISVMWestEurope*, with a **Resource group** name of *myResourceGroupTM2*, a **location** of *West Europe*, and all the other settings the same as *myIISVMEastUS*.
7. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

In this section, you install the IIS server on the two VMs - *myIISVMEastUS* & *myIISVMWestEurope*, and then update the default website page. Özelleştirilmiş web sitesi sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
7. Launch Windows PowerShell on VM *myIISVMEastUS*, and using the following commands to install IIS server and update the default htm file.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

8. Close the RDP connection with *myIISVMEastUS* VM.
9. Repeat steps 1-6 with by creating an RDP connection with the VM *myIISVMWestEurope* within the *myResourceGroupTM2* resource group to install IIS and customize its default web page.
10. Launch Windows PowerShell on *myIISVMWestEurope* VM, and using the following commands to install IIS server and update the default htm file.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. In this section, you configure the DNS names for the IIS servers - *myIISVMEastUS* and *myIISVMWestEurope*.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesini seçin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyip **Kaydet**'i seçin.
4. Repeat steps 1-3, for the VM named *myIISVMWestEurope* that is located in the *myResourceGroupTM2* resource group.

### <a name="create-test-vms"></a>Test amaçlı VM'leri oluşturma

In this section, you create a VM (*myVMEastUS* and *myVMWestEurope*) in each Azure region (**East US** and **West Europe**). You will use these VMs to test how Traffic Manager routes user traffic based on the subnet of the user's query.

1. On the upper, left corner of the Azure portal, select **Create a resource** > **Compute** > **Windows Server 2019 Datacenter**.
2. In **Create a virtual machine**, type or select the following values in the **Basics** tab:

   - **Subscription** > **Resource Group**: Select **myResourceGroupTM1**.
   - **Instance Details** > **Virtual machine name**: Type *myVMEastUS*.
   - **Instance Details** > **Region**:  Select **East US**.
   - **Administrator Account** > **Username**:  Enter a user name of your choosing.
   - **Administrator Account** > **Password**:  Enter a password of your choosing. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
   - **Inbound Port Rules** > **Public inbound ports**: Select **Allow selected ports**.
   - **Inbound Port Rules** > **Select inbound ports**: Select **RDP** in the pull down box.

3. Select the **Management** tab, or select **Next: Disks**, then **Next: Networking**, then **Next: Management**. Under **Monitoring**, set **Boot diagnostics** to **Off**.
4. **İncele ve oluştur**’u seçin.
5. Review the settings, and then click **Create**.  
6. Follow the steps to create a second VM named *myVMWestEurope*, with a **Resource group** name of *myResourceGroupTM2*, a **location** of *West Europe*, and all the other settings the same as *myVMEastUS*.
7. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

İsteğin kaynak IP adresine göre belirli uç noktalar döndürmenizi sağlayacak bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur**'u seçin.
2. **Traffic Manager profili oluştur** ekranında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**'u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Adı                   | Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı ile sonuçlanması gerekir.                                   |
    | Yönlendirme yöntemi          | **Alt ağ** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | **Var olan**’ı seçin ve sonra *myResourceGroupTM1* yazın. |
    | |                              |
    |

    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-subnet-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Add the two VMs running the IIS servers - *myIISVMEastUS* & *myIISVMWestEurope* to route user traffic based on the subnet of the user's query.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktası                                   |
    | Adı           | myInternalWebSiteEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | Genel IP adresine sahip kaynakların aynı abonelik altında listelenmesi için **Genel IP adresi seçin**. **Kaynak** bölümünde *myIISVMEastUS-ip* adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Alt ağ yönlendirme ayarları    |   Add the IP address of *myVMEastUS* test VM. Any user query originating from this VM will be directed to the *myInternalWebSiteEndpoint*.    |

4. Repeat steps 2 and 3 to add another endpoint named *myProdWebsiteEndpoint* for the public IP address *myIISVMWestEurope-ip* that is associated with the IIS server VM named *myIISVMWestEurope*. For **Subnet routing settings**, add the IP address of the test VM - *myVMWestEurope*. Bu test amaçlı VM'den gelen kullanıcı sorguları *myProdWebsiteEndpoint* adlı uç noktaya yönlendirilir.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde Traffic Manager'ın belirli bir alt ağdan gelen trafiği belirli bir uç noktaya nasıl yönlendirdiğini test edeceksiniz. Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:

1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görmek için şu adımları izleyin:
    - From the test VM (*myVMEastUS*) that is located in the **East US** region, in a web browser, browse to the DNS name of your Traffic Manager profile.
    - From the test VM (*myVMWestEurope*) that is located in the **West Europe** region, in a web browser, browse to the DNS name of your Traffic Manager profile.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme

Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır.

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz **Traffic Manager profili** adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
2. **Genel Bakış**'a tıklayın.
3. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Web sitesini görüntülemek için *myVMEastUS* adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını yazın. Since the VM *myVMEastUS* IP address is associated with the endpoint *myInternalWebsiteEndpoint*, the web browser launches the Test website server - *myIISVMEastUS*.

7. Next, connect to the VM *myVMWestEurope* located in **West Europe** using steps 1-5 and browse to the Traffic Manager profile domain name from this VM. Since the VM *myVMWestEurope* IP address is associated with the endpoint *myProductionWebsiteEndpoint*, the web browser launches the Test website server - *myIISVMWestEurope*.

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme

İhtiyacınız kalmadığında kaynak gruplarını (**ResourceGroupTM1** ve **ResourceGroupTM2**) silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
