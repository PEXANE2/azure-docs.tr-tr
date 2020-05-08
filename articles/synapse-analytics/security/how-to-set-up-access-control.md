---
title: SYNAPSE çalışma alanınızın güvenliğini sağlama (Önizleme)
description: Bu makale, SYNAPSE çalışma alanındaki etkinliklere ve verilere erişimi denetlemek için rolleri ve erişim denetimini nasıl kullanacağınızı öğretir.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: jrasnick
ms.openlocfilehash: 383a28e90c093c0885c34af03371608261ece110
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692625"
---
# <a name="secure-your-synapse-workspace-preview"></a>SYNAPSE çalışma alanınızın güvenliğini sağlama (Önizleme)

Bu makalede, etkinlikleri denetlemek ve verilere erişmek için rolleri ve erişim denetimini nasıl kullanacağınızı öğretir. Bu yönergeleri izleyerek, Azure SYNAPSE Analytics 'teki erişim denetimi basitleştirilmiştir. Yalnızca üç güvenlik grubundan birine Kullanıcı ekleyip kaldırmanız gerekir.

## <a name="overview"></a>Genel Bakış

Bir Synapse çalışma alanını (Önizleme) güvenli hale getirmek için, aşağıdaki öğeleri yapılandırma düzenine uyun:

- Azure rolleri (örneğin, sahibi, katkıda bulunanlar vb. gibi yerleşik olanlar)
- SYNAPSE rolleri – bu roller SYNAPSE için benzersizdir ve Azure rollerine dayalı değildir. Şu rollerin üçü vardır:
  - SYNAPSE çalışma alanı Yöneticisi
  - SYNAPSE SQL Yöneticisi
  - SYNAPSE Spark Yöneticisi
- Azure Data Lake Storage Gen 2 ' deki veriler için erişim denetimi (ADLSGEN2).
- SYNAPSE SQL ve Spark veritabanları için erişim denetimi

## <a name="steps-to-secure-a-synapse-workspace"></a>SYNAPSE çalışma alanını güvenli hale getirme adımları

Bu belge yönergeleri basitleştirmek için standart adları kullanır. Bunları dilediğiniz adlarla değiştirin.

|Ayar | Örnek değer | Açıklama |
| :------ | :-------------- | :---------- |
| **SYNAPSE çalışma alanı** | WS1 |  SYNAPSE çalışma alanının sahip olacağı ad. |
| **ADLSGEN2 hesabı** | STG1 | Çalışma alanınız ile kullanılacak ADLS hesabı. |
| **Kapsayıcı** | CNT1 | STG1 içinde, çalışma alanının varsayılan olarak kullanacağı kapsayıcı. |
| **Active Directory kiracısı** | contoso | Active Directory kiracı adı.|
||||

## <a name="step-1-set-up-security-groups"></a>1. Adım: güvenlik gruplarını ayarlama

Çalışma alanınız için üç güvenlik grubu oluşturun ve doldurun:

- **WS1\_wsadmins** – çalışma alanı üzerinde tamamen denetim gerektiren kullanıcılar için
- **WS1\_mini Yöneticiler** – çalışma alanının Spark yönleri üzerinde tümüyle denetim gerektiren kullanıcılar için
- **WS1\_SQLAdmins** – çalışma alanının SQL yönleri üzerinde tümüyle denetim gerektiren kullanıcılar için
- **WS1\_SQLAdmins** 'e **WS1\_wsadmins** ekleyin
- **WS1\_wsadmins** 'yi **WS1\_mini Yöneticiler** 'e Ekle

## <a name="step-2-prepare-your-data-lake-storage-gen2-account"></a>2. Adım: Data Lake Storage 2. hesabınızı hazırlama

Depolama bilgileriniz hakkında bu bilgileri tanımla:

- Çalışma alanınız için kullanılacak ADLSGEN2 hesabı. Bu belge, STG1 çağırır.  STG1, çalışma alanınız için "birincil" depolama hesabı olarak kabul edilir.
- WS1 içindeki kapsayıcı, SYNAPSE çalışma alanınızın varsayılan olarak kullanacağı kapsayıcıdır. Bu belge, CNT1 çağırır.  Bu kapsayıcı için kullanılır:
  - Spark tabloları için yedekleme verileri dosyalarını depolama
  - Spark işleri için yürütme günlükleri

- Azure portal kullanarak, güvenlik gruplarını CNT1 üzerinde aşağıdaki rolleri atayın

  - **Depolama Blobu veri katılımcısı** rolüne **WS1\_wsadmins** atama
  - **Depolama Blobu veri katılımcısı** rolüne **\_WS1 mini Yöneticiler** atama
  - **Depolama Blobu veri katılımcısı** rolüne **\_WS1 SQLAdmins** atama

