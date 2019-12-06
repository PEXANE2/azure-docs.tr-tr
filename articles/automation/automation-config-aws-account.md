---
title: Amazon Web Services ile kimlik doğrulamasını yapılandırma
description: Bu makale, AWS kaynaklarını yöneten Azure Automation'daki runbook'lar için bir AWS kimlik bilgisinin nasıl oluşturulup doğrulandığı açıklanmaktadır.
keywords: aws kimlik doğrulaması, aws yapılandırma
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9bca59efc374fedf04600ae165f04215016dd4af
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850950"
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Amazon Web Hizmetleri ile Kimlik Doğrulaması Runbook'ları

Amazon Web Hizmetleri’ndeki (AWS) kaynaklarla ortak görevlerin otomatikleştirilmesi Azure’ün Automation runbook'ları ile sonuçlandırılabilir. Tıpkı Azure’deki kaynaklarla yapabildiğiniz gibi Automation runbook'ları kullanarak AWS’deki birçok görevi otomatik hale getirebilirsiniz. Gerekenlerin tümü şu iki şeydir:

* Bir AWS aboneliği ve bir dizi kimlik bilgisi. Özellikle AWS Erişim Anahtarınız ve Gizli Anahtarınız. Daha fazla bilgi için [AWS Kimlik Bilgilerini Kullanma](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) makalesini gözden geçirin.
* Bir Azure aboneliği ve Automation hesabı.

AWS kullanarak kimlik doğrulamak için, Azure Automation’dan çalışan runbook’larınızın kimliklerini doğrulamak amacıyla bir dizi AWS kimlik bilgisi belirtmeniz gerekir. Zaten oluşturulmuş bir Otomasyon hesabınız varsa ve AWS ile kimlik doğrulamak için bunu kullanmak istiyorsanız, aşağıdaki bölümdeki adımları izleyebilirsiniz: AWS kaynaklarını hedefleyen runbook 'lar için bir hesap atamak istiyorsanız, önce yeni bir [Otomasyon hesabı](automation-offering-get-started.md) oluşturmanız (hizmet sorumlusu oluşturmak için bu seçeneği atlayın) ve aşağıdaki adımları kullanmanız gerekir:

## <a name="configure-automation-account"></a>Automation hesabı yapılandırma

Azure Otomasyonu için, AWS ile iletişim kurmak amacıyla önce AWS kimlik bilgilerinizi almanız ve bunları Azure Otomasyonu’nda varlıklar olarak depolamanız gerekir. Erişim Anahtarı oluşturmak için [AWS Hesabınız için Erişim Anahtarlarını Yönetme](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) AWS belgesinde belgelenen aşağıdaki adımları uygulayın ve **Erişim Anahtarı Kimliği** ve **Gizli Erişim Anahtarı**’nı kopyalayın (isteğe bağlı olarak, anahtar dosyanızı güvenli bir yerde saklamak için indirin).

AWS güvenlik anahtarlarınızı oluşturup kopyaladıktan sonra, bunları güvenli bir şekilde depolamak ve runbook’larınızla bunlara başvurmak için Azure Otomasyonu hesabıyla bir Kimlik Bilgisi hesabı oluşturmanız gerekir. [Azure Otomasyonu makalesindeki kimlik bilgileri varlıkları](shared-resources/credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) bölümünde **Yeni bir kimlik bilgisi oluşturmak** ve aşağıdaki bilgileri girmek için bölümündeki adımları izleyin:

1. **Ad** kutusuna **AWScred** veya adlandırma standartlarınıza uygun bir değer girin.
2. **Kullanıcı adı** kutusuna kendinize ait **Erişim Kimliği**'ni yazın; **Gizli Erişim Anahtarı**'nı da **Parola** ve **Parolayı Onayla** kutusuna girin.

## <a name="next-steps"></a>Sonraki adımlar

* AWS'de görevleri otomatikleştirmek için runbook'ların nasıl oluşturulacağını öğrenmek için [Amazon Web Hizmetleri'nde VM'nin dağıtımını otomatik hale getirme](automation-scenario-aws-deployment.md) çözüm makalesini gözden geçirin.
