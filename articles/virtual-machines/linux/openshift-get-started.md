---
title: Azure 'da OpenShift 'e genel bakış | Microsoft Docs
description: Azure 'da OpenShift 'e genel bakış.
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
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: 2293eac45da6f53b0cd29fda68b0ddc76aea1a6c
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390629"
---
# <a name="openshift-in-azure"></a>Azure’da OpenShift

OpenShift, Docker ve Kubernetes 'i kuruluşa getiren açık ve genişletilebilir bir kapsayıcı uygulama platformudur.  

OpenShift, kapsayıcı düzenleme ve yönetimi için Kubernetes içerir. Şunları sağlayan geliştirici merkezli ve işlemler merkezli araçlar ekler:

- Hızlı uygulama geliştirme.
- Kolay dağıtım ve ölçeklendirme.
- Takımlar ve uygulamalar için uzun süreli yaşam döngüsü bakımı.

Birden çok OpenShift sürümü mevcuttur.  Bu sürümler, günümüzde müşterilerin Azure 'da dağıtılması için yalnızca iki adet bulunabilir: OpenShift kapsayıcı platformu ve OKD (eski adıyla OpenShift Origin).

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Red Hat OpenShift Microsoft Azure, Azure 'da çalışan bir OpenShift 'in tamamen yönetilen bir tekliftir. Bu hizmet, Microsoft ve Red Hat tarafından ortaklaşa yönetilir ve desteklenir. Daha fazla ayrıntı için bkz. [Azure Red Hat OpenShift hizmeti](https://docs.microsoft.com/azure/openshift/) belgeleri.

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Kapsayıcı platformu, Red Hat tarafından desteklenen ve desteklenen kurumsal özellikli bir [ticari sürümdür](https://www.openshift.com) . Bu sürümde, müşteriler OpenShift kapsayıcı platformu için gerekli yetkilendirmeleri satın alarak tüm altyapının yüklenmesi ve yönetiminden sorumludur.

Müşterilerin tüm platforma "sahip olduğu" için, şirket içi veri merkezinize veya genel bir buluta (Azure gibi) yükleyebilir.

## <a name="okd"></a>OKD

OKD, topluluk tarafından desteklenen bir OpenShift 'in [Açık kaynaklı](https://www.okd.io/) yukarı akış projem. OKD, CentOS veya Red Hat Enterprise Linux (RHEL) üzerine yüklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da OpenShift için ortak önkoşulları yapılandırma](./openshift-container-platform-3x-prerequisites.md)
- [Azure 'da OpenShift kapsayıcı platformunu dağıtma](./openshift-container-platform-3x.md)
- [OpenShift kapsayıcı platformu dağıtımını otomatik olarak yönetilen Market Teklifini dağıtma](./openshift-container-platform-3x-marketplace-self-managed.md)
- [Azure Stack 'de OpenShift dağıtma](./openshift-azure-stack.md)
- [Dağıtım sonrası görevler](./openshift-container-platform-3x-post-deployment.md)
- [OpenShift dağıtımında sorun giderme](./openshift-container-platform-3x-troubleshooting.md)