## <a name="step-3-create-and-configure-your-synapse-workspace"></a>3. Adım: SYNAPSE çalışma alanınızı oluşturma ve yapılandırma

Azure portal, bir Synapse çalışma alanı oluşturun:

- Çalışma alanını WS1 olarak adlandırın
- Depolama hesabı için STG1 seçin
- "FileSystem" olarak kullanılmakta olan kapsayıcı için CNT1 seçin.
- WS1 'i SYNAPSE Studio 'da aç
- Aşağıdaki SYNAPSE rollerine güvenlik grupları atamak**Access Control** **Yönet** > ' i seçin.
  - SYNAPSE çalışma alanı yöneticilerine **WS1\_wsadmins** atama
  - SYNAPSE Spark yöneticilerine **\_WS1 mini Yöneticiler** atama
  - SYNAPSE SQL yöneticilerine **WS1\_SQLAdmins** atama

## <a name="step-4-configuring-data-lake-storage-gen2-for-use-by-synapse-workspace"></a>4. Adım: SYNAPSE çalışma alanı tarafından kullanılmak üzere Data Lake Storage 2. yapılandırma

SYNAPSE çalışma alanı, işlem hatlarını çalıştırmak ve sistem görevleri gerçekleştirmek için STG1 ve CNT1 için erişim gerektirir.

- Azure portal açın
- STG1 bulun
- CNT1 adresine gidin
- WS1 için MSI (Yönetilen Hizmet Kimliği) CNT1 üzerinde **Azure blob veri katılımcısı** rolüne atandığından emin olun
  - Atanmadığını görmüyorsanız, atayın.
  - MSI, çalışma alanıyla aynı ada sahiptir. Bu durumda, &quot;WS1&quot;olacaktır.

## <a name="step-5-configure-admin-access-for-sql-pools"></a>5. Adım: SQL havuzları için yönetici erişimini yapılandırma

- Azure portal açın
- WS1 adresine gidin
- **Ayarlar**altında, **SQL Active Directory Yöneticisi** ' ne tıklayın.
- **Yönetici ayarla** ' ya tıklayın ve\_WS1 SQLAdmins ' yi seçin

## <a name="step-6-maintaining-access-control"></a>6. Adım: erişim denetimini koruma

Yapılandırma tamamlandı.

Artık Kullanıcı erişimini yönetmek için, üç güvenlik grubuna kullanıcı ekleyip çıkarabilirsiniz.

Kullanıcıları SYNAPSE rollerine el ile atayabilmenize karşın, bunu yaparsanız, her şeyi sürekli olarak yapılandırmaz. Bunun yerine, yalnızca güvenlik gruplarına kullanıcı ekleyin veya bunları kaldırın.

## <a name="step-7-verify-access-for-users-in-the-roles"></a>7. Adım: rollerdeki kullanıcılar için erişimi doğrulama

Her roldeki kullanıcıların aşağıdaki adımları tamamlaması gerekir:

|   | Adım | Çalışma alanı yöneticileri | Spark yöneticileri | SQL yöneticileri |
| --- | --- | --- | --- | --- |
| 1 | Bir Parquet dosyasını CNT1 'a yükleme | EVET | EVET | EVET |
| 2 | İsteğe bağlı SQL kullanarak Parquet dosyasını okuyun | EVET | NO | EVET |
| 3 | Spark havuzu oluşturma | EVET [1] | EVET [1] | NO  |
| 4 | Parquet dosyasını bir not defteriyle okur | EVET | EVET | NO |
| 5 | Not defterinden bir işlem hattı oluşturun ve ardışık düzeni şimdi çalışacak şekilde tetikleyin | EVET | NO | NO |
| 6 | SQL havuzu oluşturun ve 1. &quot;Select gıbı bir SQL betiği çalıştırın&quot; | EVET [1] | NO | EVET [1] |

> [!NOTE]
> [1] SQL veya Spark havuzları oluşturmak için kullanıcının SYNAPSE çalışma alanında en az katkıda bulunan rolüne sahip olması gerekir.
> [!TIP]
>
> - Role bağlı olarak bazı adımlara izin verilmeyecektir.
> - Güvenlik tam olarak yapılandırılmamışsa bazı görevlerin başarısız olabileceğini aklınızda bulundurun. Bu görevler tabloda belirtilmiştir.

## <a name="step-8-network-security"></a>8. Adım: ağ güvenliği

