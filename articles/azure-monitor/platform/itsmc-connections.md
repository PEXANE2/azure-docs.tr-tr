---
title: Azure Monitör'de BT Hizmet Yönetimi Bağlayıcısı
description: Bu makalede, ITSM ürün/hizmetlerinizi, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Monitor'daki BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile nasıl bağlayabileceğiniz hakkında bilgiler verilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.openlocfilehash: 9441e7bb970508df4c002897ab726d6e683fa848
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733355"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>ITSM ürünlerini/hizmetlerini BT Servis Yönetimi Konektörü ile bağlayın
Bu makalede, iş öğelerinizi merkezi olarak yönetmek için LOG Analytics'teki ITSM ürünün/hizmetinizle BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantının nasıl yapılandırılabildiğini zedebilirsiniz. ITSMC hakkında daha fazla bilgi için [genel bakış](../../azure-monitor/platform/itsmc-overview.md)adabına bakın.

Aşağıdaki ITSM ürünleri/hizmetleri desteklenir. Ürünü ITSMC'ye nasıl bağlayacaklarınız hakkında ayrıntılı bilgileri görüntülemek için ürünü seçin.

- [Sistem Merkezi Servis Müdürü](#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](#connect-provance-to-it-service-management-connector-in-azure)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-azure)

> [!NOTE]
> 
> ITSM Bağlayıcısı yalnızca bulut tabanlı ServiceNow örneklerine bağlanabilir. Şirket içi ServiceNow örnekleri şu anda desteklenmemektedir.

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-azure"></a>Sistem Merkezi Servis Yöneticisini Azure'da BT Hizmet Yönetimi Bağlayıcısına Bağlayın

Aşağıdaki bölümlerde, Sistem Merkezi Servis Yöneticisi ürününüzün Azure'da ITSMC'ye nasıl bağlanılabildiğini zedebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşulların karşılandığından emin olun:

- ITSMC yüklendi. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcı Çözümü ekleme.](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution)
- Service Manager Web uygulaması (Web uygulaması) dağıtılır ve yapılandırılır. Web uygulaması hakkında bilgi [burada.](#create-and-deploy-service-manager-web-app-service)
- Karma bağlantı oluşturuldu ve yapılandırıldı. Daha fazla bilgi: [Karma Bağlantıyı yapılandırın.](#configure-the-hybrid-connection)
- Service Manager'ın desteklenen sürümleri: 2012 R2 veya 2016.
- Kullanıcı rolü: [Gelişmiş operatör](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Bağlantı yordamı

Sistem Merkezi Servis Yöneticisi örneğini ITSMC'ye bağlamak için aşağıdaki yordamı kullanın:

1. Azure portalında **Tüm Kaynaklar'a** gidin ve **ServiceDesk'i arayın(YourWorkspaceName)**

2.  **WORKSPACE DATA SOURCES** altında **ITSM Bağlantıları'nı**tıklatın.

    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmında **Ekle'yi**tıklatın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam'ı** tıklatın.

> [!NOTE]
> 
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC'ye bağlanmak istediğiniz Sistem Merkezi Servis Yöneticisi örneği için bir ad yazın.  Bu örnekte iş öğelerini yapılandırırken/ ayrıntılı günlük analitiğini görüntülediğinizde daha sonra bu adı kullanırsınız. |
| **İş ortağı türü**   | **Sistem Merkezi Servis Yöneticisi'ni**seçin. |
| **Sunucu URL'si**   | Service Manager Web uygulamasının URL'sini yazın. Service Manager Web uygulaması hakkında daha fazla bilgi [burada.](#create-and-deploy-service-manager-web-app-service)
| **İstemci Kimliği**   | Web uygulamasının kimliğini doğrulamak için oluşturduğunuz istemci kimliğini (otomatik komut dosyasını kullanarak) yazın. Otomatik komut dosyası hakkında daha fazla bilgi [burada.](../../azure-monitor/platform/itsmc-service-manager-script.md)|
| **Müşteri Sırrı**   | Bu kimlik için oluşturulan istemci sırrını yazın.   |
| **Verileri Eşitle**   | ITSMC aracılığıyla eşitlemek istediğiniz Servis Yöneticisi çalışma öğelerini seçin.  Bu iş öğeleri Log Analytics'e aktarılır. **Seçenekler:**  Olaylar, Değişiklik İstekleri.|
| **Veri Eşitleme Kapsamı** | Verileri istediğiniz geçmiş gün sayısını yazın. **Maksimum limit**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerioluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, Log Analytics, desteklenen ITSM sisteminde etkilenen CIs'leri yapılandırma öğesi olarak (mevcut olmayan CIs'ler durumunda) oluşturur. **Varsayılan**: devre dışı. |

![Servis yöneticisi bağlantısı](media/itsmc-connections/service-manager-connection.png)

**Başarıyla bağlandığında ve senkronize edildiğinde:**

- Service Manager'dan seçilen çalışma öğeleri Azure **Günlük Analizi'ne aktarılır.** Bu çalışma öğelerinin özetini BT Hizmet Yönetimi Bağlayıcısı döşemesinde görüntüleyebilirsiniz.

- Bu Service Manager örneğinde Log Analytics uyarılarından veya günlük kayıtlarından veya Azure uyarılarından olaylar oluşturabilirsiniz.


Daha fazla bilgi: [Azure uyarılarından ITSM iş öğeleri oluşturun.](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)

### <a name="create-and-deploy-service-manager-web-app-service"></a>Service Manager web uygulaması hizmeti oluşturma ve dağıtma

Şirket içi Service Manager'ı Azure'daki ITSMC'ye bağlamak için Microsoft, GitHub'da bir Service Manager Web uygulaması oluşturdu.

Service Manager'ınız için ITSM Web uygulamasını ayarlamak için aşağıdakileri yapın:

- **Web uygulamasını dağıtın** – Web uygulamasını dağıtın, özellikleri ayarlayın ve Azure AD ile kimlik doğrulaması ayarlayın. Microsoft'un size sağladığı [otomatik komut dosyasını](../../azure-monitor/platform/itsmc-service-manager-script.md) kullanarak web uygulamasını dağıtabilirsiniz.
- **Karma bağlantıyı** - [yapılandırın Bu bağlantıyı](#configure-the-hybrid-connection)el ile yapılandırın.

#### <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma
Web uygulamasını dağıtmak, özellikleri ayarlamak ve Azure AD ile kimlik doğrulaması yapmak için otomatik [komut dosyasını](../../azure-monitor/platform/itsmc-service-manager-script.md) kullanın.

Aşağıdaki gerekli ayrıntıları sağlayarak komut dosyasını çalıştırın:

- Azure abonelik ayrıntıları
- Kaynak grubu adı
- Konum
- Service Manager sunucu ayrıntıları (sunucu adı, etki alanı, kullanıcı adı ve parola)
- Web uygulamanız için site adı öneki
- ServiceBus İsim Alanı.

Komut dosyası, belirttiğiniz adı kullanarak Web uygulamasını oluşturur (benzersiz hale getirmek için birkaç ek dizeyle birlikte). Bu Web **uygulaması URL,** **istemci kimliği** ve **istemci gizli**oluşturur.

Değerleri kaydedin, ITSMC ile bağlantı oluştururken bunları kullanın.

**Web uygulaması yüklemesini kontrol edin**

1. Azure **portalı** > **Kaynakları'na**gidin.
2. Web uygulamasını seçin, **Ayarlar** > **Uygulama Ayarları'nı**tıklatın.
3. Uygulamayı komut dosyası üzerinden dağıtığınızda sağladığınız Servis Yöneticisi örneği hakkındaki bilgileri doğrulayın.

### <a name="configure-the-hybrid-connection"></a>Karma bağlantıyı yapılandırma

Azure'da ITSMC ile Servis Yöneticisi örneğini bağlayan karma bağlantıyı yapılandırmak için aşağıdaki yordamı kullanın.

1. **Azure Kaynakları**altında Service Manager Web uygulamasını bulun.
2. **Ayarlar** > **Ağ'ı**tıklatın.
3. **Karma Bağlantılar**altında, karma bağlantı **uç noktalarınızı yapılandır'ı**tıklatın.

    ![Karma bağlantı ağı](media/itsmc-connections/itsmc-hybrid-connection-networking-and-end-points.png)
4. Karma **Bağlantılar** bıyışlarında, **karma bağlantı ekle'yi**tıklatın.

    ![Karma bağlantı ekleme](media/itsmc-connections/itsmc-new-hybrid-connection-add.png)

5. Karma **Bağlantı Ekle** çubuğunda, **yeni karma bağlantı oluştur'u**tıklatın.

    ![Yeni Karma bağlantı](media/itsmc-connections/itsmc-create-new-hybrid-connection.png)

6. Aşağıdaki değerleri yazın:

   - **EndPoint Adı**: Yeni Karma bağlantı için bir ad belirtin.
   - **EndPoint Host**: Service Manager yönetim sunucusunun FQDN'si.
   - **EndPoint Bağlantı Noktası**: Type 5724
   - **Servicebus namespace**: Varolan bir servicebus ad alanı kullanın veya yeni bir tane oluşturun.
   - **Konum**: konumu seçin.
   - **Adı**: Oluşturuyorsanız servicebus'a bir ad belirtin.

     ![Karma bağlantı değerleri](media/itsmc-connections/itsmc-new-hybrid-connection-values.png)
6. Karma bağlantı bıçağı **oluştur'u** kapatmak ve karma bağlantı oluşturmaya başlamak için **Tamam'ı** tıklatın.

    Karma bağlantı oluşturulduktan sonra, bıçağın altında görüntülenir.

7. Karma bağlantı oluşturulduktan sonra bağlantıyı seçin ve **seçili karma bağlantı ekle'yi**tıklatın.

    ![Yeni karma bağlantı](media/itsmc-connections/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>Dinleyici kurulumını yapılandırma

Karma bağlantı için dinleyici kurulumünü yapılandırmak için aşağıdaki yordamı kullanın.

1. Karma **Bağlantılar** bıtır'da, **Bağlantı Yöneticisini İndir'i** ve Sistem Merkezi Servis Yöneticisi örneğinin çalıştığı makineye yükleyin'i tıklatın.

    Yükleme tamamlandıktan sonra **Başlat** menüsü altında **Karma Bağlantı Yöneticisi UI** seçeneği kullanılabilir.

2. **Karma Bağlantı Yöneticisi UI'yi** tıklatın, Azure kimlik bilgileriniz istenir.

3. Azure kimlik bilgilerinizle giriş yapın ve Karma bağlantının oluşturulduğu aboneliğinizi seçin.

4. **Kaydet**’e tıklayın.

Karma bağlantınız başarıyla bağlandı.

![başarılı hibrit bağlantı](media/itsmc-connections/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]
> 
> Karma bağlantı oluşturulduktan sonra, dağıtılan Service Manager Web uygulamasını ziyaret ederek bağlantıyı doğrulayın ve test edin. Azure'da ITSMC'ye bağlanmaya çalışmadan önce bağlantının başarılı olduğundan emin olun.

Aşağıdaki örnek resim, başarılı bir bağlantının ayrıntılarını gösterir:

![Karma bağlantı testi](media/itsmc-connections/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-azure"></a>ServiceNow'u Azure'da BT Hizmet Yönetimi Bağlayıcısına Bağlayın

Aşağıdaki bölümlerde ServiceNow ürününüzüAzure'da ITSMC'ye nasıl bağlayabileceğiniz hakkında ayrıntılı bilgi verilmiştir.

### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki ön koşulların karşılandığından emin olun:
- ITSMC yüklendi. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcı Çözümü ekleme.](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution)
- ServiceNow desteklenen sürümleri: New York, Madrid, Londra, Kingston, Jakarta, İstanbul, Helsinki, Cenevre.
> [!NOTE]
> ITSMC, Yalnızca Service Now'ın resmi SaaS teklifini destekler. Hizmet Şimdi'nin özel dağıtımları desteklenmez. 

**ServiceNow Yöneticileri ServiceNow örneğinde aşağıdakileri yapmalıdır:**
- ServiceNow ürünü için istemci kimliği ve istemci sırrı oluşturun. İstemci kimliği ve gizli nasıl oluşturacağı hakkında bilgi için aşağıdaki bilgileri gerektiği gibi görün:

    - [New York için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/newyork-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Madrid için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Londra için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Kingston için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Jakarta için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [İstanbul için OAuth'u kur](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Helsinki için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Cenevre için OAuth'u ayarlayın](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)
> [!NOTE]
> "OAuth'u Kur" tanımının bir parçası olarak şunları öneririz:
>
> 1) **Yenileme belirteci ömrünü 90 güne (7.776.000 saniye) güncelleştirin:** Aşama 2'de [OAuth'u Ayarlama'nın](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_SettingUpOAuth.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696739125&sdata=Q7mF6Ej8MCupKaEJpabTM56EDZ1T8vFVyihhoM594aA%3D&reserved=0) bir parçası olarak: Son noktanın tanımından sonra [istemcilerin örne erişmeleri](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_CreateEndpointforExternalClients.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696749123&sdata=hoAJHJAFgUeszYCX1Q%2FXr4N%2FAKiFcm5WV7mwR2UqeWA%3D&reserved=0) için bir bitiş noktası oluşturun, ServiceNow bıçak aramasisteminde System OAuth->Application Registry için tanımlanan OAuth'un adını seçin ve "Belirteç Ömrünü Tazele" alanını 7.776.000'e güncelleyin.
> Sonunda tıklayın güncelleştirme.
> 2) **Bağlantının canlı kalmasını sağlamak için bir dahili yordam oluşturmanızı öneririz:** Belirteci yenilemek için "Token Ömrünü Yenile"ye göre. Lütfen belirteç beklenen son kullanma süresini yenilemeden önce aşağıdaki işlemleri gerçekleştirdiğinden emin olun ("Token Ömrünü Yenile" süresinin sona ermesinden birkaç gün önce tavsiye ediyoruz):
>
>>  1) [ITSM konektör yapılandırması için manuel eşitleme işlemini tamamlama](https://docs.microsoft.com/azure/azure-monitor/platform/itsmc-resync-servicenow)
 >> 2) Eski anahtarları güvenlik nedeniyle uzak tutmanın önerilmemesi nedeniyle eski yenileme belirteci iptal edin. ServiceNow bıçak arama "System OAuth"-> "Jetonları yönet" ve sonra OAuth adı ve son kullanma tarihine göre listeden eski belirteç seçin. "Erişimi İptal Et"->"İptal et.

- Microsoft Log Analytics tümleştirmesi (ServiceNow uygulaması) için Kullanıcı Uygulamasını yükleyin. [Daha fazla bilgi edinin](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
- Yüklü kullanıcı uygulaması için tümleştirme kullanıcı rolü oluşturun. Entegrasyon kullanıcı rolünün nasıl oluşturulacığı ile ilgili bilgiler [burada.](#create-integration-user-role-in-servicenow-app)

### <a name="connection-procedure"></a>**Bağlantı yordamı**
ServiceNow bağlantısı oluşturmak için aşağıdaki yordamı kullanın:


1. Azure portalında **Tüm Kaynaklar'a** gidin ve **ServiceDesk'i arayın(YourWorkspaceName)**

2.  **WORKSPACE DATA SOURCES** altında **ITSM Bağlantıları'nı**tıklatın.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmında **Ekle'yi**tıklatın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam'ı** tıklatın.


> [!NOTE]
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC'ye bağlanmak istediğiniz ServiceNow örneği için bir ad yazın.  Bu ITSM/view ayrıntılı günlük analitiğinde iş öğelerini yapılandırırken bu adı daha sonra Log Analytics'te kullanırsınız. |
| **İş ortağı türü**   | **ServiceNow'u**seçin. |
| **Username**   | ITSMC bağlantısını desteklemek için ServiceNow uygulamasında oluşturduğunuz tümleştirme kullanıcı adını yazın. Daha fazla bilgi: [ServiceNow uygulaması kullanıcı rolü oluşturun.](#create-integration-user-role-in-servicenow-app)|
| **Parola**   | Bu kullanıcı adı ile ilişkili parolayı yazın. **Not**: Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ITSMC hizmeti içinde herhangi bir yerde depolanmaz.  |
| **Sunucu URL'si**   | ITSMC'ye bağlanmak istediğiniz ServiceNow örneğinin URL'sini yazın. URL, ".servicenow.com" ekiyle desteklenen bir SaaS sürümüne işaret etmelidir.|
| **İstemci Kimliği**   | Daha önce oluşturduğunuz OAuth2 Kimlik Doğrulaması için kullanmak istediğiniz istemci kimliğini yazın.  İstemci kimliği ve gizli oluşturma hakkında daha fazla bilgi: [OAuth Kurulum](https://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Müşteri Sırrı**   | Bu kimlik için oluşturulan istemci sırrını yazın.   |
| **Veri Eşitleme Kapsamı**   | ITSMC aracılığıyla Azure Günlük Analizi ile eşitlemek istediğiniz ServiceNow iş öğelerini seçin.  Seçili değerler günlük analitiğine aktarılır.   **Seçenekler:**  Olaylar ve Değişiklik İstekleri.|
| **Verileri Eşitle** | Verileri istediğiniz geçmiş gün sayısını yazın. **Maksimum limit**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerioluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, ITSMC desteklenen ITSM sisteminde konfigürasyon öğeleri (mevcut olmayan CIs'ler durumunda) olarak etkilenen CIs'leri oluşturur. **Varsayılan**: devre dışı. |

![ServiceNow bağlantısı](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Başarıyla bağlandığında ve senkronize edildiğinde:**

- ServiceNow örneğinden seçilen çalışma öğeleri Azure **Günlük Analizi'ne aktarılır.** Bu çalışma öğelerinin özetini BT Hizmet Yönetimi Bağlayıcısı döşemesinde görüntüleyebilirsiniz.

- Log Analytics uyarılarından veya günlük kayıtlarından veya bu ServiceNow örneğindeki Azure uyarılarından olaylar oluşturabilirsiniz.

Daha fazla bilgi: [Azure uyarılarından ITSM iş öğeleri oluşturun.](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)

### <a name="create-integration-user-role-in-servicenow-app"></a>ServiceNow uygulamasında tümleştirme kullanıcı sıfatı oluşturma

Kullanıcı aşağıdaki yordam:

1. [ServiceNow mağazasını](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) ziyaret edin ve **ServiceNow ve Microsoft OMS Tümleştirmesi için Kullanıcı Uygulamasını ServiceNow** Örneğinize yükleyin.
   
   >[!NOTE]
   >Microsoft Operations Management Suite'ten (OMS) Azure Monitor'a devam eden geçişin bir parçası olarak, OMS artık Log Analytics olarak anılıyor.     
2. Yüklemeden sonra ServiceNow örneğinin sol gezinti çubuğunu ziyaret edin, arama yapın ve Microsoft OMS entegratörü seçin.  
3. **Yükleme Denetim Listesi'ni**tıklatın.

   Kullanıcı rolü henüz oluşturulmazsa durum **tam değil** olarak görüntülenir.

4. **Tümleştirme oluştur kullanıcısının**yanındaki metin kutularında, Azure'da ITSMC'ye bağlanabilen kullanıcının kullanıcı adını girin.
5. Bu kullanıcı için parolayı girin ve **Tamam'ı**tıklatın.  

> [!NOTE]
> 
> Bu kimlik bilgilerini Azure'da ServiceNow bağlantısını yapmak için kullanırsınız.

Yeni oluşturulan kullanıcı varsayılan roller atanmış görüntülenir.

**Varsayılan roller:**
- personalize_choices
- import_transformer
-   x_mioms_microsoft.kullanıcı
-   ıtıl
-   template_editor
-   view_changer

Kullanıcı başarıyla oluşturulduktan sonra, **Yükleme yi Denetle Denetim Listesi'nin** durumu Tamamlandı'ya taşınır ve uygulama için oluşturulan kullanıcı rolünün ayrıntılarını listeler.

> [!NOTE]
> 
> ITSM Konektörü, ServiceNow örneğinize başka modüller yüklenmeden olayları ServiceNow'a gönderebilir. ServiceNow örneğinde EventManagement modülünü kullanıyorsanız ve bağlayıcıyı kullanarak ServiceNow'da Etkinlikler veya Uyarılar oluşturmak istiyorsanız, entegrasyon kullanıcısına aşağıdaki rolleri ekleyin:
> 
>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-azure"></a>Azure'da Provance'ı BT Hizmet Yönetimi Bağlayıcısına Bağlayın

Aşağıdaki bölümlerde Provance ürününüzüAzure'da ITSMC'ye nasıl bağlayabileceğiniz hakkında ayrıntılı bilgi verilmiştir.


### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşulların karşılandığından emin olun:


- ITSMC yüklendi. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcı Çözümü ekleme.](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution)
- Provance App Azure AD'ye kaydedilmelidir ve istemci kimliği kullanılabilir hale getirilir. Ayrıntılı bilgi [için, etkin dizin kimlik doğrulaması nasıl yapılandırılabildiğini](../../app-service/configure-authentication-provider-aad.md)görün.

- Kullanıcı rolü: Yönetici.

### <a name="connection-procedure"></a>Bağlantı Prosedürü

Provance bağlantısı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portalında **Tüm Kaynaklar'a** gidin ve **ServiceDesk'i arayın(YourWorkspaceName)**

2.  **WORKSPACE DATA SOURCES** altında **ITSM Bağlantıları'nı**tıklatın.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmında **Ekle'yi**tıklatın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam'ı** tıklatın.

> [!NOTE]
> 
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC'ye bağlanmak istediğiniz Provance örneği için bir ad yazın.  Bu ITSM/view ayrıntılı günlük analizinde iş öğelerini yapılandırırken bu adı daha sonra kullanırsınız. |
| **İş ortağı türü**   | **Provance'ı**seçin. |
| **Username**   | ITSMC'ye bağlanabilen kullanıcı adını yazın.    |
| **Parola**   | Bu kullanıcı adı ile ilişkili parolayı yazın. **Not:** Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ITSMC hizmeti içinde herhangi bir yerde depolanmaz._|
| **Sunucu URL'si**   | ITSMC'ye bağlanmak istediğiniz Provance örneğinizin URL'sini yazın. |
| **İstemci Kimliği**   | Provance örneğinde oluşturduğunuz bu bağlantının kimliğini doğrulamak için istemci kimliğini yazın.  İstemci kimliği hakkında daha fazla bilgi, [etkin dizin kimlik doğrulaması yapılandırmak için nasıl](../../app-service/configure-authentication-provider-aad.md)bakın. |
| **Veri Eşitleme Kapsamı**   | ITSMC aracılığıyla Azure Günlük Analizi ile eşitlemek istediğiniz Provance iş öğelerini seçin.  Bu iş öğeleri günlük analitiğine aktarılır.   **Seçenekler:**   Olaylar, Değişiklik İstekleri.|
| **Verileri Eşitle** | Verileri istediğiniz geçmiş gün sayısını yazın. **Maksimum limit**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerioluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, ITSMC desteklenen ITSM sisteminde konfigürasyon öğeleri (mevcut olmayan CIs'ler durumunda) olarak etkilenen CIs'leri oluşturur. **Varsayılan**: devre dışı.|

![Provance bağlantısı](media/itsmc-connections/itsm-connections-provance-latest.png)

**Başarıyla bağlandığında ve senkronize edildiğinde:**

- Bu Provance örneğinden seçilen çalışma öğeleri Azure **Günlük Analizi'ne aktarılır.** Bu çalışma öğelerinin özetini BT Hizmet Yönetimi Bağlayıcısı döşemesinde görüntüleyebilirsiniz.

- Bu Provance örneğinde Log Analytics uyarılarından veya günlük kayıtlarından veya Azure uyarılarından olaylar oluşturabilirsiniz.

Daha fazla bilgi: [Azure uyarılarından ITSM iş öğeleri oluşturun.](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)

## <a name="connect-cherwell-to-it-service-management-connector-in-azure"></a>Azure'da Cherwell'i BT Hizmet Yönetimi Bağlayıcısına Bağlayın

Aşağıdaki bölümlerde Cherwell ürününüzüAzure'da ITSMC'ye nasıl bağlayabileceğiniz hakkında ayrıntılı bilgi verilmiştir.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşulların karşılandığından emin olun:

- ITSMC yüklendi. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcı Çözümü ekleme.](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution)
- İstemci kimliği oluşturuldu. Daha fazla bilgi: [Cherwell için istemci kimliği oluşturun.](#generate-client-id-for-cherwell)
- Kullanıcı rolü: Yönetici.

### <a name="connection-procedure"></a>Bağlantı Prosedürü

Provance bağlantısı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portalında **Tüm Kaynaklar'a** gidin ve **ServiceDesk'i arayın(YourWorkspaceName)**

2.  **WORKSPACE DATA SOURCES** altında **ITSM Bağlantıları'nı**tıklatın.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmında **Ekle'yi**tıklatın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam'ı** tıklatın.

> [!NOTE]
> 
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC'ye bağlanmak istediğiniz Cherwell örneği için bir ad yazın.  Bu ITSM/view ayrıntılı günlük analizinde iş öğelerini yapılandırırken bu adı daha sonra kullanırsınız. |
| **İş ortağı türü**   | **Cherwell'i seç.** |
| **Username**   | ITSMC'ye bağlanabilen Cherwell kullanıcı adını yazın. |
| **Parola**   | Bu kullanıcı adı ile ilişkili parolayı yazın. **Not:** Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ITSMC hizmeti içinde herhangi bir yerde depolanmaz.|
| **Sunucu URL'si**   | ITSMC'ye bağlanmak istediğiniz Cherwell örneğinizin URL'sini yazın. |
| **İstemci Kimliği**   | Cherwell örneğinde oluşturduğunuz bu bağlantının kimliğini doğrulamak için istemci kimliğini yazın.   |
| **Veri Eşitleme Kapsamı**   | ITSMC ile eşitlemek istediğiniz Cherwell iş öğelerini seçin.  Bu iş öğeleri günlük analitiğine aktarılır.   **Seçenekler:**  Olaylar, Değişiklik İstekleri. |
| **Verileri Eşitle** | Verileri istediğiniz geçmiş gün sayısını yazın. **Maksimum limit**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerioluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, ITSMC desteklenen ITSM sisteminde konfigürasyon öğeleri (mevcut olmayan CIs'ler durumunda) olarak etkilenen CIs'leri oluşturur. **Varsayılan**: devre dışı. |


![Provance bağlantısı](media/itsmc-connections/itsm-connections-cherwell-latest.png)

**Başarıyla bağlandığında ve senkronize edildiğinde:**

- Bu Cherwell örneğinden seçilen çalışma öğeleri Azure **Günlük Analizi'ne aktarılır.** Bu çalışma öğelerinin özetini BT Hizmet Yönetimi Bağlayıcısı döşemesinde görüntüleyebilirsiniz.

- Bu Cherwell örneğinde Log Analytics uyarılarından veya günlük kayıtlarından veya Azure uyarılarından olaylar oluşturabilirsiniz.

Daha fazla bilgi: [Azure uyarılarından ITSM iş öğeleri oluşturun.](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)

### <a name="generate-client-id-for-cherwell"></a>Cherwell için istemci kimliği oluşturma

Cherwell için istemci kimliği/anahtarı oluşturmak için aşağıdaki yordamı kullanın:

1. Yönetici olarak Cherwell örneğinize giriş yapın.
2. **Güvenlik** > **Edit REST API istemci ayarlarını**tıklatın.
3. Yeni istemci > **istemci sırrı** **oluştur'u**seçin.

    ![Cherwell kullanıcı kimliği](media/itsmc-connections/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Sonraki adımlar
 - [Azure uyarılarından ITSM iş öğeleri oluşturma](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
