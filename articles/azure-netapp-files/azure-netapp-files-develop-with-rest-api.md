---
title: REST API için geliştirme Azure NetApp Files | Microsoft Docs
description: Azure NetApp Files REST API kullanmaya nasıl başlaılacağını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: b-juche
ms.openlocfilehash: fa243071b9e5dcec0e49c3b49b3bf77ec6fe0e65
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299613"
---
# <a name="develop-for-azure-netapp-files-with-rest-api"></a>REST API ile Azure NetApp Files için geliştirme 

Azure NetApp Files hizmetinin REST API, NetApp hesabı, kapasite havuzu, birimler ve anlık görüntüler gibi kaynaklara karşı HTTP işlemlerini tanımlar. Bu makale, Azure NetApp Files REST API kullanmaya başlamanıza yardımcı olur.

## <a name="azure-netapp-files-rest-api-specification"></a>Azure NetApp Files REST API belirtimi

Azure NetApp Files için REST API belirtimi [GitHub](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager)aracılığıyla yayımlanır:

`https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager`


## <a name="access-the-azure-netapp-files-rest-api"></a>Azure NetApp Files erişin REST API  

1. Daha önce yapmadıysanız [Azure CLI 'Yı yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) .
2. Azure Active Directory bir hizmet sorumlusu oluşturun (Azure AD):
   1. [Yeterli izinlere](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions)sahip olduğunuzu doğrulayın.

   2. Azure CLı 'de aşağıdaki komutu girin:  

           az ad sp create-for-rbac --name $YOURSPNAMEGOESHERE

      Komut çıktısı aşağıdaki örneğe benzer:  

           { 
               "appId": "appIDgoeshere", 
               "displayName": "APPNAME", 
               "name": "http://APPNAME", 
               "password": "supersecretpassword", 
               "tenant": "tenantIDgoeshere" 
           } 

      Komut çıkışını saklayın.  `appId`, Ve değerlerine ihtiyacınız olacaktır `password` `tenant` . 

3. Bir OAuth erişim belirteci isteyin:

    Bu makaledeki örneklerde kıvrımlı kullanılır. [Postman](https://www.getpostman.com/), [Insomnia](https://insomnia.rest/)ve [Paw](https://paw.cloud/)gibi çeşitli API araçlarını da kullanabilirsiniz.  

    Aşağıdaki örnekteki değişkenleri yukarıdaki 2. adımdaki komut çıktısı ile değiştirin. 

        curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token

    Çıktı aşağıdaki örneğe benzer bir erişim belirteci sağlar:

        eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9

    Görüntülenmiş belirteç 3600 saniye için geçerlidir. Bundan sonra yeni bir belirteç istemeniz gerekir. 
    Belirteci bir metin düzenleyicisine kaydedin.  Bir sonraki adımda ihtiyacınız olacak.

4. REST API erişiminizi doğrulamak için bir test çağrısı gönderin ve belirteci ekleyin:

        curl -X GET -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/[SUBSCRIPTION_ID]/providers/Microsoft.Web/sites?api-version=2019-11-01

## <a name="examples-using-the-api"></a>API kullanan örnekler  

Bu makale, istek taban çizgisi için aşağıdaki URL 'YI kullanır. Bu URL, Azure NetApp Files ad alanının köküne işaret eder. 

`https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2019-11-01`

`SUBIDGOESHERE` `RESOURCEGROUPGOESHERE` Aşağıdaki örneklerde ve değerlerini kendi değerlerinizle değiştirmelisiniz. 

### <a name="get-request-examples"></a>İstek örneklerini al

Aşağıdaki örneklerde gösterildiği gibi, bir abonelikte Azure NetApp Files nesnelerini sorgulamak için GET isteği kullanırsınız: 

        #get NetApp accounts 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2019-11-01

        #get capacity pools for NetApp account 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools?api-version=2019-11-01

        #get volumes in NetApp account & capacity pool 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes?api-version=2019-11-01

        #get snapshots for a volume 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/VOLUMEGOESHERE/snapshots?api-version=2019-11-01

### <a name="put-request-examples"></a>PUT isteği örnekleri

Aşağıdaki örneklerde gösterildiği gibi Azure NetApp Files yeni nesneler oluşturmak için bir PUT isteği kullanırsınız. PUT isteğinin gövdesi, değişiklikler için JSON biçimli verileri içerebilir. Metin veya başvuru olarak bir dosya olarak, kıvrımlı komutuna eklenmelidir. Gövdeye dosya olarak başvurmak için JSON örneğini bir dosyaya kaydedin ve `-d @<filename>` kıvrımlı komutuna ekleyin.

        #create a NetApp account  
        curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE?api-version=2019-11-01

        #create a capacity pool  
        curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE?api-version=2019-11-01

        #create a volume  
        curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME?api-version=2019-11-01

        #create a volume snapshot  
        curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME/Snapshots/SNAPNAME?api-version=2019-11-01

### <a name="json-examples"></a>JSON örnekleri

Aşağıdaki örnekte bir NetApp hesabının nasıl oluşturulacağı gösterilmektedir:

    { 
        "name": "MYNETAPPACCOUNT", 
        "type": "Microsoft.NetApp/netAppAccounts", 
        "location": "westus2", 
        "properties": { 
            "name": "MYNETAPPACCOUNT" 
        }
    } 

Aşağıdaki örnek, bir kapasite havuzunun nasıl oluşturulacağını gösterir: 

    {
        "name": "MYNETAPPACCOUNT/POOLNAME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools",
        "location": "westus2",
        "properties": {
            "name": "POOLNAME",
            "size": "4398046511104",
            "serviceLevel": "Premium"
        }
    }

Aşağıdaki örnek, yeni bir birimin nasıl oluşturulacağını göstermektedir. (Birim için varsayılan protokol NFSV3 ' dir.) 

    {
        "name": "MYNEWVOLUME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/volumes",
        "location": "westus2",
        "properties": {
            "serviceLevel": "Premium",
            "usageThreshold": "322122547200",
            "creationToken": "MY-FILEPATH",
            "snapshotId": "",
            "subnetId": "/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.Network/virtualNetworks/VNETGOESHERE/subnets/MYDELEGATEDSUBNET.sn"
            }
    }

Aşağıdaki örnekte, bir birimin anlık görüntüsünün nasıl oluşturulacağı gösterilmektedir: 

    {
        "name": "apitest2/apiPool01/apiVol01/snap02",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots",
        "location": "westus2",
        "properties": {
            "name": "snap02",
            "fileSystemId": "0168704a-bbec-da81-2c29-503825fe7420"
        }
    }

> [!NOTE] 
> `fileSystemId`Anlık görüntü oluşturmak için belirtmeniz gerekir.  `fileSystemId`Değeri bir birime YÖNELIK Get isteğiyle elde edebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure NetApp Files REST API başvurusuna bakın](https://docs.microsoft.com/rest/api/netapp/)
