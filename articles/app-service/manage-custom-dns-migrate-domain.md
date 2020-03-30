---
title: Etkin bir DNS adını geçirme
description: Herhangi bir kesinti olmaksızın canlı bir siteye atanmış özel bir DNS etki alanı adını Azure Uygulama Hizmeti'ne nasıl geçirebilirsiniz öğrenin.
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.topic: article
ms.date: 10/21/2019
ms.custom: seodec18
ms.openlocfilehash: 79bd0a19a9bd8ebd100ed80ca0206656d73ef76c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672366"
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Etkin bir DNS adını Azure Uygulama Hizmetine geçirme

Bu makalede, etkin bir DNS adının herhangi bir kesinti olmaksızın [Azure Uygulama Hizmetine](../app-service/overview.md) nasıl geçirilen bir şekilde geçirilebilirsiniz.

Canlı bir siteyi ve dns etki alanını App Service'e geçirdiğinizde, bu DNS adı zaten canlı trafiğe hizmet vermektedir. Etkin DNS adını App Service uygulamanıza önceden bağlayarak geçiş sırasında DNS çözümünde kapalı kalma süresini önleyebilirsiniz.

DNS çözümünde kapalı kalma süresi nden endişe etmiyorsanız, [varolan özel bir DNS adını Azure Uygulama Hizmeti'ne göre Harita'ya](app-service-web-tutorial-custom-domain.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Nasıl yapılacağını tamamlamak için:

- [Uygulama Hizmeti uygulamanızın ÜCRETSİz katmanda olmadığından emin olun.](app-service-web-tutorial-custom-domain.md#checkpricing)

## <a name="bind-the-domain-name-preemptively"></a>Etki alanı adını önceden bağlama

Özel bir etki alanını önceden bağladiğinizde, DNS kayıtlarınızda herhangi bir değişiklik yapmadan önce aşağıdakilerin her ikisini de gerçekleştirirsiniz:

- Etki alanı sahipliğini doğrulama
- Uygulamanızın etki alanı adını etkinleştirme

Özel DNS adınızı eski siteden App Service uygulamasına son olarak ilettiğinizde, DNS çözümünde kapalı kalma süresi olmaz.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Etki alanı doğrulama kaydı oluşturma

Etki alanı sahipliğini doğrulamak için Bir TXT kaydı ekleyin. Awverify gelen TXT kayıt _haritaları.&lt; alt etki alanı>_ _ &lt;>.azurewebsites.net appname_. 

İhtiyacınız olan TXT kaydı, geçirmek istediğiniz DNS kaydına bağlıdır. Örnekler için aşağıdaki tabloya`@` bakın (genellikle kök etki alanını temsil eder):

| DNS kayıt örneği | TXT Ana Bilgisayar | TXT Değeri |
| - | - | - |
| \@(kök) | _awverify_ | _&lt;appname>.azurewebsites.net_ |
| www (alt) | _awverify.www_ | _&lt;appname>.azurewebsites.net_ |
| \*(joker karakter) | _awverify.\*_ | _&lt;appname>.azurewebsites.net_ |

DNS kayıt sayfanızda, geçirmek istediğiniz DNS adının kayıt türünü not edin. Uygulama Hizmeti, CNAME ve A kayıtlarından eşlemeleri destekler.

> [!NOTE]
> CloudFlare gibi belirli sağlayıcılar `awverify.*` için geçerli bir kayıt değildir. Bunun `*` yerine sadece kullanın.

> [!NOTE]
> Joker `*` karakter kayıtları, varolan bir CNAME kaydıyla alt etki alanlarını doğrulamaz. Her alt etki alanı için açıkça bir TXT kaydı oluşturmanız gerekebilir.


### <a name="enable-the-domain-for-your-app"></a>Uygulamanız için etki alanını etkinleştirme

Azure [portalında,](https://portal.azure.com)uygulama sayfasının sol navigasyonunda **Özel etki alanlarını**seçin. 

![Özel etki alanı menüsü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Özel **etki alanları** sayfasında, **+** **hostname ekle'nin**yanındaki simgeyi seçin.

![Konak adı ekleme](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

TXT kaydını eklediğiniz tam nitelikli alan adı `www.contoso.com`yazın, örneğin. Joker karakter etki alanı \*için (contoso.com gibi), joker karakter etki alanıyla eşleşen herhangi bir DNS adını kullanabilirsiniz. 

**Doğrula**'yı seçin.

**Konak adı ekle** düğmesi etkinleştirilir. 

**Ana Bilgisayar adı kayıt türünün** geçirmek istediğiniz DNS kayıt türüne ayarlandığınızdan emin olun.

**Konak adı ekle**'yi seçin.

![Uygulamaya DNS adı ekleme](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Yeni konak adının uygulamanın **Özel etki alanları** sayfasına yansıtılması biraz zaman alabilir. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.

![CNAME kaydı eklenir](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Azure uygulamanızda özel DNS adınız artık etkinleştirildi. 

## <a name="remap-the-active-dns-name"></a>Etkin DNS adını yeniden eşle

Yapmanız gereken tek şey, Etkin DNS kaydınızı App Service'e işaret etmek için yeniden eşlemektir. Şu anda, hala eski sitenizi işaret ediyor.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Uygulamanın IP adresini kopyalama (Yalnızca bir kayıt)

Bir CNAME kaydını yeniden eşlerseniz, bu bölümü atlayın. 

A kaydını yeniden eşlemek için, Uygulama Hizmeti uygulamasının Özel etki **alanları** sayfasında gösterilen harici IP adresine ihtiyacınız vardır.

Sağ üst köşede **X'i** seçerek **ana bilgisayar adı ekle** sayfasını kapatın. 

**Özel etki alanları** sayfasında, uygulamanın IP adresini kopyalayın.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>DNS kaydını güncelleştirme

Etki alanı sağlayıcınızın DNS kayıtları sayfasında yeniden eşlenebilmek için DNS kaydını seçin.

`contoso.com` Kök etki alanı örneği için, Aşağıdaki tablodaki örnekler gibi A veya CNAME kaydını yeniden eşle: 

| FQDN örneği | Kayıt türü | Host | Değer |
| - | - | - | - |
| contoso.com (kök) | A | `@` | [Uygulamanın IP adresini kopyalama](#info) bölümünden IP adresi |
| www\.contoso.com (alt) | CNAME | `www` | _&lt;appname>.azurewebsites.net_ |
| \*.contoso.com (joker karakter) | CNAME | _\*_ | _&lt;appname>.azurewebsites.net_ |

Ayarlarınızı kaydedin.

DNS sorguları, DNS yayılma gerçekleşmeden hemen sonra Uygulama Hizmeti uygulamanızda çözülmeye başlamalıdır.

## <a name="active-domain-in-azure"></a>Azure'da etkin etki alanı

Etkin bir özel etki alanını Azure'da, abonelikler arasında veya aynı abonelik içinde geçirebilirsiniz. Ancak, kapalı kalma süresi olmadan böyle bir geçiş kaynak uygulama gerektirir ve hedef uygulama belirli bir zamanda aynı özel etki alanı atanır. Bu nedenle, iki uygulamanın aynı dağıtım birimine (dahili olarak web alanı olarak bilinir) dağıtılmadığından emin olmanız gerekir. Bir etki alanı adı, her dağıtım biriminde yalnızca bir uygulamaya atanabilir.

FTP/S URL'sinin `<deployment-unit>.ftp.azurewebsites.windows.net`alan adına bakarak uygulamanızın dağıtım birimini bulabilirsiniz. Dağıtım biriminin kaynak uygulama ile hedef uygulama arasında farklı olduğundan emin olun. Bir uygulamanın dağıtım birimi, içinde olduğu [Uygulama Hizmeti planına](overview-hosting-plans.md) göre belirlenir. Planı oluşturduğunuzda Azure tarafından rasgele seçilir ve değiştirilemez. Azure, [aynı kaynak grubunda *ve* aynı bölgede oluşturduğunuzda](app-service-plan-manage.md#create-an-app-service-plan)yalnızca iki planın aynı dağıtım biriminde olduğundan emin olur, ancak planların farklı dağıtım birimlerinde olduğundan emin olmak için herhangi bir mantığı yoktur. Farklı bir dağıtım biriminde plan oluşturmanın tek yolu, farklı bir dağıtım birimi alana kadar yeni bir kaynak grubunda veya bölgede bir plan oluşturmaya devam etmektir.

## <a name="next-steps"></a>Sonraki adımlar

Özel bir SSL sertifikasını Uygulama Hizmetine nasıl bağlatırmayı öğrenin.

> [!div class="nextstepaction"]
> [SSL sertifikasını Azure Uygulama Hizmetine bağlama](configure-ssl-bindings.md)
