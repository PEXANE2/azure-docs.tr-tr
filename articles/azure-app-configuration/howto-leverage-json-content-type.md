---
title: Anahtar değerleri için JSON Content-Type kullanın
titleSuffix: Azure App Configuration
description: Anahtar değerler için JSON içerik türünü nasıl kullanacağınızı öğrenin
services: azure-app-configuration
author: avanigupta
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: how-to
ms.date: 08/03/2020
ms.author: avgupta
ms.openlocfilehash: 725beb50e55852e35ee4434539ff158f082059df
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122103"
---
# <a name="leverage-content-type-to-store-json-key-values-in-app-configuration"></a>Uygulama yapılandırmasındaki JSON anahtar değerlerini depolamak için içerik türünden yararlanın

Veriler, uygulama yapılandırmasında varsayılan olarak dize türü olarak kabul edilen anahtar-değer olarak depolanır. Ancak, her anahtar değeriyle ilişkili Content-Type özelliğinden yararlanarak özel bir tür belirtebilirsiniz. böylece verilerinizin orijinal türünü koruyabilir veya uygulamanızın içerik türüne göre farklı şekilde davranmasını sağlayabilirsiniz.


## <a name="overview"></a>Genel Bakış

Uygulama Yapılandırması ' nda JSON medya türünü anahtar değerlerinizin içerik türü olarak kullanabilirsiniz; örneğin:
- **Daha basit veri yönetimi**: diziler gibi anahtar değerlerini yönetme Azure Portal çok daha kolay olacaktır.
- **Geliştirilmiş veri dışa aktarma**: temel türler, DIZILER ve JSON nesneleri veri dışa aktarma sırasında korunur.
- **Uygulama yapılandırma sağlayıcısı Ile yerel destek**: JSON Content-Type ile anahtar değerleri, uygulamalarınızda uygulama yapılandırma sağlayıcısı kitaplıkları tarafından tüketildiği zaman iyi çalışacaktır.

#### <a name="valid-json-content-type"></a>Geçerli JSON içerik türü

