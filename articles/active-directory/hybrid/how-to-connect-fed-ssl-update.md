---
title: Azure AD Connect-bir AD FS grubu için TLS/SSL sertifikasını güncelleştirin | Microsoft Docs
description: Bu belge Azure AD Connect kullanarak bir AD FS grubunun TLS/SSL sertifikasını güncelleştirme adımlarının ayrıntılarını sağlar.
services: active-directory
manager: daveba
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/09/2018
ms.subservice: hybrid
author: billmath
ms.custom: seohack1
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1983b5090604516265ea8e041ac68200ca2dc7b5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85359594"
---
# <a name="update-the-tlsssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Active Directory Federasyon Hizmetleri (AD FS) (AD FS) grubu için TLS/SSL sertifikasını güncelleştirme

## <a name="overview"></a>Genel Bakış
Bu makalede, bir Active Directory Federasyon Hizmetleri (AD FS) (AD FS) grubu için TLS/SSL sertifikasını güncelleştirmek üzere Azure AD Connect nasıl kullanabileceğiniz açıklanır. Azure AD Connect aracını, seçilen kullanıcı oturum açma yöntemi AD FS olmasa bile AD FS grubunun TLS/SSL sertifikasını kolayca güncelleştirmek için kullanabilirsiniz.

Tüm Federasyon ve Web uygulaması ara sunucusu (WAP) sunucularında AD FS grubu için TLS/SSL sertifikasını güncelleştirme işleminin tamamını üç basit adımda gerçekleştirebilirsiniz:

![Üç adım](./media/how-to-connect-fed-ssl-update/threesteps.png)


