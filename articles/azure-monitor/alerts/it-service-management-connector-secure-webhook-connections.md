---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma
description: Bu makalede, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de güvenli dışarı aktarma ile ıSM ürünlerinizi/hizmetlerinizi nasıl bağlayabileceğinizi gösterir.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: 330b97e66ba1a9b95f2ef7a34a7ea046414b8bd8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037529"
---
# <a name="connect-azure-to-itsm-tools-by-using-secure-export"></a>Güvenli dışarı aktarma kullanarak Azure 'dan ıTSM araçlarına bağlanma

Bu makalede, güvenli dışarı aktarma kullanarak BT hizmet yönetimi (ıTSM) ürün veya hizmeti arasındaki bağlantının nasıl yapılandırılacağı gösterilir.

Güvenli dışarı aktarma [BT hizmet yönetimi Bağlayıcısı (ıSMC)](./itsmc-overview.md)güncelleştirilmiş bir sürümüdür. Her iki sürüm de Azure Izleyici uyarı gönderdiğinde bir ITSM aracında iş öğeleri oluşturmanızı sağlar. Bu işlevsellik ölçüm, günlük ve etkinlik günlüğü uyarılarını içerir.

ISMC Kullanıcı adı ve parola kimlik bilgilerini kullanıyor. Güvenli dışarı aktarmanın Azure Active Directory (Azure AD) kullandığından daha güçlü kimlik doğrulaması vardır. Azure AD, Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir. Kullanıcıların oturum açıp iç veya dış kaynaklara erişmesine yardımcı olur. ITSM ile Azure AD 'nin kullanılması, dış sisteme gönderilen Azure uyarılarını (Azure AD uygulama KIMLIĞI aracılığıyla) belirlemenize yardımcı olur.

> [!NOTE]
> Güvenli dışarı aktarma kullanarak Azure 'dan ıTSM araçlarına bağlanma özelliği önizlemededir.

## <a name="secure-export-architecture"></a>Güvenli dışarı aktarma mimarisi

Güvenli dışarı aktarma mimarisi aşağıdaki yeni özellikleri tanıtır:

* **Yeni eylem grubu**: uyarılar, ITSM eylem grubu yerine güvenli Web kancası eylem grubu aracılığıyla ITSM aracına gönderilir.
* **Azure AD kimlik doğrulaması**: kimlik doğrulaması, Kullanıcı adı/parola kimlik bilgileri yerıne Azure AD aracılığıyla yapılır.

## <a name="secure-export-data-flow"></a>Güvenli dışarı aktarma veri akışı

Güvenli dışarı aktarma veri akışı adımları şunlardır:

1. Azure Izleyici, güvenli dışarı aktarma kullanmak üzere yapılandırılmış bir uyarı gönderir.
2. Uyarı yükü, ıTSM aracına güvenli bir Web kancası eylemi tarafından gönderilir.
3. Uyarının ıTSM aracını girme yetkisi varsa, ıTSM uygulaması Azure AD ile kontrol eder.
4. Uyarı yetkilendirilirse, uygulama:
   
   1. ITSM aracında bir iş öğesi (örneğin, bir olay) oluşturur.
   2. Yapılandırma öğesinin (CI) KIMLIĞINI müşteri yönetim veritabanına (CMDB) bağlar.

![ITSM aracının Azure A, Azure uyarıları ve bir eylem grubuyla nasıl iletişim kuracağını gösteren diyagram.](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="benefits-of-secure-export"></a>Güvenli dışarı aktarma avantajları

Tümleştirmenin başlıca avantajları şunlardır:

* **Daha iyi kimlik doğrulaması**: Azure AD, genellikle ısmc 'da oluşan zaman aşımları olmadan daha güvenli kimlik doğrulaması sağlar.
* **ITSM aracında çözümlenen uyarılar**: ölçüm uyarıları "tetiklenir" ve "çözümlendi" durumlarını uygular. Koşul karşılandığında, uyarı durumu "tetiklenir" olur. Koşul artık karşılanmazsa, uyarı durumu "çözüldü" olur. ISMC 'da, uyarılar otomatik olarak çözümlenemiyor. Güvenli dışarı aktarma sayesinde, çözümlenen durum ıTSM aracına akar ve bu nedenle otomatik olarak güncelleştirilir.
* **[Ortak uyarı şeması](./alerts-common-schema.md)**: ısmc 'da, uyarı yükünün şeması, uyarı türüne göre farklılık gösterir. Güvenli dışarı aktarma bölümünde tüm uyarı türleri için ortak bir şema vardır. Bu ortak şema tüm uyarı türleri için CI 'yi içerir. Tüm Uyarı türleri CI 'yi CMDB ile bağlayabilecektir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-overview.md)