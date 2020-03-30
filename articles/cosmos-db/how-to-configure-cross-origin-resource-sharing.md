---
title: Azure Cosmos DB'de Çapraz Kaynak Paylaşımı (CORS)
description: Bu makalede, Azure portalı ve Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB'de Çapraz Kaynak Paylaşımının (CORS) nasıl yapılandırılabildiğini açıklanmaktadır.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: dech
ms.openlocfilehash: 7a487cb10965a379a0a418efaa061be88c5d10dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77082983"
---
# <a name="configure-cross-origin-resource-sharing-cors"></a>Çapraz Kaynak Paylaşımını Yapıla (CORS)

Çapraz Kaynak Paylaşımı (CORS), bir etki alanı altında çalışan bir web uygulamasının başka bir etki alanındaki kaynaklara erişmesini sağlayan bir HTTP özelliğidir. Web tarayıcıları, bir web sayfasının farklı bir etki alanında API'leri aramasını engelleyen aynı kaynak ilkesi olarak bilinen bir güvenlik kısıtlaması uygular. Ancak CORS, kaynak etki alanının başka bir etki alanında API'leri aramasına izin vermek için güvenli bir yol sağlar. Azure Cosmos DB'deki Core (SQL) API'si artık "allowedOrigins" üstbilgisini kullanarak Çapraz Kaynak Paylaşımını (CORS) destekler. Azure Cosmos hesabınız için CORS desteğini etkinleştirdikten sonra, yalnızca kimlik doğrulaması yapılan istekler, belirttiğiniz kurallara göre izin verilip verilmediğini belirlemek için değerlendirilir.

Azure portalından veya Azure Kaynak Yöneticisi şablonundan Çapraz kaynak paylaşımı (CORS) ayarını yapılandırabilirsiniz. Azure Cosmos DB, Core (SQL) API'sini kullanan Cosmos hesapları için hem Düğüm.j'lerde hem de tarayıcı tabanlı ortamlarda çalışan bir JavaScript kitaplığını destekler. Bu kitaplık artık Ağ Geçidi modunu kullanırken CORS desteğinden yararlanabilir. Bu özelliği kullanmak için istemci tarafı yapılandırması gerekmez. CORS desteği yle, tarayıcıdaki kaynaklar Azure Cosmos DB'ye [JavaScript kitaplığı](https://www.npmjs.com/package/@azure/cosmos) üzerinden veya basit işlemler için [doğrudan REST API'sinden](https://docs.microsoft.com/rest/api/cosmos-db/) erişebilir.

> [!NOTE]
> CORS desteği yalnızca Azure Cosmos DB Core (SQL) API için geçerlidir ve desteklenir. Bu protokoller istemci-sunucu iletişimi için HTTP kullanmadığı için Cassandra, Gremlin veya MongoDB için Azure Cosmos DB API'leri için geçerli değildir.

## <a name="enable-cors-support-from-azure-portal"></a>Azure portalından CORS desteğini etkinleştirme

Azure portalını kullanarak Kökenler Arası Kaynak Paylaşımını etkinleştirmek için aşağıdaki adımları kullanın:

1. Azure cosmos DB hesabınıza gidin. **CORS** bıçağını açın.

2. Azure Cosmos DB hesabınıza çapraz orinis aramaları yapabilen virgülle ayrılmış bir kaynak listesi belirtin. Örneğin, `https://www.mydomain.com`, `https://mydomain.com` `https://api.mydomain.com`. Ayrıca tüm kökenlere izin\*vermek ve **Gönder'i**seçmek için " " joker karakter de kullanabilirsiniz. 

   > [!NOTE]
   > Şu anda, etki alanı adının bir parçası olarak joker karakterleri kullanamazsınız. Örneğin `https://*.mydomain.net` biçim henüz desteklenmedi. 

   ![Azure portalını kullanarak çapraz kaynak paylaşımını etkinleştirme](./media/how-to-configure-cross-origin-resource-sharing/enable-cross-origin-resource-sharing-using-azure-portal.png)

## <a name="enable-cors-support-from-resource-manager-template"></a>Kaynak Yöneticisi şablonundan CORS desteğini etkinleştirme

Kaynak Yöneticisi şablonu kullanarak CORS'i etkinleştirmek için varolan şablona "allowedOrigins" özelliğine sahip "korslar" bölümünü ekleyin. Aşağıdaki JSON, CORS etkinleştirilmiş yeni bir Azure Cosmos hesabı oluşturan bir şablon örneğidir.

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2019-08-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "cors": [
      {
        "allowedOrigins": "*"
      }
    ]
  }
}
```

## <a name="using-the-azure-cosmos-db-javascript-library-from-a-browser"></a>Azure Cosmos DB JavaScript kitaplığını tarayıcıdan kullanma

Bugün, Azure Cosmos DB JavaScript kitaplığı yalnızca kitaplığın paketiyle birlikte gönderilen CommonJS sürümüne sahiptir. Bu kitaplığı tarayıcıdan kullanmak için tarayıcı uyumlu bir kitaplık oluşturmak için Rollup veya Webpack gibi bir araç kullanmanız gerekir. Bazı Node.js kitaplıklarında onlar için tarayıcı alayları olmalıdır. Aşağıda, gerekli sahte ayarları olan bir webpack config dosyasıörneği verilmiştir.

```javascript
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  node: {
    net: "mock",
    tls: "mock"
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist")
  }
};
```
 
Burada, yeni öğeler oluşturulduğunda gerçek zamanlı güncelleştirmeler gönderen bir Todo uygulaması oluşturmak için Azure Cosmos DB JavaScript SDK kitaplığıyla TypeScript ve Webpack kullanan bir [kod örneği](https://github.com/christopheranderson/cosmos-browser-sample) verilmiştir.
En iyi uygulama olarak, tarayıcıdan Azure Cosmos DB ile iletişim kurmak için birincil anahtarı kullanmayın. Bunun yerine, iletişim kurmak için kaynak belirteçlerini kullanın. Kaynak belirteçleri hakkında daha fazla bilgi için [bkz.](secure-access-to-data.md#resource-tokens)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabınızı ngüvenliğini sağlamanın diğer yolları hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Cosmos DB](how-to-configure-firewall.md) makalesi için bir güvenlik duvarı yapılandırın.

* [Azure Cosmos DB hesabınız için sanal ağı ve alt ağ tabanlı erişimi yapılandırın](how-to-configure-vnet-service-endpoint.md)
