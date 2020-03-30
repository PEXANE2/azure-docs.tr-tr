---
title: Yönetilen örnek VNet/subnet boyutunu belirler
description: Bu konu, Azure SQL Veritabanı Yönetilen Örnekleri'nin dağıtılanınacağı alt net boyutununasıl hesaplanacağını açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 02/22/2019
ms.openlocfilehash: 7f0ef26343284b7b668e71676114586f4bec8b9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73825749"
---
# <a name="determine-vnet-subnet-size-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için VNet alt net boyutunu belirleme

Azure SQL Veritabanı Yönetilen Örnek, bir Azure [sanal ağı (VNet)](../virtual-network/virtual-networks-overview.md)içinde dağıtılmalıdır.

VNet'in alt ağına dağıtılabilen Yönetilen Örneklerin sayısı alt netin (alt ağ aralığı) boyutuna bağlıdır.

Yönetilen Örnek oluşturduğunuzda, Azure sağlama sırasında seçtiğiniz katmana bağlı olarak bir dizi sanal makine ayırır. Bu sanal makineler alt ağınızla ilişkili olduğundan IP adresleri gerektirir. Azure, düzenli işlemler ve servis bakımı sırasında yüksek kullanılabilirlik sağlamak için ek sanal makineler tahsis edebilir. Sonuç olarak, bir alt ağdaki gerekli IP adreslerinin sayısı, o alt ağdaki Yönetilen Örneklerin sayısından daha büyüktür.

Tasarım gereği, Yönetilen Örnek bir alt ağda en az 16 IP adresine ihtiyaç duyar ve en fazla 256 IP adresi kullanabilir. Sonuç olarak, alt ağ IP aralıklarınızı tanımlarken /28 ve /24 arasında bir alt ağ maskeleri kullanabilirsiniz. Ağ maskesi biti /28 (ağ başına 14 ana bilgisayar) tek bir genel amaç veya iş açısından kritik dağıtım için iyi bir boyuttır. Bir maske parçası /27 (ağ başına 30 ana bilgisayar) aynı VNet içinde birden çok Yönetilen Örnek dağıtımları için idealdir. /26 (62 ana bilgisayar) ve /24 (254 ana bilgisayar) maske bitayarları, ek Yönetilen Örnekleri desteklemek için VNet'ten daha fazla ölçekleme yapılmasına olanak tanır.

> [!IMPORTANT]
> 16 IP adresine sahip bir alt ağ boyutu, vCore boyutu değişikliği gibi bir ölçekleme işleminin desteklenmediğinin sınırlı potansiyeli olan en düşük minimum boyutdur. /27 veya en uzun öneki ile alt net seçimi şiddetle tavsiye edilir.

## <a name="determine-subnet-size"></a>Alt net boyutunu belirleme

Alt ağ içinde birden çok Yönetilen Örnek dağıtmayı planlıyorsanız ve alt ağ boyutunda en iyi duruma getirmeniz gerekiyorsa, hesaplama oluşturmak için bu parametreleri kullanın:

- Azure, kendi ihtiyaçları için alt ağda beş IP adresi kullanır
- Her Genel Amaç örneğinin iki adrese ihtiyacı vardır
- Her İş Kritik örneği dört adrese ihtiyaç duyar

**Örnek**: Üç Genel Amaç ve iki İş Kritik Yönetilen Örneği yapmayı planlıyorsunuz. Bu 5 + 3 * 2 + 2 * 4 = 19 IP adresleri gerekir anlamına gelir. IP aralıkları 2 gücünde tanımlandığı için 32 (2^5) IP adresine ihtiyacınız vardır. Bu nedenle, /27 alt net maskesi ile alt ağı rezerve etmek gerekir.

> [!IMPORTANT]
> Yukarıda görüntülenen hesaplama daha fazla iyileştirmeler ile eski haline gelecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için [bkz.](sql-database-managed-instance.md)
- [Yönetilen Örnek için Bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)hakkında daha fazla bilgi edinin.
- [Yönetilen Örnekleri dağıtacağınız VNet'i nasıl oluşturacağınızı](sql-database-managed-instance-create-vnet-subnet.md) görün
- DNS sorunları için [bkz.](sql-database-managed-instance-custom-dns.md)
