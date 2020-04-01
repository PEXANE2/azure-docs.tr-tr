---
title: v1'den v2'ye geçiş - Azure Uygulama Ağ Geçidi
description: Bu makalede, Azure Uygulama Ağ Geçidi ve Web Uygulaması Güvenlik Duvarı'nı v1'den v2'ye nasıl geçirebilirsiniz
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/31/2020
ms.author: victorh
ms.openlocfilehash: 96f3825288846e86771ef3907eb4da4e58630df3
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475183"
---
# <a name="migrate-azure-application-gateway-and-web-application-firewall-from-v1-to-v2"></a>Azure Uygulama Ağ Geçidi'ni ve Web Uygulaması Güvenlik Duvar'ı v1'den v2'ye geçirin

[Azure Uygulama Ağ Geçidi ve Web Uygulaması Güvenlik Duvarı (WAF) v2](application-gateway-autoscaling-zone-redundant.md) artık kullanılabilir durumda olup, otomatik ölçekleme ve kullanılabilirlik bölgesi artıklığı gibi ek özellikler sunar. Bununla birlikte mevcut v1 ağ geçitleri v2'ye otomatik olarak yükseltilmez. V1'den v2'ye geçiş yapmak istiyorsanız, bu makaledeki adımları izleyin.

Bir geçişin iki aşaması vardır:

1. Yapılandırmayı geçirin
2. İstemci trafiğini geçirme

