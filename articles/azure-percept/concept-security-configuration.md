---
title: Azure Percept güvenlik duvarı yapılandırması ve güvenlik önerileri
description: Azure Percept güvenlik duvarı yapılandırması ve güvenlik önerileri hakkında daha fazla bilgi edinin
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: conceptual
ms.date: 03/25/2021
ms.openlocfilehash: 086d7ec9d2bcae96ee64745a4382c4748aea291e
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105572570"
---
# <a name="azure-percept-firewall-configuration-and-security-recommendations"></a>Azure Percept güvenlik duvarı yapılandırması ve güvenlik önerileri

Azure Percept ile güvenlik duvarlarını ve genel güvenlik en iyi yöntemlerini yapılandırma hakkında bilgi için aşağıdaki yönergeleri gözden geçirin.

## <a name="configuring-firewalls-for-azure-percept-dk"></a>Azure Percept DK için güvenlik duvarlarını yapılandırma

Ağ Kurulum, Azure Percept DK cihazlarından yapılan bağlantılara açıkça izin vermenizi gerektiriyorsa, aşağıdaki bileşen listesini gözden geçirin.

Bu denetim listesi, güvenlik duvarı kuralları için bir başlangıç noktasıdır:

|URL (* = joker karakter)|Giden TCP bağlantı noktaları|Kullanım|
|-------------------|------------------|---------|
|*. auth.azureperceptdk.azure.net|443|Azure DK SOM kimlik doğrulaması ve yetkilendirme|
|*. auth.projectsantacruz.azure.net|443|Azure DK SOM kimlik doğrulaması ve yetkilendirme|

Ayrıca, [Azure IoT Edge tarafından kullanılan bağlantıların](https://docs.microsoft.com/azure/iot-edge/production-checklist#allow-connections-from-iot-edge-devices)listesini gözden geçirin.

## <a name="additional-recommendations-for-deployment-to-production"></a>Üretime dağıtım için ek öneriler

Azure Percept DK, kutudan çıkan çok çeşitli güvenlik özellikleri sunar. Microsoft, geçerli sürüme dahil edilen güçlü güvenlik özelliklerine ek olarak, üretim dağıtımlarını değerlendirirken aşağıdaki yönergeleri de önerir:

- Cihazın kendisinin güçlü fiziksel koruması
- Rest verisinin etkin olduğundan emin olun
- Cihaz duruşunu sürekli izleyin ve uyarılara hızlı bir şekilde yanıt verin
- Cihaza erişimi olan yönetici sayısını sınırlayın
