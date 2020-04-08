---
title: Bulut Hizmetleri ve yönetim sertifikaları | Microsoft Dokümanlar
description: Microsoft Azure ile sertifika oluşturma ve kullanma hakkında bilgi edinin
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 04/19/2017
ms.author: tagore
ms.openlocfilehash: 173f5c698ab44ea269995665bcbc33c726d4f03a
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811467"
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Azure Cloud Services’da sertifikalara genel bakış
Sertifikalar Azure'da bulut hizmetleri[(hizmet sertifikaları)](#what-are-service-certificates)ve yönetim API'si[(yönetim sertifikaları)](#what-are-management-certificates)ile kimlik doğrulaması için kullanılır. Bu konu, her iki sertifika türüne de genel bir genel bakış, bunları nasıl [oluşturulup](#create) Azure'a dağıtılayabilmek sağlar.

Azure'da kullanılan sertifikalar x.509 v3 sertifikalarıdır ve başka bir güvenilir sertifika tarafından imzalanabilir veya kendi imzalanabilirler. Kendi imzalı bir sertifika kendi oluşturucusu tarafından imzalandığıiçin varsayılan olarak güvenilir değildir. Tarayıcıların çoğu bu sorunu yok sayabilir. Yalnızca bulut hizmetlerinizi geliştirirken ve test ederken kendi imzalı sertifikalar kullanmalısınız. 

Azure tarafından kullanılan sertifikalar özel veya ortak anahtar içerebilir. Sertifikalar, bunları kesin bir şekilde tanımlamak için bir araç sağlayan bir parmak izi vardır. Bu parmak izi, bulut hizmetinin hangi sertifikayı kullanması gerektiğini belirlemek için Azure [yapılandırma dosyasında](cloud-services-configure-ssl-certificate-portal.md) kullanılır. 

>[!Note]
>Azure Bulut Hizmetleri, AES256-SHA256 şifreli sertifikayı kabul etmez.

## <a name="what-are-service-certificates"></a>Hizmet sertifikaları nelerdir?
Hizmet sertifikaları, bulut hizmetlerine iliştirilir ve hizmete ve hizmetten güvenli iletişimi etkinleştirir. Örneğin, bir web rolü dağıttıysanız, açıklanmış bir HTTPS bitiş noktasının kimliğini doğrulayabilen bir sertifika sağlamak isteyebilirsiniz. Hizmet tanımınızda tanımlanan hizmet sertifikaları, rolünüzün bir örneğini çalıştıran sanal makineye otomatik olarak dağıtılır. 

Hizmet sertifikalarını Azure portalı veya klasik dağıtım modelini kullanarak Azure'a yükleyebilirsiniz. Hizmet sertifikaları belirli bir bulut hizmeti ile ilişkilendirilir. Bunlar, hizmet tanımı dosyasındaki bir dağıtıma atanır.

Hizmet sertifikaları hizmetlerinizden ayrı olarak yönetilebilir ve farklı kişiler tarafından yönetilebilir. Örneğin, bir geliştirici BT yöneticisinin daha önce Azure'a yüklediği bir sertifikaya atıfta bulunan bir hizmet paketi yükleyebilir. BT yöneticisi, yeni bir hizmet paketini karşıya yüklemeye gerek kalmadan bu sertifikayı yönetebilir ve yenileyebilir (hizmetin yapılandırmasını değiştirerek). Sertifikanın mantıksal adı, deposu adı ve konumu hizmet tanım dosyasında olduğundan ve sertifika parmak izi hizmet yapılandırma dosyasında belirtildiğinden, yeni bir hizmet paketi olmadan güncelleştirme mümkündür. Sertifikayı güncelleştirmek için yalnızca yeni bir sertifika yüklemeniz ve hizmet yapılandırma dosyasındaki parmak izi değerini değiştirmeniz gerekir.

>[!Note]
>[Bulut Hizmetleri SSS - Yapılandırma ve Yönetim](cloud-services-configuration-and-management-faq.md) makalesinde sertifikalar hakkında bazı yararlı bilgiler vardır.

## <a name="what-are-management-certificates"></a>Yönetim sertifikaları nelerdir?
Yönetim sertifikaları, klasik dağıtım modeliyle kimlik doğrulaması yapmanıza izin verir. Birçok program ve araç (Visual Studio veya Azure SDK gibi), çeşitli Azure hizmetlerinin yapılandırılmasını ve dağıtımını otomatik hale getirmek için bu sertifikaları kullanır. Bunlar gerçekten bulut hizmetleri ile ilgili değildir. 

> [!WARNING]
> Dikkat et! Bu tür sertifikalar, kimlik doğrulaması yapan herkesin ilişkili oldukları aboneliği yönetmesine olanak sağlar. 
> 
> 

### <a name="limitations"></a>Sınırlamalar
Abonelik başına 100 yönetim sertifikası sınırı vardır. Ayrıca, belirli bir hizmet yöneticisinin kullanıcı kimliği altında tüm abonelikler için 100 yönetim sertifikası sınırı vardır. Hesap yöneticisinin kullanıcı kimliği zaten 100 yönetim sertifikası eklemek için kullanılmışsa ve daha fazla sertifikaya ihtiyaç varsa, ek sertifikalar eklemek için bir yardımcı yönetici ekleyebilirsiniz. 

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Yeni bir kendi imzalı sertifika oluşturma
Bu ayarlara uydukları sürece kendi imzalı sertifika oluşturmak için kullanılabilir herhangi bir aracı kullanabilirsiniz:

* X.509 sertifikası.
* Özel bir anahtar içerir.
* Anahtar değişimi (.pfx dosyası) için oluşturulmuştur.
* Konu adı, bulut hizmetine erişmek için kullanılan etki alanıyla eşleşmelidir.

    > cloudapp.net (veya Azure ile ilgili herhangi bir alan adı için) TLS/SSL sertifikası alamazsınız; sertifikanın özne adı, başvurunuza erişmek için kullanılan özel alan adı ile eşleşmelidir. Örneğin, **contoso.net,** **contoso.cloudapp.net**değil.

* En az 2048 bit şifreleme.
* **Yalnızca Hizmet Sertifikası**: İstemci tarafındaki sertifika *Kişisel* sertifika deposunda yer almalıdır.

Windows'da yardımcı program veya IIS ile `makecert.exe` birlikte bir sertifika oluşturmanın iki kolay yolu vardır.

### <a name="makecertexe"></a>Makecert.exe
Bu yardımcı program amortismana uğradı ve artık burada belgelendirildi. Daha fazla bilgi için [bu MSDN makalesine](/windows/desktop/SecCrypto/makecert)bakın.

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 2048 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Sertifikayı etki alanı yerine IP adresiyle kullanmak istiyorsanız, -DnsName parametresinde IP adresini kullanın.


Bu [sertifikayı yönetim portalı ile](../azure-api-management-certs.md)kullanmak istiyorsanız **,.cer** dosyasına aktarın:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)
IIS ile bunu nasıl kapsayacak internet üzerinde birçok sayfa vardır. [Burada](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) ben de açıklar düşünüyorum büyük bir tanesidir. 

### <a name="linux"></a>Linux
[Bu makalede,](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) SSH ile sertifikaoluşturma nasıl açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
[Hizmet sertifikanızı Azure portalına yükleyin.](cloud-services-configure-ssl-certificate-portal.md)

Azure portalına bir [yönetim API sertifikası](../azure-api-management-certs.md) yükleyin.




