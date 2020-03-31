---
title: Azure Bulut Uyp'taki kaynaklar için yönetilen kimlikleri kullanma
description: Azure Bulut BulutU'nda MSI ile kodu doğrula
services: azure
author: maertendMSFT
ms.author: damaerte
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 04/14/2018
ms.openlocfilehash: a5d49a16324a5a97f4a0507f9abf47ea602ea072
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72328709"
---
# <a name="use-managed-identities-for-azure-resources-in-azure-cloud-shell"></a>Azure Bulut Uyp'ta Azure kaynakları için yönetilen kimlikleri kullanma

Azure Bulut Su Şuru, Azure kaynakları için yönetilen kimliklerle yetkilendirmeyi destekler. Azure hizmetleriyle güvenli bir şekilde iletişim kurmak için erişim belirteçlerini almak için bunu yararlanın.

## <a name="about-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler hakkında
Bulut uygulamaları oluşturmak, bulut hizmetlerine kimlik doğrulaması yapmak için kodunuzda olması gereken kimlik bilgilerini güvenli bir şekilde nasıl yöneteceğinizdir. Cloud Shell'de, komut dosyasının ihtiyaç duyabileceği bir kimlik bilgisi için Key Vault'tan geri alma kimliğini doğrulamanız gerekebilir.

Azure kaynakları için yönetilen kimlikler, Azure hizmetlerine Azure Etkin Dizin'de (Azure AD) otomatik olarak yönetilen bir kimlik vererek bu sorunu çözmeyi kolaylaştırır. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

## <a name="acquire-access-token-in-cloud-shell"></a>Cloud Shell'de erişim jetonu edinin

MSI erişim belirtecinizi bir ortam değişkeni olarak `access_token`ayarlamak için aşağıdaki komutları uygulayın.
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>İşleme belirteci sona erme

Yerel MSI alt sistemi belirteçleri önbelleğe eder. Bu nedenle, istediğiniz sıklıkta arayabilirsiniz ve Azure AD sonuçlarına yalnızca aşağıdaki leri yapabilirse, telefonla arayabilirsiniz:
- önbellekte belirteç olmaması nedeniyle önbellek miss oluşur
- belirteci süresi doldu

Kodunuzda belirteci önbelleğe alırsak, kaynağın belirteç süresinin dolduğunu gösterdiği senaryoları işlemek için hazırlıklı olmalısınız.

Belirteç hatalarını işlemek için, [MSI erişim belirteçlerini kıvırma hakkında MSI sayfasını](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
[MSI hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[MSI VM'lerden erişim belirteçleri edinme](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
