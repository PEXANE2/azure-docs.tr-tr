---
title: MABS (Azure Backup Sunucusu) v3 UR1 koruma matrisi
description: Bu makalede, Azure Backup Sunucusu koruduğu tüm iş yükleri, veri türleri ve yüklemelerin listelendiği bir destek matrisi sunulmaktadır.
ms.date: 03/19/2020
ms.topic: conceptual
ms.openlocfilehash: f950c7be28a4ff0dd0caa12d1327c2a0e2ac4fc5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84677330"
---
# <a name="mabs-azure-backup-server-v3-ur1-protection-matrix"></a>MABS (Azure Backup Sunucusu) v3 UR1 koruma matrisi

Bu makalede, Azure Backup Sunucusu ile koruyabileceğiniz çeşitli sunucular ve iş yükleri listelenmektedir. Aşağıdaki matris Azure Backup Sunucusu ile nelerin korunabilecek olduğunu listeler.

MABS v3 UR1 için aşağıdaki matrisi kullanın:

* İş yükleri: teknolojinin iş yükü türü.

* Sürüm – iş yükleri için desteklenen MABS sürümü.

* MABS yüklemesi – MABS 'yi yüklemek istediğiniz bilgisayar/konum.

* Koruma ve kurtarma – desteklenen depolama kapsayıcısı veya desteklenen dağıtım gibi iş yükleriyle ilgili ayrıntılı bilgileri listeleyin.

## <a name="protection-support-matrix"></a>Koruma destek matrisi

Aşağıdaki bölümlerde, MABS için koruma destek matrisi ayrıntıları verilmiştir:

* [Uygulamalar yedeklemesi](#applications-backup)
* [VM yedeklemesi](#vm-backup)
* [Linux](#linux)

## <a name="applications-backup"></a>Uygulamalar yedeklemesi

| **İş yükü**               | **Sürüm**                                                  | **Azure Backup Sunucusu yükleme**                       | **Desteklenen Azure Backup Sunucusu** | **Koruma ve kurtarma**                                  |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------------------------ |
| İstemci bilgisayarlar (64-bit) | Windows 10                                                  | Fiziksel sunucu  <br><br>    Hyper-V sanal makine   <br><br>   VMware sanal makinesi | V3 UR1                            | Birim, paylaşma, klasör, dosyalar, yinelenenleri kaldırılmış birimler   <br><br>   Korumalı birimler NTFS olmalıdır. FAT ve FAT32 desteklenmez.  <br><br>    Birimler en az 1 GB olmalıdır. Azure Backup Sunucusu, veri anlık görüntüsünü almak için Birim Gölge Kopyası Hizmeti (VSS) kullanır ve anlık görüntü yalnızca birim en az 1 GB olduğunda geçerlidir. |
| Sunucular (64-bit)          | Windows Server 2019, 2016, 2012 R2, 2012                    | Azure sanal makinesi (iş yükü Azure sanal makinesi olarak çalışırken)  <br><br>    Fiziksel sunucu  <br><br>    Hyper-V sanal makine <br><br>     VMware sanal makinesi  <br><br>    Azure Stack | V3 UR1                            | Birim, paylaşma, klasör, dosya, yinelenenleri kaldırılmış birimler (NTFS ve ReFS)  <br><br>   Sistem durumu ve tam (iş yükü Azure sanal makinesi olarak çalışırken desteklenmez) |
| Sunucular (64-bit)          | Windows Server 2008 R2 SP1, Windows Server 2008 SP2 ( [Windows Yönetim çerçevesi 4,0](https://www.microsoft.com/download/details.aspx?id=40855)' i yüklemeniz gerekir) | Fiziksel sunucu  <br><br>    Hyper-V sanal makine  <br><br>      VMware sanal makinesi  <br><br>   Azure Stack | V3 UR1                            | Birim, paylaşma, klasör, dosya, sistem durumu/tam kurtarma        |
| SQL Server                | SQL Server 2019, 2017, 2016 ve [desteklenen SPS](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016), 2014 ve desteklenen [SPS](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) 'ler | Fiziksel sunucu  <br><br>     Hyper-V sanal makine   <br><br>     VMware sanal makinesi  <br><br>   Azure sanal makinesi (iş yükü Azure sanal makinesi olarak çalışırken)  <br><br>     Azure Stack | V3 UR1                            | Tüm dağıtım senaryoları: veritabanı       <br><br>  MABS v3 UR1, ReFS birimleri üzerinden SQL veritabanlarının yedeklenmesini destekler                  |
| Exchange                   | Exchange 2019, 2016                                         | Fiziksel sunucu   <br><br>   Hyper-V sanal makine  <br><br>      VMware sanal makinesi  <br><br>   Azure Stack  <br><br>    Azure sanal makinesi (iş yükü Azure sanal makinesi olarak çalışırken) | V3 UR1                            | Koruma (tüm dağıtım senaryolarında): tek başına Exchange sunucusu, veritabanı kullanılabilirlik grubu (DAG) altındaki veritabanı  <br><br>    Kurtarma (tüm dağıtım senaryolarında): Posta kutusu, DAG altındaki posta kutusu veritabanları    <br><br>  MABS v3 UR1 ile ReFS üzerinden Exchange yedeklemesi desteklenir |
| SharePoint                 | SharePoint 2019, 2016 en son SPs                       | Fiziksel sunucu  <br><br>    Hyper-V sanal makine <br><br>    VMware sanal makinesi  <br><br>   Azure sanal makinesi (iş yükü Azure sanal makinesi olarak çalışırken)   <br><br>   Azure Stack | V3 UR1                            | Koruma (tüm dağıtım senaryolarında): Grup, ön uç Web sunucusu içeriği  <br><br>    Kurtarma (tüm dağıtım senaryolarında): Grup, veritabanı, Web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç Web sunucusu  <br><br>    İçerik veritabanları için SQL Server 2012 AlwaysOn özelliğini kullanan bir SharePoint grubunu koruma desteklenmez. |

## <a name="vm-backup"></a>VM yedeklemesi

| **İş yükü**                                                 | **Sürüm**                                             | **Azure Backup Sunucusu yükleme**                      | **Desteklenen Azure Backup Sunucusu** | **Koruma ve kurtarma**                                 |
| ------------------------------------------------------------ | ------------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------- | ------------------------------------------------------------ |
| Hyper-V konağı-MABS koruma Aracısı, Hyper-V konak sunucusu, küme veya VM üzerinde | Windows Server 2019, 2016, 2012 R2, 2012               | Fiziksel sunucu  <br><br>    Hyper-V sanal makine <br><br>    VMware sanal makinesi | V3 UR1                             | Koruma: Hyper-V bilgisayarları, küme paylaşılan birimleri (CSV)  <br><br>    Kurtarma: sanal makine, dosya ve klasörün öğe düzeyinde kurtarılması yalnızca Windows, birimler, sanal sabit sürücüler için kullanılabilir |
| VMware VM 'Leri                                                  | VMware Server 5,5, 6,0 veya 6,5, 6,7 (lisanslı sürüm) | Hyper-V sanal makinesi  <br><br>   VMware sanal makinesi         | V3 UR1                             | Koruma: Küme Paylaşılan birimlerinde (CSV), NFS ve SAN depolamada VMware VM 'Leri   <br><br>     Kurtarma: sanal makine, dosya ve klasörün öğe düzeyinde kurtarılması yalnızca Windows, birimler, sanal sabit sürücüler için kullanılabilir <br><br>    VMware vApps desteklenmez. |

## <a name="linux"></a>Linux

| **İş yükü** | **Sürüm**                               | **Azure Backup Sunucusu yükleme**                      | **Desteklenen Azure Backup Sunucusu** | **Koruma ve kurtarma**                                 |
| ------------ | ----------------------------------------- | ------------------------------------------------------------ | ---------------------------------- | ------------------------------------------------------------ |
| Linux        | Hyper-V veya VMware konuğu olarak çalışan Linux | Fiziksel sunucu, şirket içi Hyper-V VM, VMWare 'de Windows VM | V3 UR1                             | Hyper-V Windows Server 2012 R2 veya Windows Server 2016 üzerinde çalışıyor olmalıdır. Koruma: tüm sanal makine   <br><br>   Kurtarma: Tüm sanal makine   <br><br>    Yalnızca dosya tutarlı anlık görüntüler desteklenir.    <br><br>   Desteklenen Linux dağıtımları ve sürümlerinin tüm listesi için bkz. [Azure tarafından onaylanan dağıtımların Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)makalesi. |

## <a name="azure-expressroute-support"></a>Azure ExpressRoute desteği

Azure ExpressRoute üzerinden verilerinizi, genel eşleme (eski devreler için kullanılabilir) ve Microsoft eşlemesi ile birlikte yedekleyebilirsiniz. Özel eşleme üzerinde yedekleme desteklenmez.

Ortak eşleme ile: aşağıdaki etki alanlarına/adreslere erişim sağlayın:

* `http://www.msftncsi.com/ncsi.txt`
* `microsoft.com`
* `.WindowsAzure.com`
* `.microsoftonline.com`
* `.windows.net`

Microsoft eşlemesi ile aşağıdaki hizmetleri/bölgeleri ve ilgili topluluk değerlerini seçin:

* Azure Active Directory (12076:5060)
* Microsoft Azure bölgesi (Kurtarma Hizmetleri kasanızın konumuna göre)
* Azure depolama (Kurtarma Hizmetleri kasanızın konumuna göre)

Daha fazla bilgi için bkz. [ExpressRoute yönlendirme gereksinimleri](https://docs.microsoft.com/azure/expressroute/expressroute-routing).

>[!NOTE]
>Ortak eşleme, yeni devreler için kullanım dışıdır.

## <a name="cluster-support"></a>Küme desteği

Azure Backup Sunucusu, aşağıdaki kümelenmiş uygulamalardaki verileri koruyabilir:

* Dosya sunucuları

* SQL Server

* Hyper-V-ölçekli MABS koruma Aracısı kullanarak bir Hyper-V kümesini koruyorduysanız, korunan Hyper-V iş yükleri için ikincil koruma ekleyemezsiniz.

* Exchange Server-Azure Backup Sunucusu, desteklenen Exchange Server sürümleri (küme sürekli çoğaltma) için paylaşılmayan disk kümelerini koruyabilir ve ayrıca yerel sürekli çoğaltma için yapılandırılmış Exchange Server 'ı da koruyabilir.

* SQL Server-Azure Backup Sunucusu, Küme Paylaşılan birimlerinde (CSV) barındırılan SQL Server veritabanlarının yedeklenmesini desteklemez.

Azure Backup Sunucusu, MABS sunucusuyla aynı etki alanında ve alt veya güvenilen bir etki alanında bulunan küme iş yüklerini koruyabilir. Güvenilmeyen etki alanları veya çalışma gruplarındaki veri kaynaklarını korumak istiyorsanız, tek bir sunucu için NTLM veya sertifika kimlik doğrulaması ya da yalnızca bir küme için sertifika kimlik doğrulaması kullanın.
