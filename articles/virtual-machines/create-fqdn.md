---
title: Azure portal VM için FQDN oluşturma
description: Azure portal bir sanal makine için tam etki alanı adı (FQDN) oluşturmayı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 1/12/2020
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aadf4000da4ab88a3c3b4dee37d2179eb49d39d7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98132073"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Linux VM için Azure portal tam etki alanı adı oluşturma

[Azure Portal](https://portal.azure.com)bir sanal makıne (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. Bu IP adresini kullanarak VM 'ye uzaktan erişin. Portal [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN OLUŞTURMASA da VM oluşturulduktan sonra bir tane ekleyebilirsiniz. Bu makalede bir DNS adı veya FQDN oluşturma adımları gösterilmektedir. 

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturmuş olduğunuz varsayılmaktadır. Gerekirse, portalda bir [Linux](./linux/quick-create-portal.md) veya [Windows](./windows/quick-create-portal.md) VM oluşturabilirsiniz. VM 'niz çalışır duruma getirildikten sonra aşağıdaki adımları izleyin:


1. Portalda VM 'nizi seçin. 
1. Sol menüde **yapılandırma** ' yı seçin.
1. **DNS adı etiketi** altında kullanmak istediğiniz ön eki girin.
1. Sayfanın üst kısmındaki **Kaydet** ' i seçin.
1. Sol menüdeki **genel bakış** ' ı seçerek VM 'ye genel bakış dikey penceresine dönün. 
1. *DNS adının* doğru göründüğünü doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, [Azure DNS bölgelerini](../dns/dns-getstarted-portal.md)kullanarak DNS yönetebilirsiniz.

