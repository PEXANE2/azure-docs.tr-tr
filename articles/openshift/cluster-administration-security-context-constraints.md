---
title: Azure Red Hat OpenShift 'te güvenlik bağlamı kısıtlamalarını yönetme | Microsoft Docs
description: Azure Red Hat OpenShift Küme Yöneticisi güvenlik bağlamı kısıtlamalarını yönetme
services: container-service
author: troy0820
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 8e85ac98683487c6b18be7f502f28cad9a0c2251
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709943"
---
# <a name="overview"></a>Genel bakış 

Güvenlik bağlamı kısıtlamaları, yöneticilerin pods izinlerini denetlemesine olanak tanır. Bu API türü hakkında daha fazla bilgi edinmek için bkz. [güvenlik bağlamı kısıtlamaları](https://https://docs.openshift.com/container-platform/3.11/architecture/additional_concepts/authorization.html) (SCCS) mimari belgeleri. CLı kullanarak, örneğiniz içindeki SCCs 'YI normal API nesneleri olarak yönetebilirsiniz.

## <a name="listing-security-context-constraints"></a>Güvenlik bağlamı kısıtlamalarını listeleme

Geçerli SCCs listesini almak için 

```bash
$ oc get scc

NAME               PRIV      CAPS      SELINUX     RUNASUSER          FSGROUP     SUPGROUP    PRIORITY   READONLYROOTFS   VOLUMES
anyuid             false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    10         false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
hostaccess         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath persistentVolumeClaim secret]
hostmount-anyuid   false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath nfs persistentVolumeClaim secret]
hostnetwork        false     []        MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
nonroot            false     []        MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
privileged         true      [*]       RunAsAny    RunAsAny           RunAsAny    RunAsAny    <none>     false            [*]
restricted         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
```

## <a name="examining-a-security-context-constraints-object"></a>Bir güvenlik bağlamı kısıtlama nesnesini İnceleme

Belirli bir SCC 'i incelemek için `oc get`, `oc describe` veya `oc edit` kullanın.  Örneğin, **Kısıtlanmış** SCC 'yi incelemek için:
```bash
$ oc describe scc restricted
Name:                   restricted
Priority:               <none>
Access:
  Users:                <none>
  Groups:               system:authenticated
Settings:
  Allow Privileged:         false
  Default Add Capabilities:     <none>
  Required Drop Capabilities:       KILL,MKNOD,SYS_CHROOT,SETUID,SETGID
  Allowed Capabilities:         <none>
  Allowed Seccomp Profiles:     <none>
  Allowed Volume Types:         configMap,downwardAPI,emptyDir,persistentVolumeClaim,projected,secret
  Allow Host Network:           false
  Allow Host Ports:         false
  Allow Host PID:           false
  Allow Host IPC:           false
  Read Only Root Filesystem:        false
  Run As User Strategy: MustRunAsRange
    UID:                <none>
    UID Range Min:          <none>
    UID Range Max:          <none>
  SELinux Context Strategy: MustRunAs
    User:               <none>
    Role:               <none>
    Type:               <none>
    Level:              <none>
  FSGroup Strategy: MustRunAs
    Ranges:             <none>
  Supplemental Groups Strategy: RunAsAny
    Ranges:             <none>
```
## <a name="next-steps"></a>Sonraki adımlar
OSA nasıl yapılandırılır-müşteri-yönetici rolü:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift için Azure Active Directory Tümleştirmesi](howto-aad-app-configuration.md) 