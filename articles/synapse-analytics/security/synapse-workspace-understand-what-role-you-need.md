---
title: SYNAPSE 'de ortak görevleri gerçekleştirmek için gereken rolleri anlayın
description: Bu makalede, belirli görevleri gerçekleştirmek için hangi yerleşik SYNAPSE RBAC rollerinin gerekli olduğu açıklanır
author: billgib
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: security
ms.date: 12/1/2020
ms.author: billgib
ms.reviewer: jrasnick
ms.openlocfilehash: aadc8e817eb2b5de856ac73cfd010b48d0531bfc
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523627"
---
# <a name="understand-the-roles-required-to-perform-common-tasks-in-synapse"></a>SYNAPSE 'de ortak görevleri gerçekleştirmek için gereken rolleri anlayın

Bu makale, SYNAPSE Studio 'da yapılması gereken SYNAPSE RBAC (rol tabanlı erişim denetimi) veya Azure RBAC rollerinin ne olduğunu anlamanıza yardımcı olur.  

## <a name="synapse-studio-access-control-and-workflow-summary"></a>SYNAPSE Studio erişim denetimi ve iş akışı Özeti 

### <a name="accessing-synapse-studio-and-viewing-its-content"></a>SYNAPSE Studio 'Ya erişme ve içeriğini görüntüleme

- Herhangi bir Synapse RBAC rolü atandıysa veya çalışma alanında Azure sahibi, katkıda bulunan veya okuyucu rolüne sahipseniz, SYNAPSE Studio 'Yu açabilir ve çalışma alanının ayrıntılarını görüntüleyebilir ve Azure kaynaklarından (SQL havuzları, Spark havuzları veya tümleştirme çalışma zamanları) herhangi birini listeleyebilirsiniz.

### <a name="resource-management"></a>Kaynak yönetimi

- Çalışma alanında bir Azure sahibiyseniz veya katkıda bulunsanız, SQL havuzları, Apache Spark havuzları ve tümleştirme çalışma zamanları oluşturabilirsiniz.
- Özel bir SQL havuzunu duraklatabilir veya ölçeklendirebilirsiniz, bir Azure sahibiyseniz veya bir tümleştirme çalışma zamanı yapılandırabilir veya çalışma alanında ya da kaynak üzerinde katkıda bulunmanız gerekir.

### <a name="viewing-and-editing-code-artifacts"></a>Kod yapılarını görüntüleme ve düzenlemeyle

- SYNAPSE Studio erişimi sayesinde SQL betikleri, Not defterleri, Spark işleri, bağlı hizmetler, işlem hatları, veri akışları, Tetikleyiciler ve kimlik bilgileri gibi yeni kod yapıtları oluşturabilirsiniz.  (Bu yapıtlar, ek izinlerle yayımlanabilir veya kaydedilebilir.)  
- SYNAPSE yapıtı bir Kullanıcı, SYNAPSE yapıt yayımcısı, SYNAPSE katılımcısı veya SYNAPSE Yöneticisi kullanıyorsanız, önceden yayınlanmış kod yapılarını listeleyebilir, açabilir ve düzenleyebilirsiniz.

### <a name="executing-your-code"></a>Kodunuzu yürütme

- SQL havuzlarında gerekli SQL izinleriniz varsa SQL havuzları üzerinde SQL betikleri çalıştırabilirsiniz.  
- Çalışma alanında veya belirli Apache Spark havuzlarda SYNAPSE Işlem Işletmeni izinleriniz varsa, not defterlerini ve Spark işlerini çalıştırabilirsiniz.  
- Çalışma alanı veya belirli tümleştirme çalışma zamanları üzerinde Işlem Işletmeni izinleri ve uygun kimlik bilgileri izinleri ile Işlem hatlarını çalıştırabilirsiniz.

