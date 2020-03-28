---
title: Quickstart - Chef'i kullanarak Azure'da Windows sanal makinesini yapılandırma
description: Bu hızlı başlangıçta, Azure'da bir Windows sanal makinesini dağıtmak ve yapılandırmak için Chef'i nasıl kullanacağınızı öğrenirsiniz
keywords: şef, masmavi, devops, sanal makine
ms.topic: tutorial
ms.service: chef
author: tomarchermsft
ms.author: tarcher
ms.date: 02/22/2020
ms.openlocfilehash: 824e4df7662ee67c3f0786877053c39a8d952d49
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77590072"
---
# <a name="quickstart---configure-a-windows-virtual-machine-in-azure-using-chef"></a>Quickstart - Chef'i kullanarak Azure'da Windows sanal makinesini yapılandırma

Şef, otomasyon ve istenilen durum yapılandırmaları sunmanızı sağlar.

En son bulut API sürümüyle Chef, Azure ile sorunsuz entegrasyon sağlayarak yapılandırma durumlarını tek bir komutla sağlama ve dağıtma olanağı sağlar.

Bu makalede, Şef ortamınızı Azure sanal makinelerini sağlamak ve bir ilke veya yemek kitabı oluşturmak ve ardından bu yemek kitabını bir Azure sanal makinesine dağıtmak için ayarlıyorsun.

## <a name="chef-basics"></a>Şef temelleri

