---
title: Azure Red Hat OpenShift kümesi yönetici rolü | Microsoft Docs
description: Azure Red Hat OpenShift Küme Yöneticisi rolünün atanması ve kullanımı
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 0cb875122c63be18f7c39cdfea7986d705ed434e
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74539266"
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

Müşteri-yönetici-kümesi kümesi rolünü yalnızca, `--customer-admin-group-id`bayrağını sağlayarak küme oluşturma sırasında yapılandırabilirsiniz. Azure Active Directory ve Yöneticiler grubunu yapılandırma hakkında bilgi edinmek için bkz. [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

Müşteri-yönetici-küme kümesi rolünü yapılandırın:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md)
