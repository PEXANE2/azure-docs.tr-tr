---
title: Harici bir ASE oluşturma
description: Uygulama nın içinde bir uygulama nın olduğu bir Uygulama Hizmeti ortamını nasıl oluşturacağız veya bağımsız (boş) bir ASE nasıl oluşturacağız öğrenin.
author: ccompy
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 6c4838e3226b91cbb5d6f86b83266a986418c120
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430507"
---
# <a name="create-an-external-app-service-environment"></a>Harici Uygulama Hizmeti ortamı oluşturma

Azure App Service Ortamı, Azure App Service’in Azure sanal ağı (VNet) içindeki bir alt ağa dağıtımıdır.

> [!NOTE]
> Her Uygulama Hizmet Ortamı, Uygulama Hizmet Ortamı ile iletişim kurmak için kullanılabilecek bir Sanal IP (VIP) vardır.

Bir App Service Ortamı (ASE) iki şekilde dağıtılabilir:

- Genellikle Dış ASE olarak adlandırılan durumda, bir dış IP adresi üzerindeki VIP ile.
- İç uç noktası bir İç Yük Dengeleyicisi (ILB) olduğundan, genellikle ILB ASE olarak adlandırılan dahili BIR IP adresi üzerinde VIP ile.

Bu makalede, harici bir ASE nasıl oluşturulacak gösterilmektedir. ASE'ye genel bir bakış için, [Uygulama Hizmeti Ortamına giriş bölümüne][Intro]bakın. ILB ASE nasıl oluşturulacak hakkında bilgi [için][MakeILBASE]bkz.

## <a name="before-you-create-your-ase"></a>ASE'nizi oluşturmadan önce

ASE'nizi oluşturduktan sonra aşağıdakileri değiştiremezsiniz:

- Konum
- Abonelik
- Kaynak grubu
- Kullanılan VNet
- Kullanılan subnet
- Alt ağ boyutu

> [!NOTE]
> Bir VNet seçtiğinizde ve bir alt ağ belirttiğiniz de, gelecekteki büyüme ve ölçekleme gereksinimlerini karşılayacak kadar büyük olduğundan emin olun. Biz 256 `/24` adresleri ile bir boyut öneririz.
>

## <a name="three-ways-to-create-an-ase"></a>ASE oluşturmanın üç yolu

ASE oluşturmanın üç yolu vardır:

- **Bir Uygulama Hizmeti planı oluştururken.** Bu yöntem, ASE ve App Service planını tek adımda oluşturur.
- **Bağımsız bir eylem olarak.** Bu yöntem, içinde hiçbir şey olmayan bir ASE olan bağımsız bir ASE oluşturur. Bu yöntem, bir ASE oluşturmak için daha gelişmiş bir işlemdir. ILB ile bir ASE oluşturmak için kullanabilirsiniz.
- **Azure Kaynak Yöneticisi şablonundan.** Bu yöntem gelişmiş kullanıcılar içindir. Daha fazla bilgi için bkz. [şablondan ASE oluştur.][MakeASEfromTemplate]

Harici BIR ASE'de genel bir VIP vardır, bu da ASE'deki uygulamalara yönelik tüm HTTP/HTTPS trafiğinin internet erişimine uygun bir IP adresine isabet ettiği anlamına gelir. ILB'li bir ASE'nin ASE tarafından kullanılan alt ağdan bir IP adresi vardır. ILB ASE'de barındırılan uygulamalar doğrudan internete açık değildir.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Birlikte bir ASE ve Uygulama Hizmeti planı oluşturma

Uygulama Hizmeti planı bir uygulama kapsayıcısI. Uygulama Hizmeti'nde bir uygulama oluşturduğunuzda, bir Uygulama Hizmeti planı seçer veya oluşturursunuz. Uygulama Hizmet Ortamları Uygulama Hizmeti planlarını ve Uygulama Hizmeti planlarını tutar.

