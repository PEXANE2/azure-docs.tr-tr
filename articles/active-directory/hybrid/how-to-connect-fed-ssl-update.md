---
title: Azure AD Connect - BIR AD FS çiftliği için TLS/SSL sertifikasını güncelleştirin | Microsoft Dokümanlar
description: Bu belge, Azure AD Connect'i kullanarak bir AD FS çiftliğinin TLS/SSL sertifikasını güncelleştirme adımlarını ayrıntılarıyla anlatır.
services: active-directory
manager: daveba
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/09/2018
ms.subservice: hybrid
author: billmath
ms.custom: seohack1
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cc768162d98402fe52b52b2826a9dbf2840a581
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331749"
---
# <a name="update-the-tlsssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Aktif Dizin Federasyon Hizmetleri (AD FS) çiftliği için TLS/SSL sertifikasını güncelleştirme

## <a name="overview"></a>Genel Bakış
Bu makalede, Etkin Dizin Federasyonu Hizmetleri (AD FS) çiftliğinin TLS/SSL sertifikasını güncelleştirmek için Azure AD Connect'i nasıl kullanabileceğiniz açıklanmaktadır. Seçilen kullanıcı oturum açma yöntemi AD FS olmasa bile AD FS çiftliğinin TLS/SSL sertifikasını kolayca güncellemek için Azure AD Connect aracını kullanabilirsiniz.

Tüm federasyon ve Web Application Proxy (WAP) sunucularında AD FS çiftliği için TLS/SSL sertifikasıgüncelleştirme işleminin tüm işlemini üç basit adımda gerçekleştirebilirsiniz:

![Üç adım](./media/how-to-connect-fed-ssl-update/threesteps.png)


