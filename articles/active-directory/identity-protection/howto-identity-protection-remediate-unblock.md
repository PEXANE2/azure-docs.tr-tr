---
title: Azure AD Kimlik Koruması'nda riskleri düzeltin ve kullanıcıların engelini kaldırın
description: Yakın etkin risk algılamaseçenekleriniz hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 350e7b37d36be70cea345db52cdfb639b2f1c1a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74382110"
---
# <a name="remediate-risks-and-unblock-users"></a>Riskleri düzeltin ve kullanıcıların engelini kaldırın

[Araştırmanızı](howto-identity-protection-investigate-risk.md)tamamladıktan sonra, riski düzeltmek veya kullanıcıların engelini kaldırmak için harekete geçmek isteyeceksiniz. Kuruluşlar ayrıca [risk ilkelerini](howto-identity-protection-configure-risk-policies.md)kullanarak otomatik düzeltmeyi etkinleştirme seçeneğine de sahiptir. Kuruluşlar, kuruluşunuzun rahat olduğu bir zaman diliminde sunulan tüm risk tespitlerini kapatmaya çalışmalıdır. Microsoft, riskle çalışırken zaman önemli olduğundan olayları mümkün olan en kısa sürede kapatmanızı önerir.

## <a name="remediation"></a>Düzeltme

Tüm etkin risk algılamaları, kullanıcı risk düzeyi adı verilen bir değerin hesaplanmasına katkıda bulunur. Kullanıcı risk düzeyi, bir hesabın tehlikeye girme olasılığı için bir göstergedir (düşük, orta, yüksek). Yönetici olarak, etkilenen kullanıcıların artık risk altında olmaması için tüm risk algılamalarının kapatılmasını istersiniz.

Bazı risk algılamaları Kimlik Koruması tarafından "Kapalı (sistem)" olarak işaretlenebilir, çünkü olayların artık riskli olduğu belirlenmemiştir.

Yöneticiler, düzeltmek için aşağıdaki seçeneklere sahiptir:

- Risk politikası ile kendi kendini düzeltme
- Manuel parola sıfırlama
- Kullanıcı riskini yok say
- Bireysel risk algılamalarını el ile kapatın

### <a name="self-remediation-with-risk-policy"></a>Risk politikası ile kendi kendini düzeltme

Risk ilkelerinizde Azure Çok Faktörlü Kimlik Doğrulama (MFA) ve self servis parola sıfırlama (SSPR) ile kullanıcıların kendi kendini düzeltmesine izin verirseniz, risk algılandığında kendilerinin engelini kaldırabilirler. Bu algılamalar daha sonra kapalı olarak kabul edilir. Risk tespit edildiğinde kullanmak için kullanıcıların Azure MFA ve SSPR'ye daha önce kaydolmuş olması gerekir.

Bazı algılamalar, kullanıcının kendi kendini düzeltmesi gereken düzeye risk getirmeyebilir, ancak yöneticilerin bu algılamaları yine de değerlendirmesi gerekir. Yöneticiler, [konumlardan erişimi engellemek](../conditional-access/howto-conditional-access-policy-location.md) veya ilkelerindeki kabul edilebilir riski azaltmak gibi ek önlemlerin gerekli olduğunu belirleyebilir.

### <a name="manual-password-reset"></a>Manuel parola sıfırlama

Kullanıcı risk ilkesi ni kullanarak parola sıfırlama gerektiren bir seçenek değilse, yöneticiler el ile parola sıfırlama ile bir kullanıcı için tüm risk algılamaları kapatabilirsiniz.

Yöneticilere kullanıcıları için parolayı sıfırlarken iki seçenek verilir:

- **Geçici bir parola oluşturma** - Geçici bir parola oluşturarak, bir kimliği nizi hemen güvenli bir duruma geri getirebilirsiniz. Bu yöntem, geçici parolanın ne olduğunu bilmeleri gerektiğinden etkilenen kullanıcılarla iletişim kurmayı gerektirir. Parola geçici olduğundan, kullanıcıdan bir sonraki oturum açma sırasında parolayı yeni bir şeyle değiştirmesi istenir.

- **Kullanıcının parolayı sıfırlamasını zorunlu** kılma - Kullanıcıların parolaları sıfırlamasını zorunlu kılmak, yardım masasına veya yöneticiye başvurmadan kendi kendine kurtarma sağlar. Bu yöntem yalnızca Azure MFA ve SSPR için kayıtlı kullanıcılar için geçerlidir. Kaydedilmemiş kullanıcılar için bu seçenek kullanılamıyor.

### <a name="dismiss-user-risk"></a>Kullanıcı riskini yok say

Parola sıfırlama sizin için bir seçenek değilse, örneğin kullanıcı silindiğinden, kullanıcı risk algılamalarını reddetmeyi seçebilirsiniz.