Bir Uygulama Hizmeti planı oluştururken bir ASE oluşturmak için:

1. Azure [portalında](https://portal.azure.com/)kaynak**Web + Mobil** > **Web Uygulaması** **Oluştur'u** > seçin.

    ![Web uygulaması oluşturma][1]

2. Aboneliğinizi seçin. Uygulama ve ASE aynı aboneliklerde oluşturulur.

3. Kaynak grubunu seçin veya oluşturun. Kaynak gruplarıyla, ilgili Azure kaynaklarını birim olarak yönetebilirsiniz. Uygulamalarınızın Rol Tabanlı Erişim Denetimi kurallarını oluşturduğunuzda kaynak grupları da yararlıdır. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış][ARMOverview].

4. İşletim sisteminizi (Windows, Linux veya Docker) seçin. 

5. Uygulama Hizmeti planını seçin ve ardından **Yeni Oluştur'u**seçin. Linux web uygulamaları ve Windows web uygulamaları aynı Uygulama Hizmet Planı'nda olamaz, ancak aynı Uygulama Hizmet Ortamında olabilir. 

    ![Yeni Uygulama Hizmeti planı][2]

6. **Konum** açılır listesinde, ASE'yi oluşturmak istediğiniz bölgeyi seçin. Varolan bir ASE seçerseniz, yeni bir ASE oluşturulmaz. Uygulama Hizmeti planı seçtiğiniz ASE'de oluşturulur. 

7. **Fiyatlandırma katmanını**seçin ve **Yalıtılmış** fiyatlandırma SUS'larından birini seçin. **İzole Edilmiş** Bir SKU kartı ve ASE olmayan bir konum seçerseniz, o konumda yeni bir ASE oluşturulur. ASE oluşturmak için işlemi başlatmak için **Seç'i**seçin. **İzole** SKU yalnızca bir ASE ile birlikte kullanılabilir. Ayrıca, **İzole**dışında bir ASE'de başka bir fiyatlandırma SKU kullanamazsınız. 

    ![Fiyatlandırma katmanı seçimi][3]

8. ASE'nizin adını girin. Bu ad, uygulamalarınız için adreslenebilir adda kullanılır. ASE adı _appsvcenvdemo_ise, etki alanı adı *.appsvcenvdemo.p.azurewebsites.net*. *Mytestapp*adında bir uygulama oluşturursanız, mytestapp.appsvcenvdemo.p.azurewebsites.net adresinde adreslenebilir. Adına beyaz boşluk kullanamazsınız. Büyük harf karakterleri kullanıyorsanız, etki alanı adı bu adın toplam küçük sürümüdür.

    ![Yeni Uygulama Hizmeti planı adı][4]

9. Azure sanal ağ bilgilerinizi belirtin. Yeni **Oluştur'u** seçin veya **Varolan'ı seçin.** Varolan bir VNet'i seçme seçeneği yalnızca seçili bölgede bir VNet'iniz varsa kullanılabilir. **Yeni Oluştur'u**seçerseniz, VNet için bir ad girin. Bu ada sahip yeni bir Kaynak Yöneticisi VNet oluşturulur. Seçili bölgedeki `192.168.250.0/23` adres alanını kullanır. **Varla'yı Seç'i**seçerseniz, şunları yapmanız gerekir:

    a. Birden fazla seçeneğiniz varsa VNet adres bloğunu seçin.

    b. Yeni bir alt ağ adı girin.

    c. Alt net boyutunu seçin. *ASE'nizin gelecekteki büyümesini karşılayacak kadar büyük bir boyut seçmeyi unutmayın.* 128 adresi olan ve maksimum boyutlu BIR ASE'yi işleyebilen bir cihazı öneririz. `/24` Örneğin, yalnızca `/28`16 adres kullanılabilir olduğundan, önermiyoruz. Altyapı en az yedi adres kullanır ve Azure Ağı 5 adres kullanır. Bir `/28` alt ağda, harici BIR ASE için maksimum 4 Uygulama Hizmeti planı örneği ve bir ILB ASE için yalnızca 3 Uygulama Hizmeti planı örneği ölçekleme ile kalırsınız.

    d. Alt net IP aralığını seçin.

