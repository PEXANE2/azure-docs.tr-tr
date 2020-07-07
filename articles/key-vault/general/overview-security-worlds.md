---
title: Azure Key Vault Security dünyaları | Microsoft Docs
description: Azure Key Vault, çok kiracılı bir hizmettir. Her Azure konumunda bir HSM havuzu kullanır. Coğrafi bölgedeki tüm konumlar şifreleme sınırını paylaşır.
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 07/03/2017
ms.openlocfilehash: 3584f83c5e1a5e83d069373395227b70c084eae9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81428959"
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault güvenlik ve coğrafi sınırlar

Azure Key Vault, çok kiracılı bir hizmettir ve her bir Azure konumunda donanım güvenlik modülleri (HSM 'ler) havuzu kullanır. 

Aynı coğrafi bölgedeki Azure konumlarında bulunan tüm HSM 'ler aynı şifreleme sınırını (Thales güvenlik dünyası) paylaşır. Örneğin, Doğu ABD ve Batı ABD ABD coğrafi konumuna ait olduklarından aynı güvenlik dünyasını paylaşır. Benzer şekilde, Japonya 'daki tüm Azure konumları aynı güvenlik dünyasını ve Avustralya, Hindistan ve benzeri tüm Azure konumlarını paylaşır. 

## <a name="backup-and-restore-behavior"></a>Yedekleme ve geri yükleme davranışı

Bu koşulların her ikisi de doğru olduğu sürece, bir Azure konumundaki anahtar kasasından bir anahtarın bir yedeğini almak başka bir Azure konumundaki anahtar kasasına geri yüklenebilir:

- Azure konumlarından her ikisi de aynı coğrafi konuma aittir
- Her iki anahtar kasası da aynı Azure aboneliğine ait

Örneğin, Batı Hindistan bir anahtar kasasındaki bir anahtarın belirli bir aboneliği tarafından alınan bir yedekleme, yalnızca aynı abonelikte ve coğrafi konumda bulunan başka bir anahtar kasasının içine geri yüklenebilir; Batı Hindistan, Orta Hindistan veya Güney Hindistan.

## <a name="regions-and-products"></a>Bölgeler ve ürünler

- [Azure bölgeleri](https://azure.microsoft.com/regions/)
- [Bölgeye göre Microsoft ürünleri](https://azure.microsoft.com/regions/services/)

Bölgeler, tablolardaki ana başlıklar olarak gösterilen güvenlik dünyalarını eşleştirmekte:

Ürünlere göre ürünler makalesinde, örneğin, **Kuzey** SEKMESI Doğu ABD, Orta ABD, Batı ABD Kuzey bölgesine eşlenir. 

>[!NOTE]
>Bir özel durum, US DOD Doğu ve ABD DOD CENTRAL 'ın kendi güvenlik dünyalarına sahip olduğu durumdur. 

Benzer şekilde, **Avrupa** SEKMESINDE Kuzey Avrupa ve Batı Avrupa, her Ikisi de Avrupa bölgesi ile eşlenir. Aynı zamanda **Asya Pasifik** sekmesinde de geçerlidir.