### <a name="monitoring-and-managing-execution"></a>Yürütmeyi izleme ve yönetme
- SYNAPSE bir Kullanıcı kullanıyorsanız, Apache Spark havuzlarında çalışan Not defterlerinin ve işlerin durumunu inceleyebilirsiniz.
- Çalışma alanında veya belirli bir Spark havuzu ya da Işlem hattı için SYNAPSE Işlem operatörü kullanıyorsanız, günlükleri gözden geçirebilir ve çalışan işleri ve işlem hatlarını iptal edebilirsiniz.  

### <a name="publishing-and-saving-your-code"></a>Kodunuzu yayımlama ve kaydetme

- SYNAPSE yapıt yayımcısı, SYNAPSE katılımcısı veya SYNAPSE yöneticisiyseniz, hizmette yeni veya güncelleştirilmiş kod yapıtları yayımlayabilirsiniz. 
- Çalışma alanı git etkinse ve git izinleriniz varsa, git deposunun çalışma dalına kod yapıtlarını kaydedebilirsiniz. Git etkin olduğunda, yalnızca işbirliği dalından yayımlamaya izin verilir.
- Kod yapıtlarına yapılan değişiklikleri yayımlamadan veya işlemeden SYNAPSE Studio 'Yu kapatırsanız, bu değişiklikler kaybedilir.


## <a name="tasks-and-required-roles"></a>Görevler ve gerekli roller

Aşağıdaki tabloda, ortak görevler ve her görev, SYNAPSE RBAC veya Azure RBAC rolleri listelenmektedir.  

>[!Note]
>- Gerekli izni sağlayan tek rol olmadığı takdirde, SYNAPSE Yöneticisi her bir görev için listelenmez.  SYNAPSE Yöneticisi, diğer SYNAPSE RBAC rolleri tarafından etkinleştirilen tüm görevleri gerçekleştirebilir.</br>
>- Herhangi bir kapsamdaki tüm SYNAPSE RBAC rolleri, çalışma alanında SYNAPSE Kullanıcı izinleri sağlar
>- Tabloda gösterilen tüm SYNAPSE RBAC izinleri/eylemleri önekli Microsoft/SYNAPSE/Workspaces/... </br>


