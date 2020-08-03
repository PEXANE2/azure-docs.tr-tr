---
title: Çoğaltma kümeleri Azure AD Domain Services için kavramlar | Microsoft Docs
description: Azure Active Directory Domain Services hangi çoğaltma kümelerinin olduğunu ve kimlik hizmetleri gerektiren uygulamalar için nasıl artıklık sağladığını öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 07/16/2020
ms.author: iainfou
ms.openlocfilehash: 698009ee8a57ed5d30e01376b4f2c63b0a27ead8
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87505737"
---
# <a name="replica-sets-concepts-and-features-for-azure-active-directory-domain-services-preview"></a>Çoğaltma, Azure Active Directory Domain Services için kavramları ve özellikleri belirler (Önizleme)

Azure Active Directory Domain Services (Azure AD DS) yönetilen bir etki alanı oluşturduğunuzda, benzersiz bir ad alanı tanımlarsınız. Bu ad alanı, *aaddscontoso.com*gibi etki alanı adıdır ve iki etki alanı denetleyicisi (DC) seçili Azure bölgenize dağıtılır. Bu DC dağıtımı bir çoğaltma kümesi olarak bilinir.

Yönetilen bir etki alanını, Azure AD kiracısı başına birden fazla çoğaltma kümesine sahip olacak şekilde genişletebilirsiniz. Çoğaltma kümeleri, Azure AD DS 'yi destekleyen herhangi bir Azure bölgesindeki eşlenen herhangi bir sanal ağa eklenebilir. Farklı Azure bölgelerindeki ek çoğaltma kümeleri, bir Azure bölgesi çevrimdışı kalırsa eski uygulamalar için coğrafi olağanüstü durum kurtarma sağlar.

Çoğaltma kümeleri Şu anda önizleme aşamasındadır.

> [!NOTE]
> Çoğaltma kümeleri, tek bir Azure kiracısında birden çok benzersiz yönetilen etki alanı dağıtmanıza izin vermez. Her çoğaltma kümesi aynı verileri içerir.

## <a name="how-replica-sets-work"></a>Çoğaltma kümeleri nasıl çalışır?

*Aaddscontoso.com*gibi yönetilen bir etki alanı oluşturduğunuzda, bir ilk çoğaltma kümesi oluşturulur. Ek çoğaltma kümeleri aynı ad alanını ve yapılandırmayı paylaşır. Yapılandırma, Kullanıcı kimliği ve kimlik bilgileri, gruplar, Grup İlkesi nesneleri, bilgisayar nesneleri ve diğer değişiklikler dahil olmak üzere Azure AD DS değişiklikleri, yönetilen etki alanındaki tüm çoğaltma kümelerine AD DS çoğaltma kullanılarak uygulanır.

Bir sanal ağ içinde her bir çoğaltma kümesini oluşturursunuz. Her sanal ağ, yönetilen bir etki alanının çoğaltma kümesini barındıran diğer tüm sanal ağlar için eşlenmelidir. Bu yapılandırma dizin çoğaltmasını destekleyen bir ağ topolojisi oluşturur. Bir sanal ağ, her bir çoğaltma kümesinin farklı bir sanal alt ağda olması şartıyla birden çok çoğaltma kümesini destekleyebilir.

Tüm çoğaltma kümeleri aynı Active Directory sitesine yerleştirilir. Sonuç olarak, tüm değişiklikler hızlı yakınsama için site içi çoğaltma kullanılarak dağıtılır.

> [!NOTE]
> Ayrı siteler tanımlamak ve çoğaltma kümeleri arasında çoğaltma ayarları tanımlamak mümkün değildir.

Aşağıdaki diyagramda, iki çoğaltma kümesi içeren bir yönetilen etki alanı gösterilmektedir. İlk çoğaltma kümesi, etki alanı ad alanıyla oluşturulur. İkinci bir çoğaltma kümesi şu şekilde oluşturulur:

![İki çoğaltma kümesiyle örnek yönetilen etki alanı diyagramı](./media/concepts-replica-sets/two-replica-set-example.png)

> [!NOTE]
> Çoğaltma kümeleri, bir çoğaltma kümesinin yapılandırıldığı bölgelerde kimlik doğrulama hizmetlerinin kullanılabilirliğini güvence altına aldığından emin olur. Bir uygulamanın bölgesel bir kesinti olması durumunda coğrafi yedeklilik olması için, yönetilen etki alanını kullanan uygulama platformunun de diğer bölgede bulunması gerekir.
>
> Azure VM 'Leri veya Azure uygulama hizmetleri gibi uygulamanın çalışması için gereken diğer hizmetlerin esnekliği, çoğaltma kümeleri tarafından sağlanmaz. Diğer uygulama bileşenlerinin kullanılabilirlik tasarımında, uygulamayı oluşturan hizmetlere yönelik dayanıklılık özellikleri dikkate alınması gerekir.

