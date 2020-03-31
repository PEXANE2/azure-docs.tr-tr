---
title: Azure Veri Gölü Depolama Gen1'de depolanan verilerin güvenliğini sağlama | Microsoft Dokümanlar
description: Grupları ve erişim denetim listelerini kullanarak Azure Veri Gölü Depolama Gen1'de verilerin nasıl güvende olduğunu öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: cebdff5ed233516683df3330e8fd3332ded664e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260312"
---
# <a name="securing-data-stored-in-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. Nesil'de depolanan verilerin güvenliğini sağlama
Azure Veri Gölü Depolama Gen1'de veri güvenliğini sağlamak üç adımlı bir yaklaşımdır.  Hem rol tabanlı erişim denetimi (RBAC) hem de erişim denetim listeleri (ALA)'lar, kullanıcılar ve güvenlik grupları için verilere erişimi tam olarak etkinleştirecek şekilde ayarlanmalıdır.

1. Azure Etkin Dizini'nde (AAD) güvenlik grupları oluşturarak başlayın. Bu güvenlik grupları, Azure portalında rol tabanlı erişim denetimi (RBAC) uygulamak için kullanılır. Daha fazla bilgi için [Microsoft Azure'da Rol Tabanlı Erişim Denetimi'ne](../role-based-access-control/role-assignments-portal.md)bakın.
2. AAD güvenlik gruplarını Veri Gölü Depolama Gen1 hesabına atayın. Bu, portaldan veya API'lerden Veri Gölü Depolama Gen1 hesabına erişimi ve yönetim işlemlerini denetler.
3. Veri Gölü Depolama Gen1 dosya sisteminde AAD güvenlik gruplarını erişim denetim listeleri (ACD'ler) olarak atayın.
4. Ayrıca, Veri Gölü Depolama Gen1'deki verilere erişebilen istemciler için bir IP adres aralığı da ayarlayabilirsiniz.

Bu makalede, yukarıdaki görevleri gerçekleştirmek için Azure portalının nasıl kullanılacağına ilişkin yönergeler verilmektedir. Veri Gölü Depolama Gen1'in hesap ve veri düzeyinde güvenliği nasıl uyguladığı hakkında ayrıntılı bilgi için Azure [Veri Gölü Depolama Gen1'de Güvenlik'e](data-lake-store-security-overview.md)bakın. ALA'ların Data Lake Storage Gen1'de nasıl uygulandığı hakkında derin bilgiler [için](data-lake-store-access-control.md)bkz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Veri Gölü Depolama Gen1 hesabı**. Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Azure Etkin Dizini'nde güvenlik grupları oluşturma
AAD güvenlik gruplarının nasıl oluşturulacağına ve gruba nasıl kullanıcı eklenacağına ilişkin talimatlar için [Azure Etkin Dizini'nde güvenlik gruplarını yönetme'ye](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)bakın.

> [!NOTE] 
> Azure portalını kullanarak Azure AD'deki bir gruba hem kullanıcıları hem de diğer grupları ekleyebilirsiniz. Ancak, bir gruba bir hizmet sorumlusu eklemek için [Azure AD'nin PowerShell modüllerini](../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md)kullanın.
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-data-lake-storage-gen1-accounts"></a>Kullanıcıları veya güvenlik gruplarını Veri Gölü Depolama Gen1 hesaplarına atama
Kullanıcıları veya güvenlik gruplarını Veri Gölü Depolama Gen1 hesaplarına atadığınızda, Azure portalı ve Azure Kaynak Yöneticisi API'lerini kullanarak hesaptaki yönetim işlemlerine erişimi denetlersiniz. 

1. Bir Veri Gölü Depolama Gen1 hesabı açın. Sol bölmeden Tüm **kaynaklar'ı**tıklatın ve ardından Tüm kaynaklar çubuğundan, bir kullanıcı veya güvenlik grubu atamak istediğiniz hesap adını tıklatın.

2. Veri Gölü Depolama Gen1 hesap bıçak, **Erişim Denetimi (IAM)** tıklatın. Bıçak varsayılan olarak abonelik sahiplerini sahip olarak listeler.
   
    ![Güvenlik grubunu Azure Veri Gölü Depolama Gen1 hesabına atama](./media/data-lake-store-secure-data/adl.select.user.icon1.png "Güvenlik grubunu Azure Veri Gölü Depolama Gen1 hesabına atama")

