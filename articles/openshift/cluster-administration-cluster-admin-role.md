---
title: Azure Red Hat OpenShift kümesi yönetici rolü | Microsoft Docs
description: Azure Red Hat OpenShift Küme Yöneticisi rol ataması ve kullanımı
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 6d7aacea5e34e21513452880448227a1ca5f9b67
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709947"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Azure Red Hat OpenShift müşteri yöneticisi rolü

Bir OpenShift kümesinin bir ARO (Azure Red Hat OpenShift) kümesi Yöneticisi olarak hesabınız, izinleri artmıştır ve Kullanıcı tarafından oluşturulan tüm projelere erişim sağlar.

Hesabınız OSA-Müşteri yöneticileri yetkilendirme rolüne bağlıysa, bir projeyi otomatik olarak yönetebilir.

> [!Note] 
> OSA-müşteri-yönetici clusterrole, küme yönetici kümerolü ile aynı değil


Örneğin, bir dizi fiil (`create`) ile ilişkili eylemleri bir kaynak adı kümesi üzerinde (`templates`) çalıştırabilirsiniz. Bu rollerin ayrıntılarını ve fiil ve kaynak kümelerini görüntülemek için aşağıdaki komutu çalıştırın:

`$ oc describe clusterrole/osa-customer-admin`

Fiil adları, tüm doğrudan OC komutlarına eşlenmemelidir, ancak gerçekleştirebileceğiniz CLı işlemleri türlerine daha fazla eş yol açabilir. Örneğin, `list` fiili olması, belirli bir kaynak adının (`oc get`) tüm nesnelerinin bir listesini görüntüleyebilen anlamına gelir, ancak @no__t 2 fiili, adını biliyorsanız belirli bir nesnenin ayrıntılarını gösterebilirsiniz (`oc describe`).

## <a name="how-to-configure-customer-administrator-role"></a>Müşteri Yöneticisi rolünü yapılandırma

Müşteri Yöneticisi rolü yalnızca küme oluşturma sırasında, `--customer-admin-group-id` bayrağı sağlanarak yapılandırılabilir. Azure Active Directory ve Yöneticiler grubunu yapılandırma nasıl yapılır Kılavuzu: [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md)

## <a name="next-steps"></a>Sonraki adımlar

OSA nasıl yapılandırılır-müşteri-yönetici rolü:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md)
