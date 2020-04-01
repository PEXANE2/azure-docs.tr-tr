---
title: ARM ile ASE oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak harici veya ILB Uygulama Hizmeti ortamını nasıl oluşturabilirsiniz öğrenin.
author: ccompy
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: e06fcdbac097e85c039e34274c61cb51ee06bcd6
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478329"
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak ASE oluşturma

## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Uygulama Hizmeti ortamları (AsEs), Bir Azure sanal ağında (VNet) dahili bir adreste internet erişimine uygun bir bitiş noktası veya bir bitiş noktasıyla oluşturulabilir. Dahili bir uç nokta ile oluşturulduğunda, bu uç nokta dahili yük dengeleyicisi (ILB) adı verilen bir Azure bileşeni tarafından sağlanır. Dahili bir IP adresindeki ASE'ye ILB ASE denir. Ortak bitiş noktası olan ASE'ye Dış ASE denir. 

Bir ASE, Azure portalı veya Azure Kaynak Yöneticisi şablonu kullanılarak oluşturulabilir. Bu makalede, Kaynak Yöneticisi şablonları ile harici ase veya ILB ASE oluşturmak için gereken adımlar ve sözdizimi üzerinden yürür. Azure portalında nasıl BIR ASE oluşturabilirsiniz öğrenmek için [bkz.][MakeExternalASE] [Make an ILB ASE][MakeILBASE]

Azure portalında bir ASE oluşturduğunuzda, VNet'inizi aynı anda oluşturabilir veya dağıtmak için önceden varolan bir VNet seçebilirsiniz. Şablondan bir ASE oluşturduğunuzda, şu şekilde başlamalısınız: 

* Kaynak Yöneticisi VNet.
* VNet'in bir alt ağı. Gelecekteki büyüme ve ölçekleme `/24` ihtiyaçlarını karşılamak için 256 adresli bir ASE alt ağ boyutu öneriyoruz. ASE oluşturulduktan sonra boyutunu değiştiremezsiniz.
* VNet'inizdeki kaynak kimliği. Bu bilgileri sanal ağ özellikleriniz altında Azure portalından alabilirsiniz.
* Dağıtmak istediğiniz abonelik.
* Dağıtmak istediğiniz konum.

ASE oluşturmanızı otomatikleştirmek için:

1. Bir şablondan ASE'yi oluşturun. Harici BIR ASE oluşturursanız, bu adımdan sonra bitirirsiniz. Bir ILB ASE oluşturursanız, yapmanız gereken birkaç şey daha vardır.

2. ILB ASE'niz oluşturulduktan sonra, ILB ASE etki alanınızın eşleşen bir TLS/SSL sertifikası yüklenir.

3. Yüklenen TLS/SSL sertifikası ILB ASE'ye "varsayılan" TLS/SSL sertifikası olarak atanır.  Bu sertifika, ASE'ye atanan ortak kök etki alanını (örneğin) `https://someapp.mycustomrootdomain.com`kullandıklarında ILB ASE'deki uygulamalara TLS/SSL trafiği için kullanılır.


