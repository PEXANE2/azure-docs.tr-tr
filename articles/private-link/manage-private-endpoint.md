---
title: Azure'da Özel Bitiş Noktası bağlantısını yönetme
description: Azure'da özel uç nokta bağlantılarını nasıl yönetebilirsiniz öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 62b24b3e2f5c1b89fa7db581ac34cf58381db2a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75452975"
---
# <a name="manage-a-private-endpoint-connection"></a>Özel Uç Nokta bağlantısını yönetme
Azure Private Link, Özel Bağlantı hizmeti tüketicisinin hizmeti tüketmek için servis sağlayıcısına bağlantı isteyebileceği bir onay çağrısı akışı modelinde çalışır. Daha sonra servis sağlayıcı, tüketicinin bağlanmasına izin verip vermemeye karar verebilir. Azure Özel Bağlantı, hizmet sağlayıcılarının kaynakları üzerindeki özel uç nokta bağlantısını yönetmesini sağlar. Bu makalede, Özel Bitiş Noktası bağlantılarının nasıl yönetilenhakkında yönergeler verilmektedir.

![Özel Uç Noktaları Yönetme](media/manage-private-endpoint/manage-private-endpoint.png)

Bir Private Link hizmeti tüketicisi arasından seçim yapabileceğiniz iki bağlantı onay yöntemi vardır:
- **Otomatik**: Hizmet tüketicisinin servis sağlayıcı kaynağında RBAC izinleri varsa, tüketici otomatik onay yöntemini seçebilir. Bu durumda, istek servis sağlayıcı kaynağına ulaştığında, servis sağlayıcıdan herhangi bir işlem gerekmez ve bağlantı otomatik olarak onaylanır. 
- **Manuel**: Aksine, hizmet tüketicisinin hizmet sağlayıcı kaynağında RBAC izinleri yoksa, tüketici manuel onay yöntemini seçebilir. Bu durumda, bağlantı isteği hizmet kaynaklarında **Beklemede**olarak görünür. Bağlantı kurulmadan önce servis sağlayıcısının isteği el ile onaylaması gerekir. El ile durumlarda, hizmet tüketicisi de servis sağlayıcısına daha fazla bağlam sağlamak için istek le birlikte bir ileti belirtebilir. Servis sağlayıcı, tüm Özel Bitiş Noktası bağlantıları için seçim yapmak için aşağıdaki seçeneklere sahiptir: **Onaylı**, **Reddet,** **Kaldır**.

Aşağıdaki tablo, çeşitli hizmet sağlayıcı eylemlerini ve Özel Bitiş Noktaları için ortaya çıkan bağlantı durumlarını gösterir.  Hizmet sağlayıcısı, tüketici müdahalesi olmadan daha sonraki bir zamanda özel uç nokta bağlantısının bağlantı durumunu da değiştirebilir. Eylem, tüketici tarafındaki bitiş noktasının durumunu güncelleştirecektir. 


|Servis Sağlayıcı Eylemi   |Hizmet Tüketici Özel Endpoint Devlet   |Açıklama   |
|---------|---------|---------|
|None    |    Beklemede     |    Bağlantı el ile oluşturulur ve Private Link kaynak sahibi tarafından onay bekliyor.       |
|Onaylama    |  Onaylandı       |  Bağlantı otomatik olarak veya el ile onaylandı ve kullanıma hazır.     |
|Reddet     | Reddedilen        | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi.        |
|Kaldır    |  Bağlantı kesildi       | Bağlantı özel bağlantı kaynak sahibi tarafından kaldırıldı, özel bitiş noktası bilgilendirici hale gelir ve temizlemek için silinmelidir.        |
|   |         |         |
   
