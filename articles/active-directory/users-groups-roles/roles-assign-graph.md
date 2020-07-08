---
title: Microsoft Graph API 'SI ile Azure AD yönetici rolleri atama | Microsoft Docs
description: Azure Active Directory Graph API Azure AD yönetici rolleri atama ve kaldırma
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 04/29/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44299a55424f9b0338ee49d2742aeedf16db22e8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84732098"
---
# <a name="assign-custom-admin-roles-using-the-microsoft-graph-api-in-azure-active-directory"></a>Azure Active Directory Microsoft Graph API 'sini kullanarak özel yönetici rolleri atama 

Microsoft Graph API kullanarak Kullanıcı hesaplarına roller atamayı otomatik hale getirebilirsiniz. Bu makalede Roleatamalarda GÖNDERI, GET ve DELETE işlemleri ele alınmaktadır.

## <a name="required-permissions"></a>Gerekli izinler

Rolleri atamak veya kaldırmak için genel yönetici hesabı veya ayrıcalıklı Kimlik Yöneticisi kullanarak Azure AD kuruluşunuza bağlanın.

## <a name="post-operations-on-roleassignment"></a>Roleatama üzerinde POST Işlemleri

Bir kullanıcı ile rol tanımı arasında rol ataması oluşturmak için HTTP isteği.

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
    "resourceScopes":"/"
}
```

Yanıt

``` HTTP
HTTP/1.1 201 Created
```

Sorumlu veya rol tanımının bulunmadığı bir rol ataması oluşturmak için HTTP isteği

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Gövde

``` HTTP
{
    "principalId":" 2142743c-a5b3-4983-8486-4532ccba12869",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/"
}
```

Yanıt

``` HTTP
HTTP/1.1 404 Not Found
```

Yerleşik rol tanımında tek bir kaynak kapsamlı rol ataması oluşturmak için HTTP isteği.

> [!NOTE] 
> Günümüzde yerleşik roller, yalnızca "/" kuruluş genelindeki kapsam veya "/AU/*" kapsamı kapsamında olabilecek bir sınırlamaya sahiptir. Tek kaynak kapsamı, yerleşik roller için çalışmaz, ancak özel roller için çalışır.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Gövde

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/ab2e1023-bddc-4038-9ac1-ad4843e7e539"
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

## <a name="get-operations-on-roleassignment"></a>Roleatama 'da Işlemleri al

Belirli bir sorumlu için rol ataması almak için HTTP isteği

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
    "resourceScopes":"/"
} ,
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":"/"
}
```

Belirli bir rol tanımı için rol ataması almak için HTTP isteği.

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
    "resourceScopes":"/"
}
```

KIMLIĞE göre rol ataması almak için HTTP isteği.

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
    "resourceScopes":"/"
}
```

## <a name="delete-operations-on-roleassignment"></a>Roleatamasında SILME Işlemleri

Bir kullanıcı ile rol tanımı arasında rol atamasını silmek için HTTP isteği.

DELETE

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Yanıt
``` HTTP
HTTP/1.1 204 No Content
```

Artık mevcut olmayan bir rol atamasını silmek için HTTP isteği

DELETE

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Yanıt

``` HTTP
HTTP/1.1 404 Not Found
```

Self ve yerleşik rol tanımı arasında rol atamasını silmek için HTTP isteği

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

* [Azure AD Yönetim rolleri forumundan](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşabilirsiniz.
* Roller ve yönetici rolü atama hakkında daha fazla bilgi için bkz. [yönetici rolleri atama](directory-assign-admin-roles.md).
* Varsayılan Kullanıcı izinleri için bkz. [varsayılan Konuk ve üye Kullanıcı izinlerinin karşılaştırması](../fundamentals/users-default-permissions.md).
