---
title: Azure Anahtar Kasası nedir? | Microsoft Belgeleri
description: Bulut uygulamalarının ve hizmetlerinin kullandığı şifreleme anahtarlarını ve gizli dizileri Azure Key Vault nasıl korutuını öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: mbaldwin
ms.openlocfilehash: 03ad504258dd9448753f37402067a0da3e0a2c00
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197649"
---
# <a name="azure-key-vault-basic-concepts"></a>Azure Key Vault temel kavramlar

Azure Key Vault, gizli dizilerin güvenli bir şekilde depolanması ve bunlara erişim sağlanması için tasarlanmış bir araçtır. Gizli dizi; API anahtarları, parolalar veya sertifikalar gibi erişimi sıkı bir şekilde denetlemek istediğiniz tüm öğelerdir. Kasa, mantıksal bir gizli dizi grubudur.

Diğer önemli terimler şunlardır:

- **Kiracı**: Kiracı, Microsoft bulut hizmetlerinin belirli bir örneğine sahip olan ve onu yöneten kuruluştur. Genellikle bir kuruluşun Azure ve Office 365 hizmetleri kümesine başvurmak için kullanılır.

- **Kasa sahibi**: Kasa sahibi, anahtar kasası oluşturabilir ve anahtar kasası üzerinde tam erişime ve denetime sahip olabilir. Kasa sahibi, gizli dizilere ve anahtarlara kimlerin eriştiğini günlüğe kaydetmek için belirli denetim özellikleri de ayarlayabilir. Yöneticiler anahtarların yaşam döngüsünü kontrol edebilir. Anahtarı yeni sürüme geçirebilir, yedekleyebilir ve ilgili görevleri gerçekleştirebilirler.

- **Kasa tüketicisi**: Kasa tüketicisi, kasa sahibi tarafından tüketici erişimi sağlandığında, anahtar kasasındaki varlıklar üzerinde belirli eylemler gerçekleştirebilir. Kullanılabilen eylemler verilen izinlere bağlıdır.

- **Kaynak**: Azure aracılığıyla kullanılan, yönetilebilir bir öğedir. Sanal makine, depolama hesabı, Web uygulaması, veritabanı ve sanal ağ örnekleri yaygın olarak verilebilir. Birçok daha vardır.

- **Kaynak grubu**: Kaynak grubu, Azure çözümleri için ilgili kaynakları bir arada tutan kapsayıcılardır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.

- **Hizmet sorumlusu**: Azure hizmet sorumlusu, Kullanıcı tarafından oluşturulan uygulamaların, hizmetlerin ve otomasyon araçlarının belirli Azure kaynaklarına erişmek için kullandığı bir güvenlik kimliğidir. Bunu belirli bir role sahip "Kullanıcı kimliği" (Kullanıcı adı ve parola veya sertifika) olarak düşünebilirsiniz ve sıkı denetimli izinlere sahip olmanız gerekir. Genel kullanıcı kimliğinin aksine, hizmet sorumlusunun yalnızca belirli şeyleri yapabilmesi gerekir. Yalnızca, yönetim görevlerini gerçekleştirmesi için ihtiyacı olan en düşük izin düzeyini verirseniz güvenliği geliştirir.

- [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md): Azure AD, kiracı için Active Directory hizmetidir. Her dizinde bir veya daha fazla etki alanı vardır. Dizinde birden fazla abonelik bulunabilir ancak tek bir kiracı olur.

- **Azure kiracı kimliği**: Kiracı kimliği, bir Azure aboneliğinde Azure AD örneğini tanımlamanın benzersiz bir yoludur.

- **Yönetilen kimlikler**: Azure Key Vault kimlik bilgilerini ve diğer anahtarları ve gizli dizileri güvenli bir şekilde depolamanıza olanak tanır, ancak kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir. Yönetilen bir kimlik kullanmak, Azure hizmetlerine Azure AD 'de otomatik olarak yönetilen bir kimlik vererek bu sorunu daha kolay bir şekilde çözmesini sağlar. Bu kimliği kullanarak, Key Vault veya Azure AD kimlik doğrulamasını destekleyen tüm hizmetler için kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. Daha fazla bilgi için, aşağıdaki görüntüye ve [Azure kaynakları için yönetilen kimliklere genel bakış](../active-directory/managed-identities-azure-resources/overview.md)bölümüne bakın.

    ![Azure kaynakları için yönetilen kimliklerin nasıl çalıştığı diyagramı](./media/key-vault-whatis/msi.png)

## <a name="authentication"></a>Kimlik Doğrulaması
Key Vault tüm işlemleri yapmak için önce bu kimlik doğrulaması yapmanız gerekir. Key Vault doğrulamak için üç yol vardır:

- [Azure kaynakları Için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md): Azure 'da bir sanal makinede uygulama dağıtırken, Key Vault erişimi olan sanal makinenize bir kimlik atayabilirsiniz. Ayrıca, [diğer Azure kaynaklarına](../active-directory/managed-identities-azure-resources/overview.md)kimlik de atayabilirsiniz. Bu yaklaşımın avantajı, uygulamanın veya hizmetin ilk gizli dizi dönüşünü yönetmediğinde. Azure, kimliği otomatik olarak döndürür. En iyi uygulama olarak bu yaklaşımı öneririz. 
- **Hizmet sorumlusu ve sertifikası**: bir hizmet sorumlusu ve Key Vault erişimi olan ilişkili bir sertifika kullanabilirsiniz. Uygulama sahibi veya geliştiricinin sertifikayı döndürmelerinden dolayı bu yaklaşımı önermiyoruz.
- **Hizmet sorumlusu ve gizli**dizi: Key Vault kimlik doğrulaması yapmak için bir hizmet sorumlusu ve gizli dizi kullanabilseniz de, bunu önermiyoruz. Key Vault için kimlik doğrulaması yapmak üzere kullanılan önyükleme gizliliğini otomatik olarak döndürmek zordur.


## <a name="key-vault-roles"></a>Key Vault rolleri

Geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılamaya Anahtar Kasası'nın nasıl yardımcı olabileceğini daha iyi anlamak için aşağıdaki tabloyu kullanın.

| Rol | Sorun bildirimi | Azure Anahtar Kasası tarafından çözüldü |
| --- | --- | --- |
| Bir Azure uygulaması geliştiricisi |"İmzalama ve şifreleme için anahtarları kullanan bir Azure uygulaması yazmak istiyorum. Ancak, çözümün coğrafi olarak dağıtılan bir uygulamaya uygun olması için bu anahtarların uygulamamın dışında olmasını istiyorum. <br/><br/>Kodu kendim yazmak zorunda kalmadan bu anahtarların ve parolaların korunmasını istiyorum. Ayrıca bu anahtarları ve parolaları uygulamamdan en iyi performans ile kolayca kullanabilmek istiyorum." |√ Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.<br/><br/> √ Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri kullanılarak Azure tarafından korunur.<br/><br/> √ Anahtarlar uygulamalarla aynı Azure veri merkezinde bulunan HSM'lerde işlenir. Bu yöntem, şirket içi gibi ayrı konumlarda bulunan anahtarlardan daha fazla güvenilirlik ve daha az gecikme sağlar. |
| Hizmet olarak yazılım (SaaS) geliştiricisi |"Müşterilerimin kiracı anahtarları ve gizli anahtarları için sorumluluk veya olası bir yükümlülük almak istemiyorum. <br/><br/>Müşterilerin kendi anahtarlarına sahip olmalarını ve bunları yönetmelerini istiyorum; böylece en iyi yaptığım şeye, yani çekirdek yazılım özelliklerini sağlamaya odaklanabilirim." |√ Müşteriler kendi anahtarları Azure'a aktarabilir ve bunları yönetebilir. SaaS uygulamasının, müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, Key Vault bu işlemleri uygulama adına yapar. Uygulama, müşterilerin anahtarlarını görmez. |
| Güvenlik başkanı (CSO) |"Uygulamalarımızın güvenli anahtar yönetimi için FIPS 140-2 Düzey 2 HSM'lerle uyumlu olduğunu bilmek istiyorum. <br/><br/>Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum. <br/><br/>Ayrıca, birden çok Azure hizmeti ve kaynağı kullanıyor olsak da, anahtarları Azure'da tek bir konumdan yönetmek istiyorum." |√ HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir.<br/><br/>√ Anahtar Kasası, Microsoft anahtarlarınızı görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.<br/><br/>√ Anahtar kullanımı neredeyse gerçek zamanlı olarak günlüğe kaydedilir.<br/><br/>√ Kasa, Azure'da kaç kasanız olduğuna, bunların hangi bölgeleri desteklediğine ve hangi uygulamaların bunları kullandığına bakmaksızın tek bir arabirim sağlar. |

Bir Azure aboneliği olan herhangi biri, anahtar kasalarını oluşturabilir ve kullanabilir. Key Vault, geliştiricilerin ve güvenlik yöneticilerinin avantajları olsa da, diğer Azure hizmetlerini yöneten bir kuruluşun yöneticisi tarafından uygulanabilir ve yönetilebilir. Örneğin, bu yönetici bir Azure aboneliğiyle oturum açabilir, anahtar depolayabileceği kuruluş için bir kasa oluşturabilir ve ardından bunlar gibi işletimsel görevlerden sorumludur:

- Bir anahtar veya gizli anahtarı oluşturma veya içeri aktarma
- Bir anahtar veya gizli anahtarı iptal etme veya silme
- Kullanıcıların veya uygulamaların, anahtar ve gizli anahtarları yönetmeleri veya kullanmaları için anahtar kasasına erişmelerini yetkilendirme
- Anahtar kullanımını (örneğin, imzalama veya şifreleme) yapılandırma
- Anahtar kullanımını izleme

Bu yönetici daha sonra geliştiricilerin uygulamalarından çağrı oluşturmasını sağlar. Bu yönetici ayrıca güvenlik yöneticisine anahtar kullanım günlüğü bilgileri verir. 

![Azure Key Vault nasıl çalıştığına genel bakış][1]

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için bkz. [Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

[Kasalarınızın güvenliğini](key-vault-secure-your-key-vault.md)nasıl sağlayacağınızı öğrenin.

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).
