---
title: 'Öğretici: SSMS kullanarak ilk ilişkisel veritabanınızı tasarla'
description: Sql Server Management Studio'u kullanarak Azure SQL Veritabanı'ndaki tek bir veritabanında ilk ilişkisel veritabanınızı tasarlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 07/29/2019
ms.openlocfilehash: 9764c4bc794eb8d133270b762fa2bca30a056fea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75459636"
---
# <a name="tutorial-design-a-relational-database-in-a-single-database-within-azure-sql-database-using-ssms"></a>Öğretici: SSMS kullanarak Azure SQL Veritabanı içinde tek bir veritabanında ilişkisel bir veritabanı tasarla

Azure SQL veritabanı, Microsoft Cloud'da (Azure) hizmet olarak ilişkisel bir veritabanıdır (DBaaS). Bu öğreticide, Azure portalını ve [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)'yu (SSMS) kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> - Azure portalını kullanarak tek bir veritabanı oluşturun*
> - Azure portalını kullanarak sunucu düzeyinde BIR IP güvenlik duvarı kuralı ayarlama
> - SSMS ile veritabanına bağlanma
> - SSMS ile tablo oluşturma
> - BCP ile toplu veri yükleme
> - SSMS ile verileri sorgula

*Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

> [!TIP]
> Aşağıdaki Microsoft Learn modülü, basit bir veritabanı nın oluşturulması da dahil olmak üzere [bir Azure SQL Veritabanını sorgulayan bir ASP.NET uygulamasının nasıl geliştirilip yapılandırılabildiğini](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/)ücretsiz olarak öğrenmenize yardımcı olur.
> [!NOTE]
> Bu öğretici amacıyla, tek bir veritabanı kullanıyoruz. Elastik bir havuzda havuzda birleştirilmiş veritabanı veya yönetilen bir örnek veritabanı da kullanabilirsiniz. Yönetilen bir örne bağlantı için şu yönetilen örnek hızlı başlangıçlarına bakın: [Hızlı başlangıç: Azure SQL Veritabanı Yönetilen Örneği'ne bağlanmak için Azure VM'yi yapılandırın](sql-database-managed-instance-configure-vm.md) [ve Quickstart: Şirket içinde Azure SQL Veritabanı Yönetilen Örneği'ne noktadan yerinde bağlantı yapınız.](sql-database-managed-instance-configure-p2s.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için yüklediğinizden emin olun:

- [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (en son sürüm)
- [BCP ve SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433) (en son sürüm)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-a-blank-single-database"></a>Boş bir tek veritabanı oluşturma

Azure SQL Veritabanı'nda tanımlanan bir işlem ve depolama kaynakları kümesiyle tek bir veritabanı oluşturulur. Veritabanı bir Azure [kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur ve bir [veritabanı sunucusu](sql-database-servers.md)kullanılarak yönetilir.

Boş bir tek veritabanı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
2. **Yeni** sayfasında, Azure Market bölümünde **Veritabanları**’nı seçin ve ardından **Öne Çıkan** bölümünde **SQL Veritabanı**’na tıklayın.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

3. **Sql Veritabanı** formunu, önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Veritabanı adı** | *sizin Veritabanınız* | Geçerli veritabanı adları için [Veritabanı tanımlayıcıları'na](/sql/relational-databases/databases/database-identifiers)bakın. |
    | **Abonelik** | *yourSubscription*  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | *sizin ResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). |
    | **Kaynağı seçin** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Varolan bir veritabanı sunucusu kullanmak veya yeni bir veritabanı sunucusu oluşturmak ve yapılandırmak için **Sunucu'ya** tıklayın. Varolan bir sunucuseçin veya **yeni bir sunucu oluştur'u** tıklatın ve **Yeni sunucu** formunu aşağıdaki bilgilerle doldurun:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). |
    | **Sunucu admin giriş** | Geçerli bir ad | Geçerli giriş adları için [Veritabanı tanımlayıcıları'na](/sql/relational-databases/databases/database-identifiers)bakın. |
    | **Parola** | Geçerli bir parola | Parolanızın en az sekiz karakteri olmalı ve aşağıdaki kategorilerden üçündeki karakterleri kullanmalıdır: büyük harf karakterleri, küçük harf karakterleri, sayılar ve alfasayısal olmayan karakterler. |
    | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

    ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç'i**tıklatın.
6. Hizmet katmanını, DTU veya sanal çekirdek sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Her hizmet katmanı için kullanabileceğiniz DTUs/vCore sayının ve depolama alanının seçeneklerini keşfedebilirsiniz.

    Hizmet katmanını, DT'lerin veya vCore'ların sayısını ve depolama miktarını seçtikten sonra **Uygula'yı**tıklatın.

7. Boş veritabanı için **bir Collation** girin (bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](/sql/t-sql/statements/collations)

8. **SQL Veritabanı** formunu tamamladığınızda, tek veritabanını sağlamak için **Oluştur'u** tıklatın. Bu adım birkaç dakika sürebilir.

9. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

   ![bildirim](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-server-level-ip-firewall-rule"></a>Sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti sunucu düzeyinde bir IP güvenlik duvarı oluşturur. Bu güvenlik duvarı, bir güvenlik duvarı kuralı IP'lerinin güvenlik duvarından geçmesine izin vermedikçe harici uygulamaların ve araçların sunucuya ve sunucudaki veritabanlarına bağlanmasını engeller. Tek veritabanınıza harici bağlantı sağlamak için, öncelikle IP adresiniz (veya IP adresi aralığı) için bir IP güvenlik duvarı kuralı eklemeniz gerekir. Bir SQL Veritabanı [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-firewall-configure.md)oluşturmak için aşağıdaki adımları izleyin.

> [!IMPORTANT]
> SQL Veritabanı hizmeti port 1433 üzerinden iletişim kurar. Bu hizmete bir şirket ağı içinden bağlanmaya çalışıyorsanız, 1433 bağlantı noktası üzerindeki giden trafiğe ağınızın güvenlik duvarı izin vermeyebilir. Bu nedenle, yöneticiniz 1433 bağlantı noktasını açmadıkça tek veritabanınıza bağlanamazsınız.

1. Dağıtım tamamlandıktan sonra, Azure portal menüsünden **SQL veritabanlarını** seçin veya herhangi bir sayfadan *SQL veritabanlarını* arayın ve seçin.  

1. **SQL veritabanları** sayfasında *Veritabanınızı* seçin. Veritabanınız için genel bakış sayfası açılır ve size tam `contosodatabaseserver01.database.windows.net`nitelikli Sunucu **adını** (örneğin) gösterir ve daha fazla yapılandırma için seçenekler sunar.

   ![sunucu adı](./media/sql-database-design-first-database/server-name.png)

1. SQL Server Management Studio'dan sunucunuza ve veritabanlarınıza bağlanmak için bu tam nitelikli sunucu adını kopyalayın.

1. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu düzeyinde IP güvenlik duvarı kuralı](./media/sql-database-design-first-database/server-firewall-rule.png)

1. Geçerli IP adresinizi yeni bir IP güvenlik duvarı kuralına eklemek için araç çubuğuna **istemci IP ekle'yi** tıklatın. IP güvenlik duvarı kuralı, tek bir IP adresi veya çeşitli IP adresleri için 1433 bağlantı noktasını açabilir.

1. **Kaydet**'e tıklayın. SQL Veritabanı sunucusunda geçerli IP adresiniz 1433 açma bağlantı noktası için sunucu düzeyinde BIR IP güvenlik duvarı kuralı oluşturulur.

1. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

IP adresiniz artık IP güvenlik duvarından geçebilir. Artık SQL Server Management Studio'u veya seçtiğiniz başka bir aracı kullanarak tek veritabanınıza bağlanabilirsiniz. Daha önce oluşturduğunuz sunucu yöneticisi hesabını kullandığınızdan emin olun.

> [!IMPORTANT]
> Varsayılan olarak, tüm Azure hizmetleri için SQL Database IP güvenlik duvarı üzerinden erişim sağlanır. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="connect-to-the-database"></a>Veritabanına bağlanın

Tek veritabanınıza bağlantı kurmak için [SQL Server Management Studio'yı](/sql/ssms/sql-server-management-studio-ssms) kullanın.

1. SQL Server Management Studio’yu açın.
2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Sunucu türü** | Veritabanı altyapısı | Bu değer gereklidir. |
   | **Sunucu adı** | Tam sunucu adı | Örneğin, *yourserver.database.windows.net.* |
   | **Kimlik doğrulaması** | SQL Server Kimlik Doğrulaması | SQL Kimlik Doğrulama, bu öğreticide yapılandırdığımız tek kimlik doğrulama türüdür. |
   | **Oturum açma** | Sunucu yöneticisi hesabı | Sunucuyu oluştururken belirttiğiniz hesap. |
   | **Parola** | Sunucu yöneticisi hesabınızın parolası | Sunucuyu oluşturduğunuzda belirttiğiniz parola. |

   ![sunucuya bağlan](./media/sql-database-design-first-database/connect.png)

3. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. **Veritabanına Bağlan** bölümünde, bu veritabanına bağlanmak için *Veritabanınızı* girin.

    ![sunucuda veritabanına bağlanma](./media/sql-database-design-first-database/options-connect-to-db.png)  

4. **Bağlan**'a tıklayın. **Nesne Gezgini** penceresi SSMS'de açılır.

5. **Object Explorer'da** **Veritabanlarını** genişletin ve ardından *veritabanınızı* örnek veritabanındaki nesneleri görüntülemek için genişletin.

   ![veritabanı nesneleri](./media/sql-database-design-first-database/connected.png)  

## <a name="create-tables-in-your-database"></a>Veritabanınızda tablo oluşturma

[Transact-SQL](/sql/t-sql/language-reference) kullanarak üniversiteler için bir öğrenci yönetim sistemi modelleyen dört tablo ile bir veritabanı şeması oluşturun:

- Kişi
- Ders
- Öğrenci
- Kredi

Aşağıdaki diyagramda bu tabloların birbirleriyle nasıl ilişkili olduğu gösterilmektedir. Bu tablolardan bazıları başka tablolardaki sütunlara başvurur. Örneğin, *Öğrenci* tablosu *Kişi* tablosunun *Kişi Kimliği* sütununa başvurur. Bu öğreticideki tabloların birbirleriyle ilişkisini anlamak için diyagram üzerinde çalışın. Etkili veritabanı tabloları oluşturmaya ilişkin ayrıntılı bir bakış için bkz. [Etkili veritabanı tabloları oluşturma](https://msdn.microsoft.com/library/cc505842.aspx). Veri türleri seçme hakkında bilgi için bkz. [Veri türleri](/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Tablolarınızı oluşturup tasarlamak için [SQL Server Management Studio’daki tablo tasarımcısını](/sql/ssms/visual-db-tools/design-database-diagrams-visual-database-tools) da kullanabilirsiniz.

![Tablo ilişkileri](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. **Object Explorer'da** *Veritabanınızı* sağ tıklatın ve **Yeni Sorgu'u**seçin. Veritabanınıza bağlı boş bir sorgu penceresi açılır.

2. Sorgu penceresinde aşağıdaki sorguyu yürüterek veritabanınızda dört tablo oluşturun:

   ```sql
   -- Create Person table
   CREATE TABLE Person
   (
       PersonId INT IDENTITY PRIMARY KEY,
       FirstName NVARCHAR(128) NOT NULL,
       MiddelInitial NVARCHAR(10),
       LastName NVARCHAR(128) NOT NULL,
       DateOfBirth DATE NOT NULL
   )

   -- Create Student table
   CREATE TABLE Student
   (
       StudentId INT IDENTITY PRIMARY KEY,
       PersonId INT REFERENCES Person (PersonId),
       Email NVARCHAR(256)
   )

   -- Create Course table
   CREATE TABLE Course
   (
       CourseId INT IDENTITY PRIMARY KEY,
       Name NVARCHAR(50) NOT NULL,
       Teacher NVARCHAR(256) NOT NULL
   )

   -- Create Credit table
   CREATE TABLE Credit
   (
       StudentId INT REFERENCES Student (StudentId),
       CourseId INT REFERENCES Course (CourseId),
       Grade DECIMAL(5,2) CHECK (Grade <= 100.00),
       Attempt TINYINT,
       CONSTRAINT [UQ_studentgrades] UNIQUE CLUSTERED
       (
           StudentId, CourseId, Grade, Attempt
       )
   )
   ```

   ![Tablo oluşturma](./media/sql-database-design-first-database/create-tables.png)

3. Oluşturduğunuz tabloları görmek için **Object Explorer'da** *Veritabanınızın* altındaki **Tablolar** düğümunu genişletin.

   ![ssms tables-created](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme

1. Veritabanınız için örnek verileri depolamak için İndirilenler klasörünüzde *örnek Veri* adlı bir klasör oluşturun.

2. Aşağıdaki bağlantıları sağ tıklatın ve *bunları sampleData* klasörüne kaydedin.

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Komut istemi penceresi açın ve *sampleData* klasörüne gidin.

4. *Sunucu,* *veritabanı,* *kullanıcı*ve *parola* değerlerini ortamınız için değerlerle değiştirmek için tablolara örnek veri eklemek için aşağıdaki komutları uygulayın.

   ```cmd
   bcp Course in SampleCourseData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   ```

Daha önce oluşturduğunuz tablolara örnek veriler yüklediniz.

## <a name="query-data"></a>Verileri sorgulama

Veritabanı tablolarından bilgi almak için aşağıdaki sorguları yürütün. Bkz. SQL sorguları yazma hakkında daha fazla bilgi edinmek için [SQL sorguları](https://technet.microsoft.com/library/bb264565.aspx) yazın. İlk sorgu, %75'in üzerinde bir nota sahip 'Dominick Pope' tarafından öğretilen öğrencileri bulmak için dört tabloya da katılır. İkinci sorgu dört tabloya da katılır ve 'Noe Coleman'ın kaydolduğu dersleri bulur.

1. SQL Server Management Studio sorgu penceresinde aşağıdaki sorguyu yürütün:

   ```sql
   -- Find the students taught by Dominick Pope who have a grade higher than 75%
   SELECT  person.FirstName, person.LastName, course.Name, credit.Grade
   FROM  Person AS person
       INNER JOIN Student AS student ON person.PersonId = student.PersonId
       INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
       INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope'
       AND Grade > 75
   ```

2. Sorgu penceresinde, aşağıdaki sorguyu yürütün:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled
   SELECT  course.Name, course.Teacher, credit.Grade
   FROM  Course AS course
       INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
       INNER JOIN Student AS student ON student.StudentId = credit.StudentId
       INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
       AND person.LastName = 'Coleman'
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, birçok temel veritabanı görevleri öğrendim. Şunları öğrendiniz:

> [!div class="checklist"]
> - Tek bir veritabanı oluşturma
> - Sunucu düzeyinde BIR IP güvenlik duvarı kuralı ayarlama
> - [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) kullanarak veritabanına bağlanma
> - Tablo oluşturma
> - Toplu veri yükleme
> - Bu verileri sorgulama

Visual Studio ve C# kullanarak veritabanı tasarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure SQL Veritabanı C# ve ADO.NET içinde tek bir veritabanında ilişkisel bir veritabanı tasarla](sql-database-design-first-database-csharp.md)
