---
title: Azure AD Application Proxy'de özel etki alanları | Microsoft Dokümanlar
description: Azure AD Application Proxy'de özel etki alanlarını yapılandırın ve yönetin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f35658a75adb4d4c6c279e45087e741b8117e65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481390"
---
# <a name="configure-custom-domains-with-azure-ad-application-proxy"></a>Azure AD Application Proxy ile özel etki alanlarını yapılandırma

Azure Active Directory Application Proxy aracılığıyla bir uygulama yayımladığınızda, kullanıcılarınız için harici bir URL oluşturursunuz. Bu URL varsayılan *etki*yourtenant.msappproxy.net alır. Örneğin, *Contoso*adlı kiracınızda *Giderler* adlı bir uygulama yayınlarsanız, harici URL *\/https' dir: /expenses-contoso.msappproxy.net*. *msappproxy.net*yerine kendi etki alanı adınızı kullanmak istiyorsanız, uygulamanız için özel bir etki alanı yapılandırabilirsiniz. 

## <a name="benefits-of-custom-domains"></a>Özel etki alanlarının avantajları

Mümkün olduğunda uygulamalarınız için özel etki alanları ayarlamak iyi bir fikirdir. Özel etki alanlarını kullanmak için bazı nedenler şunlardır:

- Uygulamalar arasındaki bağlantılar şirket ağı dışında bile çalışır. Özel bir etki alanı olmadan, uygulamanız Uygulama Proxy'si dışındaki hedeflere sabit kodlanmış dahili bağlantılara sahipse ve bağlantılar dışarıdan çözülebilir değilse, bunlar kırılır. Dahili ve harici URL'leriniz aynı olduğunda, bu sorunu önlersiniz. Özel etki alanlarını kullanamıyorsanız, bu sorunu gidermenin başka yolları için [Azure AD Application Proxy ile yayınlanan uygulamalar için yeniden yönlendir kodlu bağlantılara](../application-proxy-link-translation.md) bakın. 
  
- Kullanıcılarınızın daha kolay bir deneyimi olur, çünkü uygulamaiçin ağınızın içinden veya dışından aynı URL ile ulaşabilirsiniz. Farklı iç ve dış URL'leri öğrenmeleri veya geçerli konumlarını izlemeleri gerekmez. 

- Markanızı kontrol edebilir ve istediğiniz URL'leri oluşturabilirsiniz. Kullanıcılar *msappproxy.net*yerine tanıdık bir ad görüp kullandığından, özel bir etki alanı kullanıcılarınızın güvenini oluşturmanıza yardımcı olabilir.

- Bazı yapılandırmalar yalnızca özel etki alanlarıyla çalışır. Örneğin, Active Directory Federation Services (AD FS) kullanıyorsanız ancak WS-Federation'ı kullanamadığınız zamanlar gibi Güvenlik İddiası Biçimlendirme Dili (SAML) kullanan uygulamalar için özel etki alanları na ihtiyacınız vardır. Daha fazla bilgi için Uygulama [Proxy'de hak taleplerine duyarlı uygulamalarla Çalışma'ya](application-proxy-configure-for-claims-aware-applications.md)bakın. 

İç ve dış URL'leri eşleştiremiyorsanız, özel etki alanlarını kullanmak o kadar da önemli değildir, ancak yine de diğer avantajlardan yararlanabilirsiniz. 

## <a name="dns-configuration-options"></a>DNS yapılandırma seçenekleri

Gereksinimlerinize bağlı olarak DNS yapılandırmanızı ayarlamak için çeşitli seçenekler vardır:

### <a name="same-internal-and-external-url-different-internal-and-external-behavior"></a>Aynı iç ve dış URL, farklı iç ve dış davranış 

Dahili kullanıcılarınızın Uygulama Proxy'si aracılığıyla yönlendirilmesini istemiyorsanız, *bölünmüş beyinli bir DNS*ayarlayabilirsiniz. Bölünmüş Bir DNS altyapısı, ad çözümlemesi için dahili ana bilgisayarları dahili alan adı sunucusuna, dış ana bilgisayarları da ad çözümlemesi için harici bir etki alanı adı sunucusuna yönlendirir. 

![Ayrık beyinli DNS](./media/application-proxy-configure-custom-domain/split-brain-dns.png)

### <a name="different-internal-and-external-urls"></a>Farklı iç ve dış URL'ler 

İç ve dış URL'ler farklıysa, kullanıcı yönlendirmesi URL tarafından belirlendiği için bölünmüş beyin davranışını yapılandırmanız gerekmez. Bu durumda, yalnızca harici DNS'yi değiştirir ve dış URL'yi Uygulama Proxy bitiş noktasına yönlendirirsiniz. 

