---
title: Azure Active Directory portalındaki oturum açma etkinlik raporları | Microsoft Docs
description: Azure Active Directory portalındaki oturum açma etkinlik raporlarına giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/24/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2910933e2c57a8bc80a220726462b02915c4a8eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246526"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki oturum açma etkinlik raporları

Azure Etkin Dizin (Azure AD) raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma lar** – Yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi.
    - **Denetim günlükleri** - [Denetim günlükleri,](concept-audit-logs.md) kullanıcılar ve grup yönetimi, yönetilen uygulamalar ve dizin etkinlikleri hakkında sistem etkinlik bilgileri sağlar.
- **Güvenlik** 
    - **Riskli oturum açmalar** - [Riskli oturum](concept-risky-sign-ins.md) açma, kullanıcı hesabının yasal sahibi olmayan birinin oturum açma girişiminin göstergesidir.
    - **Risk için işaretlenmiş kullanıcılar** - Riskli bir [kullanıcı,](concept-user-at-risk.md) gizliliği ihlal edilmiş olabilecek bir kullanıcı hesabı için bir göstergedir.

Bu makalede, oturum açma raporuna genel bir bakış sağlar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?

* Güvenlik Yöneticisi, Güvenlik Okuyucusu, Global Reader ve Rapor Okuyucu rollerindeki kullanıcılar
* Global Yöneticiler
* Tüm kullanıcılar (yönetici olmayan) kendi oturum açma etkinliklerine erişebilirler 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Oturum açma etkinliğine erişebilmek için hangi Azure AD lisansınızın olması gerekir?