**Kullanıcı riskini Kapat'ı**tıklattığınızda, tüm olaylar kapatılır ve etkilenen kullanıcı artık risk altında değildir. Ancak, bu yöntemin varolan parola üzerinde bir etkisi olmadığından, ilgili kimliği güvenli bir duruma geri getirmez. 

### <a name="close-individual-risk-detections-manually"></a>Bireysel risk algılamalarını el ile kapatın

Tek tek risk algılamalarını el ile kapatabilirsiniz. Risk algılamalarını el ile kapatarak, kullanıcı risk düzeyini düşürebilirsiniz. Genellikle, risk algılamaları ilgili bir araştırmaya yanıt olarak el ile kapatılır. Örneğin, bir kullanıcıyla konuşurken, etkin bir risk algılama nın artık gerekli olmadığını ortaya koymaktadır. 
 
Risk algılamalarını el ile kapatırken, risk algılama durumunu değiştirmek için aşağıdaki eylemlerden herhangi birini almayı seçebilirsiniz:

- Kullanıcının gizliliğini zedeledim
- Kullanıcı riskini yok say
- Oturum açma güvenlionay
- Oturum açma nın tehlikeye atAsını onaylama

## <a name="unblocking-users"></a>Kullanıcıların engelini kaldırma

Yönetici, risk ilkesine veya araştırmalarına göre oturum açma'yı engellemeyi seçebilir. Oturum açma veya kullanıcı riskine bağlı olarak bir blok oluşabilir.

### <a name="unblocking-based-on-user-risk"></a>Kullanıcı riskine göre engellemeyi kaldırma

Kullanıcı riski nedeniyle engellenen bir hesabın engelini kaldırmak için yöneticilerin aşağıdaki seçeneklere sahip olması gerekir:

1. **Parolayı sıfırla** - Kullanıcının parolasını sıfırlayabilirsiniz.
1. **Kullanıcı riskini kapatma** - Erişimi engellemek için yapılandırılan kullanıcı risk düzeyine ulaşıldıysa, kullanıcı risk ilkesi kullanıcıyı engeller. Kullanıcı riskini reddederek veya bildirilen risk algılamalarını el ile kapatarak kullanıcının risk düzeyini azaltabilirsiniz.
1. **Kullanıcıyı ilkeden hariç tut** - Oturum açma ilkenizin geçerli yapılandırmasının belirli kullanıcılar için sorunlara neden olduğunu düşünüyorsanız, kullanıcıları bu politikadan hariç tutabilirsiniz. Daha fazla bilgi için, makaledeki Dışlamalar bölümüne bakın [Nasıl Yapılandırılır: Risk ilkelerini yapılandırın ve etkinleştirin.](howto-identity-protection-configure-risk-policies.md#exclusions)
1. **Devreyi devre dışı-** İlke yapılandırmanızın tüm kullanıcılarınız için sorunlara neden olduğunu düşünüyorsanız, ilkeyi devre dışı kullanabilirsiniz. Daha fazla bilgi için, nasıl yapılandırılabilen makaleye [bakın: Risk ilkelerini yapılandırın ve etkinleştirin.](howto-identity-protection-configure-risk-policies.md)

### <a name="unblocking-based-on-sign-in-risk"></a>Oturum açma riskine göre engellemeyi kaldırma

Oturum açma riskine dayalı bir hesabın engelini kaldırmak için yöneticilerin aşağıdaki seçeneklere sahip olması gerekir:

1. **Tanıdık bir konumdan veya cihazdan oturum açın** - Engellenen şüpheli oturum açmaların yaygın bir nedeni, yabancı konumlardan veya aygıtlardan gelen oturum açma girişimleridir. Kullanıcılarınız, tanıdık bir konumdan veya cihazdan oturum açmaya çalışarak bu nedenle engelleme nedeni olup olmadığını hızlı bir şekilde belirleyebilir.
1. **Kullanıcıyı ilkeden hariç tut** - Oturum açma ilkenizin geçerli yapılandırmasının belirli kullanıcılar için sorunlara neden olduğunu düşünüyorsanız, kullanıcıları bu politikadan hariç tutabilirsiniz. Daha fazla bilgi için, makaledeki Dışlamalar bölümüne bakın [Nasıl Yapılandırılır: Risk ilkelerini yapılandırın ve etkinleştirin.](howto-identity-protection-configure-risk-policies.md#exclusions)
1. **Devreyi devre dışı-** İlke yapılandırmanızın tüm kullanıcılarınız için sorunlara neden olduğunu düşünüyorsanız, ilkeyi devre dışı kullanabilirsiniz. Daha fazla bilgi için, nasıl yapılandırılabilen makaleye [bakın: Risk ilkelerini yapılandırın ve etkinleştirin.](howto-identity-protection-configure-risk-policies.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması hakkında genel bir bakış elde etmek için [Azure AD Kimlik Koruması'na genel bakış](overview-identity-protection.md)bölümüne bakın.
