---
title: Özel uç noktalarla konuşma hizmetlerini kullanma
titleSuffix: Azure Cognitive Services
description: Azure özel bağlantısı tarafından sunulan özel uç noktalarla konuşma hizmetlerini kullanma
services: cognitive-services
author: alexeyo26
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/04/2020
ms.author: alexeyo
ms.openlocfilehash: 01a0171ed2b660fbabebf4276a74f8a3ea631bde
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97516535"
---
# <a name="using-speech-services-with-private-endpoints-provided-by-azure-private-link"></a>Azure özel bağlantısı tarafından sunulan özel uç noktalarla konuşma hizmetlerini kullanma

[Azure özel bağlantısı](../../private-link/private-link-overview.md) , Azure 'Daki çeşitli PaaS hizmetlerine [özel bir uç nokta](../../private-link/private-endpoint-overview.md)aracılığıyla bağlanmanızı sağlar. Özel uç nokta, belirli bir [sanal ağ](../../virtual-network/virtual-networks-overview.md) ve alt ağ içindeki özel bir IP adresidir.

Bu makalede, Azure bilişsel konuşma Hizmetleri ile özel bağlantı ve özel uç noktaların nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır. 

> [!NOTE]
> Bu makalede, Azure bilişsel konuşma Hizmetleri ile özel bağlantı ayarlama ve kullanma özellikleri açıklanmaktadır. Devam etmeden önce lütfen bilişsel [Hizmetler ile sanal ağları kullanma](../cognitive-services-virtual-networks.md)hakkında genel makaleyi öğrenin.

