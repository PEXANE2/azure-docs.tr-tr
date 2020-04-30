---
title: Amazon Web Services ile Azure Otomasyonu runbook kimlik doğrulaması
description: Bu makale, AWS kaynaklarını yöneten Azure Automation'daki runbook'lar için bir AWS kimlik bilgisinin nasıl oluşturulup doğrulandığı açıklanmaktadır.
keywords: aws kimlik doğrulaması, aws yapılandırma
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 1bf60d17af1d9866de6a62ac538fa0bd9a15ce52
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82113400"
---
# <a name="authenticate-azure-automation-runbooks-with-amazon-web-services"></a>Amazon Web Services ile Azure Otomasyonu runbook kimlik doğrulaması

Amazon Web Hizmetleri’ndeki (AWS) kaynaklarla ortak görevlerin otomatikleştirilmesi Azure’ün Automation runbook'ları ile sonuçlandırılabilir. Tıpkı Azure’deki kaynaklarla yapabildiğiniz gibi Automation runbook'ları kullanarak AWS’deki birçok görevi otomatik hale getirebilirsiniz. Gerekenlerin tümü şu iki şeydir:

* Bir AWS aboneliği ve bir dizi kimlik bilgisi. Özellikle AWS Erişim Anahtarınız ve Gizli Anahtarınız. Daha fazla bilgi için [AWS Kimlik Bilgilerini Kullanma](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) makalesini gözden geçirin.
* Bir Azure aboneliği ve Automation hesabı.

AWS kullanarak kimlik doğrulamak için, Azure Automation’dan çalışan runbook’larınızın kimliklerini doğrulamak amacıyla bir dizi AWS kimlik bilgisi belirtmeniz gerekir. Zaten oluşturulmuş bir Automation hesabınız varsa ve AWS ile bunu kimlik doğrulamasını yapmak için kullanmak istiyorsanız aşağıdaki bölümde yer alan adımları uygulayabilirsiniz. AWS kaynaklarını hedefleyen runbook 'lar için bir hesap atamak istiyorsanız, önce yeni bir [Otomasyon hesabı](automation-create-standalone-account.md) oluşturmanız ve farklı çalıştır hesabı oluşturma adımını atlamanız gerekir. Hesabı oluşturduktan sonra, yapılandırmayı gerçekleştirmek için aşağıdaki adımları izleyin.

## <a name="configure-automation-account"></a>Automation hesabı yapılandırma

Azure Otomasyonu için, AWS ile iletişim kurmak amacıyla önce AWS kimlik bilgilerinizi almanız ve bunları Azure Otomasyonu’nda varlıklar olarak depolamanız gerekir. Erişim Anahtarı oluşturmak için [AWS Hesabınız için Erişim Anahtarlarını Yönetme](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) AWS belgesinde belgelenen aşağıdaki adımları uygulayın ve **Erişim Anahtarı Kimliği** ve **Gizli Erişim Anahtarı**’nı kopyalayın (isteğe bağlı olarak, anahtar dosyanızı güvenli bir yerde saklamak için indirin).

AWS güvenlik anahtarlarınızı oluşturup kopyaladıktan sonra, bunları güvenli bir şekilde depolamak ve runbook’larınızla bunlara başvurmak için Azure Otomasyonu hesabıyla bir Kimlik Bilgisi hesabı oluşturmanız gerekir. [Azure Otomasyonu makalesindeki kimlik bilgileri varlıkları](shared-resources/credentials.md#create-a-new-credential-asset-with-the-azure-portal) bölümünde **Yeni bir kimlik bilgisi oluşturmak** ve aşağıdaki bilgileri girmek için bölümündeki adımları izleyin:

1. **Ad** kutusuna **AWScred** veya adlandırma standartlarınıza uygun bir değer girin.
2. **Kullanıcı adı** kutusuna kendinize ait **Erişim Kimliği**'ni yazın; **Gizli Erişim Anahtarı**'nı da **Parola** ve **Parolayı Onayla** kutusuna girin.

## <a name="next-steps"></a>Sonraki adımlar

* AWS'de görevleri otomatikleştirmek için runbook'ların nasıl oluşturulacağını öğrenmek için [Amazon Web Hizmetleri'nde VM'nin dağıtımını otomatik hale getirme](automation-scenario-aws-deployment.md) çözüm makalesini gözden geçirin.
