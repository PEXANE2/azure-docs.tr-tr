---
title: Synapse çalışma alanınızı güvenli hale (önizleme)
description: Bu makalede, synapse çalışma alanında verileri kontrol etmek ve verilere erişim için rolleri ve erişim denetimi nasıl kullanılacağını öğretecektir.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: jrasnick
ms.openlocfilehash: ae8be848b5d12e01865fe6bd3b394b460252aa3e
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606001"
---
# <a name="secure-your-synapse-workspace-preview"></a>Synapse çalışma alanınızı güvenli hale (önizleme)

Bu makalede, rolleri nasıl kullanacağınızı ve denetim etkinlikleri ve verilere erişim denetimine nasıl erişeceğiniz öğretilir. Bu yönergeleri izleyerek, Azure Synapse Analytics'te erişim denetimi basitleştirilmiştir. Kullanıcıları yalnızca üç güvenlik grubundan birine eklemeniz ve kaldırmanız gerekir.

## <a name="overview"></a>Genel Bakış

Bir Synapse çalışma alanını (önizleme) güvenli hale getirmek için, aşağıdaki öğeleri yapılandırma deseni izlersiniz:

- Azure rolleri (Sahibi, Katılımcı, vb. gibi yerleşik roller gibi)
- Synapse rolleri – bu roller Synapse'ye özgüdir ve Azure rollerine dayanmaz. Bu rollerden üçü vardır:
  - Synapse çalışma alanı yöneticisi
  - Synapse SQL admin
  - Sinaps Kıvılcım admin
- Azure Veri Gölü Depolama Gen 2'deki (ADLSGEN2) veriler için erişim denetimi.
- Synapse SQL ve Spark veritabanları için erişim denetimi

## <a name="steps-to-secure-a-synapse-workspace"></a>Synapse çalışma alanını güvence altına alma adımları

Bu belge, yönergeleri basitleştirmek için standart adlar kullanır. Seçtiğiniz herhangi bir ad ile değiştirin.

|Ayar | Örnek değer | Açıklama |
| :------ | :-------------- | :---------- |
| **Sinaps çalışma alanı** | WS1 |  Synapse çalışma alanının sahip olacağı ad. |
| **ADLSGEN2 hesabı** | STG1 | Çalışma alanınızla birlikte kullanılacak ADLS hesabı. |
| **Kapsayıcı** | CNT1 | Çalışma alanının varsayılan olarak kullanacağı STG1'deki kapsayıcı. |
| **Etkin dizin kiracı** | contoso | etkin dizin kiracı adı.|
||||

## <a name="step-1-set-up-security-groups"></a>ADIM 1: Güvenlik grupları ayarlama

Çalışma alanınız için üç güvenlik grubu oluşturun ve doldurun:

- **WS1\_WSAdmins** - çalışma alanı üzerinde tam kontrole ihtiyacı olan kullanıcılar için
- **WS1\_SparkAdmins** - çalışma alanının Kıvılcım yönleri üzerinde tam kontrole ihtiyacı olan kullanıcılar için
- **WS1\_SQLAdmins** – çalışma alanının SQL yönleri üzerinde tam denetime ihtiyaç duyan kullanıcılar için
- **WS1\_SQLAdmins WS1** **\_WSAdmins** ekleme
- **WS1\_SparkAdmins** **WS1\_WSAdmins** ekle

## <a name="step-2-prepare-your-data-lake-storage-gen2-account"></a>ADIM 2: Veri Gölü Depolama Gen2 hesabınızı hazırlayın

Depolama alanınız hakkındaki bu bilgileri tanımlayın:

- Çalışma alanınız için kullanılacak ADLSGEN2 hesabı. Bu belge buna STG1 diyor.  STG1, çalışma alanınız için "birincil" depolama hesabı olarak kabul edilir.
- Synapse çalışma alanınızın varsayılan olarak kullanacağı WS1 içindeki kapsayıcı. Bu belge buna CNT1 diyor.  Bu kapsayıcı için kullanılır:
  - Spark tabloları için destek veri dosyalarını depolama
  - Kıvılcım işleri için yürütme günlükleri