Bu makalede, yapılandırma geçişi kapsar. İstemci trafiği geçişi, özel ortamınıza bağlı olarak değişir. Ancak, bazı üst düzey, genel öneriler [sağlanır.](#migrate-client-traffic)

## <a name="migration-overview"></a>Geçişe genel bakış

Aşağıdakileri yapan bir Azure PowerShell komut dosyası kullanılabilir:

* Belirttiğiniz bir sanal ağ alt ağında yeni bir Standard_v2 veya WAF_v2 ağ geçidi oluşturur.
* V1 Standard veya WAF ağ geçidiyle ilişkili yapılandırmayı yeni oluşturulan Standard_V2 veya WAF_V2 ağ geçidine sorunsuz bir şekilde kopyalar.

### <a name="caveatslimitations"></a>Uyarılar\Sınırlamalar

* Yeni v2 ağ geçidiyeni genel ve özel IP adreslerine sahiptir. Varolan v1 ağ geçidiyle ilişkili IP adreslerini sorunsuz bir şekilde v2'ye taşımak mümkün değildir. Ancak, varolan (tahsis edilmemiş) bir genel veya özel IP adresini yeni v2 ağ geçidine ayırabilirsiniz.
* V1 ağ geçidinizin bulunduğu sanal ağınızdaki başka bir alt ağ için bir IP adres alanı sağlamanız gerekir. Komut dosyası, v1 ağ geçidi olan varolan alt ağlarda v2 ağ geçidi oluşturamaz. Ancak, varolan alt ağ zaten bir v2 ağ geçidi varsa, yeterli IP adresi alanı olması koşuluyla çalışmaya devam edebilir.
* Bir SSL yapılandırmasını geçirmek için v1 ağ geçidinizde kullanılan tüm SSL sertifikalarını belirtmeniz gerekir.
* V1 ağ geçidiniz için FIPS modu etkinleştirilmişse, yeni v2 ağ geçidinize geçirilmez. FIPS modu v2'de desteklenmez.
* v2 IPv6'yı desteklemez, bu nedenle IPv6 etkin v1 ağ geçitleri geçirilemiyor. Komut dosyasını çalıştırın, tamamlanmamış olabilir.
* v1 ağ geçidinde yalnızca özel bir IP adresi varsa, komut dosyası yeni v2 ağ geçidi için ortak bir IP adresi ve özel bir IP adresi oluşturur. v2 ağ geçitleri şu anda yalnızca özel IP adreslerini desteklemiyor.
* Harfler, basamaklar, tireler ve alt çizilmeler dışında herhangi bir ad içeren üstbilgi uygulamanıza geçirilmez. Bu yalnızca üstbilgi adları için geçerlidir, üstbilgi değerleri için değil. Bu v1 bir kırılma değişikliğidir.

## <a name="download-the-script"></a>Komut dosyasını indirin

[PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureAppGWMigration)geçiş komut dosyasını indirin.

## <a name="use-the-script"></a>Komut dosyasını kullanma

Yerel PowerShell ortamı kurulumunuza ve tercihlerinize bağlı olarak sizin için iki seçenek vardır:

* Azure Az modülleri yüklü değilse veya Azure Az modüllerini kaldırmakta sakınca görmüyorsanız, en iyi seçenek `Install-Script` komut dosyasını çalıştırma seçeneğini kullanmaktır.
* Azure Az modüllerini saklamanız gerekiyorsa, en iyi seçenek komut dosyasını indirip doğrudan çalıştırmaktır.

Azure Az modüllerinin yüklü olup olmadığını belirlemek `Get-InstalledModule -Name az`için çalıştırın. Yüklü Az modülleri görmüyorsanız, `Install-Script` yöntemi kullanabilirsiniz.

### <a name="install-using-the-install-script-method"></a>Install-Script yöntemini kullanarak yükleme

Bu seçeneği kullanmak için bilgisayarınızda Azure Az modüllerinin yüklü olmaması gerekir. Yüklüyse, aşağıdaki komut bir hata görüntüler. Azure Az modüllerini kaldırabilir veya komut dosyasını el ile indirip çalıştırmak için diğer seçeneği kullanabilirsiniz.
  
Komut dosyasını aşağıdaki komutla çalıştırın:

`Install-Script -Name AzureAppGWMigration`

Bu komut aynı zamanda gerekli Az modüllerini de yükler.  

### <a name="install-using-the-script-directly"></a>Komut dosyasını kullanarak doğrudan yükleme

Bazı Azure Az modülleriniz yüklüyse ve bunları kaldıramıyorsanız (veya kaldırmak istemiyorsanız), komut dosyası indirme bağlantısındaki **Manuel İndir** sekmesini kullanarak komut dosyasını el ile indirebilirsiniz. Komut dosyası ham nupkg dosyası olarak indirilir. Bu nupkg dosyasından komut dosyasını yüklemek için Bkz. [Manuel Paket İndir.](/powershell/scripting/gallery/how-to/working-with-packages/manual-download)

Betiği çalıştırmak için:

1. Azure'a bağlanmak için kullanın. `Connect-AzAccount`

1. Az `Import-Module Az` modüllerini almak için kullanın.

1. Gerekli `Get-Help AzureAppGWMigration.ps1` parametreleri incelemek için çalıştırın:

   ```
   AzureAppGwMigration.ps1
    -resourceId <v1 application gateway Resource ID>
    -subnetAddressRange <subnet space you want to use>
    -appgwName <string to use to append>
    -sslCertificates <comma-separated SSLCert objects as above>
    -trustedRootCertificates <comma-separated Trusted Root Cert objects as above>
    -privateIpAddress <private IP string>
    -publicIpResourceId <public IP name string>
    -validateMigration -enableAutoScale
   ```

   Komut dosyası için parametreler:
   * **resourceId: [String]: Gerekli** - Bu, varolan Standart v1 veya WAF v1 ağ geçidiniz için Azure Kaynak Kimliği'dir. Bu dize değerini bulmak için Azure portalına gidin, uygulama ağ geçidinizi veya WAF kaynağınızı seçin ve ağ geçidi için **Özellikler** bağlantısını tıklatın. Kaynak Kimliği bu sayfada yer alır.

     Kaynak Kimliğini almak için aşağıdaki Azure PowerShell komutlarını da çalıştırabilirsiniz:

     ```azurepowershell
     $appgw = Get-AzApplicationGateway -Name <v1 gateway name> -ResourceGroupName <resource group Name> 
     $appgw.Id
     ```

   * **subnetAddressRange: [String]: Gerekli** - Bu, yeni v2 ağ geçidinizi içeren yeni bir alt ağ geçidi için ayırdığınız (veya ayırmak istediğiniz) IP adresi alanıdır. Bu, CIDR gösteriminde belirtilmelidir. Örneğin: 10.0.0.0/24. Bu alt ağı önceden oluşturmanız gerekmez. Komut dosyası yoksa sizin için oluşturur.
   * **appgwName: [String]: İsteğe bağlı**. Bu, yeni Standard_v2 veya WAF_v2 ağ geçidinin adı olarak kullanacağınız bir dizedir. Bu parametre sağlanmazsa, varolan v1 ağ geçidinizin adı eklenmiş *_v2* sonekiyle birlikte kullanılır.
   * **sslCertificates: [PSApplicationGatewaySslCertificate]: İsteğe bağlı**.  V1 ağ geçidinizden SSL sertifikalarını temsil etmek için oluşturduğunuz PSApplicationGatewaySslCertificate nesnelerinin virgülle ayrılmış bir listesi yeni v2 ağ geçidine yüklenmelidir. Standart v1 veya WAF v1 ağ geçidiniz için yapılandırılan SSL sertifikalarınızın her biri için, burada `New-AzApplicationGatewaySslCertificate` gösterilen komut aracılığıyla yeni bir PSApplicationGatewaySslCertificate nesnesi oluşturabilirsiniz. SSL Cert dosyanıza ve parolanıza giden yola ihtiyacınız vardır.

     Bu parametre yalnızca v1 ağ geçidiniz veya WAF'ınız için yapılandırılan HTTPS dinleyicileriniz yoksa isteğe bağlıdır. En az bir HTTPS dinleyici kurulumunuz varsa, bu parametreyi belirtmeniz gerekir.

      ```azurepowershell  
      $password = ConvertTo-SecureString <cert-password> -AsPlainText -Force
      $mySslCert1 = New-AzApplicationGatewaySslCertificate -Name "Cert01" `
        -CertificateFile <Cert-File-Path-1> `
        -Password $password 
      $mySslCert2 = New-AzApplicationGatewaySslCertificate -Name "Cert02" `
        -CertificateFile <Cert-File-Path-2> `
        -Password $password
      ```

     Önceki örnekte `$mySslCert1, $mySslCert2` bu parametrenin değerleri olarak komut dosyasına geçebilirsiniz (virgülden ayrılmış).
   * **güvenilirRootCertificates: [PSApplicationGatewayTrustedRootCertificate]: İsteğe bağlı**. V2 ağ geçidinizden arka uç örneklerinizi doğrulamak için [Güvenilen Kök sertifikalarını](ssl-overview.md) temsil etmek üzere oluşturduğunuz PSApplicationGatewayTrustedRootCertificate nesnelerinin virgülle ayrılmış listesi.
   
      ```azurepowershell
      $certFilePath = ".\rootCA.cer"
      $trustedCert = New-AzApplicationGatewayTrustedRootCertificate -Name "trustedCert1" -CertificateFile $certFilePath
      ```

      PSApplicationGatewayTrustedRootCertificate nesnelerinin bir listesini oluşturmak için Bkz. [Yeni-AzApplicationGatewayTrustedRootCertificate](https://docs.microsoft.com/powershell/module/Az.Network/New-AzApplicationGatewayTrustedRootCertificate?view=azps-2.1.0&viewFallbackFrom=azps-2.0.0).
   * **privateIpAddress: [String]: İsteğe bağlı**. Yeni v2 ağ geçidinizle ilişkilendirmek istediğiniz belirli bir özel IP adresi.  Bu, yeni v2 ağ geçidiniz için ayırdığınız VNet'ten olmalıdır. Bu belirtilmemişse, komut dosyası v2 ağ geçidiniz için özel bir IP adresi ayırır.
   * **publicIpResourceId: [String]: İsteğe bağlı**. Aboneliğinizde, yeni v2 ağ geçidine ayırmak istediğiniz mevcut genel IP adresi (standart SKU) kaynağının kaynak Kimliği. Bu belirtilmemişse, komut dosyası aynı kaynak grubuna yeni bir genel IP ayırır. Adı *-IP* eklenen v2 ağ geçidinin adıdır.
   * **doğrulamaMigration: [switch]: İsteğe bağlı**. Komut dosyasının v2 ağ geçidi oluşturma ve yapılandırma kopyasından sonra bazı temel yapılandırma karşılaştırma doğruları yapmasını istiyorsanız bu parametreyi kullanın. Varsayılan olarak, hiçbir doğrulama yapılır.
   * **EnableAutoScale: [switch]: İsteğe bağlı**. Komut dosyasının oluşturulduktan sonra yeni v2 ağ geçidinde Otomatik Ölçekleme'yi etkinleştirmesini istiyorsanız bu parametreyi kullanın. Varsayılan olarak, Otomatik Ölçekleme devre dışı bırakılır. Yeni oluşturulan v2 ağ geçidinde her zaman el ile etkinleştirebilirsiniz.

1. Uygun parametreleri kullanarak komut dosyasını çalıştırın. Bitirmesi beş ila yedi dakika sürebilir.

    **Örnek**

   ```azurepowershell
   AzureAppGWMigration.ps1 `
      -resourceId /subscriptions/8b1d0fea-8d57-4975-adfb-308f1f4d12aa/resourceGroups/MyResourceGroup/providers/Microsoft.Network/applicationGateways/myv1appgateway `
      -subnetAddressRange 10.0.0.0/24 `
      -appgwname "MynewV2gw" `
      -sslCertificates $mySslCert1,$mySslCert2 `
      -trustedRootCertificates $trustedCert `
      -privateIpAddress "10.0.0.1" `
      -publicIpResourceId "/subscriptions/8b1d0fea-8d57-4975-adfb-308f1f4d12aa/resourceGroups/MyResourceGroup/providers/Microsoft.Network/publicIPAddresses/MyPublicIP" `
      -validateMigration -enableAutoScale
   ```

## <a name="migrate-client-traffic"></a>İstemci trafiğini geçirin

İlk olarak, komut dosyasının v1 ağ geçidinizden tam yapılandırmaile birlikte yeni bir v2 ağ geçidi ni başarıyla oluşturacağından çift kontrol edin. Bunu Azure portalından doğrulayabilirsiniz.

Ayrıca, el ile test olarak v2 ağ geçidi üzerinden trafik küçük bir miktar gönderin.
  
Geçerli uygulama ağ geçidinizin (Standart) istemci trafiğini alabileceği birkaç senaryo ve her biri için önerilerimiz aşağıda verilmiştir:

* **Standart v1 veya WAF v1 ağ geçidinizle ilişkili ön uç IP adresini (A kaydı nı kullanarak) işaret eden özel bir DNS bölgesi (örneğin, contoso.com).**

    DNS kaydınızı, Standard_v2 uygulama ağ geçidinizle ilişkili ön uç IP veya DNS etiketini işaret etmek için güncelleştirebilirsiniz. DNS kaydınızda yapılandırılan TTL'ye bağlı olarak, tüm istemci trafiğinizin yeni v2 ağ geçidinize geçiş etmesi biraz zaman alabilir.
* **V1 ağ geçidinizle ilişkili DNS etiketine (örneğin: CNAME *myappgw.eastus.cloudapp.azure.com* kaydı myappgw.eastus.cloudapp.azure.com) işaret eden özel bir DNS bölgesi (örneğin, contoso.com).**

   İki seçeneğiniz var:

  * Uygulama ağ geçidinizde genel IP adresleri kullanıyorsanız, trafiği (ağırlıklı trafik yönlendirme yöntemi) yeni v2 ağ geçidine artımlı olarak yönlendirmek için Trafik Yöneticisi profilini kullanarak kontrollü, parçalı geçiş yapabilirsiniz.

    Bunu, [Trafik Yöneticisi profiline](../traffic-manager/traffic-manager-routing-methods.md#weighted-traffic-routing-method)hem v1 hem de v2 uygulama ağ geçitlerinin DNS etiketlerini ekleyerek ve özel DNS kaydınızı (örneğin,) `www.contoso.com`Trafik Yöneticisi etki alanına (örneğin, contoso.trafficmanager.net) cnameederek yapabilirsiniz.
  * Veya, yeni v2 uygulama ağ geçidinin DNS etiketini işaret etmek için özel etki alanı DNS kaydınızı güncelleştirebilirsiniz. DNS kaydınızda yapılandırılan TTL'ye bağlı olarak, tüm istemci trafiğinizin yeni v2 ağ geçidinize geçiş etmesi biraz zaman alabilir.
* **Müşterileriniz uygulama ağ geçidinizin ön uç IP adresine bağlanır.**

   Yeni oluşturulan v2 uygulama ağ geçidiyle ilişkili IP adresini(es) kullanmak için müşterilerinizi güncelleştirin. IP adreslerini doğrudan kullanmamanızı öneririz. Kendi özel DNS bölgenize (örneğin, contoso.com) CNAME yapabileceğiniz uygulama ağ geçidinizle ilişkili DNS ad etiketini (örneğin, yourgateway.eastus.cloudapp.azure.com) kullanmayı düşünün.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Azure PowerShell komut dosyasında yapılandırmayı v1'den v2'ye geçirmek için herhangi bir sınırlama var mı?

Evet. Bkz. [Uyarılar/Sınırlamalar.](#caveatslimitations)

### <a name="is-this-article-and-the-azure-powershell-script-applicable-for-application-gateway-waf-product-as-well"></a>Bu makale ve Azure PowerShell komut dosyası Application Gateway WAF ürünü için de geçerli mi? 

Evet.

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-v1-gateway-to-the-newly-created-v2-gateway"></a>Azure PowerShell komut dosyası, v1 ağ geçidimden yeni oluşturulan v2 ağ geçidine giden trafik te de geçiş yapıyor mu?

Hayır. Azure PowerShell komut dosyası yalnızca yapılandırmayı geçirtir. Gerçek trafik geçişi sizin sorumluluğunuzda dır ve sizin kontrolünüzdedir.

### <a name="is-the-new-v2-gateway-created-by-the-azure-powershell-script-sized-appropriately-to-handle-all-of-the-traffic-that-is-currently-served-by-my-v1-gateway"></a>Azure PowerShell komut dosyası tarafından oluşturulan yeni v2 ağ geçidi, şu anda v1 ağ geçidim tarafından sunulan tüm trafiği işlemek için uygun şekilde boyutlandırılır mı?

Azure PowerShell komut dosyası, varolan v1 ağ geçidinizdeki trafiği işlemek için uygun boyuta sahip yeni bir v2 ağ geçidi oluşturur. Otomatik ölçekleme varsayılan olarak devre dışı bırakılır, ancak komut dosyasını çalıştırdığınızda Otomatik Ölçekleme'yi etkinleştirebilirsiniz.

### <a name="i-configured-my-v1-gateway--to-send-logs-to-azure-storage-does-the-script-replicate-this-configuration-for-v2-as-well"></a>V1 ağ geçidimi Azure depolama alanına günlük göndermek için yapılandırıldım. Komut dosyası da v2 için bu yapılandırmayı çoğaltıyor mu?

Hayır. Komut dosyası v2 için bu yapılandırmayı çoğaltmaz. Günlük yapılandırmasını geçirilen v2 ağ geçidine ayrı olarak eklemeniz gerekir.

### <a name="does-this-script-support-certificates-uploaded-to-azure-keyvault-"></a>Bu komut dosyası, Azure KeyVault'a yüklenen sertifikaları destekliyor mu?

Hayır. Şu anda komut dosyası KeyVault sertifikaları desteklemiyor. Ancak, bu gelecekteki bir sürümü için kabul ediliyor.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Ben bu komut dosyası kullanarak bazı sorunlar la karşılaştı. Nasıl yardım alabilirim?
  
Azure Desteği ile appgwmigrationsup@microsoft.combir destek servis talebi açmak için e-posta gönderebilir veya her ikisini de yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama Ağ Geçidi v2 hakkında bilgi edinin](application-gateway-autoscaling-zone-redundant.md)
