---
title: Amazon Web Services ile Azure Otomasyonu runbook kimlik doğrulaması
description: Bu makalede, Amazon Web Services ile runbook 'ların nasıl doğrulandığını açıklanmaktadır.
keywords: aws kimlik doğrulaması, aws yapılandırma
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: acb056fb959fda320a14059e2b36a0f5e3fb0b37
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837203"
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Amazon Web Services ile kimlik doğrulaması runbook’ları

Amazon Web Hizmetleri’ndeki (AWS) kaynaklarla ortak görevlerin otomatikleştirilmesi Azure’ün Automation runbook'ları ile sonuçlandırılabilir. Tıpkı Azure’deki kaynaklarla yapabildiğiniz gibi Automation runbook'ları kullanarak AWS’deki birçok görevi otomatik hale getirebilirsiniz. Kimlik doğrulaması için bir Azure aboneliğinizin olması gerekir.

## <a name="obtain-aws-subscription-and-credentials"></a>AWS aboneliğini ve kimlik bilgilerini alma

AWS ile kimlik doğrulamak için bir AWS aboneliği edinmeniz ve Azure Otomasyonu 'ndan çalışan runbook 'larınızın kimliğini doğrulamak için bir AWS kimlik bilgileri kümesi belirtmeniz gerekir. Gereken belirli kimlik bilgileri AWS erişim anahtarı ve gizli anahtar. Bkz. [AWS kimlik bilgilerini kullanma](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).

## <a name="configure-automation-account"></a>Automation hesabı yapılandırma

AWS ile kimlik doğrulamak için mevcut bir Otomasyon hesabını kullanabilirsiniz. Alternatif olarak, AWS kaynaklarını hedefleyen runbook 'lar için bir hesap ayırabilirsiniz. Bu durumda yeni bir [Otomasyon hesabı](automation-create-standalone-account.md)oluşturun.  

## <a name="store-aws-credentials"></a>AWS kimlik bilgilerini depola

AWS kimlik bilgilerini Azure Otomasyonu 'nda varlıklar olarak depolamanız gerekir. Erişim anahtarı ve gizli anahtar oluşturma yönergeleri için bkz. [AWS hesabınıza yönelik erişim anahtarlarını yönetme](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) . Anahtarlar kullanılabilir olduğunda, erişim anahtarı KIMLIĞINI ve gizli anahtar KIMLIĞINI güvenli bir yerde kopyalayın. Anahtar dosyanızı, güvenli bir yerde depolamak için indirebilirsiniz.

## <a name="create-credential-asset"></a>Kimlik bilgisi varlığı oluştur

AWS güvenlik anahtarlarınızı oluşturup kopyaladıktan sonra, Otomasyon hesabıyla bir kimlik bilgisi varlığı oluşturmanız gerekir. Varlık AWS anahtarlarını güvenli bir şekilde depolamanızı ve Runbook 'larınızda bunlara başvurmalarını sağlar. Bkz. [Azure Portal yeni bir kimlik bilgisi varlığı oluşturma](shared-resources/credentials.md#create-a-new-credential-asset-with-the-azure-portal). Belirtilen alanlara aşağıdaki AWS bilgilerini girin:
    
* **Ad**  -  **Awscred**veya adlandırma standartlarınızı takip eden uygun bir değer
* **Kullanıcı adı** -erişim kimliğiniz
* **Parola** -gizli anahtarınızın adı 

## <a name="next-steps"></a>Sonraki adımlar

* AWS 'de görevleri otomatikleştirmek için Runbook 'ların nasıl oluşturulacağını öğrenmek için bkz. [runbook ile Amazon Web SERVICES VM dağıtma](automation-scenario-aws-deployment.md).