Aşağıdaki örnek, esneklik sağlamak ve kimlik doğrulama hizmetlerinin kullanılabilirliğini sağlamak için üç çoğaltma kümesi içeren bir yönetilen etki alanını gösterir. Her iki örnekte de uygulama iş yükleri, yönetilen etki alanı çoğaltma kümesiyle aynı bölgede bulunur:

![Üç çoğaltma kümesiyle örnek yönetilen etki alanı diyagramı](./media/concepts-replica-sets/three-replica-set-example.png)

## <a name="deployment-considerations"></a>Dağıtma konuları

Yönetilen bir etki alanı için varsayılan SKU, birden çok çoğaltma kümesini destekleyen *Kurumsal* SKU 'dır. *Standart* SKU 'ya değiştirdiyseniz ek çoğaltma kümeleri oluşturmak için, [yönetilen etki alanını](change-sku.md) *Enterprise* veya *Premium*'a yükseltin.

Önizleme süresince desteklenen en fazla çoğaltma kümesi sayısı, yönetilen etki alanını oluştururken oluşturulan ilk çoğaltma dahil olmak üzere dördü.

Her bir çoğaltma kümesi için faturalandırma, etki alanı yapılandırma SKU 'sunu temel alır. Örneğin, *Kurumsal* SKU 'yu kullanan bir yönetilen etki alanınız varsa ve üç çoğaltma kümesi varsa, aboneliğiniz her üç çoğaltma kümesi için saat başına faturalandırılır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="can-i-use-my-production-managed-domain-with-this-preview"></a>Bu önizleme ile üretim tarafından yönetilen etki alanını kullanabilir miyim?

Çoğaltma kümeleri, Azure AD Domain Services ortak bir önizleme özelliğidir. Üretim tarafından yönetilen bir etki alanını kullanabilirsiniz, ancak lütfen önizleme aşamasında özellikler için mevcut olan destek farklarını unutmayın. Önizlemeler hakkında daha fazla bilgi için, [ÖNIZLEME SLA Azure Active Directory](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="can-i-create-a-replica-set-in-subscription-different-from-my-managed-domain"></a>Abonelikte, yönetilen etki alanım 'dan farklı bir çoğaltma kümesi oluşturabilir miyim?

Hayır. Çoğaltma kümelerinin, yönetilen etki alanıyla aynı abonelikte olması gerekir.

### <a name="how-many-replica-sets-can-i-create"></a>Kaç tane çoğaltma kümesi oluşturabilirim?

Önizleme, en fazla dört çoğaltma kümesiyle sınırlıdır; yönetilen etki alanı için ilk çoğaltma kümesi ve üç ek çoğaltma kümesi vardır.

### <a name="how-does-user-and-group-information-get-synchronized-to-my-replica-sets"></a>Kullanıcı ve grup bilgileri, çoğaltma kümelerim ile nasıl eşitlenir?

Tüm çoğaltma kümeleri, bir ağ sanal ağ eşlemesi kullanılarak birbirlerine bağlanır. Bir çoğaltma kümesi, Azure AD 'den Kullanıcı ve grup güncelleştirmelerini alır. Bu değişiklikler daha sonra, eşlenmiş ağ üzerinden site içi AD DS çoğaltma kullanılarak diğer çoğaltma kümelerine çoğaltılır.

Şirket içi AD DS benzer şekilde, uzatılmış bağlantısı kesik bir durum, çoğaltmada kesintiye neden olabilir. Eşlenen sanal ağlar geçişli olmadığından, çoğaltma kümelerine yönelik tasarım gereksinimleri, tam olarak oluşturulmuş bir ağ topolojisi gerektirir.

### <a name="how-do-i-make-changes-in-my-managed-domain-after-i-have-replica-sets"></a>Çoğaltma kümelerim olduktan sonra yönetilen etki alanım 'da değişiklik Nasıl yaparım? mı?

Yönetilen etki alanı içindeki değişiklikler, daha önce yaptığdaymış gibi çalışır. [Yönetilen etki alanına KATıLMıŞ RSAT araçlarıyla bir yönetim sanal makinesi oluşturup kullanırsınız](tutorial-create-management-vm.md). Yönetilen etki alanına istediğiniz sayıda yönetim sanal makinesi ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Çoğaltma kümelerine başlamak için [Azure AD DS yönetilen bir etki alanı oluşturun ve yapılandırın][tutorial-create-advanced]. Dağıtıldığında, [ek çoğaltma kümeleri oluşturun ve kullanın][create-replica-set].

<!-- LINKS - INTERNAL -->
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-replica-set]: tutorial-create-replica-set.md
