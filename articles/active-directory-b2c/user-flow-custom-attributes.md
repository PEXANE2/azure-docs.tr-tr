---
title: Azure Etkin Dizin B2C'de özel öznitelikleri tanımlama | Microsoft Dokümanlar
description: Müşterileriniz hakkında bilgi toplamak için Azure Active Directory B2C'de uygulamanız için özel öznitelikler tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 7b1ecfba0435f827c7c7a4d05da619998140bc6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186058"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Azure Etkin Dizin B2C'de özel öznitelikleri tanımlama

 Müşteriye yönelik her uygulamanın toplanması gereken bilgiler için benzersiz gereksinimleri vardır. Azure Active Directory B2C (Azure AD B2C) kiracınız, Verilen Ad, Soyad, Şehir ve Posta Kodu gibi özniteliklerde depolanmış yerleşik bir bilgi kümesiyle birlikte gelir. Azure AD B2C ile, her müşteri hesabında depolanan öznitelikler kümesini genişletebilirsiniz.

 [Azure portalında](https://portal.azure.com/) özel öznitelikler oluşturabilir ve bunları kaydolma kullanıcı akışlarınızda, kaydolma veya kaydolma kullanıcı akışlarınızda veya profil düzenleme kullanıcı akışlarında kullanabilirsiniz. Ayrıca [Microsoft Graph API'yi](manage-user-accounts-graph-api.md)kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz.

## <a name="create-a-custom-attribute"></a>Özel bir öznitelik oluşturma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin.

    ![Azure AD B2C kiracınıza geçiş yapma](./media/user-flow-custom-attributes/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin ve Abonelik filtresinde vurgulanan B2C kiracı](./media/user-flow-custom-attributes/select-directory.PNG)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. **Kullanıcı özniteliklerini**seçin ve sonra **Ekle'yi**seçin.
5. Özel öznitelik için bir **Ad** sağlayın (örneğin, "ShoeSize")
6. Bir **Veri Türü**seçin. Yalnızca **String**, **Boolean**ve **Int** mevcuttur.
7. İsteğe bağlı olarak, bilgilendirme amacıyla bir **Açıklama** girin.
8. **Oluştur'u**tıklatın.

Özel öznitelik artık **Kullanıcı öznitelikleri** listesinde ve kullanıcı akışlarınızda kullanılmak üzere kullanılabilir. Özel bir öznitelik, yalnızca herhangi bir kullanıcı akışında ilk kez kullanıldığında oluşturulur ve **kullanıcı öznitelikleri**listesine eklediğinizde değil.

## <a name="use-a-custom-attribute-in-your-user-flow"></a>Kullanıcı akışınızda özel bir öznitelik kullanma

1. Azure AD B2C kiracınızda **Kullanıcı akışlarını**seçin.
1. Açmak için ilkenizi (örneğin, "B2C_1_SignupSignin") seçin.
1. **Kullanıcı özniteliklerini** seçin ve ardından özel özniteliği (örneğin, "ShoeSize") seçin. **Kaydet**'e tıklayın.
1. **Uygulama taleplerini** seçin ve ardından özel özniteliği seçin.
1. **Kaydet**'e tıklayın.

Yeni oluşturulan özel özniteliği kullanan bir kullanıcı akışını kullanarak yeni bir kullanıcı oluşturduktan sonra, nesne [Microsoft Graph Explorer'da](https://developer.microsoft.com/graph/graph-explorer)sorgulanabilir. Alternatif olarak, müşteri deneyimini doğrulamak için kullanıcı akışındaki [Kullanıcı akışı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows) özelliğini kullanabilirsiniz. Şimdi **ShoeSize'ı** kayıt yolculuğu sırasında toplanan öznitelikler listesinde görmeli ve başvurunuza geri gönderilen belirteçte görmelisiniz.