Özel uç nokta senaryolarında bir konuşma kaynağının etkinleştirilmesi için aşağıdaki görevlerin gerçekleştirilmesi gerekir:
- [Konuşma kaynağı özel etki alanı adı oluştur](#create-custom-domain-name)
- [Özel uç nokta (ler) oluştur ve Yapılandır](#enabling-private-endpoints)
- [Mevcut uygulamaları ve çözümleri ayarla](#using-speech-resource-with-custom-domain-name-and-private-endpoint-enabled)

Daha sonra tüm özel uç noktaları kaldırmaya karar verirseniz ve kaynağı kullanmaya devam ederseniz, gerekli eylemler [Bu bölümde](#using-speech-resource-with-custom-domain-name-without-private-endpoints)açıklanmaktadır.

## <a name="create-custom-domain-name"></a>Özel etki alanı adı oluştur

Özel uç noktalar bilişsel [Hizmetler özel alt etki alanı adlarının](../cognitive-services-custom-subdomains.md)kullanımını gerektirir. Konuşma kaynağınız için bir tane oluşturmak için aşağıdaki yönergeleri kullanın.

> [!WARNING]
> Özel etki alanı adı etkinleştirilmiş bir konuşma kaynağı, konuşma hizmetleriyle etkileşim kurmak için farklı bir yol kullanır. Büyük olasılıkla, özel [  uç nokta etkin](#using-speech-resource-with-custom-domain-name-without-private-endpoints) senaryolar için uygulama kodunuzu her iki [Özel uç nokta etkin](#using-speech-resource-with-custom-domain-name-and-private-endpoint-enabled) olacak şekilde ayarlamanız gerekecektir.
>
> Özel etki alanı adını etkinleştirme işlemi geri [**alınamaz**](../cognitive-services-custom-subdomains.md#can-i-change-a-custom-domain-name). [Bölgesel ada](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) geri dönmenin tek yolu yeni bir konuşma kaynağı oluşturmaktır. 
>
> Özellikle, konuşma kaynağınızın çok sayıda ilişkili özel modeli ve proje, [konuşma Studio](https://speech.microsoft.com/) aracılığıyla oluşturulmuş olduğu durumlarda, yapılandırmayı bir test kaynağıyla denemenizi ve yalnızca üretimde kullanılan olanı değiştirmeyi **önemle** öneririz.

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

- [Azure Portal](https://portal.azure.com/) gidin ve Azure hesabınızda oturum açın
- Gerekli konuşma kaynağını seçin
- *Ağ* seçin (*kaynak yönetim* grubu) 
- *Güvenlik duvarları ve sanal ağlar* sekmesinde (varsayılan) **özel etki alanı adı oluştur** düğmesine tıklayın.
- Kaynağınız için benzersiz bir özel alt etki alanı oluşturma yönergeleriyle birlikte yeni bir panel görüntülenir
> [!WARNING]
> Özel bir etki alanı adı oluşturduktan sonra **,** bu değiştirilemez. Yukarıdaki uyarıda daha fazla bilgi görüntüleyin.
- İşlem tamamlandıktan sonra *anahtarlar ve uç nokta* (*kaynak yönetimi* grubu) seçeneğini belirlemek ve kaynağınızın yeni uç nokta adını şu biçimde doğrulamak isteyebilirsiniz. <p />`{your custom name}.cognitiveservices.azure.com`

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Bu bölüm, Azure PowerShell Module sürümü 5.1.0 veya üzeri ile yerel olarak PowerShell sürüm 7. x veya üstünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

Daha sonra `Connect-AzAccount` Azure ile bağlantı oluşturmak için çalışmaya devam etmeden önce.

## <a name="verify-custom-domain-name-availability"></a>Özel etki alanı adının kullanılabilirliğini doğrulama

Kullanmak istediğiniz özel etki alanının serbest olup olmadığını denetlemeniz gerekir. Bilişsel hizmetler REST API, [etki alanı kullanılabilirliği yöntemini denetle](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) 'yi kullanacağız. Adımları açıklayan kod bloğundaki açıklamalara bakın.

> [!TIP]
> Aşağıdaki kod Azure Cloud Shell içinde **çalışmaz.**

```azurepowershell
$subId = "Your Azure subscription Id"
$subdomainName = "custom domain name"

# Select the Azure subscription containing Speech resource
# If your Azure account has only one active subscription
# you can skip this step
Set-AzContext -SubscriptionId $subId

# Preparing OAuth token which is used in request
# to Cognitive Services REST API
$Context = Get-AzContext
$AccessToken = (Get-AzAccessToken -TenantId $Context.Tenant.Id).Token
$token = ConvertTo-SecureString -String $AccessToken -AsPlainText -Force

# Preparing and executing the request to Cognitive Services REST API
$uri = "https://management.azure.com/subscriptions/" + $subId + `
    "/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18"
$body = @{
subdomainName = $subdomainName
type = "Microsoft.CognitiveServices/accounts"
}
$jsonBody = $body | ConvertTo-Json
Invoke-RestMethod -Method Post -Uri $uri -ContentType "application/json" -Authentication Bearer `
    -Token $token -Body $jsonBody | Format-List
```
İstenen ad varsa şöyle bir yanıt alacaksınız:
```azurepowershell
isSubdomainAvailable : True
reason               :
type                 :
subdomainName        : my-custom-name
```
Ad zaten alınmış ise, aşağıdaki yanıtı alırsınız:
```azurepowershell
isSubdomainAvailable : False
reason               : Sub domain name 'my-custom-name' is already used. Please pick a different name.
type                 :
subdomainName        : my-custom-name
```
## <a name="enabling-custom-domain-name"></a>Özel etki alanı adı etkinleştiriliyor

Seçili konuşma kaynağı için özel etki alanı adını etkinleştirmek üzere [set-azbiliveservicesaccount](/powershell/module/az.cognitiveservices/set-azcognitiveservicesaccount) cmdlet 'ini kullanırız. Adımları açıklayan kod bloğundaki açıklamalara bakın.

> [!WARNING]
> Aşağıdaki kodu başarıyla yürütmeden sonra, konuşma kaynağınız için özel bir etki alanı adı oluşturacaksınız. Bu ad **değiştirilemez** . Yukarıdaki uyarıda daha fazla bilgi görüntüleyin.

```azurepowershell
$resourceGroup = "Resource group name where Speech resource is located"
$speechResourceName = "Your Speech resource name"
$subdomainName = "custom domain name"

# Select the Azure subscription containing Speech resource
# If your Azure account has only one active subscription
# you can skip this step
$subId = "Your Azure subscription Id"
Set-AzContext -SubscriptionId $subId

# Set the custom domain name to the selected resource
# WARNING! THIS IS NOT REVERSIBLE!
Set-AzCognitiveServicesAccount -ResourceGroupName $resourceGroup `
    -Name $speechResourceName -CustomSubdomainName $subdomainName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu bölüm, Azure CLı 'nın en son sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="verify-custom-domain-name-availability"></a>Özel etki alanı adının kullanılabilirliğini doğrulama

Kullanmak istediğiniz özel etki alanının serbest olup olmadığını denetlemeniz gerekir. Bilişsel hizmetler REST API, [etki alanı kullanılabilirliği yöntemini denetle](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) 'yi kullanacağız. 

Aşağıdaki kod bloğunu kopyalayın, özel etki alanı adını ekleyin ve dosyaya kaydedin `subdomain.json` .

```json
{
    "subdomainName": "custom domain name",
    "type": "Microsoft.CognitiveServices/accounts"
}
```

Dosyayı geçerli klasörünüze kopyalayın veya Azure Cloud Shell karşıya yükleyin ve aşağıdaki komutu yürütün. ( `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` Azure ABONELIK Kimliğinizle değiştirin).

```azurecli-interactive
az rest --method post --url "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18" --body @subdomain.json
```
İstenen ad varsa şöyle bir yanıt alacaksınız:
```azurecli
{
  "isSubdomainAvailable": true,
  "reason": null,
  "subdomainName": "my-custom-name",
  "type": null
}
```

Ad zaten alınmış ise, aşağıdaki yanıtı alırsınız:
```azurecli
{
  "isSubdomainAvailable": false,
  "reason": "Sub domain name 'my-custom-name' is already used. Please pick a different name.",
  "subdomainName": "my-custom-name",
  "type": null
}
```
## <a name="enabling-custom-domain-name"></a>Özel etki alanı adı etkinleştiriliyor

Seçili konuşma kaynağı için özel etki alanı adını etkinleştirmek üzere [az biliveservices hesabı Güncelleştir](/cli/azure/cognitiveservices/account#az_cognitiveservices_account_update) komutunu kullanırız.

Konuşma kaynağını içeren Azure aboneliğini seçin. Azure hesabınızda yalnızca bir etkin abonelik varsa, bu adımı atlayabilirsiniz. ( `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` Azure ABONELIK Kimliğinizle değiştirin).
```azurecli-interactive
az account set --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
Özel etki alanı adını seçili kaynak olarak ayarlayın. Örnek parametre değerlerini gerçek olanlarla değiştirin ve aşağıdaki komutu yürütün.
> [!WARNING]
> Aşağıdaki komutu başarıyla yürütmeden sonra, konuşma kaynağınız için özel bir etki alanı adı oluşturacaksınız. Bu ad **değiştirilemez** . Yukarıdaki uyarıda daha fazla bilgi görüntüleyin.
```azurecli
az cognitiveservices account update --name my-speech-resource-name --resource-group my-resource-group-name --custom-domain my-custom-name
```

**_

## <a name="enabling-private-endpoints"></a>Özel uç noktaları etkinleştirme

Azure portal, Azure PowerShell veya Azure CLı kullanarak özel uç noktayı etkinleştirin.

Özel uç noktalar için gerekli güncelleştirmelerle sanal ağa bağlı [özel DNS bölgesini](../../dns/private-dns-overview.md) kullanmanızı öneririz. Bu işlem, sağlama işlemi sırasında varsayılan olarak oluşturulur. Ancak, kendi DNS sunucunuzu kullanıyorsanız, DNS yapılandırmanızda ek değişiklikler yapmanız gerekebilir. Bkz. [Özel uç noktalar Için DNS](#dns-for-private-endpoints) bölümü. En iyi yöntem, bir üretim konuşma kaynağı için * özel uç noktaları sağlama ' dan *önce*, DNS stratejisi _ ' i seçmemeye yöneliktir. Ayrıca, özellikle kendi DNS sunucunuzu kullanıyorsanız, ön test öneririz.

Özel uç noktaları oluşturmak için aşağıdaki makaleleri kullanın. Makaleler, Özel uç noktalarla etkinleştirilecek örnek kaynak olarak bir Web uygulaması kullanıyor. Bunun yerine aşağıdaki parametreleri kullanın:

| Ayar             | Değer                                    |
|---------------------|------------------------------------------|
| Kaynak türü       | **Microsoft. Biliveservices/hesapları** |
| Kaynak            | **\<your-speech-resource-name>**         |
| Hedef alt kaynak | **açın**                              |

- [Azure portalını kullanarak Özel Uç Nokta oluşturma](../../private-link/create-private-endpoint-portal.md)
- [Azure PowerShell kullanarak özel uç nokta oluşturma](../../private-link/create-private-endpoint-powershell.md)
- [Azure CLı kullanarak özel uç nokta oluşturma](../../private-link/create-private-endpoint-cli.md)

### <a name="dns-for-private-endpoints"></a>Özel uç noktalar için DNS

Bilişsel [Hizmetler kaynaklarındaki özel uç noktalar Için DNS](../cognitive-services-virtual-networks.md#dns-changes-for-private-endpoints)genel ilkelerini öğrenin. Daha sonra DNS yapılandırmanızın düzgün çalışıp çalışmadığını denetleyin (sonraki alt bölümleri inceleyin).

#### <a name="mandatory-check-dns-resolution-from-the-virtual-network"></a>(Zorunlu denetim). Sanal ağdan DNS çözümlemesi

`my-private-link-speech.cognitiveservices.azure.com`Bu bölüm için örnek bir konuşma kaynağı DNS adı olarak kullanacağız.

Özel uç noktanıza eklediğiniz sanal ağda bulunan bir sanal makinede oturum açın. Windows komut Istemi veya bash kabuğu 'nu açın, ' nslookup ' komutunu yürütün ve kaynak özel etki alanı adınızı başarıyla çözümleyip çözmediğine emin olun:
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  168.63.129.16

Non-authoritative answer:
Name:    my-private-link-speech.privatelink.cognitiveservices.azure.com
Address:  172.28.0.10
Aliases:  my-private-link-speech.cognitiveservices.azure.com
```
Çözümlenen IP adresinin özel uç noktanızın adresine karşılık geldiğinden emin olun.

#### <a name="optional-check-dns-resolution-from-other-networks"></a>(İsteğe bağlı denetim). Diğer ağlardan DNS çözümlemesi

Bu denetim, Özel uç nokta etkin konuşma kaynağınızı "karma" modda kullanmayı planlıyorsanız, kaynağınızın *ağ* bölümünde *tüm ağların* veya *Seçili ağların ve özel uç noktalar* erişim seçeneğinin etkin olduğunu belirten bu denetim gereklidir. Kaynağa yalnızca özel uç nokta kullanarak erişmeyi planlıyorsanız, bu bölümü atlayabilirsiniz.

`my-private-link-speech.cognitiveservices.azure.com`Bu bölüm için örnek bir konuşma kaynağı DNS adı olarak kullanacağız.

Kaynağa erişim izni veren bir ağa bağlı herhangi bir makinede, Windows komut Istemi veya bash kabuğu 'nu açın, ' nslookup ' komutunu yürütün ve kaynak özel etki alanı adınızı başarıyla çözümleyip çözmediğine emin olun:
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  fe80::1

Non-authoritative answer:
Name:    vnetproxyv1-weu-prod.westeurope.cloudapp.azure.com
Address:  13.69.67.71
Aliases:  my-private-link-speech.cognitiveservices.azure.com
          my-private-link-speech.privatelink.cognitiveservices.azure.com
          westeurope.prod.vnet.cog.trafficmanager.net
```

Çözümlenen IP adresi, ağ trafiğini özel uç nokta etkin bilişsel hizmetler kaynağına dağıtırken kullanılan bir VNet proxy uç noktasına işaret ettiğini unutmayın. Bu davranış, özel etki alanı adı etkinleştirilmiş *, ancak özel* uç noktaları yapılandırılmadığı bir kaynak için farklı olacaktır. [Bu bölüme](#dns-configuration)bakın.

## <a name="adjusting-existing-applications-and-solutions"></a>Mevcut uygulamaları ve çözümleri ayarlama 

Özel etki alanı etkinleştirilmiş bir konuşma kaynağı, konuşma hizmetleriyle etkileşim kurmak için farklı bir yol kullanır. Bu, özel bir etki alanı etkin konuşma kaynağı için, [hem hem de özel](#using-speech-resource-with-custom-domain-name-and-private-endpoint-enabled) uç noktaları [olmadan](#using-speech-resource-with-custom-domain-name-without-private-endpoints) geçerlidir. Geçerli bölümde her iki durumda da gerekli bilgiler sağlanmaktadır.

### <a name="using-speech-resource-with-custom-domain-name-and-private-endpoint-enabled"></a>Özel etki alanı adı ve özel uç nokta etkinken konuşma kaynağını kullanma

Özel etki alanı adı ve özel uç noktası etkin bir konuşma kaynağı, konuşma hizmetleriyle etkileşmek için farklı bir yol kullanır. Bu bölümde, söz konusu kaynağın konuşma Hizmetleri REST API ve [konuşma SDK 'sı](speech-sdk.md)ile nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Özel uç noktaları olmayan ancak **özel etki alanı adı** etkinleştirilmiş bir konuşma kaynağının, konuşma hizmetleriyle birlikte etkileşimde bulunmak için özel bir yöntemi olduğunu lütfen unutmayın, ancak bu yöntem özel bir uç nokta etkin konuşma kaynağının senaryosundan farklıdır. Bu kaynakta (örneğin, Özel uç noktaları olan bir kaynağınız vardı, ancak bunları kaldırmaya karar verdiyseniz), ilgili [bölümü](#using-speech-resource-with-custom-domain-name-without-private-endpoints)öğrendiğinizden emin olun.

#### <a name="speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-rest-api"></a>Özel etki alanı adı ve özel uç nokta içeren konuşma kaynağı. REST API kullanımı

`my-private-link-speech.cognitiveservices.azure.com`Bu bölüm için örnek bir konuşma kaynağı DNS adı (özel etki alanı) olarak kullanacağız.

##### <a name="note-on-speech-services-rest-api"></a>Konuşma hizmetlerinde REST API

Konuşma Hizmetleri, [konuşmadan metne](rest-speech-to-text.md) ve [metinden konuşmaya](rest-text-to-speech.md)yönelik REST API sahiptir. Özel uç nokta etkin senaryosu için aşağıdakiler göz önünde bulundurulmalıdır.

Konuşmadan metne iki farklı REST API 'si vardır. Her API farklı bir amaca hizmet eder, farklı uç noktalar kullanır ve özel uç nokta etkin senaryosunda kullanıldığında farklı bir yaklaşım gerektirir.

Konuşmadan metne REST API 'Leri şunlardır:
- [Okuma-metin REST API v 3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30) , [toplu iş dökümü](batch-transcription.md) ve [özel konuşma tanıma](custom-speech-overview.md)için kullanılır. v 3.0, [v 2.0 'ın bir ardıldır](/azure/cognitive-services/speech-service/migrate-v2-to-v3).
- [Kısa seste konuşmadan metne REST API](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) çevrimiçi döküm için kullanılır. 

Özel uç nokta senaryosunda, kısa ses ve metinden konuşmaya REST API için konuşmadan metne REST API kullanımı, bu makalenin ilerleyen kısımlarında açıklanan [konuşma SDK 'nın büyük ve küçük harfe](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk) eşittir. 

Konuşmadan metne REST API v 3.0, farklı bir uç nokta kümesi kullanıyor ve bu nedenle özel uç nokta etkin senaryosu için farklı bir yaklaşım gerektirir.

Her iki durum da sonraki alt bölümlerde açıklanmıştır.


##### <a name="speech-to-text-rest-api-v30"></a>Konuşmayı metne REST API v 3.0

Genellikle konuşma kaynakları, [konuşmadan metne REST API v 3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30)ile iletişim kurmak için bilişsel [Hizmetler bölgesel uç noktaları](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) kullanır. Bu kaynaklar aşağıdaki adlandırma biçimine sahiptir: <p/>`{region}.api.cognitive.microsoft.com`

Bu bir örnek istek URL 'sidir:

```http
https://westeurope.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions
```
Bir konuşma kaynağı için özel etki alanını etkinleştirdikten sonra (Özel uç noktalar için gereklidir), bu kaynak, temel REST API uç noktası için aşağıdaki DNS ad modelini kullanır: <p/>`{your custom name}.cognitiveservices.azure.com`

Bu, örneğimizde REST API uç nokta adı olacaktır. <p/>`my-private-link-speech.cognitiveservices.azure.com`

Ve yukarıdaki örnek istek URL 'sinin dönüştürülmesi gerekir:
```http
https://my-private-link-speech.cognitiveservices.azure.com/speechtotext/v3.0/transcriptions
```
Bu URL 'ye, Özel uç nokta eklenmiş olan sanal ağdan ulaşılamamalıdır ( [doğru DNS çözümlemesi](#mandatory-check-dns-resolution-from-the-virtual-network)sağlanmış).

Bu nedenle, bir konuşma kaynağı için özel etki alanı adı etkinleştirildikten sonra, tüm istek URL 'Lerinde ana bilgisayar adını yeni özel etki alanı ana bilgisayar adı ile değiştirmeniz gerekir. İsteğin diğer tüm bölümleri ( `/speechtotext/v3.0/transcriptions` Yukarıdaki örnekteki yol gibi) aynı kalır.

> [!TIP]
> Bazı müşteriler bölgesel uç nokta DNS adının bölge kısmını kullanan uygulamalar geliştirmiş (örneğin, isteği belirli bir Azure bölgesinde dağıtılan konuşma kaynağına göndermek için).
>
> Konuşma kaynağı özel etki alanı adı, kaynağın dağıtıldığı bölge hakkında bilgi **içermez** . **Bu nedenle, yukarıda** açıklanan uygulama mantığı çalışmayacak ve değiştirilmesi gerekiyor.

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>Kısa ses ve metinden konuşmaya REST API için konuşmadan metne REST API

Kısa ses ve [metinden konuşmaya REST API](rest-text-to-speech.md) [için konuşmadan metne REST API](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) iki uç nokta türünü kullanır:
- Bilişsel hizmetler, bir yetkilendirme belirteci almak için bilişsel hizmetler REST API iletişim için [Bölgesel uç noktalar](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints)
- Diğer tüm işlemler için özel uç noktalar

Özel bitiş noktalarının ayrıntılı açıklaması ve özel bir uç nokta etkin konuşma kaynağı için URL 'nin nasıl dönüştürülmesi gerektiği, aşağıdaki "konuşma SDK 'Sı ile kullanım" bölümünde bulunan [Bu alt](#general-principle) bölümde verilmiştir. SDK için açıklanan ilke, konuşmadan metne v 1.0 ve metinden konuşmaya REST API için geçerlidir.

Önceki paragrafta bahsedilen alt bölümünde bulunan malzemeyi öğrenin ve aşağıdaki örneğe bakın. (Örneğin, metin okuma REST API, kısa ses için konuşmadan metne REST API kullanımı tamamen eşdeğerdir)

> [!NOTE]
> Özel uç nokta senaryolarındaki **kısa ses için konuşmayı metne REST API** kullanırken, üst bilgiyle [geçirilen](rest-speech-to-text.md#request-headers) yetkilendirme belirtecini kullanmanız gerekir `Authorization` [](rest-speech-to-text.md#request-headers); konuşma abonelik anahtarını üst bilgi aracılığıyla özel uç noktaya geçirme `Ocp-Apim-Subscription-Key` çalışmaz ve 401 hatası oluşturacaktır  .

**Metin okuma REST API kullanım örneği.**

Batı Avrupa, örnek bir Azure bölgesi olarak ve `my-private-link-speech.cognitiveservices.azure.com` örnek bir konuşma kaynağı DNS adı (özel etki alanı) olarak kullanacağız. Örneğimizde özel etki alanı adı `my-private-link-speech.cognitiveservices.azure.com` , Batı Avrupa bölgesinde oluşturulan konuşma kaynağına aittir.

Tek bir bölgede desteklenen seslerin listesini almak için aşağıdaki iki işlemi yapmanız gerekir:

- Aracılığıyla yetkilendirme belirteci al
```http
https://westeurope.api.cognitive.microsoft.com/sts/v1.0/issuetoken
```
- Alınan belirteci kullanma ile seslerinin listesini alın
```http
https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list
```
(Yukarıdaki adımlar hakkında daha fazla bilgi için bkz. [metin okuma REST API belgeleri](rest-text-to-speech.md))

Özel uç nokta etkin konuşma kaynağı için aynı işlem sırası için uç nokta URL 'Lerinin değiştirilmesi gerekir. Aynı sıra şöyle görünür:
- Aracılığıyla yetkilendirme belirteci al
```http
https://my-private-link-speech.cognitiveservices.azure.com/v1.0/issuetoken
```
(Yukarıdaki [konuşmayı metne REST API v 3.0](#speech-to-text-rest-api-v30) alt bölümünde ayrıntılı açıklama bölümüne bakın)
- Alınan belirteci kullanma ile seslerinin listesini alın
```http
https://my-private-link-speech.cognitiveservices.azure.com/tts/cognitiveservices/voices/list
```
(aşağıdaki "konuşma SDK 'Sı ile kullanım" başlıklı [genel ilke](#general-principle) alt bölümünde ayrıntılı açıklama bölümüne bakın)

#### <a name="speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk"></a>Özel etki alanı adı ve özel uç nokta içeren konuşma kaynağı. Konuşma SDK 'Sı ile kullanım

Özel etki alanı adı ve özel uç nokta etkin konuşma kaynaklarıyla konuşma SDK 'sının kullanılması, uygulama kodunuzun gözden geçirilmesi ve olası değişiklikleri gerektirir. Özel uç nokta senaryosuna yönelik daha sorunsuz destek üzerinde çalışıyoruz.

`my-private-link-speech.cognitiveservices.azure.com`Bu bölüm için örnek bir konuşma kaynağı DNS adı (özel etki alanı) olarak kullanacağız.

##### <a name="general-principle"></a>Genel ilke

Genellikle SDK senaryolarında (ve metin okuma REST API senaryolarındaki) konuşma kaynakları, farklı hizmet teklifleri için özel bölgesel uç noktaları kullanır. Bu uç noktaların DNS ad biçimi: </p>`{region}.{speech service offering}.speech.microsoft.com`

Örnek: </p>`westeurope.stt.speech.microsoft.com`

Bölgenin tüm olası değerleri (DNS adının ilk öğesi) [burada](regions.md) listelenen bu tabloda, konuşma Hizmetleri sunumu için olası değer (DNS adının ikinci öğesi) gösterilmektedir:

| DNS adı değeri | Konuşma Hizmetleri teklifi                                    |
|----------------|-------------------------------------------------------------|
| `commands`     | [Özel Komutlar](custom-commands.md)                       |
| `convai`       | [Konuşma dökümü](conversation-transcription.md) |
| `s2s`          | [Konuşma Çevirisi](speech-translation.md)                 |
| `stt`          | [Konuşmayı metne dönüştürme](speech-to-text.md)                         |
| `tts`          | [Metin okuma](text-to-speech.md)                         |
| `voice`        | [Özel ses](how-to-custom-voice.md)                      |

Bu nedenle, yukarıdaki örnek ( `westeurope.stt.speech.microsoft.com` ) Batı Avrupa Içinde konuşmaya metin uç noktası için temsil eder.

Özel uç nokta etkin uç noktalar, özel bir ara sunucu aracılığıyla konuşma hizmetleriyle iletişim kurar ve **uç nokta bağlantı URL 'lerinin değiştirilmesi gerekir**. Aşağıdaki ilke uygulandı: bir "standart" uç nokta URL 'SI, <p/>`{region}.{speech service offering}.speech.microsoft.com/{URL path}`

Şu şekilde değiştirilmelidir: <p/>`{your custom name}.cognitiveservices.azure.com/{speech service offering}/{URL path}`

**Örnek 1.** Uygulama aşağıdaki URL 'YI kullanarak iletişim kuruyor (Batı Avrupa için ABD Ingilizcesi için temel modeli kullanan konuşma tanıma): 
```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Konuşma kaynağının özel etki alanı adı olduğunda bu URL 'yi özel uç nokta etkin senaryosunda kullanmak için `my-private-link-speech.cognitiveservices.azure.com` Bu URL 'nin şu şekilde değiştirilmesi gerekir:
```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Daha yakından bakalım:
- Ana bilgisayar adı `westeurope.stt.speech.microsoft.com` özel etki alanı ana bilgisayar adı ile değiştirilmiştir `my-private-link-speech.cognitiveservices.azure.com`
- Özgün DNS adının () ikinci öğesi, `stt` URL yolunun ilk öğesi olur ve özgün yolundan önce `/speech/recognition/conversation/cognitiveservices/v1?language=en-US` gelir. `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US`
 
**Örnek 2.** Uygulama aşağıdaki URL 'YI kullanarak iletişim kuruyor (Batı Avrupa içinde özel ses modeli kullanılarak konuşma senlimleme): 
```http
https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```
Konuşma kaynağının özel etki alanı adı olduğunda bu URL 'yi özel uç nokta etkin senaryosunda kullanmak için `my-private-link-speech.cognitiveservices.azure.com` Bu URL 'nin şu şekilde değiştirilmesi gerekir: 
```http
https://my-private-link-speech.cognitiveservices.azure.com/voice/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```

Örnek 1 ' deki aynı ilke uygulanır, ancak bu zaman anahtar öğesi `voice` .

##### <a name="modifying-applications"></a>Uygulamaları değiştirme

Önceki bölümde açıklanan prensibi uygulama kodunuza uygulamak için iki önemli şey yapmanız gerekir:

- Uygulamanızın kullandığı uç nokta URL 'sini belirleme
- Uç nokta URL 'nizi, önceki bölümde açıklandığı gibi değiştirin ve `SpeechConfig` Bu değiştirilmiş URL 'yi açıkça kullanarak sınıf örneğinizi oluşturun

###### <a name="determining-application-endpoint-url"></a>Uygulama uç noktası URL 'sini belirleme

- [Uygulamanızı günlüğe kaydetmeyi etkinleştirin](how-to-use-logging.md) ve günlüğü oluşturmak için çalıştırın
- Günlük dosyasında arama yapın `SPEECH-ConnectionUrl` . Dize `value` parametresi içerecektir, bu da uygulamanızın kullandığı tam URL 'yi içerir

Uç nokta URL 'SI olan bir günlük dosyası satırı örneği:
```
(114917): 41ms SPX_DBG_TRACE_VERBOSE:  property_bag_impl.cpp:138 ISpxPropertyBagImpl::LogPropertyAndValue: this=0x0000028FE4809D78; name='SPEECH-ConnectionUrl'; value='wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?traffictype=spx&language=en-US'
```
Bu nedenle, bu örnekteki uygulama tarafından kullanılan URL şu şekilde olur:
```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```
###### <a name="creating-speechconfig-instance-using-full-endpoint-url"></a>`SpeechConfig`Tam uç nokta URL 'si kullanılarak örnek oluşturuluyor

Yukarıdaki [genel prensibi](#general-principle) bölümünde açıklandığı gibi, önceki bölümde belirlediğiniz uç noktayı değiştirin.

Şimdi örneğini oluşturma şeklini değiştirmeniz gerekir `SpeechConfig` . Bugünün uygulamanız şuna benzer olabilir:
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
Bu, önceki bölümlerde açıklandığımız ana bilgisayar adı ve URL değişikliklerinden dolayı özel uç nokta özellikli konuşma kaynağı için çalışmayacaktır. Mevcut uygulamanızı özel bir uç noktası etkin bir kaynağın anahtarını kullanarak herhangi bir değişiklik yapmadan çalıştırmaya çalışırsanız, kimlik doğrulama hatası alırsınız (401).

Bu işlemi gerçekleştirmek için, sınıfın örneğini oluşturma `SpeechConfig` ve "bitiş noktası" ve "bitiş noktası" başlatmasının nasıl kullanılacağı üzerinde değişiklik yapmanız gerekir. Aşağıdaki iki değişkenin tanımlı olduğunu varsayalım:
- `subscriptionKey` Özel uç nokta etkin konuşma kaynağının anahtarını içeren
- `endPoint` Tam **değiştirilen** uç nokta URL 'sini (karşılık gelenprogramlama dili için gereken türü kullanarak) içerir. Örneğimizde bu değişkenin şunu içermesi gerekir
```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```
Daha sonra aşağıdaki gibi bir sınıf örneği oluşturmanız gerekir `SpeechConfig` :
```csharp
var config = SpeechConfig.FromEndpoint(endPoint, subscriptionKey);
```
```cpp
auto config = SpeechConfig::FromEndpoint(endPoint, subscriptionKey);
```
```java
SpeechConfig config = SpeechConfig.fromEndpoint(endPoint, subscriptionKey);
```
```python
import azure.cognitiveservices.speech as speechsdk
speech_config = speechsdk.SpeechConfig(endpoint=endPoint, subscription=subscriptionKey)
```
```objectivec
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithEndpoint:endPoint subscription:subscriptionKey];
```
> [!TIP]
> Uç nokta URI 'sinde belirtilen sorgu parametreleri diğer API 'Ler tarafından ayarlansalar bile değiştirilmez. Örneğin, tanıma dili URI 'de "Language = en-US" sorgu parametresi olarak tanımlandıysa ve aynı zamanda ilgili özelliği aracılığıyla "ru-RU" olarak ayarlanırsa, URI 'deki dil ayarı önceliklidir ve geçerli dil "en-US" dir. Yalnızca uç nokta URI 'sinde belirtilmeyen parametreler diğer API 'Ler tarafından ayarlanabilir.

Bu değişiklikten sonra uygulamanızın özel özellikli konuşma kaynaklarıyla çalışması gerekir. Özel uç nokta senaryosuna yönelik daha sorunsuz destek üzerinde çalışıyoruz.

### <a name="using-speech-resource-with-custom-domain-name-without-private-endpoints"></a>Özel uç noktaları olmadan özel etki alanı adıyla konuşma kaynağını kullanma

Bu makalede, bir konuşma kaynağı için özel etki alanını etkinleştiren geri **alınamaz** ve söz konusu kaynak, konuşma hizmetleriyle iletişim kurarak "olağan dışı" ( [Bölgesel uç noktası adlarını](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints)kullanan) ile karşılaştırıldığında farklı bir iletişim yöntemi kullanacaktır.

Bu bölümde, bir konuşma kaynağının özel etki alanı adı ve konuşma Hizmetleri REST API ve [konuşma SDK 'sı](speech-sdk.md)ile özel uç noktaları **olmadan** nasıl kullanılacağı açıklanmaktadır. Bu, bir özel uç nokta senaryosunda kullanılan, ancak özel uç noktaları silinen bir kaynak olabilir.

#### <a name="dns-configuration"></a>DNS yapılandırması

Özel uç nokta etkin konuşma kaynağının özel etki alanı DNS adının [Ortak ağlardan nasıl çözümlendiğini](#optional-check-dns-resolution-from-other-networks)unutmayın. Bu durumda IP adresi, ağ trafiğini özel uç nokta etkin bilişsel hizmetler kaynağına dağıtırken kullanılan bir VNet proxy uç noktasına işaret eder.

Ancak, **Tüm** kaynak özel uç noktaları kaldırıldığında (veya özel etki alanı adı etkinleştirildikten sonra sağ), konuşma kaynağının CNAME kaydı yeniden sağlandıysa ve şimdi uygun bilişsel [Hizmetler bölgesel uç noktasının](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints)IP adresine işaret ediyorsa.

Bu nedenle komutun çıktısı şöyle görünür `nslookup` :
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  fe80::1

Non-authoritative answer:
Name:    apimgmthskquihpkz6d90kmhvnabrx3ms3pdubscpdfk1tsx3a.cloudapp.net
Address:  13.93.122.1
Aliases:  my-private-link-speech.cognitiveservices.azure.com
          westeurope.api.cognitive.microsoft.com
          cognitiveweprod.trafficmanager.net
          cognitiveweprod.azure-api.net
          apimgmttmdjylckcx6clmh2isu2wr38uqzm63s8n4ub2y3e6xs.trafficmanager.net
          cognitiveweprod-westeurope-01.regional.azure-api.net
```
[Bu bölümün](#optional-check-dns-resolution-from-other-networks)çıkışıyla karşılaştırın.

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-rest-api"></a>Özel uç noktaları olmadan özel etki alanı adına sahip konuşma kaynağı. REST API kullanımı

##### <a name="speech-to-text-rest-api-v30"></a>Konuşmayı metne REST API v 3.0

Konuşmadan metne REST API v 3.0 kullanımı, [Özel uç nokta özellikli konuşma kaynakları](#speech-to-text-rest-api-v30)için tam olarak eşdeğerdir.

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>Kısa ses ve metinden konuşmaya REST API için konuşmadan metne REST API

Bu durumda, kısa ses ve metinden konuşmaya REST API kullanım için konuşmadan metne REST API, kısa bir ses için konuşmaya metin REST API için tek bir özel durum ile genel durumda bir farklılık yoktur (aşağıdaki nota bakın). Her iki API de, kısa ses ve [metinden konuşmaya REST API](rest-text-to-speech.md) belgeleri [için konuşmadan metne REST API](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) açıklandığı gibi kullanılmalıdır.

> [!NOTE]
> Özel etki alanı senaryolarında **kısa ses için konuşmayı metne REST API** kullanırken, üst bilgiyle [geçirilen](rest-speech-to-text.md#request-headers) yetkilendirme belirtecini kullanmanız gerekir `Authorization` [](rest-speech-to-text.md#request-headers); konuşma abonelik anahtarını üst bilgi aracılığıyla özel uç noktaya geçirme `Ocp-Apim-Subscription-Key` çalışmaz ve 401 hatası oluşturur. 

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-speech-sdk"></a>Özel uç noktaları olmadan özel etki alanı adına sahip konuşma kaynağı. Konuşma SDK 'Sı ile kullanım

Özel etki uç noktaları **olmadan** konuşma SDK 'sını etkin bir şekilde kullanmak, uygulama kodunuzun gözden geçirilmesi ve olası değişiklikleri gerektirir. Bu değişikliklerin [özel bir uç nokta etkin konuşma kaynağı](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk)ile karşılaştırıldığında **farklı** olduğunu unutmayın. Özel uç nokta/özel etki alanı senaryosuna yönelik daha sorunsuz destek üzerinde çalışıyoruz.

`my-private-link-speech.cognitiveservices.azure.com`Bu bölüm için örnek bir konuşma kaynağı DNS adı (özel etki alanı) olarak kullanacağız.

[Özel uç nokta etkin konuşma kaynağı](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk) bölümünde, kullanılan uç nokta URL 'sini nasıl belirleyebiliyoruz, bunu değiştirip "/" uç noktasından "/" bir sınıf örneği başlatmasıyla çalışmasını nasıl yapacağız anlatılmıştır `SpeechConfig` .

Ancak, tüm özel uç noktaları kaldırıldıktan sonra aynı uygulamayı çalıştırmayı denerseniz (bir süre DNS kaydı yeniden sağlanmasına izin vermek), Iç hizmet hatası alırsınız (404). Bunun nedeni, artık VNet proxy 'si yerine bölgesel bilişsel hizmetler uç noktasını işaret eden [DNS kaydıdır](#dns-configuration) ve gibi URL yolları `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US` burada bulunamacaktır, bu nedenle "bulunamadı" hatası (404).

Uygulamanızı "standart" örneklemesini "geri alma" `SpeechConfig`
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
Uygulamanız, kimlik doğrulama hatasıyla (401) sona erecek.

##### <a name="modifying-applications"></a>Uygulamaları değiştirme

Uygulamanızı özel uç noktaları olmadan özel etki alanı adı olan konuşma kaynağı senaryosuna yönelik olarak etkinleştirmek için aşağıdakileri yapmanız gerekir:
- Bilişsel hizmetler aracılığıyla yetkilendirme belirteci iste REST API
- "Yetkilendirme belirtecinden" `SpeechConfig` /"yetkilendirme belirteci ile" Using sınıfının örneğini oluşturma 

###### <a name="requesting-authorization-token"></a>Yetkilendirme belirteci isteniyor

Bilişsel hizmetler REST API belirteç alma hakkında bilgi için [Bu makaleye](../authentication.md#authenticate-with-an-authentication-token) bakın. 

Uç nokta URL 'sindeki özel etki alanı adınızı kullanın, bizim örneğimizde bu URL şu şekilde olur:
```http
https://my-private-link-speech.cognitiveservices.azure.com/sts/v1.0/issueToken
```
> [!TIP]
> Bu URL 'YI, Azure portal konuşma kaynağınızın *anahtarlar ve uç nokta* (*kaynak yönetimi* grubu) bölümünde bulabilirsiniz.

###### <a name="creating-speechconfig-instance-using-authorization-token"></a>`SpeechConfig`Yetkilendirme belirteci kullanılarak örnek oluşturma

`SpeechConfig`Önceki bölümde edindiğiniz yetkilendirme belirtecini kullanarak sınıf örneği oluşturmanız gerekir. Aşağıdaki değişkenlerin tanımlı olduğunu varsayalım:

- `token` önceki bölümde edinilen yetkilendirme belirtecini içeren
- `azureRegion` Konuşma kaynak [bölgesinin](regions.md) adını içeren (örnek: `westeurope` )
- `outError` (yalnızca [Amaç C](/objectivec/cognitive-services/speech/spxspeechconfiguration#initwithauthorizationtokenregionerror) Case için)

Daha sonra aşağıdaki gibi bir sınıf örneği oluşturmanız gerekir `SpeechConfig` :
```csharp
var config = SpeechConfig.FromAuthorizationToken(token, azureRegion);
```
```cpp
auto config = SpeechConfig::FromAuthorizationToken(token, azureRegion);
```
```java
SpeechConfig config = SpeechConfig.fromAuthorizationToken(token, azureRegion);
```
```python
import azure.cognitiveservices.speech as speechsdk
speech_config = speechsdk.SpeechConfig(auth_token=token, region=azureRegion)
```
```objectivec
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithAuthorizationToken:token region:azureRegion error:outError];
```
> [!NOTE]
> Çağıranın, yetkilendirme belirtecinin geçerli olduğundan emin olması gerekir. Yetkilendirme belirtecinin süresi dolmadan önce, çağıranın bu ayarlayıcısı yeni bir geçerli belirteçle çağırarak yenilemesi gerekir. Yeni bir tanıyıcı/birleştirici oluştururken yapılandırma değerleri kopyalanırken, yeni belirteç değeri zaten oluşturulmuş olan Tanıyıcılar için uygulanmaz. Daha önce oluşturulmuş Tanıyıcılar/seniciler için, belirteci yenilemek üzere ilgili tanıyıcı/birleştirici 'nin yetkilendirme belirtecini ayarlamanız gerekir. Aksi takdirde, Tanıyıcılar/seniciler, tanıma/birleştirme sırasında hatalarla karşılaşacaktır.

Bu değişiklikten sonra, uygulamanızın özel uç noktaları olmadan özel etki alanı adı etkinleştirilmiş konuşma kaynaklarıyla çalışması gerekir. Özel etki alanı/özel uç nokta senaryosu için sorunsuz destek üzerinde çalışıyoruz.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure özel bağlantısı](../../private-link/private-link-overview.md) hakkında daha fazla bilgi edinin
* [Konuşma SDK 'sı](speech-sdk.md) hakkında daha fazla bilgi
* [Konuşmayı metne REST API](rest-speech-to-text.md) hakkında daha fazla bilgi edinin
* [Metin okuma REST API](rest-text-to-speech.md) hakkında daha fazla bilgi edinin
