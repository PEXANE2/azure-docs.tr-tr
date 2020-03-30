---
title: Azure VM'lerde SAP HANA kullanılabilirliği - Genel Bakış | Microsoft Dokümanlar
description: Azure yerel VM'lerinde SAP HANA işlemlerini açıklar.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bccaf45cf617bd31a584b6c73f3dd08877bc8587
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71266067"
---
# <a name="sap-hana-high-availability-for-azure-virtual-machines"></a>Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik

Azure VM'lerde SAP HANA gibi görev açısından kritik veritabanlarını dağıtmak için çok sayıda Azure özelliğik kullanabilirsiniz. Bu makalede, Azure VM'lerde barındırılan SAP HANA örnekleri için kullanılabilirlik nasıl sağlanabilirlik sağlanabilir. Makalede, Azure'da SAP HANA kullanılabilirliğini artırmak için Azure altyapısını kullanarak uygulayabileceğiniz birkaç senaryo açıklanmaktadır. 

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Azure'da hizmet (IaaS) temelleri olarak altyapıyı bildiğiniz varsayar: 

- Azure portalı veya PowerShell üzerinden sanal makineler veya sanal ağlar nasıl dağıtılır?
- JavaScript Nesne Gösterimi (JSON) şablonlarını kullanma seçeneği de dahil olmak üzere Azure platform ötesi komut satırı arabirimini (Azure CLI) kullanma.

Bu makalede, SAP HANA örneklerini yükleme ve SAP HANA örneklerini yönetme ve çalıştırma konusunda da bildiğiniz varsayar. HANA sistem çoğaltma kurulumu ve işlemleri hakkında bilgi almak özellikle önemlidir. Buna SAP HANA veritabanları için yedekleme ve geri yükleme gibi görevler de dahildir.

Bu makaleler, Azure'da SAP HANA'yı kullanmaya iyi bir genel bakış sağlar:

- [Azure VM'lerde tek örnekLI SAP HANA'nın manuel olarak yüklenmesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Azure VM'lerde SAP HANA sistem çoğaltmasını ayarlama](sap-hana-high-availability.md)
- [Azure VM’leri üzerinde SAP HANA yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Sap HANA ile ilgili bu makalelere aşina olmak da iyi bir fikirdir:

- [SAP HANA için yüksek kullanılabilirlik](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [SSS: SAP HANA için yüksek kullanılabilirlik](https://archive.sap.com/documents/docs/DOC-66702)
- [SAP HANA için sistem çoğaltma gerçekleştirin](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SPS 01 Yenilikler: Yüksek kullanılabilirlik](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [SAP HANA sistem çoğaltma için ağ önerileri](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [SAP HANA sistem çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA hizmeti otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA sistem çoğaltma yapılandırma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)

Azure'da Sanal M'leri dağıtmayı tanımanın yanı, Azure'da kullanılabilirlik mimarinizi tanımlamadan önce, [Azure'daki Windows sanal makinelerinin kullanılabilirliğini yönet'i](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)okumanızı öneririz.

## <a name="service-level-agreements-for-azure-components"></a>Azure bileşenleri için hizmet düzeyi anlaşmaları

Azure ağ, depolama ve VM'ler gibi farklı bileşenler için farklı kullanılabilirlik SLA'larına sahiptir. Tüm SLA'lar belgelenmiştir. Daha fazla bilgi için [Bkz. Microsoft Azure Hizmet Düzeyi Sözleşmeleri.](https://azure.microsoft.com/support/legal/sla/) 

[Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) üç farklı SLA açıklar, üç farklı yapılandırmalar için:

- İşletim sistemi diski ve tüm veri diskleri için [Azure premium SSD'leri](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) kullanan tek bir VM. Bu seçenek, aylık yüzde 99,9'luk bir çalışma süresi sağlar.
- [Azure kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)düzenlenen birden çok (en az iki) VM. Bu seçenek, aylık yüzde 99,95'lik bir çalışma süresi sağlar.
- [Bir Availablity Zone'da](https://docs.microsoft.com/azure/availability-zones/az-overview)düzenlenen birden çok (en az iki) VM. Bu seçenek, yüzde 99,99 aylık çalışma süresi sağladı.

Kullanılabilirlik gereksiniminizi Azure bileşenlerinin sağlayabileceği SLA'lara göre ölçün. Ardından, gerekli kullanılabilirlik düzeyine ulaşmak için SAP HANA için senaryolarınızı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Tek [bir Azure bölgesinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)hakkında bilgi edinin.
- Azure [bölgelerinde SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)hakkında bilgi edinin. 















  


