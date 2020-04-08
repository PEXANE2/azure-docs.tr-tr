---
title: Bulut Hizmetlerinde özel bir etki alanı adı yapılandırma | Microsoft Dokümanlar
description: DNS ayarlarını yapılandırarak Azure uygulamanızı veya verilerinizi özel bir etki alanında internete nasıl maruz bırakabilirsiniz öğrenin.  Bu örnekler Azure portalını kullanır.
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/05/2017
ms.author: tagore
ms.openlocfilehash: e764e6a474b9843d43f9e8af9cf3b6a8ddf37189
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811639"
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Azure bulut hizmeti için özel bir etki alanı adı yapılandırma
Bir Bulut Hizmeti oluşturduğunuzda, Azure bu hizmeti **cloudapp.net**bir alt etki alanına atar. Örneğin, Bulut Hizmetinizin adı "contoso" ise, kullanıcılarınız uygulamanıza `http://contoso.cloudapp.net`. Azure ayrıca sanal bir IP adresi de atar.

Ancak, uygulamanızı **contoso.com**gibi kendi etki alanı adınıza da maruz bırakabilirsiniz. Bu makalede, Bulut Hizmeti web rolleri için özel bir etki alanı adı nasıl rezerve veya yapılandırma açıklanmaktadır.

CNAME ve A kayıtlarının ne olduğunu zaten anlıyor musunuz? [Açıklamanın yanından atla.](#add-a-cname-record-for-your-custom-domain)

> [!NOTE]
> Bu görevdeki yordamlar Azure Bulut Hizmetleri için geçerlidir. Uygulama Hizmetleri için, [Varolan özel bir DNS adını Azure Web Uygulamaları'na eşleyin' e](../app-service/app-service-web-tutorial-custom-domain.md)bakın. Depolama hesapları için bkz. [Azure Blob depolama bitiş noktanız için özel bir etki alanı adı yapılandırın.](../storage/blobs/storage-custom-domain-name.md)
> 
> 

<p/>

> [!TIP]
> Daha hızlı gitmeye alış-Yenİ Azure [kılavuzlu gözden geçirme](https://support.microsoft.com/kb/2990804)yi kullanın!  Azure Bulut Hizmetleri veya Azure Web Siteleri ile özel bir etki alanı adı ilişkilendirme ve iletişimi (TLS) sağlama yı kolaylaştırır.
> 
> 

## <a name="understand-cname-and-a-records"></a>CNAME ve A kayıtlarını anlama
CNAME (veya diğer ad kayıtları) ve A kayıtları, etki alanı adını belirli bir sunucuyla (veya bu durumda hizmetle) ilişkilendirmenize olanak sağlar, ancak bunlar farklı çalışır. Azure Bulut hizmetlerine sahip Bir kaydı kullanırken hangisini kullanacağınıza karar vermeden önce göz önünde bulundurmanız gereken bazı özel noktalar da vardır.

### <a name="cname-or-alias-record"></a>CNAME veya Alias kaydı
CNAME kaydı, **contoso.com** veya **\.www contoso.com**gibi *belirli* bir etki alanını kanonik bir etki alanı adıyla eşler. Bu durumda, kanonik alan adı Azure barındırılan uygulamanızın **[myapp].cloudapp.net** etki alanı adıdır. Oluşturulduktan sonra, CNAME **[myapp].cloudapp.net**için bir takma ad oluşturur. CNAME girişi **[myapp].cloudapp.net** hizmetinip adresine otomatik olarak çözülür, böylece bulut hizmetinin IP adresi değişirse herhangi bir işlem yapmanız gerekmez.

> [!NOTE]
> Bazı etki alanı kayıt şirketleri, alt alan adlarını yalnızca www\.contoso.com gibi bir CNAME kaydı nı kullanırken eşlemenizin yanı contoso.com gibi kök adları değil. CNAME kayıtları hakkında daha fazla bilgi için kayıt şirketiniz tarafından sağlanan belgelere, [CNAME kaydındaki Vikipedi girişine](https://en.wikipedia.org/wiki/CNAME_record)veya [IETF Alan Adları - Uygulama ve Belirtim](https://tools.ietf.org/html/rfc1035) belgesine bakın.

### <a name="a-record"></a>Bir kayıt
*Kayıt,* **contoso.com** veya **www\.contoso.com**gibi bir etki alanını veya IP adresine ** \*.contoso.com**gibi *bir joker alan adı* ile eşler. Azure Bulut Hizmeti durumunda, hizmetin sanal IP'si. Yani bir CNAME kaydı üzerinden bir kaydın ana yararı, **mail.contoso.com**, **login.contoso.com**veya \* **www\.contso.com**gibi birden fazla alt etki alanı için isteklerini işleyeceğini **.contoso.com**gibi bir joker karakter kullanan bir giriş olabilir.

> [!NOTE]
> A kaydı statik bir IP adresine eşlenediğinden, Bulut Hizmetinizin IP adresindeki değişiklikleri otomatik olarak çözemez. Bulut Hizmetiniz tarafından kullanılan IP adresi, boş bir yuvaya ilk dağıtımınızda (üretim veya evreleme) tahsis edilir. Yuva için dağıtımı silerseniz, IP adresi Azure tarafından yayımlanır ve gelecekte yuvaya yapılacak dağıtımlara yeni bir IP adresi verilebilir.
> 
> Uygun olarak, belirli bir dağıtım yuvasının (üretim veya hazırlama) IP adresi, evreleme ve üretim dağıtımları arasında geçiş yaparken veya varolan bir dağıtımın yerinde yükseltmesini gerçekleştirirken kalıcıdır. Bu eylemleri gerçekleştirme hakkında daha fazla bilgi için [bulut hizmetlerini nasıl yönetebilirsiniz'e](cloud-services-how-to-manage-portal.md)bakın.
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Özel etki alanınız için CNAME kaydı ekleme
CNAME kaydı oluşturmak için, kayıt şirketiniz tarafından sağlanan araçları kullanarak özel etki alanınız için DNS tablosuna yeni bir giriş eklemeniz gerekir. Her kayıt şirketi cname kaydı belirtmebenzer ama biraz farklı bir yönteme sahiptir, ancak kavramlar aynıdır.

1. Bulut hizmetinize atanan **.cloudapp.net** alan adını bulmak için bu yöntemlerden birini kullanın.

   * [Azure portalına]giriş yapın, bulut hizmetinizi seçin, **Genel Bakış** bölümüne bakın ve ardından **Site URL** girişini bulun.

       ![site URL'sini gösteren hızlı bakış bölümü][csurl]

       **Veya**
   * [Azure Powershell'i](/powershell/azure/overview)yükleyin ve yapılandırın ve ardından aşağıdaki komutu kullanın:

       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```

     CNAME kaydı oluştururken ihtiyacınız olacağı için, her iki yöntemle de döndürülen URL'de kullanılan alan adını kaydedin.
2. DNS kayıt şirketinizin web sitesinde oturum açın ve DNS'yi yönetmek için sayfaya gidin. **Sitenin Alan Adı**, **DNS**veya Ad Sunucusu **Yönetimi**olarak etiketlenmiş bağlantılarını veya alanlarını arayın.
3. Şimdi CNAME'leri seçebileceğiniz veya girebileceğiniz yeri bulun. Bir açılır yerden kayıt türünü seçmeniz veya gelişmiş ayarlar sayfasına gitmeniz gerekebilir. **CNAME,** **Diğer Ad**veya Alt Etki Alanları **sözcüklerini**aramalısınız.
4. **Ayrıca, www\.** customdomain.com için bir takma ad oluşturmak **istiyorsanız, www** gibi CNAME için etki alanı veya alt etki alanı takma adı da sağlamanız gerekir. Kök etki alanı için bir takma ad oluşturmak istiyorsanız, kayıt**\@** şirketinizin DNS araçlarında ' ' simgesi olarak listelenebilir.
5. Ardından, uygulamanızın bu durumda **cloudapp.net** etki alanı olan bir kanonik ana bilgisayar adı sağlamanız gerekir.

Örneğin, aşağıdaki CNAME **kaydı, wwwcontoso.comden\.** **contoso.cloudapp.net'a,** dağıtılmış uygulamanızın özel alan adı olan tüm trafiği iletir:

| Diğer ad/Ana bilgisayar adı/Alt etki alanı | Kanonik etki alanı |
| --- | --- |
| www |contoso.cloudapp.net |

> [!NOTE]
> **Www\.contoso.com** bir ziyaretçigerçek ana bilgisayar (contoso.cloudapp.net) asla görmez, bu nedenle iletme işlemi son kullanıcı ya da görünmez.
> 
> Yukarıdaki örnek yalnızca **www** alt etki alanındaki trafik için geçerlidir. CNAME kayıtları olan joker karakterleri kullanamayacağınız için, her etki alanı/alt etki alanı için bir CNAME oluşturmanız gerekir. *.contoso.com gibi alt etki alanlarından cloudapp.net adresinize trafiği yönlendirmek istiyorsanız, DNS ayarlarınızda bir **URL Yönlendirme** veya **URL İleri** girişi yapılandırabilir veya Bir Kayıt oluşturabilirsiniz.

## <a name="add-an-a-record-for-your-custom-domain"></a>Özel etki alanınız için A kaydı ekleme
A kaydı oluşturmak için öncelikle bulut hizmetinizin sanal IP adresini bulmanız gerekir. Ardından, kayıt şirketiniz tarafından sağlanan araçları kullanarak özel etki alanınız için DNS tablosuna yeni bir giriş ekleyin. Her kayıt şirketi, A kaydı nı belirtmenin benzer ancak biraz farklı bir yöntemine sahiptir, ancak kavramlar aynıdır.

1. Bulut hizmetinizin IP adresini almak için aşağıdaki yöntemlerden birini kullanın.

   * [Azure portalına]giriş yapın, bulut hizmetinizi seçin, **Genel Bakış** bölümüne bakın ve ardından Genel **IP adresleri** girişini bulun.

       ![VIP gösteren hızlı bakış bölümü][vip]

       **Veya**
   * [Azure Powershell'i](/powershell/azure/overview)yükleyin ve yapılandırın ve ardından aşağıdaki komutu kullanın:

       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```

     A kaydı oluştururken ihtiyacınız olacağı gibi IP adresini kaydedin.
2. DNS kayıt şirketinizin web sitesinde oturum açın ve DNS'yi yönetmek için sayfaya gidin. **Sitenin Alan Adı**, **DNS**veya Ad Sunucusu **Yönetimi**olarak etiketlenmiş bağlantılarını veya alanlarını arayın.
3. Şimdi a kaydıseçebileceğiniz veya girebileceğiniz yeri bulun. Bir açılır yerden kayıt türünü seçmeniz veya gelişmiş ayarlar sayfasına gitmeniz gerekebilir.
4. Bu A kaydını kullanacak etki alanını veya alt etki alanını seçin veya girin. Örneğin, **www\.customdomain.com**için bir takma ad oluşturmak istiyorsanız **www'yi** seçin. Tüm alt etki alanları için joker karakter girişi oluşturmak istiyorsanız, '*****' girin. Bu, **mail.customdomain.com,** **login.customdomain.com**ve **www\.customdomain.com**gibi tüm alt etki alanlarını kapsayacaktır.

    Kök etki alanı için bir A kaydı oluşturmak istiyorsanız, kayıt**\@** şirketinizin DNS araçlarında ' ' simgesi olarak listelenebilir.
5. Bulut hizmetinizin IP adresini sağlanan alana girin. Bu, A kaydında kullanılan etki alanı girişini bulut hizmeti dağıtımınızın IP adresiyle ilişkilendirer.

Örneğin, aşağıdaki A **kaydı,** dağıtılan uygulamanızın IP adresi olan contoso.com **137.135.70.239'a**kadar tüm trafiği ileder:

| Ana bilgisayar adı/Alt etki alanı | IP adresi |
| --- | --- |
| \@ |137.135.70.239 |

Bu örnek, kök etki alanı için bir A kaydı oluşturmayı gösterir. Tüm alt etki alanlarını kapsayacak şekilde joker karakter girişi oluşturmak istiyorsanız, alt etki alanı olarak '*****' girersiniz.

> [!WARNING]
> Azure'daki IP adresleri varsayılan olarak dinamiktir. IP [adresinizin](../virtual-network/virtual-networks-reserved-public-ip.md) değişmediğinden emin olmak için ayrılmış bir IP adresi kullanmak isteyebilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut Hizmetleri Nasıl Yönetilir?](cloud-services-how-to-manage-portal.md)
* [CDN İçeriğini Özel Etki Alanı ile Eşleme](../cdn/cdn-map-content-to-custom-domain.md)
* [Bulut hizmetinizin genel yapılandırması.](cloud-services-how-to-configure-portal.md)
* [Bulut hizmetini](cloud-services-how-to-create-deploy-portal.md)nasıl dağıtılayın öğrenin.
* [TLS/SSL sertifikalarını](cloud-services-configure-ssl-certificate-portal.md)yapılandırın.

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png



