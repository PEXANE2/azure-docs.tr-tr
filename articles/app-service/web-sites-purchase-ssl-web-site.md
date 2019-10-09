---
title: Azure 'dan bir SSL sertifikası satın alma ve yapılandırma-App Service | Microsoft Docs
description: App Service sertifikasını satın almayı ve App Service uygulamanıza bağlamayı öğrenin
services: app-service
documentationcenter: .net
author: cephalin
manager: jpconnoc
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cephalin
ms.reviewer: apurvajo
ms.custom: seodec18
ms.openlocfilehash: 9569928882bcaaa1d2406c9af1b2197c2ba6e93d
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177512"
---
# <a name="buy-and-configure-an-ssl-certificate-for-azure-app-service"></a>Azure App Service için bir SSL sertifikası satın alın ve yapılandırın

Bu öğreticide, [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) ' de bir App Service sertifikası oluşturarak (satın alarak) [App Service uygulamanızın](https://docs.microsoft.com/azure/app-service/) veya [işlev uygulamanızın](https://docs.microsoft.com/azure/azure-functions/) güvenliğini sağlama işlemi gösterilir ve sonra bunu bir App Service uygulamasına bağlarsınız.

> [!TIP]
> App Service sertifikaları Azure veya Azure dışı hizmetler için kullanılabilir ve App Services ile sınırlı değildir. Bunu yapmak için, istediğiniz her yerde kullanabileceğiniz bir App Service sertifikasının yerel bir PFX kopyasını oluşturmanız gerekir. Daha fazla bilgi için, bkz. [bir App Service sertifikası yerel PFX kopyası oluşturma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/04/12/export-an-azure-app-service-certificate-pfx-powershell/).
>

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu izlemek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanıza bir etki alanı adı eşleyin](app-service-web-tutorial-custom-domain.md) veya [Azure 'da satın alıp yapılandırın](manage-custom-dns-buy-domain.md)

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="start-certificate-order"></a>Sertifika sırasını Başlat

<a href="https://portal.azure.com/#create/Microsoft.SSL" target="_blank">App Service sertifikası oluştur sayfasında</a>bir App Service sertifikası siparişi başlatın.

![Sertifika oluşturma](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Sertifikayı yapılandırmanıza yardımcı olması için aşağıdaki tabloyu kullanın. İşiniz bittiğinde **Oluştur**' a tıklayın.

| Ayar | Description |
|-|-|
| Ad | App Service sertifikanız için kolay bir ad. |
| Naked etki alanı konak adı | Kök etki alanını burada belirtin. Verilen sertifika hem kök etki alanının *hem de* `www` alt etki alanının güvenliğini sağlar. Verilen sertifikada, ortak ad alanı kök etki alanını içerir ve konu alternatif adı alanı `www` etki alanını içerir. Yalnızca herhangi bir alt etki alanının güvenliğini sağlamak için burada alt etki alanının tam etki alanı adını belirtin (örneğin, `mysubdomain.contoso.com`).|
| Abonelik | Web uygulamasının barındırıldığı veri merkezi. |
| Kaynak grubu | Sertifikayı içeren kaynak grubu. Örneğin, yeni bir kaynak grubu kullanabilir veya App Service uygulamanız ile aynı kaynak grubunu seçebilirsiniz. |
| Sertifika SKU 'SU | Oluşturulacak sertifikanın türünü, standart bir sertifika veya [Joker bir sertifika](https://wikipedia.org/wiki/Wildcard_certificate)olduğunu belirler. |
| Yasal koşullar | Yasal koşulları kabul ettiğinizi onaylamak için tıklayın. Sertifikalar GoDaddy adresinden elde edilir. |

## <a name="store-in-azure-key-vault"></a>Azure Key Vault içinde depola

Sertifika satın alma işlemi tamamlandıktan sonra, bu sertifikayı kullanmaya başlayabilmeniz için yapmanız gereken birkaç adım daha vardır. 

[App Service sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasında sertifikayı seçin ve ardından **sertifika yapılandırması** > .**Adım 1: depolama**' ya tıklayın.

![KV içinde depolamaya hazırlanma görüntüsünü Ekle](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) , bulut uygulamaları ve Hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olan bir Azure hizmetidir. Bu, App Service sertifikaları için tercih edilen depolardır.

**Key Vault durum** sayfasında, yeni bir kasa oluşturmak veya mevcut bir kasayı seçmek Için **Key Vault deposu** ' na tıklayın. Yeni bir kasa oluşturmayı seçerseniz, kasayı yapılandırmanıza ve **Oluştur**' a tıklaetmenize yardımcı olması için aşağıdaki tabloyu kullanın.

| Ayar | Description |
|-|-|
| Ad | Alfasayısal karakterler ve tireler için oluşan benzersiz bir ad. |
| Kaynak grubu | Öneri olarak, App Service sertifikanız ile aynı kaynak grubunu seçin. |
| Konum | App Service uygulamanızla aynı konumu seçin. |
| Fiyatlandırma Katmanı | Bilgi için bkz. [Azure Key Vault fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/key-vault/). |
| Erişim ilkeleri| , Kasa kaynaklarına yönelik olan uygulamaları ve izin verilen erişimi tanımlar. Daha sonra, [birkaç uygulamaya bir anahtar kasasına erişim verme](../key-vault/key-vault-group-permissions-for-apps.md)adımlarını izleyerek daha sonra yapılandırabilirsiniz. |
| Sanal ağ erişimi | Belirli Azure sanal ağlarına kasa erişimini kısıtlayın. Daha sonra yapılandırma [Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](../key-vault/key-vault-network-security.md) içindeki adımları izleyerek yapılandırabilirsiniz |

Kasayı seçtikten sonra **Key Vault deposu** sayfasını kapatın. **Mağaza** seçeneğinde, başarılı için yeşil bir onay işareti gösterilmelidir. Sonraki adımda sayfayı açık tutun.

## <a name="verify-domain-ownership"></a>Etki alanı sahipliğini doğrulama

Son adımda kullandığınız **sertifika yapılandırma** sayfasından **2. Adım: doğrulama**' ya tıklayın.

![](./media/app-service-web-purchase-ssl-web-site/verify-domain.png)

**App Service doğrulaması**' nı seçin. Etki alanını Web uygulamanızla eşleştirdikten (bkz. [Önkoşullar](#prerequisites)), zaten doğrulandı. Bu adımı bitirebilmesi için **Doğrula** 'ya tıklamanız yeterlidir. İleti **sertifikası etki alanı doğrulanana** kadar **Yenile** düğmesine tıklayın.

> [!NOTE]
> Dört tür etki alanı doğrulama yöntemi desteklenir: 
> 
> - **App Service** -etki alanı aynı abonelikte bir App Service uygulamasına zaten eşlendiğinde en kullanışlı seçenektir. App Service uygulamasının etki alanı sahipliğini zaten doğruladığından emin olma özelliğinden yararlanır.
> - **Etki alanı** - [Azure 'dan satın aldığınız App Service etki alanını](manage-custom-dns-buy-domain.md)doğrulayın. Azure, doğrulama TXT kaydını sizin için otomatik olarak ekler ve işlemi tamamlar.
> - **Posta** -etki alanı yöneticisine bir e-posta göndererek etki alanını doğrulayın. Seçeneğini belirlediğinizde yönergeler sağlanır.
> - **El ile** -bir HTML sayfası (yalnızca**Standart** SERTIFIKA) veya bir DNS TXT kaydı kullanarak etki alanını doğrulayın. Seçeneğini belirlediğinizde yönergeler sağlanır.

## <a name="bind-certificate-to-app"></a>Sertifikayı uygulamaya bağlama

**[Azure Portal](https://portal.azure.com/)** , sol menüden **uygulama hizmetleri** >  **\<your_ App >** ' ı seçin.

Uygulamanızın sol gezinti bölmesinde **SSL ayarları** > **özel Sertifikalar (. pfx)**  > **içeri aktarma App Service sertifikası**' yı seçin.

![Içeri aktarma sertifikasının görüntüsünü Ekle](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

Yeni satın aldığınız sertifikayı seçin.

Artık sertifika içeri aktarıldığına göre, onu uygulamanızdaki eşlenmiş bir etki alanı adına bağlamanız gerekir. **Bağlamaları** > **SSL bağlaması Ekle**' yi seçin. 

![Içeri aktarma sertifikasının görüntüsünü Ekle](./media/app-service-web-purchase-ssl-web-site/AddBinding.png)

**SSL bağlamaları** iletişim kutusunda bağlamayı yapılandırmanıza yardımcı olması için aşağıdaki tabloyu kullanın, sonra **bağlama Ekle**' ye tıklayın.

| Ayar | Description |
|-|-|
| Konak | SSL bağlaması eklenecek etki alanı adı. |
| Özel sertifika parmak Izi | Bağlanacak sertifika. |
| SSL türü | <ul><li>**SNI SSL** -birden çok SNı tabanlı SSL bağlamaları eklenebilir. Bu seçenek, birden çok SSL sertifikasının aynı IP adresindeki birden fazla etki alanının güvenliğini sağlar. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNı destekler ( [sunucu adı belirtme](https://wikipedia.org/wiki/Server_Name_Indication)daha kapsamlı tarayıcı destek bilgileri bulabilirsiniz).</li><li>**IP tabanlı SSL** -yalnızca tek bir IP tabanlı SSL bağlaması eklenebilir. Bu seçenek, yalnızca bir SSL sertifikasının ayrılmış bir genel IP adresinin güvenliğini sağlamasına izin verir. Bağlamayı yapılandırdıktan sonra, [IP SSL Için bir kaydı](app-service-web-tutorial-custom-ssl.md#remap-a-record-for-ip-ssl)yeniden eşleme bölümündeki adımları uygulayın. </li></ul> |

## <a name="verify-https-access"></a>HTTPS erişimini doğrula

Sertifikanın doğru şekilde yapılandırıldığını doğrulamak için `HTTP://<domain_name>` yerine `HTTPS://<domain_name>` kullanarak uygulamanızı ziyaret edin.

## <a name="rekey-certificate"></a>Sertifikayı yeniden anahtarlama

Sertifikanızın özel anahtarının tehlikede olduğunu düşünüyorsanız, sertifikanızı yeniden anahtarınıza dönüştürebilirsiniz. [App Service sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasında sertifikayı seçin ve ardından sol gezinmede yeniden **anahtarlama ve eşitleme** ' yi seçin.

İşlemi başlatmak için yeniden **anahtarlama** ' ye tıklayın. Bu işlemin tamamlanması 1-10 dakika sürebilir.

![yeniden anahtarlama SSL görüntüsünü Ekle](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Sertifikanızın yeniden oluşturulması sertifikayı sertifika yetkilisinden verilen yeni bir sertifikayla kaydeder.

Yeniden anahtarlama işlemi tamamlandıktan sonra **Eşitle**' ye tıklayın. Eşitleme işlemi, uygulamalarınıza herhangi bir kapalı kalma süresine neden olmadan App Service içindeki sertifika için konak bağlamalarını otomatik olarak güncelleştirir.

> [!NOTE]
> **Eşitle**' yi tıklamıyorsanız, App Service sertifikanızı otomatik olarak 48 saat içinde eşitler.

## <a name="renew-certificate"></a>Sertifikayı Yenile

İstediğiniz zaman sertifikanızın otomatik yenilenmesini açmak için [App Service sertifikalar](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasında sertifikayı seçin ve ardından sol gezinti bölmesinde **Ayarları otomatik Yenile** ' ye tıklayın. App Service sertifikaların varsayılan olarak 1 yıl geçerlilik süresi vardır.

**Açık** ' i seçin ve **Kaydet**' e tıklayın. Otomatik yenileme açıksa sertifikalar, süresi dolmadan 60 gün önce otomatik olarak yenilenbaşlayabilir.

![sertifikayı otomatik olarak Yenile](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Bunun yerine sertifikayı el ile yenilemek için **El Ile Yenile**' ye tıklayın. Sertifikanızın süresi dolmadan 60 gün önce el ile yenileme isteğinde bulunabilir.

Yenileme işlemi tamamlandıktan sonra **Eşitle**' ye tıklayın. Eşitleme işlemi, uygulamalarınıza herhangi bir kapalı kalma süresine neden olmadan App Service içindeki sertifika için konak bağlamalarını otomatik olarak güncelleştirir.

> [!NOTE]
> **Eşitle**' yi tıklamıyorsanız, App Service sertifikanızı otomatik olarak 48 saat içinde eşitler.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

### <a name="azure-cli"></a>Azure CLı

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="more-resources"></a>Daha fazla kaynak

* [HTTPS 'yi zorla](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [TLS 1.1/1.2 'yi zorla](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions)
* [Uygulama kodunuzda bir SSL sertifikası kullanın Azure App Service](app-service-web-ssl-cert-load.md)
* [SSS: sertifikalar App Service](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
