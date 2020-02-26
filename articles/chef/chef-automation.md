---
title: Hızlı başlangıç-Chef kullanarak Azure 'da bir Windows sanal makinesi yapılandırma
description: Bu hızlı başlangıçta, Azure 'da bir Windows sanal makinesi dağıtmak ve yapılandırmak için Chef 'i nasıl kullanacağınızı öğrenirsiniz.
keywords: Chef, Azure, DevOps, sanal makine
ms.topic: tutorial
ms.service: chef
author: tomarchermsft
ms.author: tarcher
ms.date: 02/22/2020
ms.openlocfilehash: 824e4df7662ee67c3f0786877053c39a8d952d49
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590072"
---
# <a name="quickstart---configure-a-windows-virtual-machine-in-azure-using-chef"></a>Hızlı başlangıç-Chef kullanarak Azure 'da bir Windows sanal makinesi yapılandırma

Chef, otomasyon ve istenen durum konfigürasyonları sunmanızı sağlar.

En son bulut API sürümü sayesinde Chef, Azure ile sorunsuz tümleştirme sağlarken, tek bir komut aracılığıyla yapılandırma durumlarını sağlama ve dağıtma olanağı sunar.

Bu makalede, Azure sanal makinelerini sağlamak ve bir ilke veya tanıtım rehberi oluşturup bu tanıtım rehberini bir Azure sanal makinesine dağıtmak için Chef ortamınızı ayarlarsınız.

## <a name="chef-basics"></a>Chef temelleri

