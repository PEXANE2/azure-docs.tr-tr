---
title: Key Vault kullanmak için en iyi uygulamalar | Azure Key Vault | Microsoft Docs
description: Bu belgede, kullanmak için en iyi uygulamalardan bazıları açıklanmaktadır Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-key-vault
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 16828435dc8913fff8da4717ee7f77cc701504ed
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213083"
---
# <a name="best-practices-to-use-key-vault"></a>Key Vault kullanmak için en iyi uygulamalar

## <a name="control-access-to-your-vault"></a>Kasanıza erişimi denetleme

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamalara ve kullanıcılara izin vererek anahtar kasalarınıza güvenli bir şekilde erişmeniz gerekir. Bu [makalede](secure-your-key-vault.md) Key Vault erişim modeline genel bir bakış sunulmaktadır. Kimlik doğrulama ve yetkilendirmeyi açıklar ve anahtar kasalarınıza erişimin güvenliğini nasıl sağlayabileceğinizi açıklar.

Kasanıza erişimi denetlerken öneriler aşağıdaki gibidir:
1. Aboneliğiniz, kaynak grubunuz ve Anahtar kasaları (RBAC) için erişimi kilitleme
2. Her kasa için erişim ilkeleri oluşturma
3. Erişim vermek için en az ayrıcalık erişim sorumlusunu kullanın
4. Güvenlik Duvarı ve [sanal ağ hizmet uç noktalarını](overview-vnet-service-endpoints.md) aç

## <a name="use-separate-key-vault"></a>Ayrı Key Vault kullan

Önerimiz, ortam başına uygulama başına (geliştirme, ön üretim ve üretim) bir kasa kullanmaktır. Bu, ortamlar genelinde gizli dizileri paylaşmanıza ve ayrıca bir ihlal durumunda tehdidi azaltmanıza yardımcı olur.

## <a name="backup"></a>Backup

Bir kasadaki nesnelerin güncelleştirme/silme/silme işlemleri sırasında kasanızın düzenli olarak arka pencerelerini aldığınızdan emin olun.

### <a name="azure-powershell-backup-commands"></a>Azure PowerShell yedekleme komutları

* [Yedekleme sertifikası](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultCertificate?view=azurermps-6.13.0)
* [Yedekleme anahtarı](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultKey?view=azurermps-6.13.0)
* [Yedekleme gizli dizisi](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultSecret?view=azurermps-6.13.0)

### <a name="azure-cli-backup-commands"></a>Azure CLı yedekleme komutları

* [Yedekleme sertifikası](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-backup)
* [Yedekleme anahtarı](https://docs.microsoft.com/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-backup)
* [Yedekleme gizli dizisi](https://docs.microsoft.com/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-backup)


## <a name="turn-on-logging"></a>Günlüğü aç

Kasanızın [günlüğünü açın](logging.md) . Ayrıca uyarıları ayarlayın.

## <a name="turn-on-recovery-options"></a>Kurtarma seçeneklerini aç

1. [Geçici silme](overview-soft-delete.md)özelliğini açın.
2. Geçici silme etkin olduktan sonra bile gizli dizi/kasaların silinmesini zorlamak istiyorsanız Temizleme korumasını açın.