>[!NOTE]
>AD FS tarafından kullanılan sertifikalar hakkında daha fazla bilgi edinmek için, [AD FS tarafından kullanılan sertifikaları anlama](https://technet.microsoft.com/library/cc730660.aspx)'ya bakın.

## <a name="prerequisites"></a>Ön koşullar

* **AD FS Farm**: AD FS çiftliğinizin Windows Server 2012 R2 tabanlı veya daha yeni olduğundan emin olun.
* **Azure AD Connect**: Azure AD Connect sürümünün 1.1.553.0 veya daha yüksek olduğundan emin olun. Görevi Güncelleştirme AD **FS SSL sertifikasını**kullanırsınız.

![TLS görevini güncelleştir](./media/how-to-connect-fed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>Adım 1: AD FS çiftlik bilgileri sağlayın

Azure AD Connect, AD FS çiftliği hakkında otomatik olarak aşağıdaki ler tarafından bilgi edinmeye çalışır:
1. AD FS'den (Windows Server 2016 veya sonraki) çiftlik bilgilerini sorgulama.
2. Azure AD Connect ile yerel olarak depolanan önceki çalıştırmalardan elde edilen bilgilere başvuru.

AD FS çiftliğinin geçerli yapılandırmasını yansıtacak şekilde sunucuları ekleyerek veya kaldırarak görüntülenen sunucuların listesini değiştirebilirsiniz. Sunucu bilgileri sağlanır sağlanmaz Azure AD Connect bağlantı ve geçerli TLS/SSL sertifika durumunu görüntüler.

![AD FS sunucu bilgileri](./media/how-to-connect-fed-ssl-update/adfsserverinfo.png)

Listede artık AD FS çiftliğinin bir parçası olmayan bir sunucu varsa, reklamı nızı AD FS çiftliğinizdeki sunucular listesinden silmek için **Kaldır'ı** tıklatın.

![Listede çevrimdışı sunucu](./media/how-to-connect-fed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Bir sunucuyu Azure AD Connect'teki bir AD FS çiftliğinin sunucu listesinden kaldırmak yerel bir işlemdir ve Azure AD Connect'in yerel olarak koruduğu AD FS çiftliğinin bilgilerini güncelleştirir. Azure AD Connect, AD FS'deki yapılandırmayı değişikliği yansıtacak şekilde değiştirmez.    

## <a name="step-2-provide-a-new-tlsssl-certificate"></a>Adım 2: Yeni bir TLS/SSL sertifikası sağlayın

AD FS çiftlik sunucuları hakkındaki bilgileri doğruladıktan sonra Azure AD Connect yeni TLS/SSL sertifikasını ister. Yüklemeye devam etmek için parola korumalı PFX sertifikası sağlayın.

![TLS/SSL sertifikası](./media/how-to-connect-fed-ssl-update/certificate.png)

Sertifikayı sağladıktan sonra Azure AD Connect bir dizi ön koşuldan geçer. SERTIFIKAnın AD FS çiftliği için doğru olduğundan emin olmak için sertifikayı doğrulayın:

-   Sertifikanın özne adı/alternatif özne adı, federasyon hizmet adı ile aynıdır veya joker karakter sertifikasıdır.
-   Sertifika 30 günden fazla geçerlidir.
-   Sertifika güven zinciri geçerlidir.
-   Sertifika parola korumalıdır.

## <a name="step-3-select-servers-for-the-update"></a>Adım 3: Güncelleştirme için sunucuları seçin

Bir sonraki adımda, TLS/SSL sertifikasının güncellenmesi gereken sunucuları seçin. Çevrimdışı olan sunucular güncelleştirme için seçilenemiyor.

![Güncelleştirilecek sunucuları seçin](./media/how-to-connect-fed-ssl-update/selectservers.png)

Yapılandırmayı tamamladıktan sonra, Azure AD Connect güncelleştirmenin durumunu belirten iletiyi görüntüler ve AD FS oturum açma oturumlarını doğrulamak için bir seçenek sunar.

![Yapılandırma tamamlandı](./media/how-to-connect-fed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>SSS

* **Yeni AD FS TLS/SSL sertifikası için sertifikanın özne adı ne olmalıdır?**

    Azure AD Connect, sertifikanın özne adının/alternatif özadının federasyon hizmet adını içerip içermeyolmadığını denetler. Örneğin, federasyon hizmet adınız fs.contoso.com ise, konu adı/alternatif konu adı fs.contoso.com olmalıdır.  Joker karakter sertifikaları da kabul edilmektedir.

* **NEDEN WAP sunucu sayfasında tekrar kimlik bilgileri isteniyor?**

    AD FS sunucularına bağlanmak için sağladığınız kimlik bilgileri DE WAP sunucularını yönetme ayrıcalığına sahip değilse, Azure AD Connect WAP sunucularında yönetim ayrıcalığına sahip kimlik bilgilerini ister.

* **Sunucu çevrimdışı olarak gösterilir. Ne yapmalıyım?**

    Azure AD Connect, sunucu çevrimdışıysa herhangi bir işlem gerçekleştiremez. Sunucu AD FS çiftliğinin bir parçasıysa, sunucuya bağlantıyı denetleyin. Sorunu çözdükten sonra, sihirbazdaki durumu güncelleştirmek için yenileme simgesine basın. Sunucu daha önce çiftliğin bir parçasıysa ancak artık yoksa, Azure AD Connect'in koruduğu sunucular listesinden silmek için **Kaldır'ı** tıklatın. Sunucuyu Azure AD Connect'teki listeden kaldırmak, AD FS yapılandırmasının kendisini değiştirmez. Windows Server 2016 veya sonraki yıllarda AD FS kullanıyorsanız, sunucu yapılandırma ayarlarında kalır ve görev bir sonraki çalıştırılışta yeniden gösterilir.

* **Çiftlik sunucularımın bir alt kümesini yeni TLS/SSL sertifikasıyla güncelleyebilir miyim?**

    Evet. Kalan sunucuları güncelleştirmek için görev **Güncelleştirme SSL Sertifikası'nı** her zaman yeniden çalıştırabilirsiniz. **SSL sertifika güncelleştirmesi için Seç sunucularında,** henüz güncelleştirilmeyen sunuculara kolayca erişmek için **SSL Son Kullanma tarihindeki** sunucuların listesini sıralayabilirsiniz.

* **Önceki çalıştırmada sunucukaldırıldı, ama yine de çevrimdışı olarak gösteriliyor ve AD FS Sunucular sayfasında listeleniyor. Ben kaldırdıktan sonra bile çevrimdışı sunucu neden hala orada?**

    Sunucuyu Azure AD Connect'teki listeden kaldırmak, sunucuyu AD FS yapılandırmasında kaldırmaz. Azure AD Connect, çiftlik le ilgili herhangi bir bilgi için AD FS'ye (Windows Server 2016 veya üzeri) başvurur. Sunucu AD FS yapılandırmasında hala mevcutsa, listede geri listelenir.  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Connect ve federasyon](how-to-connect-fed-whatis.md)
- [Azure AD Connect ile Etkin Dizin Federasyonu Hizmetleri yönetimi ve özelleştirme](how-to-connect-fed-management.md)

