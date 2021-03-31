---
title: ONNX kullanarak Azure SQL Edge 'de ML modeli dağıtma
description: Bu üç parçalı Azure SQL Edge öğreticisinin üçüncü bir bölümünde, Iron ve yamallikleri tahmin etmek için, SQL Edge 'de ONNX makine öğrenimi modellerini çalıştırırsınız.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 9e5bb037b88b7c370e31d05c2d20fc6f558a8b39
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93422204"
---
# <a name="deploy-ml-model-on-azure-sql-edge-using-onnx"></a>ONNX kullanarak Azure SQL Edge 'de ML modeli dağıtma 

Azure SQL Edge 'de Iron ve yamallikleri tahmin etmek için üç bölümden oluşan Bu öğreticinin üçüncü kısmında şunları yapmanız gerekir:

1. Azure SQL Edge örneğindeki SQL veritabanına bağlanmak için Azure Data Studio kullanın.
2. Azure SQL Edge 'de ONNX ile Iron Ore 'yi tahmin edin.

## <a name="key-components"></a>Anahtar bileşenler

1. Çözüm, Edge hub 'ına gönderilen her ileti arasında varsayılan 500 milisaniye kullanır. Bu, **program. cs** dosyasında değiştirilebilir 
   ```json
   TimeSpan messageDelay = configuration.GetValue("MessageDelay", TimeSpan.FromMilliseconds(500));
   ```
2. Çözüm, aşağıdaki özniteliklere sahip bir ileti oluşturdu. Öznitelikleri gereksinimlere göre ekleyin veya kaldırın. 
```json
{
    timestamp 
    cur_Iron_Feed
    cur_Silica_Feed 
    cur_Starch_Flow 
    cur_Amina_Flow 
    cur_Ore_Pulp_pH
    cur_Flotation_Column_01_Air_Flow
    cur_Flotation_Column_02_Air_Flow
    cur_Flotation_Column_03_Air_Flow
    cur_Flotation_Column_04_Air_Flow
    cur_Flotation_Column_01_Level
    cur_Flotation_Column_02_Level
    cur_Flotation_Column_03_Level
    cur_Flotation_Column_04_Level
    cur_Iron_Concentrate
}
```

## <a name="connect-to-the-sql-database-in-the-azure-sql-edge-instance-to-train-deploy-and-test-the-ml-model"></a>ML modelini eğitmek, dağıtmak ve test etmek için Azure SQL Edge örneğindeki SQL veritabanına bağlanma

1. Azure Data Studio'yu açın.

2. **Hoş geldiniz** sekmesinde, aşağıdaki ayrıntılarla yeni bir bağlantı başlatın:

   |_Alan_|_Değer_|
   |-------|-------|
   |Bağlantı türü| Microsoft SQL Server|
   |Sunucu|Bu tanıtım için oluşturulan VM 'de belirtilen genel IP adresi|
   |Kullanıcı adı|sa|
   |Parola|Azure SQL Edge örneği oluşturulurken kullanılan güçlü parola|
   |Veritabanı|Varsayılan|
   |Sunucu grubu|Varsayılan|
   |Ad (isteğe bağlı)|İsteğe bağlı bir ad sağlayın|

3. **Bağlan** 'a tıklayın

4. Dosya bölümünde, makinenizde proje dosyalarını Klonladığınız klasörden **/Deploymentscripts/MiningProcess_ONNX. Jpynb** **dosyasını** açın.

5. Çekirdeği Python 3 olarak ayarlayın.


## <a name="next-steps"></a>Sonraki adımlar

Azure SQL Edge 'de ONNX modellerini kullanma hakkında daha fazla bilgi için bkz. [SQL Edge 'de onnx Ile Machine Learning ve AI](onnx-overview.md).