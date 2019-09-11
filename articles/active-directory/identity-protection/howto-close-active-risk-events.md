---
title: Azure Active Directory Kimlik Koruması 'de etkin risk algılamalarını kapatma | Microsoft Docs
description: Etkin risk algılamalarını kapatan seçenekler hakkında bilgi edinin.
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
ms.openlocfilehash: d24ec94d0381fc2e79fdef97b6d525b7cec33fef
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126466"
---
# <a name="how-to-close-active-risk-detections"></a>Nasıl Yapılır: Etkin risk algılamalarını kapat

Risk algılamalarından Azure Active Directory, [riskli](../reports-monitoring/concept-risk-events.md)olabilecek Kullanıcı hesaplarının göstergelerini algılar. Yönetici olarak, etkilenen kullanıcıların artık risk altında olmaması için tüm risk algılamalarını kapalı olarak almak istersiniz.

Bu makale, etkin risk algılamalarını kapatmanız için sahip olduğunuz ek seçeneklere genel bir bakış sunar.

## <a name="options-to-close-risk-detections"></a>Risk algılamalarını kapatma seçenekleri 

Risk algılamanın durumu **etkin** ya da **kapalı**olur. Tüm etkin risk algılamaları, Kullanıcı risk düzeyi adlı bir değerin hesaplamasına katkıda bulunur. Kullanıcı risk düzeyi bir hesap güvenliğinin tehlikeye girdiği olasılığa karşı bir göstergedir (düşük, orta, yüksek). 

Etkin risk algılamalarını kapatmak için aşağıdaki seçeneklere sahipsiniz:

- Kullanıcı riski ilkesiyle parola sıfırlama gerektir
- El ile parola sıfırlama
- Tüm risk algılamalarını kapat 
- Bireysel risk algılamalarını el ile kapatın

## <a name="require-password-reset-with-a-user-risk-policy"></a>Kullanıcı riski ilkesiyle parola sıfırlama gerektir

[Kullanıcı riski koşullu erişim ilkesini](howto-user-risk-policy.md)yapılandırarak, belirtilen Kullanıcı risk düzeyi otomatik olarak algılanırsa parola değişikliği yapmanız gerekebilir. 

![Parola sıfırla](./media/howto-close-active-risk-events/13.png)

Parola sıfırlama ilgili kullanıcının tüm etkin risk olaylarını kapatır ve kimliği güvenli bir duruma geri getirir. Bu yöntem otomatikleştirildiğinde, etkin risk algılamalarını kapatmak için tercih edilen yöntem bir Kullanıcı risk ilkesi kullanmaktır. Etkilenen Kullanıcı ve yardım masası ya da yönetici arasında herhangi bir etkileşime gerek yoktur.

Ancak, bir Kullanıcı risk İlkesi kullanmak her zaman uygulanabilir değildir. Bu, örneğin, şunları yapmak için geçerlidir:

- Multi-Factor Authentication (MFA) için kaydedilmemiş kullanıcılar.
- Etkin risk algılamaları olan kullanıcılar silinmiş.
- Yasal Kullanıcı tarafından bildirilen bir risk algılama işleminin gerçekleştirildiğini ortaya çıkaran bir araştırma.

## <a name="manual-password-reset"></a>El ile parola sıfırlama

Bir Kullanıcı risk ilkesi kullanarak parola sıfırlamayı gerektirmek bir seçenek değilse, bir kullanıcı için el ile parola sıfırlama ile kapatılan tüm risk algılamalarını alabilirsiniz.

![Parola sıfırla](./media/howto-close-active-risk-events/04.png)

İlgili iletişim kutusu, parolayı sıfırlamak için iki farklı yöntem sağlar:

![Parola sıfırla](./media/howto-close-active-risk-events/05.png)

**Geçici parola oluşturma** -geçici bir parola oluşturarak, bir kimliği hemen güvenli bir duruma getirebilirsiniz. Bu yöntem, geçici parolanın ne olduğunu bilmeleri gerektiğinden etkilenen kullanıcılarla etkileşim kurmayı gerektirir. Örneğin, yeni geçici parolayı Kullanıcı veya kullanıcının Yöneticisi için alternatif bir e-posta adresine gönderebilirsiniz. Parola geçici olduğundan, kullanıcıdan bir sonraki oturum açma işlemi sırasında parolayı değiştirmesi istenir.

