---
title: Azure AD 'de Federasyon sertifikalarını yönetme | Microsoft Docs
description: Federasyon sertifikalarınız için sona erme tarihini özelleştirmeyi ve yakında süresi dolacak sertifikaların nasıl yenileneceğini öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: mimart
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: de60dc5095ce4ab4d0219a388c445b08f544e1f9
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77159038"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Azure Active Directory 'da federe çoklu oturum açma için sertifikaları yönetme

Bu makalede, hizmet olarak yazılım (SaaS) uygulamaları için Federasyon çoklu oturum açma (SSO) oluşturmak üzere Azure Active Directory (Azure AD) tarafından oluşturulan sertifikalarla ilgili yaygın soruların ve bilgilerin ele alınmaktadır. Azure AD Uygulama Galerisi 'nden veya Galeri dışı bir uygulama şablonu kullanarak uygulamalar ekleyin. Federasyon SSO seçeneğini kullanarak uygulamayı yapılandırın.

Bu makale yalnızca [Security assertion Markup Language](https://wikipedia.org/wiki/Security_Assertion_Markup_Language) (SAML) Federasyonu aracılığıyla Azure AD SSO 'yu kullanacak şekilde yapılandırılmış uygulamalar için geçerlidir.

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Galeri ve Galeri olmayan uygulamalar için otomatik olarak oluşturulan sertifika

Galeriden yeni bir uygulama eklediğinizde ve SAML tabanlı bir oturum açma yapılandırdığınızda (uygulamaya genel bakış sayfasından **Çoklu oturum açma** > **SAML** ' yi seçerek), Azure AD, uygulama için üç yıl geçerli bir sertifika oluşturur. Etkin sertifikayı bir güvenlik sertifikası ( **. cer**) dosyası olarak indirmek için bu sayfaya (**SAML tabanlı oturum açma**) dönün ve **SAML imzalama sertifikası** başlığından bir indirme bağlantısı seçin. Ham (ikili) sertifika veya Base64 (temel 64-kodlanmış metin) sertifikası arasında seçim yapabilirsiniz. Galeri uygulamaları için bu bölüm, uygulamanın gereksinimine bağlı olarak sertifikayı Federasyon meta verileri XML (bir **. xml** dosyası) olarak indirmek için bir bağlantı da gösterebilir.

![SAML etkin imza sertifikası indirme seçenekleri](./media/manage-certificates-for-federated-single-sign-on/active-certificate-download-options.png)

**SAML imzalama** sertifikası sayfasının **düzenleme** simgesini (bir kurşun kalem) seçerek, SAML **imzalama sertifikası** sayfasını görüntüleyen etkin veya etkin olmayan bir sertifikayı da indirebilirsiniz. İndirmek istediğiniz sertifikanın yanındaki üç nokta ( **...** ) simgesini seçin ve ardından istediğiniz sertifika biçimini seçin. Sertifikayı gizlilik Gelişmiş posta (pek) biçiminde indirmek için ek seçeneğiniz vardır. Bu biçim Base64 ile aynıdır, ancak Windows 'da sertifika biçimi olarak tanınmayan bir **. ped** dosya adı uzantısıyla aynıdır.

![SAML imzalama sertifikası indirme seçenekleri (etkin ve etkin değil)](./media/manage-certificates-for-federated-single-sign-on/all-certificate-download-options.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Federasyon sertifikanızın sona erme tarihini özelleştirin ve yeni bir sertifikaya geri alın

Varsayılan olarak, Azure bir sertifikayı SAML çoklu oturum açma yapılandırması sırasında otomatik olarak oluşturulduğunda üç yıl sonra sona ermek üzere yapılandırır. Kaydettikten sonra bir sertifikanın tarihini değiştiremiyoruz, şunları yapmanız gerekir:

1. İstenen tarihle yeni bir sertifika oluşturun.
1. Yeni sertifikayı kaydedin.
1. Yeni sertifikayı doğru biçimde indirin.
1. Yeni sertifikayı uygulamaya yükleyin.
1. Yeni sertifikayı Azure Active Directory portalında etkin hale getirin.

Aşağıdaki iki bölüm, bu adımları gerçekleştirmenize yardımcı olur.

### <a name="create-a-new-certificate"></a>Yeni bir sertifika oluştur

İlk olarak, farklı bir sona erme tarihi ile yeni sertifika oluşturun ve kaydedin:

1. [Azure Active Directory portalında](https://aad.portal.azure.com/)oturum açın. **Azure Active Directory Yönetim Merkezi** sayfası görüntülenir.
1. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Hesabınızdaki kurumsal uygulamaların bir listesi görüntülenir.
1. Etkilenen uygulamayı seçin. Uygulama için bir genel bakış sayfası görüntülenir.
1. Uygulamaya Genel Bakış sayfasının sol bölmesinde, **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seç** sayfası görünürse, **SAML**' yi seçin.
1. **SAML-Preview Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** başlığını bulun ve **düzenleme** simgesini (bir kurşun kalem) seçin. Her sertifikanın durumunu (**etkin** veya **etkin değil**), sona erme tarihini ve parmak izini (bir karma dize) gösteren **SAML imzalama sertifikası** sayfası görüntülenir.
1. **Yeni sertifika**' yı seçin. Sertifika listesinin altında, sona erme tarihi varsayılan olarak geçerli tarihten üç yıl sonra olacak şekilde yeni bir satır görüntülenir. (Değişiklikleriniz henüz kaydedilmedi, bu nedenle süre sonu tarihini değiştirebilirsiniz.)
1. Yeni sertifika satırında, sona erme tarihi sütununun üzerine gelin ve **Tarih Seç** simgesini (Takvim) seçin. Yeni satırın geçerli sona erme tarihi için bir ayın günlerini görüntüleyen bir Takvim denetimi görünür.
1. Takvim denetimini kullanarak yeni bir tarih ayarlayın. Geçerli tarih ile üç yıl arasında herhangi bir tarihi geçerli tarihten sonra ayarlayabilirsiniz.
1. **Kaydet**’i seçin. Yeni sertifika artık **etkin değil**durumu, seçtiğiniz sona erme tarihi ve bir parmak izi ile birlikte görüntülenir.
1. **SAML-Preview Ile çoklu oturum açmayı ayarlama** sayfasına dönmek için **X** işaretini seçin.

### <a name="upload-and-activate-a-certificate"></a>Sertifika yükleme ve etkinleştirme

Ardından, yeni sertifikayı doğru biçimde indirin, uygulamayı uygulamaya yükleyin ve Azure Active Directory etkin hale getirin:

1. Uygulamanın ek SAML oturum açma yapılandırma yönergelerini şunlardan biriyle görüntüleyin:

   - Ayrı bir tarayıcı penceresinde veya sekmede görüntülemek için **yapılandırma kılavuzu** bağlantısını seçme veya
   - Başlığa gidip bir kenar çubuğunda görüntülemek için **adım adım yönergeleri görüntüle** seçeneğini **belirleyin** .

1. Yönergelerde, sertifika karşıya yüklemesi için gereken kodlama biçimine göz önünde.
1. Daha önce [Galeri ve Galeri olmayan uygulamalar Için otomatik olarak oluşturulan sertifika](#auto-generated-certificate-for-gallery-and-non-gallery-applications) bölümündeki yönergeleri izleyin. Bu adım, sertifikayı uygulama tarafından karşıya yüklemek için gereken kodlama biçiminde indirir.
1. Yeni sertifikayı almak istediğinizde, **SAML Imzalama sertifikası** sayfasına dönün ve yeni kaydedilen sertifika satırında üç nokta ( **...** ) simgesini seçin ve **sertifikayı etkin yap**' ı seçin. Yeni sertifikanın durumu **etkin**olarak değişir ve önceden etkin sertifika, etkin **olmayan**bir duruma geçer.
1. SAML imzalama sertifikasını doğru kodlama biçiminde karşıya yükleyebilmeniz için, daha önce kullandığınız SAML oturum açma yapılandırma yönergelerini izleyerek devam edin.

## <a name="add-email-notification-addresses-for-certificate-expiration"></a>Sertifika süre sonu için e-posta bildirim adresleri ekleme

Azure AD, SAML sertifikasının süresi dolmadan 60, 30 ve 7 gün önce bir e-posta bildirimi gönderir. Bildirimleri almak için birden fazla e-posta adresi ekleyebilirsiniz. Bildirimlerin gönderilmesini istediğiniz e-posta adreslerini belirtmek için:

1. **SAML Imzalama sertifikası** sayfasında, **bildirim e-posta adresleri** başlığına gidin. Varsayılan olarak, bu başlık yalnızca uygulamayı ekleyen yöneticinin e-posta adresini kullanır.
1. Son e-posta adresinin altına, sertifikanın süre sonu bildirimini alması gereken e-posta adresini yazın ve ardından ENTER tuşuna basın.
1. Eklemek istediğiniz her e-posta adresi için önceki adımı tekrarlayın.
1. Silmek istediğiniz her e-posta adresi için, e-posta adresinin yanındaki **Sil** simgesini (çöp kutusu) seçin.
1. **Kaydet**’i seçin.

aadnotification@microsoft.combildirim e-postasını alırsınız. E-postaları istenmeyen posta konumunuza karşı önlemek için bu e-postayı kişilerinize ekleyin.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Yakında sona erecek bir Sertifikayı Yenile

Bir sertifikanın süresi sona ermek üzereyken, kullanıcılarınız için önemli bir kesinti yaşanmasına neden olan bir yordamı kullanarak yenileyebilirsiniz. Süresi dolan bir sertifikayı yenilemek için:

1. Mevcut sertifikayla örtüşen bir tarih kullanarak daha önce [Yeni sertifika oluşturma](#create-a-new-certificate) bölümünde yer alan yönergeleri izleyin. Bu tarih, sertifika süresinin dolmasından kaynaklanan kesinti süresini sınırlar.
1. Uygulama bir sertifikayı otomatik olarak alabilir, aşağıdaki adımları izleyerek yeni sertifikayı etkin olarak ayarlayın:
   1. **SAML Imzalama sertifikası** sayfasına dönün.
   1. Yeni kaydedilen sertifika satırında üç nokta ( **...** ) simgesini seçin ve ardından **sertifikayı etkin yap**' ı seçin.
   1. Sonraki iki adımı atlayın.

1. Uygulama aynı anda yalnızca bir sertifikayı işleyebilir, sonraki adımı gerçekleştirmek için bir kesinti süresi seçin. (Aksi halde, uygulama yeni sertifikayı otomatik olarak içermiyorsa, ancak birden fazla imzalama sertifikasını işleyemezse, bir sonraki adımı dilediğiniz zaman gerçekleştirebilirsiniz.)
1. Eski sertifikanın süresi dolmadan önce, daha önce [sertifikayı yükle ve Etkinleştir](#upload-and-activate-a-certificate) bölümündeki yönergeleri izleyin.
1. Sertifikanın doğru çalıştığından emin olmak için uygulamada oturum açın.

## <a name="related-articles"></a>İlgili makaleler

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](../saas-apps/tutorial-list.md)
- [Azure Active Directory ile uygulama yönetimi](what-is-application-management.md)
- [Azure Active Directory uygulamalarda çoklu oturum açma](what-is-single-sign-on.md)
- [Azure Active Directory uygulamalarda SAML tabanlı çoklu oturum açma hatalarını ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
