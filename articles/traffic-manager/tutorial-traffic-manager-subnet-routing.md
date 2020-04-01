---
title: Öğretici - Azure Trafik Yöneticisi ile alt net trafiğini yapılandırma
description: Bu öğretici, trafiği kullanıcı alt ağlarından belirli uç noktalara yönlendirmek için Trafik Yöneticisi'ni nasıl yapılandırılabildiğini açıklar.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: rohink
ms.openlocfilehash: 49e0bce6eea8fac32f49bb905c225e898e709af0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77136295"
---
# <a name="tutorial-direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Öğretici: Trafik Yöneticisi'ni kullanarak kullanıcı alt ağına göre belirli uç noktalara doğrudan trafik

Bu makalede alt ağ trafiği yönlendirme yöntemini yapılandırma adımları anlatılmaktadır. **Alt ağ** trafik yönlendirme yöntemi, bir IP adresi kümesini belirli uç noktalarla eşlemenizi sağlar. Traffic Manager'a bir istek geldiğinde kaynak IP adresi incelenir ve bu adresle ilişkilendirilmiş uç nokta döndürülür.

Bu öğreticide alt ağ yönlendirme ile kullanıcı sorgusunun IP adresine bağlı olarak trafik iç web sitesine veya üretim web sitesine yönlendirilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma
> * IIS çalıştıran VM'lerin DNS adını yapılandırma
> * Trafiği kullanıcının alt ağına göre yönlendirmek için bir Traffic Manager profili oluşturma
> * Traffic Manager profiline VM uç noktaları ekleme
> * Traffic Manager'ın nasıl çalıştığını görün

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide Traffic Manager'ın çalışmasını uygulamalı olarak görmek için şu sistemleri dağıtmanız gerekir:

- farklı Azure bölgelerinde çalışan iki basit web sitesi: **Doğu ABD** (iç web sitesi olarak görev yapar) ve **Batı Avrupa** (üretim web sitesi olarak görev yapar).
- Traffic Manager'ı test etmek için iki test amaçlı VM: bir VM **Doğu ABD** bölgesinde, ikinci VM ise **Batı Avrupa** bölgesinde olmalıdır.

Test amaçlı VM'ler Traffic Manager'ın kullanıcı sorgusunun geldiği alt ağa göre kullanıcı trafiğini iç web sitesine veya üretim web sitesine nasıl yönlendirdiğini göstermek için kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesi oluşturmak için aşağıdaki adımları izleyin:

1. Biri **Doğu ABD** diğeri **Batı Avrupa** bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'de de IIS sunucusu yükleyin ve varsayılan web sitesi sayfasını web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma

Bu bölümde, **Doğu ABD** ve **Batı Avrupa** Azure bölgelerinde iki VMs *myIISVMEastUS* ve *myIISVMWestEurope* oluşturursunuz.

1. Azure portalının sol üst köşesinde, **kaynak** > **Oluştur Windows** > **Server 2019 Datacenter'ı**seçin.
2. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:

   - **Abonelik** > **Kaynak Grubu**: Yeni **Oluştur'u** seçin ve ardından **myResourceGroupTM1**yazın.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: *myIISVMEastUS*yazın.
   - **Örnek Ayrıntılar** > **Bölge**: **Doğu ABD**seçin.
   - **Yönetici Hesabı** > **Kullanıcı Adı**: Seçtiğiniz bir kullanıcı adı girin.
   - **Yönetici Hesap** > **Şifresi**: Seçtiğiniz bir şifre girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
   - **Gelen Bağlantı Noktası Kuralları** > **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen Bağlantı Noktası Kuralları** > **Gelen bağlantı noktalarını seçin**: Çekme kutusunda **RDP** ve **HTTP'yi** seçin.

3. **Yönetim** sekmesini seçin veya **Sonraki'ni seçin: Diskler,** sonra **Sonraki: Ağ,** sonra **Sonraki: Yönetim**. **İzleme**altında, **Boot tanılamayı** **Kapalı**olarak ayarlayın.
4. **İncele ve oluştur**’u seçin.
5. Ayarları gözden geçirin ve ardından **Oluştur'u**tıklatın.  
6. *MyIISVMWestEurope*adlı ikinci bir VM oluşturmak için adımları izleyin , *myResourceGroupTM2*bir **Kaynak grubu** adı ile , Batı **Avrupa'nın** bir yer , ve tüm diğer ayarları *myIISVMEastUS*aynı . *West Europe*
7. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iIS sunucusunu iki VM'ye yüklersiniz - *myIISVMEastUS* & *myIISVMWestEurope*, ve sonra varsayılan web sitesi sayfasını güncellersiniz. Özelleştirilmiş web sitesi sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam'ı**seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarıyı alırsanız, bağlantıya devam etmek için **Evet** veya **Devam**et'i seçin.
6. Sunucu masaüstünde, **Windows Administrative Tools**>**Server Manager'a**gidin.
7. VM *myIISVMEastUS'ta*Windows PowerShell'i başlatın ve IIS sunucusunu yüklemek ve varsayılan HTM dosyasını güncelleştirmek için aşağıdaki komutları kullanarak.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

8. RDP bağlantısını *myIISVMEastUS* VM ile kapatın.
9. IIS'yi yüklemek ve varsayılan web sayfasını özelleştirmek için *myResourceGroupTM2* kaynak grubu içinde VM *myIISVMWestEurope* ile bir RDP bağlantısı oluşturarak 1-6 adımlarını tekrarlayın.
10. Windows PowerShell'i *myIISVMWestEurope* VM'de başlatın ve IIS sunucusunu yüklemek ve varsayılan HTM dosyasını güncelleştirmek için aşağıdaki komutları kullanarak.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde, IIS sunucuları için DNS adlarını yapılandırırsınız - *myIISVMEastUS* ve *myIISVMWestEurope.*

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesini seçin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyip **Kaydet**'i seçin.
4. *MyResourceGroupTM2* kaynak grubunda bulunan *myIISVMWestEurope* adlı VM için 1-3 adımlarını tekrarlayın.

