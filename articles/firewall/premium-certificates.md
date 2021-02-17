---
title: Azure Güvenlik Duvarı Premium önizleme sertifikaları
description: Azure Güvenlik Duvarı Premium önizlemesinde TLS incelemesini doğru şekilde yapılandırmak için ara CA sertifikalarını yapılandırmanız ve kurmanız gerekir.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/16/2021
ms.author: victorh
ms.openlocfilehash: 3914a82903c293cf1a8306b5ecc1f542fef83e72
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549914"
---
# <a name="azure-firewall-premium-preview-certificates"></a>Azure Güvenlik Duvarı Premium önizleme sertifikaları 

> [!IMPORTANT]
> Azure Güvenlik Duvarı Premium Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 Azure Güvenlik Duvarı Premium önizleme TLS incelemesini doğru şekilde yapılandırmak için, geçerli bir ara CA sertifikası sağlamalı ve Azure Anahtar Kasası 'na yatırmalısınız.

## <a name="certificates-used-by-azure-firewall-premium-preview"></a>Azure Güvenlik Duvarı Premium önizleme tarafından kullanılan sertifikalar

Tipik bir dağıtımda kullanılan üç tür sertifika vardır:

- **Ara CA sertifikası (CA sertifikası)**

   Sertifika yetkilisi (CA), dijital sertifikaları imzalamak için güvenilen bir kuruluştur. Bir CA, bir şirketin veya bir sertifika isteyen kişinin kimliğini ve yasallığını doğrular. Doğrulama başarılı olursa, CA imzalı bir sertifika verir. Sunucu, bir SSL/TLS anlaşması sırasında sertifikayı istemciye (örneğin, Web tarayıcınız) sunduğunda, istemci imzayı *bilinen iyi* imzalayanların bir listesine karşı doğrulamaya çalışır. Web tarayıcıları normalde ana bilgisayarları tanımlamak için örtülü olarak güvendikleri CA listeleriyle birlikte gelir. Yetkili, kendi sertifikalarını imzalayan bazı sitelerde olduğu gibi listede değilse, tarayıcıda kullanıcının sertifikayı tanınan bir yetkili tarafından imzalanmadığını ve doğrulanmamış siteyle iletişime devam etmek isteyip istemediğini sorar.

- **Sunucu sertifikası (Web sitesi sertifikası)**

   Belirli bir etki alanı adıyla ilişkili bir sertifika. Bir Web sitesinin geçerli bir sertifikası varsa, bu, bir sertifika yetkilisinin, Web adresinin gerçekten o kuruluşa ait olduğunu doğrulamak için adımlar gerçekleştirdiğini gösterir. Bir URL yazdığınızda veya güvenli bir Web sitesinin bağlantısını izlediğinizde, tarayıcınız sertifikayı aşağıdaki özelliklerle denetler:
   - Web sitesi adresi, sertifikadaki adresle eşleşiyor.
   - Sertifika, tarayıcının *güvenilir* bir yetkili olarak tanıdığı bir sertifika yetkilisi tarafından imzalanır.
   
   Bazen kullanıcılar güvenilmeyen sertifikayla bir sunucuya bağlanabilir. Sunucu bağlantıyı sonlandırdığından Azure Güvenlik duvarı bağlantıyı bırakacak.

- **Kök CA sertifikası (kök sertifika)**

   Bir sertifika yetkilisi, ağaç yapısı biçiminde birden çok sertifika verebilir. Kök sertifika, ağacın en yüksek sertifikasıdır.

Azure Güvenlik Duvarı Premium önizleme, giden HTTP/S trafiğini ve için bir sunucu sertifikasını otomatik olarak oluşturmayı yakalayabilir `www.website.com` . Bu sertifika, sağladığınız ara CA sertifikası kullanılarak oluşturulur. Son Kullanıcı tarayıcısı ve istemci uygulamaları, bu yordamın çalışması için kuruluşunuzun kök CA sertifikasına veya ara CA sertifikasına güvenmelidir. 

:::image type="content" source="media/premium-certificates/certificate-process.png" alt-text="Sertifika işlemi":::

## <a name="intermediate-ca-certificate-requirements"></a>Ara CA sertifika gereksinimleri

CA sertifikanızın aşağıdaki gereksinimlerle uyumlu olduğundan emin olun:

- Key Vault gizli dizi olarak dağıtıldığında, bir sertifika ve özel anahtarla parola kullanmayan PFX (PKCS12) kullanmanız gerekir.

