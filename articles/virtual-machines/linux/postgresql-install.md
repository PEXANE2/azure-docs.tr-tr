---
title: Linux VM'de PostgreSQL'i ayarlama
description: Azure'daki bir Linux sanal makinesine PostgreSQL'i nasıl yükleyip yapılandırılamayı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: bbfad994de663881e3aa03292fc0d0611a0d0933
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75747808"
---
# <a name="install-and-configure-postgresql-on-azure"></a>PostgreSQL'i Azure'da yükleme ve yapılandırma
PostgreSQL Oracle ve DB2 benzer gelişmiş bir açık kaynak veritabanıdır. Tam ACID uyumluluğu, güvenilir işlem işleme ve çok sürümlü eşzamanlılık denetimi gibi kurumsal kullanıma hazır özellikleri içerir. Ayrıca ANSI SQL ve SQL/MED gibi standartları da destekler (Oracle, MySQL, MongoDB ve diğerleri için yabancı veri ambalajları dahil). 12'den fazla yordam dili, GIN ve GiST dizinleri, uzamsal veri desteği ve JSON veya anahtar değeri tabanlı uygulamalar için birden fazla NoSQL benzeri özellik desteği ile son derece genişletilebilir.

Bu makalede, Linux çalıştıran bir Azure sanal makinesine PostgreSQL'i nasıl yükleyeceğinizi ve yapılandıracağınızı öğreneceksiniz.


