---
title: Azure Red Hat OpenShift 'te kaynakları yönetme | Microsoft Docs
description: Azure Red Hat OpenShift kümesinde projeleri, şablonları, görüntü akışlarını yönetme
services: openshift
keywords: Red Hat OpenShift projeleri kendi kendine hazırlayıcı ister
author: mjudeikis
ms.author: gwallace
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: d4f53238951784a74e6e3fc8a73d1f112ce75608
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79139122"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde projeleri, şablonları, görüntü akışlarını yönetme 

Bir OpenShift kapsayıcı platformunda, projeler ilişkili nesneleri gruplandırmak ve yalıtmak için kullanılır. Yönetici olarak, geliştiricilere belirli projelere erişim verebilir, kendi projelerini oluşturmalarına izin verebilir ve bu kullanıcılara ayrı projeler için yönetici hakları verebilirsiniz.

## <a name="self-provisioning-projects"></a>Kendi kendine sağlama projeleri

Geliştiricilerin kendi projelerini oluşturmasını sağlayabilirsiniz. Bir API uç noktası proje-istek adlı bir şablona göre bir proje sağlamaktan sorumludur. Web Konsolu ve komutu, `oc new-project` Geliştirici yeni bir proje oluşturduğunda bu uç noktayı kullanır.

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

1. Ayrıcalıkları olan bir kullanıcı olarak oturum açın `customer-admin` .

2. Varsayılan proje-istek şablonunu düzenleyin.

   ```
   oc edit template project-request -n openshift
   ```

3. Aşağıdaki ek açıklamayı ekleyerek varsayılan proje şablonunu Azure Red Hat OpenShift (ARO) güncelleştirme işleminden kaldırın:`openshift.io/reconcile-protect: "true"`

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

1. Ayrıcalıkları olan bir kullanıcı olarak oturum açın `customer-admin` .

2. Kendi kendine hazırlayıcılar kümesi rol bağlamasını düzenleyin.

   ```
   oc edit clusterrolebinding.rbac.authorization.k8s.io self-provisioners
   ```

3. Aşağıdaki ek açıklamayı ekleyerek, rolü ARO güncelleştirme işleminden kaldırın: `openshift.io/reconcile-protect: "true"` .

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. Proje oluşturmaktan kaçınmak için küme rolü bağlamasını değiştirin `system:authenticated:oauth` :

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

Azure Red Hat OpenShift 'te, ad alanı içindeki tüm varsayılan şablonlar ve görüntü akışları için güncelleştirmeleri devre dışı bırakabilirsiniz `openshift` .
Tüm `Templates` ve `ImageStreams` ad alanı için güncelleştirmeleri devre dışı bırakmak için `openshift` :

1. Ayrıcalıkları olan bir kullanıcı olarak oturum açın `customer-admin` .

2. `openshift`Ad alanını Düzenle:

   ```
   oc edit namespace openshift
   ```

3. `openshift`Aşağıdaki ek açıklamayı ekleyerek Aro güncelleştirme işlemindeki ad alanını kaldırın:`openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Ad alanındaki herhangi bir nesne, `openshift` buna ek açıklama eklenerek güncelleştirme işleminden kaldırılabilir `openshift.io/reconcile-protect: "true"` .

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
