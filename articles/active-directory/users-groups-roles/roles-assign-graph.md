---
title: Microsoft Graph API ile Azure AD yönetici rollerini atama | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde Graph API ile Azure AD yöneticisi rollerini atama ve kaldırma
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3632f8a360df8837569104232b7380fdc8383953
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77559156"
---
# <a name="assign-custom-admin-roles-using-the-microsoft-graph-api-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki Microsoft Graph API'sini kullanarak özel yönetici rolleri atama 

Microsoft Graph API'yi kullanarak kullanıcı hesaplarına nasıl rol atadığınızı otomatikleştirebilirsiniz. Bu makale, roleAssignments'taki POST, GET ve DELETE işlemlerini kapsar.

## <a name="required-permissions"></a>Gerekli izinler

Rolleri atamak veya kaldırmak için Bir Global yönetici hesabı veya Ayrıcalıklı Kimlik yöneticisi ni kullanarak Azure AD kiracınıza bağlanın.

## <a name="post-operations-on-roleassignment"></a>RoleAssignment POST İşlemleri

BIR kullanıcı ve rol tanımı arasında rol ataması oluşturmak için HTTP isteği.

POST

``` HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
Content-type: application/json
```

Gövde

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":["/"]
}
```

Yanıt

``` HTTP
HTTP/1.1 201 Created
```

Http, asıl veya rol tanımının olmadığı bir rol ataması oluşturma isteği

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Gövde

``` HTTP
{
    "principalId":" 2142743c-a5b3-4983-8486-4532ccba12869",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":["/"]
}
```

Yanıt

``` HTTP
HTTP/1.1 404 Not Found
```

HTTP, yerleşik bir rol tanımında tek bir kaynak kapsamlı rol ataması oluşturma isteğinde bulunun.

> [!NOTE] 
> Yerleşik roller, günümüzde yalnızca "/" kuruluş genelindeki kapsamveya "/AU/*" kapsamına dahil edilebilen bir sınırlamaya sahiptir. Tek kaynak kapsam, yerleşik roller için çalışmaz, ancak özel roller için çalışır.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Gövde

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":["/ab2e1023-bddc-4038-9ac1-ad4843e7e539"]
}
```

Yanıt

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Provided authorization scope is not supported for built-in role definitions."},
            "values":
            [
                {
                    "item":"scope",
                    "value":"/ab2e1023-bddc-4038-9ac1-ad4843e7e539"
                }
            ]
        }
    }
}
```

## <a name="get-operations-on-roleassignment"></a>RoleAssignment'da Get İşlemleri

Belirli bir müdür için rol ataması almak için HTTP isteği

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=principalId eq ‘<object-id-of-principal>’
```

Yanıt

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "resourceScopes":["/"]
} ,
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

HTTP belirli bir rol tanımı için bir rol ataması almak için istek.

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Yanıt

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

HTTP kimliğine göre bir rol ataması almak için istek.

GET

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Yanıt

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1",
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "resourceScopes":["/"]
}
```

## <a name="delete-operations-on-roleassignment"></a>RoleAssignment'da DELETE İşlemleri

HTTP, kullanıcı ve rol tanımı arasındaki rol atamasını silme isteğinde bulunun.

DELETE

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Yanıt
``` HTTP
HTTP/1.1 204 No Content
```

ARTıK var olmayan bir rol atamasını silme isteği HTTP

DELETE

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Yanıt

``` HTTP
HTTP/1.1 404 Not Found
```

HTTP isteği kendi ve yerleşik rol tanımı arasında bir rol atamasi silmek için

DELETE

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Yanıt

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Cannot remove self from built-in role definitions."},
            "values":null
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD yönetim rolleri [forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşmakta çekinmeyin.
* Roller ve Yönetici rol ataması hakkında daha fazla şey için [yönetici rollerini atay'a](directory-assign-admin-roles.md)bakın.
* Varsayılan kullanıcı izinleri için varsayılan [konuk ve üye kullanıcı izinlerinin karşılaştırılmasına](../fundamentals/users-default-permissions.md)bakın.
