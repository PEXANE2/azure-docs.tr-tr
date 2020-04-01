---
title: Sertifika bağlamaları
description: Uygulama Hizmet Ortamında sertifikalarla ilgili çok sayıda konuyu açıklayın. Sertifika bağlamalarının bir ASE'deki tek kiracılı uygulamalarda nasıl çalıştığını öğrenin.
author: ccompy
ms.assetid: 9e21a7e4-2436-4e81-bb05-4a6ba70eeaf7
ms.topic: article
ms.date: 08/29/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: ba1d06ce83d50b6f0db84d1e423e66eae98f665d
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80477505"
---
# <a name="certificates-and-the-app-service-environment"></a>Sertifikalar ve Uygulama Hizmet Ortamı 

Uygulama Hizmeti Ortamı (ASE), Azure Sanal Ağınız (VNet) içinde çalışan Azure Uygulama Hizmeti'nin dağıtımıdır. Internet'e erişilebilen bir uygulama bitiş noktası veya VNet'inizde bulunan bir uygulama bitiş noktasıyla dağıtılabilir. ASE'yi internet erişimine açık bir uç noktasıyla dağıtırsanız, bu dağıtıma Harici ASE denir. ASE'yi VNet'inizde bir bitiş noktasıyla dağıtırsanız, bu dağıtıma ILB ASE denir. ILB ASE hakkında daha fazla bilgi oluşturabilir [ve bir ILB ASE belgesik kullanabilirsiniz.](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase)

ASE tek bir kiracı sistemidir. Tek kiracı olduğundan, yalnızca çok kiracılı Uygulama Hizmetinde bulunmayan bir ASE ile kullanılabilen bazı özellikler vardır. 

## <a name="ilb-ase-certificates"></a>ILB ASE sertifikaları 

Harici ASE kullanıyorsanız, uygulamalarınız [appname] adresinden ulaşılır. [asename].p.azurewebsites.net. Varsayılan olarak tüm ASE'ler, hatta ILB'ler bile bu biçimi izleyen sertifikalarla oluşturulur. Bir ILB ASE'niz olduğunda, uygulamalara ILB ASE oluştururken belirttiğiniz alan adı temel alınarak erişilir. Uygulamaların TLS'yi desteklemesi için sertifika yüklemeniz gerekir. Dahili sertifika yetkililerini kullanarak, harici bir kuruluştan sertifika satın alarak veya kendi imzalanmış bir sertifika kullanarak geçerli bir TLS/SSL sertifikası edinin. 

ILB ASE'niz ile sertifikaları yapılandırmak için iki seçenek vardır.  ILB ASE için joker karakter varsayılan sertifikası ayarlayabilir veya ASE'deki tek tek web uygulamalarında sertifika ayarlayabilirsiniz.  Yaptığınız seçimden bağımsız olarak, aşağıdaki sertifika öznitelikleri nin düzgün şekilde yapılandırılması gerekir:

- **Konu:** Bu öznitelik *olarak ayarlanmalıdır. [your-root-etki alanı-burada] bir joker ILB ASE sertifikası için. Uygulamanız için sertifika oluşturuyorsanız, bu sertifika [appname] olmalıdır. [your-root-etki alanı-burada]
- **Konu Alternatif Adı:** Bu öznitelik her ikisini de içermelidir *. [your-root-etki alanı-burada] ve *.scm. Joker ILB ASE sertifikası için [root-etki alanınız-burada] Uygulamanız için sertifika oluşturuyorsanız, bu sertifika [appname] olmalıdır. [your-root-etki alanı-burada] ve [appname].scm. [sizin-kök-etki alanı-burada].

Üçüncü bir varyant olarak, joker karakter başvurusu kullanmak yerine, sertifikanın SAN'ındaki tüm bireysel uygulama adlarınızı içeren bir ILB ASE sertifikası oluşturabilirsiniz. Bu yöntemdeki sorun, ASE'ye koyduğunuz uygulamaların adlarını önceden bilmeniz veya ILB ASE sertifikasını güncellemeye devam etmeniz gerektiğidir.

### <a name="upload-certificate-to-ilb-ase"></a>Sertifikayı ILB ASE'ye yükleyin 

Portalda bir ILB ASE oluşturulduktan sonra, sertifika ILB ASE için ayarlanmalıdır. Sertifika ayarlanana kadar ASE, sertifikanın ayarlanmadığını belirten bir afiş gösterir.  

Yüklediğiniz sertifika bir .pfx dosyası olmalıdır. Sertifika yüklendikten sonra ASE, sertifikayı ayarlamak için bir ölçek işlemi gerçekleştirir. 

