---
title: Koşullu Erişim kullanım koşulları gerektirir - Azure Etkin Dizini
description: Bu hızlı başlangıçta, belirli bulut uygulamalarına erişim Azure Active Active Directory Koşullu Erişim tarafından verilmeden önce kullanım koşullarınızın kabul edilmesini nasıl talep edebileceğinizi öğrenirsiniz.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3dd1b4cf554e773f49a15ac5cedcbcc5b3e710b9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74380095"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Hızlı başlatma: Bulut uygulamalarına erişmeden önce kullanım koşullarının kabul edilmesini zorunlu kılmasını zorunlu kılması

Ortamınızdaki belirli bulut uygulamalarına erişmeden önce, kullanım koşullarınızı (ToU) kabul etme şeklinde kullanıcılardan onay almak isteyebilirsiniz. Azure Etkin Dizin (Azure AD) Koşullu Erişim size şunları sağlar:

- ToU yapılandırmak için basit bir yöntem
- Koşullu Erişim ilkesi yle kullanım koşullarınızı kabul etmeyi talep etme seçeneği  

Bu hızlı başlangıç, ortamınızda seçili bir bulut uygulaması için bir ToU'nun kabul edilmesi gereken bir [Azure AD Koşullu Erişim ilkesini](../active-directory-conditional-access-azure-portal.md) nasıl yapılandırıştırılabildiğini gösterir.

