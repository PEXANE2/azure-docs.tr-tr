---
title: FHıR için Azure API için ek ayarlar
description: FHıR için Azure API için ayarlayabileceğiniz ek ayarlara genel bakış
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.reviewer: mihansen
ms.author: cavoeg
author: CaitlinV39
ms.date: 11/22/2019
ms.openlocfilehash: 896d5bafd879ff3ba09bd5b8922cde4cd8345689
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "84871955"
---
# <a name="additional-settings-for-azure-api-for-fhir"></a>FHıR için Azure API için ek ayarlar

Bu nasıl yapılır kılavuzunda, FHıR için Azure API 'niz içinde ayarlamak isteyebileceğiniz ek ayarları gözden geçitireceğiz. Daha ayrıntılı bilgi edinmek için ek sayfalar vardır.

## <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma

FHıR için Azure API, verilerini depolamak için veritabanını kullanır. Temel alınan veritabanının performansı, hizmet sağlama sırasında veya hizmet sağlandıktan sonra veritabanı ayarlarında seçilen Istek birimlerinin (RU) sayısına bağlıdır.

Veritabanı için her zaman yeterli sistem kaynaklarının kullanılabilir olduğundan emin olmak için üretilen iş sağlanmalıdır. Uygulamanız için ihtiyacınız olan RUs, gerçekleştirdiğiniz işlemlere göre değişir. İşlemler basit okuma ve yazma işlemlerini daha karmaşık sorgulara göre değişebilir.

Varsayılan ayarların nasıl değiştirileceği hakkında daha fazla bilgi için bkz. [veritabanı ayarlarını yapılandırma](configure-database.md).

## <a name="access-control"></a>Erişim denetimi

FHıR için Azure API yalnızca yetkili kullanıcıların FHıR API 'sine erişmesine izin verir. Yetkili kullanıcıları iki farklı mekanizma aracılığıyla yapılandırabilirsiniz. Erişim denetimini yapılandırmanın birincil ve önerilen yolu, **erişim denetimi (IAM)** dikey penceresi aracılığıyla erişilebilen [Azure rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/)kullanmaktır. Azure RBAC yalnızca aboneliğinizle ilişkili Azure Active Directory kiracı kullanarak veri düzlemi erişimini güvenli hale getirmek istiyorsanız işe yarar. Farklı bir kiracı kullanmak istiyorsanız, FHıR için Azure API 'SI, yerel bir FHıR veri düzlemi erişim denetimi mekanizması sunar. Yapılandırma seçenekleri, yerel RBAC mekanizması kullanılırken zengin değildir. Ayrıntılar için aşağıdaki seçeneklerden birini belirleyin:

* [FHıR veri düzlemi Için Azure RBAC](configure-azure-rbac.md). Aboneliğinizle ilişkili Azure Active Directory kiracı kullanırken bu tercih edilen seçenektir.
* [Yerel fhır veri düzlemi erişim denetimi](configure-local-rbac.md). Bu seçeneği yalnızca veri düzlemi erişim denetimi için bir dış Azure Active Directory kiracısı kullanmanız gerektiğinde kullanın. 

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme
Hizmetinizi izleyebilmek ve uyumluluk amaçlarıyla doğru şekilde raporlanmasını sağlamak için, kurulum 'un bir parçası olarak tanılama günlüğünü etkinleştirmek isteyebilirsiniz. Tanılama günlüğünü ayarlama hakkında daha fazla bilgi için bkz. tanılama günlüğünü ayarlama hakkında, bazı örnek sorgularla birlikte nasıl yapılır [Kılavuzu](enable-diagnostic-logging.md) . 

## <a name="use-custom-headers-to-add-data-to-audit-logs"></a>Denetim günlüklerine veri eklemek için özel üst bilgileri kullanma
FHıR için Azure API 'sinde, arama sisteminden gelen günlüklere ek bilgi eklemek isteyebilirsiniz. Bu bilgileri dahil etmek için özel üst bilgileri kullanabilirsiniz.

Çeşitli bilgi türlerini yakalamak için özel üst bilgileri kullanabilirsiniz. Örneğin:

* Kimlik veya yetkilendirme bilgileri
* Çağıranın kaynağı
* Kaynak kuruluş
* İstemci sistem ayrıntıları (elektronik sistem durumu kaydı, hasta portalı)

Bu verileri denetim günlüklerinizi eklemek için bkz. [Denetim günlüklerine veri eklemek Için özel http üst bilgileri kullanma](use-custom-headers.md) nasıl yapılır Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda, FHıR için Azure API 'SI için ek ayarlar ayarlarsınız.

Daha sonra, FHıR verilerini okuyan bir Web uygulaması oluşturmak için öğretici dizisine göz atın.

>[!div class="nextstepaction"]
>[JavaScript uygulaması dağıtma](tutorial-web-app-fhir-server.md)