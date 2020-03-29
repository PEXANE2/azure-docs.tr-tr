---
title: REST API ile Azure NetApp Dosyaları için geliştirin | Microsoft Dokümanlar
description: Azure NetApp Files REST API'yi kullanmaya nasıl başnan açıklar.
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
ms.date: 05/17/2019
ms.author: b-juche
ms.openlocfilehash: 996fbcc7c3c9af0da9160216785ecd54840660e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65957030"
---
# <a name="develop-for-azure-netapp-files-with-rest-api"></a>REST API'li Azure NetApp Dosyaları için geliştirin 

Azure NetApp Files hizmetinin REST API'si, NetApp hesabı, kapasite havuzu, birimler ve anlık görüntüler gibi kaynaklara karşı HTTP işlemlerini tanımlar. Bu makale, Azure NetApp Files REST API'yi kullanmaya başlamanıza yardımcı olur.

## <a name="azure-netapp-files-rest-api-specification"></a>Azure NetApp Dosyaları REST API belirtimi

Azure NetApp Dosyaları için REST API belirtimi [GitHub](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager)üzerinden yayınlanır:

`https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager`


## <a name="access-the-azure-netapp-files-rest-api"></a>Azure NetApp Dosyalarına ErişREST API  

1. Bunu daha önce yapmadıysanız [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
2. Azure Etkin Dizininizde (Azure AD) bir hizmet ilkesi oluşturun:
   1. [Yeterli izinlere](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions)sahip olduğunuzu doğrulayın.

   1. Azure CLI'ye aşağıdaki komutu girin:  

           az ad sp create-for-rbac --name $YOURSPNAMEGOESHERE--password $YOURGENERATEDPASSWORDGOESHERE

      Komut çıktısı aşağıdaki örneğe benzer:  

           { 
               "appId": "appIDgoeshere", 
               "displayName": "APPNAME", 
               "name": "http://APPNAME", 
               "password": "supersecretpassword", 
               "tenant": "tenantIDgoeshere" 
           } 

      Komut çıktısını koruyun.  "Ve `appId` `password` `tenant` değerlere" ihtiyacınız olacaktır. 

3. OAuth erişim jetonunu isteyin:

    Bu makaledeki örnekler cURL'yi kullanır.  Ayrıca [Postacı,](https://www.getpostman.com/) [Uykusuzluk](https://insomnia.rest/)ve [Paw](https://paw.cloud/)gibi çeşitli API araçları kullanabilirsiniz.  

    Aşağıdaki örnekteki değişkenleri yukarıdaki Adım 2'den gelen komut çıktısıyla değiştirin. 

        curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token

    Çıktı aşağıdaki örneğe benzer bir erişim belirteci sağlar:

        eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9

    Görüntülenen belirteç 3600 saniye için geçerlidir. Bundan sonra, yeni bir belirteç talep etmek gerekir. 
    Belirteci bir metin düzenleyicisine kaydedin.  Bir sonraki adım için ihtiyacınız olacak.

4. Bir test çağrısı gönderin ve REST API'ye erişiminizi doğrulamak için belirteci ekleyin:

        curl -X GET -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/[SUBSCRIPTION_ID]/providers/Microsoft.Web/sites?api-version=2016-08-01

## <a name="examples-using-the-api"></a>API'yi kullanan örnekler  

Bu makalede, isteklerin temeli için aşağıdaki URL kullanır. Bu URL, Azure NetApp Dosyaları ad alanının köküne işaret edin. 

`https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2017-08-15`

Aşağıdaki örneklerdeki `subID` `resourceGroups` değerleri ve değerleri kendi değerlerinizle değiştirmelisiniz. 

### <a name="get-request-examples"></a>İstek örnekleri alın

Aşağıdaki örneklerin gösterdiği gibi, bir abonelikteki Azure NetApp Dosyalarının nesnelerini sorgulamak için GET isteği kullanırsınız: 

        #get NetApp accounts 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2017-08-15

        #get capacity pools for NetApp account 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools?api-version=2017-08-15

        #get volumes in NetApp account & capacity pool 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes?api-version=2017-08-15

        #get snapshots for a volume 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/VOLUMEGOESHERE/snapshots?api-version=2017-08-15

### <a name="put-request-examples"></a>PUT istek örnekleri

Aşağıdaki örneklerde olduğu gibi Azure NetApp Dosyalarında yeni nesneler oluşturmak için PUT isteği kullanırsınız. PUT isteğinin gövdesi değişiklikler için JSON biçimlendirilmiş verileri içerebilir veya okunacak bir dosya belirtebilir. 

        #create a NetApp account  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE?api-version=2017-08-15

        #create a capacity pool  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE?api-version=2017-08-15

        #create a volume  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME?api-version=2017-08-15

        #create a volume snapshot  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME/Snapshots/SNAPNAME?api-version=2017-08-15

### <a name="json-examples"></a>JSON örnekleri

Aşağıdaki örnekte, NetApp hesabının nasıl oluşturulabildiğini gösterilmektedir:

    { 
        "name": "MYNETAPPACCOUNT", 
        "type": "Microsoft.NetApp/netAppAccounts", 
        "location": "westus2", 
        "properties": { 
            "name": "MYNETAPPACCOUNT" 
        }
    } 

Aşağıdaki örnek, kapasite havuzunun nasıl oluşturulabildiğini gösterir: 

    {
        "name": "MYNETAPPACCOUNT/POOLNAME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools",
        "location": "westus2",
        "properties": {
            "name": "POOLNAME"
            "size": "4398046511104",
            "serviceLevel": "Premium"
        }
    }

Aşağıdaki örnekte, yeni bir birimin nasıl oluşturulacak olduğu gösterilmektedir: 

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

Aşağıdaki örnek, bir birimin anlık görüntüsününasıl oluşturulacak gösterilmektedir: 

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
> Anlık görüntü `fileSystemId` oluşturmak için belirtmeniz gerekir.  Bir GET `fileSystemId` isteği yle değeri bir birimden elde edebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure NetApp Dosyaları REST API başvurusuna bakın](https://docs.microsoft.com/rest/api/netapp/)
