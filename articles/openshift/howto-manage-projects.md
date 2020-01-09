---
title: Azure Red Hat OpenShift 'te kaynakları yönetme | Microsoft Docs
description: Azure Red Hat OpenShift kümesinde projeleri, şablonları, görüntü akışlarını yönetme
services: openshift
keywords: Red Hat OpenShift projeleri kendi kendine hazırlayıcı ister
author: mjudeikis
ms.author: b-majude
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: d88be50468f55a848b43613e1f7851621202052d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75378237"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde projeleri, şablonları, görüntü akışlarını yönetme 

Bir OpenShift kapsayıcı platformunda, projeler ilişkili nesneleri gruplandırmak ve yalıtmak için kullanılır. Yönetici olarak, geliştiricilere belirli projelere erişim verebilir, kendi projelerini oluşturmalarına izin verebilir ve bu kullanıcılara ayrı projeler için yönetici hakları verebilirsiniz.

## <a name="self-provisioning-projects"></a>Kendi kendine sağlama projeleri

Geliştiricilerin kendi projelerini oluşturmasını sağlayabilirsiniz. Bir API uç noktası proje-istek adlı bir şablona göre bir proje sağlamaktan sorumludur. Web Konsolu ve `oc new-project` komutu, geliştirici yeni bir proje oluşturduğunda bu uç noktayı kullanır.

Bir proje isteği gönderildiğinde, API, şablonda aşağıdaki parametreleri yerine koyar:

| Parametre               | Açıklama                                    |
| ----------------------- | ---------------------------------------------- |
| PROJECT_NAME            | Projenin adı. Gereklidir.             |
| PROJECT_DISPLAYNAME     | Projenin görünen adı. Boş olabilir. |
| PROJECT_DESCRIPTION     | Projenin açıklaması. Boş olabilir.  |
| PROJECT_ADMIN_USER      | Yönetme kullanıcısının Kullanıcı adı.       |
| PROJECT_REQUESTING_USER | İstekte bulunan kullanıcının Kullanıcı adı.           |

API 'ye erişim, kendi kendini hazırlayıcılar kümesi rol bağlamasıyla geliştiricilere verilir. Bu özellik tüm kimliği doğrulanmış geliştiriciler için varsayılan olarak kullanılabilir.

## <a name="modify-the-template-for-a-new-project"></a>Yeni bir proje için şablonu değiştirme 

1. `customer-admin` ayrıcalıkları olan bir kullanıcı olarak oturum açın.

2. Varsayılan proje-istek şablonunu düzenleyin.

   ```
   oc edit template project-request -n openshift
   ```

3. Aşağıdaki ek açıklamayı ekleyerek varsayılan proje şablonunu Azure Red Hat OpenShift (ARO) güncelleştirme işleminden kaldırın: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Proje-istek şablonu, ARO güncelleştirme işlemi tarafından güncellenmeyecektir. Bu, müşterilerin şablonu özelleştirmesini ve küme güncelleştirilirken bu özelleştirmeleri korumalarını sağlar.

## <a name="disable-the-self-provisioning-role"></a>Kendi kendine sağlama rolünü devre dışı bırakma

Kimliği doğrulanmış bir Kullanıcı grubunun kendi kendine yeni projeler sağlamasını engelleyebilirsiniz.

1. `customer-admin` ayrıcalıkları olan bir kullanıcı olarak oturum açın.

2. Kendi kendine hazırlayıcılar kümesi rol bağlamasını düzenleyin.

   ```
   oc edit clusterrolebinding.rbac.authorization.k8s.io self-provisioners
   ```

3. Aşağıdaki ek açıklamayı ekleyerek, rolü ARO güncelleştirme işleminden kaldırın: `openshift.io/reconcile-protect: "true"`.

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. `system:authenticated:oauth` proje oluşturmasını engellemek için küme rolü bağlamasını değiştirin:

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

## <a name="manage-default-templates-and-imagestreams"></a>Varsayılan şablonları ve ımagestreams 'yi yönetme

Azure Red Hat OpenShift 'te, `openshift` ad alanı içindeki tüm varsayılan şablonlar ve görüntü akışları için güncelleştirmeleri devre dışı bırakabilirsiniz.
Tüm `Templates` ve `openshift` ad alanındaki `ImageStreams` güncelleştirmelerini devre dışı bırakmak için:

1. `customer-admin` ayrıcalıkları olan bir kullanıcı olarak oturum açın.

2. `openshift` ad alanını Düzenle:

   ```
   oc edit namespace openshift
   ```

3. Aşağıdaki ek açıklamayı ekleyerek `openshift` ad alanını ARO güncelleştirme işleminden kaldırın: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   `openshift` ad alanındaki herhangi bir nesne, `openshift.io/reconcile-protect: "true"` ek açıklama ekleyerek güncelleştirme işleminden kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