10. ASE'yi oluşturmak için **Oluştur'u** seçin. Bu işlem aynı zamanda App Service planını ve uygulamayı oluşturur. ASE, App Service planı ve uygulamanın tümü aynı abonelik altında ve aynı kaynak grubundadır. ASE'nizin ayrı bir kaynak grubuna ihtiyacı varsa veya bir ILB ASE'ye ihtiyacınız varsa, kendi başına bir ASE oluşturmak için aşağıdaki adımları izleyin.

## <a name="create-an-ase-and-a-linux-web-app-using-a-custom-docker-image-together"></a>Birlikte özel bir Docker görüntüsü kullanarak bir ASE ve Linux web uygulaması oluşturun

1. Azure [portalında,](https://portal.azure.com/)Kapsayıcılar için Kaynak > **Web + Mobil** > **Web Uygulaması Oluşturun.** **Create a Resource** 

    ![Web uygulaması oluşturma][7]

1. Aboneliğinizi seçin. Uygulama ve ASE aynı aboneliklerde oluşturulur.

1. Kaynak grubunu seçin veya oluşturun. Kaynak gruplarıyla, ilgili Azure kaynaklarını birim olarak yönetebilirsiniz. Uygulamalarınızın Rol Tabanlı Erişim Denetimi kurallarını oluşturduğunuzda kaynak grupları da yararlıdır. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış][ARMOverview].

1. Uygulama Hizmeti planını seçin ve ardından **Yeni Oluştur'u**seçin. Linux web uygulamaları ve Windows web uygulamaları aynı Uygulama Hizmet Planı'nda olamaz, ancak aynı Uygulama Hizmet Ortamında olabilir. 

    ![Yeni Uygulama Hizmeti planı][8]

1. **Konum** açılır listesinde, ASE'yi oluşturmak istediğiniz bölgeyi seçin. Varolan bir ASE seçerseniz, yeni bir ASE oluşturulmaz. Uygulama Hizmeti planı seçtiğiniz ASE'de oluşturulur. 

1. **Fiyatlandırma katmanını**seçin ve **Yalıtılmış** fiyatlandırma SUS'larından birini seçin. **İzole Edilmiş** Bir SKU kartı ve ASE olmayan bir konum seçerseniz, o konumda yeni bir ASE oluşturulur. ASE oluşturmak için işlemi başlatmak için **Seç'i**seçin. **İzole** SKU yalnızca bir ASE ile birlikte kullanılabilir. Ayrıca, **İzole**dışında bir ASE'de başka bir fiyatlandırma SKU kullanamazsınız. 

    ![Fiyatlandırma katmanı seçimi][3]

1. ASE'nizin adını girin. Bu ad, uygulamalarınız için adreslenebilir adda kullanılır. ASE adı _appsvcenvdemo_ise, etki alanı adı *.appsvcenvdemo.p.azurewebsites.net*. *Mytestapp*adında bir uygulama oluşturursanız, mytestapp.appsvcenvdemo.p.azurewebsites.net adresinde adreslenebilir. Adına beyaz boşluk kullanamazsınız. Büyük harf karakterleri kullanıyorsanız, etki alanı adı bu adın toplam küçük sürümüdür.

    ![Yeni Uygulama Hizmeti planı adı][4]

