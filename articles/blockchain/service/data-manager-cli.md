---
title: Azure CLI - Azure Blockchain Hizmeti kullanarak Blockchain Veri Yöneticisini Yapılandırma
description: Azure CLI'yi kullanarak Azure Blockchain Hizmeti için Bir Blockchain Veri Yöneticisi oluşturma ve yönetme
ms.date: 03/30/2020
ms.topic: article
ms.reviewer: ravastra
ms.openlocfilehash: e490803fabeed7d6234bd6984acbfb9f5270e0c0
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81254419"
---
# <a name="configure-blockchain-data-manager-using-azure-cli"></a>CLI kullanarak Blok Zinciri Veri Yöneticisi'ni yapılandırma

Blockchain verilerini yakalamak için Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi'ni yapılandırın ve bu bilgileri bir Azure Olay Izgara Konusuna gönderin.

Blockchain Veri Yöneticisi örneğini yapılandırmak için şunları

* Blockchain Yöneticisi örneği oluşturma
* Azure Blockchain Hizmeti işlem düğümüne giriş oluşturma
* Azure Olay Izgara Konusuna çıktı oluşturma
* Blockchain uygulaması ekleme
* Örnek başlatma

## <a name="prerequisites"></a>Ön koşullar

* En son [Azure CLI'sini](https://docs.microsoft.com/cli/azure/install-azure-cli) yükleyin ve kullanarak `az login`oturum açtı.
* Tam [Quickstart: Azure Blockchain Hizmeti konsorsiyum ağına bağlanmak için Visual Studio Kodunu kullanın.](connect-vscode.md) Blockchain Data Manager kullanılırken Azure Blockchain Service *Standard* katmanı önerilir.
* Olay [Izgara Konusu](../../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) Oluşturma
* Azure [Etkinlik Kılavuz'undaki Olay işleyicileri](../../event-grid/event-handlers.md) hakkında bilgi edinin

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/bash](https://shell.azure.com/bash)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yüklemeyi ve kullanmayı tercih ederseniz, bu hızlı başlatma azure CLI sürümü 2.0.51 veya sonrası gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye](https://docs.microsoft.com/cli/azure/install-azure-cli)bakın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myRG --location eastus
```

## <a name="create-instance"></a>Örnek oluşturma

Blockchain Veri Yöneticisi örneği, Azure Blockchain Hizmeti hareket düğümünü izler. Bir örnek, işlem düğümündeki tüm ham blok ve ham hareket verilerini yakalar. Blockchain Data Manager, web3.eth [getBlock](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html#getblock) ve [getTransaction](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html#gettransaction) sorgularından döndürülen bilgilerin bir üst kümesi olan **rawblockAndTransactionMsg** iletisi yayınlar.

``` azurecli
az resource create \
                   --resource-group <Resource group> \
                   --name <Blockchain Data Manager instance name> \
                   --resource-type Microsoft.blockchain/watchers \
                   --is-full-object \
                   --properties <watcher resource properties>
```

| Parametre | Açıklama |
|-----------|-------------|
| resource-group | Blockchain Veri Yöneticisi örneğinin oluşturulacağı kaynak grubu adı. |
| ad | Blockchain Veri Yöneticisi örneğinin adı. |
| kaynak türü | Blockchain Veri Yöneticisi örneği için kaynak türü **Microsoft.blockchain/watchers'dır.** |
| tam nesne | Özellikleri gösterir izleyici kaynağı için seçenekler içerir. |
| properties | İzleyici kaynağıiçin özellikler içeren JSON biçimlendirilmiş dize. Bir dize veya dosya olarak geçirilebilir.  |

### <a name="create-instance-examples"></a>Örnek örnekleri oluşturma

**Doğu ABD** bölgesinde bir Blockchain Yöneticisi örneği oluşturmak için JSON yapılandırma örneği.

``` json
{
    "location": "eastus",
    "properties": {
    }
}
```

| Öğe | Açıklama |
|---------|-------------|
| location | İzleyici kaynağının oluşturulacağı bölge |
| properties | İzleyici kaynağı oluştururken ayarlatacak özellikler |

Yapılandırma için JSON dizesini kullanarak *mywatcher* adlı bir Blockchain Veri Yöneticisi örneği oluşturun.

``` azurecli-interactive
az resource create \
                    --resource-group myRG \
                    --name mywatcher \
                    --resource-type Microsoft.blockchain/watchers \
                     --is-full-object \
                     --properties '{"location":"eastus"}'
```

JSON yapılandırma dosyalarını kullanarak *mywatcher* adlı bir Blockchain Veri Yöneticisi örneği oluşturun.

``` azurecli
az resource create \
                    --resource-group myRG \
                    --name mywatcher \
                    --resource-type Microsoft.blockchain/watchers \
                    --is-full-object \
                    --properties @watcher.json
```

## <a name="create-input"></a>Giriş oluşturma

Giriş, Blockchain Veri Yöneticisi'ni Azure Blockchain Hizmeti işlem düğümüne bağlar. Yalnızca işlem düğümüne erişimi olan kullanıcılar bir bağlantı oluşturabilir.

``` azurecli
az resource create \
                   --resource-group <Resource group> \
                   --name <Input name> \
                   --namespace Microsoft.Blockchain \
                   --resource-type inputs \
                   --parent watchers/<Watcher name> \
                   --is-full-object \
                   --properties <input resource properties>
```

| Parametre | Açıklama |
|-----------|-------------|
| resource-group | Kaynak grubu adı nerede giriş kaynağı oluşturmak için. |
| ad | Girişin adı. |
| ad alanı | **Microsoft.Blockchain** sağlayıcı ad alanını kullanın. |
| kaynak türü | Blockchain Data Manager girişi için kaynak türü **girişleridir.** |
| Üst | Girişin ilişkili olduğu izleyiciye giden yol. Örneğin, **izleyiciler / mywatcher**. |
| tam nesne | Özellikleri gösterir giriş kaynağı için seçenekler içerir. |
| properties | Giriş kaynağı için özellikler içeren JSON biçimlendirilmiş dize. Bir dize veya dosya olarak geçirilebilir. |

### <a name="input-examples"></a>Giriş örnekleri

Blockchain üyesine\>bağlı *Doğu ABD* bölgesinde bir giriş kaynağı \<oluşturmak için Yapılandırma JSON örneği.

``` json
{
    "location": "eastus",
    "properties": {
        "inputType": "Ethereum",
        "dataSource": {
            "resourceId": "/subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Blockchain/blockchainMembers/<Blockchain member>/transactionNodes/transaction-node"
        }
    }
}
```

| Öğe | Açıklama |
|---------|-------------|
| location | Giriş kaynağının oluşturulacağı bölge. |
| inputType | Azure Blockchain Service üyesinin genel muhasebe türü. Şu anda, **Ethereum** desteklenir. |
| resourceId | Girişin bağlı olduğu hareket düğümü. Abonelik \<\>Kimliği, \<Kaynak\>grubu \<ve\> Blockchain üyesini hareket düğümü kaynağının değerleriyle değiştirin. Giriş, Azure Blockchain Hizmeti üyesi için varsayılan işlem düğümüne bağlanır. |

Yapılandırma için Bir JSON dizesini kullanarak *myWatcher* için *myInput* adlı bir giriş oluşturun.

``` azurecli-interactive
az resource create \
                   --resource-group myRG \
                   --name myInput \
                   --namespace Microsoft.Blockchain \
                   --resource-type inputs \
                   --parent watchers/mywatcher \
                   --is-full-object \
                   --properties '{"location":"eastus", "properties":{"inputType":"Ethereum","dataSource":{"resourceId":"/subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Blockchain/BlockchainMembers/<Blockchain member>/transactionNodes/transaction-node"}}}'
```

JSON yapılandırma dosyalarını kullanarak *myWatcher* için *myInput* adlı bir giriş oluşturun.

``` azurecli
az resource create \
                   --resource-group myRG \
                   --name input \
                   --namespace Microsoft.Blockchain \ --resource-type inputs \
                   --parent watchers/mywatcher \
                   --is-full-object \
                   --properties @input.json
```

## <a name="create-output"></a>Çıktı oluşturma

Giden bağlantı blockchain verilerini Azure Olay Grid'e gönderir. Blockchain verilerini tek bir hedefe gönderebilir veya blockchain verilerini birden çok hedefe gönderebilirsiniz. Blockchain Veri Yöneticisi, belirli bir Blockchain Veri Yöneticisi örneği için birden çok Olay Izgara Konu giden bağlantıları destekler.

``` azurecli
az resource create \
                   --resource-group <Resource group> \
                   --name <Output name> \
                   --namespace Microsoft.Blockchain \
                   --resource-type outputs \
                   --parent watchers/<Watcher name> \
                   --is-full-object \
                   --properties <output resource properties>
```

| Parametre | Açıklama |
|-----------|-------------|
| resource-group | Çıktı kaynağının oluşturulacağı kaynak grubu adı. |
| ad | Çıktının adı. |
| ad alanı | **Microsoft.Blockchain** sağlayıcı ad alanını kullanın. |
| kaynak türü | Blockchain Data Manager çıktısı için kaynak türü **çıktılardır.** |
| Üst | Çıktının ilişkili olduğu izleyiciye giden yol. Örneğin, **izleyiciler / mywatcher**. |
| tam nesne | Özellikleri gösterir çıktı kaynağı için seçenekler içerir. |
| properties | Çıktı kaynağı için özellikler içeren JSON biçimlendirilmiş dize. Bir dize veya dosya olarak geçirilebilir. |

### <a name="output-examples"></a>Çıktı örnekleri

Yapılandırma JSON örneği *Doğu ABD* bölgesinde olay ızgarası konu \<\>adlı bir olay ızgara sıcağı konu bağlı bir çıktı kaynağı oluşturmak için.

``` json
{
    "location": "eastus",
    "properties": {
        "outputType": "EventGrid",
        "dataSource": {
            "resourceId": "/subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.EventGrid/topics/<event grid topic>"
        }
    }
}
```

| Öğe | Açıklama |
|---------|-------------|
| location | Çıktı kaynağının oluşturulacağı bölge. |
| outputType | Çıkış türü. Şu **anda, EventGrid** desteklenir. |
| resourceId | Çıktının bağlı olduğu kaynak. Abonelik \<\>Kimliği, \<Kaynak\>grubu \<ve\> Blockchain üyesini olay ızgarası kaynağının değerleriyle değiştirin. |

JSON yapılandırma dizesini kullanarak bir olay ızgarası konusuna bağlanan *mywatcher* için *çıktımı* adlandıran bir çıktı oluşturun.

``` azurecli-interactive
az resource create \
                   --resource-group myRG \
                   --name myoutput \
                   --namespace Microsoft.Blockchain \
                   --resource-type outputs \
                   --parent watchers/mywatcher \
                   --is-full-object \
                   --properties '{"location":"eastus","properties":{"outputType":"EventGrid","dataSource":{"resourceId":"/subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.EventGrid/topics/<event grid topic>"}}}'
```

JSON yapılandırma dosyasını kullanarak olay ızgarası konusuna bağlanan *mywatcher* için *çıktımı* adlandıran bir çıktı oluşturun.

``` azurecli
az resource create \
                   --resource-group myRG \
                   --name myoutput \
                   --namespace Microsoft.Blockchain \
                   --resource-type outputs \
                   --parent watchers/mywatcher \
                   --is-full-object \
                   --properties @output.json
```

## <a name="add-blockchain-application"></a>Blockchain uygulaması ekle

Blockchain uygulaması eklerseniz, Blockchain Data Manager uygulama nın olay ve özellik durumunu çözer. Aksi takdirde, yalnızca ham blok ve ham işlem verileri gönderilir. Blockchain Data Manager, sözleşme dağıtıldığında sözleşme adreslerini de keşfeder. Blockchain Data Manager örneğine birden çok blockchain uygulaması ekleyebilirsiniz.


> [!IMPORTANT]
> Şu anda, Solidity [dizi türlerini](https://solidity.readthedocs.io/en/v0.5.12/types.html#arrays) veya [eşleme türlerini](https://solidity.readthedocs.io/en/v0.5.12/types.html#mapping-types) bildiren blockchain uygulamaları tam olarak desteklenmez. Dizi veya eşleme türleri olarak bildirilen özellikler *ContractPropertiesMsg* veya *DecodedContractEventsMsg* iletilerinde deşifre edilmez.

``` azurecli
az resource create \
                   --resource-group <Resource group> \
                   --name <Application name> \
                   --namespace Microsoft.Blockchain \
                   --resource-type artifacts \
                   --parent watchers/<Watcher name> \
                   --is-full-object \
                   --properties <Application resource properties>
```

| Parametre | Açıklama |
|-----------|-------------|
| resource-group | Uygulama kaynağının oluşturulacağı kaynak grubu adı. |
| ad | Uygulamanın adı. |
| ad alanı | **Microsoft.Blockchain** sağlayıcı ad alanını kullanın. |
| kaynak türü | Blockchain Data Manager uygulamasının kaynak türü **yapıttır.** |
| Üst | Uygulamanın ilişkili olduğu izleyiciye giden yol. Örneğin, **izleyiciler / mywatcher**. |
| tam nesne | Özellikleri gösterir uygulama kaynağı için seçenekler içerir. |
| properties | JSON biçimlendirilmiş dize uygulama kaynağı için özellikleri içeren. Bir dize veya dosya olarak geçirilebilir. |

### <a name="blockchain-application-examples"></a>Blockchain uygulama örnekleri

Yapılandırma JSON *örneği, Doğu ABD* bölgesinde SÖZLEŞME ABI ve bytecode tarafından tanımlanan akıllı bir sözleşme izleyen bir uygulama kaynağı oluşturmak için.

``` json
{
    "location": "eastus",
    "properties": {
        "artifactType": "EthereumSmartContract",
        "content": {
            "abiFileUrl": "<ABI URL>",
            "bytecodeFileUrl": "<Bytecode URL>",
            "queryTargetTypes": [
                "ContractProperties",
                "ContractEvents"
            ]
        }
    }
}
```

| Öğe | Açıklama |
|---------|-------------|
| location | Uygulama kaynağının oluşturulacağı bölge. |
| Artifacttype | Uygulama türü. Şu anda, **EthereumSmartContract** desteklenir. |
| abiFileUrl | Akıllı sözleşme ABI JSON dosyası için URL. Sözleşme ABI'si edinme ve URL oluşturma hakkında daha fazla bilgi [Create contract ABI and bytecode URL](data-manager-portal.md#create-contract-abi-and-bytecode-url)için [bkz.](data-manager-portal.md#get-contract-abi-and-bytecode) |
| bytecodeFileUrl | Bytecode JSON dosyası dağıtılan akıllı sözleşme için URL. Bytecode dağıtılan akıllı sözleşme nin elde edilmesi ve bir URL oluşturma hakkında daha fazla bilgi için [Bkz. Sözleşme ABI ve bytecode alın](data-manager-portal.md#get-contract-abi-and-bytecode) ve [sözleşme ABI ve bytecode URL oluştur.](data-manager-portal.md#create-contract-abi-and-bytecode-url) Not: Blockchain Veri Yöneticisi **dağıtılan bytecode**gerektirir. |
| queryTargetTypes | Yayımlanmış ileti türleri. **SözleşmeÖzellikleri** belirtilmesi *ContractPropertiesMsg* ileti türü yayınlar. Sözleşme **Olayları** belirtme *DecodedContractEventsMsg* mesaj türü yayınlar. Not: *RawBlockAndTransactionMsg* ve *RawTransactionContractCreationMsg* mesaj türleri her zaman yayınlanır. |

JSON dizesi tarafından tanımlanan akıllı bir sözleşmeyi izleyen *myWatcher* için *myApplication* adlı bir uygulama oluşturun.

``` azurecli-interactive
az resource create \
                   --resource-group myRG \
                   --name myApplication \
                   --namespace Microsoft.Blockchain \
                   --resource-type artifacts \
                   --parent watchers/mywatcher \
                   --is-full-object \
                   --properties '{"location":"eastus","properties":{"artifactType":"EthereumSmartContract","content":{"abiFileUrl":"<ABI URL>","bytecodeFileUrl":"<Bytecode URL>","queryTargetTypes":["ContractProperties","ContractEvents"]}}}'
```

JSON yapılandırma dosyası kullanılarak tanımlanan akıllı bir sözleşmeyi izleyen *mywatcher* için *myApplication* adlı bir uygulama oluşturun.

``` azurecli
az resource create \
                   --resource-group myRG \
                   --name myApplication \
                   --namespace Microsoft.Blockchain \
                   --resource-type artifacts \
                   --parent watchers/mywatcher \
                   --is-full-object \
                   --properties @artifact.json
```

## <a name="start-instance"></a>Başlangıç örneği

Çalışırken Blockchain Manager örneği, tanımlanan girdilerdeki blockchain olaylarını izler ve tanımlanan çıktılara veri gönderir.

``` azurecli
az resource invoke-action \
                          --action start \
                          --ids /subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Blockchain/watchers/<Watcher name>
```

| Parametre | Açıklama |
|-----------|-------------|
| action | İzleyiciyi çalıştırmak için **başlat'ı** kullanın. |
| Kimlik | İzleyici kaynak kimliği. Abonelik \<\>Kimliği, \<Kaynak\>grubu \<ve\> İzleyici adını izleyici kaynağının değerleriyle değiştirin.|

### <a name="start-instance-example"></a>Örnek başlat

*Mywatcher*adlı bir Blockchain Veri Yöneticisi örneği başlatın.

``` azurecli-interactive
az resource invoke-action \
                          --action start \
                          --ids /subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Blockchain/watchers/mywatcher
```

## <a name="stop-instance"></a>Örneği durdur

Blockchain Veri Yöneticisi örneğini durdurun.

``` azurecli
az resource invoke-action \
                          --action stop \
                          --ids /subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Blockchain/watchers/<Watcher name>
```

| Parametre | Açıklama |
|-----------|-------------|
| action | İzleyiciyi durdurmak için **dur'u** kullanın. |
| Kimlik | Gözcünün adı. Abonelik \<\>Kimliği, \<Kaynak\>grubu \<ve\> İzleyici adını izleyici kaynağının değerleriyle değiştirin. |

### <a name="stop-watcher-example"></a>İzlemeyi durdurun örneği

Mywatcher adlı bir örneği *durdurun.*

``` azurecli-interactive
az resource invoke-action \
                          --action stop \
                          --ids /subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Blockchain/watchers/mywatcher
```

## <a name="delete-instance"></a>Örneği silme

Blockchain Veri Yöneticisi örneğini silin.

``` azurecli
az resource delete \
                   --resource-group <Resource group> \
                   --name <Watcher name> \
                   --resource-type Microsoft.Blockchain/watchers
```

| Parametre | Açıklama |
|-----------|-------------|
| resource-group | İzleyicinin kaynak grubu adını silmek. |
| ad | Silmek için izleyenin adı. |
| kaynak türü | Blockchain Veri Yöneticisi izleyicisinin kaynak türü **Microsoft.blockchain/watchers'dır.** |

### <a name="delete-instance-example"></a>Örnek örneği silme

*myRG* kaynak grubunda *mywatcher* adlı bir örneği silin.

``` azurecli-interactive
az resource delete \
                   --resource-group myRG \
                   --name mywatcher \
                   --resource-type Microsoft.blockchain/watchers
```

## <a name="next-steps"></a>Sonraki adımlar

Blockchain Data Manager ve Azure Cosmos DB'yi kullanarak blockchain işlem iletisi gezgini oluşturan bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure Cosmos DB'ye veri göndermek için Blok Zinciri Veri Yöneticisi'ni kullanma](data-manager-cosmosdb.md)
