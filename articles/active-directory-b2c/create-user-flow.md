---
title: Kullanıcı akışı oluşturma-Azure Active Directory B2C
description: Azure Active Directory B2C ' de uygulamalarınız için kaydolma, oturum açma ve Kullanıcı profili düzenlemesini etkinleştirmek üzere Azure portal Kullanıcı akışları oluşturmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9bd436b972dfb1549232831b1f07c3726ff459dd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100556500"
---
# <a name="create-a-user-flow-in-azure-active-directory-b2c"></a>Azure Active Directory B2C bir Kullanıcı akışı oluşturma

Azure Active Directory B2C (Azure AD B2C) kiracınızda farklı türlerde [Kullanıcı akışları](user-flow-overview.md) oluşturabilir ve bunları gerektiği şekilde uygulamalarınızda kullanabilirsiniz. Kullanıcı akışları, uygulamalar arasında yeniden kullanılabilir.

> [!IMPORTANT]
> Kullanıcı akışı sürümlerine başvuru şeklimizi değiştirdik. Daha önce V1 (üretime hazır) sürümleriyle V1.1 ve V2 (önizleme) sürümlerini sunduk. Artık Kullanıcı akışlarını **Önerilen** (yeni nesil Önizleme) ve **Standart** (genel kullanıma açık) sürümler halinde birleştiriyoruz. Tüm V 1.1 ve v2 eski Preview Kullanıcı akışları **, 1 ağustos 2021 '** e kadar kullanımdan kalkmaya yönelik bir yoldur. Ayrıntılar için bkz. [Azure AD B2C Içindeki Kullanıcı akışı sürümleri](user-flow-versions.md).

## <a name="before-you-begin"></a>Başlamadan önce