- Azure portalını kullanarak, güvenlik gruplarını CNT1'de aşağıdaki rolleri atayın

  - **DEPOLAMA Blob Veri Katılımcısı** rolüne **WS1\_WSAdmin'leri** atama
  - **DEPOLAMA Blob Veri Katılımcısı** rolüne **WS1\_SparkAdmins** atama
  - **DEPOLAMA Blob Veri Katılımcısı** rolüne **WS1\_SQLAdmins** atama

## <a name="step-3-create-and-configure-your-synapse-workspace"></a>ADIM 3: Synapse Çalışma Alanınızı oluşturun ve yapılandırın

Azure portalında bir Synapse çalışma alanı oluşturun:

- Çalışma alanını WS1 olarak adlandırın
- Depolama hesabı için STG1'i seçin
- "Dosya sistemi" olarak kullanılan kapsayıcı için CNT1'i seçin.
- Synapse Studio'da WS1'i açın
- **Erişim Denetimini** **Yönet'i** > seçin güvenlik gruplarını aşağıdaki Synapse rollerine atayın.
  - SYNApse Workspace yöneticilerine **WS1\_WSAdmin'leri** atama
  - Synapse Spark yöneticilerine **WS1\_SparkAdmins** atama
  - Synapse SQL yöneticilerine **WS1\_SQLAdmins** atama

## <a name="step-4-configuring-data-lake-storage-gen2-for-use-by-synapse-workspace"></a>ADIM 4: Synapse çalışma alanı tarafından kullanılmak üzere Veri Gölü Depolama Gen2 yapılandırma

Synapse çalışma alanının, ardışık hatlar çalıştırabilmesi ve sistem görevlerini gerçekleştirebilmesi için STG1 ve CNT1'e erişmesi gerekir.

- Azure portalını açın
- STG1'i bul
- CNT1'e git
- WS1 için MSI (Yönetilen Hizmet Kimliği) CNT1'deki **Azure Blob Veri Katılımcısı** rolüne atanmış olduğundan emin olun
  - Atanmış görmüyorsanız, atayın.
  - MSI çalışma alanıyla aynı ada sahiptir. Bu durumda, WS1 &quot;&quot;olacaktır.

## <a name="step-5-configure-admin-access-for-sql-pools"></a>ADIM 5: SQL havuzları için yönetici erişimini yapılandırma

- Azure portalını açın
- WS1'e gidin
- **Ayarlar**altında, **SQL Active Directory admin'e** tıklayın
- **Yöneticiyi Ayarla'yı** tıklatın ve WS1\_SQLAdmins'i seçin

## <a name="step-6-maintaining-access-control"></a>ADIM 6: Erişim denetimini koruma

Yapılandırma tamamlandı.

Artık, kullanıcıların erişimini yönetmek için, kullanıcıları üç güvenlik grubuna ekleyebilir ve kaldırabilirsiniz.

Kullanıcıları Synapse rollerine el ile atayabilirsiniz, ancak bunu yaparsanız, bu işleri tutarlı bir şekilde yapılandırmaz. Bunun yerine, yalnızca güvenlik gruplarına kullanıcıları ekleyin veya kaldırın.

## <a name="step-7-verify-access-for-users-in-the-roles"></a>ADIM 7: Rollerdeki kullanıcılar için erişimi doğrulayın

Her roldeki kullanıcıların aşağıdaki adımları tamamlamaları gerekir:

|   | Adım | Çalışma alanı yöneticileri | Kıvılcım yöneticileri | SQL yöneticileri |
| --- | --- | --- | --- | --- |
| 1 | Parke dosyayı CNT1'e yükleme | EVET | EVET | EVET |
| 2 | İsteğe bağlı OLARAK SQL kullanarak parke dosyasını okuyun | EVET | NO | EVET |
| 3 | Kıvılcım havuzu oluşturma | EVET [1] | EVET [1] | NO  |
| 4 | Parke dosyasını Not Defteri ile okur | EVET | EVET | NO |
| 5 | Not Defteri'nden bir ardışık kaynak oluşturma ve şimdi çalışacak ardışık hattı tetikle | EVET | NO | NO |
| 6 | SQL Havuzu oluşturun ve SELECT 1 &quot;gibi bir SQL komut dosyası çalıştırın&quot; | EVET [1] | NO | EVET[1] |

