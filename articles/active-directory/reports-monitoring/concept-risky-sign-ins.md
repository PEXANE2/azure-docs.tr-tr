---
title: Portalda riskli oturum açma raporu | Microsoft Dokümanlar
description: Azure Active Directory portalındaki riskli oturum açma işlemleri raporu hakkında bilgi edinin
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b77486064139895799ac5a48327377154f75da6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273832"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki riskli oturum açma işlemleri raporu

Azure Etkin Dizin (Azure AD), kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılar. Algılanan her eylem için **risk algılama** adı verilen bir kayıt oluşturulur. Daha fazla bilgi için Azure [AD risk algılamalarına](concept-risk-events.md)bakın. 

**Azure Active Directory** bıçağını seçip **Güvenlik** bölümüne giderek [Güvenlik portalından](https://portal.azure.com) güvenlik raporlarına erişebilirsiniz. 

Risk algılamalarına göre hesaplanan iki farklı güvenlik raporu vardır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/10.png)

Bu risk algılamalarını tetikleyen ilkeleri nasıl yapılandırılabildiğini öğrenmek [için, kullanıcı risk ilkesini nasıl yapılandırılatırınız.](../identity-protection/howto-user-risk-policy.md)  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Riskli oturum açma raporuna kimler erişebilir?

Riskli oturum açma raporları aşağıdaki rollerde kullanıcılar tarafından kullanılabilir:

- Güvenlik Yöneticisi
- Genel Yönetici
- Güvenlik Okuyucu

Azure Etkin Dizin'inde bir kullanıcıya yönetim rolleri nasıl ataysüreceğinizi öğrenmek için Azure [Etkin Dizin'de Görüntüle ve Yönetici rollerini atayabakın.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal)

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Azure AD'nin tüm sürümleri, riskli oturum açma raporları sağlar. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- Azure **Active Directory Free sürümünde**riskli oturum açma ların bir listesini alırsınız. 

- Ayrıca, **Azure Active Directory Premium 1** sürümü, her rapor için algılanan temel risk algılamalarından bazılarını incelemenize olanak tanır. 

- **Azure Active Directory Premium 2** sürümü, temel risk algılamaları hakkında en ayrıntılı bilgileri sağlar ve ayrıca yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkelerini yapılandırmanızı sağlar.

## <a name="risky-sign-ins-report-for-azure-ad-free-edition"></a>Azure AD ücretsiz sürümü için riskli oturum açma raporu

Azure AD ücretsiz sürümü, kullanıcılarınız için algılanan riskli oturum açmaların bir listesini sağlar. Her kayıt aşağıdaki öznitelikleri içerir:

- **Kullanıcı** - Oturum açma işlemi sırasında kullanılan kullanıcının adı.
- **IP** - Azure Active Directory'ye bağlanmak için kullanılan aygıtın IP adresi.
- **Konum** - Azure Etkin Dizini'ne bağlanmak için kullanılan konum. Bu, izlemelere, kayıt defteri verilerine, ters arama up'larına ve diğer bilgilere dayalı en iyi çaba yaklaşıklamasıdır.
- **Oturum açma saati** - Oturum açma işleminin gerçekleştirildiği saat
- **Durum** - Oturum açma durumu

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/01.png)

Riskli oturum açma ile ilgili araştırmanıza bağlı olarak, aşağıdaki eylemleri yaparak Azure AD'ye geri bildirim sağlayabilirsiniz:

- Çözümleme
- Yanlış pozitif olarak işaretleme
- Yoksayma
- Yeniden etkinleştirme

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/21.png)

Bu rapor ayrıca şunları yapmak için bir seçenek sunar:

- Kaynak arama
- Rapor verilerini indir

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Azure AD premium sürümleri için riskli oturum açma raporu

Azure AD premium sürümlerindeki riskli oturum açma raporu size şunları sağlar:

- Algılanan [risk algılama türleri](concept-risk-events.md) hakkında toplu bilgiler. Azure **AD Premium P1 sürümünde,** lisansınızın kapsamında olmayan algılamaları **ek risk algılanan**risk algılama oturum açma olarak görünür. Azure **AD Premium P2 sürümü**yle, temel deki tüm algılamalar hakkında en ayrıntılı bilgilere sahip olursunuz.

- Raporu indirme seçeneği

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/456.png)

Bir risk algılama seçtiğinizde, aşağıdakileri yapmanızı sağlayan bu risk tespiti için ayrıntılı bir rapor görünümü elde edersiniz:

- [Kullanıcı riskini azaltma ilkesi](../identity-protection/howto-user-risk-policy.md) yapılandırma seçeneği  

- Risk tespiti için algılama zaman çizelgesini gözden geçirin  

- Bu risk tespitinin algılandığı kullanıcıların listesini gözden geçirin

- Risk algılamaları el ile kapatın. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> Bazen, [oturum açma raporunda](concept-sign-ins.md)karşılık gelen bir oturum açma girişi olmadan bir risk algılama bulabilirsiniz. Bunun nedeni, Kimlik Korumasının hem **etkileşimli** hem de **etkileşimli olmayan** oturum açma riskini değerlendirirken, oturum açma raporunda yalnızca etkileşimli oturum açma ları gösterir.

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcı için bildirilen risk algılamalarını araştırın. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/324.png)

Risk tespitini araştırmak için listeden birini seçin.  
Bu, bu risk tespiti için **Ayrıntılar** bıçak açılır. **Ayrıntılar** bıçağında, bir risk algılamayı el ile kapatma veya el ile kapatılan bir risk algılamayı yeniden etkinleştirme seçeneğiniz bulunmaktadır. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı risk ilkesi nasıl yapılandırılır?](../identity-protection/howto-user-risk-policy.md)
- [Risk düzeltme ilkesi nasıl yapılandırılır?](../identity-protection/howto-user-risk-policy.md)
- [Risk algılama türleri](concept-risk-events.md)
