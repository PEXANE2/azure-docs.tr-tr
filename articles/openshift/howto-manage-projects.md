---
title: Azure Red Hat OpenShift'te kaynakları yönetme | Microsoft Dokümanlar
description: Azure Red Hat OpenShift kümesinde projeleri, şablonları, görüntü akışlarını yönetme
services: openshift
keywords: kırmızı şapka openshift projeler istekleri self-provisioner
author: mjudeikis
ms.author: gwallace
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: d4f53238951784a74e6e3fc8a73d1f112ce75608
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139122"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde projeleri, şablonları, görüntü akışlarını yönetme 

OpenShift Kapsayıcı Platformu'nda, ilgili nesneleri gruplandırmak ve yalıtmak için projeler kullanılır. Yönetici olarak, geliştiricilere belirli projelere erişim sağlayabilir, kendi projelerini oluşturmalarına izin verebilir ve onlara tek tek projeleriçin yönetim hakları verebilirsiniz.

## <a name="self-provisioning-projects"></a>Kendi kendini sağlayan projeler

Geliştiricilerin kendi projelerini oluşturmalarını sağlayabilirsiniz. API bitiş noktası, proje isteği adlı şablona göre proje sağlamadan sorumludur. Geliştirici yeni bir `oc new-project` proje oluşturduğunda web konsolu ve komut bu bitiş noktasını kullanır.

Proje isteği gönderildiğinde, API şablonda aşağıdaki parametrelerin yerine geçer:

| Parametre               | Açıklama                                    |
| ----------------------- | ---------------------------------------------- |
| PROJECT_NAME            | Projenin adı. Gereklidir.             |
| PROJECT_DISPLAYNAME     | Projenin görüntü adı. Boş olabilir. |
| PROJECT_DESCRIPTION     | Projenin açıklaması. Boş olabilir.  |
| PROJECT_ADMIN_USER      | Yönetici kullanıcının kullanıcı adı.       |
| PROJECT_REQUESTING_USER | İstenen kullanıcının kullanıcı adı.           |

API'ye erişim, kendi kendini sağımlayıcıküme rol bağlama ile geliştiricilere verilir. Bu özellik varsayılan olarak tüm kimlik doğrulaması yapılan geliştiriciler tarafından kullanılabilir.

## <a name="modify-the-template-for-a-new-project"></a>Yeni bir proje için şablonu değiştirme 

1. Ayrıcalıkları olan `customer-admin` bir kullanıcı olarak oturum açın.

2. Varsayılan proje isteği şablonu'nu edin.

   ```
   oc edit template project-request -n openshift
   ```

3. Aşağıdaki ek açıklamayı ekleyerek Azure Red Hat OpenShift (ARO) güncelleştirme işleminden varsayılan proje şablonunu kaldırın:`openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Proje isteği şablonu ARO güncelleştirme işlemi tarafından güncelleştirilmeyecektir. Bu, müşterilerin şablonu özelleştirmesine ve küme güncelleştirildiğinde bu özelleştirmeleri korumasına olanak tanır.

## <a name="disable-the-self-provisioning-role"></a>Kendi kendini sağlama rolünü devre dışı

Kimlik doğrulaması yapılan bir kullanıcı grubunun kendi kendine yeni projeler sağlamasını engelleyebilirsiniz.

1. Ayrıcalıkları olan `customer-admin` bir kullanıcı olarak oturum açın.

2. Kendi kendini bulalı kümerolünü bağlamayı edin.

   ```
   oc edit clusterrolebinding.rbac.authorization.k8s.io self-provisioners
   ```

3. Aşağıdaki ek açıklamaekleyerek rolü ARO güncelleştirme işleminden `openshift.io/reconcile-protect: "true"`kaldırın: .

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. Proje oluşturmayı önlemek `system:authenticated:oauth` için küme rol bağlamayı değiştirin:

   ```
   apiVersion: rbac.authorization.k8s.io/v1
   groupNames:
   - osa-customer-admins
   kind: ClusterRoleBinding
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
     labels:
       azure.openshift.io/owned-by-sync-pod: "true"
     name: self-provisioners
   roleRef:
     name: self-provisioner
   subjects:
   - kind: SystemGroup
     name: osa-customer-admins
   ```

## <a name="manage-default-templates-and-imagestreams"></a>Varsayılan şablonları ve görüntüAkışlarını yönetme

Azure Red Hat OpenShift'te, ad alanı içindeki `openshift` varsayılan şablonlar ve görüntü akışları için güncelleştirmeleri devre dışı kullanabilirsiniz.
TÜM `Templates` ve `ImageStreams` `openshift` ad alanında güncelleştirmeleri devre dışı kalmak için:

1. Ayrıcalıkları olan `customer-admin` bir kullanıcı olarak oturum açın.

2. Ad `openshift` alanını edin:

   ```
   oc edit namespace openshift
   ```

3. Aşağıdaki `openshift` ek açıklamaekleyerek Ad alanını ARO güncelleştirme işleminden kaldırın:`openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   `openshift` Ad alanındaki herhangi bir tek tek nesne, ek açıklama `openshift.io/reconcile-protect: "true"` ekleyerek güncelleştirme işleminden kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
