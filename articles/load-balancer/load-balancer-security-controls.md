---
title: Security controls for Azure Load Balancer
description: A checklist of security controls for evaluating Load Balancer
services: load-balancer
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: allensu
ms.openlocfilehash: 6043e574697489b6566641c352bc21a2b6d87f51
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74214894"
---
# <a name="security-controls-for-azure-load-balancer"></a>Security controls for Azure Load Balancer

This article documents the security controls built into Azure Load Balancer.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Security control | Evet/Hayır | Notlar |
|---|---|--|
| Service endpoint support| Yok | |
| VNet injection support| Yok | |
| Network Isolation and Firewalling support| Yok |  |
| Forced tunneling support| Yok | |

## <a name="monitoring--logging"></a>Monitoring & logging

| Security control | Evet/Hayır | Notlar|
|---|---|--|
| Azure monitoring support (Log analytics, App insights, etc.)| Yes | See [Azure Monitor logs for public Basic Load Balancer](load-balancer-monitor-log.md). |
| Control and management plane logging and audit| Yes | See [Azure Monitor logs for public Basic Load Balancer](load-balancer-monitor-log.md). |
| Data plane logging and audit | Yok |  |

## <a name="identity"></a>Kimlik

| Security control | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Yok |  |
| Yetkilendirme| Yok |  |

## <a name="data-protection"></a>Veri koruması

| Security control | Evet/Hayır | Notlar |
|---|---|--|
| Server-side encryption at rest: Microsoft-managed keys | Yok | |
| Encryption in transit (such as ExpressRoute encryption, in VNet encryption, and VNet-VNet encryption )| Yok | |
| Server-side encryption at rest: customer-managed keys (BYOK) | Yok | |
| Column level encryption (Azure Data Services)| Yok | |
| API calls encrypted| Yes | Via the [Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Security control | Evet/Hayır | Notlar|
|---|---|--|
| Configuration management support (versioning of configuration, etc.)| Yok |  | 

## <a name="next-steps"></a>Sonraki adımlar

- Learn more about the [built-in security controls across Azure services](../security/fundamentals/security-controls.md).