- Oturum açma etkinlik raporu [Azure AD'nin tüm sürümlerinde](reference-reports-data-retention.md#how-long-does-azure-ad-store-the-data)kullanılabilir.

- Oturum açma verilerine bir API kullanarak erişmek istiyorsanız, kiracınızın bu verilerle ilişkili bir [Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) lisansına sahip olması gerekir.



## <a name="sign-ins-report"></a>Oturum açma raporu

Kullanıcı oturum açma raporu aşağıdaki soruların yanıtlarını sağlar:

* Belirli bir kullanıcının oturum açma düzeni nedir?
* Bir hafta içerisinde kaç kullanıcı oturum açtı?
* Bu açılan oturumların durumu nedir?

Azure [portalı](https://portal.azure.com) menüsünde **Azure Active Directory'yi**seçin veya herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.

![Azure Active Directory'yi seçin](./media/concept-sign-ins/select-azure-active-directory.png "Azure Active Directory")

**İzleme**altında, Oturum Açma raporunu açmak için **Oturum** [Açma'yı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)seçin.

![Oturum açma etkinliği](./media/concept-sign-ins/monitoring-sign-ins-in-azure-active-directory.png "Oturum açma etkinliği")

Bazı oturum açma kayıtlarının portalda gösterilmesi iki saat kadar sürebilir.

> [!IMPORTANT]
> Oturum açma raporu yalnızca, kullanıcının kullanıcı adını ve parolasını kullanarak el ile oturum açtığını **niçin oturum** açma ları, yani etkileşimli oturum açmaları görüntüler. Hizmetten hizmete kimlik doğrulaması gibi etkileşimli olmayan oturum açmalar oturum açma raporunda görüntülenmez. 

Oturum açma günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Oturum açma tarihi
- İlgili kullanıcı
- Kullanıcının oturum açmış olduğu uygulama
- Oturum açma durumu
- Risk algılama durumu
- Çok faktörlü kimlik doğrulaması (MFA) gereksiniminin durumu

![Oturum açma etkinliği](./media/concept-sign-ins/sign-in-activity.png "Oturum açma etkinliği")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Oturum açma etkinliği](./media/concept-sign-ins/19.png "Oturum açma etkinliği")

**Sütunlar** iletişim kutusu seçilebilir özniteliklere erişmenizi sağlar. Oturum açma raporunda, sütun olarak belirli bir oturum açma isteği için birden fazla değeri olan alanlar olamaz. Bu, örneğin, kimlik doğrulama ayrıntıları, koşullu erişim verileri ve ağ konumu için geçerlidir.   

![Oturum açma etkinliği](./media/concept-sign-ins/columns.png "Oturum açma etkinliği")

Daha ayrıntılı bilgi almak için liste görünümünde bir öğe seçin.

![Oturum açma etkinliği](./media/concept-sign-ins/basic-sign-in.png "Oturum açma etkinliği")

> [!NOTE]
> Müşteriler artık tüm oturum açma raporları aracılığıyla Koşullu Erişim ilkelerini sorun giderebilir. Oturum açma kaydı için **Koşullu Erişim** sekmesini tıklatarak, müşteriler Koşullu Erişim durumunu gözden geçirebilir ve oturum açma ve her ilke için uygulanan ilkelerin ayrıntılarına dalabilir.
> Daha fazla bilgi için, [tüm oturum açma oturum açmalarda CA bilgileri hakkında sık sorulan sorulara](reports-faq.md#conditional-access)bakın.



## <a name="filter-sign-in-activities"></a>Oturum açma etkinliklerini filtreleme

İlk olarak, bildirilen verileri sizin için çalışan bir düzeye daraltma. İkinci olarak, varsayılan filtre olarak tarih alanını kullanarak oturum açma verilerini filtreleyin. Azure AD, ayarlayabileceğiniz çok çeşitli ek filtreler sağlar:

![Oturum açma etkinliği](./media/concept-sign-ins/04.png "Oturum açma etkinliği")

**İstek Kimliği** - Önemsediğiniz isteğin kimliği.

**Kullanıcı** - Önemsediğiniz kullanıcının adı veya kullanıcı ana adı (UPN).

**Uygulama** - Hedef uygulamanın adı.
 
**Durum** - Önemsediğiniz oturum açma durumu:

- Başarılı

- Hata

- Kesintiye


**IP adresi** - Kiracınıza bağlanmak için kullanılan cihazın IP adresi.

**Konum** - Bağlantının başlatıldığı konum:

- Şehir

- Eyalet / İl

- Ülke/Bölge


**Kaynak** - Oturum açma için kullanılan hizmetin adı.


**Kaynak Kimliği** - Oturum açma için kullanılan hizmetin kimliği.


**İstemci uygulaması** - Kiracınıza bağlanmak için kullanılan istemci uygulamasının türü:

![İstemci uygulaması filtresi](./media/concept-sign-ins/client-app-filter.png)


|Adı|Modern kimlik doğrulama|Açıklama|
|---|:-:|---|
|Kimlik doğrulaması SMTP| |POP ve IMAP istemcileri tarafından e-posta iletileri göndermek için kullanılır.|
|Autodiscover| |Exchange Online'daki posta kutularını bulmak ve bağlanmak için Outlook ve EAS istemcileri tarafından kullanılır.|
|Exchange ActiveSync| |Bu filtre, EAS protokolünün denendiği tüm oturum açma girişimlerini gösterir.|
|Tarayıcı|![İşaretli](./media/concept-sign-ins/check.png)|Web tarayıcılarını kullanan kullanıcıların tüm oturum açma girişimlerini gösterir|
|Exchange ActiveSync| | Exchange Online'a bağlanmak için Exchange ActiceSync'i kullanan istemci uygulamaları olan kullanıcıların tüm oturum açma girişimlerini gösterir|
|Exchange Online PowerShell| |Uzaktan PowerShell ile Exchange Online'a bağlanmak için kullanılır. Exchange Online PowerShell için temel kimlik doğrulamasını engellerseniz, bağlanmak için Exchange Online PowerShell modülünü kullanmanız gerekir. Talimatlar için, [çok faktörlü kimlik doğrulaması kullanarak Exchange Online PowerShell'e Bağlan'a](https://docs.microsoft.com/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell)bakın.|
|Exchange Web Hizmetleri| |Outlook, Outlook for Mac ve üçüncü taraf uygulamalar tarafından kullanılan bir programlama arabirimi.|
|IMAP4| |E-posta almak için IMAP kullanan eski bir posta istemcisi.|
|MAPI üzerinde HTTP| |Outlook 2010 ve sonrası tarafından kullanılır.|
|Mobil uygulamalar ve masaüstü istemcileri|![İşaretli](./media/concept-sign-ins/check.png)|Mobil uygulamaları ve masaüstü istemcilerini kullanan kullanıcıların tüm oturum açma girişimlerini gösterir.|
|Çevrimdışı Adres Defteri| |Outlook tarafından indirilen ve kullanılan adres listesi koleksiyonlarının bir kopyası.|
|Outlook Anywhere (RPC http üzerinden)| |Outlook 2016 ve daha önceki ler tarafından kullanılır.|
|Outlook Hizmeti| |Windows 10 için Posta ve Takvim uygulaması tarafından kullanılır.|
|POP3| |E-posta almak için POP3 kullanan eski bir posta istemcisi.|
|Web Hizmetlerini Raporlama| |Exchange Online'da rapor verilerini almak için kullanılır.|
|Diğer istemciler| |İstemci uygulamasının dahil olmadığı veya bilinmeyen tüm oturum açma girişimlerini kullanıcılardan gösterir.|



**İşletim sistemi** - Cihazüzerinde çalışan işletim sistemi kiracınıza oturum açma yı kullanır. 


**Aygıt tarayıcısı** - Bağlantı bir tarayıcıdan başlatıldıysa, bu alan tarayıcı adına göre filtre lemenizi sağlar.


**Korelasyon Kimliği** - Etkinliğin korelasyon kimliği.




**Koşullu erişim** - Uygulanan koşullu erişim kurallarının durumu

- **Uygulanmaz**: Oturum açma sırasında kullanıcıya ve uygulamaya uygulanan bir ilke yoktur.

- **Başarı**: Oturum açma sırasında kullanıcıya ve uygulamaya (ancak diğer koşullara bağlı değildir) uygulanan bir veya daha fazla koşullu erişim ilkeleri. 

- **Hata**: Bir veya daha fazla koşullu erişim ilkeleri uygulandı ve oturum açma sırasında tatmin edilmedi.









## <a name="download-sign-in-activities"></a>Oturum açma etkinliklerini indirme

En son 250.000 kaydın csv veya JSON dosyasını oluşturmak için **İndir** seçeneğini tıklatın. Azure portalı dışında çalışmak istiyorsanız [oturum açma verilerini indirin.](quickstart-download-sign-in-report.md)  

![İndir](./media/concept-sign-ins/71.png "İndirme")

> [!IMPORTANT]
> İndirebileceğiniz kayıt sayısı [Azure Etkin Dizin raporu bekletme ilkeleriyle](reference-reports-data-retention.md)sınırlandırılmıştır.  


## <a name="sign-ins-data-shortcuts"></a>Oturum açma veri kısayolları

Azure AD ve Azure portalı, oturum açma verileri için ek giriş noktaları sağlar:

- Kimlik güvenliği koruma genel bakış
- Kullanıcılar
- Gruplar
- Kurumsal uygulamalar

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Kimlik güvenliği korumasında kullanıcılar verileri oturum alar

**Kimlik güvenliği koruma** sayfasına göre kullanıcı oturum açma grafiği, haftalık oturum açma toplamalarını gösterir. Zaman dilimi için varsayılan değer 30 gündür.

![Oturum açma etkinliği](./media/concept-sign-ins/06.png "Oturum açma etkinliği")

Oturum açma grafiğinde bir güne tıkladığınızda, o güne ait oturum açma etkinliklerinin genel bir açıklamasını alırsınız.

Oturum açma etkinlikleri listesinin her satırında aşağıdakiler gösterilir:

* Kim oturum açtı?
* Oturum açmanın hedefi hangi uygulamaydı?
* Oturum açma durumu nedir?
* Oturum açma MFA durumu nedir?

Bir öğeye tıklayarak oturum açma işlemi hakkında daha fazla bilgi alabilirsiniz:

- Kullanıcı Kimliği
- Kullanıcı
- Kullanıcı adı
- Uygulama Kimliği
- Uygulama
- İstemci
- Konum
- IP adresi
- Tarih
- MFA Gerekli
- Oturum açma durumu

> [!NOTE]
> IP adresleri, IP adresi ile bu adrese sahip bilgisayarın fiziksel olarak bulunduğu yer arasında kesin bir bağlantı olmayacak şekilde verilir. IP adreslerinin eşleme, mobil sağlayıcıların ve VPN'lerin genellikle istemci aygıtının gerçekte kullanıldığı yerden çok uzak olan merkezi havuzlardan IP adresleri yayınlaması yla karmaşıktır. Şu anda Azure AD raporlarında, IP adresini fiziksel bir konuma dönüştürmek, izlemelere, kayıt defteri verilerine, ters görünümlere ve diğer bilgilere dayalı en iyi çabadır.

**Kullanıcılar** sayfasında, **Etkinlik** bölümündeki **Oturum açma** öğesine tıklayarak tüm kullanıcı oturum açma işlemlerine eksiksiz bir genel bakış elde edebilirsiniz.

![Oturum açma etkinliği](./media/concept-sign-ins/08.png "Oturum açma etkinliği")

## <a name="usage-of-managed-applications"></a>Yönetilen uygulamaların kullanımı

Oturum açma bilgilerinizin uygulama odaklı bir görünümüyle aşağıdakiler gibi sorular yanıtlanabilir:

* Uygulamalarımı kimler kullanıyor?
* Kuruluşunuzdaki ilk üç uygulama nelerdir?
* En yeni uygulamam nasıl gidiyor?

Bu verilerin giriş noktası, kuruluşunuzdaki en iyi üç uygulamadır. Veriler, **Kurumsal uygulamalar**altında **Genel Bakış** bölümünde son 30 gün raporu içinde yer almaktadır.

![Oturum açma etkinliği](./media/concept-sign-ins/10.png "Oturum açma etkinliği")

Uygulama kullanımı, belirli bir zaman diliminde ilk üç uygulamanız için haftalık oturum açma toplamalarını grafikle gösterir. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/concept-sign-ins/graph-chart.png "Oturum açma etkinliği")

İsterseniz belirli bir uygulamaya odaklanabilirsiniz.

![Raporlama](./media/concept-sign-ins/single-app-usage-graph.png "Raporlama")

Uygulama kullanımı grafiğinde bir güne tıkladığınızda, oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.

**Oturum açma işlemleri** seçeneği, size tüm uygulamalarınıza ait oturum açma olaylarına genel bir bakış sunar.

## <a name="office-365-activity-logs"></a>Office 365 etkinlik günlükleri

[Microsoft 365 yönetici merkezinden Office 365](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center)etkinlik günlüklerini görüntüleyebilirsiniz. Office 365 etkinliği ve Azure REKLAM etkinlik günlüklerinin önemli sayıda dizin kaynaklarını paylaştığı noktayı göz önünde bulundurun. Yalnızca Microsoft 365 yönetici merkezi, Office 365 etkinlik günlüklerinin tam görünümünü sağlar. 

Ayrıca, [Office 365 Yönetim API'lerini](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview)kullanarak Office 365 etkinlik günlüklerine programlı olarak erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma faaliyet raporu hata kodları](reference-sign-ins-error-codes.md)
* [Azure AD veri saklama ilkeleri](reference-reports-data-retention.md)
* [Azure AD raporu gecikmeleri](reference-reports-latencies.md)

