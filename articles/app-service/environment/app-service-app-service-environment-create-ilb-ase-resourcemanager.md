---
title: ILB ASE v1 oluşturma
description: Dahili yük dengeleyici (ILB ASE) ile bir Uygulama Hizmeti ortamı oluşturun. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 1a0ec9465be3b714e90bfca6a15b60423d6065a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295573"
---
# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Azure Resource Manager Şablonlarını kullanarak ILB ASE oluşturma

> [!NOTE] 
> Bu makale, App Service Environment v1 hakkındadır. Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.
>

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Uygulama Hizmet Ortamları, herkese açık bir VIP yerine sanal ağ dahili adresiyle oluşturulabilir.  Bu iç adres, dahili yük dengeleyicisi (ILB) adı verilen bir Azure bileşeni tarafından sağlanır.  Azure portalı kullanılarak bir ILB ASE oluşturulabilir.  Azure Kaynak Yöneticisi şablonları ile otomasyon kullanılarak da oluşturulabilir.  Bu makale, Azure Kaynak Yöneticisi şablonları ile bir ILB ASE oluşturmak için gereken adımları ve sözdizimini gözden geçirir.

Bir ILB ASE'nin oluşturulmasını otomatikleştirmek için üç adım vardır:

1. İlk temel ASE bir sanal ağ da bir iç yük dengeleyici adresi yerine bir kamu VIP kullanılarak oluşturulur.  Bu adımın bir parçası olarak, ILB ASE'ye bir kök etki alanı adı atanır.
2. ILB ASE oluşturulduktan sonra bir SSL sertifikası yüklenir.  
3. Yüklenen SSL sertifikası açıkça ILB ASE'ye "varsayılan" SSL sertifikası olarak atanır.  Bu SSL sertifikası, uygulamalar ASE'ye atanan ortak kök etki alanı kullanılarak ele alındığında ILB ASE'deki uygulamalara SSL trafiği için kullanılacaktır (örn. `https://someapp.mycustomrootcomain.com`)

