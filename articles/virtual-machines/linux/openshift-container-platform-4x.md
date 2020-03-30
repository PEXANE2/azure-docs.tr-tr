---
title: Azure'da OpenShift Kapsayıcı Platformu 4.x'i dağıtma
description: Azure'da OpenShift Kapsayıcı Platformu 4.x'i dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 213c02b76f822d134729ebc4c0e6bff40f62089f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74035429"
---
# <a name="deploy-openshift-container-platform-4x-in-azure"></a>Azure'da OpenShift Kapsayıcı Platformu 4.x'i dağıtma

OpenShift Kapsayıcı Platformu (OCP) 4.2 dağıtımı artık Yükleyici Destekli Altyapı (IPI) modeli yle Azure'da destekleniyor.  OpenShift 4 denemek için açılış sayfası [try.openshift.com.](https://try.openshift.com/) Azure'da OCP 4.2'yi yüklemek için [Red Hat OpenShift Cluster Manager](https://cloud.redhat.com/openshift/install/azure/installer-provisioned) sayfasını ziyaret edin.  Red Hat kimlik bilgileri bu siteye erişmek için gereklidir.


## <a name="notes"></a>Notlar 

 - Azure'da OCP 4.x'i yüklemek ve çalıştırmak için bir Azure Etkin Dizin (AAD) Hizmet Yöneticisi (SP) gereklidir
     - SP'ye **Application.ReadWrite.OwnedBy** for Azure Active Directory Graph'ın API izni verilmelidir
     - Bir AAD Kiracı Yöneticisi, bu API'nin yürürlüğe girmesi için Yönetici Onayı vermelidir
     - SP'ye aboneye **Katkıda Bulunan** ve Kullanıcı Erişimi **Yöneticisi** rolleri verilmelidir
 - OCP 4.x için yükleme modeli 3.x'ten farklıdır ve Azure'da OCP 4.x dağıtmak için kullanılabilir Azure Kaynak Yöneticisi şablonu yoktur
 - Yükleme işlemi sırasında sorunlarla karşılaşılırsa, ilgili şirkete (Microsoft veya Red Hat) başvurun

| Sorun Açıklaması | İletişim Noktası |
|-------------------|---------------|
| Azure'a özgü sorunlar (AAD, SP, Azure Aboneliği, vb.)                              | Microsoft |
| OpenShift'e özel sorunlar (Kurulum hataları / hataları, Red Hat aboneliği, vb.) |  Red Hat  |




## <a name="next-steps"></a>Sonraki adımlar

- [OpenShift Konteyner Platformu ile başlarken](https://docs.openshift.com)
