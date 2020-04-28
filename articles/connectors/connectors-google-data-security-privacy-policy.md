---
title: Google bağlayıcıları için veri güvenliği ve gizlilik ilkeleri
description: Google güvenlik ve gizlilik ilkelerinin Gmail gibi Google bağlayıcılarına sahip olduğu etkileri hakkında bilgi edinin Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 04/24/2020
ms.openlocfilehash: 8c2ef8d174e8449b88f2257074bee69c3f45c6d9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181948"
---
# <a name="data-security-and-privacy-policies-for-google-connectors-in-azure-logic-apps"></a>Azure Logic Apps 'de Google bağlayıcıları için veri güvenliği ve gizlilik ilkeleri

**1 mayıs 2020**' den Itibaren, Google 'ın [veri güvenliği ve gizlilik ilkeleri](https://www.blog.google/technology/safety-security/project-strobe/) nedeniyle yapılan değişiklikler [Gmail bağlayıcısını](https://docs.microsoft.com/connectors/gmail/)kullanan mantıksal uygulama iş akışlarınızı etkileyebilir. Mantıksal uygulamalarınız Gmail bağlayıcısını bir Gmail tüketicisi hesabı (veya @gmail.com @googlemail.comile biten e-posta adresi) ile kullanıyorsa, Logic Apps yalnızca belirli [Google-onaylanan Tetikleyiciler, Eylemler ve bağlayıcılar](#approved-connectors)kullanabilir. 

> [!NOTE]
> Mantıksal uygulamalarınız Gmail bağlayıcısını bir G-Suite iş hesabıyla (özel bir etki alanı olan e-posta adresi) kullanıyorsa, mantıksal uygulamalarınız etkilenmez ve Gmail bağlayıcısını kullanma konusunda hiçbir kısıtlama yoktur.

## <a name="affected-logic-apps"></a>Etkilenen Logic Apps

Gmail bağlayıcısını kullanan Logic Apps varsa, potansiyel olarak etkilenen Logic Apps hakkında bir e-posta alırsınız. Ancak, **15 haziran 2020**' den itibaren uyumlu olmayan tüm iş akışları devre dışı bırakılacaktır. Şu eylemlerden birini gerçekleştirebilirsiniz:

* Etkilenen mantık uygulamalarını, [Bu konudaki adımları izleyerek](#update-affected-workflows)güncelleştirin. Gmail tetikleyicinizin veya eyleminizdeki kimlik doğrulaması için kullandığınız istemci KIMLIĞI ve gizli anahtar sağlayan bir Google istemci uygulaması oluşturmanız gerekir.

* Etkilenen mantık uygulamalarını, devre dışı bırakılan Logic Apps 'i yeniden etkinleştirmeden önce yalnızca [Google onaylı bağlayıcıları](#approved-connectors) kullanacak şekilde güncelleştirin.

<a name="approved-connectors"></a>

## <a name="google-approved-connectors"></a>Google onaylı bağlayıcılar

Bu ilke altında, bir Gmail tüketici hesabı kullandığınızda Gmail bağlayıcısını yalnızca belirli Google onaylı hizmetlerle (değiştirilebilir) kullanabilirsiniz. Mühendislik takımlarımız, bu listeye daha fazla hizmet eklemek için Google ile çalışmaya devam eder. Şimdilik, Gmail tüketici hesabı kullandığınızda Gmail Bağlayıcısı ile aynı Logic App iş akışında kullanabileceğiniz Google onaylı Tetikleyiciler, Eylemler ve bağlayıcılar aşağıda verilmiştir:

* Logic Apps yerleşik Tetikleyiciler ve eylemler: Batch, Control, veri Işlemleri, tarih saat, düz dosya, HTTP, sıvı, Istek, zamanlama, değişkenler ve XML

* Google hizmetleri: Gmail, Google Takvim, Google Contacts, Google Drive, Google sayfaları ve Google görevleri

* Onaylanan Microsoft Hizmetleri: Dynamics 365, Excel Online, Microsoft ekipleri, Office 365, OneDrive ve SharePoint Online

* Müşteri tarafından yönetilen veri kaynakları için bağlayıcılar: FTP, RSS, SFTP, SMTP ve SQL Server

En son bilgiler için [Gmail bağlayıcısının teknik başvuru belgelerine](https://docs.microsoft.com/connectors/gmail/)bakın.

<a name="update-affected-workflows"></a>

## <a name="steps-for-affected-logic-apps"></a>Etkilenen Logic Apps için adımlar

Bir mantıksal uygulamada Gmail adlı bir tüketici hesabı ve Google onaylanmamış bağlayıcılar ile Gmail bağlayıcısını kullanmanız gerekiyorsa, kuruluşunuzda kişisel veya dahili kullanım için kendi Google uygulamanızı oluşturabilirsiniz. Bu senaryo için yapmanız gereken üst düzey adımlar şunlardır:

1. [Google API konsolunu](https://console.developers.google.com)kullanarak bir Google istemci uygulaması oluşturun.

1. Gmail bağlayıcısında, Google istemci uygulamanızdan istemci KIMLIĞI ve istemci gizli değerlerini kullanın.

Daha fazla bilgi için [Gmail bağlayıcısının teknik başvuru belgelerine](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application)bakın.

### <a name="create-google-client-app"></a>Google istemci uygulaması oluşturma

İstemci uygulamanız için bir proje kurmak üzere [Google API konsol Sihirbazı 'nı](https://console.developers.google.com/start/api?id=gmail&credential=client_key) kullanın ve yönergeleri izleyin. Ya da, ayrıntılı adımlar için [Gmail bağlayıcısının teknik başvuru belgelerindeki](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application)yönergeleri gözden geçirin.

İşiniz bittiğinde, ekranınızda daha sonra mantıksal uygulamanızda kullanacağınız kendi **ISTEMCI kimliğiniz** ve **istemci gizli** değerleriniz olacak şekilde bu örneğe benzer şekilde görünür.

![Google istemci uygulamanız için istemci KIMLIĞI ve istemci gizli anahtarı](./media/connectors-google-data-security-privacy-policy/google-api-console.png)

### <a name="use-client-app-settings-in-logic-app"></a>Mantıksal uygulamada istemci uygulama ayarlarını kullan

Google istemci uygulamanızdan Gmail Tetikleyiciniz veya eyleminizdeki istemci KIMLIĞINI ve istemci gizli anahtarını kullanmak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Yeni bir Gmail tetikleyicisi veya eylemi ekliyorsanız ve tamamen yeni bir bağlantı oluşturuyorsanız, sonraki adımla devam edin. Aksi takdirde, Gmail tetikleyicisinde veya eyleminde **bağlantıyı** > Değiştir**Yeni Ekle**' yi seçin, örneğin:

   !["Bağlantıyı değiştir" > "Yeni Ekle" seçeneğini belirleyin](./media/connectors-google-data-security-privacy-policy/change-gmail-connection.png)

1. Bağlantı bilgilerinizi sağlayın, örneğin:

   ![Bağlantı bilgilerini girin](./media/connectors-google-data-security-privacy-policy/authentication-type-bring-your-own.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Kimlik doğrulama türü** | **Kendi uygulamanızı getir** | Kimlik doğrulaması için kendi istemci uygulamanızı kullanacağınızı belirtir. |
   | **İstemci KIMLIĞI** | <*istemci KIMLIĞI*> | Google istemci uygulamanızdan istemci KIMLIĞI |
   | **İstemci parolası** | <*istemci parolası*> | Google istemci uygulamanızdan istemci parolası |
   ||||

1. İşiniz bittiğinde **oturum aç**' ı seçin.

   Oluşturduğunuz istemci uygulamasını gösteren bir sayfa görüntülenir. Gmail tüketici hesabı kullanıyorsanız, istemci uygulamanızın Google tarafından doğrulanmayacağını gösteren bir sayfa alabilir ve önce Google hesabınıza erişime izin vermenizi ister.

   ![Google hesabınıza erişim istemi](./media/connectors-google-data-security-privacy-policy/allow-access-authorized-domain.png)

1. Gerekirse, **Izin ver**' i seçin.

   Artık, mantıksal uygulamanızda kısıtlama olmadan Gmail bağlayıcısını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Gmail Bağlayıcısı](https://docs.microsoft.com/connectors/gmail/) hakkında daha fazla bilgi edinin
