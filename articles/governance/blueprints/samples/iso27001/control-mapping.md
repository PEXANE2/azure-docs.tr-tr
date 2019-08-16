---
title: Örnek-ISO 27001 şema-Control eşleme
description: ISO 27001 şema örneğinin Azure ilkesi için denetim eşlemesi.
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/22/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 7be9a208f82dd9a487c1794a7996f9bb11bb09d8
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515443"
---
# <a name="control-mapping-of-the-iso-27001-blueprint-sample"></a>ISO 27001 şema örneğinin denetim eşlemesi

Aşağıdaki makalede, Azure şemaları ISO 27001 şema örneği 'nin ISO 27001 denetimlerine nasıl eşleştiği açıklanır. Denetimler hakkında daha fazla bilgi için bkz. [ıso 27001](https://www.iso.org/isoiec-27001-information-security.html).

Aşağıdaki eşlemeler **ıso 27001:2013** denetimlerine göre yapılır. Sağ taraftaki gezinmeyi kullanarak doğrudan belirli bir denetim eşlemesine atlayın. Eşlenmiş denetimlerin birçoğu bir [Azure Policy](../../../policy/overview.md) girişimi ile uygulanır. Tüm girişimi gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin. Ardından,  **\[Önizleme\] denetimi ISO 27001:2013 denetimlerini bulun ve seçin ve denetim gereksinimleri yerleşik ilke girişimi ' ni desteklemek için belirli VM uzantılarını dağıtın** .

## <a name="a612-segregation-of-duties"></a>A. 6.1.2 ayrımı

Yalnızca bir Azure aboneliğinin sahibi, yönetici artıklığına izin vermez. Bunun tersine, çok fazla sayıda Azure aboneliği sahibi, güvenliği aşılmış bir sahip hesabı aracılığıyla ihlal olasılığını artırabilir. Bu şema, Azure abonelikleri için sahip sayısını denetleyen iki [Azure ilke](../../../policy/overview.md) tanımı atayarak uygun sayıda Azure abonelik sahibini korumanıza yardımcı olur. Abonelik sahibi izinlerinin yönetilmesi, görevlerin uygun bir şekilde ayrılmasını sağlamanıza yardımcı olabilir.

- \[Önizleme\]: Abonelik için en az sahip sayısını denetleme
- \[Önizleme\]: Abonelik için en fazla sahip sayısını denetleme

## <a name="a821-classification-of-information"></a>A. 8.2.1 bilgi sınıflandırması

Azure 'un [SQL güvenlik açığı değerlendirme hizmeti](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) , veritabanlarınızda depolanan hassas verileri bulmanıza yardımcı olabilir ve bu verileri sınıflandırmaya yönelik öneriler içerir. Bu şema, SQL güvenlik açığı değerlendirme taraması sırasında tanımlanan güvenlik açıklarını denetlemek için bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- \[Önizleme\]: Azure Güvenlik Merkezi 'nde SQL güvenlik açığı değerlendirmesi sonuçlarını izleme

## <a name="a912-access-to-networks-and-network-services"></a>A. 9.1.2, ağlara ve ağ hizmetlerine erişim

Azure, Azure kaynaklarına kimlerin erişebileceğini yönetmek için [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) uygular. Bu şema yedi [Azure ilke](../../../policy/overview.md) tanımı atayarak Azure kaynaklarına erişimi denetlemenize yardımcı olur. Bu ilkeler, kaynaklara daha fazla izin vermeyi sağlayan kaynak türlerinin ve yapılandırmaların kullanımını denetler.
Bu ilkeleri ihlal eden kaynakları anlamak, Azure kaynaklarının yetkili kullanıcılarla sınırlı olduğundan emin olmak için düzeltici eylemler almanıza yardımcı olabilir.

- \[Önizleme\]: Linux VM hesaplarını parola olmadan denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Linux VM 'yi denetlemek için VM Uzantısı 'nı, parola içermeyen hesaplardan uzak bağlantılara izin verecek şekilde dağıtın
- \[Önizleme\]: Parolasız Linux VM hesaplarını denetleme
- \[Önizleme\]: Parola içermeyen hesaplardan gelen uzak bağlantılara izin veren Linux VM 'yi denetleme
- Klasik depolama hesaplarının kullanımını denetleyin
- Klasik sanal makinelerin kullanımını denetleyin
- Yönetilen diskler kullanmayan VM'leri denetle

## <a name="a923-management-of-privileged-access-rights"></a>A. 9.2.3 ayrıcalıklı erişim haklarının yönetimi