Bu makaleye başlamadan [önce, Şef'in temel kavramlarını gözden geçirin.](https://www.chef.io/chef)

Aşağıdaki diyagram, üst düzey Chef mimarisini gösterir.

![Şef mimarisi](media/chef-automation/chef-architecure.png)

Chef üç ana mimari bileşene sahiptir: 
- Chef Server - Yönetim noktası ve Chef Server için iki seçenek vardır: barındırılan bir çözüm veya şirket içi bir çözüm.
- Chef Client (düğüm) - Yönettiğiniz sunucularda yer alan aracı.
- Chef Workstation - Hem yönetici iş istasyonunun (politikalar oluşturduğunuz ve yönetim komutlarını çalıştırdığınız) hem de Chef araçlarının yazılım paketinin adı.

Genellikle, iş **istasyonunuzu** komutları çalıştırdığınız konum ve yazılım paketi için **Chef Workstation** olarak görürsünüz.

Örneğin, **Chef Workstation'ın**bir parçası olarak bıçak komutunu indirirsiniz, ancak altyapıyı yönetmek için **iş istasyonunuzdan** bıçak komutları çalıştırın.

Şef de yemek *kitapları* ve *yemek tarifleri*kavramları kullanır. Bu terimler, sırasıyla sunuculara tanımlanan ve uygulanan ilkelerdir.

## <a name="preparing-your-workstation"></a>İş istasyonunuzu hazırlama

İlk olarak, Chef yapılandırma dosyalarını ve yemek kitaplarını depolamak için bir dizin oluşturarak iş istasyonunuzu hazırlayın.

`C:\Chef` adlı bir dizin oluşturun.

En son [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sürümünü iş istasyonunuzun üzerine indirin ve yükleyin.

## <a name="configure-azure-service-principal"></a>Azure hizmet sorumlusunu yapılandırma

Şef İş İstasyonumuzdan Azure kaynakları oluşturmamıza yardımcı olması için bir Hizmet Müdürü kullanacağız.  Gerekli izinleri içeren ilgili Servis Sorumlusu'nu oluşturmak için PowerShell içinde aşağıdaki komutları çalıştırın:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Bu değerlere ihtiyacınız olacağı için SubscriptionID, TenantID, ClientID ve Client Secret (bu eğitimde daha önce belirlediğiniz parola) not alın. 

## <a name="setup-chef-server"></a>Kurulum Chef Server

Bu kılavuz, Hosted Chef'e kaydolduğunuzu varsayar.

Zaten bir Chef Server kullanmıyorsanız, şunları yapabilirsiniz:

* Chef ile başlamak için en hızlı yoldur [Hosted Chef](https://manage.chef.io/signup)için kaydolun.
* [Chef Docs'un](https://docs.chef.io/) [kurulum yönergelerini](https://docs.chef.io/install_server.html) izleyerek linux tabanlı makineye bağımsız bir Chef Server yükleyin.

### <a name="creating-a-hosted-chef-account"></a>Barındırılan Şef hesabı oluşturma

[Burada](https://manage.chef.io/signup)bir Hosted Chef hesabı için kaydolun.

Kayıt işlemi sırasında, yeni bir kuruluş oluşturmanız istenir.

![Kuruluş penceresi oluşturma](media/chef-automation/create-organization.png)

Kuruluşunuz oluşturulduktan sonra başlangıç kitini indirin.

![Şef'i Yapılandırma](media/chef-automation/configure-chef.png)

> [!NOTE]
> Anahtarlarınızın sıfırlanacağına dair bir uyarı alırsanız, henüz yapılandırılmış bir altyapımız olmadığı için devam etmenizde bir sakınca yoktur.
>

Bu başlangıç kiti zip dosyası, `.chef` dizinde kuruluş yapılandırma dosyalarınızı ve kullanıcı anahtarınızı içerir.

Özel `organization-validator.pem` bir anahtar olduğundan ve özel anahtarlar Chef Server'da depolanmamalıdır. [Chef Manage'den](https://manage.chef.io/)İdare bölümüne gidin ve ayrı olarak indirmeniz için bir dosya sağlayan "Doğrulama Anahtarını Sıfırla"yı seçin. Dosyayı c:\chef'e kaydedin.

### <a name="configuring-your-chef-workstation"></a>Şef iş istasyonunuzu yapılandırma

`chef-starter.zip` To `c:\chef`içeriğini ayıklayın.

Tüm dosyaları `chef-starter\chef-repo\.chef` dizininizin `c:\chef` altında kopyalayın.

Dosyayı `organization-validator.pem` , `c:\chef`kaydedilirse `c:\Downloads`' ye kopyalayın.

Dizininiz şimdi aşağıdaki örneğe benzer.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

Şimdi c:\chef kökünde beş dosya ve dört dizin (boş şef-repo dizini dahil) olmalıdır.

### <a name="edit-kniferb"></a>Knife.rb'yi edit

PEM dosyaları kuruluşunuzu ve iletişim için yönetim özel anahtarlarını ve knife.rb dosyanızı bıçak yapılandırmanızı içerir. Knife.rb dosyasını hazırlamamız gerekecek.

Seçtiğiniz editörde knife.rb dosyasını açın. Değiştirilmemiş dosya aşağıdaki gibi görünmelidir:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Yer tutucularını bilgilerinizle değiştirerek knife.rb'nize aşağıdaki bilgileri ekleyin:

```rb
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg.pem"
knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] =     "#1234p$wdchef19"
```

Bu satırlar Bıçak altında yemek kitapları `c:\chef\cookbooks`dizini referanslar sağlayacaktır.

Dosyanız `knife.rb` şimdi aşağıdaki örneğe benzer olmalıdır:

![Bıçak dosyası örneği](./media/chef-automation/knife-file-example.png)

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Şef İş İstasyonu'nu Yükleyin

Ardından, [Chef Workstation'ı indirin ve kurun.](https://downloads.chef.io/chef-workstation/)

Chef Workstation'ı varsayılan konuma yükleyin.

Masaüstünde bir CW PowerShell görürsünüz. Bu araç, Chef ürünleriyle etkileşim kurmak için kullanılır. CW PowerShell, Chef CLI komutları `chef-run` gibi (örneğin) `chef`yeni komutları kullanılabilir hale getirir. Chef Workstation'ın yüklü sürümünü ve Chef `chef -v`araçlarını . Chef Workstation App'ten Chef **Workstation Hakkında'yı** seçerek Workstation sürümünüzü de kontrol edebilirsiniz.

`chef --version`gibi bir şey dönmelidir:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> Yolun düzeni önemlidir! Özet kodunuz doğru sırada değilse, sorunlar ortaya çıkacaktır.
>

Devam etmeden önce iş istasyonunuzu yeniden başlatın.

### <a name="install-knife-azure"></a>Bıçak Azure'u Yükle

Bu öğretici, sanal makinenizle etkileşimde kalmak için Azure Kaynak Yöneticisi'ni kullandığınızı varsayar.

Azure Eklentisini içeren Knife Azure uzantısını yükleyin.

Şu komutu çalıştırın.

    chef gem install knife-azure ––pre

> [!NOTE]
> Bağımsız `–-pre` değişken, En son API kümesine erişim sağlayan Knife Azure Eklentisi'nin en son RC sürümünü almanızı sağlar.
>
>

Bir dizi bağımlılık da aynı anda yüklenmiş olabilir.

![Bıçak-azure yükleme çıktısı](./media/chef-automation/install-knife-azure.png)

Her şeyin doğru şekilde yapılandırıldığından emin olmak için aşağıdaki komutu çalıştırın.

    knife azurerm server list

Her şey doğru şekilde yapılandırılırsa, kullanılabilir Azure görüntülerinin bir listesini görürsünüz.

Tebrikler. İş istasyonunuzun kurulumu!

## <a name="creating-a-cookbook"></a>Yemek kitabı oluşturma

Yemek kitabı, Yönetilen istemcinizde çalıştırmak istediğiniz komutlar kümesini tanımlamak için Chef tarafından kullanılır. Bir yemek kitabı oluşturmak basittir, `chef generate cookbook` sadece yemek kitabı şablonu oluşturmak için komutu kullanın. Bu yemek kitabı, IIS'yi otomatik olarak dağıtan bir web sunucusu içindir.

Sizin `C:\Chef directory`altında, aşağıdaki komutu çalıştırın.

    chef generate cookbook webserver

Bu komut C:\Chef\cookbooks\webserver dizininin altında bir dosya kümesi oluşturur. Ardından, Chef istemcisinin yönetilen sanal makinede çalışması için komut kümesini tanımlayın.

Komutlar default.rb dosyasında depolanır. Bu dosyada, IIS'yi yükleyen, IIS'yi başlatan ve şablon dosyasını `wwwroot` klasöre kopyalayan bir komut kümesi tanımlayın.

C:\chef\cookbooks\webserver\recipes\default.rb dosyasını değiştirin ve aşağıdaki satırları ekleyin.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Bittikten sonra dosyayı kaydedin.

## <a name="creating-a-template"></a>Şablon oluşturma

Bu adımda, `default.html` sayfa olarak kullanmak üzere bir şablon dosyası oluşturursunuz.

Şablonu oluşturmak için aşağıdaki komutu çalıştırın:

    chef generate template webserver Default.htm

Dosyaya `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb` gidin. Basit bir *Hello World* HTML kodu ekleyerek dosyayı düzenleme ve ardından dosyayı kaydedin.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Yemek kitabını Chef Server'a yükleyin

Bu adımda, yerel makinede oluşturduğunuz yemek kitabının bir kopyasını hazırlayıp Chef Hosted Server'a yüklersiniz. Yüklendikten sonra, yemek kitabı **İlke** sekmesi altında görünür.

    knife cookbook upload webserver

![Chef Server'a yemek kitabı yüklemenin sonuçları](./media/chef-automation/cookbook-installation-under-policy-tab.png)

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Knife Azure ile sanal makine dağıtma

Bir Azure sanal makinesini `Webserver` dağıtın `knife` ve komutu kullanarak yemek kitabını uygulayın.

Komut `knife` ayrıca IIS web hizmetini ve varsayılan web sayfasını da yükler.

```bash
    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"
```

Komut `knife` örneği, Batı ABD bölgesinde Windows Server 2016 yüklü bir *Standard_DS2_v2* sanal makine oluşturur. Bu değerleri uygulama gereksinimlerinize göre değiştirin.

Komutu çalıştırdıktan sonra, makinenizin hizmet vermeye başladığını görmek için Azure portalına göz atın.

![Sanal makine sağlanıyor](./media/chef-automation/virtual-machine-being-provisioned.png)

Komut istemi sonraki görünür.

![Sanal makine oluştururken bıçak çıkışı](./media/chef-automation/knife-output-when-creating-vm.png)

Dağıtım tamamlandıktan sonra, yeni sanal makinenin genel IP adresi görüntülenir. Yeni web sitesini görüntülemek için bu değeri bir web tarayıcısına yapıştırın. Sanal makineyi dağıttığımızda, port 80'i dışarıdan kullanılabilir hale gelecek şekilde açtık.   

![Sanal makineyi test etme](./media/chef-automation/testing-the-virtual-machine.png)

Bu örnekte yaratıcı HTML kodu kullanır.

Düğümün durumunu da görebilirsiniz [Chef Manage.](https://manage.chef.io/) 

![Düğüm durumunu görüntüleme](./media/chef-automation/viewing-node-status.png)

Azure portalından 3389 portu üzerinden bir RDP oturumu aracılığıyla da bağlanabileceğinizi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Chef](/azure/chef/)