ASE'yi oluşturup sertifikayı portalda tek bir eylem olarak, hatta tek bir şablonda yükleyemezsiniz. Ayrı bir eylem olarak, şablon [belgesinden ASE Oluştur'da](./create-from-template.md) açıklandığı gibi bir şablon kullanarak sertifikayı yükleyebilirsiniz.  

Test için hızlı bir şekilde kendi imzalı bir sertifika oluşturmak istiyorsanız, aşağıdaki PowerShell parçasını kullanabilirsiniz:

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
Kendi imzalı bir sertifika oluştururken, özne adının CN={ASE_NAME_HERE}_InternalLoadBalancingASE biçimine sahip olduğundan emin olmanız gerekir.

## <a name="application-certificates"></a>Başvuru sertifikaları 

BIR ASE'de barındırılan uygulamalar, çok kiracılı Uygulama Hizmeti'nde bulunan uygulama merkezli sertifika özelliklerini kullanabilir. Bu özellikler şunlardır:  

- SNI sertifikaları 
- IP tabanlı SSL, yalnızca harici bir ASE ile desteklenir.  ILB ASE, IP tabanlı SSL'yi desteklemez.
- KeyVault sertifikaları barındırılan 

Bu sertifikaları yükleme ve yönetme yönergeleri, [Azure Uygulama Hizmeti'nde TLS/SSL sertifikası ekle'de](../configure-ssl-certificate.md)kullanılabilir.  Sertifikaları yalnızca web uygulamanıza atadığınız özel bir alan adı yla eşleşecek şekilde yapılandırıyorsanız, bu talimatlar yeterli olacaktır. Varsayılan alan adı içeren bir ILB ASE web uygulamasının sertifikasını yüklüyorsanız, sertifikanın SAN'ındaki scm sitesini daha önce belirtildiği gibi belirtin. 

## <a name="tls-settings"></a>TLS ayarları 

TLS ayarını uygulama düzeyinde yapılandırabilirsiniz.  

## <a name="private-client-certificate"></a>Özel müşteri sertifikası 

Yaygın kullanım örneği, uygulamanızı istemci-sunucu modelinde istemci olarak yapılandırmaktır. Sunucunuzu özel bir CA sertifikasıyla güvence ye tirseniz, istemci sertifikasını uygulamanıza yüklemeniz gerekir.  Aşağıdaki talimatlar, uygulamanızın çalışmakta olduğu çalışanların güven deposuna sertifikaları yükler. Sertifikayı bir uygulamaya yüklerseniz, sertifikayı yeniden yüklemeden aynı Uygulama Hizmeti planında diğer uygulamalarınız ile birlikte kullanabilirsiniz.

Sertifikayı ASE'nizde uygulamanız için yüklemek için:

1. Sertifikanız için bir *.cer* dosyası oluşturun. 
2. Azure portalında sertifikaya ihtiyaç olan uygulamaya gidin
3. Uygulamadaki SSL ayarlarına gidin. Sertifikayı Karşıya Yükle’ye tıklayın. Genel'i seçin. Yerel Makine'yi seçin. Bir ad sağlayın. *.cer* dosyanıza göz atın ve seçin. Yükle'yi seçin. 
4. Parmak izini kopyala.
5. Uygulama Ayarları'na gidin. Değer olarak parmak izi ile bir Uygulama Ayarı WEBSITE_LOAD_ROOT_CERTIFICATES oluşturun. Birden çok sertifikanız varsa, bunları virgülle ayrılmış aynı ayara koyabilirsiniz ve 

    84EC242A4EC7957817B8E48913E50953552DAFA6,6A5C65DC9247F762FE17BF8D4906E04FE6B31819

Sertifika, bu ayarı yapılandıran uygulamayla aynı uygulama hizmet planındaki tüm uygulamalar tarafından kullanılabilir. Farklı bir Uygulama Hizmeti planındaki uygulamalar için kullanılabilir olması gerekiyorsa, Uygulama Hizmeti planındaki bir uygulamada Uygulama Ayarı işlemini yinelemeniz gerekir. Sertifikanın ayarlanıp ayarlı Kudu konsoluna gidin ve PowerShell hata ayıklama konsolunda aşağıdaki komutu girin:

    dir cert:\localmachine\root

Sınama gerçekleştirmek için, kendi imzalı bir sertifika oluşturabilir ve aşağıdaki PowerShell içeren bir *.cer* dosyası oluşturabilirsiniz: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.cer"
    export-certificate -Cert $certThumbprint -FilePath $fileName -Type CERT