3. Erişim **Denetimi (IAM)** bıçağında, İzin **Ekle** bıçağını açmak için **Ekle'yi** tıklatın. İzin **Ekle** bıçağında, kullanıcı/grup için bir **Rol** seçin. Azure Etkin Dizini'nde daha önce oluşturduğunuz güvenlik grubunu arayın ve seçin. Arama yapacak çok fazla kullanıcı nız ve grubunüz varsa, grup adına filtre uygulayacak şekilde **Seç** metin kutusunu kullanın. 
   
    ![Kullanıcı için rol ekleme](./media/data-lake-store-secure-data/adl.add.user.1.png "Kullanıcı için rol ekleme")
   
    **Sahip** ve **Katılımcı** rolü, veri gölü hesabındaki çeşitli yönetim işlevlerine erişim sağlar. Veri gölündeki verilerle etkileşimde bulunacak ancak yine de hesap yönetimi bilgilerini görüntülemesi gereken kullanıcılar için bunları **Reader** rolüne ekleyebilirsiniz. Bu rollerin kapsamı, Veri Gölü Depolama Gen1 hesabıyla ilgili yönetim işlemleriyle sınırlıdır.
   
    Veri işlemleri için, tek tek dosya sistemi izinleri kullanıcıların neler yapabileceğini tanımlar. Bu nedenle, Reader rolüne sahip bir kullanıcı yalnızca hesapla ilişkili yönetim ayarlarını görüntüleyebilir, ancak kendilerine atanan dosya sistemi izinlerini temel alarak verileri okuyup yazabilir. Veri Gölü Depolama Gen1 dosya sistemi [izinleri, Güvenlik grubuna Azure Veri Gölü Depolama Gen1 dosya sistemine ADC olarak atarken](#filepermissions)açıklanmıştır.

    > [!IMPORTANT]
    > Yalnızca **Sahip** rolü dosya sistemine otomatik olarak erişilmesini sağlar. **Katılımcı**, **Okuyucu**ve diğer tüm roller, klasörlere ve dosyalara herhangi bir düzeyde erişim sağlamak için ABILEr gerektirir.  **Sahip** rolü, ALA'lar aracılığıyla geçersiz kılınamayan süper kullanıcı dosyası ve klasör izinleri sağlar. RBAC ilkelerinin veri erişimiyle nasıl eşeşteiş yaptığı hakkında daha fazla bilgi [için hesap yönetimi için RBAC'a](data-lake-store-security-overview.md#rbac-for-account-management)bakın.

4. **İzin Ekle** kutusunda listelenmemiş bir grup/kullanıcı eklemek istiyorsanız, e-posta adreslerini **Seç** metin kutusuna yazıp listeden seçerek onları davet edebilirsiniz.
   
    ![Güvenlik grubu ekleme](./media/data-lake-store-secure-data/adl.add.user.2.png "Güvenlik grubu ekleme")
   
5. **Kaydet**'e tıklayın. Aşağıda gösterildiği gibi eklenen güvenlik grubunu görmeniz gerekir.
   
    ![Güvenlik grubu eklendi](./media/data-lake-store-secure-data/adl.add.user.3.png "Güvenlik grubu eklendi")

6. Kullanıcı/güvenlik grubunuzun artık Veri Gölü Depolama Gen1 hesabına erişimi vardır. Belirli kullanıcılara erişim sağlamak istiyorsanız, bunları güvenlik grubuna ekleyebilirsiniz. Benzer şekilde, bir kullanıcının erişimini iptal etmek istiyorsanız, bunları güvenlik grubundan kaldırabilirsiniz. Bir hesaba birden çok güvenlik grubu atayabilirsiniz. 

## <a name="assign-users-or-security-groups-as-acls-to-the-data-lake-storage-gen1-file-system"></a><a name="filepermissions"></a>Kullanıcıları veya güvenlik gruplarını Veri Gölü Depolama Gen1 dosya sistemine AÇOlarak atama
Kullanıcı/güvenlik gruplarını Veri Gölü Depolama Gen1 dosya sistemine atayarak, Veri Gölü Depolama Gen1'de depolanan veriler üzerinde erişim denetimini ayarlarsınız.

1. Veri Gölü Depolama Gen1 hesap bıçak, **Veri Gezgini'ni**tıklatın.
   
    ![Verileri Veri Gezgini ile görüntüleme](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Verileri Veri Gezgini ile görüntüleme")
2. Veri **Gezgini** bıtır'da, ACL'yi yapılandırmak istediğiniz klasörü tıklatın ve ardından **Access'i**tıklatın. Bir dosyaya ALA'lar atamak için önce dosyayı önizlemek için dosyayı tıklatmanız ve ardından **Dosya Önizleme** çubuğundan **Eriş'i** tıklatmanız gerekir.
   
    ![Data Lake Storage Gen1 dosya sisteminde ALA'ları ayarlama](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake Storage Gen1 dosya sisteminde ALA'ları ayarlama")
3. **Access** blade sahipleri listeler ve zaten kök atanmış izinleri atanmış atanmış. Ek Erişim ALA'ları eklemek için **Ekle** simgesini tıklatın.
    > [!IMPORTANT]
    > Tek bir dosya için erişim izinlerinin ayarlanması, kullanıcı/grup erişimi nin bu dosyaya verilmesi ne kadar gerekdir. Dosyaya giden yol atanan kullanıcı/grup tarafından erişilebilir olmalıdır. Daha fazla bilgi ve örnekler için, [izinlerle ilgili yaygın senaryolara](data-lake-store-access-control.md#common-scenarios-related-to-permissions)bakın.
   
    ![Standart ve özel erişimi listele](./media/data-lake-store-secure-data/adl.acl.2.png "Standart ve özel erişimi listele")
   
   * **Sahipleri** ve **Herkes** unix tarzı erişim sağlar, burada üç farklı kullanıcı sınıfları için okuma, yazma, yürütme (rwx) belirtin: sahibi, grup ve diğerleri.
   * **Atanan izinler,** belirli adlandırılmış kullanıcılar veya grupların dosya sahibinin veya grubunun dışında izinler ayarlamanızı sağlayan POSIX ALİ'lere karşılık gelir. 
     
     Daha fazla bilgi için [HDFS ALIK'lerine](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)bakın. ACD'lerin Veri Gölü Depolama Gen1'de nasıl uygulandığı hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'de Erişim Denetimi'ne](data-lake-store-access-control.md)bakın.
4. **İzin atama** bıçağını açmak için **Ekle** simgesini tıklatın. Bu bıçakta, **Kullanıcı yı seç'i veya grubu**seçin ve ardından Select kullanıcı veya **grup** bıçağında Azure Etkin Dizini'nde daha önce oluşturduğunuz güvenlik grubunu arayın. Arama yapacak çok fazla grubunüz varsa, grup adına filtre uygulayabilmek için üstteki metin kutusunu kullanın. Eklemek istediğiniz grubu tıklatın ve sonra **Seç'i**tıklatın.
   
    ![Grup ekleme](./media/data-lake-store-secure-data/adl.acl.3.png "Grup ekleme")
5. **İzinleri Seç'i**tıklatın, izinleri seçin, izinlerin özyinelemeli olarak uygulanıp uygulanmayacağı ve izinleri erişim ACL, varsayılan ACL veya her ikisi olarak atamak isteyip istemediğiniz. **Tamam**'a tıklayın.
   
    ![Gruplandırmak için izin atama](./media/data-lake-store-secure-data/adl.acl.4.png "Gruplandırmak için izin atama")
   
    Veri Gölü Depolama Gen1 ve Varsayılan/Erişim Aç'ları'ndaki izinler hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'de Erişim Denetimi'ne](data-lake-store-access-control.md)bakın.
6. **İzinleri Seç** çubuğunda **Tamam'ı** tıklattıktan sonra, yeni eklenen grup ve ilişkili izinler artık **Access** bladeinde listelenir.
   
    ![Gruplandırmak için izin atama](./media/data-lake-store-secure-data/adl.acl.5.png "Gruplandırmak için izin atama")
   
   > [!IMPORTANT]
   > Geçerli sürümde, **Atanan izinler**altında en fazla 28 girişe sahip olabilirsiniz. 28'den fazla kullanıcı eklemek istiyorsanız, güvenlik grupları oluşturmalı, güvenlik gruplarına kullanıcı eklemeli, Veri Gölü Depolama Gen1 hesabı için bu güvenlik gruplarına erişim sağlamanız gerekir.
   > 
   > 
7. Gerekirse, grubu ekledikten sonra erişim izinlerini de değiştirebilirsiniz. Güvenlik grubuna bu izni kaldırmak veya atamak isteyip istemediğinize bağlı olarak her izin türü (Oku, Yaz, Yürüt) için onay kutusunu temizleyin veya seçin. Değişiklikleri kaydetmek için **Kaydet'i** tıklatın veya değişiklikleri geri almak için **At'ı** tıklatın.

## <a name="set-ip-address-range-for-data-access"></a>Veri erişimi için IP adres aralığını ayarlama
Veri Gölü Depolama Gen1, ağ düzeyinde veri deponuza erişimi daha da kilitlemenizi sağlar. Güvenlik duvarını etkinleştirebilir, bir IP adresi belirtebilir veya güvenilir müşterileriniz için bir IP adresi aralığı tanımlayabilirsiniz. Etkinleştirildiğinde, yalnızca tanımlı aralıktaki IP adreslerine sahip istemciler mağazaya bağlanabilir.

![Güvenlik duvarı ayarları ve IP erişimi](./media/data-lake-store-secure-data/firewall-ip-access.png "Güvenlik duvarı ayarları ve IP adresi")

## <a name="remove-security-groups-for-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabıiçin güvenlik gruplarını kaldırma
Güvenlik gruplarını Data Lake Storage Gen1 hesaplarından kaldırdığınızda, yalnızca Azure Portalı ve Azure Kaynak Yöneticisi API'lerini kullanarak hesaptaki yönetim işlemlerine erişimi değiştirirsiniz.  

Verilere erişim değişmez ve erişim ALA'ları tarafından yönetilir.  Bunun istisnası, Sahipler rolündeki kullanıcılar/gruplardır.  Sahipler rolünden kaldırılan kullanıcılar/gruplar artık süper kullanıcılar değildir ve erişimleri ACL ayarlarına geri döner. 

1. Veri Gölü Depolama Gen1 hesap bıçak, **Erişim Denetimi (IAM)** tıklatın. 
   
    ![Veri Gölü Depolama Gen1 hesabına güvenlik grubu atama](./media/data-lake-store-secure-data/adl.select.user.icon.png "Veri Gölü Depolama Gen1 hesabına güvenlik grubu atama")
2. Erişim **Denetimi (IAM)** bıçak larında kaldırmak istediğiniz güvenlik grubunu tıklatın. **Kaldır**’a tıklayın.
   
    ![Güvenlik grubu kaldırıldı](./media/data-lake-store-secure-data/adl.remove.group.png "Güvenlik grubu kaldırıldı")

## <a name="remove-security-group-acls-from-a-data-lake-storage-gen1-file-system"></a>Veri Gölü Depolama Gen1 dosya sisteminden güvenlik grubu AÇ'larını kaldırma
Güvenlik grubu Aç'larını bir Veri Gölü Depolama Gen1 dosya sisteminden kaldırdığınızda, Veri Gölü Depolama Gen1 hesabındaki verilere erişimi değiştirirsiniz.

1. Veri Gölü Depolama Gen1 hesap bıçak, **Veri Gezgini'ni**tıklatın.
   
    ![Veri Gölü Depolama Gen1 hesabında dizin oluşturma](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Veri Gölü Depolama Gen1 hesabında dizin oluşturma")
2. Veri **Gezgini** bısınız, ACL'yi kaldırmak istediğiniz klasörü tıklatın ve ardından **Eriş'i**tıklatın. Bir dosyanın ALA'larını kaldırmak için önce dosyayı önizlemek için dosyayı tıklatmanız ve ardından **Dosya Önizleme** çubuğundan **Eriş'i** tıklatmanız gerekir. 
   
    ![Data Lake Storage Gen1 dosya sisteminde ALA'ları ayarlama](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake Storage Gen1 dosya sisteminde ALA'ları ayarlama")
3. **Access** bıyığını tıklatın ve kaldırmak istediğiniz güvenlik grubunu tıklatın. Access **ayrıntıları** bıçak, **Kaldır'ı**tıklatın.
   
    ![Gruplandırmak için izin atama](./media/data-lake-store-secure-data/adl.remove.acl.png "Gruplandırmak için izin atama")

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Verileri Azure Depolama Blobs'undan Veri Gölü Depolama Gen1'e kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell ile Data Lake Storage Gen1'i kullanmaya başlama](data-lake-store-get-started-powershell.md)
* [.NET SDK kullanarak Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-net-sdk.md)
* [Veri Gölü Depolama Gen1 için tanılama günlüklerine erişin](data-lake-store-diagnostic-logs.md)

