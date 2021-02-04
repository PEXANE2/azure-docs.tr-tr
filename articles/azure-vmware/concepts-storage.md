---
title: Kavramlar-depolama
description: Azure VMware Çözüm özel bulutlarında anahtar depolama özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 522e4f651b36532ac0c144b3889b2b67c91dc77b
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536959"
---
#  <a name="azure-vmware-solution-storage-concepts"></a>Azure VMware Çözüm depolama kavramları

Azure VMware çözümü özel bulutları, VMware vSAN ile yerel, küme genelinde depolama sağlar. Bir kümedeki her bir konaktaki tüm yerel depolama birimi bir vSAN veri deposunda kullanılır ve varsayılan olarak, bekleyen veri şifrelemesi kullanılabilir ve etkinleştirilir. Azure depolama kaynaklarını kullanarak özel bulutlarınızın depolama özelliklerini genişletebilirsiniz.

## <a name="vsan-clusters"></a>vSAN kümeleri

Her küme konağındaki yerel depolama alanı bir vSAN veri deposunun parçası olarak kullanılır. Tüm diskgroups, 1,6 TB 'lık bir NVMe önbellek katmanını, ana bilgisayar başına, SSD tabanlı 2 TB 15,4 kapasitesini kullanır. Kümenin ham kapasite katmanının boyutu, konak sayısının ana bilgisayar kapasitesinin süreleriyle aynı olur. Örneğin, dört ana bilgisayar kümesi vSAN kapasite katmanında 61,6 TB ham kapasite sağlar.

Küme ana bilgisayarlarındaki yerel depolama alanı, küme genelinde vSAN veri deposunda kullanılır. Tüm veri depoları özel bulut dağıtımının bir parçası olarak oluşturulur ve hemen kullanılabilir. Cloudadmin kullanıcısı ve CloudAdmin grubundaki tüm kullanıcılar, bu vSAN ayrıcalıklarıyla veri depolarını yönetebilir:
- DataStore. AllocateSpace
- Datastore.Browse
- Datastore.Config
- DataStore. DeleteFile
- DataStore. FileManagement
- DataStore. UpdateVirtualMachineMetadata

## <a name="data-at-rest-encryption"></a>Bekleyen veri şifrelemesi

vSAN veri depoları varsayılan olarak rest şifreleme kullanır. Şifreleme çözümü KMS tabanlıdır ve anahtar yönetimi için vCenter işlemlerini destekler. Anahtarlar şifreli olarak depolanır ve bir Azure Key Vault ana anahtarıyla sarmalanır. Bir konak kümeden herhangi bir nedenden dolayı kaldırıldığında SSD 'Ler üzerindeki veriler hemen geçersiz kılınır.

## <a name="scaling"></a>Ölçeklendirme

Yerel küme depolama kapasitesi, bir kümeye ana bilgisayar eklenerek ölçeklendirilir. Ana bilgisayarları kullanan kümeler için, ham küme genelinde kapasite, eklenen her konakla 15,4 TB ile artırılır. GP konaklarıyla oluşturulan kümelerin ham kapasitesi, eklenen her konak ile 7,7 TB artar. Her iki tür küme içinde, ana bilgisayarların bir kümeye eklenmesi yaklaşık 10 dakika sürer. Kümeleri ölçeklendirmeyle ilgili yönergeler için bkz. [özel bulutu ölçeklendirme öğreticisi][tutorial-scale-private-cloud].

## <a name="azure-storage-integration"></a>Azure depolama tümleştirmesi

Azure Storage hizmetlerini özel bulutunuzda çalışan iş yükleri üzerinde kullanabilirsiniz. Azure depolama hizmetleri, depolama hesapları, tablo depolama ve BLOB depolama alanı içerir. Azure depolama hizmetlerine iş yüklerinin bağlantısı internet 'e çapraz geçiş yapmaz. Bu bağlantı daha fazla güvenlik sağlar ve özel bulut iş yüklerinizde SLA tabanlı Azure depolama hizmetlerini kullanmanıza olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure VMware Çözüm depolama kavramlarını kapsadığınıza göre, şunları öğrenmek isteyebilirsiniz:

- [Özel bulut kimliği kavramları](concepts-identity.md).
- [Azure VMware çözümü Için vSphere rol tabanlı erişim denetimi](concepts-role-based-access-control.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