![İlke oluşturma](./media/require-tou/5555.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta senaryoyu tamamlamak için şunları yapmanız gerekir:

- **Azure AD Premium sürümüne erişim** - Azure AD Koşullu Erişim bir Azure AD Premium özelliğidir.
- **Isabella Simonsen adlı** bir test hesabı - Bir test hesabı oluşturmayı bilmiyorsanız, [bulut tabanlı kullanıcı ekle'ye](../fundamentals/add-users-azure-active-directory.md#add-a-new-user)bakın.

## <a name="test-your-sign-in"></a>Oturum açmanızı test edin

Bu adımın amacı, Koşullu Erişim ilkesi olmadan oturum açma deneyimi hakkında bir izlenim elde etmektir.

**Oturum açmanızı test etmek için:**

1. [Azure portalınızda](https://portal.azure.com/) Isabella Simonsen olarak oturum açın.
1. Oturumunuzu kapatın.

## <a name="create-your-terms-of-use"></a>Kullanım koşullarınızı oluşturun

Bu bölümde, örnek Bir ToU oluşturmak için adımlar sağlar. Bir ToU oluşturduğunuzda, **Koşullu Erişim ilkesi şablonları ile Uygula**için bir değer seçersiniz. Özel **ilke** yi seçmek, ToU'nuz oluşturulur oluşturulmaz yeni bir Koşullu Erişim ilkesi oluşturmak için iletişim kutusunu açar.

**Kullanım koşullarınızı oluşturmak için:**

1. Microsoft Word'de yeni bir belge oluşturun.
1. **Kullanım koşullarımı**yazın ve belgeyi **mytou.pdf**olarak bilgisayarınıza kaydedin.
1. [Azure portalınızda](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. Azure portalında, sol daki gezinme çubuğunda **Azure Etkin Dizini'ni**tıklatın.

   ![Azure Active Directory](./media/require-tou/02.png)

1. Azure **Etkin Dizin** sayfasında, **Güvenlik** bölümünde **Koşullu Erişim'i**tıklatın.

   ![Koşullu Erişim](./media/require-tou/03.png)

1. **Yönet** bölümünde kullanım **koşulları'nı**tıklatın.

   ![Kullanım koşulları](./media/require-tou/04.png)

1. Üstteki menüde Yeni **terimler'i**tıklatın.

   ![Kullanım koşulları](./media/require-tou/05.png)

1. Yeni **kullanım koşulları** sayfasında:

   ![Kullanım koşulları](./media/require-tou/112.png)

   1. **Ad** metin kutusuna, **Tou'm**yazın.
   1. Görüntü **adı** metin kutusuna, **Tou'm**yazın.
   1. Kullanım koşullarınızı PDF dosyanıza yükleyin.
   1. **Dil**olarak , **İngilizce**seçin.
   1. **Kullanıcıların kullanım koşullarını genişletmelerini gerektirdiğinde,** **Aç'ı**seçin.
   1. **Koşullu Erişim ilkesi şablonları ile Uygula'da** **Özel ilke**yi seçin.
   1. **Oluştur'u**tıklatın.

## <a name="create-your-conditional-access-policy"></a>Koşullu Erişim ilkenizi oluşturun

Bu bölümde, gerekli Koşullu Erişim ilkesinin nasıl oluşturulacak olduğu gösterilmektedir. Bu quickstart'taki senaryo da kullanır:

- Azure portalı, ToU'nuzun kabul edilmesi gereken bir bulut uygulamasının yer tutucusu olarak sunulur. 
- Koşullu Erişim ilkesini sınamak için örnek kullanıcınız.  

İlkenizde şunları ayarlayın:

| Ayar | Değer |
| --- | --- |
| Kullanıcılar ve gruplar | Isabella Simonsen |
| Bulut uygulamaları | Microsoft Azure Yönetimi |
| Erişim verme | Benim TOU |

![İlke oluşturma](./media/require-tou/1234.png)

**Koşullu Erişim ilkenizi yapılandırmak için:**

1. **Yeni** sayfada, **Ad** metin kutusunda, **Isabella için TOU'yu gerektir**yazın.

   ![Adı](./media/require-tou/71.png)

1. **Atama** bölümünde, **Kullanıcılar ve gruplar'ı**tıklatın.

   ![Kullanıcılar ve gruplar](./media/require-tou/06.png)

1. Kullanıcılar **ve gruplar** sayfasında:

   ![Kullanıcılar ve gruplar](./media/require-tou/24.png)

   1. **Kullanıcıları ve grupları seçin'i**tıklatın ve ardından Kullanıcıları ve **grupları**seçin.
   1. **Seç'i**tıklatın.
   1. **Seç** sayfasında Isabella **Simonsen'i**seçin ve sonra **Seç'i**tıklatın.
   1. Kullanıcılar **ve gruplar** **sayfasında, Bitti'yi**tıklatın.
1. **Bulut uygulamalarını**tıklatın.

   ![Bulut uygulamaları](./media/require-tou/08.png)

1. Bulut **uygulamaları** sayfasında:

   ![Bulut uygulamalarını seçin](./media/require-tou/26.png)

   1. **Uygulamaları Seç'i**tıklatın.
   1. **Seç'i**tıklatın.
   1. **Seç** sayfasında Microsoft **Azure Yönetimi'ni**seçin ve sonra **Seç'i**tıklatın.
   1. Bulut **uygulamaları** **sayfasında, Bitti'yi**tıklatın.
1. Access **denetimleri** **bölümünde, Ver'i**tıklatın.

   ![Erişim denetimleri](./media/require-tou/10.png)

1. **Hibe** sayfasında:

   ![İzin verme](./media/require-tou/111.png)

   1. **Erişime Ver'i**seçin.
   1. **Benim TOU**seçin.
   1. **Seç'i**tıklatın.
1. Etkinleştir **ilkesi** **bölümünde, '''yi**tıklatın.

   ![İlkeyi etkinleştirme](./media/require-tou/18.png)

1. **Oluştur'u**tıklatın.

## <a name="evaluate-a-simulated-sign-in"></a>Benzetimli oturum açma yı değerlendirme

Koşullu Erişim ilkenizi yapılandırdığınıza göre, büyük olasılıkla beklendiği gibi çalışıp çalışmadığını bilmek istersiniz. İlk adım olarak, test kullanıcınızın oturum açma simüle etmek için ilke aracı ne olur Koşullu Erişim kullanın. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.  

**What If** ilke değerlendirme aracını başlatmayı ayarlayın:

- **Isabella Simonsen** kullanıcı olarak
- Bulut uygulaması olarak **Microsoft Azure Yönetimi**

**What'ı** tıklatma, şunları gösteren bir simülasyon raporu oluşturur:

- **Uygulanacak Politikalar** Kapsamında Isabella **için TOU'ya ihtiyaç**
- **Hibe Kontrolleri**olarak **Benim TOU** .

![Ya ilke aracı](./media/require-tou/79.png)

**Koşullu Erişim ilkenizi değerlendirmek için:**

1. [Koşullu Erişim - İlkeler](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **Ne Varsa'yı**tıklatın.  

   ![What If](./media/require-tou/14.png)

1. **Kullanıcılar'ı**tıklatın, **Isabella Simonsen'i**seçin ve sonra **Seç'i**tıklatın.

   ![Kullanıcı](./media/require-tou/15.png)

1. Bir bulut uygulaması seçmek için:

   ![Bulut uygulamaları](./media/require-tou/16.png)

   1. **Bulut uygulamalarını**tıklatın.
   1. Bulut **uygulamaları sayfasında,** **Uygulamaları Seç'i**tıklatın.
   1. **Seç'i**tıklatın.
   1. **Seç** sayfasında Microsoft **Azure Yönetimi'ni**seçin ve sonra **Seç'i**tıklatın.
   1. Bulut uygulamaları **sayfasında, Bitti'yi**tıklatın.
1. **What'ı**tıklatın.

## <a name="test-your-conditional-access-policy"></a>Koşullu Erişim ilkenizi test edin

Önceki bölümde, benzetimli oturum açma nasıl değerlendirileceklerini öğrendiniz. Simülasyona ek olarak, beklendiği gibi çalıştığından emin olmak için Koşullu Erişim ilkenizi de test etmeniz gerekir.

İlkinizi test etmek **için, Isabella Simonsen** test hesabınızı kullanarak [Azure portalınızda](https://portal.azure.com) oturum açmayı deneyin. Kullanım koşullarınızı kabul etmenizi gerektiren bir iletişim kutusu görmeniz gerekir.

![Kullanım koşulları](./media/require-tou/57.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, test kullanıcısını ve Koşullu Erişim ilkesini silin:

- Bir Azure REKLAM kullanıcısını nasıl sildiğinizi bilmiyorsanız, [bkz.](../fundamentals/add-users-azure-active-directory.md#delete-a-user)
- İlkenizi silmek için ilkenizi seçin ve ardından hızlı erişim araç çubuğunda **Sil'i** tıklatın.

    ![Multi-factor authentication](./media/require-tou/33.png)

- Kullanım koşullarınızı silmek için seçin ve ardından üstteki araç çubuğundaki **terimleri sil'i** tıklatın.

    ![Multi-factor authentication](./media/require-tou/29.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirli uygulamalar](app-based-mfa.md)
> için MFA'yı gerektiren[Oturum riski algılandığında erişimi engelleyin](app-sign-in-risk.md)