Harici bir URL için özel bir etki alanı seçtiğinizde, harici Bir URL sağlayıcısına eklemeniz gereken CNAME girişini bir bilgi çubuğu gösterir. Bu bilgileri her zaman uygulamanın Uygulama **proxy** sayfasına giderek görebilirsiniz.

## <a name="set-up-and-use-custom-domains"></a>Özel etki alanlarını ayarlama ve kullanma

Özel bir etki alanı kullanacak şekilde şirket içi bir uygulamayı yapılandırmak için doğrulanmış bir Azure Active Directory özel etki alanı, özel etki alanı için PFX sertifikası ve yapılandırmak için şirket içi bir uygulamaya ihtiyacınız vardır. 

### <a name="create-and-verify-a-custom-domain"></a>Özel bir etki alanı oluşturma ve doğrulama

Özel bir etki alanı oluşturmak ve doğrulamak için:

1. Azure Etkin Dizini'nde, sol gezintide **Özel alan adları** seçin ve ardından Özel etki alanı **ekle'yi**seçin. 
1. Özel alan adınızı girin ve **Etki Alanı Ekle'yi**seçin. 
1. Etki alanı sayfasında, etki alanınız için TXT kayıt bilgilerini kopyalayın. 
1. Etki alanı kayıt şirketinize gidin ve kopyalanan DNS bilgilerinize dayalı olarak etki alanınız için yeni bir TXT kaydı oluşturun.
1. Etki alanını kaydettikten sonra, Azure Etkin Dizini'nde etki alanının sayfasında **Doğrula'yı**seçin. Etki alanı durumu **Doğrulandıktan**sonra, etki alanını Application Proxy de dahil olmak üzere tüm Azure AD yapılandırmalarınızda kullanabilirsiniz. 

Daha ayrıntılı talimatlar için Azure [Active Directory portalını kullanarak özel alan adınızı ekle'ye](../fundamentals/add-custom-domain.md)bakın.

### <a name="configure-an-app-to-use-a-custom-domain"></a>Özel bir etki alanı kullanacak şekilde bir uygulamayı yapılandırma

Uygulamanızı özel bir etki alanıyla Application Proxy üzerinden yayınlamak için:

1. Azure Active Directory'de yeni bir uygulama için sol navigasyonda **Kurumsal uygulamaları** seçin. **Yeni uygulama**’yı seçin. Şirket **içi uygulamalar** bölümünde, **şirket içi uygulama ekle'yi**seçin. 
   
   **Kurumsal uygulamalarda**zaten bulunan bir uygulama için, listeden seçin ve ardından sol navigasyonda **Uygulama proxy'sini** seçin. 

2. Uygulama Proxy ayarları sayfasında, kendi şirket içi uygulamanızı ekliyorsanız bir **Ad** girin.

3.  **Dahili Url** alanına, uygulamanızın dahili URL'sini girin.
   
4. Dış **Url** alanında, listeyi bırakın ve kullanmak istediğiniz özel etki alanını seçin.
   
5. **Ekle'yi**seçin.
   
   ![Özel etki alanı seçin](./media/application-proxy-configure-custom-domain/application-proxy.png)
   
6. Etki alanının zaten bir sertifikası varsa, **Sertifika** alanı sertifika bilgilerini görüntüler. Aksi takdirde, **Sertifika** alanını seçin. 
   
   ![Sertifika yüklemek için tıklayın](./media/application-proxy-configure-custom-domain/certificate.png)
   
7. **SSL sertifika** sayfasında PFX sertifika dosyanıza göz atın ve seçin. Sertifikanın parolasını girin ve **Sertifikayı Yükle'yi**seçin. Sertifikalar hakkında daha fazla bilgi için özel etki alanları için [Sertifikalar](#certificates-for-custom-domains) bölümüne bakın.
   
   ![Sertifika Yükle](./media/application-proxy-configure-custom-domain/ssl-certificate.png)
   
   > [!TIP] 
   > Özel bir etki alanının sertifikasının yalnızca bir kez yüklenmesi gerekir. Bundan sonra, yüklenen sertifika diğer uygulamalar için özel etki alanını kullandığınızda otomatik olarak uygulanır.
   
8. **Uygulama proxy** sayfasına bir sertifika eklediyseniz, **Kaydet'i**seçin. 
   
9. **Uygulama proxy** sayfasındaki bilgi çubuğunda, DNS bölgenize eklemeniz gereken CNAME girişine dikkat edin. 
   
   ![CNAME DNS girişi ekle](./media/application-proxy-configure-custom-domain/dns-info.png)
   
10. Yeni harici URL'yi *msappproxy.net* etki alanına yönlendiren bir DNS kaydı eklemek için [Azure portalını kullanarak DNS kayıtlarını ve kayıt kümelerini yönet'teki](../../dns/dns-operations-recordsets-portal.md) yönergeleri izleyin.
   