Görev (istiyorum...) |Rol (yapmam gerekiyor...)|SYNAPSE RBAC izni/eylemi
--|--|--
|Çalışma alanında SYNAPSE Studio 'Yu açma|SYNAPSE kullanıcısı veya|read
| |Çalışma alanında Azure sahibi, katkıda bulunan veya okuyucu|yok
|SQL havuzlarını, Apache Spark havuzlarını, tümleştirme çalışma zamanlarını listeleyin ve yapılandırma ayrıntılarına erişin|SYNAPSE kullanıcısı veya|read|
||Çalışma alanında Azure sahibi, katkıda bulunan veya okuyucu|yok
|Bağlı hizmetleri, kimlik bilgilerini, yönetilen özel uç noktaları listeleyin|SYNAPSE kullanıcısı|read
**SQL havuzları**||
Adanmış bir SQL havuzu veya sunucusuz SQL havuzu oluşturma|Çalışma alanında Azure sahibi veya katkıda bulunan|yok
Adanmış bir SQL havuzunu yönetme (duraklatma, ölçekleme veya silme)|SQL havuzunda veya çalışma alanında Azure sahibi veya katkıda bulunan|yok
SQL betiği oluşturma</br>|SYNAPSE kullanıcısı veya </br>Çalışma alanında Azure sahibi veya katkıda bulunan </br>*SQL betiğini çalıştırmak Için ek SQL izinleri gereklidir*.|
Yayınlanan tüm SQL betiğini listeleyin ve açın| SYNAPSE yapıt kullanıcısı, yapıt yayımcısı, SYNAPSE katkıda bulunan|yapıtlar/okuma
Sunucusuz SQL havuzunda SQL betiği çalıştırma|Havuzda SQL izinleri (bir Synapse yöneticisine otomatik olarak verilir)|yok
Adanmış bir SQL havuzunda SQL betiği çalıştırma|Havuzda SQL izinleri gerektirir|yok
Yeni, güncelleştirilmiş veya silinmiş bir SQL betiği yayımlama|SYNAPSE yapıt yayımcısı, SYNAPSE katkıda bulunan|sqlScripts/Write, Sil
Bir SQL komut dosyasında değişiklikleri git deposuna işleme|Depoda git izinleri gerekir|
Çalışma alanına yönetici Active Directory atama (Azure portalındaki çalışma alanı özellikleri aracılığıyla)|Çalışma alanında Azure sahibi veya katkıda bulunan |
**Apache Spark havuzları**||
Apache Spark havuzu oluşturma|Çalışma alanında Azure sahibi veya katkıda bulunan|
Apache Spark uygulamalarını izleme| SYNAPSE kullanıcısı|read
Not defteri ve iş yürütmeye yönelik günlükleri görüntüleme |SYNAPSE Işlem Işleci|
Apache Spark havuzunda çalışan herhangi bir not defteri veya Spark işini iptal etme|Apache Spark havuzunda SYNAPSE Işlem Işletmeni.|bigDataPools/useCompute
Bir not defteri veya iş tanımı oluşturun|Çalışma alanında SYNAPSE Kullanıcı veya Azure sahibi, katkıda bulunan veya okuyucu</br> *Çalıştırmak, yayımlamak veya kaydetmek için ek izinler gerekir*|read
Kaydedilmiş çıktıları gözden geçirme dahil yayımlanmış bir not defteri veya iş tanımı listeleyin ve açın|Çalışma alanında SYNAPSE yapıt kullanıcısı, SYNAPSE yapıt yayımcısı, SYNAPSE katkıda bulunan|yapıtlar/okuma
Bir not defteri çalıştırın ve çıkışını gözden geçirin|SYNAPSE Apache Spark Administrator, SYNAPSE COMPUTE Işleci seçili Apache Spark havuzunda|bigDataPools/useCompute 
Hizmete bir not defteri veya iş tanımı (çıktı dahil) yayımlama veya silme|Çalışma alanında yapıt yayımcısı, SYNAPSE Apache Spark Yöneticisi|Not defterleri/yazma, silme
Değişiklikleri bir not defterine veya iş tanımına git çalışma dalına Kaydet|Git izinleri|yok
**İşlem hatları, tümleştirme çalışma zamanları, veri akışları, veri kümeleri ve Tetikleyiciler**||
Tümleştirme çalışma zamanı oluşturma, güncelleştirme veya silme|Çalışma alanında Azure sahibi veya katkıda bulunan|
Tümleştirme çalışma zamanı durumunu izleme|SYNAPSE kullanıcısı|okuma, işlem hatları/Viewçıktılar
İşlem hattı çalıştırmalarını gözden geçirme|SYNAPSE yapıt yayımcısı/SYNAPSE katkıda bulunan|okuma, işlem hatları/Viewçıktılar 
İşlem hattı oluşturma |SYNAPSE kullanıcısı </br>[**_WorkspaceSystemIdentity _ üzerinde değerlendirme + SYNAPSE kimlik bilgisi kullanıcısı_*)</br>Yayımlamak veya kaydetmek için _Additional izinleri gerekir *|okuma, kimlik bilgileri/UseSecret/eylem
Veri akışı, veri kümesi veya tetikleyici oluşturma |SYNAPSE kullanıcısı</br>*Yayımlamak veya kaydetmek için ek izinler gerekir*|read
Yayımlanmış bir işlem hattını listeleme ve açma |SYNAPSE yapıt kullanıcısı | yapıtlar/okuma
Veri kümesi verilerini Önizle|SYNAPSE Kullanıcı + SYNAPSE kimlik bilgisi kullanıcısı WorkspaceSystemIdentity| 
Varsayılan tümleştirme çalışma zamanını kullanarak bir işlem hattının hatalarını ayıklama|WorkspaceSystemIdentity kimlik bilgilerinde Kullanıcı + SYNAPSE kimlik bilgisi kullanıcısı SYNAPSE|okuyamaz </br>kimlik bilgileri/useSecret
Şimdi Tetikle dahil bir tetikleyici oluşturun|SYNAPSE Kullanıcı + SYNAPSE kimlik bilgisi kullanıcısı WorkspaceSystemIdentity|okuma, kimlik bilgileri/useSecret/eylem
Veri Kopyalama aracını kullanarak veri kopyalama|SYNAPSE Kullanıcı + SYNAPSE kimlik bilgileri Kullanıcı çalışma alanı sistem kimliği|okuma, kimlik bilgileri/useSecret/eylem
Veri alma (bir zamanlama kullanarak)|SYNAPSE Author + SYNAPSE, çalışma alanı sistem kimliği üzerinde Kullanıcı kimlik bilgileri|okuma, kimlik bilgileri/useSecret/eylem
Yeni, güncelleştirilmiş veya silinmiş bir işlem hattını, veri akışını veya tetikleyiciyi hizmete yayımlayın|Çalışma alanında SYNAPSE yapıt yayımcısı|işlem hatları/yazma, silme</br>veri akışları yazma, silme</br>Tetikleyiciler/yazma, silme
Yeni, güncelleştirilmiş veya silinmiş bir veri akışı, veri kümesi veya tetikleyiciyi hizmete yayımlayın|Çalışma alanında yapıt yayımcısı|Tetikleyiciler/yazma, silme
İşlem hatlarına, veri akışlarına, veri kümelerine ve değişiklikleri git deposuna Kaydet (Yürüt) |Git izinleri|yok 
**Bağlı hizmetler**||
Bağlı hizmet oluşturma (kimlik bilgisi atamayı içerir)|SYNAPSE kullanıcısı</br>*Çalıştırmak, yayımlamak veya kaydetmek için ek izinler gerekir*|read
Yayınlanan bağlı hizmeti listeleyin ve açın|SYNAPSE yapıt kullanıcısı|linkedServices/Write, Sil  
Bir kimlik bilgisi ile korunan bağlı bir hizmette test bağlantısı|SYNAPSE User ve SYNAPSE Credential User|kimlik bilgileri/useSecret/eylem|
Bağlı hizmet yayımlama|SYNAPSE yapıt yayımcısı|linkedServices/Write, Sil
Bağlı hizmet tanımlarını git deposuna Kaydet (Yürüt)|Git izinleri|yok
**Erişim yönetimi**||
Herhangi bir kapsamdaki SYNAPSE RBAC rol atamalarını gözden geçirin|SYNAPSE kullanıcısı|read
Kullanıcılar, gruplar ve hizmet sorumluları için SYNAPSE RBAC rol atamaları atama ve kaldırma| Çalışma alanında veya belirli bir çalışma alanı öğesi kapsamında SYNAPSE Yöneticisi|Roleatamalar/yazma, silme
Kod yapılarına SYNAPSE RBAC erişimi oluşturma veya kaldırma|SYNAPSE Yöneticisi çalışma alanı kapsamında|Roleatamalar/yazma, silme   

>[!Note]
>Başka bir kiracıya ait Konuk kullanıcılar atanan rolden bağımsız olarak rol atamalarını gözden geçiremez, ekleyemez veya değiştiremezler. 

## <a name="next-steps"></a>Sonraki adımlar

[SYNAPSE RBAC rol atamalarını incelemeyi](./how-to-review-synapse-rbac-role-assignments.md) öğrenin

[SYNAPSE RBAC rol atamalarını yönetmeyi](./how-to-manage-synapse-rbac-role-assignments.md)öğrenin. 