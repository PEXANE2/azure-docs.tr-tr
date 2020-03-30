---
title: Azure AD'de federasyon sertifikalarını yönetme | Microsoft Dokümanlar
description: Federasyon sertifikalarınızın son kullanma tarihini nasıl özelleştireceğinizi ve yakında süresi dolacak sertifikaları nasıl yenileyeceğinizi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159038"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Azure Active Directory'de federe tek oturum açma sertifikalarını yönetme

Bu makalede, Azure Active Directory'nin (Azure AD) yazılımınıza hizmet (SaaS) uygulamaları olarak federalerated tek oturum açma (SSO) oluşturmak için oluşturduğu sertifikalarla ilgili sık sorulan soruları ve bilgileri ele alıyoruz. Azure AD uygulama galerisinden veya galeri dışı bir uygulama şablonu kullanarak uygulama ekleyin. Federated SSO seçeneğini kullanarak uygulamayı yapılandırın.

Bu makale, yalnızca [Güvenlik İddiası Biçimlendirme Dili](https://wikipedia.org/wiki/Security_Assertion_Markup_Language) (SAML) federasyonu aracılığıyla Azure AD SSO'su kullanacak şekilde yapılandırılan uygulamalarla alakalıdır.

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Galeri ve galeri dışı uygulamalar için otomatik oluşturulan sertifika

Galeriden yeni bir uygulama eklediğinizde ve SAML tabanlı oturum açıldığında (uygulamaya genel bakış sayfasından **Tek oturum açma** > **SAML'i** seçerek), Azure AD uygulama için üç yıl boyunca geçerli olan bir sertifika oluşturur. Etkin sertifikayı güvenlik sertifikası (**.cer**) dosyası olarak indirmek için, bu sayfaya **(SAML tabanlı oturum açma)** dönün ve **SAML İmzaSertifikası** başlığında bir indirme bağlantısı seçin. Ham (ikili) sertifika veya Base64 (base 64 kodlanmış metin) sertifikası arasında seçim yapabilirsiniz. Galeri uygulamaları için bu bölümde, uygulamanın gereksinimine bağlı olarak, sertifikayı federasyon metadata XML **(.xml** dosyası) olarak indirmek için bir bağlantı da gösterilebilir.

![SAML etkin imzalama sertifikası indirme seçenekleri](./media/manage-certificates-for-federated-single-sign-on/active-certificate-download-options.png)

Ayrıca, **SAML İmzalama Sertifikası** sayfasını görüntüleyen **SAML İmzalama Sertifikası** başlığının **Düzenleme** simgesini (kalem) seçerek etkin veya etkin olmayan bir sertifika indirebilirsiniz. İndirmek istediğiniz sertifikanın yanındaki elipsis (**...**) seçeneğini belirleyin ve ardından istediğiniz sertifika biçimini seçin. Sertifikayı gizlilik le geliştirilmiş posta (PEM) biçiminde indirmek için ek seçeneğiniz vardır. Bu biçim Base64 ile aynıdır, ancak Windows'da sertifika biçimi olarak tanınmayan **.pem** dosya adı uzantısı ile.

![SAML imzalama sertifikası indirme seçenekleri (etkin ve etkin olmayan)](./media/manage-certificates-for-federated-single-sign-on/all-certificate-download-options.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Federasyon sertifikanızın son kullanma tarihini özelleştirin ve yeni bir sertifikaya yuvarlayın

Varsayılan olarak, Azure bir sertifikayı SAML tek oturum açma yapılandırması sırasında otomatik olarak oluşturulduğunda üç yıl sonra sona erecek şekilde yapılandırır. Sertifikayı kaydettikten sonra tarihini değiştiremediğinden şunları yapmalısın:

1. İstenilen tarihte yeni bir sertifika oluşturun.
1. Yeni sertifikayı kaydedin.
1. Yeni sertifikayı doğru biçimde indirin.
1. Yeni sertifikayı uygulamaya yükleyin.
1. Azure Etkin Dizin portalında yeni sertifikayı etkin hale getirin.

Aşağıdaki iki bölüm, aşağıdaki adımları gerçekleştirmenize yardımcı olur.

### <a name="create-a-new-certificate"></a>Yeni bir sertifika oluşturma

İlk olarak, farklı bir son kullanma tarihiyle yeni sertifika oluşturun ve kaydedin:

1. Azure Active [Directory portalında](https://aad.portal.azure.com/)oturum açın. **Azure Etkin Dizin yöneticisi merkezi** sayfası görüntülenir.
1. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Hesabınızdaki kurumsal uygulamaların listesi görüntülenir.
1. Etkilenen uygulamayı seçin. Uygulama için genel bir sayfa görüntülenir.
1. Uygulamaya genel bakış sayfasının sol bölmesinde **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** sayfası seç seçeneği görünüyorsa, **SAML'yi**seçin.
1. **SAML ile Tek Oturum Açma - Önizleme** sayfasında **SAML İmza Sertifikası** başlığını bulun ve **Edit** simgesini (kalem) seçin. **SamL İmzaLama Sertifikası** sayfası görüntülenir ve bu sayfa her sertifikanın durumunu **(Etkin** veya **Etkin Olmayan),** son kullanma tarihini ve her sertifikanın parmak izini (karma dize) görüntüler.
1. **Yeni Sertifika**seçin. Son kullanma tarihi geçerli tarihten tam olarak üç yıl sonra varsayılan olarak sertifika listesinin altında yeni bir satır görüntülenir. (Değişiklikleriniz henüz kaydedilmedi, bu nedenle son kullanma tarihini yine de değiştirebilirsiniz.)
1. Yeni sertifika satırında, son kullanma tarihi sütununa doğru gidip **Tarihi Seç** simgesini (takvim) seçin. Yeni satırın geçerli son kullanma tarihi olan ayın günlerini gösteren bir takvim denetimi görüntülenir.
1. Yeni bir tarih ayarlamak için takvim denetimini kullanın. Geçerli tarih ile geçerli tarihten sonraki üç yıl arasındaki herhangi bir tarihi ayarlayabilirsiniz.
1. **Kaydet'i**seçin. Yeni sertifika artık **Etkin Olmayan**bir durumla, seçtiğiniz son kullanma tarihiyle ve bir parmak iziyle görünür.
1. **SAML - Önizleme sayfasıyla Tek Oturum Açma'yı** ayarla'ya dönmek için **X'i** seçin.

### <a name="upload-and-activate-a-certificate"></a>Sertifika yükleme ve etkinleştirme

Ardından, yeni sertifikayı doğru biçimde indirin, uygulamaya yükleyin ve Azure Etkin Dizini'nde etkin hale getirin:

1. Uygulamanın ek SAML oturum açma yapılandırma yönergelerini aşağıdakilerden birini görüntüleyin:

   - Ayrı bir tarayıcı penceresinde veya sekmede görüntülemek için **yapılandırma kılavuzu** bağlantısını seçme veya
   - **Ayar başlığına** gitme ve kenar çubuğunda görüntülemek için **adım adım yönergeleri görüntüle'yi** seçin.

1. Yönergelerde, sertifika yüklemesi için gereken kodlama biçimini not edin.
1. Galeri ve galeri dışı uygulamalar bölümü [için Otomatik oluşturulan sertifikadaki](#auto-generated-certificate-for-gallery-and-non-gallery-applications) yönergeleri daha önce izleyin. Bu adım, sertifikayı uygulama tarafından yüklemek için gereken kodlama biçiminde indirir.
1. Yeni sertifikaya geçmek istediğinizde, **SAML İmzaSertifikası** sayfasına geri dön ve yeni kaydedilen sertifika satırında elipsleri (**...**) seçin ve **sertifikayı etkin hale'i**seçin. Yeni sertifikanın durumu **Etkin**olarak değişir ve daha önce etkin olan sertifika **Etkin Olmayan**bir durumla değişir.
1. Saml imzalama sertifikasını doğru kodlama biçiminde yükleyebilmeniz için, daha önce görüntülediğiniz UYGULAMANIN SAML oturum açma yapılandırma yönergelerini takip edin.

## <a name="add-email-notification-addresses-for-certificate-expiration"></a>Sertifika nın süresi nin dolması için e-posta bildirim adresleri ekleme

Azure AD, SAML sertifikasının süresinin dolmasından 60, 30 ve 7 gün önce bir e-posta bildirimi gönderir. Bildirimleri almak için birden fazla e-posta adresi ekleyebilirsiniz. Bildirimlerin gönderilmesini istediğiniz e-posta adresini(es) belirtmek için:

1. **SAML İmza Sertifikası** sayfasında, **bildirim e-posta adresleri** başlığına gidin. Varsayılan olarak, bu başlık yalnızca uygulamayı ekleyen yöneticinin e-posta adresini kullanır.
1. Son e-posta adresinin altına, sertifikanın son kullanma bildirimini alması gereken e-posta adresini yazın ve enter tuşuna basın.
1. Eklemek istediğiniz her e-posta adresi için önceki adımı yineleyin.
1. Silmek istediğiniz her e-posta adresi için, e-posta adresinin yanındaki **Sil** simgesini (çöp kutusu) seçin.
1. **Kaydet'i**seçin.

Bildirim e-postasını aadnotification@microsoft.com. İstenmeyen posta konumunuza giden e-postayı önlemek için bu e-postayı kişilerinize ekleyin.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Yakında süresi dolacak bir sertifikayı yenileme

Sertifikanın süresi dolmak üzereyse, kullanıcılarınız için önemli bir kapalı kalma süresi olmayan bir yordam kullanarak sertifikayı yenileyebilirsiniz. Süresi dolan bir sertifikayı yenilemek için:

1. Varolan sertifikayla çakışan bir tarih [kullanarak, yeni sertifika oluştur](#create-a-new-certificate) bölümündeki yönergeleri daha önce izleyin. Bu tarih, sertifikanın sona ermesinden kaynaklanan kapalı kalma süresini sınırlar.
1. Uygulama bir sertifikayı otomatik olarak devredebiliyorsa, aşağıdaki adımları izleyerek yeni sertifikayı etkin olarak ayarlayın:
   1. **SAML İmza Sertifikası** sayfasına geri dön.
   1. Yeni kaydedilen sertifika satırında, elipsis **(...**) seçeneğini belirleyin ve ardından **sertifikayı etkin hale'yi**seçin.
   1. Sonraki iki adımı atlayın.

1. Uygulama aynı anda yalnızca bir sertifikayı işleyebiliyorsa, bir sonraki adımı gerçekleştirmek için bir kesinti aralığı seçin. (Aksi takdirde, uygulama yeni sertifikayı otomatik olarak almazsa ve birden fazla imza sertifikasını işleyebilirse, bir sonraki adımı istediğiniz zaman gerçekleştirebilirsiniz.)
1. Eski sertifikanın süresi dolmadan önce Yükle'deki yönergeleri izleyin ve sertifika bölümünü daha önce [etkinleştirin.](#upload-and-activate-a-certificate)
1. Sertifikanın doğru çalıştığından emin olmak için uygulamada oturum açın.

## <a name="related-articles"></a>İlgili makaleler:

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](../saas-apps/tutorial-list.md)
- [Azure Active Directory ile uygulama yönetimi](what-is-application-management.md)
- [Azure Active Directory'deki uygulamalarda tek oturum açma](what-is-single-sign-on.md)
- [Azure Active Directory'deki uygulamalarda SAML tabanlı tek oturum açma hatası](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
