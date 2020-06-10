---
title: Gerekli alt ağ boyutunu & aralığı belirle
titleSuffix: Azure SQL Managed Instance
description: Bu konu başlığı altında, Azure SQL yönetilen örneğinin dağıtılacağı alt ağın boyutunun nasıl hesaplanacağı açıklanır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 02/22/2019
ms.openlocfilehash: 2ad140e7f0040552cbdc2490d25a07f3d1184720
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84657872"
---
# <a name="determine-required-subnet-size--range-for-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği için gereken alt ağ boyutunu & aralığını belirleme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneğinin bir Azure [sanal ağı (VNet)](../../virtual-network/virtual-networks-overview.md)içinde dağıtılması gerekir.

Bir sanal ağın alt ağında dağıtılabilecek yönetilen örneklerin sayısı alt ağın boyutuna bağlıdır (alt ağ aralığı).

Yönetilen bir örnek oluşturduğunuzda Azure, sağlama sırasında seçtiğiniz katmana bağlı olarak bir dizi sanal makine ayırır. Bu sanal makineler alt ağınızda ilişkili olduğundan, IP adresleri gerektirir. Düzenli işlemler ve hizmet bakımı sırasında yüksek kullanılabilirlik sağlamak için Azure, ek sanal makineler ayırabilir. Sonuç olarak, bir alt ağdaki gerekli IP adreslerinin sayısı o alt ağdaki yönetilen örnek sayısından daha büyük.

Tasarıma göre, yönetilen bir örnek bir alt ağda en az 16 IP adresine ihtiyaç duyuyor ve en fazla 256 IP adresi kullanıyor olabilir. Sonuç olarak, alt ağ IP aralıklarını tanımlarken/28 ile/24 arasında bir alt ağ maskesi kullanabilirsiniz. Ağ maskesi biti/28 (ağ başına 14 ana bilgisayar), tek bir genel amaçlı veya iş açısından kritik dağıtım için iyi bir boyutdir. Bir maske bit/27 (ağ başına 30 ana bilgisayar), aynı VNet içinde birden çok SQL yönetilen örnek dağıtımı için idealdir. Maske bit ayarları/26 (62 ana bilgisayar) ve/24 (254 ana bilgisayar), ek yönetilen örnekleri desteklemek için VNet 'ten daha fazla ölçeklendirme yapılmasına izin verir.

> [!IMPORTANT]
> 16 IP adresi olan bir alt ağ boyutu, sanal çekirdek boyutu değişikliği gibi bir ölçeklendirme işleminin desteklenmediği sınırlı potansiyel olan en düşük gereksinimdir. Önek/27 veya en uzun ön ek olan alt ağ seçilmesi kesinlikle önerilir.

## <a name="determine-subnet-size"></a>Alt ağ boyutunu belirle

Alt ağ içinde birden fazla yönetilen örnek dağıtmayı planlıyorsanız ve alt ağ boyutunu iyileştirmeniz gerekiyorsa, bir hesaplama oluşturmak için bu parametreleri kullanın:

- Azure, alt ağda kendi ihtiyaçları için beş IP adresi kullanır
- Her genel amaçlı örneğin iki adresi olmalıdır
- İş açısından kritik her örnek için dört adres gerekir

**Örnek**: üç genel amaçlı ve iki iş açısından kritik yönetilen örneğe sahip olmak için plan yapın. Bu, 5 + 3 * 2 + 2 * 4 = 19 IP adresiniz olması gerektiği anlamına gelir. IP aralıkları 2 ' nin gücüne göre tanımlandığından, IP aralığı 32 (2 ^ 5) IP adresi olmalıdır. Bu nedenle, alt ağı/27 alt ağ maskesiyle ayırmanız gerekir.

> [!IMPORTANT]
> Yukarıda görüntülenecek hesaplama, daha fazla geliştirmelerle kullanım dışı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- [SQL yönetilen örneği için bağlantı mimarisi](connectivity-architecture-overview.md)hakkında daha fazla bilgi edinin.
- Bkz. [SQL yönetilen örneği dağıtacağınız sanal ağ oluşturma](virtual-network-subnet-create-arm-template.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](custom-dns-configure.md).
