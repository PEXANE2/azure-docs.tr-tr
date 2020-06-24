---
title: Azure blok zinciri çalışma ekranı REST API 'Lerini kullanma
description: Azure blok zinciri çalışma ekranı önizlemesi 'nin nasıl kullanılacağına yönelik senaryolar REST API
ms.date: 03/05/2020
ms.topic: how-to
ms.reviewer: brendal
ms.openlocfilehash: 7c96e45c8bcc9834df7ef2a0b2a59f53fd8315cf
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85210071"
---
# <a name="using-the-azure-blockchain-workbench-preview-rest-api"></a>Azure blok zinciri çalışma ekranı önizleme 'yi kullanma REST API

Azure blok zinciri çalışma ekranı önizlemesi REST API geliştiricilere ve bilgi çalışanlarına, blok zinciri uygulamalarına yönelik zengin tümleştirmeler oluşturma olanağı sağlar. Bu makalede, REST API çalışma ekranı kullanımı hakkında çeşitli senaryolar vurgulanmıştır. Örneğin, oturum açan kullanıcıların atanan blok zinciri uygulamalarını görüntülemesine ve bunlarla etkileşime girmesine izin veren özel bir blok zinciri istemcisi oluşturmak istediğinizi varsayalım. İstemci, sözleşme örneklerini görüntülemek ve akıllı sözleşmelerde işlem yapmak için blok zinciri çalışma ekranı API 'sini kullanabilir.

## <a name="blockchain-workbench-api-endpoint"></a>Blok zinciri çalışma ekranı API uç noktası

