---
title: Azure DevTest Labs'da Uzak Masaüstü Ağ Geçidi'ni kullanacak şekilde bir laboratuvar yapılandırın
description: RDP bağlantı noktasını ortaya çıkarmak zorunda kalmadan laboratuvar VM'lerine güvenli erişim sağlamak için Azure DevTest Labs'daki bir laboratuarı uzak bir masaüstü ağ geçidiyle nasıl yapılandıracaklarını öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 2cdafa9a36a5f906151ca6946e18ef82bc7f1e01
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529416"
---
# <a name="configure-your-lab-in-azure-devtest-labs-to-use-a-remote-desktop-gateway"></a>Azure DevTest Labs'daki laboratuvarınızı uzak bir masaüstü ağ geçidi kullanacak şekilde yapılandırın
Azure DevTest Labs'da, RDP bağlantı noktasını ortaya çıkarmak zorunda kalmadan laboratuvar sanal makinelerine (VM) güvenli erişim sağlamak için laboratuarınız için uzak bir masaüstü ağ geçidi yapılandırabilirsiniz. Laboratuvar, laboratuvar kullanıcılarınızın erişebildikleri tüm sanal makineleri görüntülemeleri ve bunlara bağlanmaları için merkezi bir yer sağlar. **Sanal Makine** sayfasındaki **Bağlan** düğmesi, makineye bağlanmak için açabileceğiniz makineye özgü bir RDP dosyası oluşturur. Laboratuvarınızı uzak bir masaüstü ağ geçidine bağlayarak RDP bağlantısını daha da özelleştirebilir ve güvene alabilirsiniz. 

Laboratuvar kullanıcısı doğrudan ağ geçidi makinesine doğruluğunu yaptığı ndan veya makinelerine bağlanmak için etki alanı yla birleştirilmiş ağ geçidi makinesinde şirket kimlik bilgilerini kullanabileceğinden, bu yaklaşım daha güvenlidir. Laboratuvar ayrıca kullanıcıların RDP bağlantı noktası internete maruz kalmadan kendi laboratuvar sanal makineleri bağlanmak için izin veren ağ geçidi makinesine belirteç kimlik doğrulaması kullanarak destekler. Bu makalede, laboratuvar makinelerine bağlanmak için belirteç kimlik doğrulaması kullanan bir laboratuvar nasıl ayarlanacak bir örnek üzerinden yürür.

## <a name="architecture-of-the-solution"></a>Çözümün mimarisi

![Çözümün mimarisi](./media/configure-lab-remote-desktop-gateway/architecture.png)

1. **Bağlan** düğmesini seçtiğinizde [RDP dosya içeriğini al](/rest/api/dtl/virtualmachines/getrdpfilecontents) eylemi çağrılır.1. 
1. RDP dosya içeriği al `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` eylemi, kimlik doğrulama belirteci istemek için çağrıda bulundu.
    1. `{gateway-hostname}`Azure portalındaki laboratuvarınız için **Laboratuvar Ayarları** sayfasında belirtilen ağ geçidi ana bilgisayarıadıdır. 
    1. `{lab-machine-name}`bağlamaya çalıştığınız makinenin adıdır.
    1. `{port-number}`bağlantının yapılması gereken bağlantı noktasıdır. Genellikle bu bağlantı noktası 3389'dur. Laboratuvar VM DevTest Labs [paylaşılan IP](devtest-lab-shared-ip.md) özelliği kullanıyorsa, bağlantı noktası farklı olacaktır.
1. Uzak masaüstü ağ geçidi, kimlik `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` doğrulama belirteci oluşturmak için aramayı Azure işlevinden erteler. DevTest Labs hizmeti, istek üstbilgisinde işlev anahtarını otomatik olarak içerir. İşlev anahtarı laboratuvarın anahtar kasasına kaydedilecek. Bu sırrın adı, laboratuvarın **Laboratuvar Ayarları** sayfasında Ağ **Geçidi belirteci sırrı** olarak gösterilir.
1. Azure işlevinin ağ geçidi makinesine karşı sertifika tabanlı belirteç kimlik doğrulaması için bir belirteç döndürmesi beklenir.  
1. RDP dosya içeriğini al eylemi, kimlik doğrulama bilgileri de dahil olmak üzere rdp dosyasının tamamını döndürür.
1. Tercih ettiğiniz RDP bağlantı programını kullanarak RDP dosyasını açarsınız. Tüm RDP bağlantı programlarının belirteç kimlik doğrulamasını desteklemediğini unutmayın. Kimlik doğrulama belirteci, işlev uygulaması tarafından ayarlanan bir son kullanma tarihine sahiptir. Belirteç süresi dolmadan önce laboratuvar VM bağlantısını yapın.
1. Uzak masaüstü ağ geçidi makinesi RDP dosyasındaki belirteci doğruladıktan sonra bağlantı laboratuvar makinenize iletilir.