## <a name="manage-private-endpoint-connections-on-azure-paas-resources"></a>Azure PaaS kaynaklarında Özel Uç Nokta Bağlantılarını yönetme
Portal, Azure PaaS kaynaklarında özel uç nokta bağlantılarını yönetmek için tercih edilen yöntemdir. Şu anda, Azure PaaS kaynaklarındaki bağlantıları yönetmek için PowerShell/CLI desteğimiz yok.
1. https://portal.azure.com adresinden Azure portalında oturum açın.
2. Özel Bağlantı Merkezi'ne gidin.
3. **Kaynaklar**altında, özel bitiş noktası bağlantılarını yönetmek istediğiniz kaynak türünü seçin.
4. Kaynak türünün her biri için, onunla ilişkili Özel Bitiş Noktası Bağlantılarının sayısını görüntüleyebilirsiniz. Kaynakları gerektiği gibi filtreleyebilirsiniz.
5. Özel bitiş noktası bağlantılarını seçin.  Listelenen bağlantıların altında, yönetmek istediğiniz bağlantıyı seçin. 
6. En üstteki seçeneklerden seçerek bağlantının durumunu değiştirebilirsiniz.

## <a name="manage-private-endpoint-connections-on-a-customerpartner-owned-private-link-service"></a>Müşteri/iş ortağına ait bir Private Link hizmetinde Özel Uç Nokta bağlantılarını yönetme

Azure PowerShell ve Azure CLI, Microsoft İş Ortağı Hizmetlerinde veya müşteriye ait hizmetlerde Özel Bitiş Noktası bağlantılarını yönetmek için tercih edilen yöntemlerdir. Şu anda, Özel Bağlantı hizmetindeki bağlantıları yönetmek için herhangi bir portal desteğimiz yok.  
 
### <a name="powershell"></a>PowerShell 
  
Özel uç nokta bağlantılarını yönetmek için aşağıdaki PowerShell komutlarını kullanın.  
#### <a name="get-private-link-connection-states"></a>Özel Bağlantı bağlantı durumları alın 
Özel `Get-AzPrivateLinkService` Bitiş Noktası bağlantılarını ve durumlarını almak için cmdlet'i kullanın.  
```azurepowershell
Get-AzPrivateLinkService -Name myPrivateLinkService -ResourceGroupName myResourceGroup 
 ```
 
#### <a name="approve-a-private-endpoint-connection"></a>Özel Bitiş Noktası bağlantısını onaylama 
 
Özel `Approve-AzPrivateEndpointConnection` Bitiş Noktası bağlantısını onaylamak için cmdlet'i kullanın. 
 
```azurepowershell
Approve-AzPrivateEndpointConnection -Name myPrivateEndpointConnection -ResourceGroupName myResourceGroup -ServiceName myPrivateLinkService
```
 
#### <a name="deny-private-endpoint-connection"></a>Özel Bitiş Noktası bağlantısını reddet 
 
Özel `Deny-AzPrivateEndpointConnection` Bitiş Noktası bağlantısını reddetmek için cmdlet'i kullanın. 
```azurepowershell
Deny-AzPrivateEndpointConnection -Name myPrivateEndpointConnection -ResourceGroupName myResourceGroup -ServiceName myPrivateLinkService 
```
#### <a name="remove-private-endpoint-connection"></a>Özel Bitiş Noktası Bağlantısını Kaldırma 
 
Özel `Remove-AzPrivateEndpointConnection` Bitiş Noktası bağlantısını kaldırmak için cmdlet'i kullanın. 
```azurepowershell
Remove-AzPrivateEndpointConnection -Name myPrivateEndpointConnection1 -ResourceGroupName myResourceGroup -ServiceName myPrivateLinkServiceName 
```
 
### <a name="azure-cli"></a>Azure CLI 
 
Özel `az network private-link-service update` Bitiş Noktası bağlantılarınızı yönetmek için kullanın. Bağlantı durumu ```azurecli connection-status``` parametrede belirtilir. 
```azurecli
az network private-link-service connection update -g myResourceGroup -n myPrivateEndpointConnection1 --service-name myPLS --connection-status Approved 
```

   

## <a name="next-steps"></a>Sonraki adımlar
- [Özel Uç Noktalar hakkında bilgi edinin](private-endpoint-overview.md)
 