Blok zinciri çalışma ekranı API 'Lerine, dağıtımınız için bir uç nokta üzerinden erişilir. Dağıtımınız için API uç nokta URL 'sini almak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol taraftaki Gezinti bölmesinde **kaynak grupları**' nı seçin.
1. Dağıtılmış blok zinciri çalışma ekranı ' nı kaynak grubu adı ' nı seçin.
1. Listeyi türe göre alfabetik olarak sıralamak için **tür** sütun başlığını seçin.
1. **App Service**türünde iki kaynak vardır. "-Api" sonekiyle **App Service** *with* türde kaynağı seçin.
1. App Service **genel bakışta**, dağıtılan blok zinciri çalışma EKRANıNA yönelik API uç noktası URL 'sini temsil eden **URL** değerini kopyalayın.

    ![App Service API uç nokta URL 'SI](media/use-api/app-service-api.png)

## <a name="authentication"></a>Kimlik doğrulaması

Blok zinciri çalışma ekranı REST API istekleri Azure Active Directory (Azure AD) ile korunuyor.

REST API 'Lerine kimliği doğrulanmış bir istek yapmak için, API 'yi çağırabilmeniz için istemci kodu geçerli kimlik bilgileriyle kimlik doğrulaması gerektirir. Kimlik doğrulaması, Azure AD tarafından çeşitli aktörler arasında düzenlenir ve istemcinizi kimlik doğrulaması kanıtı olarak bir [erişim belirteci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#access-token) sağlar. Belirteç daha sonra REST API isteklerinin HTTP yetkilendirme üstbilgisinde gönderilir. Azure AD kimlik doğrulaması hakkında daha fazla bilgi için bkz. [geliştiriciler için Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

Kimlik doğrulaması ile ilgili örnekler için bkz. [REST API örnekleri](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/rest-api-samples) .

## <a name="using-postman"></a>Postman'i kullanma

Çalışma ekranı API 'Lerini test etmek veya denemek istiyorsanız, dağıtımınıza yönelik API çağrıları yapmak için [Postman](https://www.postman.com) kullanabilirsiniz. GitHub 'dan [bir çalışma ekranı API 'si isteklerinin örnek Postman koleksiyonunu indirin](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/rest-api-samples/postman) . Örnek API isteklerini kimlik doğrulama ve kullanma hakkında ayrıntılı bilgi için BENIOKU dosyasına bakın.

## <a name="create-an-application"></a>Uygulama oluşturma

İki API çağrısı kullanarak bir blok zinciri çalışma ekranı uygulaması oluşturun. Bu yöntem yalnızca, çalışma ekranı yöneticileri olan kullanıcılar tarafından gerçekleştirilebilir.

Uygulamanın JSON dosyasını karşıya yüklemek ve bir uygulama KIMLIĞI almak için [uygulamalar GÖNDERI API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationspost) 'sini kullanın.

### <a name="applications-post-request"></a>Uygulamalar POST isteği

Yapılandırma dosyasını istek gövdesinin bir parçası olarak göndermek için **Appfile** parametresini kullanın.

``` http
POST /api/v1/applications
Content-Type: multipart/form-data;
Authorization : Bearer {access token}
Content-Disposition: form-data; name="appFile"; filename="/C:/smart-contract-samples/HelloWorld.json"
Content-Type: application/json
```

### <a name="applications-post-response"></a>Uygulamalar GÖNDERI yanıtı

Oluşturulan uygulama KIMLIĞI yanıtta döndürülür. Sonraki API 'YI çağırdığınızda yapılandırma dosyasını kod dosyası ile ilişkilendirmek için uygulama KIMLIĞI gereklidir.

``` http
HTTP/1.1 200 OK
Content-Type: "application/json"
1
```

### <a name="contract-code-post-request"></a>Sözleşme kodu GÖNDERI isteği

Uygulamanın Solidity kod dosyasını karşıya yüklemek için uygulama KIMLIĞINI geçirerek [uygulamalar anlaşma kodu GÖNDERI API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/contractcodepost) 'sini kullanın. Yük, tek bir Solidity dosyası veya Solidity dosyalarını içeren daraltılmış bir dosya olabilir.

Aşağıdaki değerleri değiştirin:

| Parametre | Değer |
|-----------|-------|
| Uygulama | Uygulama SONRASı API 'sindeki değeri döndürün. |
| {LedgerID} | Genel muhasebe dizini. Değer genellikle 1 ' dir. Ayrıca, bu değere ait [Defter tablosunu](data-sql-management-studio.md) da denetleyebilirsiniz. |

``` http
POST /api/v1/applications/{applicationId}/contractCode?ledgerId={ledgerId}
Content-Type: multipart/form-data;
Authorization : Bearer {access token}
Content-Disposition: form-data; name="contractFile"; filename="/C:/smart-contract-samples/HelloWorld.sol"
```

### <a name="contract-code-post-response"></a>Sözleşme kodu GÖNDERI yanıtı

Başarılı olursa, yanıt, [contractcode tablosundan](data-sql-management-studio.md)oluşturulan sözleşme kodu kimliğini içerir.

``` http
HTTP/1.1 200 OK
Content-Type: "application/json"
2
```

## <a name="assign-roles-to-users"></a>Kullanıcılara rol atama

Belirtilen blok zinciri uygulamasında bir kullanıcı rolü eşlemesi oluşturmak için uygulama KIMLIĞI, Kullanıcı KIMLIĞI ve uygulama rolü KIMLIĞINI geçirerek [uygulamalar rol ATAMALARı gönderme API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/roleassignmentspost) 'sini kullanın. Bu yöntem yalnızca, çalışma ekranı yöneticileri olan kullanıcılar tarafından gerçekleştirilebilir.

### <a name="role-assignments-post-request"></a>Rol atamaları POST isteği

Aşağıdaki değerleri değiştirin:

| Parametre | Değer |
|-----------|-------|
| Uygulama | Uygulama SONRASı API 'sindeki değeri döndürün. |
| UserID | [Kullanıcı tablosundan](data-sql-management-studio.md)Kullanıcı kimliği değeri. |
| {Applicationroleıd} | [ApplicationRole tablosundan](data-sql-management-studio.md)uygulama kimliğiyle ilişkili uygulama rolü kimliği değeri. |

``` http
POST /api/v1/applications/{applicationId}/roleAssignments
Content-Type: application/json;
Authorization : Bearer {access token}

{
  "userId": {userId},
  "applicationRoleId": {applicationRoleId}
}
```

### <a name="role-assignments-post-response"></a>Rol atamaları GÖNDERI yanıtı

Başarılı olursa, yanıt [roleatama tablosundan](data-sql-management-studio.md)oluşturulan rol atama kimliğini içerir.

``` http
HTTP/1.1 200
1
```

## <a name="list-applications"></a>Uygulamaları listeleme

Kullanıcı için tüm blok zinciri çalışma ekranı uygulamalarını almak için [UYGULAMALARı al API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget) 'sini kullanın. Bu örnekte, oturum açmış kullanıcının iki uygulamaya erişimi vardır:

- [Varlık aktarımı](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
- [Soğutma ulaşım](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

### <a name="applications-get-request"></a>Uygulamalar GET isteği

``` http
GET /api/v1/applications
Authorization : Bearer {access token}
```

### <a name="applications-get-response"></a>Uygulamalar GET yanıtı

Yanıt, bir kullanıcının blok zinciri çalışma ekranı 'nda erişebileceği tüm blok zinciri uygulamalarını listeler. Blok zinciri çalışma ekranı yöneticileri tüm blok zinciri uygulamasını alır. Çalışma ekranı olmayan yöneticiler, en az bir ilişkili uygulama rolüne ya da ilişkili bir akıllı sözleşme örneği rolüne sahip oldukları tüm blok zinciri uygulamalarını alırlar.

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

[Uygulama Iş akışları](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget) , bir kullanıcının blok zinciri çalışma ekranına erişimi olan belirtilen bir blok zinciri uygulamasının tüm iş akışlarını LISTELEMEK IÇIN API Al ' i kullanın. Her blok zinciri uygulamasının bir veya birden çok iş akışı vardır ve her iş akışının sıfır veya akıllı anlaşma örnekleri bulunur. Yalnızca bir iş akışına sahip bir blok zinciri istemci uygulaması için, kullanıcıların uygun iş akışını seçmesini sağlayan kullanıcı deneyimi akışının atlanmasını öneririz.

### <a name="application-workflows-request"></a>Uygulama iş akışları isteği

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

### <a name="application-workflows-response"></a>Uygulama iş akışları yanıtı

Blok zinciri çalışma ekranı yöneticileri tüm blok zinciri iş akışını alır. Çalışma ekranı olmayan yöneticiler, en az bir ilişkili uygulama rolüne sahip oldukları veya bir akıllı sözleşme örneği rolüyle ilişkilendirilen tüm iş akışlarını alırlar.

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

Bir iş akışı için yeni bir akıllı sözleşme örneği oluşturmak üzere [sözleşmeleri v2 Post API 'sini](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contractsv2/contractpost) kullanın. Kullanıcılar, iş akışı için bir akıllı sözleşme örneği başlatabilecek bir uygulama rolüyle ilişkilendirildiğinde yalnızca yeni bir akıllı sözleşme örneği oluşturabilir.

> [!NOTE]
> Bu örnekte, API 'nin 2. sürümü kullanılır. Sürüm 2 sözleşme API 'Leri, ilişkili ProvisioningStatus alanları için daha fazla ayrıntı sağlar.

### <a name="contracts-post-request"></a>Sözleşmeler POST isteği

Aşağıdaki değerleri değiştirin:

| Parametre | Değer |
|-----------|-------|
| Workflowıd | İş akışı KIMLIĞI değeri, [Iş akışı tablosundan](data-sql-management-studio.md)sözleşmenin construcıd değeridir. |
| {Contractcodeıd} | [Contractcode tablosundan](data-sql-management-studio.md)anlaşma kodu kimliği değeri. Oluşturmak istediğiniz sözleşme örneği için uygulama KIMLIĞI ve hesap KIMLIĞI ile bağıntı kurun. |
| ConnectionID | [Bağlantı tablosundan](data-sql-management-studio.md)bağlantı kimliği değeri. |

İstek gövdesi için aşağıdaki bilgileri kullanarak değerleri ayarlayın:

| Parametre | Değer |
|-----------|-------|
| Workflowfunctionıd | [Workflowfunction TABLOSUNDAN](data-sql-management-studio.md)kimlik. |
| workflowActionParameters | Oluşturucuya geçirilen parametrelerin değer çiftlerini adlandırın. Her parametre için [Workflowfunctionparameter](data-sql-management-studio.md) tablosundan Workflowfunctionparameterıd değerini kullanın. |

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

### <a name="contracts-post-response"></a>Sözleşmelerin GÖNDERI yanıtı

Başarılı olursa, rol atamaları API 'SI [Contractactionparameter tablosundan](data-sql-management-studio.md)sözleşmeli Tactionıd döndürür.

``` http
HTTP/1.1 200 OK
4
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>İş akışı için akıllı anlaşma örneklerini listeleme

Sözleşmeleri kullanarak bir iş akışının tüm akıllı sözleşme örneklerini göstermek için [API al](/rest/api/azure-blockchain-workbench/contractsv2/contractsget) . Ya da kullanıcıların gösterilen akıllı sözleşme örneklerinin herhangi birine derinlemesine bir şekilde gidebilsin.

### <a name="contracts-request"></a>Sözleşme isteği

Bu örnekte, kullanıcının bir eylem yapmak için akıllı anlaşma örneklerinden biriyle etkileşimli çalışmak istediğini düşünün.

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

### <a name="contracts-response"></a>Sözleşme yanıtı

Yanıt, belirtilen iş akışının tüm akıllı sözleşme örneklerini listeler. Çalışma ekranı yöneticileri tüm akıllı sözleşme örneklerini alır. Çalışma ekranı olmayan yöneticiler, en az bir ilişkili uygulama rolüne sahip oldukları veya bir akıllı sözleşme örneği rolüyle ilişkilendirilen her akıllı sözleşme örneğini alır.

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

Sözleşmenin durumuna verilen kullanıcı eylemlerini göstermek için [API al sözleşme eylemini](/rest/api/azure-blockchain-workbench/contractsv2/contractactionget) kullanın. 

### <a name="contract-action-request"></a>Sözleşme eylemi isteği

Bu örnekte, Kullanıcı oluşturdukları yeni bir akıllı sözleşme için tüm kullanılabilir eylemlere bakıyor.

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

### <a name="contract-action-response"></a>Sözleşme eylem yanıtı

Yanıtta belirtilen akıllı anlaşma örneğinin geçerli durumuna göre kullanıcının gerçekleştirebileceği tüm eylemler listelenir.

* Modify: Kullanıcının, bir varlığın açıklamasını ve fiyatını değiştirmesine olanak tanır.
* Sonlandır: kullanıcının varlık sözleşmesini sonlandırmasına Izin verir.

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

Belirtilen akıllı sözleşme örneği için işlem gerçekleştirmek üzere [sözleşme EYLEMI gönderi API 'sini](/rest/api/azure-blockchain-workbench/contractsv2/contractactionpost) kullanın.

### <a name="contract-action-post-request"></a>Sözleşme eylemi POST isteği

Bu durumda, bir kullanıcının bir varlığın açıklamasını ve fiyatını değiştirmek istediğiniz senaryoyu göz önünde bulundurun.

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

### <a name="contract-action-post-response"></a>Sözleşme eylemi GÖNDERI yanıtı

Gönderi başarılı olursa, yanıt gövdesi olmayan bir HTTP 200 OK yanıtı döndürülür.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Sonraki adımlar

Blok zinciri çalışma ekranı API 'Leri hakkında başvuru bilgileri için bkz. [Azure blok zinciri çalışma ekranı REST API başvurusu](https://docs.microsoft.com/rest/api/azure-blockchain-workbench).
