---
title: Azure Active Directory portalındaki riskli oturum açma işlemleri raporu | Microsoft Docs
description: Azure Active Directory portalındaki riskli oturum açma işlemleri raporu hakkında bilgi edinin
services: active-directory
author: cawrites
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 629e7bf8ee7100b5927483ecfd4efa1d9223c151
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989914"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki riskli oturum açma işlemleri raporu

Azure Active Directory (Azure AD), kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için, **risk olayı** adlı bir kayıt oluşturulur. Daha ayrıntılı bilgi için bkz. [Azure AD risk olayları](concept-risk-events.md). 

[Azure portal](https://portal.azure.com) **Azure Active Directory** dikey penceresini seçip **güvenlik** bölümüne giderek güvenlik raporlarına erişebilirsiniz. 

Risk olaylarına göre hesaplanan iki farklı güvenlik raporu vardır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/10.png)

Bu risk olaylarını tetikleyen ilkelerin nasıl yapılandırılacağını öğrenmek için bkz. [Kullanıcı risk ilkesini yapılandırma](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Riskli oturum açma işlemleri raporuna kimler erişebilir?

Riskli oturum açma işlemleri raporları aşağıdaki rollerdeki kullanıcılar için kullanılabilir:

- Güvenlik Yöneticisi
- Genel Yönetici
- Güvenlik Okuyucusu

Azure Active Directory ' de bir kullanıcıya yönetim rolleri atamayı öğrenmek için, bkz. [Azure Active Directory yönetici rollerini görüntüleme ve atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Tüm Azure AD sürümlerinde riskli oturum açma işlemleri raporları sağlanır. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- **Azure Active Directory ücretsiz ve temel sürümlerde**riskli oturum açma işlemlerinin bir listesini alırsınız. 

- Ayrıca, **Azure Active Directory Premium 1** sürümü, her rapor için algılanan temeldeki bazı risk olaylarını incelemenizi sağlar. 

- **Azure Active Directory Premium 2** sürümü, tüm temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.

## <a name="risky-sign-ins-report-for-azure-ad-free-and-basic-edition"></a>Azure AD ücretsiz ve temel sürüm için riskli oturum açma işlemleri raporu

Azure AD ücretsiz ve temel sürümleri, kullanıcılarınız için algılanan riskli oturum açma işlemlerinin bir listesini sağlar. Her kayıt aşağıdaki öznitelikleri içerir:

- **Kullanıcı** -oturum açma işlemi sırasında kullanılan kullanıcının adı.
- **IP** -Azure Active Directory bağlanmak için kullanılan cihazın IP adresi.
- **Konum** -Azure Active Directory bağlanmak için kullanılan konum. Bu, izlemelere, kayıt defteri verilerine, ters ara pencerelere ve diğer bilgilere göre en iyi çabadır.
- **Oturum açma saati** - Oturum açma işleminin gerçekleştirildiği saat
- **Durum** - Oturum açma durumu

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/01.png)

Riskli oturum açma araştırmanıza bağlı olarak, aşağıdaki eylemleri gerçekleştirerek Azure AD 'ye geri bildirim sağlayabilirsiniz:

- Çözümle
- Yanlış pozitif olarak işaretleme
- Yoksayma
- Yeniden etkinleştirme

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/21.png)

Bu rapor ayrıca şunları yapmak için bir seçenek sunar:

- Kaynak ara
- Rapor verilerini indir

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Azure AD Premium sürümleri için riskli oturum açma işlemleri raporu

Azure AD Premium sürümlerindeki riskli oturum açma işlemleri raporu şunları sağlar:

- Algılanan [risk olayı türleri](concept-risk-events.md) hakkında toplu bilgiler. **Azure AD Premium P1 sürümü**sayesinde, lisansınız kapsamında olmayan algılamalar, risk olayı **algılanan ek risklerle birlikte**görüntülenir. **Azure AD Premium P2 sürümüyle**, temeldeki Tüm algılamalar hakkında en ayrıntılı bilgileri alırsınız.

- Raporu indirme seçeneği

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/456.png)

Bir risk olayını seçtiğinizde bu risk olayına ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Bir [kullanıcı riskini azaltma ilkesi](../identity-protection/howto-user-risk-policy.md) yapılandırabilirsiniz.  

- Risk olayının algılanma zaman çizelgesini gözden geçirebilirsiniz.  

- Bu risk olayının hangi kullanıcılarla ilgili olarak algılandığının listesini gözden geçirebilirsiniz.

- Risk olaylarını el ile kapatın. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> Bazen, [oturum açma raporuna](concept-sign-ins.md)karşılık gelen bir oturum açma girişi olmadan bir risk olayı bulabilirsiniz. Bunun nedeni, kimlik korumasının hem **etkileşimli** hem de **etkileşimli olmayan** oturum açma işlemlerinin riskini değerlendirmesinde, oturum açma raporunda yalnızca etkileşimli oturum açma işlemleri gösterilir.

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcıya ilişkin bildirilmiş risk olaylarını araştırabilirsiniz. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/324.png)

Bir risk olayını araştırmak için, söz konusu olayı listeden seçin.  
Bu risk olayına ilişkin **Ayrıntılar** dikey penceresi açılır. **Ayrıntılar** dikey penceresinde, risk olayını elle kapatma ve elle kapatılmış risk olayını yeniden etkinleştirme seçenekleri sunulur. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı risk ilkesini yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Risk düzeltme ilkesini yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Risk olayı türleri](concept-risk-events.md)