[Burada](https://www.iana.org/assignments/media-types/media-types.xhtml)tanımlandığı gibi medya türleri, her anahtar değeriyle ilişkilendirilmiş içerik türüne atanabilir.
Medya türü bir tür ve alt tür içerir. Tür ise `application` ve alt tür ise (ya da sonek), `json` medya türü GEÇERLI bir JSON içerik türü olarak kabul edilir.
Geçerli JSON içerik türlerine örnek olarak şunlar verilebilir:

- uygulama/json
- uygulama/etkinlik + JSON
- application/vnd. foobar + JSON; charset = UTF-8

#### <a name="valid-json-values"></a>Geçerli JSON değerleri

Anahtar değerinde JSON içerik türü olduğunda, istemcilerinin doğru işlemesi için değeri geçerli bir JSON biçiminde olmalıdır. Aksi takdirde, istemciler başarısız olabilir veya geri dönemez ve dize biçimi olarak kabul edebilir.
Geçerli JSON değerlerinin bazı örnekleri şunlardır:

- "John Doe"
- 723
- yanlış
- null
- "2020-01-01T12:34:56.789 Z"
- [1, 2, 3, 4]
- {"ObjectSetting": {"hedefleme": {"default": true, "Level": "Information"}}}

> [!NOTE]
> Bu makalenin geri kalanında, geçerli bir JSON içerik türü ve geçerli bir JSON değeri olan uygulama yapılandırmasındaki anahtar-değer **JSON anahtar değeri**olarak adlandırılır. 

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:
> [!div class="checklist"]
> * Uygulama yapılandırmasında JSON anahtar değerleri oluşturun.
> * JSON anahtar değerlerini JSON dosyasından içeri aktarın.
> * JSON anahtar değerlerini bir JSON dosyasına dışarı aktarın.
> * Uygulamalarınızda JSON anahtar değerlerini tükettin.


## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
- Azure CLı 'nın en son sürümü (2.10.0 veya üzeri). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLı kullanıyorsanız, önce kullanarak oturum açmanız gerekir `az login` . İsteğe bağlı olarak Azure Cloud Shell kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]


## <a name="create-json-key-values-in-app-configuration"></a>Uygulama yapılandırmasında JSON anahtar değerleri oluşturma

JSON anahtar-değerleri Azure portal, Azure CLı kullanılarak veya bir JSON dosyasından içeri aktarılarak oluşturulabilir. Bu bölümde, tüm üç yöntemi kullanarak aynı JSON anahtar değerlerini oluşturma yönergelerini bulacaksınız.

### <a name="create-json-key-values-using-azure-portal"></a>Azure portal kullanarak JSON anahtar değerleri oluşturma

Uygulama yapılandırma deponuza gidin ve **Configuration Explorer**  >  **Create**  >  aşağıdaki anahtar-değer çiftlerini eklemek için yapılandırma Gezgini**anahtar değeri** oluştur ' u seçin:

| Anahtar | Değer | İçerik Türü |
|---|---|---|
| Ayarlar: BackgroundColor | Renkli | uygulama/json |
| Ayarlar: FontSize | 24 | uygulama/json |
| Ayarlar: UseDefaultRouting | yanlış | uygulama/json |
| Ayarlar: BlockedUsers | null | uygulama/json |
| Ayarlar: ReleaseDate | "2020-08-04T12:34:56.789 Z" | uygulama/json |
| Ayarlar: RolloutPercentage | [25, 50, 75100] | uygulama/json |
| Ayarlar: günlüğe kaydetme | {"Test": {"Level": "hata ayıklama"}, "üretim": {"Level": "uyarı"}} | uygulama/json |

**Etiketi** boş bırakın ve **Uygula**' yı seçin.

### <a name="create-json-key-values-using-azure-cli"></a>Azure CLı kullanarak JSON anahtar değerleri oluşturma

Aşağıdaki komutlar, uygulama yapılandırma deponuzda JSON anahtar değerleri oluşturacak. `<appconfig_name>`Uygulama yapılandırma deponuzın adıyla değiştirin.

```azurecli-interactive
appConfigName=<appconfig_name>
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:BackgroundColor --value \"Green\"
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:FontSize --value 24
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:UseDefaultRouting --value false
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:BlockedUsers --value null
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:ReleaseDate --value \"2020-08-04T12:34:56.789Z\"
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:RolloutPercentage --value [25,50,75,100]
az appconfig kv set -n $appConfigName --content-type application/json --key Settings:Logging --value {\"Test\":{\"Level\":\"Debug\"},\"Prod\":{\"Level\":\"Warning\"}}
```

> [!IMPORTANT]
> JSON anahtar değerlerini oluşturmak için Azure CLı veya Azure Cloud Shell kullanıyorsanız, belirtilen değerin bir kaçış JSON dizesi olması gerekir.

### <a name="import-json-key-values-from-a-file"></a>JSON anahtar değerlerini bir dosyadan içeri aktar

Aşağıdaki içerikle adlı bir JSON dosyası oluşturun `Import.json` ve uygulama yapılandırmasına anahtar değerleri olarak içeri aktarın:

```json
{
  "Settings": {
    "BackgroundColor": "Green",
    "BlockedUsers": null,
    "FontSize": 24,
    "Logging": {"Test":{"Level":"Debug"},"Prod":{"Level":"Warning"}},
    "ReleaseDate": "2020-08-04T12:34:56.789Z",
    "RolloutPercentage": [25,50,75,100],
    "UseDefaultRouting": false
  }
}
```

```azurecli-interactive
az appconfig kv import -s file --format json --path "~/Import.json" --content-type "application/json" --separator : --depth 2
```

> [!Note]
> `--depth`Bağımsız değişkeni bir dosyadaki hiyerarşik verilerin anahtar-değer çiftlerine düzleştirilmesi için kullanılır. Bu öğreticide, JSON nesnelerini uygulama yapılandırması 'nda değer olarak depolayabilmeniz için derinlik belirtilir. Derinlik belirtilmemişse, JSON nesneleri varsayılan olarak en derin düzeye düzleştirilir.

Oluşturduğunuz JSON anahtar değerleri, uygulama yapılandırmasında şöyle görünmelidir:

![JSON anahtar değerlerini içeren yapılandırma deposu](./media/create-json-settings.png)


## <a name="export-json-key-values-to-a-file"></a>JSON anahtar değerlerini bir dosyaya aktar

JSON anahtar değerlerini kullanmanın önemli avantajlarından biri, dışarı aktarılırken değerlerinizin özgün veri türünü koruyabilme yeteneğidir. Uygulama yapılandırmasındaki anahtar değerde JSON içerik türü yoksa, değeri dize olarak kabul edilir. 

Bu anahtar değerlerini JSON içerik türü olmadan göz önünde bulundurun:

| Anahtar | Değer | İçerik Türü |
|---|---|---|
| Ayarlar: FontSize | 24 | |
| Ayarlar: UseDefaultRouting | yanlış | |

Bu anahtar değerlerini bir JSON dosyasına aktardığınızda, değerler dizeler olarak dışarı aktarılabilir:
```json
{
  "Settings": {
    "FontSize": "24",
    "UseDefaultRouting": "false"
  }
}
```

Ancak, JSON anahtar değerlerini bir dosyaya aktardığınızda, tüm değerler özgün veri türlerini korur. Bunu doğrulamak için, uygulama yapılandırmanızda anahtar değerlerini bir JSON dosyasına dışarı aktarın. Aktarılan dosyanın, `Import.json` daha önce içe aktardığınız dosyayla aynı içeriğe sahip olduğunu görürsünüz.

```azurecli-interactive
az appconfig kv export -d file --format json --path "~/Export.json" --separator :
```

> [!NOTE]
> Uygulama yapılandırma deponuzda JSON içerik türü olmayan bazı anahtar değerleri varsa, bu değerler dize biçimindeki aynı dosyaya da aktarılabilir. Yalnızca JSON anahtar değerlerini dışarı aktarmak istiyorsanız, JSON anahtar değerleriniz için benzersiz bir etiket veya ön ek atayın ve dışa aktarma sırasında etiket veya önek filtrelemeyi kullanın.


## <a name="consuming-json-key-values-in-applications"></a>JSON anahtar değerlerini kullanan uygulamalar

Uygulamanızdaki JSON anahtar değerlerini kullanmanın en kolay yolu, uygulama yapılandırma sağlayıcısı kitaplıkları kullanmaktır. Sağlayıcı kitaplıkları ile uygulamanızdaki JSON anahtar değerlerini özel olarak işlemeyi uygulamanız gerekmez. Bunlar her zaman uygulamanız için diğer JSON yapılandırma sağlayıcısı kitaplıklarının yapacağından aynı şekilde seri durumdan çıkarılacağız. 

> [!Important]
> JSON anahtar değerleri için yerel destek .NET yapılandırma sağlayıcısı sürüm 4.0.0 (veya üstü) içinde kullanılabilir. Daha ayrıntılı bilgi için [*sonraki adımlar*](#next-steps) bölümüne bakın.

İçerik türü temelinde uygulama yapılandırmasından anahtar değerlerini okumak için SDK veya REST API kullanıyorsanız, uygulamanız herhangi bir standart JSON seri hale getirici kullanarak bir JSON anahtar-değer değerinin serisini kaldırmaktan sorumludur.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık uygulama yapılandırma deponuzdaki JSON anahtar değerleriyle nasıl çalışabildiğinizi öğrenmiş olduğunuza göre, bu anahtar değerlerini kullanmak için bir uygulama oluşturun:

* [Hızlı başlangıç ASP.NET Core](./quickstart-aspnet-core-app.md)
    * Önkoşul: [Microsoft. Azure. AppConfiguration. AspNetCore](https://www.nuget.org/packages/Microsoft.Azure.AppConfiguration.AspNetCore) Package v 4.0.0 veya üzeri.

* [.NET Core hızlı başlangıç](./quickstart-dotnet-core-app.md)
    * Önkoşul: [Microsoft.Extensions.Configurlama. AzureAppConfiguration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureAppConfiguration) paketi v 4.0.0 veya üzeri.