> [!NOTE]
> [1] SQL veya Spark havuzları oluşturmak için kullanıcının Synapse çalışma alanında en az Katkıda Bulunan rolü olmalıdır.
> [!TIP]
>
> - Bazı adımlar alameti, role bağlı olarak kasıtlı olarak izin verilmeyecektir.
> - Güvenlik tam olarak yapılandırılmamışsa bazı görevlerin başarısız olabileceğini unutmayın. Bu görevler tabloda belirtilir.

## <a name="step-8-network-security"></a>ADIM 8: Ağ Güvenliği

Çalışma alanı güvenlik duvarını, sanal ağı ve [Özel Bağlantıyı](../../sql-database/sql-database-private-endpoint-overview.md)yapılandırmak için.

## <a name="step-9-completion"></a>ADIM 9: Tamamlanma

Çalışma alanınız artık tamamen yapılandırılmış ve güvenli.

## <a name="how-roles-interact-with-synapse-studio"></a>Synapse Studio ile roller nasıl etkileşir?

Synapse Studio kullanıcı rollerine göre farklı davranacaktır. Kullanıcı uygun erişimi sağlayan rollere atanmazsa, bazı öğeler gizlenebilir veya devre dışı bırakılabilir. Aşağıdaki tablo synapse Studio üzerindeki etkisini özetleyilmiştir.

| Görev | Çalışma Alanı Yöneticileri | Kıvılcım yöneticileri | SQL yöneticileri |
| --- | --- | --- | --- |
| Açık Synapse Studio | EVET | EVET | EVET |
| Ana Sayfa merkezini görüntüle | EVET | EVET | EVET |
| Veri Merkezini Görüntüle | EVET | EVET | EVET |
| Veri Hub / Bkz. bağlantılı ADLSGen2 hesapları ve konteynerler | EVET [1] | EVET[1] | EVET[1] |
| Veri Hub / Veritabanlarına Bakın | EVET | EVET | EVET |
| Veri Hub / Veritabanlarındaki nesneleri gör | EVET | EVET | EVET |
| SQL havuz veritabanlarında Veri Hub / Erişim verileri | EVET   | NO   | EVET   |
| SQL isteğe bağlı veritabanlarında Veri Hub / Erişim verileri | EVET [2]  | NO  | EVET [2]  |
| Veri Hub / Spark veritabanlarındaki verilere erişim | EVET [2] | EVET [2] | EVET [2] |
| Geliştirme merkezini kullanma | EVET | EVET | EVET |
| Hub / yazar SQL Scripts geliştirin | EVET | NO | EVET |
| Hub / yazar Spark İş Tanımları geliştirin | EVET | EVET | NO |
| Hub / yazar Not Defterleri geliştirin | EVET | EVET | NO |
| Hub / yazar Dataflows geliştirme | EVET | NO | NO |
| Orkestral hub'ı kullanma | EVET | EVET | EVET |
| Hub'ı düzenleme / Boru Hatlarını kullanma | EVET | NO | NO |
| Yönet Merkezini Kullanma | EVET | EVET | EVET |
| Hub / SQL havuzlarını yönetme | EVET | NO | EVET |
| Hub / Kıvılcım havuzlarını yönetin | EVET | EVET | NO |
| Hub / Tetikleyicileri Yönet | EVET | NO | NO |
| Hub / Bağlantılı hizmetleri yönetme | EVET | EVET | EVET |
| Hub /Erişim Denetimini Yönet (kullanıcıları Synapse çalışma alanı rollerine atama) | EVET | NO | NO |
| Hub / Tümleştirme çalışma sürelerini yönetme | EVET | EVET | EVET |

> [!NOTE]
> [1] Kapsayıcıdaki verilere erişim, ADLSGen2[2] SQL OD tablolarındaki erişim denetimine bağlıdır ve Spark tabloları verilerini ADLSGen2'de saklar ve erişim ADLSGen2'de uygun izinleri gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Synapse Çalışma Alanı](../quickstart-create-workspace.md) Oluşturma
