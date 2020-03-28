---
title: include dosyası
description: include dosyası
author: jonels-msft
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: include
ms.date: 09/12/2019
ms.author: jonels
ms.custom: include file
ms.openlocfilehash: e7a6f7b4ba4219483cd3eb8f4600bc94213df131
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973432"
---
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-an-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanı Oluşturma

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1. Azure portalının sol üst köşesinde **kaynak oluştur'u** tıklatın.
2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
3. Dağıtım seçeneği **için, Hyperscale (Citus) sunucu grubunun** altındaki **Oluştur** düğmesini tıklatın.
4. Yeni sunucu ayrıntıları formunu aşağıdaki bilgilerle doldurun:
   - Kaynak grubu: Bu alan için metin kutusunun altındaki yeni bağlantı **oluştur'u** tıklatın. **Myresourcegroup**gibi bir ad girin.
   - Sunucu grup adı: yeni sunucu grubu için benzersiz bir ad girin, bu ad sunucu alt etki alanı için de kullanılacaktır.
   - Yönetici kullanıcı adı: şu anda değeri **citus**olması gereklidir ve değiştirilemez.
   - Şifre: en az sekiz karakter uzunluğunda olmalı ve aşağıdaki kategorilerden üçünden karakterler içermelidir – İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.)
   - Konum: Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın konumu kullanın.

   > [!IMPORTANT]
   > Burada belirttiğiniz sunucu yöneticisi parolasının sunucuda ve veritabanlarında oturum açması gerekir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

5. **Sunucu grubunu yapılandır'ı**tıklatın. Bu bölümdeki ayarları değiştirmeden bırakın ve **Kaydet'i**tıklatın.
6. İleri' yi tıklatın : Ekranın alt kısmında **ağ >.**

7. **Ağ** sekmesinde, Ortak **bitiş noktası** radyo düğmesini tıklatın.
   ![Genel bitiş noktası seçildi](./media/azure-postgresql-hyperscale-create-db/network-public-endpoint.png)
8. Bağlantıyı tıklatın **+ Geçerli istemci IP adresi ekle**.
   ![İstemci IP eklendi](./media/azure-postgresql-hyperscale-create-db/network-add-client-ip.png)

   > [!NOTE]
   > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu nedenle, BT departmanınız 5432 bağlantı noktasını açmadıkça Hyperscale (Citus) kümenize bağlanamazsınız.
   >

9. **Sunucuyu sağlamak** için Gözden Geçir + oluştur'u ve ardından **Oluştur'u** tıklatın. Sağlama birkaç dakika sürer.
10. Sayfa dağıtımı izlemek için yönlendirecektir. **Dağıtımınızdan** **dağıtımınıza**kadar canlı durum değişiklikleri tamamlandığında, sayfanın solundaki **Çıktılar** menü öğesini tıklatın.
11. Çıktılar sayfası, değeri panoya kopyalamak için yanındaki düğmeyi içeren bir koordinatör ana bilgisayar adı içerir. Bu bilgileri daha sonra kullanmak üzere kaydedin.

## <a name="connect-to-the-database-using-psql"></a>psql kullanarak veritabanına bağlanın

PostgreSQL sunucusu için Azure Veritabanınızı oluşturduğunuzda, **citus** adında varsayılan bir veritabanı oluşturulur. Veritabanı sunucunuza bağlanmak için bir bağlantı dizesi ve yönetici parolası gerekir.

1. Bağlantı dizesini edinin. Sunucu grubu sayfasında **Bağlantı dizeleri** menü öğesini tıklatın. (Ayarlar altında **Settings**.) **Psql**işaretli dizeyi bulun. Bu formda olacak:

   ```
   psql "host=hostname.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require"
   ```

   Dizekopyalayın. "{parolanız}"\_ile daha önce seçtiğiniz yönetim parolasını değiştirmeniz gerekir. Sistem düz metin parolanızı depolamaz ve bu nedenle bağlantı dizesinde sizin için görüntülemez.

2. Yerel bilgisayarınızda bir terminal penceresi açın.

3. Komut istemiyle, [psql](https://www.postgresql.org/docs/current/app-psql.html) yardımcı programıyla PostgreSQL sunucusu için Azure Veritabanınıza bağlanın. Parolanızı içerdiğinden emin olmak için bağlantı dizenizi tırnak içinde geçirin:
   ```bash
   psql "host=..."
   ```

   Örneğin, aşağıdaki komut sunucu grubu **mydemoserver**koordinatör düğümüne bağlanır:

   ```bash
   psql "host=mydemoserver-c.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require"
   ```
