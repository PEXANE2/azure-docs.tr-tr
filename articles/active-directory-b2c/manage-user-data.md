---
title: Azure Active Directory B2C'de kullanıcı verilerini yönetme | Microsoft Dokümanlar
description: Azure AD B2C'de kullanıcı verilerini nasıl sildiğinizi veya dışa aktarırken öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/06/2018
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: e245b58449ab773914fc60be056082b82f05035a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184494"
---
# <a name="manage-user-data-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı verilerini yönetme

 Bu makalede, [Microsoft Graph API](https://docs.microsoft.com/graph/use-the-api)tarafından sağlanan işlemleri kullanarak Azure Active Directory B2C 'deki (Azure AD B2C) kullanıcı verilerini nasıl yönetebileceğiniz açıklanmaktadır. Kullanıcı verilerini yönetmek, denetim günlüklerinden veri silmeyi veya dışa aktarmayı içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-user-data"></a>Kullanıcı verilerini silme

Kullanıcı verileri Azure AD B2C dizininde ve denetim günlüklerinde depolanır. Tüm kullanıcı denetim verileri Azure AD B2C'de 7 gün boyunca saklanır. Bu 7 günlük süre içinde kullanıcı verilerini silmek istiyorsanız, kullanıcı işlemini [sil](https://docs.microsoft.com/graph/api/user-delete) işlemini kullanabilirsiniz. Verilerin bulunduğu Azure AD B2C kiracılarının her biri için DELETE işlemi gereklidir.

Azure AD B2C'deki her kullanıcıya bir nesne kimliği atanır. Nesne kimliği, Azure AD B2C'deki kullanıcı verilerini silmek için kullanabileceğiniz kesin bir tanımlayıcı sağlar. Mimarinize bağlı olarak, nesne kimliği finansal, pazarlama ve müşteri ilişkileri yönetimi veritabanları gibi diğer hizmetler arasında yararlı bir korelasyon tanımlayıcısı olabilir.

Bir kullanıcı için nesne kimliğini almanın en doğru yolu, Azure AD B2C ile kimlik doğrulama yolculuğunun bir parçası olarak bu kimliği elde etmektir. Başka yöntemler kullanarak kullanıcıdan geçerli bir veri isteği alırsanız, kullanıcıyı bulmak ve ilişkili nesne kimliğini not almak için müşteri hizmetleri destek aracısının araması gibi çevrimdışı bir işlem gerekebilir.

Aşağıdaki örnekolası bir veri silme akışını gösterir:

1. Kullanıcı kaydolur ve **verilerimi sil'i**seçer.
2. Uygulama, uygulamanın yönetim bölümündeki verileri silme seçeneği sunar.
3. Uygulama, kimlik doğrulamayı Azure AD B2C'ye zorlar. Azure AD B2C, kullanıcının nesne kimliğini uygulamaya geri getirebilmek için bir belirteç sağlar.
4. Belirteç uygulama tarafından alınır ve nesne kimliği Microsoft Graph API'ye yapılan bir çağrı yla kullanıcı verilerini silmek için kullanılır. Microsoft Graph API kullanıcı verilerini siler ve 200 Ok durum kodunu döndürür.
5. Uygulama, nesne kimliği veya diğer tanımlayıcılar kullanılarak gerektiğinde kullanıcı verilerinin diğer kuruluş sistemlerinde silinmesini yönetir.
6. Uygulama verilerin silinmesini onaylar ve kullanıcıya sonraki adımları sağlar.

## <a name="export-customer-data"></a>Müşteri verilerini dışa aktarma

Azure AD B2C'den müşteri verilerini dışa aktarma işlemi silme işlemine benzer.

Azure AD B2C kullanıcı verileri aşağıdakiile sınırlıdır:

- **Azure Etkin Dizini'nde depolanan veriler**: Nesne kimliğini veya e-posta adresi veya kullanıcı adı gibi oturum açma adını kullanarak Azure AD B2C kimlik doğrulama kullanıcı yolculuğunda veri alabilirsiniz.
- **Kullanıcıya özel denetim olayları raporu**: Nesne kimliğini kullanarak verileri dizine ekebilirsiniz.

Bir dışa aktarım veri akışı aşağıdaki örnekte, uygulama tarafından gerçekleştirilen olarak tanımlanan adımlar da bir arka uç işlemi veya dizinde yönetici rolü olan bir kullanıcı tarafından gerçekleştirilebilir:

1. Kullanıcı uygulamaya başvurur. Azure AD B2C, gerekirse Azure Çok Faktörlü Kimlik Doğrulaması ile kimlik doğrulaması zorlar.
2. Uygulama, kullanıcı özniteliklerini almak için bir Microsoft Graph API işlemini aramak için kullanıcı kimlik bilgilerini kullanır. Microsoft Graph API, öznitelik verilerini JSON biçiminde sağlar. Şemaya bağlı olarak, kimlik belirteci içeriğini kullanıcı hakkındaki tüm kişisel verileri içerecek şekilde ayarlayabilirsiniz.
3. Uygulama kullanıcı denetim etkinliğini alır. Microsoft Graph API, uygulama için olay verilerini sağlar.
4. Uygulama verileri toplar ve kullanıcıtarafından kullanılabilir hale getirir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcıların uygulamanıza nasıl erişeceklerini öğrenmek için [bkz.](manage-user-access.md)
