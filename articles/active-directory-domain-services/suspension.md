---
title: Azure AD Etki Alanı Hizmetlerinde askıya alınan etki alanları | Microsoft Dokümanlar
description: Azure AD DS yönetilen bir etki alanı için farklı sistem durumu durumları ve askıya alınmış bir etki alanını nasıl geri yükleyebilirsiniz hakkında bilgi edinin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 42b26911c12b1e7c62444a6fb2ee68720b02a56b
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654598"
---
# <a name="understand-the-health-states-and-resolve-suspended-domains-in-azure-active-directory-domain-services"></a>Azure Etkin Dizin Etki Alanı Hizmetleri'ndeki sistem durumu durumlarını anlama ve askıya alınan etki alanlarını çözümle

Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) yönetilen bir etki alanına uzun süre hizmet veremediğinde, yönetilen etki alanını askıya alınmış duruma sokar. Yönetilen bir etki alanı askıya alınmış durumda kalırsa, otomatik olarak silinir. Azure AD DS yönetilen etki alanınızın sağlıklı olmasını sağlamak ve askıya alınmayı önlemek için uyarıları olabildiğince hızlı çözün.

Bu makalede, yönetilen etki alanlarının neden askıya alındığını ve askıya alınan bir etki alanının nasıl kurtarılabildiğini açıklar.

## <a name="overview-of-managed-domain-states"></a>Yönetilen etki alanı durumlarına genel bakış

Azure AD DS yönetilen bir etki alanının yaşam döngüsü sayesinde, sistem durumunu gösteren farklı durumlar vardır. Yönetilen etki alanı bir sorun bildiriyorsa, temel nedenin, devletin bozulmaya devam etmesini durdurmak için hızlı bir şekilde çözümlemesini.

![Azure AD DS yönetilen bir etki alanının askıya alınmasına yönelik olarak gerçekleşen durumların ilerlemesi](media/active-directory-domain-services-suspension/suspension-timeline.PNG)

Azure AD DS yönetilen etki alanı aşağıdaki durumlardan birinde olabilir:

* [Çalışıyor](#running-state)
* [İlgiye ihtiyacı var](#needs-attention-state)
* [Askıya alındı](#suspended-state)
* [Silme](#deleted-state)

## <a name="running-state"></a>Çalışan durum

Doğru yapılandırılan ve sorunsuz çalışan Azure AD DS yönetilen etki alanı *Çalışan* durumundadır. Bu, yönetilen bir etki alanı için istenen durumdur.

### <a name="what-to-expect"></a>Ne beklenebilir

* Azure platformu, yönetilen etki alanının durumunu düzenli olarak izleyebilir.
* Yönetilen etki alanı için etki alanı denetleyicileri düzenli olarak yamalı ve güncelleştirilir.
* Azure Etkin Dizin'indeki değişiklikler düzenli olarak yönetilen etki alanına eşitlenir.
* Yönetilen etki alanı için düzenli yedeklemeler alınır.

## <a name="needs-attention-state"></a>İhtiyaçlar Dikkat devlet

Düzeltilmesi gereken bir veya daha fazla soruna sahip Azure AD DS yönetilen bir etki alanı *İhtiyaçlar dikkat durumundadır.* Yönetilen etki alanının sistem durumu sayfası uyarıları listeler ve nerede sorun olduğunu gösterir. Bazı uyarılar geçicidir ve Azure platformu tarafından otomatik olarak çözülür. Diğer uyarılar için, sağlanan çözüm adımlarını izleyerek sorunu çözebilirsiniz. Kritik bir uyarı vardır, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

Uyarının bir örneği, kısıtlayıcı bir ağ güvenlik grubu olmasıdır. Bu yapılandırmada, Azure platformu yönetilen etki alanını güncelleştiremeyebilir ve izleyemeyebilir. Bir uyarı oluşturulur ve durum *dikkat çekmek için*değişir.

Daha fazla bilgi için, [Azure AD DS yönetilen etki alanı için uyarıları nasıl giderebilirsiniz'][resolve-alerts]e bakın.

### <a name="what-to-expect"></a>Ne beklenebilir

Azure AD DS yönetilen bir etki alanı *İhtiyaçlara Dikkat* durumundayken, Azure platformu verileri düzenli olarak izleyemeyecek, yarayamayabilir, güncellemeyebilir veya yedeklenemeyebilir. Bazı durumlarda, geçersiz bir ağ yapılandırması gibi, yönetilen etki alanı için etki alanı denetleyicilerine erişilemez olabilir.

* Yönetilen etki alanı sağlıksız bir durumdadır ve uyarı çözülene kadar devam eden sistem durumu izleme durdurulabilir.
* Yönetilen etki alanı için etki alanı denetleyicileri yamalı veya güncelleştirilemez.
* Azure Etkin Dizin'indeki değişiklikler yönetilen etki alanına eşitlenmeyebilir.
* Yönetilen etki alanı için yedeklemeler alınmayabilir.
* Yönetilen etki alanını etkileyen kritik olmayan uyarıları çözerseniz, sistem durumu *Çalışan* duruma geri dönmelidir.
* Azure platformunun etki alanı denetleyicilerine ulaşamadığı yapılandırma sorunları için kritik uyarılar tetiklenir. Bu kritik uyarılar 15 gün içinde çözülmezse, yönetilen etki alanı *Askıya Alınan* duruma girer.

## <a name="suspended-state"></a>Askıya alınan durum

Azure AD DS yönetilen etki alanı aşağıdaki nedenlerden biriyle **Askıya Alındı** durumuna girer:

* 15 gündür bir veya daha fazla kritik uyarı çözülmedi.
    * Kritik uyarılar, Azure AD DS tarafından gereken kaynaklara erişimi engelleyen bir yanlış yapılandırmadan kaynaklanabilir. Örneğin, [AADDS104 uyarısı: Ağ Hatası][alert-nsg] yönetilen etki alanında 15 günden uzun süredir çözümlenmemiştir.
* Azure aboneliğiyle ilgili bir fatura sorunu var veya Azure aboneliğinin süresi doldu.

Azure platformu etki alanını yönetemediğinde, izleyemiyor, yarayamıyor veya yedeklenemediğinde yönetilen etki alanları askıya alınır. Yönetilen etki alanı Askıya *Alınan* durumda 15 gün kalır. Yönetilen etki alanına erişimi sağlamak için kritik uyarıları hemen çözüme kavuşturun.

### <a name="what-to-expect"></a>Ne beklenebilir

Azure AD DS yönetilen bir etki alanı *Askıya Alındı* durumunda aşağıdaki davranışlar yaşanır:

* Yönetilen etki alanı için etki alanı denetleyicileri, sanal ağ içinde kullanılabilir hale alınır ve erişilemez.
* Etkinse, internet üzerinden yönetilen etki alanına güvenli LDAP erişimi çalışmayı durdurur.
* Yönetilen etki alanına kimlik doğrulamada, etki alanına katılan VM'lerde oturum açmada veya LDAP/LDAPS üzerinden bağlanmada hatalar vardır.
* Yönetilen etki alanının yedekleri artık alınmaz.
* Azure AD ile eşitleme durur.

### <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Yönetilen etki alanınızın askıya alındığını nasıl anlarsınız?

Azure portalında Azure AD DS Health sayfasında etki alanının askıya alındığını belirten bir [uyarı][resolve-alerts] görürsünüz. Etki alanının durumu da *Askıya Alındı*gösterir.

### <a name="restore-a-suspended-domain"></a>Askıya alınan etki alanını geri yükleme

*Askıya Alınan* durumda olan Azure AD DS yönetilen bir etki alanının sistem durumunu geri yüklemek için aşağıdaki adımları tamamlayın:

1. Azure portalında Etki Alanı **hizmetlerini**arayın ve seçin.
1. *aaddscontoso.com*gibi Azure AD DS yönetilen etki alanınızı listeden seçin ve **ardından Sistem Durumu'nu**seçin.
1. Süspansiyon un nedenine bağlı olarak *AADDS503* veya *AADDS504*gibi uyarıyı seçin.
1. Uyarıda sağlanan çözüm bağlantısını seçin ve çözüme yönelik adımları izleyin.

Yönetilen bir etki alanı yalnızca son yedekleme tarihine kadar geri yüklenebilir. Son yedeklemenizin tarihi yönetilen etki alanının **Sistem Durumu** sayfasında görüntülenir. Son yedeklemeden sonra gerçekleşen değişiklikler geri yüklenmez. Yönetilen bir etki alanının yedekleri 30 güne kadar saklanır. 30 günden eski yedeklemeler silinir.

Yönetilen etki alanı *Askıya Alınan* durumdayken uyarıları çözdükten sonra, sağlıklı bir duruma dönmek için bir Azure destek [isteği açın.][azure-support] 30 günden kısa bir yedek varsa, Azure desteği yönetilen etki alanını geri yükleyebilir.

## <a name="deleted-state"></a>Silinen durum

Azure AD DS yönetilen bir etki alanı Askıya *Alınan* durumda 15 gün kalırsa silinir. Bu işlem kurtarılamaz.

### <a name="what-to-expect"></a>Ne beklenebilir

Azure AD DS yönetilen bir etki alanı *Silinmiş* durumuna girdiğinde aşağıdaki davranış lar görülür:

* Yönetilen etki alanının tüm kaynakları ve yedekleri silinir.
* Yönetilen etki alanını geri yükleyemezsiniz ve Azure AD DS'yi yeniden kullanmak için yeni yönetilen etki alanı oluşturmanız gerekir.
* Silindikten sonra yönetilen etki alanı için faturalandırılmezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS yönetilen etki alanınızı sağlıklı tutmak ve askıya alınma riskini en aza indirmek için yönetilen etki alanınız için uyarıları nasıl [çözeceğinizi][resolve-alerts]öğrenin.

<!-- INTERNAL LINKS -->
[alert-nsg]: alert-nsg.md
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[resolve-alerts]: troubleshoot-alerts.md