Çalışma alanı güvenlik duvarını, sanal ağı ve [özel bağlantıyı](../../sql-database/sql-database-private-endpoint-overview.md)yapılandırmak için.

## <a name="step-9-completion"></a>9. Adım: tamamlama

Çalışma alanınız artık tam olarak yapılandırılmış ve güvenli hale getirildi.

## <a name="how-roles-interact-with-synapse-studio"></a>Roller SYNAPSE Studio ile nasıl etkileşebilir

SYNAPSE Studio, Kullanıcı rollerine göre farklı davranır. Bir kullanıcı uygun erişimi veren rollere atanmamışsa bazı öğeler gizlenebilir veya devre dışı bırakılabilir. Aşağıdaki tabloda SYNAPSE Studio üzerindeki etkisi özetlenmektedir.

| Görev | Çalışma alanı yöneticileri | Spark yöneticileri | SQL yöneticileri |
| --- | --- | --- | --- |
| SYNAPSE Studio 'Yu açın | EVET | EVET | EVET |
| Ana Hub 'ı görüntüle | EVET | EVET | EVET |
| Veri merkezini görüntüle | EVET | EVET | EVET |
| Veri hub 'ı/bkz. bağlantılı ADLSGen2 hesapları ve kapsayıcıları | EVET [1] | EVET [1] | EVET [1] |
| Veri merkezi/bkz. veritabanları | EVET | EVET | EVET |
| Veri merkezi/veritabanlarındaki nesneleri görüntüle | EVET | EVET | EVET |
| SQL havuzu veritabanlarındaki veri merkezi/erişim verileri | EVET   | NO   | EVET   |
| SQL isteğe bağlı veritabanlarında veri merkezi/erişim verileri | EVET [2]  | NO  | EVET [2]  |
| Spark veritabanlarındaki veri merkezi/erişim verileri | EVET [2] | EVET [2] | EVET [2] |
| Geliştirme Merkezi 'ni kullanma | EVET | EVET | EVET |
| Merkez/yazar SQL betikleri geliştirme | EVET | NO | EVET |
| Merkez/yazar Spark Iş tanımları geliştirme | EVET | EVET | NO |
| Merkez/yazar not defterleri geliştirme | EVET | EVET | NO |
| Merkez/yazar veri akışları geliştirin | EVET | NO | NO |
| Orchestrate merkezini kullanma | EVET | EVET | EVET |
| Merkeze göre düzenleme/işlem hatlarını kullanma | EVET | NO | NO |
| Yönetim hub 'ını kullanma | EVET | EVET | EVET |
| Hub/SQL havuzlarını yönetme | EVET | NO | EVET |
| Merkez/Spark havuzlarını yönetme | EVET | EVET | NO |
| Hub/Tetikleyicileri yönetme | EVET | NO | NO |
| Hub/bağlı hizmetleri yönetme | EVET | EVET | EVET |
| Hub/Access Control yönetme (kullanıcıları SYNAPSE çalışma alanı rollerine atama) | EVET | NO | NO |
| Hub/tümleştirme çalışma zamanlarını yönetme | EVET | EVET | EVET |
| Izleyici hub 'ını kullanma | EVET | EVET | EVET |
| Hub/Orchestration/işlem hattı çalıştırmalarını izleme  | EVET | NO | NO |
| Hub/düzenleme/tetikleyici çalıştırmalarını izleme  | EVET | NO | NO |
| Hub/Orchestration/Integration çalışma zamanlarını izleme  | EVET | EVET | EVET |
| Merkez/etkinlik/Spark uygulamalarını izleme | EVET | EVET | NO  |
| Merkezi/etkinlikleri/SQL isteklerini izleme | EVET | NO | EVET |
| Merkezi/etkinlikleri/Spark havuzlarını izleme | EVET | EVET | NO  |
| Hub/Tetikleyicileri izleme | EVET | NO | NO |
| Hub/bağlı hizmetleri yönetme | EVET | EVET | EVET |
| Hub/Access Control yönetme (kullanıcıları SYNAPSE çalışma alanı rollerine atama) | EVET | NO | NO |
| Hub/tümleştirme çalışma zamanlarını yönetme | EVET | EVET | EVET |


> [!NOTE]
> [1] kapsayıcılardaki verilere erişim, ADLSGen2 içindeki erişim denetimine göre değişir. </br>
> [2] SQL OD tabloları ve Spark tabloları, verilerini ADLSGen2 ' de depolar ve erişim, ADLSGen2 üzerinde uygun izinleri gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[SYNAPSE çalışma alanı](../quickstart-create-workspace.md) oluşturma
