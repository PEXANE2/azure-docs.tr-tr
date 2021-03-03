---
title: Azure Percept güvenliğine genel bakış
description: Azure Percept güvenliği hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: b5a345139114842c83cb1f11792076efb1461870
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664102"
---
# <a name="azure-percept-security-overview"></a>Azure Percept güvenliğine genel bakış

Azure Percept DK cihazları, her cihazda yerleşik güvenlik olmak üzere, bir güven donanım köküyle tasarlanmıştır. Kameralar ve mikrofonlar, çıkarım verileri gibi gizlilik açısından duyarlı algılayıcıları korumanıza yardımcı olur ve Azure Percept Studio Hizmetleri için cihaz kimlik doğrulaması ve yetkilendirmesini sağlar.

> [!NOTE]
> Azure Percept DK, geliştirme ve test ortamlarında ve kavram kanıtı senaryolarında kullanılmak üzere tasarlanmıştır.

## <a name="devices"></a>Cihazlar

### <a name="azure-percept-dk"></a>Azure Percept DK

Azure Percept DK, cihazı Azure cihaz sağlama hizmetleri 'ne güvenli bir şekilde bağlamak için kullanılabilecek Güvenilir Platform Modülü (TPM) 2,0 sürümünü içerir. TPM, Trusted Computing Group bir sektör genelinde, ISO standardıdır ve TPM hakkında daha fazla bilgi edinmek için [tam tpm 2,0 belirtiminde](https://trustedcomputinggroup.org/resource/tpm-library-specification/) veya ISO/IEC 11889 belirtiminde daha fazla bilgi edinebilirsiniz. DPS cihazları güvenli bir şekilde nasıl sağlayabildiği hakkında daha fazla bilgi için bkz. [Azure IoT Hub cihaz sağlama hizmeti-TPM kanıtlama](https://docs.microsoft.com/azure/iot-dps/concepts-tpm-attestation).

### <a name="azure-percept-system-on-module-som"></a>Modül üzerinde Azure Percept System (SOM)

Azure Percept DK Vision-etkinleştirilmiş sistem modülü (SOM) ve Azure Percept Audio donatısı SOM, gömülü AI algılayıcılarının erişimini korumak için mikro denetleyici birimi (MCU) içerir. Her önyüklemede, MCU bellenimi, cihaz tanımlayıcı bileşim altyapısı (zar) mimarisini kullanarak Azure Percept Studio Hizmetleri ile AI hızlandırıcısının kimliğini doğrular ve yetkilendirir. ZAR, katmanlara önyükleme gerçekleştirerek ve benzersiz bir cihaz gizli dizisi (UDS) temelinde her bir katmana ve yapılandırmaya özgü gizli dizileri oluşturarak çalışır. Farklı kod veya yapılandırma önyüklenmiyorsa, zincirdeki herhangi bir noktada gizlilikler farklı olur. Zar [çalışma grubu belirtiminde](https://trustedcomputinggroup.org/work-groups/dice-architectures/), zar hakkında daha fazla bilgi edinebilirsiniz. Azure Percept Studio ve gerekli hizmetlere erişimi yapılandırmak için aşağıdaki **Azure için güvenlik duvarlarını yapılandırma PERCEPT dk** bölümüne bakın.

Azure Percept cihazları, bellenimi güvenli hale getirmek için donanım kök güvenini kullanır. Önyükleme ROM 'U, ROM ve işletim sistemi (OS) yükleyicisi arasındaki üretici yazılımının bütünlüğünü sağlar ve bu da bir güven zinciri oluşturan diğer yazılım bileşenlerinin bütünlüğünü sağlar.

## <a name="services"></a>Hizmetler

### <a name="iot-edge"></a>IoT Edge

Azure Percept DK, Azure Percept Studio 'ya ve Aktarım Katmanı Güvenliği (TLS) protokolü kullanan diğer Azure hizmetlerine güvenli bir şekilde bağlanır. Azure Percept DK, Azure IoT Edge özellikli bir cihazdır. IoT Edge Runtime, bir cihazı IoT Edge cihazına veren bir programlar koleksiyonudur. Toplu olarak, IoT Edge çalışma zamanı bileşenleri, IoT Edge cihazların kenarda çalışacak kodu almasını ve sonuçları iletmelerini sağlar. Azure Percept DK, ana bilgisayar işletim sistemi ve kenar özellikli uygulamalardan IoT Edge iş yüklerini yalıtmak için Docker kapsayıcılarını kullanır. Azure IoT Edge güvenlik çerçevesi hakkında daha fazla bilgi için [IoT Edge Güvenlik Yöneticisi](https://docs.microsoft.com/azure/iot-edge/iot-edge-security-manager?view=iotedge-2018-06)hakkında makalesini okuyun.

### <a name="device-update-for-iot-hub"></a>IoT Hub için cihaz güncelleştirmesi

IoT Hub için cihaz güncelleştirmesi, Azure Percept cihazlarına yenilenebilir güvenlik sağlayan güvenli, ölçeklenebilir ve güvenilir, kablosuz bir güncelleştirme sağlar. Öngörüler aracılığıyla zengin yönetim denetimleri ve güncelleştirme uyumluluğu sağlar. Azure Percept DK, üretici yazılımından işletim sistemi katmanlarına dayanıklı güncelleştirme (A/B) sağlayan önceden tümleştirilmiş bir cihaz güncelleştirme çözümü içerir.

<!---I think the below topics need to be somewhere else, (i.e. not on the main page)
--->

## <a name="configuring-firewalls-for-azure-percept-dk"></a>Azure Percept DK için güvenlik duvarlarını yapılandırma

Ağ Kurulum, Azure Percept DK cihazlarından yapılan bağlantılara açıkça izin vermenizi gerektiriyorsa, aşağıdaki bileşen listesini gözden geçirin.

Bu denetim listesi, güvenlik duvarı kuralları için bir başlangıç noktasıdır:

|URL (* = joker karakter) |Giden TCP bağlantı noktaları|    Kullanım|
|-------------------|------------------|---------|
|*. auth.azureperceptdk.azure.net|   443|    Azure DK SOM kimlik doğrulaması ve yetkilendirme|
|*. auth.projectsantacruz.azure.net| 443|    Azure DK SOM kimlik doğrulaması ve yetkilendirme|

Ayrıca, [Azure IoT Edge tarafından kullanılan bağlantıların](https://docs.microsoft.com/azure/iot-edge/production-checklist?view=iotedge-2018-06#allow-connections-from-iot-edge-devices)listesini gözden geçirin.

## <a name="additional-recommendations-for-deployment-to-production"></a>Üretime dağıtım için ek öneriler

Azure Percept DK, kutudan çıkan çok çeşitli güvenlik özellikleri sunar. Microsoft, geçerli sürüme dahil edilen güçlü güvenlik özelliklerine ek olarak, üretim dağıtımlarını değerlendirirken aşağıdaki yönergeleri de önerir:

- Cihazın kendisinin güçlü fiziksel koruması
- Rest şifrelemede verilerin etkinleştirildiğinden emin olma
- Cihaz duruşunu sürekli olarak izleme ve uyarılara hızlı bir şekilde yanıt verme
- Cihaza erişimi olan yönetici sayısını sınırlandırma

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir [Azure PERCEPT AI modelleri](./overview-ai-models.md)hakkında bilgi edinin.