## <a name="create-the-ase"></a>ASE'yi oluştur
Bir ASE ve ilişkili parametreler dosyası oluşturan bir Kaynak Yöneticisi şablonu [GitHub'daki bir örnekte][quickstartasev2create] kullanılabilir.

ILB ASE yapmak istiyorsanız, bu Kaynak Yöneticisi şablon [örneklerini][quickstartilbasecreate]kullanın. O kullanım çantasına hizmet ediyorlar. *Azuredeploy.parameters.json* dosyasındaki parametrelerin çoğu ILB'ler ve Harici AS'ler oluşturulmasında yaygındır. Aşağıdaki liste, bir ILB ASE oluşturduğunuzda özel not parametrelerini veya benzersiz parametreleri çağırır:

* *internalLoadBalancingMode*: Çoğu durumda, bu 3, hangi port80/443 hem HTTP / HTTPS trafik anlamına gelir ve ase FTP hizmeti tarafından dinlenen kontrol / veri kanalı bağlantı noktaları, bir ILB tahsis sanal ağ iç adresine bağlı olacaktır ayarlayın. Bu özellik 2 olarak ayarlanmışsa, yalnızca FTP hizmetiyle ilgili bağlantı noktaları (hem denetim hem de veri kanalları) bir ILB adresine bağlıdır. HTTP/HTTPS trafiği genel VIP'de kalır.
* *dnsSuffix*: Bu parametre, ASE'ye atanan varsayılan kök etki alanını tanımlar. Azure Uygulama Hizmeti'nin genel varyasyonunda, tüm web uygulamaları için varsayılan kök etki alanı *azurewebsites.net.* ILB ASE müşterinin sanal ağına dahil olduğundan, kamu hizmetinin varsayılan kök etki alanını kullanmak mantıklı değildir. Bunun yerine, Bir ILB ASE bir şirketin iç sanal ağ içinde kullanmak için mantıklı varsayılan bir kök etki alanı olmalıdır. Örneğin, Contoso Corporation, yalnızca *Contoso'nun* sanal ağında çözülebilir ve erişilebilir olması amaçlanan uygulamalar için varsayılan kök internal-contoso.com etki alanı kullanabilir. 
* *ipSslAddressCount*: ILB'lerin yalnızca tek bir ILB adresi olduğundan, bu parametre *azuredeploy.json* dosyasında otomatik olarak 0 değerine varsayılandır. ILB ASE için açık IP-SSL adresi yoktur. Bu nedenle, bir ILB ASE için IP-SSL adres havuzu sıfır olarak ayarlanmalıdır. Aksi takdirde, bir sağlama hatası oluşur. 

*azuredeploy.parameters.json* dosyası dolduruldıktan sonra PowerShell kod snippet'ini kullanarak ASE'yi oluşturun. Makinenizdeki Kaynak Yöneticisi şablon dosyası konumlarını eşleşecek şekilde dosya yollarını değiştirin. Kaynak Yöneticisi dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamayı unutmayın:

```powershell
$templatePath="PATH\azuredeploy.json"
$parameterPath="PATH\azuredeploy.parameters.json"

New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath
```

ASE'nin oluşturulması yaklaşık bir saat sürer. Ardından ASE, dağıtımı tetikleyen abonelik için ASE listesinde portalda belirir.

## <a name="upload-and-configure-the-default-tlsssl-certificate"></a>"Varsayılan" TLS/SSL sertifikasını yükleme ve yapılandırma
TLS/SSL sertifikası, uygulamalara TLS bağlantıları kurmak için kullanılan "varsayılan" TLS/SSL sertifikası olarak ASE ile ilişkilendirilmelidir. ASE'nin varsayılan DNS soneki *internal-contoso.com*ise `https://some-random-app.internal-contoso.com` , bağlantı **.internal-contoso.com*için geçerli bir TLS/SSL sertifikası gerektirir. 

Dahili sertifika yetkililerini kullanarak, harici bir kuruluştan sertifika satın alarak veya kendi imzalanmış bir sertifika kullanarak geçerli bir TLS/SSL sertifikası edinin. TLS/SSL sertifikasının kaynağıne bakılmaksızın, aşağıdaki sertifika özniteliklerinin doğru şekilde yapılandırılması gerekir:

* **Konu**: Bu öznitelik **.your-root-domain-here.com*olarak ayarlanmalıdır.
* **Konu Alternatif Adı**: Bu öznitelik hem **.your-root-domain-here.com* ve **.scm.your-root-domain-here.com*içermelidir. Her uygulama ile ilişkili SCM/Kudu sitesine TLS bağlantıları *your-app-name.scm.your-root-domain-here.com*formun adresini kullanır.

Elinizde geçerli bir TLS/SSL sertifikası ile iki ek hazırlık adımı gereklidir. TLS/SSL sertifikasını .pfx dosyası olarak dönüştürün/kaydedin. .pfx dosyasının tüm ara ve kök sertifikaları içermesi gerektiğini unutmayın. Bir parola ile güvenli hale getirin.

TLS/SSL sertifikası Kaynak Yöneticisi şablonu kullanılarak yüklendiğinden.pfx dosyasının base64 dizesine dönüştürülmesi gerekir. Kaynak Yöneticisi şablonları metin dosyaları olduğundan, .pfx dosyası base64 dizesine dönüştürülmelidir. Bu şekilde şablonun bir parametresi olarak eklenebilir.

Aşağıdaki PowerShell kod parçacıklarını kullanarak:

* Kendi imzalı bir sertifika oluşturun.
* Sertifikayı .pfx dosyası olarak dışa aktarın.
* .pfx dosyasını base64 kodlanmış bir dize dönüştürün.
* Base64 kodlanmış dizeyi ayrı bir dosyaya kaydedin. 

Base64 kodlama için bu PowerShell kodu [PowerShell komut blogundan][examplebase64encoding]uyarlanmıştır:

```powershell
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.pfx"
Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

$fileContentBytes = get-content -encoding byte $fileName
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$fileContentEncoded | set-content ($fileName + ".b64")
```

TLS/SSL sertifikası başarıyla oluşturulduktan ve base64 kodlanmış bir dize dönüştürüldükten sonra, VerHub'daki [varsayılan SSL sertifikasını yapılandırma][quickstartconfiguressl] örneği Kaynak Yöneticisi şablonuna kullanın. 

*azuredeploy.parameters.json* dosyasındaki parametreler burada listelenmiştir:

* *appServiceEnvironmentName*: ILB ASE'nin adı yapılandırıldı.
* *existingAseLocation*: ILB ASE'nin dağıtıldığı Azure bölgesini içeren metin dizesi.  Örneğin: "Güney Orta ABD".
* *pfxBlobString*: .pfx dosyasının 64 kodlu dize gösterimi. Daha önce gösterilen kod parçacıklarını kullanın ve "exportedcert.pfx.b64" de bulunan dizekopyalayın. *PfxBlobString* özniteliğinin değeri olarak yapıştırın.
* şifre : .pfx dosyasını güvenli hale getirmek için kullanılan *şifre.*
* *sertifikaThumbprint*: Sertifikanın parmak izi. Bu değeri PowerShell'den alırsanız (örneğin, *$certificate. *Önceki kod parçacığından parmak izi), değeri olduğu gibi kullanabilirsiniz. Değeri Windows sertifikası iletişim kutusundan kopyalarsanız, gereksiz alanları söktüğünü unutmayın. *SertifikaThumbprint* AF3143EB61D43F6727842115BB7F17BBCECAEE gibi bir şey görünmelidir.
* *certificateName*: Sertifikayı kimlik olarak kullanarak kendi seçtiğiniz dostane bir dize tanımlayıcısıdır. Ad, TLS/SSL sertifikasını temsil eden *Microsoft.Web/sertifikalar* tüzel kişiliğinin benzersiz Kaynak Yöneticisi tanımlayıcısının bir parçası olarak kullanılır. Ad aşağıdaki sonek ile *bitmelidir:* \_yourASENameHere_InternalLoadBalancingASE. Azure portalı bu sonek, sertifikanın ILB özellikli bir ASE'yi güvence altına almak için kullanıldığını gösteren bir gösterge olarak kullanır.

*Azuredeploy.parameters.json'un* kısaltılmış bir örneği burada gösterilmiştir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "appServiceEnvironmentName": {
      "value": "yourASENameHere"
    },
    "existingAseLocation": {
      "value": "East US 2"
    },
    "pfxBlobString": {
      "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
    },
    "password": {
      "value": "PASSWORDGOESHERE"
    },
    "certificateThumbprint": {
      "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
    },
    "certificateName": {
      "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
    }
  }
}
```

*azuredeploy.parameters.json* dosyası doldurulduktan sonra, PowerShell kodu snippet'ini kullanarak varsayılan TLS/SSL sertifikasını yapılandırın. Dosya yollarını, Kaynak Yöneticisi şablon dosyalarının makinenizde bulunduğu yerle eşleşecek şekilde değiştirin. Kaynak Yöneticisi dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamayı unutmayın:

```powershell
$templatePath="PATH\azuredeploy.json"
$parameterPath="PATH\azuredeploy.parameters.json"