### <a name="solution-requirements"></a>Çözüm gereksinimleri
DevTest Labs belirteç kimlik doğrulama özelliğiyle çalışmak için ağ geçidi makineleri, etki alanı adı hizmetleri (DNS) ve işlevler için birkaç yapılandırma gereksinimi vardır.

### <a name="requirements-for-remote-desktop-gateway-machines"></a>Uzak masaüstü ağ geçidi makineleri için gereksinimler
- TLS/SSL sertifikası, HTTPS trafiğini işlemek için ağ geçidi makinesine yüklenmelidir. Sertifika, yalnızca bir makine varsa, ağ geçidi çiftliği için yük dengeleyicisinin tam nitelikli alan adı (FQDN) veya makinenin fqdn'si ile eşleşmelidir. Joker TLS/SSL sertifikaları çalışmıyor.  
- Ağ geçidi makinesine (ler) yüklenen imza sertifikası. [Create-SigningCertificate.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1) komut dosyalarını kullanarak bir imzalama sertifikası oluşturun.
- Uzak masaüstü ağ geçidi için belirteç kimlik doğrulamasını destekleyen [Takılabilir Kimlik Doğrulama](https://code.msdn.microsoft.com/windowsdesktop/Remote-Desktop-Gateway-517d6273) modülünü yükleyin. Böyle bir modül `RDGatewayFedAuth.msi` bir örnek [System Center Virtual Machine Manager (VMM) görüntüleri](/system-center/vmm/install-console?view=sc-vmm-1807)ile birlikte geliyor. Sistem Merkezi hakkında daha fazla bilgi [için, Sistem Merkezi dokümantasyon](https://docs.microsoft.com/system-center/) ve [fiyatlandırma ayrıntılarına](https://www.microsoft.com/cloud-platform/system-center-pricing)bakın.  
- Ağ geçidi `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}`sunucusu' nun yaptığı istekler işleyebilir.

    Ağ geçidi ana bilgisayar adı, yalnızca bir makine varsa ağ geçidi çiftliğinin yük dengeleyicisinin FQDN'si veya makinenin fqdn'sidir. Bağlamaya `{lab-machine-name}` çalıştığınız laboratuvar makinesinin adı ve bağlantının `{port-number}` yapılacağı bağlantı noktasıdır.  Varsayılan olarak, bu bağlantı noktası 3389'dur.  Ancak, sanal makine DevTest Labs [paylaşılan IP](devtest-lab-shared-ip.md) özelliği kullanıyorsa, bağlantı noktası farklı olacaktır.
- Internet Information Server (IIS) için [Uygulama Yönlendirme İstek](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) modülü, `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` istekleri kimlik doğrulama için bir belirteç almak için isteği işleyen azure işlevine yönlendirmek için kullanılabilir.


## <a name="requirements-for-azure-function"></a>Azure işlevi için gereksinimler
Azure işlevi isteği biçimiyle `https://{function-app-uri}/app/host/{lab-machine-name}/port/{port-number}` işler ve ağ geçidi makinelerinde yüklü olan aynı imzalama sertifikasını temel alan kimlik doğrulama belirteci döndürür. İşlevlere `{function-app-uri}` erişmek için kullanılan uri. İşlev anahtarı, isteğin üstbilgisinde otomatik olarak geçirilir. Örnek bir işlev [https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs)için bkz. 


## <a name="requirements-for-network"></a>Ağ gereksinimleri

- Ağ geçidi makinelerine yüklenen TLS/SSL sertifikası ile ilişkili FQDN için DNS, trafiği ağ geçidi makinesine veya ağ geçidi makine çiftliğinin yük dengeleyicisine yönlendirmelidir.
- Laboratuvar makinesi özel IP'ler kullanıyorsa, ağ geçidi makinesinden laboratuvar makinesine, aynı sanal ağı paylaşarak veya eşlenmiş sanal ağları kullanarak bir ağ yolu olmalıdır.

## <a name="configure-the-lab-to-use-token-authentication"></a>Laboratuvarda belirteç kimlik doğrulaması kullanacak şekilde yapılandırma 
Bu bölümde, bir laboratuvarda belirteç kimlik doğrulamasını destekleyen uzak bir masaüstü ağ geçidi makinesini kullanacak şekilde nasıl yapılandırılanın gösterilmektedir. Bu bölüm, uzak bir masaüstü ağ geçidi çiftliğinin kendisini nasıl ayarlayacaklarını kapsamaz. Bu bilgiler [için,](#sample-to-create-a-remote-desktop-gateway) bu makalenin sonunda uzak bir masaüstü ağ geçidi bölümü oluşturmak için Örnek'e bakın. 

Laboratuvar ayarlarını güncelleştirmeden önce, laboratuarın anahtar kasasında kimlik doğrulama belirteci döndürmek için işlevi başarıyla yürütmek için gereken anahtarı depolayın. Azure portalındaki işleviçin **Yönet** sayfasında işlev anahtar değerini alabilirsiniz. Bir sırrı önemli bir kasada nasıl kaydedebilirsiniz hakkında daha fazla bilgi için [bkz.](../key-vault/quick-create-portal.md#add-a-secret-to-key-vault) Sırrın adını daha sonra kullanmak üzere kaydedin.

Laboratuvarın anahtar kasasının kimliğini bulmak için aşağıdaki Azure CLI komutunu çalıştırın: 

```azurecli
az resource show --name {lab-name} --resource-type 'Microsoft.DevTestLab/labs' --resource-group {lab-resource-group-name} --query properties.vaultName
```

Aşağıdaki adımları kullanarak belirteç kimlik doğrulamasını kullanacak şekilde laboratuarı yapılandırın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden, **laboratuvarınızı**seçin.
1. Laboratuvarın sayfasında **Yapılandırma ve ilkeler'i**seçin.
1. Sol menüde Ayarlar **bölümünde** Lab **ayarlarını**seçin.
1. Uzak **masaüstü** bölümüne, **Ağ Geçidi ana bilgisayar adı** alanı için uzak masaüstü hizmetleri ağ geçidi makinesinin veya çiftliğinin tam nitelikli alan adı (FQDN) veya IP adresini girin. Bu değer, ağ geçidi makinelerinde kullanılan TLS/SSL sertifikasının FQDN'si ile eşleşmelidir.

    ![Laboratuar ayarlarında uzak masaüstü seçenekleri](./media/configure-lab-remote-desktop-gateway/remote-desktop-options-in-lab-settings.png)
1. Uzak **masaüstü** bölümünde, **Ağ Geçidi belirteci** gizli için, daha önce oluşturulan sırrın adını girin. Bu değer işlev anahtarının kendisi değil, işlev anahtarını tutan laboratuvarın anahtar kasasındaki sırrın adıdır.

    ![Laboratuar ayarlarında ağ geçidi belirteç sırrı](./media/configure-lab-remote-desktop-gateway/gateway-token-secret.png)
1. **Kaydet** Değişiklik.

    > [!NOTE] 
    > **Kaydet'i**tıklatarak, [Uzak Masaüstü Ağ Geçidi'nin lisans koşullarını](https://www.microsoft.com/licensing/product-licensing/products)kabul edersiniz. Uzak ağ geçidi hakkında daha fazla bilgi için, [Uzak Masaüstü Hizmetlerine Hoş Geldiniz](https://aka.ms/rds) ve uzak masaüstü [ortamınızı dağıtın'](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure)a bakın.


Laboratuarı otomasyon yoluyla yapılandırma tercih edilirse, **ağ geçidi ana bilgisayarı** ve ağ geçidi **belirteç gizli** ayarlarını ayarlamak için örnek bir PowerShell komut dosyası için [Set-DevTestLabGateway.ps1'e](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Set-DevTestLabGateway.ps1) bakın. [Azure DevTest Labs GitHub deposu,](https://github.com/Azure/azure-devtestlab) ağ **geçidi ana bilgisayarı** ve ağ geçidi **belirteç gizli** ayarlarıyla bir laboratuvar oluşturan veya güncelleyen bir Azure Kaynak Yöneticisi şablonu da sağlar.

## <a name="configure-network-security-group"></a>Ağ güvenlik grubunu yapılandırma
Laboratuarı daha da güvenli hale getirmek için, laboratuvar sanal makineleri tarafından kullanılan sanal ağa bir ağ güvenlik grubu (NSG) eklenebilir. NSG'yi nasıl ayarlayaceksiniz yönergeler için [bkz.](../virtual-network/manage-network-security-group.md)

Burada sadece ilk laboratuvar makineleri ulaşmak için ağ geçidi geçer trafik sağlayan bir örnek NSG olduğunu. Bu kuraldaki kaynak, tek ağ geçidi makinesinin IP adresi veya ağ geçidi makinelerinin önündeki yük dengeleyicisinin IP adresidir.

![Ağ güvenlik grubu - kurallar](./media/configure-lab-remote-desktop-gateway/network-security-group-rules.png)

## <a name="sample-to-create-a-remote-desktop-gateway"></a>Uzak bir masaüstü ağ geçidi oluşturmak için örnek

> [!NOTE] 
> Örnek şablonları kullanarak, Uzak [Masaüstü Ağ Geçidi'nin lisans koşullarını](https://www.microsoft.com/licensing/product-licensing/products)kabul edersiniz. Uzak ağ geçidi hakkında daha fazla bilgi için, [Uzak Masaüstü Hizmetlerine Hoş Geldiniz](https://aka.ms/rds) ve uzak masaüstü [ortamınızı dağıtın'](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure)a bakın.

[Azure DevTest Labs GitHub deposu, DevTest](https://github.com/Azure/azure-devtestlab) Labs ile belirteç kimlik doğrulaması ve uzak masaüstü ağ geçidi ni kullanmak için gereken kaynakların kurulumuna yardımcı olmak için birkaç örnek sağlar. Bu örnekler, ağ geçidi makineleri, laboratuvar ayarları ve işlev uygulaması için Azure Kaynak Yöneticisi şablonlarını içerir.

Uzak masaüstü ağ geçidi çiftliği için örnek bir çözüm ayarlamak için aşağıdaki adımları izleyin.

1. İmza sertifikası oluşturun.  [Oluştur-İmzaSertifikası.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1)çalıştırın. Oluşturulan sertifikanın parmak izini, parolayı ve Base64 kodlamasını kaydedin.
2. TLS/SSL sertifikası alın. TLS/SSL sertifikasıile ilişkili FQDN, kontrol ettiğiniz etki alanı için olmalıdır. Bu sertifika için parmak izini, parolayı ve Base64 kodlamasını kaydedin. PowerShell kullanarak parmak izi almak için aşağıdaki komutları kullanın.

    ```powershell
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate;
    $cer.Import(‘path-to-certificate’);
    $hash = $cer.GetCertHashString()
    ```

    PowerShell kullanarak Base64 kodlamaalmak için aşağıdaki komutu kullanın.

    ```powershell
    [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(‘path-to-certificate’))
    ```
3. Dosyaları ' [https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway)dan indirin.

    Şablon, aynı temel URI'de birkaç diğer Kaynak Yöneticisi şablonuna ve ilgili kaynağa erişim gerektirir. Ve RDGatewayFedAuth.msi'deki [https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway) tüm dosyaları bir depolama hesabındaki blob konteynerine kopyalayın.  
4. **azuredeploy.json'dan** [https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway)dağıt. Şablon aşağıdaki parametreleri alır:
    - adminUsername – Gerekli.  Ağ geçidi makineleri için yönetici kullanıcı adı.
    - adminPassword – Gerekli. Ağ geçidi makineleri için yönetici hesabı nın parolası.
    - instanceCount – Oluşturulacak ağ geçidi makineleri sayısı.  
    - alwaysOn – Oluşturulan Azure İşlevler uygulamasını sıcak bir durumda tutup tutmayacağını gösterir. Azure İşlevleri uygulamasını tutmak, kullanıcıların ilk olarak laboratuvar VM'lerine bağlanmaya çalıştıklarında gecikmeleri önler, ancak bunun maliyet etkileri vardır.  
    - tokenLifetime – Oluşturulan belirteç süresi geçerli olacaktır. Biçim HH:MM:SS'dir.
    - sslCertificate – Ağ geçidi makinesi için TLS/SSL sertifikasının Base64 kodlaması.
    - sslCertificatePassword – Ağ geçidi makinesi için TLS/SSL sertifikasının şifresi.
    - sslCertificateThumbprint - TLS/SSL sertifikasının yerel sertifika deposunda kimlik tespiti için sertifika parmak izi.
    - signCertificate – Ağ geçidi makinesi için imza sertifikası için Base64 kodlama.
    - signCertificatePassword – Ağ geçidi makinesi için sertifika imzalama parolası.
    - signCertificateThumbprint - İmza sertifikasının yerel sertifika deposunda kimlik tespiti için sertifika parmak izi.
    - _artifactsLocation – URI tüm destekleyici kaynakların bulunabileceği yer. Bu değer, göreceli bir yol değil, tam nitelikli bir UIR olmalıdır.
    - _artifactsLocationSasToken - Konum bir Azure depolama hesabıysa, destek kaynaklarına erişmek için kullanılan Paylaşılan Erişim İmzası (SAS) belirteci.

    Şablon, aşağıdaki komutu kullanarak Azure CLI kullanılarak dağıtılabilir:

    ```azurecli
    az group deployment create --resource-group {resource-group} --template-file azuredeploy.json --parameters @azuredeploy.parameters.json -–parameters _artifactsLocation="{storage-account-endpoint}/{container-name}" -–parameters _artifactsLocationSasToken = "?{sas-token}"
    ```

    Parametrelerin açıklamaları şunlardır:

    - {depolama-hesap sonu noktası} çalıştırılarak `az storage account show --name {storage-acct-name} --query primaryEndpoints.blob`elde edilebilir.  {storage-acct-name} yüklediğiniz dosyaları tutan depolama hesabının adıdır.  
    - {kapsayıcı adı} yüklediğiniz dosyaları tutan {depolama-acct adı} içinde kapsayıcının adıdır.  
    - {sas-token} çalıştırılarak `az storage container generate-sas --name {container-name} --account-name {storage-acct-name} --https-only –permissions drlw –expiry {utc-expiration-date}`elde edilebilir. 
        - {storage-acct-name} yüklediğiniz dosyaları tutan depolama hesabının adıdır.  
        - {kapsayıcı adı} yüklediğiniz dosyaları tutan {depolama-acct adı} içinde kapsayıcının adıdır.  
        - {utc-son kullanma tarihi} UTC'de SAS belirteci süresinin dolacağı ve SAS belirtecinin artık depolama hesabına erişmek için kullanılamayacağı tarihtir.

    Şablon dağıtım çıktısından ağ geçidiFQDN ve gatewayIP değerlerini kaydedin. Ayrıca, [işlev uygulaması ayarları](../azure-functions/functions-how-to-use-azure-function-app-settings.md) sekmesinde bulunan yeni oluşturulan işlev için işlev anahtarının değerini kaydetmeniz gerekir.
5. TlS/SSL sertifikasının FQDN'sinin önceki adımdan gatewayIP'in IP adresine yönlendirilen DNS'yi yapılandırın.

    Uzak Masaüstü Ağ Geçidi çiftliği oluşturulduktan ve uygun DNS güncelleştirmeleri yapıldıktan sonra, DevTest Labs'daki bir laboratuvar tarafından kullanılmaya hazırdır. **Ağ geçidi ana bilgisayar adı** ve ağ geçidi **belirteç gizli** ayarları, dağıttığınız ağ geçidi makinesini kullanacak şekilde yapılandırılmalıdır. 

    > [!NOTE]
    > Laboratuvar makinesi özel IP'ler kullanıyorsa, ağ geçidi makinesinden laboratuvar makinesine, aynı sanal ağı paylaşarak veya eşlenmiş bir sanal ağ kullanarak bir ağ yolu olmalıdır.

    Hem ağ geçidi hem de laboratuvar yapılandırıldıktan sonra, laboratuvar kullanıcısı **Connect'i** tıklattığında oluşturulan bağlantı dosyası, token kimlik doğrulaması kullanarak bağlanmak için gereken bilgileri otomatik olarak içerir.     

## <a name="next-steps"></a>Sonraki adımlar
Uzak Masaüstü Hizmetleri hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [Uzak Masaüstü Hizmetleri belgeleri](/windows-server/remote/remote-desktop-services/Welcome-to-rds)


