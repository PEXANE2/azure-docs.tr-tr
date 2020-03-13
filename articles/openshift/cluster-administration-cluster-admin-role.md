---
title: Azure Red Hat OpenShift kümesi yönetici rolü | Microsoft Docs
description: Azure Red Hat OpenShift Küme Yöneticisi rolünün atanması ve kullanımı
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: ae9a421a165d6c8bda688819c5233ae5bb1a8562
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139105"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Azure Red Hat OpenShift müşteri yöneticisi rolü

Azure Red Hat OpenShift kümesinin küme yöneticisidir. Hesabınızın izinleri arttı ve Kullanıcı tarafından oluşturulan tüm projelere erişimi vardır.

Hesabınıza bir müşteri-yönetici-küme yetkilendirme rolü bağlandığında, bir projeyi otomatik olarak yönetebilir.

> [!Note] 
> Müşteri-yönetici-küme kümesi rolü, küme yönetici kümesi rolüyle aynı değildir.

Örneğin, bir dizi fiil (`create`) ile ilişkili eylemleri bir kaynak adı kümesi üzerinde (`templates`) çalıştırabilirsiniz. Bu rollerin ayrıntılarını ve fiil ve kaynak kümelerini görüntülemek için aşağıdaki komutu çalıştırın:

`$ oc get clusterroles customer-admin-cluster -o yaml`

Fiil adları tüm `oc` komutlara doğrudan eşlenmeyebilir. Bunlar, gerçekleştirebileceğiniz CLı işlemleri türlerine daha fazla eş alabilir. 

Örneğin, `list` fiilinin olması, kaynak adının (`oc get`) tüm nesnelerinin bir listesini görüntüleyebilen anlamına gelir. `get` fiil, adını biliyorsanız belirli bir nesnenin ayrıntılarını görüntüleyebilen anlamına gelir (`oc describe`).

## <a name="configure-the-customer-administrator-role"></a>Müşteri Yöneticisi rolünü yapılandırma

Müşteri-yönetici-kümesi kümesi rolünü yalnızca, `--customer-admin-group-id`bayrağını sağlayarak küme oluşturma sırasında yapılandırabilirsiniz. Bu alan Azure portal Şu anda yapılandırılamaz. Azure Active Directory ve Yöneticiler grubunu yapılandırma hakkında bilgi edinmek için bkz. [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md).

## <a name="confirm-membership-in-the-customer-administrator-role"></a>Müşteri Yöneticisi rolünde üyeliği onaylama

Müşteri yöneticisi grubundaki üyeliğinizi onaylamak için, OpenShift CLı komutlarını `oc get nodes` veya `oc projects`deneyin. `oc get nodes`, müşterinin yönetici kümesi rolüne sahipseniz bir düğüm listesi ve yalnızca müşteri-yönetici-proje rolüne sahipseniz bir izin hatası gösterir. `oc projects`, yalnızca çalıştığınız projelerin aksine kümedeki tüm projeleri gösterecektir.

Kümenizdeki rol ve izinleri daha fazla incelemek için [`oc policy who-can <verb> <resource>`](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_rbac.html#managing-role-bindings) komutunu kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Müşteri-yönetici-küme kümesi rolünü yapılandırın:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md)
