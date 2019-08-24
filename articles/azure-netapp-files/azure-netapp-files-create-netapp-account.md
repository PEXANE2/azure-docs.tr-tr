---
title: Azure NetApp Files'a erişim için NetApp hesabı oluşturma | Microsoft Docs
description: Azure NetApp Files'a erişme ve bir kapasite havuzu ayarlayıp birim oluşturabilmek için NetApp hesabı oluşturma işlemleri açıklanır.
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
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 8e60f5251f911ffe2b917dcc30e433e0a960823e
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012579"
---
# <a name="create-a-netapp-account"></a>NetApp hesabı oluşturma
NetApp hesabı oluşturmak, kapasite havuzu ayarlamanıza ve ardından birim oluşturmanıza olanak tanır. Yeni NetApp hesabını oluşturmak için Azure NetApp Files dikey penceresini kullanırsınız.

## <a name="before-you-begin"></a>Başlamadan önce
Azure NetApp Files takımdan, hizmete erişim verildiğini onaylayan bir e-posta almış olmanız gerekir. Bkz. [hizmete erişmek için bir eklenebileceğinizi isteği gönderme](azure-netapp-files-register.md#waitlist).

Ayrıca, NetApp kaynak sağlayıcısını kullanmak için aboneliğinizi kaydetmiş olmanız gerekir. Bkz. [NetApp kaynak sağlayıcısını kaydetme](azure-netapp-files-register.md#resource-provider).

## <a name="steps"></a>Adımlar 

1. Azure Portal’da oturum açın. 
2. Aşağıdaki yöntemlerden birini kullanarak Azure NetApp Files dikey penceresine ulaşın:  
   * Azure portalı arama kutusunda **Azure NetApp Files** için arama yapın.  
   * Gezintide **Tüm hizmetler**'e tıklayın ve ardından Azure NetApp Files için filtreleyin.  

   Azure NetApp Files dikey penceresinin yanındaki yıldız simgesine tıklayarak bunu "sık kullanılanlara" ekleyebilirsiniz. 

3. Yeni bir NetApp hesabı oluşturmak için **+ Ekle**'ye tıklayın.  
   Yeni NetApp hesabı penceresi görüntülenir.  

4. NetApp hesabınızla ilgili şu bilgileri sağlayın: 
   * **Hesap adı**  
     Abonelik için benzersiz bir ad belirtin.
   * **Abonelik**  
     Mevcut abonelikleriniz arasından bir abonelik seçin.
   * **Kaynak grubu**   
     Mevcut Kaynak Grubunu kullanın ya da yeni bir tane oluşturun.
   * **Location**  
     Hesabın ve alt kaynaklarının içinde yer almasını istediğiniz bölgeyi seçin.  

     ![Yeni NetApp hesabı](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. **Oluştur**'a tıklayın.     
   Oluşturduğunuz NetApp hesabı artık Azure NetApp Files dikey penceresinde gösterilir. 

> [!NOTE] 
> Azure NetApp Files hizmetine erişim verilmemişse, ilk NetApp hesabını oluşturmaya çalıştığınızda aşağıdaki hatayı alırsınız:  
>
> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"NotFound","message":"{\r\n \"error\": {\r\n \"code\": \"InvalidResourceType\",\r\n \"message\": \"The resource type could not be found in the namespace 'Microsoft.NetApp' for api version '2017-08-15'.\"\r\n }\r\n}"}]}`

## <a name="next-steps"></a>Sonraki adımlar  

* [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure CLı kullanarak Azure NetApp Files kaynaklarını yönetme](https://docs.microsoft.com/cli/azure/netappfiles?view=azure-cli-latest)