1. Azure sanal ağ bilgilerinizi belirtin. Yeni **Oluştur'u** seçin veya **Varolan'ı seçin.** Varolan bir VNet'i seçme seçeneği yalnızca seçili bölgede bir VNet'iniz varsa kullanılabilir. **Yeni Oluştur'u**seçerseniz, VNet için bir ad girin. Bu ada sahip yeni bir Kaynak Yöneticisi VNet oluşturulur. Seçili bölgedeki `192.168.250.0/23` adres alanını kullanır. **Varla'yı Seç'i**seçerseniz, şunları yapmanız gerekir:

    a. Birden fazla seçeneğiniz varsa VNet adres bloğunu seçin.

    b. Yeni bir alt ağ adı girin.

    c. Alt net boyutunu seçin. *ASE'nizin gelecekteki büyümesini karşılayacak kadar büyük bir boyut seçmeyi unutmayın.* 128 adresi olan ve maksimum boyutlu BIR ASE'yi işleyebilen bir cihazı öneririz. `/24` Örneğin, yalnızca `/28`16 adres kullanılabilir olduğundan, önermiyoruz. Altyapı en az yedi adres kullanır ve Azure Ağı 5 adres kullanır. Bir `/28` alt ağda, harici BIR ASE için maksimum 4 Uygulama Hizmeti planı örneği ve bir ILB ASE için yalnızca 3 Uygulama Hizmeti planı örneği ölçekleme ile kalırsınız.

    d. Alt net IP aralığını seçin.

1.  "Yapılandırılan Kapsayıcı"yı seçin.
    * Özel resim adınızı girin (Azure Kapsayıcı Kayıt Defteri'ni, Docker Hub'ı ve kendi özel kayıt defterinizi kullanabilirsiniz). Kendi özel kapsayıcınızı kullanmak istemiyorsanız, yukarıdaki talimatları kullanarak kodunuzu getirebilir ve Linux'ta App Service ile yerleşik bir resim kullanabilirsiniz. 

    ![Konteyneri Yapılandır][9]

1. ASE'yi oluşturmak için **Oluştur'u** seçin. Bu işlem aynı zamanda App Service planını ve uygulamayı oluşturur. ASE, App Service planı ve uygulamanın tümü aynı abonelik altında ve aynı kaynak grubundadır. ASE'nizin ayrı bir kaynak grubuna ihtiyacı varsa veya bir ILB ASE'ye ihtiyacınız varsa, kendi başına bir ASE oluşturmak için aşağıdaki adımları izleyin.


## <a name="create-an-ase-by-itself"></a>Kendi başına bir ASE oluşturun

Eğer tek başına bir ASE bağımsız oluşturursanız, içinde hiçbir şey vardır. Boş bir ASE hala altyapı için aylık ücret tabi. ILB ile bir ASE oluşturmak veya kendi kaynak grubunda bir ASE oluşturmak için aşağıdaki adımları izleyin. ASE'nizi oluşturduktan sonra, normal işlemi kullanarak içinde uygulamalar oluşturabilirsiniz. Konum olarak yeni ASE'nizi seçin.

1. **Uygulama Hizmeti Ortamı**için Azure Marketi'nde arama yapın veya kaynak > Web**Mobil** > **Uygulama Hizmeti Ortamı** **Oluştur'u**seçin. 

1. ASE'nizin adını girin. Bu ad, ASE'de oluşturulan uygulamalar için kullanılır. Adı *mynewdemoase*ise, alt etki alanı adı *.mynewdemoase.p.azurewebsites.net.* *Mytestapp*adında bir uygulama oluşturursanız, mytestapp.mynewdemoase.p.azurewebsites.net adresinde adreslenebilir. Adına beyaz boşluk kullanamazsınız. Büyük harf karakterleri kullanıyorsanız, etki alanı adı adın toplam küçük sürümüdür. Bir ILB kullanıyorsanız, ASE adınız alt etki alanınızda kullanılmaz, bunun yerine ASE oluşturma sırasında açıkça belirtilir.

    ![ASE adlandırma][5]

1. Aboneliğinizi seçin. Bu abonelik aynı zamanda ASE'deki tüm uygulamaların kullandığı aboneliktir. ASE'nizi başka bir abonelikteki bir VNet'e koyamazsınız.

