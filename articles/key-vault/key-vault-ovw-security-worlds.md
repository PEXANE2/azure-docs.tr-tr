---
title: Azure Key Vault güvenlik dünyaları | Microsoft Dokümanlar
description: Azure Key Vault çok kiracılı bir hizmettir. Her Azure konumunda bir HSM havuzu kullanır. Coğrafi bölgedeki tüm konumlar şifreleme sınırını paylaşır.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 07/03/2017
ms.openlocfilehash: 3584f83c5e1a5e83d069373395227b70c084eae9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79457398"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault güvenlik dünyaları ve coğrafi sınırlar

Azure Key Vault çok kiracılı bir hizmettir ve her Azure konumunda donanım güvenlik modülleri (HSM) havuzu kullanır. 

Aynı coğrafi bölgedeki Azure konumlarındaki tüm HSM'ler aynı şifreleme sınırını (Thales Security World) paylaşır. Örneğin, Doğu ABD ve Batı ABD aynı güvenlik dünyasını paylaşırlar çünkü abd'nin coğrafi konumuna aittirler. Benzer şekilde, Japonya'daki tüm Azure konumları aynı güvenlik dünyasını ve Avustralya, Hindistan ve benzeri tüm Azure konumlarını paylaşır. 

## <a name="backup-and-restore-behavior"></a>Yedekleme ve geri yükleme davranışı

Bir Azure konumundaki bir anahtar kasasından alınan bir yedekleme, bu koşulların her ikisi de geçerli olduğu sürece, başka bir Azure konumundaki anahtar kasasına geri yüklenebilir:

- Azure konumlarının her ikisi de aynı coğrafi konuma aittir
- Anahtar kasaların her ikisi de aynı Azure aboneliğine aittir

Örneğin, Batı Hindistan'daki bir anahtar kasasında ki belirli bir anahtar aboneliği tarafından alınan bir yedekleme, yalnızca aynı abonelik ve coğrafi konumdaki başka bir anahtar kasasına geri yüklenebilir; Batı Hindistan, Orta Hindistan veya Güney Hindistan.

## <a name="regions-and-products"></a>Bölgeler ve ürünler

- [Azure bölgeleri](https://azure.microsoft.com/regions/)
- [Bölgeye göre Microsoft ürünleri](https://azure.microsoft.com/regions/services/)

Bölgeler, tablolarda ana başlıklar olarak gösterilen güvenlik dünyalarına eşlenir:

Bölge makalesine göre ürünlerde, örneğin, **Amerika** sekmesi DOĞU ABD, ORTA ABD, BATI ABD tüm Amerika bölgesine eşleme içerir. 

>[!NOTE]
>Bir istisna abd DOD DOĞU ve ABD DOD CENTRAL kendi güvenlik dünyaları olmasıdır. 

Benzer şekilde, **Avrupa** sekmesinde, KUZEY AVRUPA ve BATI AVRUPA'nın her ikisi de Avrupa bölgesine ait bir haritadır. Aynı şey **Asya Pasifik** sekmesinde de geçerlidir.