New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath
```

Bu değişikliği uygulamak için ASE ön uç başına yaklaşık 40 dakika sürer. Örneğin, iki ön uç kullanan varsayılan boyutlu bir ASE için şablonun tamamlanması yaklaşık bir saat 20 dakika sürer. Şablon çalışırken, ASE ölçeklendiremez.  

Şablon bittikten sonra, ILB ASE'deki uygulamalara HTTPS üzerinden erişilebilir. Bağlantılar varsayılan TLS/SSL sertifikası kullanılarak güvence altına alınır. Varsayılan TLS/SSL sertifikası, ILB ASE'deki uygulamalar uygulama adı artı varsayılan ana bilgisayar adının birleşimi kullanılarak ele alınınca kullanılır. Örneğin, `https://mycustomapp.internal-contoso.com` **.internal-contoso.com*için varsayılan TLS/SSL sertifikasını kullanır.

Ancak, genel çok kiracılı hizmette çalışan uygulamalar gibi, geliştiriciler de tek tek uygulamalar için özel ana bilgisayar adlarını yapılandırabilir. Ayrıca, tek tek uygulamalar için benzersiz SNI TLS/SSL sertifika bağlamalarını da yapılandırabilirler.

## <a name="app-service-environment-v1"></a>App Service Ortamı v1 ##
App Service Ortamının iki sürümü vardır: ASEv1 ve ASEv2. Yukarıdaki bilgiler ASEv2’yi temel alır. Bu bölümde ASEv1 ile ASEv2 arasındaki farklar gösterilmektedir.

ASEv1'de tüm kaynakları el ile yönetirsiniz. Buna ön uçlar, çalışanlar ve IP tabanlı SSL için kullanılan IP adresleri dahildir. Uygulama Hizmeti planınızı ölçeklendirmeden önce, barındırmak istediğiniz işçi havuzunu ölçeklendirmeniz gerekir.

ASEv1, ASEv2’den farklı bir fiyatlandırma modeli kullanır. ASEv1’de ayrılmış her vCPU için ücret ödersiniz. Buna, ön uçlar veya iş yükü barındırmayan işçiler için kullanılan vCPUs'lar dahildir. ASEv1’de bir ASE’nin varsayılan en büyük ölçek boyutu toplam 55 konaktır. Buna çalışanlar ve ön uçlar dahildir. ASEv1’in bir avantajı, klasik bir sanal ağa ve bir Resource Manager sanal ağına dağıtılabilmesidir. ASEv1 hakkında daha fazla bilgi için bkz. [App Service Ortamı v1’e giriş][ASEv1Intro].

Kaynak Yöneticisi şablonu kullanarak bir ASEv1 oluşturmak için [bkz.][ILBASEv1Template]


<!--Links-->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: https://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: https://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
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
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../../app-service/configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