### <a name="create-test-vms"></a>Test amaçlı VM'leri oluşturma

Bu bölümde, her Azure bölgesinde **(Doğu ABD** ve **Batı Avrupa)** bir*VM (myVMEastUS* ve *myVMWestEurope)* oluşturursunuz. Bu VM'leri, Trafik Yöneticisi'nin kullanıcı sorgusunun alt ağına göre kullanıcı trafiğini nasıl güzergahlar olduğunu test etmek için kullanırsınız.

1. Azure portalının sol üst köşesinde, **kaynak** > **Oluştur Windows** > **Server 2019 Datacenter'ı**seçin.
2. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:

   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupTM1'i**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: *myVMEastUS*yazın.
   - **Örnek Ayrıntılar** > **Bölge**: **Doğu ABD**seçin.
   - **Yönetici Hesabı** > **Kullanıcı Adı**: Seçtiğiniz bir kullanıcı adı girin.
   - **Yönetici Hesap** > **Şifresi**: Seçtiğiniz bir şifre girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
   - **Gelen Bağlantı Noktası Kuralları** > **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen Bağlantı Noktası Kuralları** > **Gelen bağlantı noktalarını seçin**: Çekme kutusunda **RDP'yi** seçin.

3. **Yönetim** sekmesini seçin veya **Sonraki'ni seçin: Diskler,** sonra **Sonraki: Ağ,** sonra **Sonraki: Yönetim**. **İzleme**altında, **Boot tanılamayı** **Kapalı**olarak ayarlayın.
4. **İncele ve oluştur**’u seçin.
5. Ayarları gözden geçirin ve ardından **Oluştur'u**tıklatın.  
6. *MyVMWestEurope*adlı ikinci bir VM oluşturmak için adımları izleyin , *myResourceGroupTM2*bir **Kaynak grup** adı ile , Batı **Avrupa'nın** bir yer , ve tüm diğer ayarları *myVMEastUS*aynı . *West Europe*
7. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

İsteğin kaynak IP adresine göre belirli uç noktalar döndürmenizi sağlayacak bir Traffic Manager profili oluşturun.

1. **Networking** > Ekranın sol üst tarafında, **kaynak** > Oluştur**Trafik Yöneticisi profili** > **Oluştur'u**seçin.
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

Kullanıcı sorgusunun alt ağına göre kullanıcı trafiğini yönlendirmek için IIS sunucularını çalıştıran iki VM'yi ekleyin - *myIISVMEastUS* & *myIISVMWestEurope.*

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktası                                   |
    | Adı           | myInternalWebSiteEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | Aynı abonelik altında Genel IP adreslerine sahip kaynakların listesini göstermek için **bir Genel IP adresi seçin.** **Kaynak** bölümünde *myIISVMEastUS-ip* adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Alt ağ yönlendirme ayarları    |   *myVMEastUS* test VM IP adresini ekleyin. Bu VM kaynaklanan herhangi bir kullanıcı sorgusu *myInternalWebSiteEndpoint*yönlendirilir.    |

4. *MyIISVMWestEurope*adlı IIS sunucusu VM ile ilişkili kamu IP adresi *myIISVMWestEurope-ip* için *myProdWebsiteEndpoint* adlı başka bir uç nokta eklemek için adımları 2 ve 3 tekrarlayın. **Subnet yönlendirme ayarları**için, test VM IP adresini ekleyin - *myVMWestEurope*. Bu test amaçlı VM'den gelen kullanıcı sorguları *myProdWebsiteEndpoint* adlı uç noktaya yönlendirilir.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde Traffic Manager'ın belirli bir alt ağdan gelen trafiği belirli bir uç noktaya nasıl yönlendirdiğini test edeceksiniz. Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:

1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görmek için şu adımları izleyin:
    - **Doğu ABD** bölgesinde bulunan*VM (myVMEastUS)* testinden, bir web tarayıcısında, Trafik Yöneticisi profilinizin DNS adına göz atın.
    - **Batı Avrupa** bölgesinde bulunan vm *(myVMWestEurope)* testinden, bir web tarayıcısında, Trafik Yöneticisi profilinizin DNS adına göz atın.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme

Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır.

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1. Portalın arama çubuğunda, önceki bölümde oluşturduğunuz **Trafik Yöneticisi profil** adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
2. **Genel Bakış**'a tıklayın.
3. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam'ı**seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarıyı alırsanız, bağlantıya devam etmek için **Evet** veya **Devam**et'i seçin.
6. Web sitesini görüntülemek için *myVMEastUS* adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını yazın. VM *myVMEastUS* IP adresi uç noktası *myInternalWebsiteEndpoint*ile ilişkili olduğundan, web tarayıcısı Test web sitesi sunucusu başlattı - *myIISVMEastUS*.

7. Ardından, 1-5 adımlarını kullanarak **Batı Avrupa'da** bulunan VM *myVMWestEurope'a* bağlanın ve bu VM'den Traffic Manager profil etki alanına göz atın. VM *myVMWestEurope* IP adresi uç noktası *myProductionWebsiteEndpoint*ile ilişkili olduğundan, web tarayıcısı Test web sitesi sunucusu başlattı - *myIISVMWestEurope*.

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme

İhtiyacınız kalmadığında kaynak gruplarını (**ResourceGroupTM1** ve **ResourceGroupTM2**) silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