Bu şema, sahibi ve/veya yazma izinleri ile dış hesapları ve/veya yazma izinlerini ve/veya yazma izinlerine sahip olmayan hesapları denetlemek için dört [Azure ilke](../../../policy/overview.md) tanımı atayarak ayrıcalıklı erişim haklarını kısıtlayıp denetlemenize yardımcı olur. Multi-Factor Authentication etkinleştirildi. Azure, Azure kaynaklarına kimlerin erişebileceğini yönetmek için rol tabanlı erişim denetimi (RBAC) uygular. Bu şema Ayrıca, SQL Server ve Service Fabric için Azure Active Directory kimlik doğrulaması kullanımını denetlemek üzere üç Azure ilke tanımı da atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcıları ve diğer Microsoft Hizmetleri için Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimine izin verebilir. Bu şema ayrıca özel RBAC kurallarının kullanımını denetlemek için bir Azure ilke tanımı atar. Özel RBAC kurallarının hangi noktada uygulanacağını anlamak, özel RBAC kuralları hata durumunda olduğundan, gereksinimi ve uygun uygulamayı doğrulamanıza yardımcı olabilir.

- \[Önizleme\]: Bir abonelikte MFA 'nın etkin olmadığı sahip izinleri olan denetim hesapları
- \[Önizleme\]: Abonelik üzerinde MFA etkinleştirilmemiş yazma izinleri olan hesapları denetleme
- \[Önizleme\]: Bir abonelikte sahip izinleri olan dış hesapları denetleme
- \[Önizleme\]: Bir abonelikteki yazma izinleri olan dış hesapları denetleyin
- SQL sunucusuna Azure Active Directory Yöneticisi sağlanmasını denetleyin.
- Service Fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanımını denetleyin
- Özel RBAC kurallarının kullanımını denetleyin

## <a name="a924-management-of-secret-authentication-information-of-users"></a>A. 9.2.4, kullanıcıların gizli kimlik doğrulama bilgilerinin yönetimi

Bu şema, Multi-Factor Authentication etkinleştirilmemiş hesapları denetlemek için üç [Azure ilke](../../../policy/overview.md) tanımı atar. Multi-Factor Authentication, bir dizi kimlik doğrulama bilgisinin tehlikeye düşmesi durumunda bile hesapların güvenli kalmasına yardımcı olur. Multi-Factor Authentication 'ı etkin olmayan hesapları izleyerek, tehlikeye geçmek daha olası olabilecek hesapları belirleyebilirsiniz. Bu şema Ayrıca, Linux VM parola dosyası izinlerinin yanlış ayarlandıklarında uyarı vermek üzere denetdukları iki Azure ilke tanımı da atar. Bu kurulum, kimlik doğrulayıcılar güvenliğinin aşılmadığından emin olmak için düzeltici eylem gerçekleştirmenizi sağlar.

- \[Önizleme\]: Bir abonelikte MFA 'nın etkin olmadığı sahip izinleri olan denetim hesapları
- \[Önizleme\]: Bir abonelikte MFA etkinleştirilmemiş okuma izinleri olan denetim hesapları
- \[Önizleme\]: Abonelik üzerinde MFA etkinleştirilmemiş yazma izinleri olan hesapları denetleme
- \[Önizleme\]: Linux VM passwd dosya izinlerini denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Linux VM/etc/passwd dosya izinlerinin denetimi 0644 olarak ayarlanır

## <a name="a925-review-of-user-access-rights"></a>A. 9.2.5 Kullanıcı erişimi haklarının Incelemesi

Azure, Azure 'daki kaynaklara kimlerin erişebileceğini yönetmenize yardımcı olmak için [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) uygular. Azure portal kullanarak, Azure kaynaklarına kimlerin erişebileceğini ve bunların izinlerini gözden geçirebilirsiniz. Bu şema, hesap ve yükseltilmiş izinlere sahip harici hesaplar dahil olmak üzere, gözden geçirme için öncelik verilmelidir, denetlenecek hesaplara dört adet [Azure ilke](../../../policy/overview.md) tanımı atar.

- \[Önizleme\]: Abonelik üzerinde kullanımdan kaldırılan hesapları denetleme
- \[Önizleme\]: Abonelik üzerinde sahip izinleri olan kullanım dışı hesapları denetleme
- \[Önizleme\]: Bir abonelikte sahip izinleri olan dış hesapları denetleme
- \[Önizleme\]: Bir abonelikteki yazma izinleri olan dış hesapları denetleyin

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A. 9.2.6 kaldırma veya erişim hakları ayarlama

