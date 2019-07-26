---
title: Azure Active Directory Kimlik Koruması 'de etkin risk olaylarını kapatma | Microsoft Docs
description: Etkin risk olaylarınızın kapalı olduğu seçenekler hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5e24c12b72852ee7009533c8dc24d231fe636f2
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333990"
---
# <a name="how-to-close-active-risk-events"></a>Nasıl Yapılır: Etkin risk olaylarını kapatma

[Risk olaylarıyla](../reports-monitoring/concept-risk-events.md)Azure Active Directory, riskli olabilecek Kullanıcı hesaplarının göstergelerini algılar. Yönetici olarak, etkilenen kullanıcıların artık risk altında olmaması için tüm risk olaylarını kapalı olarak almak istersiniz.

Bu makale, etkin risk olaylarını kapatmak için sahip olduğunuz ek seçeneklere genel bir bakış sağlar.

## <a name="options-to-close-risk-events"></a>Risk olaylarını kapatma seçenekleri 

Risk olayının durumu **etkin** ya da **kapalı**olur. Tüm etkin risk olayları, Kullanıcı risk düzeyi adlı bir değer hesaplamasına katkıda bulunur. Kullanıcı risk düzeyi bir hesap güvenliğinin tehlikeye girdiği olasılığa karşı bir göstergedir (düşük, orta, yüksek). 

Etkin risk olaylarını kapatmak için aşağıdaki seçeneklere sahipsiniz:

- Kullanıcı riski ilkesiyle parola sıfırlama gerektir
- El ile parola sıfırlama
- Tüm risk olaylarını kapat 
- Bireysel risk olaylarını el ile kapatın

## <a name="require-password-reset-with-a-user-risk-policy"></a>Kullanıcı riski ilkesiyle parola sıfırlama gerektir

[Kullanıcı riski koşullu erişim ilkesini](howto-user-risk-policy.md)yapılandırarak, belirtilen Kullanıcı risk düzeyi otomatik olarak algılanırsa parola değişikliği yapmanız gerekebilir. 

![Parola sıfırla](./media/howto-close-active-risk-events/13.png)

Parola sıfırlama ilgili kullanıcının tüm etkin risk olaylarını kapatır ve kimliği güvenli bir duruma geri getirir. Bu yöntem otomatikleştirildiğinde, etkin risk olaylarını kapatmak için tercih edilen yöntem bir Kullanıcı risk ilkesini kullanmaktır. Etkilenen Kullanıcı ve yardım masası ya da yönetici arasında herhangi bir etkileşime gerek yoktur.

Ancak, bir Kullanıcı risk İlkesi kullanmak her zaman uygulanabilir değildir. Bu, örneğin, şunları yapmak için geçerlidir:

- Multi-Factor Authentication (MFA) için kaydedilmemiş kullanıcılar.
- Silinen etkin risk olayları olan kullanıcılar.
- Yasal Kullanıcı tarafından bildirilen bir risk olayının gerçekleştirildiğini ortaya çıkaran bir araştırma.

## <a name="manual-password-reset"></a>El ile parola sıfırlama

Bir Kullanıcı risk ilkesi kullanarak parola sıfırlamayı gerektirmek bir seçenek değilse, bir kullanıcı için tüm risk olaylarını el ile parola sıfırlama ile kapatabilirsiniz.

![Parola sıfırla](./media/howto-close-active-risk-events/04.png)

İlgili iletişim kutusu, parolayı sıfırlamak için iki farklı yöntem sağlar:

![Parola sıfırla](./media/howto-close-active-risk-events/05.png)

**Geçici parola oluşturma** -geçici bir parola oluşturarak, bir kimliği hemen güvenli bir duruma getirebilirsiniz. Bu yöntem, geçici parolanın ne olduğunu bilmeleri gerektiğinden etkilenen kullanıcılarla etkileşim kurmayı gerektirir. Örneğin, yeni geçici parolayı Kullanıcı veya kullanıcının Yöneticisi için alternatif bir e-posta adresine gönderebilirsiniz. Parola geçici olduğundan, kullanıcıdan bir sonraki oturum açma işlemi sırasında parolayı değiştirmesi istenir.

