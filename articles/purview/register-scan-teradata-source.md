---
title: Teradata kaynağını kaydetme ve taramaları ayarlama (Önizleme)
description: Bu makalede, Azure purview 'a bir Teradata kaynağı kaydetme ve tarama ayarlama özetlenmektedir.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: overview
ms.date: 12/06/2020
ms.openlocfilehash: 170ece293f8d24f3a29774c64c14f9334fe0930c
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96841955"
---
# <a name="register-and-scan-teradata-source-preview"></a>Teradata kaynağını kaydetme ve tarama (Önizleme)

Bu makalede, bir Teradata kaynağını Takiview 'a kaydetme ve tarama ayarlama özetlenmektedir.

> [!IMPORTANT]
> Bu veri kaynağı şu anda önizleme aşamasındadır. Deneyebilir ve geri bildirimde bulunun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Teradata kaynağı bir Teradata veritabanından meta verileri ayıklamak için **tam taramayı** destekler ve veri varlıkları arasında **kökenini** getirir.

## <a name="prerequisites"></a>Önkoşullar

- Bağlayıcı, yalnızca şirket içi ağ, Azure sanal ağ veya Amazon sanal özel bulutu içinde bulunan veri deposunu destekler. Bu nedenle, kendisine bağlanmak için [Şirket içinde barındırılan bir tümleştirme çalışma zamanı](manage-integration-runtimes.md) ayarlamanız gerekir.

- Şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olduğu sanal makinenizde Java Runtime Environment (JRE) ' nin yüklü olduğundan emin olun.
 
- Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde "Visual C++ Redistributable 2012 güncelleştirme 4" ' ün yüklü olduğundan emin olun. Henüz yüklemediyseniz, [buradan](https://www.microsoft.com/download/details.aspx?id=30679)indirin.

