---
title: Azure Stack Edge Pro cihaz hatası için hazırlanma
description: Azure Stack Edge Pro başarısız bir cihaz için nasıl değişiklik alınacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 12/11/2020
ms.author: alkohli
ms.openlocfilehash: b437ce7b6894ebefe38b32f27d370d9f8c4bfe80
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97369030"
---
# <a name="prepare-for-an-azure-stack-edge-pro-gpu-device-failure"></a>Azure Stack Edge Pro GPU cihaz hatası için hazırlanma

Bu makale, Azure Stack Edge Pro GPU cihazınıza cihaz yapılandırma ve verilerini kaydetme ve yedeklemenin nasıl yapılacağını ayrıntılandıran bir cihaz hatasına hazırlanmanıza yardımcı olur. 

Makale, Azure Stack Edge Pro GPU cihazınıza dağıtılan Kubernetes ve IoT kapsayıcılarını yedeklemek için gereken adımları içermez. 

## <a name="understand-device-failures"></a>Cihaz başarısızlıklarını anlama

Azure Stack Edge Pro GPU cihazınız, iki tür donanım arızasına karşılaşabilir.

- Bir donanım bileşenini değiştirmenizi gerektiren toleranable sorunları. Bu arızalar cihazı düşürülmüş bir durumda çalıştırmanıza olanak sağlar. Bu hatalara örnek olarak, tek bir başarısız güç kaynağı birimi (PSU) veya cihazdaki tek bir hatalı disk verilebilir. Bu durumların her birinde cihaz çalışmaya devam edebilir. Başarısız olan bileşenleri değiştirmek için en kısa sürede Microsoft Desteği başvurun.

- Cihazınızda iki disk başarısız olduğunda, cihazın tamamını değiştirmenizi gerektiren, toleransız olmayan hata (örneğin,). Bu durumlarda Microsoft Desteği hemen iletişim kurun. Bir cihaz değişikliği gerekip gerekmediğini belirledikten sonra, destek Azure Stack Edge cihazınızı değiştirme konusunda yardımcı olur.

Toleransız hatalara karşı hazırlanmak için cihazınızda aşağıdaki şeyleri yedeklemeniz gerekir:

- Cihaz yapılandırması hakkında bilgi
- Edge Yerel paylaşımlarında ve Edge bulut paylaşımlarında bulunan veriler
- Cihazınızda çalışan VM 'lerle ilişkili dosya ve klasörler


## <a name="back-up-device-configuration"></a>Cihaz yapılandırmasını yedekleme

Cihazın ilk yapılandırması sırasında, cihaz yapılandırma bilgilerinin bir kopyasının [dağıtım denetim listesinde](azure-stack-edge-gpu-deploy-checklist.md)özetlenen şekilde tutulması önemlidir. Kurtarma sırasında bu yapılandırma bilgileri yeni değiştirme cihazına uygulamak için kullanılacaktır. 

## <a name="protect-device-data"></a>Cihaz verilerini koruma

Cihaz verileri aşağıdaki türlerden biri olabilir:

- Edge bulut paylaşımlarında veri
- Yerel paylaşımlardaki veriler
- VM 'lerdeki dosyalar ve klasörler

Aşağıdaki bölümlerde, cihazınızdaki bu tür verilerin her birini korumaya yönelik adımlar ve öneriler ele alınmaktadır.

## <a name="protect-data-in-edge-cloud-shares"></a>Uç bulut paylaşımlarındaki verileri koruma

Cihazınızdaki verileri Azure 'a katmanı eden Edge bulut paylaşımları oluşturabilirsiniz. Kullanılabilir ağ bant genişliğine bağlı olarak, toleransız bir hata oluşursa herhangi bir veri kaybını en aza indirmek için cihazınızdaki bant genişliği şablonlarını yapılandırın.

> [!IMPORTANT]
> Cihazda toleransız bir arıza varsa, cihazdan Azure 'a katmanlanmayan yerel veriler kaybolabilir. 

## <a name="protect-data-in-edge-local-shares"></a>Edge Yerel paylaşımlarında verileri koruma

Kubernetes veya IoT Edge dağıtıyorsanız, uygulama verilerini cihaza yerel olarak kaydetmek ve Azure depolama ile eşitleme yapmak için yapılandırın. Veriler cihazdaki bir paylaşımda depolanır. Bu paylaşımlardaki verileri yedeklemenin önemli olduğunu fark edebilirsiniz.

Aşağıdaki üçüncü taraf veri koruma çözümleri, yerel SMB veya NFS paylaşımlarında bulunan veriler için bir yedekleme çözümü sağlayabilir. 

| Üçüncü taraf yazılım           | Çözüme başvuru                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Ayrıntılar için Cohesity başvurun.          |
| Commvault                      | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br> Ayrıntılar için, Commkasaya başvurun.          |
| Veritas                        | [http://veritas.com/azure](http://veritas.com/azure) <br> Ayrıntılar için VERITAS ile iletişim kurun.   |
| Veeam                          | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Ayrıntılar için, Veead ile iletişim kurun. |


## <a name="protect-files-and-folders-on-vms"></a>VM 'lerde dosya ve klasörleri koruma

Azure Stack Edge, cihaza dağıtılan VM 'lerde bulunan verileri korumak için bir yedekleme çözümü sağlamak üzere Azure Backup ve diğer üçüncü taraf veri koruma çözümleriyle birlikte çalışarak. Aşağıdaki tabloda, aralarından seçim yapabileceğiniz kullanılabilir çözümlere yönelik başvurular listelenmektedir.


| Yedekleme çözümleri        | Desteklenen işletim sistemi   | Başvuru                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| Azure Backup için Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı | Windows        | [MARS aracısı hakkında](../backup/backup-azure-about-mars.md)    |
| Cohesity                | Windows, Linux | [Microsoft Azure tümleştirme, yedekleme & kurtarma çözümü kısa](https://www.cohesity.com/solution/cloud/azure) <br>Ayrıntılar için Cohesity başvurun.                          |
| Commvault               | Windows, Linux | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br>Ayrıntılar için, Commkasaya başvurun.                          |
| Veritas                 | Windows, Linux | [https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-Edge-with-NetBackup/ba-p/883370](https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-Edge-with-NetBackup/ba-p/883370) <br> Ayrıntılar için VERITAS ile iletişim kurun.                    |
| Veeam                   | Windows, Linux | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Ayrıntılar için, Veead ile iletişim kurun. |


## <a name="next-steps"></a>Sonraki adımlar

- [Başarısız Azure Stack Edge Pro GPU cihazından nasıl kurtarılacağı](azure-stack-edge-gpu-recover-device-failure.md)hakkında bilgi edinin.