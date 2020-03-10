---
title: Azure NetApp Files için kaydolun | Microsoft Docs
description: Azure NetApp Files kullanmak için nasıl kayıt yapılacağını açıklar.
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
ms.date: 05/06/2019
ms.author: b-juche
ms.openlocfilehash: 6f5d84dea2e835fd12a062b628181354295ed9f6
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78381304"
---
# <a name="register-for-azure-netapp-files"></a>Azure NetApp Files için kaydolma

> [!IMPORTANT] 
> Azure NetApp Files kaynak sağlayıcısını kaydetmeden önce, Azure NetApp Files ekibinden hizmete erişim verildiğini onaylayan bir e-posta almış olmanız gerekir. 

Bu makalede, hizmeti kullanmaya başlayabilmeniz için Azure NetApp Files nasıl kaydolacağınızı öğrenin.

## <a name="waitlist"></a>Hizmete erişmek için bir eklenebileceğinizi isteği gönder

1. [Azure NetApp Files eklenebileceğinizi gönderim sayfası](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u)aracılığıyla Azure NetApp Files hizmetine erişmek için bir eklenebileceğinizi isteği gönderir. 

    Waitlist kayıt hizmeti, anında hizmet erişimini garanti etmez. 

2. Diğer görevlerle devam etmeden önce Azure NetApp Files ekibinden resmi bir onay e-postası için bekleyin. 

## <a name="resource-provider"></a>NetApp kaynak sağlayıcısını kaydetme

Hizmeti kullanmak için, Azure NetApp Files için Azure Kaynak sağlayıcısını kaydetmeniz gerekir.

> [!NOTE] 
> Hizmet için erişim izni verilmeden de NetApp kaynak sağlayıcısını başarıyla kaydedebileceksiniz. Ancak, erişim yetkilendirmesi olmadan bir NetApp hesabı ya da başka bir Azure NetApp Files kaynağı oluşturmaya yönelik Azure portal veya API isteği aşağıdaki hata ile reddedilir:  
>
> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"NotFound","message":"{\r\n \"error\": {\r\n \"code\": \"InvalidResourceType\",\r\n \"message\": \"The resource type could not be found in the namespace 'Microsoft.NetApp' for api version '2017-08-15'.\"\r\n }\r\n}"}]}`


1. Azure portal sağ üst köşedeki Azure Cloud Shell simgesine tıklayın:

      ![Azure Cloud Shell simgesi](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Azure hesabınızda birden çok aboneliğiniz varsa, Azure NetApp Files için beyaz listeye alınmış olanı seçin:
    
        az account set --subscription <subscriptionId>

3. Azure Cloud Shell konsolunda, aboneliğinizin beyaz listeye eklendiğini doğrulamak için aşağıdaki komutu girin:
    
        az feature list | grep NetApp

   Komut çıktısı şu şekilde görünür:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/ANFGA",  
       "name": "Microsoft.NetApp/ANFGA" 
       
   abonelik KIMLIĞINIZ `<SubID>`.

    `Microsoft.NetApp/ANFGA`özellik adını görmüyorsanız, hizmete erişiminiz yok demektir. Bu adımla durdur. Devam etmeden önce hizmet erişimi istemek üzere [hizmete erişmek için bir eklenebileceğinizi isteği gönderme](#waitlist) konusundaki yönergeleri izleyin. 

4. Azure Cloud Shell konsolunda, Azure Kaynak sağlayıcısı 'nı kaydetmek için aşağıdaki komutu girin: 
    
        az provider register --namespace Microsoft.NetApp --wait

   `--wait` parametresi, konsolun kaydın tamamlanmasını beklemesini ister. Kayıt işleminin tamamlanması biraz zaman alabilir.

5. Azure Cloud Shell konsolunda, Azure Kaynak sağlayıcısı 'nın kaydedildiğini doğrulamak için aşağıdaki komutu girin: 
    
        az provider show --namespace Microsoft.NetApp

   Komut çıktısı şu şekilde görünür:
   
        {
        "id": "/subscriptions/<SubID>/providers/Microsoft.NetApp",
        "namespace": "Microsoft.NetApp", 
        "registrationState": "Registered", 
        "resourceTypes": […. 

   abonelik KIMLIĞINIZ `<SubID>`.  `state` parametre değeri `Registered`gösterir.

6. Azure portal, **abonelikler** dikey penceresine tıklayın.
7. Abonelikler dikey penceresinde abonelik KIMLIĞINIZ ' ne tıklayın. 
8. Abonelik ayarları ' nda, Microsoft. NetApp sağlayıcısı 'nın kayıtlı durumu olduğunu doğrulamak için **kaynak sağlayıcıları** ' na tıklayın: 

      ![Kayıtlı Microsoft. NetApp](../media/azure-netapp-files/azure-netapp-files-registered-resource-providers.png)


## <a name="next-steps"></a>Sonraki adımlar

[NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)