Azure, Azure 'daki kaynaklara kimlerin erişebileceğini yönetmenize yardımcı olmak için [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) uygular. [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) ve RBAC kullanarak, Kullanıcı rollerini kurumsal değişiklikleri yansıtacak şekilde güncelleştirebilirsiniz. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu şema, kaldırma için göz önünde bulundurmanız gereken amorti edilmiş hesabı denetlemek için iki [Azure ilke](../../../policy/overview.md) tanımı atar.

- \[Önizleme\]: Abonelik üzerinde kullanımdan kaldırılan hesapları denetleme
- \[Önizleme\]: Abonelik üzerinde sahip izinleri olan kullanım dışı hesapları denetleme

## <a name="a942-secure-log-on-procedures"></a>A. 9.4.2 Secure oturum açma yordamları

Bu şema, Multi-Factor Authentication etkinleştirilmemiş hesapları denetlemek için üç Azure Ilke tanımı atar. Azure Multi-Factor Authentication ikinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve güçlü kimlik doğrulaması sunar. Multi-Factor Authentication 'ı etkin olmayan hesapları izleyerek, tehlikeye geçmek daha olası olabilecek hesapları belirleyebilirsiniz.

- \[Önizleme\]: Bir abonelikte MFA 'nın etkin olmadığı sahip izinleri olan denetim hesapları
- \[Önizleme\]: Bir abonelikte MFA etkinleştirilmemiş okuma izinleri olan denetim hesapları
- \[Önizleme\]: Abonelik üzerinde MFA etkinleştirilmemiş yazma izinleri olan hesapları denetleme

## <a name="a943-password-management-system"></a>A. 9.4.3 Password yönetim sistemi

Bu şema, en düşük güç ve diğer parola gereksinimlerini zorlayamama Windows VM 'Leri denetleyen 10 [Azure ilke](../../../policy/overview.md) tanımı atayarak güçlü parolalar zorlamanıza yardımcı olur. Parola gücü ilkesini ihlal eden VM 'lerin farkında, tüm VM Kullanıcı hesaplarının parolalarının ilkeyle uyumlu olduğundan emin olmak için düzeltici eylemler almanıza yardımcı olur.

- \[Önizleme\]: Windows VM 'yi denetlemek için VM uzantısı dağıtma parola karmaşıklığı gereksinimlerini zorlar
- \[Önizleme\]: Windows VM 'yi denetlemek için VM Uzantısı 'nı dağıtma en fazla parola yaşı 70 gün
- \[Önizleme\]: Windows VM 'yi denetlemek için VM Uzantısı 'nı dağıt en az parola yaşı 1 gün
- \[Önizleme\]: Windows VM parolalarını denetlemek için VM uzantısının dağıtılması en az 14 karakter olmalıdır
- \[Önizleme\]: Windows VM 'yi denetlemek için VM Uzantısı 'nı dağıtma önceki 24 parolalara izin vermiyor
- \[Önizleme\]: Windows VM 'yi denetleme parola karmaşıklığı gereksinimlerini zorlar
- \[Önizleme\]: Windows VM 'yi denetleme maksimum parola yaşı 70 gün
- \[Önizleme\]: Windows VM 'yi denetle en az parola yaşı 1 gün
- \[Önizleme\]: Windows VM parolalarının denetimi en az 14 karakter olmalıdır
- \[Önizleme\]: Windows VM 'nin önceki 24 parolaya izin vermemelidir

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>Şifreleme denetimlerinin kullanımıyla ilgili bir. 10.1.1 Ilkesi

Bu şema, belirli bir cryptograph denetimi uygulayan ve zayıf şifreleme ayarlarının kullanımını kontrol eden 13 [Azure ilke](../../../policy/overview.md) tanımı atayarak, bu şemayı cryptograph denetimlerinin kullanımı üzerinde zorlamanıza yardımcı olur.
Azure kaynaklarınızın en iyi durumda olmayan şifreleme yapılandırmalarının nerede olabileceğini anlamak, kaynakların bilgi güvenliği ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemler almanıza yardımcı olabilir. Özellikle, bu şema tarafından atanan ilkeler BLOB depolama hesapları ve Data Lake Storage hesapları için şifreleme gerektirir; SQL veritabanlarında saydam veri şifrelemesi gerektir; depolama hesaplarında, SQL veritabanlarında, sanal makine disklerinde ve Otomasyon hesabı değişkenlerinde eksik şifrelemeyi denetleyin; depolama hesaplarına, Işlev uygulamalarına, Web uygulamasına, API Apps ve Redis Cache yönelik güvenli olmayan bağlantıları denetleyin zayıf sanal makine parola şifrelemesini denetleme; ve şifrelenmemiş Service Fabric iletişimini denetleyin.