- Tek bir sertifika olmalıdır ve tüm sertifika zincirini içermemelidir.  

- Bir yıllık ileri için geçerli olmalıdır.  

- En az 4096 bayt boyutunda bir RSA özel anahtarı olmalıdır.  

- `KeyUsage`Uzantının, `KeyCertSign` BAYRAĞıN (RFC 5280; 4.2.1.3 Key Usage) kritik olarak işaretlenmiş olması gerekir.

- `BasicContraints`Uzantının kritik (RFC 5280; 4.2.1.9 Basic kısıtlamaları) olarak işaretlenmiş olması gerekir.  

- `CA`Bayrak true olarak ayarlanmalıdır.

- Yolun uzunluğu bire büyük veya buna eşit olmalıdır.

## <a name="azure-key-vault"></a>Azure Key Vault

[Azure Key Vault](../key-vault/general/overview.md) , gizli dizileri, anahtarları ve TLS/SSL sertifikalarını korumak için kullanabileceğiniz, platform tarafından yönetilen bir gizli depodır. Azure Güvenlik Duvarı Premium, bir güvenlik duvarı Ilkesine eklenen sunucu sertifikaları için Key Vault tümleştirmeyi destekler.
 
Anahtar kasanızı yapılandırmak için:

- Anahtar çiftine sahip mevcut bir sertifikayı anahtar kasanıza aktarmanız gerekir. 
- Alternatif olarak, parola daha az, temel 64 kodlamalı PFX dosyası olarak depolanan bir Anahtar Kasası gizli anahtarını da kullanabilirsiniz.  PFX dosyası, hem özel anahtar hem de ortak anahtar içeren dijital bir sertifikadır.
- Sertifika sona erme tarihini temel alarak bir uyarı yapılandırmanıza izin verdiğinden, CA sertifikası içeri aktarmanın kullanılması önerilir.
- Bir sertifikayı veya parolayı içeri aktardıktan sonra, kimliğe sertifikaya/parolaya erişim izni verilmesini sağlamak için anahtar kasasında erişim ilkeleri tanımlamanız gerekir.
- Azure iş yükünüz tarafından, belirtilen CA sertifikasının güvenilir olması gerekir. Doğru şekilde dağıtıldığından emin olun.

Azure Güvenlik duvarının sizin adınıza Key Vault sertifikaları almak için kullandığı, Kullanıcı tarafından atanan mevcut bir yönetilen kimliği oluşturabilir veya yeniden kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md) 

## <a name="configure-a-certificate-in-your-policy"></a>İlkenizde bir sertifika yapılandırma

Güvenlik Duvarı Premium ilkenizde bir CA sertifikası yapılandırmak için ilkenizi seçin ve ardından **TLS incelemesi (Önizleme)** öğesini seçin. **TLS İnceleme** sayfasında **etkin** ' i seçin. Ardından, aşağıdaki şekilde gösterildiği gibi Azure Key Vault CA sertifikanızı seçin:

:::image type="content" source="media/premium-certificates/tls-inspection.png" alt-text="Azure Güvenlik Duvarı Premium genel bakış Diyagramı":::
 
> [!IMPORTANT]
> Azure portal bir sertifikayı görmek ve yapılandırmak için, Azure kullanıcı hesabınızı Key Vault erişim ilkesine eklemeniz gerekir. **Gizli izinler** altında Kullanıcı hesabınıza **Al** ve **Listele** ' ye izin verin.
   :::image type="content" source="media/premium-certificates/secret-permissions.png" alt-text="Azure Key Vault erişim ilkesi":::


## <a name="troubleshooting"></a>Sorun giderme

CA sertifikanız geçerliyse, ancak TLS incelemesi altındaki FQDN 'Lere veya URL 'Lere erişemiyorsanız, aşağıdaki öğeleri kontrol edin:

- Web sunucusu sertifikasının geçerli olduğundan emin olun.  

- Kök CA sertifikasının istemci işletim sisteminde yüklü olduğundan emin olun.  

- Tarayıcının veya HTTPS istemcisinin geçerli bir kök sertifika içerdiğinden emin olun. Firefox ve diğer tarayıcıların bazıları özel sertifika ilkelerine sahip olabilir.  

- Uygulama kuralınızın URL hedef türünün doğru yolu ve hedef HTML sayfasına katıştırılmış diğer köprüleri kapsadığından emin olun. Tüm gerekli URL yolunun kolay kapsamı için joker karakterler kullanabilirsiniz.  


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Premium özellikleri hakkında daha fazla bilgi edinin](premium-features.md)