1. Yeni bir kaynak grubu seçin veya belirtin. ASE'niz için kullanılan kaynak grubu, VNet'iniz için kullanılan kaynak grubuyla aynı olmalıdır. Varolan bir VNet seçerseniz, ASE'nizin kaynak grubu seçimi VNet'inizin seçimini yansıtacak şekilde güncelleştirilir. *Kaynak Yöneticisi şablonu kullanıyorsanız, VNet kaynak grubundan farklı bir kaynak grubu yla bir ASE oluşturabilirsiniz.* Şablondan ASE oluşturmak için [bkz.][MakeASEfromTemplate]

    ![Kaynak grubu seçimi][6]

1. VNet'inizi ve konumunuzu seçin. Yeni bir VNet oluşturabilir veya varolan bir VNet seçebilirsiniz: 

    * Yeni bir VNet seçerseniz, bir ad ve konum belirtebilirsiniz. 
    
    * Yeni VNet adres aralığı 192.168.250.0/23 ve varsayılan adlı bir alt ağ vardır. Alt ağ 192.168.250.0/24 olarak tanımlanır. Yalnızca bir Kaynak Yöneticisi VNet seçebilirsiniz. **VIP Türü** seçimi, ASE'nize internetten (Harici) doğrudan erişilip erişilemeyebileceğini veya ILB kullanıp kullanmayabileceğini belirler. Bu seçenekler hakkında daha fazla bilgi edinmek için [bkz.][MakeILBASE] 

      * **VIP Türü**için **Dış'ı** seçerseniz, ip tabanlı SSL amaçları yla sistemin kaç harici IP adresi oluşturulduğunu seçebilirsiniz. 
    
      * **VIP Türü**için **İçerseçeneğini** seçerseniz, ASE'nizin kullandığı etki alanını belirtmeniz gerekir. Ortak veya özel adres aralıklarını kullanan bir VNet'e ASE dağıtabilirsiniz. Genel adres aralığına sahip bir VNet kullanmak için, VNet'i önceden oluşturmanız gerekir. 
    
    * Varolan bir VNet seçerseniz, ASE oluşturulduğunda yeni bir alt ağ oluşturulur. *Portalda önceden oluşturulmuş bir alt ağ kullanamazsınız. Kaynak Yöneticisi şablonu kullanıyorsanız, varolan bir alt ağla bir ASE oluşturabilirsiniz.* Şablondan ASE oluşturmak için [bkz.][MakeASEfromTemplate]

## <a name="app-service-environment-v1"></a>App Service Ortamı v1

Yine de App Service Environment'ın (ASEv1) ilk sürümünün örneklerini oluşturabilirsiniz. Bu işlemi başlatmak için, **Uygulama Hizmet Ortamı v1**için Market'te arama yapın. ASE'yi, bağımsız ASE'yi oluşturduğunuz şekilde oluşturursunuz. Bittiğinde, ASEv1 iki ön uçları ve iki işçi vardır. ASEv1 ile ön uçları ve işçileri yönetmelisiniz. Uygulama Hizmeti planlarınızı oluştururken otomatik olarak eklenmez. Ön uçlar HTTP/HTTPS bitiş noktaları olarak hareket ve işçilere trafik göndermek. Çalışanlar, uygulamalarınızı barındıran rollerdir. ASE'nizi oluşturduktan sonra ön uçların ve işçilerin miktarını ayarlayabilirsiniz. 

ASEv1 hakkında daha fazla bilgi edinmek [için, Uygulama Hizmet Ortamına Giriş v1'e][ASEv1Intro]bakın. ASEv1'i ölçekleme, yönetme ve izleme hakkında daha fazla bilgi için [Uygulama Hizmet Ortamını nasıl yapılandırılabildiğinize][ConfigureASEv1]bakın.

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png
[7]: ./media/how_to_create_an_external_app_service_environment/createexternalase-createwafc.png
[8]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreatewafc.png
[9]: ./media/how_to_create_an_external_app_service_environment/createexternalase-configurecontainer.png



<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