## <a name="install-postgresql"></a>PostgreSQL Yükle
> [!NOTE]
> Bu öğreticiyi tamamlamak için Linux çalıştıran bir Azure sanal makineniz olması gerekir. Devam etmeden önce bir Linux VM oluşturmak ve kurmak için [Azure Linux VM öğreticisine](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.
> 
> 

Bu durumda, PostgreSQL bağlantı noktası olarak 1999 bağlantı noktasını kullanın.  

PuTTY aracılığıyla oluşturduğunuz Linux VM'ye bağlanın. İlk kez bir Azure Linux VM kullanıyorsanız, Linux VM'ye bağlanmak için PuTTY'yi nasıl kullanacağınızı öğrenmek için [Azure'da Linux ile SSH'yi nasıl kullanacağınızı](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) öğrenin.

1. Köke (admin) geçmek için aşağıdaki komutu çalıştırın:
   
        # sudo su -
2. Bazı dağıtımlarda PostgreSQL'i yüklemeden önce yüklemeniz gereken bağımlılıklar vardır. Bu listede dağıtım için kontrol edin ve uygun komutu çalıştırın:
   
   * Red Hat taban Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian temel Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. PostgreSQL'i kök dizinine indirin ve paketin zipini aç:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Yukarıdaki bir örnektir. Daha ayrıntılı indirme adresini [/pub/source/ dizininde](https://ftp.postgresql.org/pub/source/)bulabilirsiniz.
4. Yapıyı başlatmak için şu komutları çalıştırın:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Dokümantasyon (HTML ve adam sayfaları) ve ek modüller (`contrib`), bunun yerine aşağıdaki komutu çalıştırın dahil olmak üzere oluşturulabilecek her şeyi oluşturmak istiyorsanız:
   
        # gmake install-world
   
    Aşağıdaki onay iletisini almalısınız:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL'i Yapılandırma
1. (İsteğe bağlı) PostgreSQL başvurularını sürüm numarasını içermeyecek şekilde kısaltmak için sembolik bir bağlantı oluşturun:
   
        # ln -s /opt/postgresql-9.3.5 /opt/pgsql
2. Veritabanı için bir dizin oluşturun:
   
        # mkdir -p /opt/pgsql_data
3. Kök olmayan bir kullanıcı oluşturun ve bu kullanıcının profilini değiştirin. Daha sonra, bu yeni kullanıcıya geçin (örneğimizde *postgres* olarak adlandırılır):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Güvenlik nedenleriyle, PostgreSQL veritabanını başlatmak, başlatmak veya kapatmak için kök olmayan bir kullanıcı kullanır.
   > 
   > 
4. Aşağıdaki komutları girerek *bash_profile* dosyasını düzenleme. Bu satırlar *bash_profile* dosyasının sonuna eklenir:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. *bash_profile* dosyayı çalıştırın:
   
        $ source .bash_profile
6. Yüklemenizi aşağıdaki komutu kullanarak doğrulayın:
   
        $ which psql
   
    Yüklemeniz başarılı olursa, aşağıdaki yanıtı görürsünüz:
   
        /opt/pgsql/bin/psql
7. Ayrıca PostgreSQL sürümünü de kontrol edebilirsiniz:
   
        $ psql -V

8. Veritabanını başlatma:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Aşağıdaki çıktıyı almalısınız:

![image](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL'i ayarlama
<!--    [postgres@ test ~]$ exit -->

Aşağıdaki komutları çalıştırın:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

/etc/init.d/postgresql dosyasındaki iki değişkeni değiştirin. Önek PostgreSQL yükleme yoluna ayarlanır: **/opt/pgsql**. PGDATA, PostgreSQL veri depolama yoluna ayarlanmıştır: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/postgresql-install/no2.png)

Dosyayı yürütülebilir hale getirmek için değiştirin:

    # chmod +x /etc/init.d/postgresql

PostgreSQL'i Başlat:

    # /etc/init.d/postgresql start

PostgreSQL'in bitiş noktasının açık olup olmadığını kontrol edin:

    # netstat -tunlp|grep 1999

Aşağıdaki çıktıyı görmeniz gerekir:

![image](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Postgres veritabanına bağlanın
Bir kez daha postgres kullanıcısına geçin:

    # su - postgres

Bir Postgres veritabanı oluşturma:

    $ createdb events

Az önce oluşturduğunuz olaylar veritabanına bağlanın:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Bir Postgres tablosu oluşturma ve silme
Artık veritabanına bağlandığınıza göre, veritabanında tablolar oluşturabilirsiniz.

Örneğin, aşağıdaki komutu kullanarak yeni bir örnek Postgres tablosu oluşturun:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Şimdi aşağıdaki sütun adları ve kısıtlamaları içeren dört sütunlu bir tablo ayarladınız:

1. "Ad" sütunu VARCHAR komutu ile 20 karakter uzunluğunda olmak üzere sınırlandırılmıştır.
2. "Gıda" sütunu, her kişinin getireceği gıda maddesini gösterir. VARCHAR bu metnin 30 karakterin altında olmasını sınırlar.
3. "Onaylanan" sütun, kişinin potluck rsvp'd olup olmadığını kaydeder. Kabul edilebilir değerler "Y" ve "N"dir.
4. "Tarih" sütunu, etkinliğe ne zaman kaydolduklarını gösterir. Postgres tarihlerin yyyy-mm-dd olarak yazılmasını gerektirir.

Tablonuz başarıyla oluşturulduysa aşağıdakileri görmeniz gerekir:

![image](./media/postgresql-install/no4.png)

Tablo yapısını aşağıdaki komutu kullanarak da denetleyebilirsiniz:

![image](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Tabloya veri ekleme
İlk olarak, bir satıra bilgi ekleyin:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Bu çıkışı görmelisiniz:

![image](./media/postgresql-install/no6.png)

Masaya birkaç kişi daha ekleyebilirsiniz. Aşağıda bazı seçenekler vardır veya kendi seçeneklerinizi oluşturabilirsiniz:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabloları göster
Tabloyu göstermek için aşağıdaki komutu kullanın:

    select * from potluck;

Çıkış şöyle olur:

![image](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Tablodaki verileri silme
Tablodaki verileri silmek için aşağıdaki komutu kullanın:

    delete from potluck where name=’John’;

Bu, "John" satırındaki tüm bilgileri siler. Çıkış şöyle olur:

![image](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Tablodaki verileri güncelleştirme
Tablodaki verileri güncelleştirmek için aşağıdaki komutu kullanın. Bunun için, Sandy onların katıldığını doğruladı, bu yüzden "N" den "Y" için RSVP değişecektir:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>PostgreSQL hakkında daha fazla bilgi alın
PostgreSQL'in azure Linux VM'de yüklenmesini tamamladığınızda, Azure'da kullanmanın keyfini çıkarabilirsiniz. PostgreSQL hakkında daha fazla bilgi edinmek için [PostgreSQL web sitesini](https://www.postgresql.org/)ziyaret edin.

