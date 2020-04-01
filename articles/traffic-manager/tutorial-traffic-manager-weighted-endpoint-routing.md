---
title: Öğretici:Trafiği ağırlıklı uç noktalara yönlendirin - Azure Trafik Yöneticisi
description: Bu öğretici makalede, Traffic Manager'ı kullanarak trafiği ağırlıklı uç noktalara yönlendirme açıklanmaktadır.
services: traffic-manager
author: rohinkoul
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: rohink
ms.openlocfilehash: a4738b2e36786cd627f53af3e36bd8f1e3fbc375
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76939474"
---
# <a name="tutorial-control-traffic-routing-with-weighted-endpoints-by-using-traffic-manager"></a>Öğretici: Traffic Manager'ı kullanarak ağırlıklı uç noktalar ile trafiği denetleme

Bu öğreticide, ağırlıklı yönlendirme yöntemini kullanarak kullanıcı trafiğini uç noktalar arasında yönlendirmeyi denetlemek için Azure Traffic Manager'ı kullanma adımları açıklanmaktadır. Bu yönlendirme yönteminde Traffic Manager profil yapılandırmasında her uç noktasına bir ağırlık değeri atarsınız. Kullanıcı trafiği, her bir uç noktaya atanan ağırlık değerine göre yönlendirilir. Ağırlık 1 ile 1.000 arasında bir tamsayıdır. Bir uç noktaya ne kadar yüksek bir ağırlık değeri atanırsa uç nokta o kadar yüksek önceliğe sahip olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma.
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma.
> * IIS çalıştıran VM'lerin DNS adını yapılandırma.
> * Traffic Manager profili oluşturma.
> * Traffic Manager profiline VM uç noktaları ekleme.
> * Traffic Manager'ın nasıl çalıştığını görün.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Traffic Manager'ın nasıl çalıştığını görmek için bu öğretici kapsamında aşağıdaki kaynakları dağıtın:

- Farklı Azure bölgelerinde (Doğu ABD ve Batı Avrupa) çalışan iki temel web sitesi örneği.
- Traffic Manager'ı test etmek için iki test amaçlı VM: bir VM Doğu ABD bölgesinde, diğeri ise Batı Avrupa bölgesinde olmalıdır. Traffic Manager’ın kullanıcı trafiğini uç noktasına daha yüksek bir ağırlık atanan bir web sitesine nasıl yönlendirdiğini göstermek için test VM’leri kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesini oluşturmak için, aşağıdaki adımları tamamlayın:

1. Biri Doğu ABD diğeri Batı Avrupa bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'ye de IIS sunucusu yükleyin. Varsayılan web sayfasını, web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma

Bu bölümde, Doğu ABD ve Batı Avrupa Azure bölgelerinde iki VM *(myIISVMEastUS* ve *myIISVMWestEurope)* oluşturursunuz.

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

#### <a name="install-iis-and-customize-the-default-webpage"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iIS sunucusunu iki VMs myIISVMEastUS ve myIISVMWestEurope'a yükler ve varsayılan web sayfasını güncellersiniz. Özelleştirilmiş web sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myIISVMEastUS** seçeneğini belirleyin.
2. **Genel Bakış** sayfasında **Bağlan**'ı seçin. **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen .rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. VM'yi oluştururken yapılandırdığınız kullanıcı adı ve parolayı girin. VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için **farklı** > **bir hesap kullanma**seçeneğini seçmeniz gerekebilir.
4. **Tamam'ı**seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Sunucu masaüstünde, Windows **Administrative Tools** > **Server Manager'a**göz atın.
7. VM1'de Windows PowerShell'i açın. IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default .htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom .htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

    ![IIS yükleme ve web sayfasını özelleştirme](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)

8. **myIISVMEastUS** ile RDP bağlantısını kapatın.
9. 1-8 arası adımları tekrarlayın. IIS'yi yüklemek ve varsayılan web sayfasını özelleştirmek için **myResourceGroupTM2** kaynak grubu içinde VM **myIISVMWestEurope** ile bir RDP bağlantısı oluşturun.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde, MyIISVMEastUS ve myIISVMWestEurope IIS sunucularının DNS adlarını yapılandırabilirsiniz.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myIISVMEastUS** seçeneğini belirleyin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyin. Ardından **Kaydet'i**seçin.
4. **MyResourceGroupTM2** kaynak grubunda **myIISVMWestEurope** adlı VM için 1-3 adımlarını yineleyin.