**Kullanıcının parolayı sıfırlamasını gerektir** -kullanıcıların parola sıfırlamasını sağlamak, yardım masası veya yönetici ile iletişim kurmadan kendinden kurtarmaya olanak sağlar. Kullanıcı risk ilkesi söz konusu olduğunda bu yöntem yalnızca MFA için kayıtlı olan kullanıcılar için geçerlidir. MFA için henüz kaydedilmemiş kullanıcılar için bu seçenek kullanılamaz.

## <a name="dismiss-all-risk-detections"></a>Tüm risk algılamalarını kapat

Parola sıfırlama sizin için bir seçenek değilse, tüm risk algılamalarını da kapatabilirsiniz. 

![Parola sıfırla](./media/howto-close-active-risk-events/03.png)

**Tüm olayları kapat**' a tıkladığınızda, tüm olaylar kapatılır ve etkilenen Kullanıcı artık risk altında olmaz. Ancak, bu yöntemin var olan parola üzerinde bir etkisi olmadığından, ilgili kimliği güvenli bir duruma getirmez. Bu yöntem için tercih edilen kullanım örneği, etkin risk algılamaları olan silinmiş bir kullanıcı. 

## <a name="close-individual-risk-detections-manually"></a>Bireysel risk algılamalarını el ile kapatın

Bireysel risk algılamalarını el ile kapatabilirsiniz. Risk algılamalarını el ile kapatarak, Kullanıcı risk düzeyini düşürebilirsiniz. Genellikle, risk algılamaları ilgili bir araştırmaya yanıt olarak el ile kapatılır. Örneğin, bir kullanıcıya konuşduğunda etkin bir risk algılamada artık gerekli değildir. 
 
Risk algılamalarını el ile kapatırken, risk algılamanın durumunu değiştirmek için aşağıdaki eylemlerden birini seçebilirsiniz:

![Eylemler](./media/howto-close-active-risk-events/06.png)

- **Çözümle** -risk algılamayı araştırdıktan sonra kimlik koruması dışında uygun bir düzeltme eylemi gerçekleştirmenize ve risk algılamanın kapalı kabul edilmesi gerektiğini düşündüğü takdirde olayı çözüldü olarak işaretleyin. Çözümlenen olaylar risk algılama durumunu kapalı olarak ayarlar ve risk algılama artık Kullanıcı riskine katkıda bulunmaz.
- **Yanlış-pozitif olarak işaretle** -bazı durumlarda risk algılamayı araştırabilir ve yanlış bir riskli olarak işaretlenip işaretlenmediğini keşfedebilirsiniz. Risk algılamayı yanlış pozitif olarak işaretleyerek bu tür oluşumların sayısını azaltmaya yardımcı olabilirsiniz. Bu, daha sonra benzer olayların sınıflandırmasını geliştirmek için makine öğrenimi algoritmalarına yardımcı olur. False-pozitif olayların durumu kapanacaktır ve artık Kullanıcı riskine katkıda bulunmaz.
- **Yoksay** -herhangi bir düzeltme eylemi gerçekleştirme, ancak risk algılamanın etkin listeden kaldırılmasını istiyorsanız, bir risk algılama yok saymayı işaretleyebilirsiniz ve olay durumu kapatılacak olur. Yoksayılan olaylar Kullanıcı riskine katkıda bulunmuyor. Bu seçenek yalnızca Olağandışı koşullarda kullanılmalıdır.
- **Yeniden etkinleştirme** -el ile kapatılan risk algılamaları (çözümle, yanlış pozitif veya Yoksay seçilerek) yeniden etkinleştirilebilir ve olay durumu tekrar etkin olarak ayarlanabilir. Yeniden etkinleştirilen risk algılamaları, Kullanıcı risk düzeyi hesaplamasına katkıda bulunur. Düzeltme (güvenli parola sıfırlama gibi) ile kapatılan risk algılamaları yeniden etkinleştirilemez.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview.md).
