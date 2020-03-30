---
title: Azure Red Hat OpenShift küme yöneticisi rolü | Microsoft Dokümanlar
description: Azure Red Hat OpenShift küme yöneticisi rolünün atanması ve kullanımı
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: ae9a421a165d6c8bda688819c5233ae5bb1a8562
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139105"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Azure Red Hat OpenShift müşteri yöneticisi rolü

Azure Red Hat OpenShift kümesinin küme yöneticisisiniz. Hesabınız izinleri ve kullanıcı tarafından oluşturulan tüm projelere erişimi artırdı.

Hesabınızda müşteri-yönetici kümesi yetkilendirme rolü varsa, projeyi otomatik olarak yönetebilir.

> [!Note] 
> Müşteri-yönetici küme küme rolü küme-yönetici küme rolüyle aynı değildir.

Örneğin, bir dizi fiille ilişkili eylemleri (`create`) kaynak adlarını (`templates`) çalıştırmak için gerçekleştirebilirsiniz. Bu rollerin ayrıntılarını ve bunların fiil ve kaynak kümelerini görüntülemek için aşağıdaki komutu çalıştırın:

`$ oc get clusterroles customer-admin-cluster -o yaml`

Fiil adlarının tümü doğrudan `oc` komutlarla eşleneme gerekmez. Bunlar daha genel olarak gerçekleştirebileceğiniz CLI işlem türlerine eşittir. 

Örneğin, fiilin `list` olması, bir kaynak adının tüm nesnelerinin listesini`oc get`gösterebileceğiniz anlamına gelir ( ). Fiil, `get` belirli bir nesnenin adını biliyorsanız ayrıntılarını gösterebileceğiniz`oc describe`anlamına gelir ( ).

## <a name="configure-the-customer-administrator-role"></a>Müşteri yöneticisi rolünü yapılandırma

Yalnızca küme oluşturma sırasında müşteri-yönetici-küme küme rolünü bayrağı `--customer-admin-group-id`sağlayarak yapılandırabilirsiniz. Bu alan şu anda Azure portalında yapılandırılamaz. Azure Active Directory ve Yöneticiler grubunu nasıl yapılandıracağız öğrenmek [için Azure Red Hat OpenShift için Azure Etkin Dizin tümleştirmesine](howto-aad-app-configuration.md)bakın.

## <a name="confirm-membership-in-the-customer-administrator-role"></a>Müşteri yöneticisi rolündeüyeliği onaylama

Müşteri yöneticisi grubundaki üyeliğinizi onaylamak için OpenShift CLI komutlarını `oc get nodes` veya `oc projects`. `oc get nodes`müşteri-yönetici kümesi rolünüz varsa düğümlerin bir listesini ve yalnızca müşteri-yönetici-proje rolüne sahipseniz bir izin hatası gösterir. `oc projects`sadece çalıştığınız projelerin aksine kümedeki tüm projeleri gösterir.

Kümenizdeki rolleri ve izinleri daha fazla keşfetmek [`oc policy who-can <verb> <resource>`](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_rbac.html#managing-role-bindings) için komutu kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Müşteri-yönetici kümesi küme rolünü yapılandırın:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift için Azure Active Directory tümleştirmesi](howto-aad-app-configuration.md)