### <a name="create-a-test-vm"></a>Test VM’si oluşturma

Bu bölümde, her Azure bölgesinde **(Doğu ABD** ve **Batı Avrupa)** bir*VM (myVMEastUS* ve *myVMWestEurope)* oluşturursunuz. Trafik Yöneticisi'nin trafiği daha yüksek ağırlık değerine sahip web sitesi bitiş noktasına nasıl yönlendirdiğini test etmek için bu VM'leri kullanırsınız.

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

**Ağırlıklı** yönlendirme yöntemini temel alan bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafında, **kaynak** > **Oluştur** > **Trafik Yöneticisi profili** > **oluştur'u**seçin.
2. **Traffic Manager profili oluştur** bölümünde aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılanları kabul edin ve sonra **Oluştur'u**seçin.

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Adı                   | trafficmanager.net bölgesi içinde benzersiz bir ad girin. Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adında bulunur.                                   |
    | Yönlendirme yöntemi          | **Ağırlıklı** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | **Var olanı kullan**’ı seçin ve sonra **myResourceGroupTM1** öğesini seçin. |
    |        |   |

    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-weighted-endpoint-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Kullanıcı trafiğini onlara yönlendirmek için MyIISVMEastUS ve myIISVMWestEurope iis sunucularını çalıştıran iki VM'yi ekleyin.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın. Görüntülenen sonuçlardan profili seçin.
2. **Traffic Manager profilinin****Ayarlar** bölümünde **Uç noktalar** > **Ekle**'yi seçin.
3. Aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılan değerleri kabul edin ve **Tamam**’ı seçin.

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktasını girin.                                   |
    | Adı           | **myEastUSEndpoint** yazın.                                        |
    | Hedef kaynak türü           | **Genel IP adresi**'ni seçin.                          |
    | Hedef kaynak          | Genel IP adresine sahip kaynakların aynı abonelik altında listelenmesi için genel IP adresi seçin. **Kaynak** bölümünde **myIISVMEastUS-ip** adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Ağırlık      | **100** değerini girin.        |
    |        |           |

4. Genel IP adresi **myIISVMWestEurope-ip**için **myWestEuropeEndpoint** adlı başka bir bitiş noktası eklemek için adımları 2 ve 3 tekrarlayın. Bu adres myIISVMWestEurope adlı IIS sunucusu VM ile ilişkilidir. **Ağırlık** için **25** değerini girin.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte Traffic Manager profili bölümünde gösterilir.

## <a name="test-the-traffic-manager-profile"></a>Traffic Manager profilini test etme

Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:

1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görün.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme

Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır.

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
2. **Genel Bakış**’ı seçin.
3. Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

   ![Traffic Manager DNS adı](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myVMEastUS** seçeneğini belirleyin.
2. **Genel Bakış** sayfasında **Bağlan**'ı seçin. **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen .rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için **farklı** > **bir hesap kullanma**seçeneğini seçmeniz gerekebilir.
4. **Tamam'ı**seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Web sitesini görüntülemek için myVMEastUS adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını girin. Ağırlık değeri **100** ile daha yüksek olduğundan myIISVMEastUS üzerindeki IIS sunucusunda barındırılan web sitesine yönlendirilirsiniz. IIS sunucusu myIISVMWestEurope **25**daha düşük uç nokta ağırlık değeri atanır.

   ![Traffic Manager profilini test etme](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

7. Ağırlıklı web sitesi yanıtını görmek için VM myVMWestEurope'daki 1-6 adımlarını tekrarlayın.

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme

Bu öğreticide oluşturduğunuz kaynak gruplarına ihtiyacınız kalmadığında onları silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanıcının coğrafi konumuna göre trafiği belirli uç noktalara yönlendirme](traffic-manager-configure-geographic-routing-method.md)
