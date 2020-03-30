---
title: Azure Blockchain Workbench REST API'lerini kullanma
description: Azure Blockchain Workbench Preview REST API'nin nasıl kullanılacağına yönelik senaryolar
ms.date: 03/05/2020
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 3084fcf343bc42fe01bf352b6791916d62f63540
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672744"
---
# <a name="using-the-azure-blockchain-workbench-preview-rest-api"></a>Azure Blockchain Çalışma Tezgahı Önizleme REST API'sini kullanma

Azure Blockchain Workbench Preview REST API, geliştiricilere ve bilgi çalışanlarına blockchain uygulamalarına zengin entegrasyonlar oluşturmanın bir yolunu sağlar. Bu makalede, Workbench REST API'nin nasıl kullanılacağına ilişkin çok sayıda senaryo vurgulanmıştır. Örneğin, oturum açmış kullanıcıların atanan blockchain uygulamalarını görüntülemesine ve bunlarla etkileşimde kurmasına olanak tanıyan özel bir blockchain istemcisi oluşturmak istediğinizi varsayalım. İstemci, sözleşme örneklerini görüntülemek ve akıllı sözleşmeler üzerinde işlem yapmak için Blockchain Workbench API'sini kullanabilir.

## <a name="blockchain-workbench-api-endpoint"></a>Blockchain Çalışma Tezgahı API bitiş noktası