- Şirket içi sanal makinenize Teradata JDBC sürücüsü adlı bir sürücüyü el ile kurmanız gerekir. Yürütülebilir JAR dosyası, [Teradata Web sitesinden](https://downloads.teradata.com/)indirilebilir.

    > [!Note] 
    > Sürücü, VM 'deki tüm hesaplara erişebilmelidir. Lütfen bir kullanıcı hesabına yüklemeyin.

- Desteklenen Teradata veritabanı sürümleri **12. x ile 16. x** arasında. Taranmakta olan Teradata kaynağına okuma erişiminizin olduğundan emin olun.

### <a name="feature-flag"></a>Özellik bayrağı

Bir Teradata kaynağının kaydı ve taranması bir özellik bayrağının arkasında bulunabilir. URL 'niz için aşağıdakini ekleyin: *? Feature. ext. DataSource =% 7B "Teradata": "true"% 7d* 

Örneğin, tam URL [ https://web.purview.azure.com/?feature.ext.datasource=%7b "metadata": "true"% 7d](https://web.purview.azure.com/?feature.ext.datasource=%7b"teradata":"true"%7d)

## <a name="setting-up-authentication-for-a-scan"></a>Tarama için kimlik doğrulamasını ayarlama
Teradata kaynağı için kimlik doğrulamasını kurmak için desteklenen tek yol, **temel veritabanı** kimlik doğrulamadır

## <a name="register-a-teradata-source"></a>Teradata kaynağını kaydetme

Veri kataloğunuza yeni bir Teradata kaynağı kaydetmek için aşağıdakileri yapın:

1. Purview hesabınıza gidin
2. Sol gezinti bölmesinde **kaynakları** seçin
3. **Kaydol** ' u seçin
4. **Kayıt kaynakları** üzerinde **Teradata** 'yi seçin
5. **Devam**'ı seçin

**Kaynakları Kaydet (Teradata)** ekranında şunları yapın:

1. Veri kaynağının katalogda listelenecek bir **ad** girin.
2. Bir Teradata kaynağına bağlanmak için **ana bilgisayar** adını girin. Ayrıca sunucuya bir IP adresi veya tam bir bağlantı dizesi de olabilir.
3. Bir koleksiyon seçin veya yeni bir tane oluşturun (Isteğe bağlı)
4. Veri kaynağını kaydetmek için **son** ' a gidin.

    :::image type="content" source="media/register-scan-teradata-source/register-sources.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

## <a name="creating-and-running-a-scan"></a>Tarama oluşturma ve çalıştırma

Yeni bir tarama oluşturup çalıştırmak için aşağıdakileri yapın:
1. Yönetim Merkezi 'nde *tümleştirme çalışma zamanları*' na tıklayın. Şirket içinde barındırılan bir tümleştirme çalışma zamanının ayarlandığından emin olun. Ayarlanmamışsa, şirket içi ağınıza erişimi olan şirket içi veya Azure VM 'de tarama için şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturmak üzere [burada](manage-integration-runtimes.md) bahsedilen adımları kullanın.

2. Sonra, **kaynaklarda** gezin

3. Kayıtlı Teradata kaynağını seçin.

4. + Yeni tarama seçin
 
5. Aşağıdaki ayrıntıları sağlayın:

    - Ad: taramanın adı

    - Tümleştirme çalışma zamanı aracılığıyla Bağlan: yapılandırılmış şirket içinde barındırılan tümleştirme çalışma zamanını seçin

    - Kimlik doğrulama yöntemi: veritabanı kimlik doğrulaması şimdilik desteklenen tek seçenektir. Bu, varsayılan olarak seçilecek

    - Kullanıcı adı: veritabanı sunucusuna bağlanmak için bir Kullanıcı adı. Bu Kullanıcı adının sunucuya okuma erişiminin olması gerekir

    - Parola: veritabanı sunucusuna bağlanmak için kullanılan Kullanıcı parolası

    - Şema: içeri aktarılacak, noktalı virgülle ayrılmış bir liste olarak ifade edilen şemaların alt kümesini listeleyin. ör. Schema1; schema2. Tüm Kullanıcı şemaları, bu liste boşsa içeri aktarılır. Tüm sistem şemaları (örneğin, SysAdmin) ve nesneler varsayılan olarak yok sayılır.

        SQL LIKE ifadeleri sözdizimi kullanan kabul edilebilir şema adı desenleri%, örneğin%; Kenarı % C%; TID
        - veya ile başlayın    
        - B veya ile bitir    
        - C veya içerir    
        - eşit D

        NOT ve özel karakterlerin kullanımı kabul edilemez

    - Sürücü konumu: müşterinin VM 'sinde Teradata sürücü konumunun yolunu doldurun. Teradata JDBC sürücü adı şu olmalıdır: com. Teradata. JDBC. TeraDriver

    - Kullanılabilir maksimum bellek: müşteri VM 'sinde, işlem tarama tarafından kullanılacak maksimum bellek (GB cinsinden). Bu, taranacak Teradata kaynağının boyutuna bağlıdır.

    > [!Note] 
    > Thumb kuralı olarak, lütfen her 1000 tablo için 2GB bellek sağlayın

    :::image type="content" source="media/register-scan-teradata-source/setup-scan.png" alt-text="Kurulum taraması" border="true":::

6. Devam ' a tıklayın *.*

7. Tarama tetikleyiciyi seçin. Bir zamanlama ayarlayabilir veya taramayı bir kez çalıştırabilirsiniz.

    :::image type="content" source="media/register-scan-teradata-source/scan-trigger.png" alt-text="tarama tetikleyicisi" border="true":::

8. Taramayı gözden geçirin ve *Kaydet ve Çalıştır* ' a tıklayın.

## <a name="viewing-your-scans-and-scan-runs"></a>Taramalarınızı ve tarama çalıştırmalarını görüntüleme

1. Yönetim merkezine gidin. **Kaynaklar ve tarama** bölümünde **veri kaynakları** ' nı seçin.

2. İstediğiniz veri kaynağını seçin. Bu veri kaynağındaki mevcut taramaların bir listesini görürsünüz.

3. Sonuçları görüntülemeye ilgilendiğiniz taramayı seçin.

4. Bu sayfada, her bir tarama çalışmasının ölçümlerinin ve durumlarının yanı sıra önceki tüm tarama çalışmalarının hepsi gösterilecektir. Ayrıca, taramalarınızın zamanlandığını veya el ile, kaç varlık sınıflandırdığını, kaç tane varlık olduğunu, taramanın başlangıç ve bitiş zamanını ve toplam tarama süresini de görüntüler.

## <a name="manage-your-scans"></a>Taramalarınızı yönetin

Bir taramayı yönetmek veya silmek için aşağıdakileri yapın:

1. Yönetim merkezine gidin. **Kaynaklar ve tarama** bölümünde **veri kaynakları** ' nı seçin ve ardından istediğiniz veri kaynağını seçin.

2. Yönetmek istediğiniz taramayı seçin. Taramayı **Düzenle** seçeneğini belirleyerek düzenleyebilirsiniz.

3. , **Sil**' i seçerek taramayı silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)
