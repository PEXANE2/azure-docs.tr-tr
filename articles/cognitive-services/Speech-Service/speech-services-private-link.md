---
title: Konuşma hizmeti ile özel uç noktaları kullanma
titleSuffix: Azure Cognitive Services
description: Azure özel bağlantı tarafından sunulan özel uç noktalarla konuşma hizmetini nasıl kullanacağınızı öğrenin
services: cognitive-services
author: alexeyo26
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: alexeyo
ms.openlocfilehash: d5822b6eeecfc61a5092519618ddfcaf88a625ae
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98018539"
---
# <a name="use-speech-service-through-a-private-endpoint"></a>Konuşma hizmetini özel bir uç nokta aracılığıyla kullanma

[Azure özel bağlantısı](../../private-link/private-link-overview.md) , [özel bir uç nokta](../../private-link/private-endpoint-overview.md)kullanarak Azure 'daki hizmetlere bağlanmanızı sağlar.
Özel uç nokta yalnızca belirli bir [sanal ağ](../../virtual-network/virtual-networks-overview.md) ve alt ağ içinden erişilebilen özel bir IP adresidir.

Bu makalede, Azure bilişsel konuşma Hizmetleri ile özel bağlantı ve özel uç noktaların nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır.

> [!NOTE]
> Bu makalede, Azure bilişsel konuşma Hizmetleri ile özel bağlantı ayarlama ve kullanma özellikleri açıklanmaktadır. Devam etmeden önce bilişsel [Hizmetler ile sanal ağları nasıl kullanacağınızı](../cognitive-services-virtual-networks.md)inceleyin.

Bir konuşma hizmetini özel bir uç nokta aracılığıyla kullanmak için aşağıdaki görevleri gerçekleştirin:

1. [Konuşma kaynağı özel etki alanı adı oluştur](#create-a-custom-domain-name)
2. [Özel uç nokta (ler) oluştur ve Yapılandır](#enable-private-endpoints)
3. [Mevcut uygulamaları ve çözümleri ayarla](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled)

Özel uç noktaları daha sonra kaldırmak, ancak konuşma kaynağını kullanmaya devam etmek için, [Bu bölümde](#use-speech-resource-with-custom-domain-name-without-private-endpoints)bulunan görevleri gerçekleştirirsiniz.

## <a name="create-a-custom-domain-name"></a>Özel etki alanı adı oluşturma

Özel uç noktalar bilişsel [Hizmetler özel alt etki alanı adı](../cognitive-services-custom-subdomains.md)gerektirir. Konuşma kaynağınız için bir tane oluşturmak için aşağıdaki yönergeleri izleyin.

> [!WARNING]
> Özel etki alanı adı etkinleştirilmiş bir konuşma kaynağı, konuşma hizmetiyle etkileşim kurmak için farklı bir yol kullanır.
> Büyük olasılıkla, özel [  uç nokta etkin](#use-speech-resource-with-custom-domain-name-without-private-endpoints) senaryolar için uygulama kodunuzu her iki [Özel uç nokta etkin](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled) olacak şekilde ayarlamanız gerekir.
>
> Özel bir etki alanı adını etkinleştirdiğinizde, işlem geri [**alınamaz**](../cognitive-services-custom-subdomains.md#can-i-change-a-custom-domain-name). [Bölgesel ada](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) geri dönmenin tek yolu yeni bir konuşma kaynağı oluşturmaktır.
>
> Konuşma kaynağınız çok sayıda ilişkili özel model ve proje içeriyorsa, [konuşma Studio](https://speech.microsoft.com/) aracılığıyla oluşturulan kaynağı değiştirmeden önce yapılandırmayı bir test kaynağıyla denemeyi **önemle** öneririz.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal kullanarak özel bir etki alanı adı oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) gidin ve Azure hesabınızda oturum açın.
1. Gerekli konuşma kaynağını seçin.
1. Sol gezinti bölmesindeki **kaynak yönetimi** grubunda, **ağ**' a tıklayın.
1. **Güvenlik duvarları ve sanal ağlar** sekmesinde **özel etki alanı adı oluştur**' a tıklayın. Kaynağınız için benzersiz bir özel alt etki alanı oluşturma yönergeleriyle birlikte yeni bir sağ panel görüntülenir.
1. Özel etki alanı adı Oluştur panelinde, özel bir etki alanı adı bölümü girin. Tam özel etki alanınız şöyle görünür: `https://{your custom name}.cognitiveservices.azure.com` . 
    **Özel bir etki alanı adı oluşturduktan sonra _, değiştirilemez!_ Yukarıdaki uyarı uyarısını yeniden okuyun.** Özel etki alanı adınızı girdikten sonra **Kaydet**' e tıklayın.
1. İşlem tamamlandıktan sonra, **kaynak yönetimi** grubunda **anahtarlar ve uç nokta**' ı tıklatın. Kaynağınızın yeni uç nokta adının bu şekilde başlatıldığını onaylayın:

    `https://{your custom name}.cognitiveservices.azure.com`

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell kullanarak özel bir etki alanı adı oluşturmak için, bilgisayarınızda Azure PowerShell Module Version 5.1.0 veya üzeri ile PowerShell sürüm 7. x veya üzeri olduğunu doğrulayın. Bu araçların sürümlerini görmek için şu adımları izleyin:

1. Bir PowerShell penceresinde, şunu yazın:

    `$PSVersionTable`

    PSVersion değerinin 7. x değerinden büyük olduğunu onaylayın. PowerShell 'i yükseltmek için, yükseltmek üzere [çeşitli PowerShell sürümlerini yükleme](/powershell/scripting/install/installing-powershell) konusundaki yönergeleri izleyin.

1. Bir PowerShell penceresinde, şunu yazın:

    `Get-Module -ListAvailable Az`

    Hiçbir şey yoksa veya Azure PowerShell modül sürümü 5.1.0 ' den düşükse, yükseltmek için [Azure PowerShell modülünü Install](/powershell/azure/install-Az-ps) bölümündeki yönergeleri izleyin.

Devam etmeden önce, `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

## <a name="verify-custom-domain-name-is-available"></a>Özel etki alanı adının kullanılabilir olduğunu doğrulayın

Kullanmak istediğiniz özel etki alanının kullanılabilir olup olmadığını denetleyin. Bilişsel hizmetler REST API [etki alanı kullanılabilirliğini denetle](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) işlemini kullanarak etki alanının kullanılabilir olduğunu doğrulamak için aşağıdaki adımları izleyin.

> [!TIP]
> Aşağıdaki kod Azure Cloud Shell içinde **çalışmaz.**

```azurepowershell
$subId = "Your Azure subscription Id"
$subdomainName = "custom domain name"

# Select the Azure subscription that contains Speech resource.
# You can skip this step if your Azure account has only one active subscription.
Set-AzContext -SubscriptionId $subId

# Prepare OAuth token to use in request to Cognitive Services REST API.
$Context = Get-AzContext
$AccessToken = (Get-AzAccessToken -TenantId $Context.Tenant.Id).Token
$token = ConvertTo-SecureString -String $AccessToken -AsPlainText -Force

# Prepare and send the request to Cognitive Services REST API.
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
İstenen ad varsa şöyle bir yanıt görürsünüz:
```azurepowershell
isSubdomainAvailable : True
reason               :
type                 :
subdomainName        : my-custom-name
```
Ad zaten alınmış ise, aşağıdaki yanıtı görürsünüz:
```azurepowershell
isSubdomainAvailable : False
reason               : Sub domain name 'my-custom-name' is already used. Please pick a different name.
type                 :
subdomainName        : my-custom-name
```
## <a name="create-your-custom-domain-name"></a>Özel etki alanı adınızı oluşturma

Seçili konuşma kaynağı için özel etki alanı adını etkinleştirmek üzere [set-azbiliveservicesaccount](/powershell/module/az.cognitiveservices/set-azcognitiveservicesaccount) cmdlet 'ini kullanırız.

> [!WARNING]
> Aşağıdaki kod başarıyla çalıştıktan sonra, konuşma kaynağınız için özel bir etki alanı adı oluşturacaksınız.
> Bu ad **değiştirilemez** . Yukarıdaki **Uyarı** uyarısında daha fazla bilgi görüntüleyin.

```azurepowershell
$resourceGroup = "Resource group name where Speech resource is located"
$speechResourceName = "Your Speech resource name"
$subdomainName = "custom domain name"

# Select the Azure subscription that contains Speech resource.
# You can skip this step if your Azure account has only one active subscription.
$subId = "Your Azure subscription Id"
Set-AzContext -SubscriptionId $subId

# Set the custom domain name to the selected resource.
# WARNING: THIS CANNOT BE CHANGED OR UNDONE!
Set-AzCognitiveServicesAccount -ResourceGroupName $resourceGroup `
    -Name $speechResourceName -CustomSubdomainName $subdomainName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu bölüm, Azure CLı 'nın en son sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="verify-the-custom-domain-name-is-available"></a>Özel etki alanı adının kullanılabilir olduğunu doğrulayın

Kullanmak istediğiniz özel etki alanının serbest olup olmadığını denetleyin. Bilişsel hizmetler REST API, [etki alanı kullanılabilirliği yöntemini denetle](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) 'yi kullanacağız.

Aşağıdaki kod bloğunu kopyalayın, tercih ettiğiniz özel etki alanı adınızı ekleyin ve dosyaya kaydedin `subdomain.json` .

```json
{
    "subdomainName": "custom domain name",
    "type": "Microsoft.CognitiveServices/accounts"
}
```

Dosyayı geçerli klasörünüze kopyalayın veya Azure Cloud Shell karşıya yükleyin ve aşağıdaki komutu çalıştırın. ( `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` Azure ABONELIK Kimliğinizle değiştirin).

```azurecli-interactive
az rest --method post --url "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18" --body @subdomain.json
```
İstenen ad varsa şöyle bir yanıt görürsünüz:
```azurecli
{
  "isSubdomainAvailable": true,
  "reason": null,
  "subdomainName": "my-custom-name",
  "type": null
}
```

Ad zaten alınmış ise, aşağıdaki yanıtı görürsünüz:
```azurecli
{
  "isSubdomainAvailable": false,
  "reason": "Sub domain name 'my-custom-name' is already used. Please pick a different name.",
  "subdomainName": "my-custom-name",
  "type": null
}
```
## <a name="enable-custom-domain-name"></a>Özel etki alanı adını etkinleştir

Seçili konuşma kaynağı için özel etki alanı adını etkinleştirmek üzere [az biliveservices hesabı Güncelleştir](/cli/azure/cognitiveservices/account#az_cognitiveservices_account_update) komutunu kullanırız.

Konuşma kaynağını içeren Azure aboneliğini seçin. Azure hesabınızda yalnızca bir etkin abonelik varsa, bu adımı atlayabilirsiniz. ( `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` Azure ABONELIK Kimliğinizle değiştirin).
```azurecli-interactive
az account set --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
Özel etki alanı adını seçili kaynak olarak ayarlayın. Örnek parametre değerlerini gerçek olanlarla değiştirin ve aşağıdaki komutu çalıştırın.

> [!WARNING]
> Aşağıdaki komutu başarıyla yürütmeden sonra, konuşma kaynağınız için özel bir etki alanı adı oluşturacaksınız. Bu ad **değiştirilemez** . Yukarıdaki uyarı uyarısıyla daha fazla bilgi görüntüleyin.

```azurecli
az cognitiveservices account update --name my-speech-resource-name --resource-group my-resource-group-name --custom-domain my-custom-name
```

**_

## <a name="enable-private-endpoints"></a>Özel uç noktaları etkinleştir

Özel uç noktalar için gerekli güncelleştirmelerle sanal ağa bağlı [özel DNS bölgesini](../../dns/private-dns-overview.md) kullanmanızı öneririz. Bu işlem, sağlama işlemi sırasında varsayılan olarak oluşturulur. Ancak, kendi DNS sunucunuzu kullanıyorsanız, aşağıdaki _Özel uç noktalar Için DNS_' de GÖSTERILDIĞI gibi DNS yapılandırmanızı de değiştirmeniz gerekebilir. , Özellikle kendi DNS sunucunuzu kullanıyorsanız, bir üretim konuşma kaynağı *için * özel* uç nokta (ler) sağlama ve DNS değişikliklerinizi test etme konusunda, DNS stratejisi _ ' i belirleyin.

Özel uç noktaları oluşturmak için aşağıdaki makalelerden birini kullanın. Makaleler, Özel uç noktalarla olanak tanımak için bir Web uygulamasını örnek kaynak olarak kullanır. Bu parametreleri makalesindeki parametreler yerine kullanacaksınız:

| Ayar             | Değer                                    |
|---------------------|------------------------------------------|
| Kaynak türü       | **Microsoft. Biliveservices/hesapları** |
| Kaynak            | **\<your-speech-resource-name>**         |
| Hedef alt kaynak | **açın**                              |

- [Azure portalını kullanarak Özel Uç Nokta oluşturma](../../private-link/create-private-endpoint-portal.md)
- [Azure PowerShell kullanarak özel uç nokta oluşturma](../../private-link/create-private-endpoint-powershell.md)
- [Azure CLı kullanarak özel uç nokta oluşturma](../../private-link/create-private-endpoint-cli.md)

**Özel uç noktalar Için DNS:** Bilişsel [Hizmetler kaynaklarındaki özel uç noktalar Için DNS](../cognitive-services-virtual-networks.md#dns-changes-for-private-endpoints)genel ilkelerini gözden geçirin. Ardından, bu denetimleri gerçekleştirerek DNS yapılandırmanızın düzgün çalıştığını doğrulayın:

### <a name="resolve-dns-from-the-virtual-network"></a>DNS 'i sanal ağdan çözümle

Bu denetim **gereklidir**.

Özel DNS girişini sanal ağınızdan test etmek için aşağıdaki adımları izleyin.

1. Özel uç noktanıza eklediğiniz sanal ağda bulunan bir sanal makinede oturum açın. 
1. Windows komut Istemi veya bash kabuğu 'nu açın, çalıştırın `nslookup` ve kaynak özel etki alanı adınızı başarıyla çözümleyip onaylayın.

```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  168.63.129.16

Non-authoritative answer:
Name:    my-private-link-speech.privatelink.cognitiveservices.azure.com
Address:  172.28.0.10
Aliases:  my-private-link-speech.cognitiveservices.azure.com
```

3. IP adresinin özel uç noktanızın IP adresiyle eşleştiğinden emin olun.

### <a name="resolve-dns-from-other-networks"></a>Diğer ağlardan DNS 'i çözümleme

Bu denetimi yalnızca, kaynağınızın **ağ** bölümünde **tüm ağlar** veya **Seçili ağlar ve özel uç noktalar** erişim seçeneğini etkinleştirdiğiniz "karma" modda özel uç nokta etkin konuşma kaynağını kullanmayı planlıyorsanız gerçekleştirin. Kaynağa yalnızca özel bir uç nokta kullanarak erişmeyi planlıyorsanız, bu bölümü atlayabilirsiniz.

1. Kaynağa erişim izni verilen bir ağa bağlı bir bilgisayarda oturum açın.
2. Windows komut Istemi veya bash kabuğu 'nu açın, çalıştırın `nslookup` ve kaynak özel etki alanı adınızı başarıyla çözümleyip onaylayın.

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

3. IP adresinin özel uç noktanızın IP adresiyle eşleştiğinden emin olun.

> [!NOTE]
> Çözümlenen IP adresi, ağ trafiğini bilişsel hizmetler kaynağı için özel uç noktaya bağlayan bir sanal ağ proxy uç noktasını işaret eder. Özel bir etki alanı adı olan ancak özel uç noktaları *olmayan* bir kaynak için davranış farklı olacaktır. Ayrıntılar için [Bu bölüme](#dns-configuration) bakın.

## <a name="adjust-existing-applications-and-solutions"></a>Mevcut uygulamaları ve çözümleri ayarla

Özel etki alanı etkinleştirilmiş bir konuşma kaynağı, konuşma hizmetleriyle etkileşim kurmak için farklı bir yol kullanır. Bu, özel bir etki alanı etkin konuşma kaynağı için, hem hem de özel uç noktaları olmadan geçerlidir. Bu bölümdeki bilgiler her iki senaryo için de geçerlidir.

### <a name="use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled"></a>Özel etki alanı adı ve özel uç nokta etkinken konuşma kaynağını kullanma

Özel etki alanı adı ve özel uç noktası etkin bir konuşma kaynağı, konuşma hizmetleriyle etkileşmek için farklı bir yol kullanır. Bu bölümde, söz konusu kaynağın konuşma Hizmetleri REST API ve [konuşma SDK 'sı](speech-sdk.md)ile nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Özel uç noktaları olmayan ancak **özel etki alanı adı** etkinleştirilmiş bir konuşma kaynağının, konuşma hizmetleriyle birlikte etkileşimde bulunmak için özel bir yöntemi olduğunu lütfen unutmayın, ancak bu yöntem özel bir uç nokta etkin konuşma kaynağının senaryosundan farklıdır. Bu kaynakta (örneğin, Özel uç noktaları olan bir kaynağınız vardı, ancak bunları kaldırmaya karar verdiyseniz), ilgili [bölümü](#use-speech-resource-with-custom-domain-name-without-private-endpoints)öğrendiğinizden emin olun.

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
Bu URL 'ye, Özel uç nokta eklenmiş olan sanal ağdan ulaşılamamalıdır ( [doğru DNS çözümlemesi](#resolve-dns-from-the virtual-network)sağlanmış).

Genellikle, bir konuşma kaynağı için özel etki alanı adı etkinleştirildikten sonra, tüm istek URL 'Lerinde ana bilgisayar adını yeni özel etki alanı ana bilgisayar adı ile değiştirirsiniz. İsteğin diğer tüm bölümleri ( `/speechtotext/v3.0/transcriptions` Yukarıdaki örnekteki yol gibi) aynı kalır.

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
> Özel uç nokta senaryolarında **kısa ses için konuşmayı metne REST API** kullanırken, üst bilgiyle [geçirilmiş](rest-speech-to-text.md#request-headers) bir yetkilendirme belirteci kullanın `Authorization` [](rest-speech-to-text.md#request-headers). Üstbilgi aracılığıyla özel uç noktaya konuşma abonelik anahtarı geçirilmeyecektir `Ocp-Apim-Subscription-Key` ve  401 hatası oluşturacaktır.

**Metin okuma REST API kullanım örneği.**

Batı Avrupa, örnek bir Azure bölgesi olarak ve `my-private-link-speech.cognitiveservices.azure.com` örnek bir konuşma kaynağı DNS adı (özel etki alanı) olarak kullanacağız. Örneğimizde özel etki alanı adı `my-private-link-speech.cognitiveservices.azure.com` , Batı Avrupa bölgesinde oluşturulan konuşma kaynağına aittir.

Tek bir bölgede desteklenen seslerin listesini almak için aşağıdaki iki işlemi yapmanız gerekir:

- Yetkilendirme belirteci al:
```http
https://westeurope.api.cognitive.microsoft.com/sts/v1.0/issuetoken
```
- Belirteci kullanarak, seslerin listesini alın:
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

Özel etki alanı adı ve özel uç nokta etkin konuşma kaynaklarıyla konuşma SDK 'sının kullanılması, uygulama kodunuzun gözden geçirilmesi ve olası değişiklikleri gerektirir.

`my-private-link-speech.cognitiveservices.azure.com`Bu bölüm için örnek bir konuşma kaynağı DNS adı (özel etki alanı) olarak kullanacağız.

##### <a name="general-principle"></a>Genel ilke

Genellikle SDK senaryolarında (Ayrıca, metin okuma REST API senaryolarında) konuşma kaynakları, farklı hizmet teklifleri için adanmış bölgesel uç noktaları kullanır. Bu uç noktaların DNS ad biçimi: </p>`{region}.{speech service offering}.speech.microsoft.com`

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

Özel uç nokta etkin uç noktalar, özel bir ara sunucu aracılığıyla konuşma hizmetleriyle iletişim kurar ve **uç nokta bağlantı URL 'lerini değiştirmeniz gerekir**. 

"Standart" uç nokta URL 'SI şöyle görünür: <p/>`{region}.{speech service offering}.speech.microsoft.com/{URL path}`

Özel bir uç nokta URL 'SI şöyle görünür: <p/>`{your custom name}.cognitiveservices.azure.com/{speech service offering}/{URL path}`

**Örnek 1.** Uygulama aşağıdaki URL 'YI kullanarak iletişim kuruyor (Batı Avrupa için ABD Ingilizcesi için temel modeli kullanan konuşma tanıma):

```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Konuşma kaynağının özel etki alanı adı olduğunda bunu özel uç nokta etkin senaryosunda kullanmak için `my-private-link-speech.cognitiveservices.azure.com` URL 'yi şöyle değiştirmeniz gerekir:

```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Ayrıntılara dikkat edin:

- Ana bilgisayar adı, `westeurope.stt.speech.microsoft.com` özel etki alanı ana bilgisayar adı ile değiştirilmiştir `my-private-link-speech.cognitiveservices.azure.com` .
- Özgün DNS adının ikinci öğesi (), `stt` URL yolunun ilk öğesi olur ve özgün yoldan önce gelir. Yani özgün URL `/speech/recognition/conversation/cognitiveservices/v1?language=en-US` olur `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US` .

**Örnek 2.** Uygulama, Batı Avrupa özel bir ses modeli kullanarak konuşmayı sentezleştirmek için aşağıdaki URL 'YI kullanır):
```http
https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```

Aşağıda, konuşma kaynağının özel etki alanı adının kullanıldığı özel bir uç nokta kullanan eşdeğer bir URL verilmiştir `my-private-link-speech.cognitiveservices.azure.com` :

```http
https://my-private-link-speech.cognitiveservices.azure.com/voice/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```

Örnek 1 ' deki aynı ilke uygulanır, ancak bu zaman anahtar öğesi `voice` .

##### <a name="modify-applications"></a>Uygulamaları değiştirme

Kodunuzu değiştirmek için şu adımları izleyin:

**1. uygulama uç noktası URL 'sini belirleme**

- [Uygulamanız için günlüğe kaydetmeyi etkinleştirin](how-to-use-logging.md) ve günlüğe kaydetmek için çalıştırın.
- Günlük dosyasında, araması yapın `SPEECH-ConnectionUrl` . Eşleşen satırlarda `value` parametresi, uygulamanızın konuşma hizmetine erişmek için kullandığı tam URL 'yi içerir.

Örnek:

```
(114917): 41ms SPX_DBG_TRACE_VERBOSE:  property_bag_impl.cpp:138 ISpxPropertyBagImpl::LogPropertyAndValue: this=0x0000028FE4809D78; name='SPEECH-ConnectionUrl'; value='wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?traffictype=spx&language=en-US'
```

Bu nedenle, uygulama tarafından kullanılan URL Bu örnekteki:

```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

**2. `SpeechConfig` tam uç nokta URL 'sini kullanarak örnek oluşturun**

Yukarıdaki [genel prensibi](#general-principle) bölümünde açıklandığı gibi, önceki bölümde belirlediğiniz uç noktayı değiştirin.

Şimdi örneğini oluşturma şeklini değiştirin `SpeechConfig` . Bugünün uygulamanız şuna benzer olabilir:
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
Bu, önceki bölümlerde açıklandığımız ana bilgisayar adı ve URL değişikliklerinden dolayı özel uç nokta özellikli konuşma kaynağı için çalışmayacaktır. Mevcut uygulamanızı özel bir uç noktası etkin bir kaynağın anahtarını kullanarak herhangi bir değişiklik yapmadan çalıştırmaya çalışırsanız, kimlik doğrulama hatası alırsınız (401).

Bu işlemi gerçekleştirmek için, sınıfın örneğini oluşturma ve "bitiş noktası" ve " `SpeechConfig` bitiş noktası" başlatmasının nasıl kullanılacağı ile değiştirin. Aşağıdaki iki değişkenin tanımlı olduğunu varsayalım:
- `subscriptionKey` Özel uç nokta etkin konuşma kaynağının anahtarını içeren
- `endPoint` Tam **değiştirilen** uç nokta URL 'sini (karşılık gelenprogramlama dili için gereken türü kullanarak) içerir. Örneğimizde bu değişkenin şunu içermesi gerekir
```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Sonra bir örnek oluşturun `SpeechConfig` :
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
> Uç nokta URI 'sinde belirtilen sorgu parametreleri diğer API 'Ler tarafından ayarlansalar bile değiştirilmez. Örneğin, tanıma dili URI 'de "Language = en-US" sorgu parametresi olarak tanımlandıysa ve aynı zamanda ilgili özelliği aracılığıyla "ru-RU" olarak ayarlanırsa, URI 'deki dil ayarı kullanılır ve geçerli dil "en-US" dir. Uç nokta URI 'sinde ayarlanan parametreler her zaman ön tanımlayıcı alır. Yalnızca uç nokta URI 'sinde belirtilmeyen parametreler diğer API 'Ler tarafından geçersiz kılınabilir.

Bu değişiklikten sonra uygulamanızın özel özellikli konuşma kaynaklarıyla çalışması gerekir. Özel uç nokta senaryosuna yönelik daha sorunsuz destek üzerinde çalışıyoruz.

### <a name="use-speech-resource-with-custom-domain-name-without-private-endpoints"></a>Özel uç noktaları olmadan özel etki alanı adıyla konuşma kaynağını kullanma

Bu makalede, bir konuşma kaynağı için özel etki alanını etkinleştiren geri **alınamaz** ve söz konusu kaynak, konuşma hizmetleriyle iletişim kurarak "olağan dışı" ( [Bölgesel uç noktası adlarını](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints)kullanan) ile karşılaştırıldığında farklı bir iletişim yöntemi kullanacaktır.

Bu bölümde, bir konuşma kaynağının özel etki alanı adı ve konuşma Hizmetleri REST API ve [konuşma SDK 'sı](speech-sdk.md)ile özel uç noktaları **olmadan** nasıl kullanılacağı açıklanmaktadır. Bu, bir özel uç nokta senaryosunda kullanılan, ancak özel uç noktaları silinen bir kaynak olabilir.

#### <a name="dns-configuration"></a>DNS yapılandırması

Özel uç nokta etkin konuşma kaynağının özel etki alanı DNS adının [Ortak ağlardan nasıl çözümlendiğini](#resolve-dns-from-other-networks)unutmayın. Bu durumda IP adresi, ağ trafiğini özel uç nokta etkin bilişsel hizmetler kaynağına dağıtırken kullanılan bir VNet proxy uç noktasına işaret eder.

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
[Bu bölümün](#resolve-dns-from-other-networks)çıkışıyla karşılaştırın.

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-rest-api"></a>Özel uç noktaları olmadan özel etki alanı adına sahip konuşma kaynağı. REST API kullanımı

##### <a name="speech-to-text-rest-api-v30"></a>Konuşmayı metne REST API v 3.0

Konuşmadan metne REST API v 3.0 kullanımı, [Özel uç nokta özellikli konuşma kaynakları](#speech-to-text-rest-api-v30)için tam olarak eşdeğerdir.

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>Kısa ses ve metinden konuşmaya REST API için konuşmadan metne REST API

Bu durumda, kısa ses ve metinden konuşmaya REST API kullanım için konuşmadan metne REST API, kısa bir ses için konuşmaya metin REST API için tek bir özel durum ile genel durumda bir farklılık yoktur (aşağıdaki nota bakın). Her iki API de, kısa ses ve [metinden konuşmaya REST API](rest-text-to-speech.md) belgeleri [için konuşmadan metne REST API](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) açıklandığı gibi kullanılmalıdır.

> [!NOTE]
> Özel etki alanı senaryolarında **kısa ses için konuşmayı metne REST API** kullanırken, üst bilgiyle [geçirilmiş](rest-speech-to-text.md#request-headers) bir yetkilendirme belirteci kullanın `Authorization` [](rest-speech-to-text.md#request-headers). Üstbilgi aracılığıyla özel uç noktaya konuşma abonelik anahtarı geçirilmeyecektir `Ocp-Apim-Subscription-Key` ve  401 hatası oluşturacaktır.

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

Uygulamanızın özel bir etki alanı adı ve özel uç noktaları olmadan bir konuşma kaynağı kullanmasına izin vermek için aşağıdaki adımları izleyin:

**1. bilişsel hizmetlerden kimlik doğrulama belirteci iste REST API**

[Bu makalede](../authentication.md#authenticate-with-an-authentication-token) bilişsel hizmetler REST API kullanarak belirtecin nasıl alınacağı gösterilmektedir.

Uç nokta URL 'sindeki özel etki alanı adınızı kullanın, bizim örneğimizde bu URL şu şekilde olur:
```http
https://my-private-link-speech.cognitiveservices.azure.com/sts/v1.0/issueToken
```
> [!TIP]
> Bu URL 'YI Azure portal bulabilirsiniz. Konuşma kaynağı sayfanızda, **kaynak yönetimi** grubunun altında **anahtarlar ve uç nokta**' ı seçin.

**2. "yetkilendirme belirtecinden" `SpeechConfig` /"yetkilendirme belirteci ile" kullanarak bir örnek oluşturun.**

`SpeechConfig`Önceki bölümde edindiğiniz yetkilendirme belirtecini kullanarak bir örnek oluşturun. Aşağıdaki değişkenlerin tanımlı olduğunu varsayalım:

- `token`: önceki bölümde edinilen yetkilendirme belirteci
- `azureRegion`: konuşma kaynak [bölgesinin](regions.md) adı (örnek: `westeurope` )
- `outError`: (yalnızca [Amaç C](/objectivec/cognitive-services/speech/spxspeechconfiguration#initwithauthorizationtokenregionerror) Case için)

Sonra bir örnek oluşturun `SpeechConfig` :

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
> Çağıranın, yetkilendirme belirtecinin geçerli olduğundan emin olması gerekir.
> Yetkilendirme belirtecinin süresi dolmadan önce, çağıranın bu ayarlayıcısı yeni bir geçerli belirteçle çağırarak yenilemesi gerekir.
> Yeni bir tanıyıcı veya birleştirici oluştururken yapılandırma değerleri kopyalanırken, yeni belirteç değeri zaten oluşturulmuş Tanıyıcılar veya birleştiriciler için uygulanmaz.
> Bunlar için, belirteci yenilemek üzere ilgili tanıyıcı veya birleştirici 'nin yetkilendirme belirtecini ayarlayın.
> Belirteci yenilemezseniz tanıyıcı veya birleştirici çalışırken hatalarla karşılaşacaktır.

Bu değişiklikten sonra, uygulamanızın özel uç noktaları olmadan özel bir etki alanı adı kullanan konuşma kaynaklarıyla çalışması gerekir.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link).

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Azure Özel Bağlantı](../../private-link/private-link-overview.md)
* [Konuşma SDK'sı](speech-sdk.md)
* [Konuşmayı metne dönüştürme REST API'si](rest-speech-to-text.md)
* [Metin okuma REST API'si](rest-text-to-speech.md)