>[!NOTE]
>AD FS tarafından kullanılan sertifikalar hakkında daha fazla bilgi için bkz. [AD FS tarafından kullanılan sertifikaları anlama](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Ön koşullar

* **AD FS grubu**: AD FS grubunuzun Windows Server 2012 R2 tabanlı veya sonraki bir sürümü olduğundan emin olun.
* **Azure AD Connect**: Azure AD Connect sürümünün 1.1.553.0 veya daha yüksek olduğundan emin olun. **AD FS SSL sertifikasını görev Güncelleştir**' i kullanacaksınız.

![TLS görevini Güncelleştir](./media/how-to-connect-fed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>1. Adım: AD FS grubu bilgilerini sağlama

Azure AD Connect, AD FS grubu hakkında otomatik olarak bilgi edinmeye çalışır:
1. AD FS grup bilgilerini sorgulama (Windows Server 2016 veya üzeri).
2. Azure AD Connect ile yerel olarak depolanan önceki çalıştırmaların bilgilerine başvuru.

Sunucuları ekleyerek veya kaldırarak görüntülenen sunucuların listesini AD FS grubunun geçerli yapılandırmasını yansıtacak şekilde değiştirebilirsiniz. Sunucu bilgileri sağlandığı anda, Azure AD Connect bağlantı ve geçerli TLS/SSL sertifika durumunu görüntüler.

![AD FS Server bilgileri](./media/how-to-connect-fed-ssl-update/adfsserverinfo.png)

Liste, artık AD FS grubunun parçası olmayan bir sunucu içeriyorsa, sunucuyu AD FS grubunuzdaki sunucu listesinden silmek için **Kaldır** ' a tıklayın.

![Listede çevrimdışı sunucu](./media/how-to-connect-fed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Bir sunucunun Azure AD Connect bir AD FS grubu için sunucu listesinden kaldırılması yerel bir işlemdir ve Azure AD Connect yerel olarak tuttuğu AD FS grubu için bilgileri güncelleştirir. Azure AD Connect, AD FS yapılandırmayı değişikliği yansıtacak şekilde değiştirmez.    

## <a name="step-2-provide-a-new-tlsssl-certificate"></a>2. Adım: yeni bir TLS/SSL sertifikası sağlama

AD FS grubu sunucularıyla ilgili bilgileri onayladıktan sonra, Azure AD Connect yeni TLS/SSL sertifikasını ister. Yüklemeye devam etmek için parola korumalı bir PFX sertifikası sağlayın.

![TLS/SSL sertifikası](./media/how-to-connect-fed-ssl-update/certificate.png)

Sertifikayı verdikten sonra Azure AD Connect bir dizi önkoşul üzerinden ilerler. Sertifikanın AD FS grubu için doğru olduğundan emin olmak için sertifikayı doğrulayın:

-   Sertifika için konu adı/alternatif konu adı, Federasyon Hizmeti adı ile aynı veya bir joker karakter sertifikasıdır.
-   Sertifika 30 günden daha uzun bir süre geçerlidir.
-   Sertifika güven zinciri geçerli.
-   Sertifika, parola korumalı.

## <a name="step-3-select-servers-for-the-update"></a>3. Adım: güncelleştirme için sunucuları seçin

Sonraki adımda, TLS/SSL sertifikasının güncel olması gereken sunucuları seçin. Çevrimdışı olan sunucular güncelleştirme için seçilemez.

![Güncelleştirilecek sunucuları seçin](./media/how-to-connect-fed-ssl-update/selectservers.png)

Yapılandırmayı tamamladıktan sonra, Azure AD Connect güncelleştirmenin durumunu belirten iletiyi görüntüler ve AD FS oturum açma doğrulaması için bir seçenek sağlar.

![Yapılandırma tamamlandı](./media/how-to-connect-fed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>SSS

* **Yeni AD FS TLS/SSL sertifikası için sertifikanın konu adı ne olmalıdır?**

    Azure AD Connect, sertifikanın konu adı/alternatif konu adının Federasyon Hizmeti adını içerip içermediğini denetler. Örneğin, Federasyon Hizmeti adınız fs.contoso.com ise, konu adı/alternatif konu adı fs.contoso.com olmalıdır.  Joker karakter sertifikaları da kabul edilir.

* **WAP sunucusu sayfasında neden kimlik bilgileri yeniden soruluyor?**

    AD FS sunucusuna bağlanmak için sağladığınız kimlik bilgileri aynı zamanda WAP sunucularını yönetme ayrıcalığına sahip değilse, Azure AD Connect WAP sunucularında yönetici ayrıcalıklarına sahip kimlik bilgilerini ister.

* **Sunucu çevrimdışı olarak gösterilir. Ne yapmam gerekir?**

    Sunucu çevrimdışıysa Azure AD Connect hiçbir işlem gerçekleştiremez. Sunucu AD FS grubunun bir parçasıysa sunucuya olan bağlantıyı denetleyin. Sorunu çözdükten sonra, sihirbazdaki durumu güncelleştirmek için Yenile simgesine basın. Sunucu daha önce grubun parçasıysa, ancak artık mevcut değilse, Azure AD Connect koruduğu sunucu listesinden silmek için **Kaldır** ' a tıklayın. Sunucusunun Azure AD Connect listeden kaldırılması AD FS yapılandırmasının kendisini değiştirmez. Windows Server 2016 veya sonraki sürümlerde AD FS kullanıyorsanız, sunucu yapılandırma ayarlarında kalır ve görevin bir sonraki çalıştırılışında yeniden gösterilir.

* **Grup sunucularım 'ın bir alt kümesini yeni TLS/SSL sertifikasıyla güncelleştirebilir miyim?**

    Evet. Kalan sunucuları güncelleştirmek için, her zaman **SSL sertifikasını** yeniden çalıştırabilirsiniz. **SSL sertifikası için sunucuları seçin güncelleştirme** sayfasında, henüz güncelleştirilmemiş sunuculara kolayca erişmek Için **SSL bitiş tarihi** ' nin sunucu listesini sıralayabilirsiniz.

* **Önceki çalıştırmada sunucuyu kaldırdım, ancak hala çevrimdışı olarak gösterilmekte ve AD FS sunucuları sayfasında listelenmeye devam ediyor. Neden çevrimdışı sunucu kaldırıldıktan sonra bile devam ediyor?**

    Sunucusunun Azure AD Connect listesinden kaldırılması, AD FS yapılandırmasında kaldırmaz. Azure AD Connect, grupla ilgili herhangi bir bilgi için AD FS başvuruları (Windows Server 2016 veya üzeri). Sunucu hala AD FS yapılandırmasında mevcutsa, listede geri listelenecektir.  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Connect ve Federasyon](how-to-connect-fed-whatis.md)
- [Azure AD Connect ile yönetimi ve özelleştirmeyi Active Directory Federasyon Hizmetleri (AD FS)](how-to-connect-fed-management.md)