## <a name="creating-the-base-ilb-ase"></a>Temel ILB ASE oluşturma
Örnek bir Azure Kaynak Yöneticisi şablonu ve ilişkili parametreler dosyası [GitHub'da bulunmaktadır.][quickstartilbasecreate]

*azuredeploy.parameters.json* dosyasındaki parametrelerin çoğu, hem ILB'ler hem de genel VIP'ye bağlı AS'ler oluşturmak için yaygındır.  Aşağıdaki liste, bir ILB ASE oluştururken özel not parametrelerini veya benzersiz parametreleri çağırır:

* *internalLoadBalancingMode*: Çoğu durumda bu 3 olarak ayarlayın, hangi bağlantı noktaları 80/443 hem HTTP / HTTPS trafik anlamına gelir, ve kontrol / veri kanalı bağlantı noktaları ASE FTP hizmeti tarafından dinlenir, bir ILB tahsis sanal ağ iç adresine bağlı olacaktır.  Bu özellik bunun yerine 2 olarak ayarlanırsa, yalnızca FTP hizmetiyle ilgili bağlantı noktaları (hem denetim hem de veri kanalları) Bir ILB adresine bağlanır, HTTP/HTTPS trafiği ise genel VIP'te kalır.
* *dnsSuffix*: Bu parametre, ASE'ye atanacak varsayılan kök etki alanını tanımlar.  Azure Uygulama Hizmeti'nin genel varyasyonunda, tüm web uygulamaları için varsayılan kök etki alanı *azurewebsites.net.*  Ancak Bir ILB ASE müşterinin sanal ağına dahil olduğundan, kamu hizmetinin varsayılan kök etki alanını kullanmak mantıklı değildir.  Bunun yerine, Bir ILB ASE bir şirketin iç sanal ağ içinde kullanmak için mantıklı varsayılan bir kök etki alanı olmalıdır.  Örneğin, varsayımsal bir Contoso Corporation, yalnızca çözülebilir ve Contoso'nun sanal ağında erişilebilir olması amaçlanan uygulamalar için varsayılan kök *internal-contoso.com* etki alanı kullanabilir. 
* *ipSslAddressCount*: ILB'lerin yalnızca tek bir ILB adresi olduğundan, bu parametre *azuredeploy.json* dosyasında otomatik olarak 0 değerine varsayılandır.  ILB ASE için açık BIR IP-SSL adresi yoktur ve bu nedenle Bir ILB ASE için IP-SSL adres havuzu sıfır olarak ayarlanmalıdır, aksi takdirde bir sağlama hatası oluşur. 

*Azuredeploy.parameters.json* dosyası ILB ASE için doldurulduğunda, ILB ASE aşağıdaki Powershell kod parçacığı kullanılarak oluşturulabilir.  PATH'ler dosyasını, makinenizde Azure Kaynak Yöneticisi şablon dosyalarının bulunduğu yerle eşleşecek şekilde değiştirin.  Ayrıca Azure Kaynak Yöneticisi dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamayı unutmayın.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Azure Kaynak Yöneticisi şablonu gönderildikten sonra ILB ASE'nin oluşturulması birkaç saat sürer.  Oluşturma tamamlandıktan sonra, ILB ASE, dağıtımı tetikleyen abonelik için App Service Environments listesinde ki PORTAL UX'de gösterecektir.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>"Varsayılan" SSL Sertifikasını Karşıya Yükleme ve Yapılandırma
ILB ASE oluşturulduktan sonra, uygulamalara SSL bağlantıları kurmak için "varsayılan" SSL sertifika kullanımı olarak ASE sertifikası ile ilişkilendirilmelidir.  Varsayımsal Contoso Corporation örneği ile devam, ASE varsayılan DNS soneki *internal-contoso.com*ise, *https://some-random-app.internal-contoso.com* o zaman bir bağlantı **.internal-contoso.com*için geçerli bir SSL sertifikası gerektirir . 

Dahili BIR SSL sertifikası almanın, harici bir verenden sertifika satın almanın ve kendi imzasını taşıyan bir sertifika kullanmanın çeşitli yolları vardır.  SSL sertifikasının kaynağından bağımsız olarak, aşağıdaki sertifika özniteliklerinin doğru şekilde yapılandırılması gerekir:

* *Konu*: Bu öznitelik **.your-root-domain-here.com* olarak ayarlanmalıdır
* *Özne Alternatif Adı*: Bu öznitelik hem **.your-root-domain-here.com*ve **.scm.your-root-domain-here.com*içermelidir.  İkinci girişin nedeni, her uygulamayla ilişkili SCM/Kudu sitesine SSL bağlantılarının *your-app-name.scm.your-root-domain-here.com*formun adresi kullanılarak yapılmasıdır.

Elinizde geçerli bir SSL sertifikası ile iki ek hazırlık adımı gereklidir.  SSL sertifikasının .pfx dosyası olarak dönüştürülmesi/kaydedilmesi gerekir.  .pfx dosyasının tüm ara ve kök sertifikaları içermesi ve ayrıca parolayla güvenli hale alınması gerektiğini unutmayın.

Ardından ortaya çıkan .pfx dosyasının base64 dizesine dönüştürülmesi gerekir, çünkü SSL sertifikası Azure Kaynak Yöneticisi şablonu kullanılarak yüklenir.  Azure Kaynak Yöneticisi şablonları metin dosyaları olduğundan, şablonun parametresi olarak eklenebilmesi için .pfx dosyasının base64 dizesine dönüştürülmesi gerekir.

Aşağıdaki Powershell kod parçacığı, kendi imzalı bir sertifika oluşturma, sertifikayı .pfx dosyası olarak dışa aktarma, .pfx dosyasını base64 kodlanmış bir dize haline dönüştürme ve ardından base64 kodlanmış dizeyi ayrı bir dosyaya kaydetme örneğini gösterir.  Base64 kodlama için Powershell kodu [Powershell Scripts blog][examplebase64encoding]uyarlanmıştır.

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

SSL sertifikası başarıyla oluşturulduktan ve base64 kodlanmış bir dize dönüştürüldükten sonra, [varsayılan SSL sertifikasını yapılandırmak][configuringDefaultSSLCertificate] için GitHub'daki Azure Kaynak Yöneticisi şablonu örneği kullanılabilir.

*azuredeploy.parameters.json* dosyasındaki parametreler aşağıda listelenmiştir:

* *appServiceEnvironmentName*: ILB ASE'nin adı yapılandırıldı.
* *existingAseLocation*: ILB ASE'nin dağıtıldığı Azure bölgesini içeren metin dizesi.  Örneğin: "Güney Orta ABD".
* *pfxBlobString*: .pfx dosyasının based64 kodlanmış dize gösterimi.  Daha önce gösterilen kod parçacıklarını kullanarak, "exportedcert.pfx.b64" de yer alan dizeyi kopyalar ve *pfxBlobString* özniteliğinin değeri olarak yapıştırırsınız.
* şifre : .pfx dosyasını güvenli hale getirmek için kullanılan *şifre.*
* *sertifikaThumbprint*: Sertifikanın parmak izi.  Bu değeri Powershell'den alırsanız (örn. *$certificate. *Önceki kod parçacığından parmak izi), olduğu gibi değeri kullanabilirsiniz.  Ancak, Değeri Windows sertifikası iletişim kutusundan kopyalarsanız, gereksiz alanları söktüğünü unutmayın.  *SertifikaThumbprint* gibi bir şey görünmelidir: AF3143EB61D43F6727842115BB7F17BBCECAEE
* *certificateName*: Sertifikayı kimlik olarak kullanarak kendi seçtiğiniz dostane bir dize tanımlayıcısıdır.  Ad, SSL sertifikasını temsil eden *Microsoft.Web/sertifikalar* tüzel kişiliğinin benzersiz Azure Kaynak Yöneticisi tanımlayıcısının bir parçası olarak kullanılır.  Ad aşağıdaki sonek ile **bitmelidir:** \_yourASENameHere_InternalLoadBalancingASE.  Bu sonek, portal tarafından sertifikanın ILB özellikli bir ASE'yi güvence altına almak için kullanıldığını gösteren bir gösterge olarak kullanılır.

*Azuredeploy.parameters.json'un* kısaltılmış bir örneği aşağıda gösterilmiştir:

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

*Azuredeploy.parameters.json* dosyası doldurulduktan sonra varsayılan SSL sertifikası aşağıdaki Powershell kod parçacığı kullanılarak yapılandırılabilir.  PATH'ler dosyasını, makinenizde Azure Kaynak Yöneticisi şablon dosyalarının bulunduğu yerle eşleşecek şekilde değiştirin.  Ayrıca Azure Kaynak Yöneticisi dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamayı unutmayın.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Azure Kaynak Yöneticisi şablonu gönderildikten sonra, değişikliği uygulamak ASE ön uç başına yaklaşık kırk dakika sürer.  Örneğin, iki ön uç kullanarak varsayılan boyutlandırılmış BIR ASE ile şablonun tamamlanması yaklaşık bir saat yirmi dakika sürer.  Şablon çalışırken ASE ölçeklendirilemez.  

Şablon tamamlandıktan sonra, ILB ASE'deki uygulamalara HTTPS üzerinden erişilebilir ve bağlantılar varsayılan SSL sertifikası kullanılarak güvenli hale gelir.  Varsayılan SSL sertifikası, ILB ASE'deki uygulamalar uygulama adı artı varsayılan ana bilgisayar adının birleşimi kullanılarak ele alınınca kullanılır.  Örneğin *https://mycustomapp.internal-contoso.com* **.internal-contoso.com*için varsayılan SSL sertifikasını kullanır.

Ancak, tıpkı genel çok kiracılı hizmette çalışan uygulamalar gibi, geliştiriciler de tek tek uygulamalar için özel ana bilgisayar adlarını yapılandırabilir ve ardından tek tek uygulamalar için benzersiz SNI SSL sertifika bağlamalarını yapılandırabilir.  

## <a name="getting-started"></a>Başlarken
Uygulama Hizmet Ortamlarına başlamak için Uygulama [Hizmet Ortamına Giriş](app-service-app-service-environment-intro.md)

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

