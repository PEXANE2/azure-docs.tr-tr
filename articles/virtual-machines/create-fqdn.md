---
title: Azure portal VM için FQDN oluşturma
description: Azure portal bir sanal makine için tam etki alanı adı (FQDN) oluşturmayı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 11/03/2020
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08d5e20bf0755a71e70a0e446cf96d33bb42ad59
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93351931"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Linux VM için Azure portal tam etki alanı adı oluşturma

[Azure Portal](https://portal.azure.com)bir sanal makıne (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. Bu IP adresini kullanarak VM 'ye uzaktan erişin. Portal [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN OLUŞTURMASA da VM oluşturulduktan sonra bir tane ekleyebilirsiniz. Bu makalede bir DNS adı veya FQDN oluşturma adımları gösterilmektedir. 

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturmuş olduğunuz varsayılmaktadır. Gerekirse, portalda bir [Linux](./linux/quick-create-portal.md) veya [Windows](./windows/quick-create-portal.md) VM oluşturabilirsiniz. VM 'niz çalışır duruma getirildikten sonra aşağıdaki adımları izleyin:


1. Portalda VM 'nizi seçin. **DNS adı** altında **Yapılandır** ' ı seçin.
2. DNS adını girip sayfanın en üstünde bulunan **Kaydet** ' i seçin.
3. VM 'ye genel bakış dikey penceresine dönmek için sağ üst köşedeki **X** ' i seçerek **yapılandırma** dikey penceresini kapatın. 
4. *DNS adının* artık doğru gösterildiğini doğrulayın.
   



## <a name="next-steps"></a>Sonraki adımlar
Artık VM 'nizin genel IP ve DNS adı olduğuna göre, NGINX, MongoDB ve Docker gibi ortak uygulama çerçevelerini veya hizmetlerini dağıtabilirsiniz.

Ayrıca, Azure dağıtımlarınızı oluşturmaya yönelik ipuçları için [Kaynak Yöneticisi kullanma](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinebilirsiniz.