- Yeni Kullanıcı akışını sınamak için kullanmak istediğiniz **uygulamayı kaydedin** . Bir örnek için bkz. [öğretici: bir Web uygulamasını Azure AD B2C kaydetme](tutorial-register-applications.md).
- Azure AD, Amazon, Facebook, GitHub, LinkedIn, Microsoft veya Twitter gibi sağlayıcılardan Kullanıcı oturum açmasını etkinleştirmek istiyorsanız **dış kimlik sağlayıcıları ekleyin** . Bkz. [Azure AD B2C uygulamalarınıza kimlik sağlayıcıları ekleme](add-identity-provider.md).
- Kiracınızdaki yerel hesaplar için desteklemek istediğiniz kimlik türlerini (e-posta, Kullanıcı adı, telefon numarası) belirtmek üzere **yerel hesap kimlik sağlayıcısını yapılandırın** . Ardından, bireysel kullanıcı akışları oluştururken desteklenen bu kimlik türleri arasından seçim yapabilirsiniz. Bir Kullanıcı Kullanıcı akışını tamamladığında, Azure AD B2C dizininizde yerel bir hesap oluşturulur ve **yerel hesap** kimlik sağlayıcınız Kullanıcı bilgilerinin kimliğini doğrular. Kiracının yerel hesap kimliği sağlayıcınızı şu adımlarla yapılandırın:

   1. [Azure portalında](https://portal.azure.com/) oturum açın. 
   2. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
   3. Azure portal üst kısmındaki arama çubuğunda **Azure AD B2C**' i arayıp seçin.
   4. **Yönet** altında **kimlik sağlayıcıları**' nı seçin.
   5. Kimlik sağlayıcısı listesinde **yerel hesap**' ı seçin.
   6. **Yerel IDP 'Yi yapılandırma** sayfasında, desteklemek istediğiniz tüm kimlik türlerini seçin. Burada seçeneklerin seçilmesi yalnızca daha sonra oluşturduğunuz Kullanıcı akışları için kullanılabilir hale getirir:
      - **Telefon** (Önizleme): kullanıcının, kayıt sırasında doğrulanan ve Kullanıcı kimliği olduğu bir telefon numarası girmesine izin verir.
      - **E-posta** (varsayılan): kullanıcının, kayıt sırasında doğrulanmış bir e-posta adresi girmesini sağlar ve Kullanıcı kimliği olur.
      - Kullanıcı **adı**: bir kullanıcının kendi BENZERSIZ Kullanıcı kimliğini oluşturmasına izin verir. Kullanıcıdan bir e-posta adresi toplanır ve doğrulanır.
    7. **Kaydet**’i seçin.

## <a name="create-a-user-flow"></a>Kullanıcı akışı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

    ![B2C kiracısı, dizin ve abonelik bölmesi, Azure portal](./media/create-user-flow/directory-subscription-pane.png)

3. Azure portal, araması yapın ve **Azure AD B2C** seçin.
4. **İlkeler** altında **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.

    ![Yeni Kullanıcı akışı düğmesi vurgulanmış şekilde portalda Kullanıcı akışları sayfası](./media/create-user-flow/signup-signin-user-flow.png)

5. **Kullanıcı akışı oluştur** sayfasında, oluşturmak istediğiniz kullanıcı akışı türünü seçin (bkz. bir genel bakış Için [Azure AD B2C Kullanıcı akışları](user-flow-overview.md) ).

    ![Kaydolma ve oturum açma akışı vurgulanmış bir Kullanıcı akış sayfası seçin](./media/create-user-flow/select-user-flow-type.png)

6. **Sürüm seçin** altında **Önerilen**' i seçin ve ardından **Oluştur**' u seçin. (Kullanıcı akışı sürümleri hakkında[daha fazla bilgi edinin](user-flow-versions.md) .)

    ![Azure portal ' de vurgulanan özelliklerle Kullanıcı akışı oluşturma sayfası](./media/create-user-flow/select-version.png)

7. Kullanıcı akışı için bir **ad** girin (örneğin, *signupsignin1*, *profileediting1*, *passwordreset1*).
8. **Kimlik sağlayıcıları** altında oluşturmakta olduğunuz Kullanıcı akışının türüne bağlı olarak seçenekleri belirleyin:

   - **Yerel hesap**. Kullanıcıların Azure AD B2C kiracınızda yerel hesaplar oluşturmalarına izin vermek istiyorsanız, kullanmak istediğiniz tanımlayıcı türünü (örneğin, e-posta, Kullanıcı KIMLIĞI veya telefon) seçin. Yalnızca [yerel hesap kimlik sağlayıcısı](#before-you-begin) ayarlarınızda yapılandırılan kimlik türleri listelenir.

   - **Sosyal kimlik sağlayıcıları**. Azure AD, Amazon, Facebook, GitHub, LinkedIn, Microsoft veya Twitter gibi [eklediğiniz sosyal kimlik sağlayıcılarıyla](add-identity-provider.md)Kullanıcı oturum açma izni vermek istiyorsanız, listeden sağlayıcıları seçin.

9. **Kullanıcı öznitelikleri ve talepler** için, kayıt sırasında kullanıcıdan toplamak ve göndermek istediğiniz talepleri ve öznitelikleri seçin. **Daha fazla göster**' i seçin. Öznitelikler ve talepler ' i seçin ve ardından **Tamam**' ı seçin.

    ![Üç talep seçili olan öznitelikler ve talepler seçim sayfası](./media/create-user-flow/signup-signin-attributes.png)

10. Kullanıcı akışını eklemek için **Oluştur** ' u seçin. *B2C_1* bir ön eki otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. **İlkeler**  >  **Kullanıcı akışları**' nı seçin ve ardından oluşturduğunuz kullanıcı akışını seçin. Kullanıcı akışına Genel Bakış sayfasında, **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama** için adım 1 ' de kaydettiğiniz Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır**' ı seçin.
2. Test ettiğiniz Kullanıcı akışının türüne bağlı olarak, geçerli bir e-posta adresi kullanarak kaydolun ve kaydolma akışını izleyin ya da daha önce oluşturduğunuz bir hesabı kullanarak oturum açın.

    ![Kullanıcı akışını Çalıştır düğmesi vurgulanmış şekilde portalda Kullanıcı akış sayfasını Çalıştır](./media/create-user-flow/sign-up-sign-in-run-now.png)

1. Kullanıcı akışı istemlerini izleyin. Kullanıcı akışını tamamladığınızda belirteç öğesine döner `https://jwt.ms` ve size gösterilmesi gerekir.

> [!NOTE]
> "Kullanıcı akışını Çalıştır" deneyimi şu anda yetkilendirme kodu akışını kullanan SPA yanıt URL 'SI türüyle uyumlu değil. "Kullanıcı akışını Çalıştır" deneyimini bu tür uygulamalarla kullanmak için, "Web" türünde bir yanıt URL 'SI kaydedin ve [burada](tutorial-register-spa.md)açıklandığı gibi örtük akışı etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C Kullanıcı akışlarına koşullu erişim ekleme](conditional-access-user-flow.md)
- [Azure AD B2C Kullanıcı akışında Kullanıcı arabirimini özelleştirme](customize-ui-with-html.md)