- \[Önizleme\]: İşlev Uygulaması için yalnızca HTTPS erişimini denetle
- \[Önizleme\]: Bir Web uygulaması için yalnızca HTTPS erişimini denetle
- \[Önizleme\]: Bir API uygulaması için yalnızca HTTPS erişimini denetle
- \[Önizleme\]: Depolama hesapları için eksik blob şifrelemesini denetle
- \[Önizleme\]: Windows VM 'yi denetlemek için VM Uzantısı 'nı dağıtma, ters çevrilebilir şifreleme kullanarak parolaları depolamamalıdır
- \[Önizleme\]: Windows VM 'yi denetleme, parolaları ters çevrilebilir şifreleme kullanarak depolamamalıdır
- \[Önizleme\]: Azure Güvenlik Merkezi 'nde şifrelenmemiş VM disklerini izleme
- Otomasyon hesabı değişkenlerinin şifrelemesinin etkinleştirilmesini denetleyin
- Redis Cache önbelleğinizde yalnızca güvenli bağlantıların etkinleştirilmesini denetleyin
- Depolama hesaplarına güvenli aktarımı denetleyin
- Service Fabric'te ClusterProtectionLevel özelliğinin EncryptAndSign olarak ayarlanmasını denetleyin
- Saydam veri şifreleme durumunu denetle
- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="a1241-event-logging"></a>A. 12.4.1 olay günlüğü

Bu şema, Azure kaynaklarında günlük ayarlarını denetleyen yedi [Azure ilke](../../../policy/overview.md) tanımı atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur.
Tanılama günlükleri, Azure kaynakları içinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar.

- \[Önizleme\]: Denetim Dependency Agent dağıtımı-VM görüntüsü (OS) listelenmemiş
- \[Önizleme\]: VMSS-VM görüntüsündeki (OS) denetim Dependency Agent dağıtımı listelenmemiş
- \[Önizleme\]: Denetim Log Analytics aracı dağıtımı-VM görüntüsü (OS) listelenmemiş
- \[Önizleme\]: VMSS-VM görüntüsündeki (OS) denetim Log Analytics Aracısı dağıtımı listelenmemiş
- Tanılama ayarını denetle
- SQL sunucu düzeyi Denetim ayarlarını denetle
- SQL Server üzerindeki gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="a1243-administrator-and-operator-logs"></a>A. 12.4.3 Yöneticisi ve işleç günlükleri

Bu şema, Azure kaynaklarında günlük ayarlarını denetleyen yedi Azure ilke tanımı atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur. Tanılama günlükleri, Azure kaynakları içinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar.

- \[Önizleme\]: Denetim Dependency Agent dağıtımı-VM görüntüsü (OS) listelenmemiş
- \[Önizleme\]: VMSS-VM görüntüsündeki (OS) denetim Dependency Agent dağıtımı listelenmemiş
- \[Önizleme\]: Denetim Log Analytics aracı dağıtımı-VM görüntüsü (OS) listelenmemiş
- \[Önizleme\]: VMSS-VM görüntüsündeki (OS) denetim Log Analytics Aracısı dağıtımı listelenmemiş
- Tanılama ayarını denetle
- SQL sunucu düzeyi Denetim ayarlarını denetle
- SQL Server üzerindeki gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="a1244-clock-synchronization"></a>A. 12.4.4 saat eşitleme

Bu şema, Azure kaynaklarında günlük ayarlarını denetleyen yedi Azure ilke tanımı atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur. Azure günlükleri, kaynak genelinde olayların zaman bağıntılı bir kaydını oluşturmak için eşitlenmiş iç saatleri kullanır.

- \[Önizleme\]: Denetim Dependency Agent dağıtımı-VM görüntüsü (OS) listelenmemiş
- \[Önizleme\]: VMSS-VM görüntüsündeki (OS) denetim Dependency Agent dağıtımı listelenmemiş
- \[Önizleme\]: Denetim Log Analytics aracı dağıtımı-VM görüntüsü (OS) listelenmemiş
- \[Önizleme\]: VMSS-VM görüntüsündeki (OS) denetim Log Analytics Aracısı dağıtımı listelenmemiş
- Tanılama ayarını denetle
- SQL sunucu düzeyi Denetim ayarlarını denetle
- SQL Server üzerindeki gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="a1251-installation-of-software-on-operational-systems"></a>A. 12.5.1 işletimsel sistemlere yazılım yüklemesi