Bu makaleye başlamadan önce, [Chef 'in temel kavramlarını gözden geçirin](https://www.chef.io/chef).

Aşağıdaki diyagramda, üst düzey Chef mimarisi gösterilmektedir.

![Chef mimarisi](media/chef-automation/chef-architecure.png)

Chef 'in üç ana mimari bileşeni vardır: 
- Chef sunucusu-yönetim noktası ve Chef sunucusu için iki seçenek vardır: barındırılan bir çözüm veya şirket içi bir çözüm.
- Chef Istemcisi (node)-yönettiğiniz sunucularda bulunan aracı.
- Chef Workstation-hem yönetici iş istasyonunun adı (ilke oluşturduğunuz ve yönetim komutları çalıştırmaklardır) ve Chef araçlarının yazılım paketi.

Genellikle, **iş istasyonunuzu** yazılım paketi için komutları ve **Chef iş istasyonunu** çalıştırdığınız konum olarak görürsünüz.

Örneğin, bıis komutunu **Chef Iş istasyonunun**parçası olarak indirirler, ancak altyapıyı yönetmek için **iş istasyonunuzdan** bıı komutlarını çalıştırırsınız.

Chef Ayrıca, *tanımlama kitapları* ve *yemek tarifleri*kavramlarını kullanır. Bu terimler sırasıyla sunuculara tanımlanan ve uygulanan ilkelerdir.

## <a name="preparing-your-workstation"></a>İş istasyonunuzu hazırlama

İlk olarak, Chef yapılandırma dosyalarını ve tanıtım defterlerini depolamak için bir dizin oluşturarak iş istasyonunuzu hazırlayın.

`C:\Chef` adlı bir dizin oluşturun.

En son [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sürümünü iş istasyonunuza indirin ve yükleyin.

## <a name="configure-azure-service-principal"></a>Azure hizmet sorumlusunu yapılandırma

Chef Iş Istasyonumuzdan Azure kaynakları oluşturmamıza yardımcı olmak için bir hizmet sorumlusu kullanacağız.  Gerekli izinlerle ilgili hizmet sorumlusunu oluşturmak için PowerShell 'de aşağıdaki komutları çalıştırın:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Bu değerlere ihtiyacınız olacak şekilde, SubscriptionID, Tenantıd, ClientID ve Istemci gizli dizisini (Bu öğreticide daha önce ayarladığınız parola) göz önünde yararlanın. 

## <a name="setup-chef-server"></a>Chef sunucusu kurma

Bu kılavuzda, barındırılan Chef 'e kaydolduğunuzu kabul edersiniz.

Zaten bir Chef sunucusu kullanmıyorsanız şunları yapabilirsiniz:

* Chef ile çalışmaya başlamanın en hızlı yolu olan [barındırılan Chef](https://manage.chef.io/signup)'e kaydolun.
* [Chef belgelerinden](https://docs.chef.io/) [yükleme yönergelerini](https://docs.chef.io/install_server.html) izleyerek Linux tabanlı makineye tek başına Chef sunucusu yükleme.

### <a name="creating-a-hosted-chef-account"></a>Barındırılan Chef hesabı oluşturma

[Burada](https://manage.chef.io/signup)barındırılan bir Chef hesabına kaydolun.

Kaydolma işlemi sırasında yeni bir kuruluş oluşturmanız istenir.

![Kuruluş penceresi oluştur](media/chef-automation/create-organization.png)

Kuruluşunuz oluşturulduktan sonra, başlangıç paketini indirin.

![Chef yapılandırma](media/chef-automation/configure-chef.png)

> [!NOTE]
> Anahtarlarınızın sıfırlandığını bildiren bir uyarı alırsanız, henüz var olan bir altyapı yapılandırıldığımızda devam etmeniz normaldir.
>

Bu başlatıcı seti ZIP dosyası `.chef` dizinindeki kuruluş yapılandırma dosyalarınızı ve Kullanıcı anahtarınızı içerir.

`organization-validator.pem`, özel bir anahtar olduğundan ve özel anahtarların Chef sunucusunda depolanmadığından bağımsız olarak indirilmelidir. [Chef Yönet](https://manage.chef.io/)'ten Yönetim bölümüne gidin ve ayrı olarak indirmeniz için bir dosya sağlayan "doğrulama anahtarını Sıfırla" yı seçin. Dosyayı c:\chefdizinine kaydedin.

### <a name="configuring-your-chef-workstation"></a>Chef iş istasyonunuzu yapılandırma

`chef-starter.zip` içeriğini `c:\chef`ayıklayın.

`chef-starter\chef-repo\.chef` altındaki tüm dosyaları `c:\chef` dizininize kopyalayın.

`organization-validator.pem` dosyasını, `c:\Downloads`kaydedilmişse `c:\chef`kopyalayın.

Dizininiz artık aşağıdaki örneğe benzer şekilde görünmelidir.

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

Artık c:\chefroot dizininde beş dosya ve dört dizin (boş Chef-repo dizini dahil) olmalıdır.

### <a name="edit-kniferb"></a>Bıçak. RB 'yi Düzenle

PEK dosyaları, iletişim için kuruluşunuzu ve yönetim özel anahtarlarını içerir ve bıçak. rb dosyası bıçak yapılandırmanızı içerir. Bıçak. RB dosyasını düzenlememiz gerekir.

Bıçak. RB dosyasını istediğiniz düzenleyicide açın. Değiştirilmemiş dosya şuna benzemelidir:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Aşağıdaki bilgileri bıçak. RB 'nize ekleyerek yer tutucuları bilgilerinizi ile değiştirin:

```rb
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg.pem"
knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] =     "#1234p$wdchef19"
```

Bu satırlar, bıçak 'nın `c:\chef\cookbooks`altındaki tanımlama kitapları dizinine başvurduğundan emin olur.

`knife.rb` dosyanız aşağıdaki örneğe benzer şekilde görünmelidir:

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

## <a name="install-chef-workstation"></a>Chef Iş Istasyonunu yükler

Sonra, [Chef Iş istasyonunu indirin ve yükleyin](https://downloads.chef.io/chef-workstation/).

Chef Iş Istasyonunu varsayılan konuma yükler.

Masaüstünde bir FA PowerShell görürsünüz. Bu araç Chef ürünleriyle etkileşim kurmak için kullanılır. FA PowerShell, `chef-run` ve Chef CLı komutları gibi yeni komutların kullanılabilmesini sağlar (örneğin `chef`). Yüklü Chef Iş Istasyonu sürümünüzü ve `chef -v`ile Chef araçlarını inceleyin. Chef iş istasyonu uygulamasında **Chef Workstation hakkında** ' yı seçerek iş istasyonu sürümünüzü de denetleyebilirsiniz.

`chef --version` şöyle bir şey döndürmelidir:

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
> Yolun sırası önemlidir! Opscode yollarınız doğru sırada değilse, sorunlar ortaya çıkabilir.
>

Devam etmeden önce iş istasyonunuzu yeniden başlatın.

### <a name="install-knife-azure"></a>Bıçak Azure 'ı yükler

Bu öğreticide, sanal makineli etkileşimde bulunmak için Azure Resource Manager kullandığınızı varsayılmaktadır.

Azure eklentisini içeren bıçak Azure uzantısını yükler.

Aşağıdaki komutu çalıştırın.

    chef gem install knife-azure ––pre

> [!NOTE]
> `–-pre` bağımsız değişkeni, en son API kümesine erişim sağlayan bıçak Azure eklentisinin en son RC sürümünü almanızı sağlar.
>
>

Aynı zamanda aynı anda bir dizi bağımlılığı da yüklenecektir.

![Bıı yükleme-Azure](./media/chef-automation/install-knife-azure.png)

Her şeyin doğru şekilde yapılandırıldığından emin olmak için aşağıdaki komutu çalıştırın.

    knife azurerm server list

Her şey doğru yapılandırılmışsa, kullanılabilecek Azure görüntülerinin bir listesini görürsünüz.

Tebrikler. İş istasyonunuz ayarlandı!

## <a name="creating-a-cookbook"></a>Bir tanımlama kitabı oluşturma

Bir tanımlama kitabı, yönetilen istemciniz üzerinde çalıştırmak istediğiniz bir komut kümesini tanımlamak için Chef tarafından kullanılır. Bir tanımlama kitabı oluşturmak basittir, tanımlama kitabı şablonunu oluşturmak için yalnızca `chef generate cookbook` komutunu kullanın. Bu kılavuz kitabı, IIS 'yi otomatik olarak dağıtan bir Web sunucusu içindir.

`C:\Chef directory`altında aşağıdaki komutu çalıştırın.

    chef generate cookbook webserver

Bu komut, C:\chef\pişirbooks\webserverdizininde bir dosya kümesi oluşturur. Ardından, Chef istemcisinin yönetilen sanal makinede çalışacağı komut kümesini tanımlayın.

Komutlar default. RB dosyasında depolanır. Bu dosyada, IIS 'yi yükleyen bir komutlar kümesi tanımlayın, IIS 'yi başlatır ve bir şablon dosyasını `wwwroot` klasörüne kopyalar.

C:\chef\cookbooks\webserver\recipes\default.RB dosyasını değiştirin ve aşağıdaki satırları ekleyin.

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

İşiniz bittiğinde dosyayı kaydedin.

## <a name="creating-a-template"></a>Şablon oluşturma

Bu adımda, `default.html` sayfası olarak kullanılacak bir şablon dosyası oluşturacaksınız.

Şablonu oluşturmak için aşağıdaki komutu çalıştırın:

    chef generate template webserver Default.htm

`C:\chef\cookbooks\webserver\templates\default\Default.htm.erb` dosyasına gidin. Basit *Merhaba Dünya* HTML kodu ekleyerek dosyayı düzenleyin ve ardından dosyayı kaydedin.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Tanıtım rehberini Chef sunucusuna yükleme

Bu adımda, yerel makinede oluşturduğunuz tanıtım rehberinin bir kopyasını oluşturursunuz ve bu dosyayı Chef barındırılan sunucusuna yüklersiniz. Karşıya yüklendikten sonra, tanımlama kitabı **ilke** sekmesinin altında görüntülenir.

    knife cookbook upload webserver

![Chef sunucusuna tanımlama kitabı yükleme sonuçları](./media/chef-automation/cookbook-installation-under-policy-tab.png)

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bıçak Azure ile sanal makine dağıtma

Bir Azure sanal makinesini dağıtın ve `knife` komutunu kullanarak `Webserver` tanımlama kitabını uygulayın.

`knife` komutu ayrıca IIS Web hizmetini ve varsayılan Web sayfasını yükler.

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

`knife` komutu örneği, Batı ABD bölgesi içinde Windows Server 2016 yüklü bir *Standard_DS2_v2* sanal makine oluşturur. Bu değerleri uygulama gereksinimlerinize göre değiştirin.

Komutu çalıştırdıktan sonra makinenizin sağlamaya başlamasını görmek için Azure portal gidin.

![Sağlanan sanal makine](./media/chef-automation/virtual-machine-being-provisioned.png)

Sonraki komut istemi görüntülenir.

![Sanal makine oluşturulurken bıçak çıkışı](./media/chef-automation/knife-output-when-creating-vm.png)

Dağıtım tamamlandıktan sonra, yeni sanal makinenin genel IP adresi görüntülenir. Yeni Web sitesini görüntülemek için bu değeri bir Web tarayıcısına yapıştırın. Sanal makineyi dağıttığımız için bağlantı noktası 80 ' i açtık, bu nedenle dışarıdan kullanılabilir olmalıdır.   

![Sanal makineyi test etme](./media/chef-automation/testing-the-virtual-machine.png)

Bu örnek, Creative HTML kodunu kullanır.

Ayrıca, düğümün durum [Chef Manage](https://manage.chef.io/)' i görüntüleyebilirsiniz. 

![Düğüm durumunu görüntüleme](./media/chef-automation/viewing-node-status.png)

3389 numaralı bağlantı noktası üzerinden Azure portal bir RDP oturumu aracılığıyla da bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Chef](/azure/chef/)