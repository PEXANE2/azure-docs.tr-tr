---
title: Azure Key Vault'a Genel Bakış - Azure Key Vault | Microsoft Dokümanlar
description: Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: overview
ms.custom: mvc
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 4e2953b107b017d032e737e2878472166c677839
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78194963"
---
# <a name="what-is-azure-key-vault"></a>Azure Anahtar Kasası nedir?

Azure Key Vault aşağıdaki sorunları çözmeye yardımcı olur:

- **Gizli Dizi Yönetimi**: Belirteçleri, parolaları, sertifikaları, API anahtarlarını ve diğer gizli dizileri Güvenle depolamak ve bunlara erişimi sıkı bir şekilde denetlemek için Azure Key Vault kullanılabilir.
- **Anahtar Yönetimi**: Azure Key Vault, Anahtar Yönetimi çözümü olarak da kullanılabilir. Azure Key Vault, verilerinizi şifrelemek için kullanılan şifreleme anahtarlarını oluşturmayı ve denetlemeyi kolaylaştırır. 
- **Sertifika Yönetimi** - Azure Key Vault, Azure ve dahili bağlı kaynaklarınızla kullanmak üzere genel ve özel Taşıma Katmanı Güvenliği/Güvenli Soketkatmanı (TLS/SSL) sertifikalarını kolayca sağlamanızı, yönetmenize ve dağıtmanıza olanak tanıyan bir hizmettir. 
- **Donanım Güvenlik Modülleri tarafından desteklenen saklama sırları** - Sırlar ve anahtarlar yazılım veya FIPS 140-2 Seviye 2 onaylı HSM'ler tarafından korunabilir

## <a name="why-use-azure-key-vault"></a>Neden Azure Key Vault kullanmalıyım?

### <a name="centralize-application-secrets"></a>Uygulama gizli dizilerini merkezi hale getirme

Azure Key Vault’ta uygulama gizli dizilerinin depolanmasını merkezi hale getirerek dağılımlarını denetleyebilirsiniz. Key Vault, gizli dizilerin yanlışlıkla sızdırılma olasılığını büyük oranda azaltır. Key Vault kullanırken, uygulama geliştiricilerinin güvenlik bilgilerini uygulamalarında depolaması artık gerekli değildir. Uygulamalarda güvenlik bilgilerini depolamak zorunda olmamak, bu bilgileri kodun bir parçası haline getirme gereksinimini ortadan kaldırır. Örneğin, bir uygulamanın bir veritabanına bağlanması gerekebilir. Bağlantı dizesini uygulamanın kodunda depolamak yerine, güvenli bir şekilde Key Vault'ta saklayabilirsiniz.

Uygulamalarınız, URI'leri kullanarak ihtiyaç duydukları bilgilere güvenli bir şekilde erişebilir. Bu URI'ler, uygulamaların bir sırrın belirli sürümlerini almasına izin verir. Key Vault'ta depolanan gizli bilgilerin hiçbirini korumak için özel kod yazmaya gerek yoktur.

### <a name="securely-store-secrets-and-keys"></a>Gizli dizileri ve anahtarları güvenle depolama

