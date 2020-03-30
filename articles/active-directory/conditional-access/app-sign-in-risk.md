---
title: Azure Active Directory Koşullu Erişim ile risk algılamalarını kullanma
description: Bu hızlı başlangıçta, oturum risklerine göre oturum açma işlemlerini engellemek için bir Azure Etkin Dizin (Azure AD) Koşullu Erişim ilkesini nasıl yapılandırabileceğinizi öğrenirsiniz.
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
ms.openlocfilehash: be447b001b0b2e14999aac98ba2125f8cbfe9853
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77186604"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Hızlı başlatma: Azure Etkin Dizin Koşullu Erişim ile oturum riski algılandığında erişimi engelleyin  

Ortamınızı korumak için şüpheli kullanıcıların oturum açmasını engellemek isteyebilirsiniz. [Azure Etkin Dizin (Azure AD) Kimlik Koruması,](../active-directory-identityprotection.md) her oturum açma işleminin çözümlemesi ve oturum açma girişiminin bir kullanıcı hesabının yasal sahibi tarafından gerçekleştirilme olasılığını hesaplar. Olasılık (düşük, orta, yüksek) oturum açma risk [düzeyleri](concept-conditional-access-conditions.md#sign-in-risk)olarak adlandırılan hesaplanmış bir değer şeklinde gösterilir. Oturum açma risk durumunu ayarlayarak, belirli oturum açma risk düzeylerine yanıt verecek koşullu erişim ilkesini yapılandırabilirsiniz.

Bu hızlı başlatma, yapılandırılmış oturum açma risk düzeyi algılandığında oturum açma'yı engelleyen [Koşullu Erişim ilkesinin](../active-directory-conditional-access-azure-portal.md) nasıl yapılandırılabildiğini gösterir.

![İlke oluşturma](./media/app-sign-in-risk/1000.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

- **Azure AD Premium P2 sürümüne erişim** - Koşullu Erişim bir Azure AD Premium P1 özelliği olsa da, bu hızlı başlatmasenaryosu kimlik koruması gerektirdiğinden bir P2 sürümüne ihtiyacınız vardır.
- **Kimlik Koruması** - Bu hızlı başlatmadaki senaryo, Kimlik Korumasının etkinleştirilmesini gerektirir. Kimlik Korumayı nasıl etkinleştirdiğinizi bilmiyorsanız, [bkz.](../identity-protection/overview-identity-protection.md)
- **Tor Browser** - [Tor Browser,](https://www.torproject.org/projects/torbrowser.html.en) çevrimiçi gizliliğinizi korumanıza yardımcı olmak için tasarlanmıştır. Kimlik Koruması, bir Tor Browser'dan oturum açma oturum açma olarak orta risk düzeyine sahip anonim IP adreslerinden algılar. Daha fazla bilgi için Azure [Etkin Dizin risk algılamalarına](../reports-monitoring/concept-risk-events.md)bakın.  
- **Alain Charon adlı** bir test hesabı - Test hesabı oluşturmayı bilmiyorsanız, [bulut tabanlı kullanıcı ekle'ye](../fundamentals/add-users-azure-active-directory.md#add-a-new-user)bakın.

## <a name="test-your-sign-in"></a>Oturum açmanızı test edin

Bu adımın amacı, test hesabınızın Tor Browser'ı kullanarak kiracınıza erişebilmesini sağlamaktır.

**Oturum açmanızı test etmek için:**

1. [Azure portalınızda](https://portal.azure.com) **Alain Charon**olarak oturum açın.
1. Oturumunuzu kapatın.

## <a name="create-your-conditional-access-policy"></a>Koşullu Erişim ilkenizi oluşturun

Bu hızlı başlatmadaki senaryo, **anonim IP adreslerinden** algılanan bir Oturum Açma özelliği oluşturmak için Bir Tor Browser'dan bir oturum açma özelliğini kullanır. Bu risk tespiti risk düzeyi orta. Bu risk algılamaya yanıt vermek için oturum açma risk durumunu ortama ayarlarsınız. Üretim ortamında, oturum açma risk durumunu yüksek veya orta ve yüksek olarak ayarlamalısınız.

Bu bölümde, gerekli Koşullu Erişim ilkesinin nasıl oluşturulacak olduğu gösterilmektedir. İlkenizde şunları ayarlayın:

| Ayar | Değer |
| --- | --- |
| Kullanıcılar ve gruplar | Alain Charon  |
| Bulut uygulamaları | Tüm bulut uygulamaları |
| Oturum açma riski | Orta |
| İzin verme | Hizmete erişimi |

![İlke oluşturma](./media/app-sign-in-risk/130.png)

**Koşullu Erişim ilkenizi yapılandırmak için:**

1. [Azure portalınızda](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. Azure portalında, sol daki gezinme çubuğunda **Azure Etkin Dizini'ni**tıklatın.

   ![Azure Active Directory](./media/app-sign-in-risk/02.png)

1. Azure **Etkin Dizin** sayfasında, **Güvenlik** bölümünde **Koşullu Erişim'i**tıklatın.

   ![Koşullu Erişim](./media/app-sign-in-risk/03.png)

1. **Koşullu Erişim** sayfasında, üstteki araç çubuğunda **Ekle'yi**tıklatın.

   ![Adı](./media/app-sign-in-risk/108.png)

1. **Yeni** sayfada, **Ad** metin kutusunda, **orta risk düzeyi için Erişimi Engelle**yazın.

   ![Adı](./media/app-sign-in-risk/104.png)

1. **Atama** bölümünde, **Kullanıcılar ve gruplar'ı**tıklatın.

   ![Kullanıcılar ve gruplar](./media/app-sign-in-risk/06.png)

1. Kullanıcılar **ve gruplar** sayfasında:

   ![Koşullu Erişim](./media/app-sign-in-risk/107.png)

   1. **Kullanıcıları ve grupları seçin'i**tıklatın ve ardından Kullanıcıları ve **grupları**seçin.
   1. **Seç'i**tıklatın.
   1. **Seç** sayfasında **Alain Charon'u**seçin ve sonra **Seç'i**tıklatın.
   1. Kullanıcılar **ve gruplar** **sayfasında, Bitti'yi**tıklatın.
1. **Bulut uygulamalarını**tıklatın.

   ![Bulut uygulamaları](./media/app-sign-in-risk/08.png)

1. Bulut **uygulamaları** sayfasında:

   ![Koşullu Erişim](./media/app-sign-in-risk/109.png)

   1. **Tüm bulut uygulamalarını**tıklatın.
   1. **Bitti**’ye tıklayın.
1. **Koşullar'ı**tıklatın.

   ![Erişim denetimleri](./media/app-sign-in-risk/19.png)

1. **Koşullar** sayfasında:

   ![Oturum açma riski düzeyi](./media/app-sign-in-risk/21.png)

   1. **Oturum Açma riskini**tıklatın.
   1. **Yapılandırıldıkça** **Evet'i**tıklatın.
   1. Oturum açma risk düzeyi olarak **Orta'yı**seçin.
   1. **Seç'i**tıklatın.
   1. **Koşullar** **sayfasında, Bitti'yi**tıklatın.
1. Access **denetimleri** **bölümünde, Ver'i**tıklatın.

   ![Erişim denetimleri](./media/app-sign-in-risk/10.png)

1. **Hibe** sayfasında:

   ![Koşullu Erişim](./media/app-sign-in-risk/105.png)

   1. **Erişimi Engelle'yi**seçin.
   1. **Seç'i**tıklatın.
1. Etkinleştir **ilkesi** **bölümünde, '''yi**tıklatın.

   ![İlkeyi etkinleştirme](./media/app-sign-in-risk/18.png)

1. **Oluştur'u**tıklatın.

## <a name="evaluate-a-simulated-sign-in"></a>Benzetimli oturum açma yı değerlendirme

Koşullu Erişim ilkenizi yapılandırdığınıza göre, büyük olasılıkla beklendiği gibi çalışıp çalışmadığını bilmek istersiniz. İlk adım olarak, test kullanıcınızın oturum açma simüle etmek için **ilke aracı ne olur** Koşullu Erişim kullanın. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.  

Bu senaryo için **"if" ilke aracını** çalıştırdığınızda, **orta risk düzeyi için Engelle erişimi** uygulanacak **Ilkeler**altında listelenmelidir.

![Kullanıcı](./media/app-sign-in-risk/117.png)

**Koşullu Erişim ilkenizi değerlendirmek için:**

1. [Koşullu Erişim - İlkeler](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **Ne Varsa'yı**tıklatın.  

   ![What If](./media/app-sign-in-risk/14.png)

1. **Kullanıcı'yı**tıklatın, **Kullanıcılar** sayfasında **Alan Charon'u** seçin ve sonra **Seç'i**tıklatın.

   ![Kullanıcı](./media/app-sign-in-risk/116.png)

1. **Oturum açma riski**olarak, **Orta'yı**seçin.

   ![Kullanıcı](./media/app-sign-in-risk/119.png)

1. **What'ı**tıklatın.

## <a name="test-your-conditional-access-policy"></a>Koşullu Erişim ilkenizi test edin

Önceki bölümde, benzetimli oturum açma nasıl değerlendirileceklerini öğrendiniz. Simülasyona ek olarak, beklendiği gibi çalıştığından emin olmak için Koşullu Erişim ilkenizi de test etmeniz gerekir.

İlkinizi test etmek için, Tor Browser'ı kullanarak [Azure portalınızda](https://portal.azure.com) **Alan Charon** olarak oturum açmayı deneyin. Oturum açma girişiminiz Koşullu Erişim ilkeniz tarafından engellenmelidir.

![Multi-factor authentication](./media/app-sign-in-risk/118.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, test kullanıcısını, Tor Browser'ı ve Koşullu Erişim ilkesini silin:

- Bir Azure REKLAM kullanıcısını nasıl sildiğinizi bilmiyorsanız, [bkz.](../fundamentals/add-users-azure-active-directory.md#delete-a-user)
- İlkenizi silmek için ilkenizi seçin ve ardından hızlı erişim araç çubuğunda **Sil'i** tıklatın.

   ![Multi-factor authentication](./media/app-sign-in-risk/33.png)

- Tor Browser'ı kaldırmak için [Uninstalling](https://tb-manual.torproject.org/uninstalling/)bkz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanım koşullarının kabul](require-tou.md)
> edilmesini zorunlu kılması[belirli uygulamalar için MFA Gerektirir](app-based-mfa.md)
