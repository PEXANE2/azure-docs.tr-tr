---
title: ServiceNow 'ı BT Hizmet Yönetimi Bağlayıcısı bağlama
description: Bu makalede, ıTSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile ServiceNow hakkında bilgi sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 662c36e4f0082c376a6e250e9a0885f0cd225964
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97729725"
---
# <a name="connect-servicenow-with-it-service-management-connector"></a>ServiceNow 'ı BT Hizmet Yönetimi Bağlayıcısı bağlama

Bu makalede, iş öğelerinizi merkezi olarak yönetmek için Log Analytics ServiceNow örneğiniz ile BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantının nasıl yapılandırılacağı hakkında bilgi sağlanır.

Aşağıdaki bölümlerde, ServiceNow ürününüzü Azure 'daki ıSMC 'a bağlama hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların karşılandığından emin olun:
- ISMC yüklendi. Daha fazla bilgi: [BT hizmet yönetimi Bağlayıcısı çözümü ekleme](./itsmc-definition.md#add-it-service-management-connector).
- ServiceNow desteklenen sürümler: Orlando, New York, Madrid, Londra, Kingston, Jakarta, Istanbul, Helsinki, Genfiliz.
- Bugün Azure Izleyici 'den gönderilen uyarılar ServiceNow içinde şu öğelerden birini oluşturabilir: olaylar, olaylar veya uyarılar.
> [!NOTE]
> ISMC, şimdi hizmetten yalnızca resmi SaaS teklifini destekler. Hizmetin özel dağıtımları artık desteklenmiyor. 

**ServiceNow yöneticilerinin ServiceNow örneğinde aşağıdakileri yapması gerekir**:
- ServiceNow ürünü için istemci KIMLIĞI ve istemci gizli dizisi oluşturun. İstemci KIMLIĞI ve gizli dizi oluşturma hakkında bilgi için, aşağıdaki bilgileri gerekli şekilde inceleyin:

    - [Orlando için OAuth ayarla](https://docs.servicenow.com/bundle/orlando-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [New York için OAuth ayarlama](https://docs.servicenow.com/bundle/newyork-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Madrid için OAuth ayarlama](https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Londra için OAuth ayarlama](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Kingston için OAuth ayarla](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Cakarta için OAuth ayarlama](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Istanbul için OAuth ayarlama](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Helsinki için OAuth ayarlama](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Genfiliz için OAuth ayarlama](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)
> [!NOTE]
> "OAuth ayarla" tanımının bir parçası olarak şunları öneririz:
>
> 1) **Yenileme belirtecini 90 gün (7.776.000 saniye) olarak güncelleştirin:** 2. aşama 'da [set up OAuth](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_SettingUpOAuth.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696739125&sdata=Q7mF6Ej8MCupKaEJpabTM56EDZ1T8vFVyihhoM594aA%3D&reserved=0) 'un bir parçası olarak: bir uç noktanın tanımından sonra, örneğin, ServiceNow dikey penceresinde, sistem OAuth ' ın SELECT Application Registry ' yi aramak için [bir uç nokta oluşturun](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_CreateEndpointforExternalClients.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696749123&sdata=hoAJHJAFgUeszYCX1Q%2FXr4N%2FAKiFcm5WV7mwR2UqeWA%3D&reserved=0) . Tanımlanan OAuth adını seçin ve yenileme belirteci alanını 7.776.000 olarak güncelleştirin (saniye cinsinden 90 gün).
> Son tıklama Güncelleştir ' e tıklayın.
> 2) **Bağlantının canlı kalmasını sağlamak için bir iç yordam oluşturmanız önerilir:** Belirteci yenilemek için kullanım belirtecini yenileme belirtecine göre. Lütfen yenileme belirtecinin ön bitiş saati (yenileme belirtecinin süresi dolmadan kaç gün önce önerilir) için önce aşağıdaki işlemleri gerçekleştirdiğinizden emin olun:
>
>     1. [ITSM Bağlayıcısı yapılandırması için el ile eşitleme işlemini tamaml](./itsmc-resync-servicenow.md)
>     2. Eski yenileme belirtecine geri dönmek için eski anahtarların güvenlik nedenlerinden haberdar olması önerilmez. ServiceNow dikey penceresinde sistem OAuth için arama belirteçleri Yönet ' i seçin. OAuth adı ve sona erme tarihine göre listeden eski belirteci seçin.
> ![KAR sistemi OAuth tanımı](media/itsmc-connections/snow-system-oauth.png)
>     3. Erişimi Iptal et ' e tıklayın.

