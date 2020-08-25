---
title: Kavramlar-depolama
description: Azure VMware Çözüm önizleme özel bulutlarındaki anahtar depolama özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 91484c1baae9801684bc49b931fa2080db676a00
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752216"
---
# <a name="azure-vmware-solution-preview-storage-concepts"></a>Azure VMware Çözüm önizleme depolama kavramları

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

vSAN veri depoları varsayılan olarak rest şifreleme kullanır. Şifreleme çözümü KMS tabanlıdır ve anahtar yönetimi için vCenter işlemlerini destekler. Anahtarlar, HSM tabanlı bir Azure Key Vault ana anahtarı tarafından Sarmalanan şifreli olarak depolanır. Bir konak kümeden herhangi bir nedenden dolayı kaldırıldığında SSD 'Ler üzerindeki veriler hemen geçersiz kılınır.

## <a name="scaling"></a>Ölçeklendirme

Yerel küme depolama kapasitesi, bir kümeye ana bilgisayar eklenerek ölçeklendirilir. Ana bilgisayarları kullanan kümeler için, ham küme genelinde kapasite, her ek ana bilgisayar ile 15,4 TB artar. GP konaklarıyla oluşturulan kümelerin ham kapasitesi, her ek konakla 7,7 TB artmıştır. Her iki tür küme içinde, ana bilgisayarların bir kümeye eklenmesi yaklaşık 10 dakika sürer. Kümeleri ölçeklendirmeyle ilgili yönergeler için bkz. [özel bulutu ölçeklendirme öğreticisi][tutorial-scale-private-cloud] .

## <a name="azure-storage-integration"></a>Azure depolama tümleştirmesi

Azure Storage hizmetlerini özel bulutunuzda çalışan iş yükleri üzerinde kullanabilirsiniz. Azure depolama hizmetleri, depolama hesapları, tablo depolama ve BLOB depolama alanı içerir. Azure depolama hizmetlerine iş yüklerinin bağlantısı internet 'e çapraz geçiş yapmaz. Bu bağlantı, ek güvenlik sağlar ve özel bulut iş yüklerinizde SLA tabanlı Azure depolama hizmetlerini kullanmanıza olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, [özel bulut kimliği kavramları][concepts-identity]hakkında bilgi edinirsiniz.

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
