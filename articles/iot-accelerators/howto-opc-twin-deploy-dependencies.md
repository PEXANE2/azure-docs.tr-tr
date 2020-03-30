---
title: Azure'da OPC İkiz bulut bağımlılıkları nasıl dağıtılır | Microsoft Dokümanlar
description: Bu makalede, yerel geliştirme ve hata ayıklama yapmak için gereken OPC İkiz Azure bağımlılıklarının nasıl dağıtılanılacağa açıklanmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 231d1efa02ec80e8ad56a8895d4262d774480111
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73824107"
---
# <a name="deploying-dependencies-for-local-development"></a>Yerel kalkınma için bağımlılıkları dağıtma

Bu makalede, yalnızca yerel geliştirme ve hata ayıklama yapmak için gereken Azure Platform Hizmetlerinin nasıl dağıtılancağa açıklanmaktadır.   Sonunda, yerel geliştirme ve hata ayıklama için ihtiyacınız olan her şeyi içeren bir kaynak grubu dağıtılır.

## <a name="deploy-azure-platform-services"></a>Azure platform hizmetlerini dağıtma

1. PowerShell ve [AzureRM PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) uzantılarının yüklü olduğundan emin olun.  Komut istemini veya terminali açın ve çalıştırın:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Dağıtımınız için kaynak grubuna bir ad atamak için istemleri izleyin.  Komut dosyası, yalnızca Azure aboneliğinizdeki bu kaynak grubuna olan bağımlılıkları dağıtır, ancak mikro hizmetleri dağıtmaz.  Komut dosyası, Azure Etkin Dizini'nde bir Uygulama da kaydeder.  Bu, OAUTH tabanlı kimlik doğrulamasını desteklemek için gereklidir.  Dağıtım birkaç dakika sürebilir.

3. Komut dosyası tamamlandıktan sonra .env dosyasını kaydetmeyi seçebilirsiniz.  .env ortamı dosyası, geliştirme makinenizde çalıştırmak istediğiniz tüm hizmetlerin ve araçların yapılandırma dosyasıdır.  

## <a name="troubleshooting-deployment-failures"></a>Sorun giderme dağıtım hataları

### <a name="resource-group-name"></a>Kaynak grubu adı

Kısa ve basit bir kaynak grubu adı kullandığınızdan emin olun.  Ad, kaynak adlandırma gereksinimlerine uyması gerektiği gibi kaynakları adlandırmak için de kullanılır.  

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory (AAD) kaydı

Dağıtım komut dosyası, AAD uygulamalarını Azure Etkin Dizini'nde kaydetmeye çalışır.  Seçili AAD kiracısı üzerindeki haklarınıza bağlı olarak, bu başarısız olabilir.   Üç seçenek vardır:

1. Kiracılar listesinden bir AAD kiracıseçtiyseniz, komut dosyasını yeniden başlatın ve listeden farklı bir tane seçin.
2. Alternatif olarak, özel bir AAD kiracısı dağıtın, komut dosyasını yeniden başlatın ve kullanmayı seçin.
3. Kimlik Doğrulama olmadan devam edin.  Mikro hizmetlerinizi yerel olarak çalıştırdığınıziçin, bu kabul edilebilir, ancak üretim ortamlarını taklit etmez.  

## <a name="next-steps"></a>Sonraki adımlar

OPC Twin hizmetlerini varolan bir projeye başarıyla dağıttığınız için, önerilen bir sonraki adım aşağıda veda edilmiştir:

> [!div class="nextstepaction"]
> [OPC Twin modüllerini nasıl dağıtabilirsiniz hakkında bilgi edinin](howto-opc-twin-deploy-modules.md)
