---
title: Öğretici - Azure Trafik Yöneticisi ile web sitesi yanıtını geliştirin
description: Bu öğretici makalede, son derece duyarlı bir web sitesi oluşturmak için trafik yöneticisi profili nasıl oluşturulacak açıklanmaktadır.
services: traffic-manager
author: rohinkoul
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: rohink
ms.openlocfilehash: f7778b60a5e84f5d8900b8e37bfa655a7915d403
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77136395"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Öğretici: Trafik Yöneticisi'ni kullanarak web sitesi yanıtını geliştirin

Bu öğretici, kullanıcı trafiğini en düşük gecikme süreyle web sitesine yönlendirerek son derece duyarlı bir web sitesi oluşturmak için Trafik Yöneticisi'nin nasıl kullanılacağını açıklar. Genellikle, en düşük gecikme gecikmesi ile veri merkezi coğrafi uzaklık en yakın olandır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma
> * IIS çalıştıran VM'lerin DNS adını yapılandırma
> * Gelişmiş web sitesi performansı için Trafik Yöneticisi profili oluşturma
> * Traffic Manager profiline VM uç noktaları ekleme
> * Traffic Manager'ın nasıl çalıştığını görün

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide Traffic Manager'ın çalışmasını uygulamalı olarak görmek için şu sistemleri dağıtmanız gerekir:

- Doğu ABD ve **Batı Avrupa**- **Farklı** Azure bölgelerinde çalışan temel web sitelerinin iki örneği.
- Trafik Yöneticisi test etmek için iki test VM - **Doğu ABD'de** bir VM ve **Batı Avrupa'da**ikinci VM . Test VM'leri, Trafik Yöneticisi'nin kullanıcı trafiğini en düşük gecikme süresini sağladığı yla aynı bölgede çalışan web sitesine nasıl yönlendirirğini göstermek için kullanılır.

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

   ![VM oluşturma](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iIS sunucusunu iki VMs *myIISVMEastUS* ve *myIISVMWestEurope'a*yükler ve ardından varsayılan web sitesi sayfasını güncellersiniz. Özelleştirilmiş web sitesi sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam'ı**seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarıyı alırsanız, bağlantıya devam etmek için **Evet** veya **Devam**et'i seçin.
6. Sunucu masaüstünde, **Windows Administrative Tools**>**Server Manager'a**gidin.
7. VM1’de Windows PowerShell’i başlatın ve IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![IIS yükleme ve web sayfasını özelleştirme](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. *myIISVMEastUS* ile RDP bağlantısını kapatın.
9. IIS'yi yüklemek ve varsayılan web sayfasını özelleştirmek için *myResourceGroupTM2* kaynak grubu içinde VM *myIISVMWestEurope* ile bir RDP bağlantısı oluşturarak 1-8 adımlarını yineleyin.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde, IIS sunucuları için DNS adlarını yapılandırırsınız - *myIISVMEastUS* ve *myIISVMWestEurope.*

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesini seçin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyip **Kaydet**'i seçin.
4. *MyResourceGroupTM2* kaynak grubunda bulunan *myIISVMWestEurope* adlı VM için 1-3 adımlarını tekrarlayın.

### <a name="create-test-vms"></a>Test amaçlı VM'leri oluşturma

Bu bölümde, her Azure bölgesinde **(Doğu ABD** ve **Batı Avrupa)** bir*VM (myVMEastUS* ve *myVMWestEurope)* oluşturursunuz. Bu VM'leri web sitesine göz attığınızda Traffic Manager'ın trafiği en yakın IIS sunucusuna nasıl yönlendirdiğini test etmek için kullanacaksınız.

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

En düşük gecikme ile bitiş noktasına göndererek kullanıcı trafiğini yönlendiren bir Trafik Yöneticisi profili oluşturun.

1. **Networking** > Ekranın sol üst tarafında, **kaynak** > Oluştur**Trafik Yöneticisi profili** > **Oluştur'u**seçin.
2. **Traffic Manager profili oluştur** ekranında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**'u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Adı                   | Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı ile sonuçlanması gerekir.                                   |
    | Yönlendirme yöntemi          | **Performans** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | Kaynak grubu *myResourceGroupTM1'i*seçin. |
    | Konum                | **Doğu ABD**’yi seçin. Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.                              |
    |

    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Kullanıcı trafiğini kullanıcıya en yakın bitiş noktasına yönlendirmek için IIS sunucularını çalıştıran iki VM'yi ekleyin - *myIISVMEastUS* & *myIISVMWestEurope.*

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktası                                   |
    | Adı           | myEastUSEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | Aynı abonelik altında Genel IP adreslerine sahip kaynakların listesini göstermek için **bir Genel IP adresi seçin.** **Kaynak** bölümünde *myIISVMEastUS-ip* adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |        |           |

4. IIS sunucusu VM *myIISVMWestEurope*adlı kamu IP adresi *myIISVMWestEurope-ip* için *myWestEuropeEndpoint* adlı başka bir bitiş noktası eklemek için adımları 2 ve 3 tekrarlayın.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

    ![Traffic Manager uç noktası ekleme](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde, Trafik Yöneticisi'nin kullanıcı trafiğini en az gecikme kolaylığı sağlamak için web sitesini çalıştıran en yakın VM'lere nasıl yönlendirirsiniz test edersiniz. Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:

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

   ![Traffic Manager DNS adı](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam'ı**seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarıyı alırsanız, bağlantıya devam etmek için **Evet** veya **Devam**et'i seçin.
1. Web sitesini görüntülemek için *myVMEastUS* adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını yazın. **VM Doğu ABD'de**bulunan bu yana, **Size Doğu ABD'de**bulunan en yakın IIS sunucusu *myIISVMEastUS* barındırılan en yakın web sitesine yönlendirilir.

   ![Traffic Manager profilini test etme](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. Ardından, 1-5 adımlarını kullanarak **Batı Avrupa'da** bulunan VM *myVMWestEurope'a* bağlanın ve bu VM'den Traffic Manager profil etki alanına göz atın. VM **Batı Avrupa'da**bulunan bu yana, şimdi **Batı Avrupa'da**bulunan IIS sunucusu *myIISVMWestEurope* en yakın barındırılan web sitesine yönlendirilir.

   ![Traffic Manager profilini test etme](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme

İhtiyacınız kalmadığında kaynak gruplarını (**ResourceGroupTM1** ve **ResourceGroupTM2**) silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Trafiği bir uç nokta kümesine dağıtma](traffic-manager-configure-weighted-routing-method.md)
