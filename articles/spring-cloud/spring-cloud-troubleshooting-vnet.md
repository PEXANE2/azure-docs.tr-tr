---
title: Sanal ağda Azure Spring Cloud sorunlarını giderme
description: Azure yay bulutu sanal ağı için sorun giderme kılavuzu.
author: mikedodaro
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/19/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: c2b0488663233546411b3a1b5ac1b1eb6c0b899c
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98887264"
---
# <a name="troubleshooting-azure-spring-cloud-in-virtual-networks"></a>Sanal ağlarda Azure Spring Cloud sorunlarını giderme

Bu belge, sanal ağlarda Azure yay bulutu kullanılırken oluşabilecek çeşitli sorunları çözmenize yardımcı olur.

## <a name="i-encountered-a-problem-with-creating-an-azure-spring-cloud-service-instance"></a>Azure yay bulut hizmeti örneği oluştururken bir sorunla karşılaştım

Azure Spring Cloud 'ın bir örneğini oluşturmak için, örneği sanal ağa dağıtmak için yeterli izne sahip olmanız gerekir.  Spring Cloud Service örneğinin kendisi [, sanal ağa Azure Spring Cloud Service Izni vermelidir](spring-cloud-tutorial-deploy-in-azure-virtual-network.md#grant-service-permission-to-the-virtual-network).

Azure yay bulut hizmeti örneğini kurmak için Azure portal kullanıyorsanız, Azure portal izinleri doğrular.

Azure [CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)'Yı kullanarak Azure yay bulut hizmeti örneğini ayarlamak için şunları doğrulayın:

- Abonelik etkin.
- Konum, Azure Spring Cloud tarafından desteklenir.
- Örnek için kaynak grubu zaten oluşturulmuş.
- Kaynak adı adlandırma kuralına uyar. Yalnızca küçük harf, sayı ve kısa çizgi içermelidir. İlk karakter harf olmalıdır. Son karakter harf veya sayı olmalıdır. Değer, 2 ile 32 karakter arasında olmalıdır.

Azure Spring Cloud Service örneğini Kaynak Yöneticisi şablonunu kullanarak ayarlamak için, [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)bölümüne bakın.

### <a name="common-creation-issues"></a>Yaygın oluşturma sorunları

| Hata İletisi | Nasıl düzeltilir |
|------|------|
| Azure Spring Cloud tarafından oluşturulan kaynaklara ilke tarafından izin verilmedi. | Ağ kaynakları, Azure Spring Cloud 'ı kendi sanal ağınıza dağıtırken oluşturulacaktır. Lütfen bu oluşturmayı engellemek için [Azure ilkesinin](https://docs.microsoft.com/azure/governance/policy/overview) tanımlanıp tanımlanmadığını denetleyin. Oluşturulacak kaynaklar hata iletisinde bulunabilir. |
| Belirtilen alt ağlar rota tablolarıyla ilişkili, lütfen bunların ilişkisini kaldırın. | Şu anda, mevcut yol tablolarıyla ilişkili olan alt ağda Azure Spring Cloud dağıtmak için desteklenmiyor, lütfen bunların ilişkilendirmesini kaldırın ve yeniden deneyin. |
| Gerekli trafik beyaz listeye alınmamış. | Gerekli trafiğin beyaz listeye eklendiğinden emin olmak için lütfen [VNET 'Te Azure yay bulutu 'Nı çalıştırmaya yönelik müşteri sorumluluklarına](spring-cloud-vnet-customer-responsibilities.md) bakın. |

## <a name="my-application-cant-be-registered"></a>Uygulamam kaydedilemiyor

Bu sorun, sanal ağınız özel DNS ayarlarıyla yapılandırılmışsa oluşur. Bu durumda, Azure Spring Cloud tarafından kullanılan özel DNS bölgesi verimsiz değildir. IP 168.63.129.16 Azure DNS özel DNS sunucusuna yukarı akış DNS sunucusu olarak ekleyin.

## <a name="other-issues"></a>Diğer sorunlar

[Yaygın Azure yay bulutu sorunlarını giderin](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-troubleshoot).