**Kullanıcının parolayı sıfırlamasını gerektir** -kullanıcıların parola sıfırlamasını sağlamak, yardım masası veya yönetici ile iletişim kurmadan kendinden kurtarmaya olanak sağlar. Kullanıcı risk ilkesi söz konusu olduğunda bu yöntem yalnızca MFA için kayıtlı olan kullanıcılar için geçerlidir. MFA için henüz kaydedilmemiş kullanıcılar için bu seçenek kullanılamaz.

## <a name="dismiss-all-risk-events"></a>Tüm risk olaylarını kapat

Parola sıfırlama sizin için bir seçenek değilse, tüm risk olaylarını da kapatabilirsiniz. 

![Parola sıfırla](./media/howto-close-active-risk-events/03.png)

**Tüm olayları kapat**' a tıkladığınızda, tüm olaylar kapatılır ve etkilenen Kullanıcı artık risk altında olmaz. Ancak, bu yöntemin var olan parola üzerinde bir etkisi olmadığından, ilgili kimliği güvenli bir duruma getirmez. Bu yöntem için tercih edilen kullanım örneği, etkin risk olayları olan silinmiş bir kullanıcı. 

## <a name="close-individual-risk-events-manually"></a>Bireysel risk olaylarını el ile kapatın

Bireysel risk olaylarını el ile kapatabilirsiniz. Risk olaylarını el ile kapatarak, Kullanıcı risk düzeyini düşürebilirsiniz. Genellikle, risk olayları ilgili bir araştırmaya yanıt olarak el ile kapatılır. Örneğin, bir kullanıcıya konuştuğu zaman etkin bir risk olayının artık gerekli olmadığı ortaya çıkarır. 
 
Risk olaylarını el ile kapatırken, risk olayının durumunu değiştirmek için aşağıdaki eylemlerden herhangi birini yapabilirsiniz:

![Eylemler](./media/howto-close-active-risk-events/06.png)

- **Çözümle** -bir risk olayını araştırdıktan sonra kimlik koruması dışında uygun bir düzeltme eylemi gerçekleştiyse ve risk olayının kapalı kabul edilmesi gerektiğini düşünüyorsanız, olayı çözümlenmiş olarak işaretleyin. Çözümlenen olaylar risk olayının durumunu kapalı olarak ayarlar ve risk olayı artık Kullanıcı riskine katkıda bulunmaz.
- **Yanlış-pozitif olarak işaretle** -bazı durumlarda bir risk olayını araştırabilir ve yanlış bir riskli olarak işaretlenip işaretlenmediğini keşfedebilirsiniz. Risk olayını yanlış pozitif olarak işaretleyerek bu tür oluşumların sayısını azaltmaya yardımcı olabilirsiniz. Bu, daha sonra benzer olayların sınıflandırmasını geliştirmek için makine öğrenimi algoritmalarına yardımcı olur. False-pozitif olayların durumu kapanacaktır ve artık Kullanıcı riskine katkıda bulunmaz.
- **Yoksay** -herhangi bir düzeltme eylemi gerçekleştirme, ancak risk olayının etkin listeden kaldırılmasını istiyorsanız, bir risk olayı yok saymayı işaretleyebilirsiniz ve olay durumu kapatılacak. Yoksayılan olaylar Kullanıcı riskine katkıda bulunmuyor. Bu seçenek yalnızca Olağandışı koşullarda kullanılmalıdır.
- **Yeniden etkinleştirme** -el ile kapatılan risk olayları (çöz, yanlış pozitif veya Yoksay seçilerek) yeniden etkinleştirilebilir ve olay durumu tekrar etkin olarak ayarlanabilir. Yeniden etkinleştirilen risk olayları, Kullanıcı risk düzeyi hesaplamasına katkıda bulunur. Düzeltme aracılığıyla kapatılan risk olayları (güvenli parola sıfırlama gibi) yeniden etkinleştirilemez.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview.md).