Uyarlamalı uygulama denetimi, Azure Güvenlik Merkezi 'nin Azure 'da bulunan sanal makinelerinizde hangi uygulamaların çalıştırılacağını denetlemenize yardımcı olan bir çözümdür. Bu şema, izin verilen uygulamalar kümesinde yapılan değişiklikleri izleyen bir Azure ilke tanımı atar. Bu özellik, Azure VM 'lerinde yazılım ve uygulama yüklemelerini denetlemenize yardımcı olur.

- \[Önizleme\]: Azure Güvenlik Merkezi 'nde olası uygulama beyaz listesini izleme

## <a name="a1261-management-of-technical-vulnerabilities"></a>A. 12.6.1 teknik güvenlik açıklarının yönetimi

Bu şema, içinde eksik sistem güncelleştirmelerini, işletim sistemi güvenlik açıklarını, SQL güvenlik açıklarını ve sanal makine güvenlik açıklarını izleyen beş [Azure ilke](../../../policy/overview.md) tanımı atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı Öngörüler elde etme olanağı sunan raporlama özellikleri sağlar.

- \[Önizleme\]: Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme
- \[Önizleme\]: Azure Güvenlik Merkezi 'nde eksik sistem güncelleştirmelerini izleme
- \[Önizleme\]: Azure Güvenlik Merkezi 'nde işletim sistemi güvenlik açıklarını izleme
- \[Önizleme\]: Azure Güvenlik Merkezi 'nde SQL güvenlik açığı değerlendirmesi sonuçlarını izleme
- \[Önizleme\]: Azure Güvenlik Merkezi'nde VM Güvenlik Açıklarını İzleme

## <a name="a1262-restrictions-on-software-installation"></a>Yazılım yüklemesinde bir. 12.6.2 kısıtlamaları

Uyarlamalı uygulama denetimi, Azure Güvenlik Merkezi 'nin Azure 'da bulunan sanal makinelerinizde hangi uygulamaların çalıştırılacağını denetlemenize yardımcı olan bir çözümdür. Bu şema, izin verilen uygulamalar kümesinde yapılan değişiklikleri izleyen bir Azure ilke tanımı atar. Yazılım yükleme kısıtlamaları, yazılım güvenlik açıklarına giriş olasılığını azaltmanıza yardımcı olabilir.

- \[Önizleme\]: Azure Güvenlik Merkezi 'nde olası uygulama beyaz listesini izleme

## <a name="a1311-network-controls"></a>A. 13.1.1 ağ denetimleri

Bu şema, ağ güvenlik gruplarını, izin veren kurallarla izleyen bir [Azure ilke](../../../policy/overview.md) tanımı atayarak ağları yönetmenize ve denetlemenize yardımcı olur. Çok izin verilen kurallar istenmeyen ağ erişimine izin verebilir ve incelenmelidir. Bu şema Ayrıca korumasız uç noktaları, uygulamalar ve depolama hesaplarını izleyen üç Azure ilke tanımı da atar. Bir güvenlik duvarı tarafından korunmayan uç noktalar ve uygulamalar ve Kısıtlanmamış erişimi olan depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeden erişime izin verebilir.

- \[Önizleme\]: Azure Güvenlik Merkezi 'nde izin veren ağ erişimini izleme
- \[Önizleme\]: Azure Güvenlik Merkezi 'nde korumasız ağ uç noktalarını izleme
- \[Önizleme\]: Azure Güvenlik Merkezi 'nde korumasız Web uygulamasını izleme
- Depolama hesaplarına kısıtlanmamış ağ erişimini denetleyin

## <a name="a1321-information-transfer-policies-and-procedures"></a>A. 13.2.1 Information transfer ilkeleri ve yordamları

Şema, depolama hesaplarına Güvenli olmayan bağlantıları denetlemek için iki [Azure ilke](../../../policy/overview.md) tanımı atayarak ve Redis Cache Azure hizmetleri ile bilgi aktarımını güvence altına almanıza yardımcı olur.

- Redis Cache önbelleğinizde yalnızca güvenli bağlantıların etkinleştirilmesini denetleyin
- Depolama hesaplarına güvenli aktarımı denetleyin

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 Blueprint öğesinin denetim eşlemesini gözden geçirdiğinize göre, mimari ve bu örneğin nasıl dağıtılacağı hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 şema-genel bakış](./index.md)
> [ISO 27001 şema-Deploy adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.