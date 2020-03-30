---
title: Azure portalı ile zonlu bir Windows VM oluşturma
description: Azure portalı ile kullanılabilirlik bölgesinde Windows VM oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: c6f843693ccfa16f9b31027ba370242e0462b138
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033886"
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-the-azure-portal"></a>Azure portalı ile kullanılabilirlik bölgesinde bir Windows sanal makinesi oluşturma

Bu makale, Azure kullanılabilirlik bölgesinde sanal bir makine oluşturmak için Azure portalını kullanma adımlarını oluşturur. [Kullanılabilirlik alanı](../../availability-zones/az-overview.md), bir Azure bölgesinde fiziksel olarak ayrılmış bir alandır. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Kullanılabilirlik alanı kullanmak için, [desteklenen bir Azure bölgesinde](../../availability-zones/az-overview.md#services-support-by-region) sanal makinenizi oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 

3. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. Kullanılabilirlik bölgelerini destekleyen Doğu ABD 2 gibi bir Konum seçin. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/create-portal-availability-zone/create-windows-vm-portal-basic-blade.png)

4. VM için bir boyut seçin. Önerilen boyutu seçin veya özelliklere göre filtre uygulayın. Boyutun kullanmak istediğiniz bölgede kullanılabildiğinizi onaylayın.

    ![VM boyutu seçin](./media/create-portal-availability-zone/create-windows-vm-portal-sizes.png)  

5. **Ayarlar** > **Yüksek kullanılabilirlik**altında, **Kullanılabilirlik bölgesi** açılır tarihinden numaralı bölgelerden birini seçin, kalan varsayılanları tutun ve **Tamam'ı**tıklatın.

    ![Kullanılabilirlik bölgesi seçin](./media/create-portal-availability-zone/create-windows-vm-portal-availability-zone.png)

6. Özet sayfasında, sanal makine dağıtımını başlatmak için **Oluştur'u** tıklatın.

7. VM, Azure portalı panosuna sabitlenir. Dağıtım tamamlandıktan sonra VM özeti otomatik olarak açılır.

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Yönetilen disk ve IP adresi için onay bölgesi

VM bir kullanılabilirlik bölgesinde dağıtıldığında, Aynı kullanılabilirlik bölgesinde VM için yönetilen bir disk oluşturulur. Varsayılan olarak, bu bölgede ortak bir IP adresi de oluşturulur.

Portaldaki bu kaynakların bölge ayarlarını onaylayabilirsiniz.  

1. **Kaynak gruplarını** ve ardından *myResourceGroup*gibi VM için kaynak grubunun adını tıklatın.

2. Disk kaynağının adını tıklatın. **Genel Bakış** sayfası, kaynağın konumu ve kullanılabilirlik bölgesi yle ilgili ayrıntıları içerir.

    ![Yönetilen disk için kullanılabilirlik bölgesi](./media/create-portal-availability-zone/create-windows-vm-portal-disk.png)

3. Genel IP adresi kaynağının adını tıklatın. **Genel Bakış** sayfası, kaynağın konumu ve kullanılabilirlik bölgesi yle ilgili ayrıntıları içerir.

    ![IP adresi için kullanılabilirlik bölgesi](./media/create-portal-availability-zone/create-windows-vm-portal-ip.png)



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanılabilirlik alanında nasıl sanal makine oluşturulacağını öğrendiniz. Azure VM'lerin [kullanılabilirliği](availability.md) hakkında daha fazla bilgi edinin.