Blockchain Workbench API'leri dağıtım Için bir bitiş noktasından erişilir. Dağıtımınız için API uç noktası URL'sini almak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol daki gezinti bölmesinde **Kaynak gruplarını**seçin.
1. Dağıtılmış Blockchain Workbench kaynak grubu adını seçin.
1. Listeyi türe göre alfabetik olarak sıralamak için **TYPE** sütununa başlığını seçin.
1. Tip **Uygulama Hizmeti**ile iki kaynak vardır. "-api" soneki *ile* **Uygulama Hizmeti** türünü seçin.
1. Uygulama **Hizmetine Genel**Bakış'ta, dağıtılan Blockchain Çalışma Tezgahı'nın API uç noktası URL'sini temsil eden **URL** değerini kopyalayın.

    ![Uygulama hizmeti API bitiş noktası URL'si](media/use-api/app-service-api.png)

## <a name="authentication"></a>Kimlik doğrulaması

Blockchain Workbench REST API'sine gelen istekler Azure Etkin Dizini (Azure AD) ile korunur.

REST API'lerine kimlik doğrulaması yapmak için, ISTEMCI kodunun API'yi aramadan önce geçerli kimlik bilgilerini içeren kimlik doğrulaması gerekir. Kimlik doğrulama, Azure AD tarafından çeşitli aktörler arasında koordine edilir ve müşterinize kimlik doğrulamasının kanıtı olarak bir [erişim belirteci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#access-token) sağlar. Belirteç daha sonra REST API isteklerinin HTTP Yetkilendirme üstbilgisinde gönderilir. Azure AD kimlik doğrulaması hakkında daha fazla bilgi edinmek [için geliştiriciler için Azure Etkin Dizini'ne](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)bakın.

Kimlik doğrulama örnekleri için [REST API örneklerine](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/rest-api-samples) bakın.

## <a name="using-postman"></a>Postman'i kullanma

Workbench API'lerini sınamak veya deneme yapmak istiyorsanız, dağıtımınıza API çağrıları yapmak için [Postacı'yı](https://www.postman.com) kullanabilirsiniz. GitHub'dan [Workbench API isteklerinden oluşan örnek bir Postacı koleksiyonunu indirin.](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/rest-api-samples/postman) API isteklerini doğrulamak ve örnek API isteklerini kullanma yla ilgili ayrıntılar için README dosyasına bakın.

## <a name="create-an-application"></a>Uygulama oluşturma

Blockchain Workbench uygulaması oluşturmak için iki API çağrısı kullanırsınız. Bu yöntem yalnızca Workbench yöneticisi olan kullanıcılar tarafından gerçekleştirilebilir.

Uygulamanın JSON dosyasını yüklemek ve bir uygulama kimliği almak için [Applications POST API'yi](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationspost) kullanın.

### <a name="applications-post-request"></a>Uygulamalar POST isteği

Yapılandırma dosyasını istek gövdesinin bir parçası olarak göndermek için **appFile** parametresini kullanın.

``` http
POST /api/v1/applications
Content-Type: multipart/form-data;
Authorization : Bearer {access token}
Content-Disposition: form-data; name="appFile"; filename="/C:/smart-contract-samples/HelloWorld.json"
Content-Type: application/json
```

### <a name="applications-post-response"></a>Uygulamalar POST yanıtı

Oluşturulan uygulama kimliği yanıt olarak döndürülür. Bir sonraki API'yi aradiğinizde yapılandırma dosyasını kod dosyasıyla ilişkilendirmek için uygulama kimliğine ihtiyacınız vardır.

``` http
HTTP/1.1 200 OK
Content-Type: "application/json"
1
```

### <a name="contract-code-post-request"></a>Sözleşme kodu POST isteği

Uygulamanın Solidity kodu dosyasını yüklemek için uygulama kimliğini geçirerek [Uygulamalar sözleşme kodu POST API'yi](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/contractcodepost) kullanın. Yük tek bir Solidity dosyası veya Katılık dosyaları içeren sıkıştırılmış bir dosya olabilir.

Aşağıdaki değerleri değiştirin:

| Parametre | Değer |
|-----------|-------|
| {applicationId} | Post API uygulamalarından iade değeri. |
| {ledgerId} | Genel muhasebe dizini. Değer genellikle 1'dir. Değer için [Genel Muhasebe tablosunu](data-sql-management-studio.md) da denetleyebilirsiniz. |

``` http
POST /api/v1/applications/{applicationId}/contractCode?ledgerId={ledgerId}
Content-Type: multipart/form-data;
Authorization : Bearer {access token}
Content-Disposition: form-data; name="contractFile"; filename="/C:/smart-contract-samples/HelloWorld.sol"
```

### <a name="contract-code-post-response"></a>Sözleşme kodu POST yanıtı

Başarılı olursa, [yanıt, ContractCode tablosundan](data-sql-management-studio.md)oluşturulan sözleşme kodu kimliğini içerir.

``` http
HTTP/1.1 200 OK
Content-Type: "application/json"
2
```

## <a name="assign-roles-to-users"></a>Kullanıcılara rol atama

Belirtilen blockchain uygulamasında kullanıcıdan role eşleme oluşturmak için uygulama kimliği, kullanıcı kimliği ve uygulama rol kimliği geçirerek [Uygulamalar rol atamaları POST API'yi](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/roleassignmentspost) kullanın. Bu yöntem yalnızca Workbench yöneticisi olan kullanıcılar tarafından gerçekleştirilebilir.

### <a name="role-assignments-post-request"></a>Rol atamaları POST isteği

Aşağıdaki değerleri değiştirin:

| Parametre | Değer |
|-----------|-------|
| {applicationId} | Uygulamalar POST API'den iade değeri. |
| {userId} | [Kullanıcı tablosundan kullanıcı](data-sql-management-studio.md)kimliği değeri. |
| {applicationRoleId} | [ApplicationRole tablosundan](data-sql-management-studio.md)uygulama kimliğiyle ilişkili uygulama rolü kimliği değeri. |

``` http
POST /api/v1/applications/{applicationId}/roleAssignments
Content-Type: application/json;
Authorization : Bearer {access token}

{
  "userId": {userId},
  "applicationRoleId": {applicationRoleId}
}
```

### <a name="role-assignments-post-response"></a>Rol atamaları POST yanıtı

Yanıt başarılı olursa, [RoleAssignment tablosundan](data-sql-management-studio.md)oluşturulan rol atama kimliğini içerir.

``` http
HTTP/1.1 200
1
```

## <a name="list-applications"></a>Uygulamaları listeleme

Kullanıcı için tüm Blockchain Workbench uygulamalarını almak için [GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget) uygulamalarını kullanın. Bu örnekte, oturum açmış kullanıcıiki uygulamaya erişebilir:

- [Varlık transferi](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
- [Soğutulmuş taşımacılık](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

### <a name="applications-get-request"></a>Başvurular İstek alın

``` http
GET /api/v1/applications
Authorization : Bearer {access token}
```

### <a name="applications-get-response"></a>Uygulamalar yanıt alın

Yanıt, bir kullanıcının Blockchain Workbench'te erişebilen tüm blockchain uygulamalarını listeler. Blockchain Workbench yöneticileri her blockchain uygulamasını alır. Çalışma Tezgahı olmayan yöneticiler, en az bir ilişkili uygulama rolüne veya ilişkili akıllı sözleşme örneği rolüne sahip oldukları tüm blockchain uygulamalarını alır.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications?skip=2",
    "applications": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
            "displayName": "Asset Transfer",
            "createdByUserId": 1,
            "createdDtTm": "2018-04-28T05:59:14.4733333",
            "enabled": true,
            "applicationRoles": null
        },
        {
            "id": 2,
            "name": "RefrigeratedTransportation",
            "description": "Application to track end-to-end transportation of perishable goods.",
            "displayName": "Refrigerated Transportation",
            "createdByUserId": 7,
            "createdDtTm": "2018-04-28T18:25:38.71",
            "enabled": true,
            "applicationRoles": null
        }
    ]
}
```

## <a name="list-workflows-for-an-application"></a>Uygulama için iş akışlarını listeleme

Bir kullanıcının Blockchain Workbench'te erişebilen belirli bir blockchain uygulamasının tüm iş akışlarını listelemek için [Uygulamalar İş Akışlarını GET API'yi](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget) kullanın. Her blok zinciri uygulamasının bir veya birden çok iş akışı vardır ve her iş akışının sıfır veya akıllı anlaşma örnekleri bulunur. Yalnızca bir iş akışına sahip bir blockchain istemci uygulaması için, kullanıcıların uygun iş akışını seçmesine olanak tanıyan kullanıcı deneyimi akışını atlamanızı öneririz.

### <a name="application-workflows-request"></a>Uygulama iş akışları isteği

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

### <a name="application-workflows-response"></a>Uygulama iş akışları yanıtı

Blockchain Workbench yöneticileri her blockchain iş akışını alır. Çalışma Tezgahı olmayan yöneticiler, en az bir ilişkili uygulama rolüne sahip oldukları veya akıllı sözleşme örneği rolüyle ilişkili tüm iş akışlarını alır.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications/1/workflows?skip=1",
    "workflows": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Handles the business logic for the asset transfer scenario",
            "displayName": "Asset Transfer",
            "applicationId": 1,
            "constructorId": 1,
            "startStateId": 1
        }
    ]
}
```

## <a name="create-a-contract-instance"></a>Sözleşme örneği oluşturma

İş akışı için yeni bir akıllı sözleşme örneği oluşturmak için [Sözleşmeler V2 POST API'yi](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contractsv2/contractpost) kullanın. Kullanıcılar yalnızca, kullanıcı iş akışı için akıllı bir sözleşme örneği başlatabilecek bir uygulama rolüyle ilişkiliyse yeni bir akıllı sözleşme örneği oluşturabilir.

> [!NOTE]
> Bu örnekte, API sürümü 2 kullanılır. Sürüm 2 sözleşme API'leri, ilişkili ProvisioningStatus alanları için daha ayrıntılı bilgi sağlar.

### <a name="contracts-post-request"></a>Sözleşmeler POST isteği

Aşağıdaki değerleri değiştirin:

| Parametre | Değer |
|-----------|-------|
| {iş akışıId} | İş akışı kimliği değeri, [Sözleşmenin İş Akışı tablosundaki](data-sql-management-studio.md)ConstructorID'sidir. |
| {contractCodeId} | [ContractCode tablosundan sözleşme](data-sql-management-studio.md)kodu kimlik değeri. Oluşturmak istediğiniz sözleşme örneğinin uygulama kimliğini ve genel muhasebe kimliğini ilişkilendirin. |
| {connectionId} | [Bağlantı tablosundan](data-sql-management-studio.md)bağlantı kimliği değeri. |

İstek gövdesi için, değerleri aşağıdaki bilgileri kullanarak ayarlayın:

| Parametre | Değer |
|-----------|-------|
| iş akışıFunctionID | [İş Akışı İşlevi tablosundan](data-sql-management-studio.md)kimlik. |
| iş akışıEylemParametreleri | Ad değeri çiftlerini oluşturana geçirilen parametreler. Her parametre [için, İş AkışıFonksiyonuParametre](data-sql-management-studio.md) tablosundaki İş akışıFonksiyonParameterID değerini kullanın. |

``` http
POST /api/v2/contracts?workflowId={workflowId}&contractCodeId={contractCodeId}&connectionId={connectionId}
Content-Type: application/json;
Authorization : Bearer {access token}

{
  "workflowFunctionID": 2,
  "workflowActionParameters": [
    {
      "name": "message",
      "value": "Hello, world!",
      "workflowFunctionParameterId": 3
    }
  ]
}
```

### <a name="contracts-post-response"></a>Sözleşmeler POST yanıtı

Başarılı olursa, rol atamaları API [ContractActionPARAmeter tablosundan](data-sql-management-studio.md)ContractActionID döndürür.

``` http
HTTP/1.1 200 OK
4
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>İş akışı için akıllı anlaşma örneklerini listeleme

İş akışı için tüm akıllı sözleşme örneklerini göstermek için [GET API'yi](/rest/api/azure-blockchain-workbench/contractsv2/contractsget) kullanın. Ya da kullanıcıların gösterilen akıllı sözleşme örneklerinden herhangi birini derinlemesine dalmalarına izin verebilirsiniz.

### <a name="contracts-request"></a>Sözleşme talebi

Bu örnekte, kullanıcının bir eylem yapmak için akıllı anlaşma örneklerinden biriyle etkileşimli çalışmak istediğini düşünün.

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

### <a name="contracts-response"></a>Sözleşme yanıtı

Yanıt, belirtilen iş akışının tüm akıllı sözleşme örneklerini listeler. Çalışma tezgahı yöneticileri tüm akıllı sözleşme örnekleri olsun. Çalışma Tezgahı olmayan yöneticiler, en az bir ilişkili uygulama rolüne sahip oldukları veya akıllı sözleşme örneği rolüyle ilişkili olan her akıllı sözleşme örneğini alır.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts?skip=3&workflowId=1",
    "contracts": [
        {
            "id": 1,
            "provisioningStatus": 2,
            "connectionID": 1,
            "ledgerIdentifier": "0xbcb6127be062acd37818af290c0e43479a153a1c",
            "deployedByUserId": 1,
            "workflowId": 1,
            "contractCodeId": 1,
            "contractProperties": [
                {
                    "workflowPropertyId": 1,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 2,
                    "value": "My first car"
                },
                {
                    "workflowPropertyId": 3,
                    "value": "54321"
                },
                {
                    "workflowPropertyId": 4,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 5,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 6,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 7,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 8,
                    "value": "0xd882530eb3d6395e697508287900c7679dbe02d7"
                }
            ],
            "transactions": [
                {
                    "id": 1,
                    "connectionId": 1,
                    "transactionHash": "0xf3abb829884dc396e03ae9e115a770b230fcf41bb03d39457201449e077080f4",
                    "blockID": 241,
                    "from": "0xd882530eb3d6395e697508287900c7679dbe02d7",
                    "to": null,
                    "value": 0,
                    "isAppBuilderTx": true
                }
            ],
            "contractActions": [
                {
                    "id": 1,
                    "userId": 1,
                    "provisioningStatus": 2,
                    "timestamp": "2018-04-29T23:41:14.9333333",
                    "parameters": [
                        {
                            "name": "Description",
                            "value": "My first car"
                        },
                        {
                            "name": "Price",
                            "value": "54321"
                        }
                    ],
                    "workflowFunctionId": 1,
                    "transactionId": 1,
                    "workflowStateId": 1
                }
            ]
        }
    ]
}
```

## <a name="list-available-actions-for-a-contract"></a>Anlaşma için kullanılabilir eylemleri listeleme

Sözleşmenin durumu göz önüne alındığında kullanılabilir kullanıcı eylemlerini göstermek için [Sözleşme Eylem GET API'sını](/rest/api/azure-blockchain-workbench/contractsv2/contractactionget) kullanın. 

### <a name="contract-action-request"></a>Sözleşme eylem isteği

Bu örnekte, kullanıcı oluşturdukları yeni bir akıllı sözleşme için kullanılabilir tüm eylemlere bakıyor.

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

### <a name="contract-action-response"></a>Sözleşme eylem yanıtı

Yanıtta belirtilen akıllı anlaşma örneğinin geçerli durumuna göre kullanıcının gerçekleştirebileceği tüm eylemler listelenir.

* Modify: Kullanıcının, bir varlığın açıklamasını ve fiyatını değiştirmesine olanak tanır.
* Sonlandırma: Kullanıcının varlığın sözleşmesini sona erdirmesine izin verir.

Belirtilen akıllı anlaşma örneğinin geçerli durumuna göre ilişkili bir uygulama rolü olan veya akıllı anlaşma örneği rolüyle ilişkili olan kullanıcılar tüm uygun eylemleri alır.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts/1/actions?skip=2",
    "workflowFunctions": [
        {
            "id": 2,
            "name": "Modify",
            "description": "Modify the description/price attributes of this asset transfer instance",
            "displayName": "Modify",
            "parameters": [
                {
                    "id": 1,
                    "name": "description",
                    "description": "The new description of the asset",
                    "displayName": "Description",
                    "type": {
                        "id": 2,
                        "name": "string",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                },
                {
                    "id": 2,
                    "name": "price",
                    "description": "The new price of the asset",
                    "displayName": "Price",
                    "type": {
                        "id": 3,
                        "name": "money",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                }
            ],
            "workflowId": 1
        },
        {
            "id": 3,
            "name": "Terminate",
            "description": "Used to cancel this particular instance of asset transfer",
            "displayName": "Terminate",
            "parameters": [],
            "workflowId": 1
        }
    ]
}
```

## <a name="execute-an-action-for-a-contract"></a>Anlaşma için bir eylem yürütme

Belirtilen akıllı sözleşme örneği için işlem yapmak için [Sözleşme Eylem SONRASı API'yi](/rest/api/azure-blockchain-workbench/contractsv2/contractactionpost) kullanın.

### <a name="contract-action-post-request"></a>Sözleşme eylem POST isteği

Bu durumda, bir kullanıcının bir varlığın açıklamasını ve fiyatını değiştirmek istediği senaryoyu düşünün.

``` http
POST /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
actionInformation: {
    "workflowFunctionId": 2,
    "workflowActionParameters": [
        {
            "name": "description",
            "value": "My updated car"
        },
        {
            "name": "price",
            "value": "54321"
        }
    ]
}
```

Kullanıcılar ancak belirtilen akıllı anlaşma örneğinin geçerli durumuna ve kullanıcının ilişkili uygulama rolüne veya akıllı anlaşma örneği rolüne göre eylem yürütebilir.

### <a name="contract-action-post-response"></a>Sözleşme eylem POST yanıtı

Gönderi başarılı olursa, yanıt gövdesi olmayan bir HTTP 200 OK yanıtı döndürülür.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Sonraki adımlar

Blockchain Workbench API'leri hakkında başvuru bilgileri için [Azure Blockchain Workbench REST API başvurusuna](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)bakın.
