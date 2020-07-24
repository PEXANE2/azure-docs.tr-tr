---
title: Eğitim için Azure geliştirme araçları 'nda oturum açma hatasını çözme
description: Bu, eğitim için Azure dev araçları 'nda oturum açarken bir hata mesajı aldıklarında bir öğrencinin gerçekleşmesi gereken eylemleri açıklar.
author: rymend
ms.author: rymend
ms.topic: troubleshooting
ms.service: azure-education
ms.subservice: education-hub
ms.date: 06/30/2020
ms.openlocfilehash: 276ab2cc1d79235c00ce1bb50ff9b648bb1de6ea
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099721"
---
# <a name="troubleshooting-student-login-issues"></a>Öğrenci oturum açma sorunlarını giderme
Eğitim için Azure dev araçlarına erişmek için bir kullanıcının Microsoft hesabına (MSA) sahip olması gerekir. Kullanıcılar, hesabı zaten bir MSA veya bir MSA ile bağlantılı değilse bir MSA oluşturmak için otomatik olarak yönlendirilir. Etki alanınız Active Directory ilişkili ise, bu etki alanındaki tüm hesaplar zaten bir MSA olarak kabul edilir.

Bir öğrenci oturum açmaya çalışır ve aşağıdaki hata iletisini alırsa, aşağıda açıklanan çözümlerden birini kullanın.

:::image type="content" source="media/troubleshoot-login/error-login-message.png" alt-text="Oturum açma hata iletisi." border="false":::

İki çözüm vardır: yeni bir Microsoft hesabı oluşturun veya var olan bir Microsoft hesabı kullanın.

## <a name="create-a-new-microsoft-account"></a>Yeni bir Microsoft hesabı oluştur
### <a name="use-a-university-email-address"></a>Bir üniversite e-posta adresi kullanın
Bir üniversite e-posta adresiyle oturum açıyorsanız (örneğin, `John.Smith@university.edu` ), bu e-posta adresiyle bir Microsoft hesabı oluşturmanız gerekir. Hesap oluşturma ücretsizdir ve yalnızca birkaç dakika sürer. Microsoft hesabı olması, tüm Microsoft ürünlerinize otomatik olarak tek bir Kullanıcı adı ve parola ile oturum açabilsin.

### <a name="use-a-personal-email-address"></a>Kişisel e-posta adresi kullan
Kişisel bir e-posta adresi (örneğin,) ile oturum açıyorsanız, `John.Smith@email.com` ancak aynı zamanda bir üniversite e-posta adresi varsa, University e-posta adresini kullanmayı deneyin. Daha önce kurumunuzun web deposunda kişisel bir e-posta adresi ile oturum açtıysanız veya bir üniversite e-posta adresi yoksa, kişisel e-posta adresinizle bir Microsoft hesabı oluşturmanız veya bağlamanız gerekir.

## <a name="use-an-existing-microsoft-account"></a>Mevcut bir Microsoft hesabı kullan
Bir öğrenciye mevcut bir Microsoft hesabı (örneğin, Xbox) varsa, bu hesap, bu hesabı bir Azure dev araçları hesabına bağlayabilirler.

1. https://account.microsoft.com öğesine gidin.
1. Microsoft hesabı kimlik bilgilerinizle oturum açın.
1. Üst şerit menüsünden **bilgilerinizi** seçin.

1. **Microsoft 'ta oturum açma şeklini Yönet ' e**tıklayın. Kimliğinizi doğrulamanız istenir. Bir güvenlik kodu e-postayla gönderilir.

    :::image type="content" source="media/troubleshoot-accounts/manage-sign-in.png" alt-text="Oturum açma yönetimi." border="false":::

1. E-postayla gönderilen güvenlik kodunu girin.

    :::image type="content" source="media/troubleshoot-accounts/sign-in-enter-code.png" alt-text="Oturum açma kodunu girin." border="false":::!

1. Hesabınıza **e-posta Ekle** ' ye tıklayın ve üniversite e-posta adresinizi girin.
Bir sonraki oturum açışınızda, öğretme için Azure geliştirme araçlarına erişmek üzere üniversite e-posta adresinizi kullanabilirsiniz.

    :::image type="content" source="media/troubleshoot-accounts/manage-sign-in-add-email.png" alt-text="Microsoft 'ta oturum açma şeklini yönetin." border="false":::

## <a name="next-steps"></a>Sonraki adımlar
- [SSS](program-faq.md)

- [Destek seçenekleri](program-support.md)