11. DNS kaydının doğru şekilde yapılandırıldığından kontrol etmek için, dış URL'nize erişilebildiğini ve *msapproxy.net* etki alanının takma ad olarak göründüğünü doğrulamak için [nslookup](https://social.technet.microsoft.com/wiki/contents/articles/29184.nslookup-for-beginners.aspx) komutunu kullanın.

Uygulamanız artık özel etki alanını kullanacak şekilde ayarlanmıştır. Test etmeden veya yayınlamadan önce kullanıcıları uygulamanız için atadığınızdan emin olun. 

Bir uygulamanın etki alanını değiştirmek için, uygulamanın **Uygulama proxy** sayfasında Harici **URL'deki** açılır listeden farklı bir etki alanı seçin. Gerekirse güncelleştirilmiş etki alanı için bir sertifika yükleyin ve DNS kaydını güncelleştirin. **Harici URL'deki**açılır listede istediğiniz özel etki alanını görmüyorsanız, bu etki doğrulanmamış olabilir.

Uygulama Proxy'si için daha ayrıntılı talimatlar için [Bkz. Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](application-proxy-add-on-premises-application.md)

## <a name="certificates-for-custom-domains"></a>Özel etki alanları için sertifikalar

Sertifika, özel etki alanınız için güvenli TLS bağlantısı oluşturur. 

### <a name="certificate-formats"></a>Sertifika biçimleri

Gerekli tüm ara sertifikaların dahil olduğundan emin olmak için bir PFX sertifikası kullanmanız gerekir. Sertifika özel anahtarı içermelidir.

Sertifika imza yöntemleriüzerinde herhangi bir kısıtlama yoktur. Eliptik Eğri Şifrelemesi (ECC), Konu Alternatif Adı (SAN) ve diğer yaygın sertifika türleri desteklenir. 

Joker karakter, harici URL ile eşleşse joker karakter sertifikalarını kullanabilirsiniz. [Joker karakter uygulamaları](application-proxy-wildcard.md)için joker karakter sertifikaları kullanmanız gerekir. Alt etki alan adlarına erişmek için sertifikayı kullanmak istiyorsanız, alt alan adlarını aynı sertifikada özne alternatif adları olarak eklemeniz gerekir. Örneğin, * \*.adventure-works.com* için bir * \*sertifika.apps.adventure-works.com* için bir konu alternatif adı olarak * \*.apps.adventure-works.com* eklemediğiniz sürece çalışmaz. 

Sertifika zinciri istemci aygıtlarınızda yüklüyse, kendi ortak anahtar altyapınız (PKI) tarafından verilen sertifikaları kullanabilirsiniz. Intune bu sertifikaları yönetilen aygıtlara dağıtabilir. Yönetilmeyen aygıtlar için bu sertifikaları el ile yüklemeniz gerekir. 

Özel kök CA'nın istemci makinelere itilmesi gerektiğinden, özel bir kök CA kullanmanızı önermiyoruz, bu da birçok zorlukla karşılaşabilir.

### <a name="certificate-management"></a>Sertifika yönetimi

Tüm sertifika yönetimi tek tek başvuru sayfaları üzerinden yapılır. **Sertifika** alanına erişmek için uygulamanın **Uygulama proxy** sayfasına gidin.

Aynı sertifikayı birden çok uygulama için kullanabilirsiniz. Yüklenen bir sertifika başka bir uygulamayla çalışırsa, otomatik olarak uygulanır. Uygulamayı eklediğinizde veya yapılandırdığınızda yeniden yüklemeniz istenmez. 

Sertifikanın süresi dolduğunda, başka bir sertifika yüklemenizi söyleyen bir uyarı alırsınız. Sertifika iptal edilirse, kullanıcılarınız uygulamaya erişirken bir güvenlik uyarısı görebilir. Bir uygulamanın sertifikasını güncellemek için uygulamanın **Uygulama proxy** sayfasına gidin, **Sertifika'yı**seçin ve yeni bir sertifika yükleyin. Eski sertifika diğer uygulamalar tarafından kullanılmıyorsa, otomatik olarak silinir. 

## <a name="next-steps"></a>Sonraki adımlar
* Azure AD kimlik doğrulaması ile yayınlanmış uygulamalarınızda tek oturum açma olanağı [nı etkinleştirin.](application-proxy-configure-single-sign-on-with-kcd.md)
* Yayınlanmış uygulamalarınıza [Koşullu Erişim'i etkinleştirin.](../conditional-access/overview.md)

