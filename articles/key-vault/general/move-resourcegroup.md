---
title: Kasayı farklı bir kaynak grubuna taşımak Azure Key Vault | Microsoft Docs
description: Anahtar kasasını farklı bir kaynak grubuna taşıma Kılavuzu.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 04/29/2020
ms.author: sudbalas
ms.openlocfilehash: b26ebbbf256d7bde8f7f790cc0779c2eda729d1c
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106066641"
---
# <a name="moving-an-azure-key-vault-across-resource-groups"></a>Azure Key Vault'u kaynak grupları arasında taşıma

## <a name="overview"></a>Genel Bakış

Anahtar kasasının kaynak grupları arasında taşınması, desteklenen bir Anahtar Kasası özelliğidir. Anahtar kasasının kaynak grupları arasında taşınması, Anahtar Kasası güvenlik duvarını veya erişim ilkesi yapılandırmasını etkilemez. Bağlı uygulamalar ve hizmet sorumluları, istendiği gibi çalışmaya devam etmelidir.

> [!IMPORTANT]
> **Disk şifrelemesi için kullanılan anahtar kasaları taşınamaz.**
> Bir VM için disk şifrelemesi ile Anahtar Kasası kullanıyorsanız, disk şifrelemesi etkinken Anahtar Kasası farklı bir kaynak grubuna veya aboneliğe taşınamaz. Anahtar kasasını yeni bir kaynak grubuna veya aboneliğe taşımadan önce disk şifrelemesini devre dışı bırakmanız gerekir. 

## <a name="design-considerations"></a>Tasarımda Dikkat Edilmesi Gerekenler

Kuruluşunuz, kaynak grubu düzeyinde uygulama veya dışlamaları olan Azure Ilkesi uygulamış olabilir. Anahtar kasasının Şu anda bulunduğu kaynak grubunda ve anahtar kasanızı taşıdığınız kaynak grubunda farklı bir ilke atamaları kümesi olabilir. İlke gereksinimlerinde bir çakışma, uygulamalarınızı bozma potansiyelini içerir.

### <a name="example"></a>Örnek

İki yıl geçerli olan sertifikaları oluşturan anahtar kasasına bağlı bir uygulamanız var. Anahtar kasanızı taşımaya çalıştığınız kaynak grubu, bir yıldan daha uzun bir süre geçerli olan sertifikaların oluşturulmasını engelleyen bir ilke atamasına sahip. Anahtar kasanızı yeni kaynak grubuna taşıdıktan sonra, iki yıl için geçerli olan bir sertifika oluşturma işlemi bir Azure ilke ataması tarafından engellenir.

### <a name="solution"></a>Çözüm

Azure portal Azure Ilke sayfasına gitdiğinizden emin olun ve geçerli kaynak grubunuzun ilke atamalarını ve taşıdığınız kaynak grubunu ve hiçbir uyuşmazlıkların olmamasını sağlayın.

## <a name="procedure"></a>Yordam

1. Azure portalında oturum açma
2. Anahtar kasanıza gidin
3. "Genel bakış" sekmesine tıklayın
4. "Taşı" düğmesini seçin
5. Açılan menüden "başka bir kaynak grubuna geçme" seçeneğini belirleyin
6. Anahtar kasanızı taşımak istediğiniz kaynak grubunu seçin
7. Kaynakları taşıma hakkında uyarı bildirimi
8. "Tamam"ı seçin

Key Vault, şimdi kaynak taşımanın geçerliliğini değerlendirir ve herhangi bir hatayla ilgili olarak sizi uyarır. Herhangi bir hata bulunmazsa, kaynak taşıma işlemi tamamlanır. 