Gizli diziler ve anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri (HSM'ler) kullanılarak Azure tarafından korunur. Kullanılan HSM’ler Federal Bilgi İşleme Standartları (FIPS) 140-2 Düzey 2 ile doğrulanmıştır.

Bir anahtar kasasına erişim, bir çağıranın (kullanıcı ya da uygulama) erişim elde edebilmesi için uygun kimlik doğrulaması ve yetkilendirme gerektirir. Kimlik doğrulama işlemi çağıranın kimliğini, yetkilendirme işlemi ise çağıranın yapmaya izinli olduğu işlemleri belirler.

Kimlik doğrulaması Azure Active Directory aracılığıyla yapılır. Yetkilendirme, rol tabanlı erişim denetimi (RBAC) veya Key Vault erişim ilkesi aracılığıyla yapılabilir. Kasaların yönetimi gerçekleştirilirken RBAC, bir kasada depolanmış verilere erişmeye çalışırken ise anahtar kasası erişim ilkesi kullanılır.

Azure Anahtar Kasaları yazılım veya donanım HSM korumalı olabilir. Ek güvence gereken durumlar için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. Microsoft, nCipher donanım güvenlik modüllerini kullanır. Bir anahtarı HSM'nizden Azure Key Vault'a taşımak için nCipher araçlarını kullanabilirsiniz.

Son olarak Azure Key Vault, Microsoft verilerinizi görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.

### <a name="monitor-access-and-use"></a>Erişimi ve kullanımı izleme

Birkaç Key Vault oluşturduktan sonra anahtarlarınıza ve gizli dizilerinize nasıl ve ne zaman erişildiğini izlemek istersiniz. Kasalarınız için günlüğe kaydetmeyi etkinleştirerek etkinliği izleyebilirsiniz. Azure Key Vault’u aşağıdaki işlemleri yapacak şekilde yapılandırabilirsiniz:

- Bir depolama hesabına arşivleme.
- Bir olay hub'ına akış yapma.
- Günlükleri Azure Monitor günlüklerine gönderin.

Günlükleriniz üzerinde denetime sahip olursunuz ve erişimi kısıtlayarak günlükleri güvenli hale getirebilir ve artık gerekli olmayan günlükleri de silebilirsiniz.

### <a name="simplified-administration-of-application-secrets"></a>Uygulama gizli dizilerinin basitleştirilmiş yönetimi

Değerli verileri depolarken birkaç adım uygulamanız gerekir. Güvenlik bilgileri güvenli olmalı, bir yaşam döngüsü takip etmeli ve son derece kullanılabilir olmalıdır. Azure Key Vault, aşağıdaki leri yaparak bu gereksinimleri karşılama işlemini kolaylaştırır:

- Donanım Güvenlik Modülleri şirket içi bilgi ihtiyacını ortadan kaldırmak.
- Kuruluşunuzun ani kullanım artışlarını karşılamak için kısa süre içinde ölçek artırma.
- Bir bölge içindeki Anahtar Kasanızın içeriklerini ikincil bir bölgeye çoğaltma. Veri çoğaltma yüksek kullanılabilirlik sağlar ve failover tetiklemek için yöneticiden herhangi bir eylem ihtiyacını ortadan alır.
- Portal, Azure CLI ve PowerShell aracılığıyla standart Azure yönetim seçeneklerini sağlama.
- Genel CA’lardan satın aldığınız sertifikalarla ilgili kaydetme ve yenileme gibi belirli görevleri otomatikleştirme.

Ayrıca, Azure Anahtar Kasalarını kullanarak uygulama gizli dizilerini ayırabilirsiniz. Uygulamalar yalnızca erişebilecekleri kasaya erişebilir ve yalnızca belirli işlemleri gerçekleştirmekle sınırlandırılabilir. Uygulama başına bir Azure Key Vault oluşturabilir ve bir Key Vault’ta depolanmış gizli dizileri belirli bir uygulama ve geliştirici takımı ile kısıtlayabilirsiniz.

### <a name="integrate-with-other-azure-services"></a>Diğer Azure hizmetleri ile tümleştirme

Azure'da güvenli bir mağaza olarak Key Vault, şu senaryoları basitleştirmek için kullanılmıştır:
-  [Azure Disk Şifrelemesi](../security/fundamentals/encryption-overview.md)
-  SQL sunucusu nda ve Azure SQL Veritabanı'nda [her zaman şifrelenmiş]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) işlevsellik
- [Azure Uygulama Hizmeti]( https://docs.microsoft.com/azure/app-service/configure-ssl-certificate). 

Key Vault, depolama hesapları, olay hub’ları ve günlük analizi ile tümleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: CLI kullanarak bir Azure Key Vault oluşturma](quick-create-cli.md)
- [Anahtar Kasasından gizli dizi okumak için bir Azure web uygulaması yapılandırma](tutorial-web-application-keyvault.md)