- Microsoft Log Analytics Integration (ServiceNow uygulaması) için Kullanıcı uygulamasını yükler. [Daha fazla bilgi edinin](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
> [!NOTE]
> ISMC, ServiceNow mağazasından indirilen Microsoft Log Analytics tümleştirmesi için yalnızca resmi Kullanıcı uygulamasını destekler. ISMC, ServiceNow tarafında veya resmi ServiceNow çözümünün parçası olmayan uygulamada herhangi bir kod alımı desteklemez. 
- Yüklü kullanıcı uygulaması için tümleştirme Kullanıcı rolü oluşturun. Tümleştirme Kullanıcı rolünü oluşturma hakkında bilgi [burada](#create-integration-user-role-in-servicenow-app)verilmiştir.

## <a name="connection-procedure"></a>**Bağlantı yordamı**
Bir ServiceNow bağlantısı oluşturmak için aşağıdaki yordamı kullanın:


1. Azure portal ' de **tüm kaynaklar** ' a gidin ve **ServiceDesk (yourçalışmaalanıadı)** araması yapın

2.  **Çalışma alanı VERI kaynakları** altında **ITSM bağlantıları**' na tıklayın.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmındaki **Ekle**' ye tıklayın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam** ' ı tıklatın.


> [!NOTE]
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı adı**   | ISMC ile bağlanmak istediğiniz ServiceNow örneği için bir ad yazın.  Bu ıTSM 'de iş öğelerini yapılandırdığınızda ve ayrıntılı Log Analytics 'i görüntülemek için bu adı daha sonra Log Analytics ' de kullanırsınız. |
| **İş ortağı türü**   | **ServiceNow**' ı seçin. |
| **Kullanıcı adı**   | ISMC bağlantısını desteklemek için ServiceNow uygulamasında oluşturduğunuz tümleştirme Kullanıcı adını yazın. Daha fazla bilgi: [ServiceNow uygulaması kullanıcı rolü oluşturun](#create-integration-user-role-in-servicenow-app).|
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not**: Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ısmc hizmetinin içinde herhangi bir yerde depolanmaz.  |
| **Sunucu URL 'SI**   | ISMC 'a bağlamak istediğiniz ServiceNow örneğinin URL 'sini yazın. URL, ". servicenow.com" sonekiyle desteklenen bir SaaS sürümüne işaret etmelidir.|
| **İstemci Kimliği**   | Daha önce oluşturduğunuz OAuth2 kimlik doğrulaması için kullanmak istediğiniz istemci KIMLIĞINI yazın.  İstemci KIMLIĞI ve gizli anahtar oluşturma hakkında daha fazla bilgi:   [OAuth kurulumu](https://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **İstemci parolası**   | Bu KIMLIK için oluşturulan istemci parolasını yazın.   |
| **Veri eşitleme kapsamı**   | ISMC aracılığıyla Azure Log Analytics eşitlemek istediğiniz ServiceNow iş öğelerini seçin.  Seçilen değerler Log Analytics 'e aktarılır.   **Seçenekler:**  Olaylar ve değişiklik Istekleri.|
| **Verileri eşitleme** | Verilerin içinden istediğiniz geçmiş gün sayısını yazın. **Maksimum sınır**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluştur** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, ıSMC, desteklenen ıTSM sisteminde, etkilenen CIS 'yi yapılandırma öğeleri (mevcut olmayan CIS olması durumunda) olarak oluşturur. **Varsayılan**: devre dışı. |

![ServiceNow bağlantısı](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Başarıyla bağlandığında ve eşitlendiğinde**:

- ServiceNow örneğinden seçilen iş öğeleri Azure Log Analytics içeri aktarılmalıdır **.** BT Hizmet Yönetimi Bağlayıcısı kutucuğunda bu iş öğelerinin özetini görüntüleyebilirsiniz.

- Log Analytics uyarılarından veya günlük kayıtlarından ya da bu ServiceNow örneğindeki Azure uyarılarından olaylar oluşturabilirsiniz.

> [!NOTE]
> ServiceNow 'da saat başına istekler için bir hız sınırı vardır. Limiti yapılandırmak için ServiceNow örneğinde "gelen REST API hız sınırlaması" tanımlayarak bunu kullanın.

## <a name="create-integration-user-role-in-servicenow-app"></a>ServiceNow uygulaması 'nda tümleştirme Kullanıcı rolü oluşturma

Aşağıdaki yordamı Kullanıcı:

1. ServiceNow [mağazasını](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) ziyaret edin ve ServiceNow örneğiniz **Için Kullanıcı uygulamasını ServiceNow ve Microsoft OMS tümleştirmesi için** yüklemeyi yapın.
   
   >[!NOTE]
   >Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS artık Log Analytics olarak adlandırılır.     
2. Yüklemeden sonra ServiceNow örneğinin sol gezinti çubuğunu ziyaret edin, arama yapın ve Microsoft OMS tümleştirici ' i seçin.  
3. **Yükleme denetim listesi**' ne tıklayın.

   Kullanıcı rolü henüz oluşturulmadıysa durum  **tamamlanmamış** olarak görüntülenir.

4. Metin kutularında, **tümleştirme kullanıcısı oluştur**' un yanındaki Azure 'DA ısmc 'a bağlanabilecek kullanıcı için Kullanıcı adını girin.
5. Bu kullanıcının parolasını girin ve **Tamam**' a tıklayın.  

> [!NOTE]
> Azure 'da ServiceNow bağlantısı kurmak için bu kimlik bilgilerini kullanırsınız.

Yeni oluşturulan kullanıcı varsayılan rollerle birlikte görüntülenir.

**Varsayılan roller**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft. Kullanıcı
-   ITIL
-   template_editor
-   view_changer

Kullanıcı başarılı bir şekilde oluşturulduktan sonra, **yükleme denetim listesinin** durumu tamamlandı olarak getirilir ve uygulama için oluşturulan kullanıcı rolünün ayrıntıları denetlenir.

> [!NOTE]
> ITSM Bağlayıcısı, ServiceNow Örneğinizde yüklü başka modüller olmadan ServiceNow 'a olay gönderebilir. ServiceNow Örneğinizde EventManagement modülünü kullanıyorsanız ve bağlayıcıyı kullanarak ServiceNow 'da olay veya uyarı oluşturmak istiyorsanız, tümleştirme kullanıcısına aşağıdaki rolleri ekleyin:
> 
>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısı genel bakış](itsmc-overview.md)
* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)