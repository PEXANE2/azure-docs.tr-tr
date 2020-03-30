---
title: HANA'yı Azure'da SAP HANA'ya yükleme (Büyük Örnekler) | Microsoft Dokümanlar
description: HANA'yı Azure'da SAP HANA'ya yükleme (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: hermannd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 023f32fce01ffbd974b182fa89fd604e62332936
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617202"
---
# <a name="install-hana-on-sap-hana-on-azure-large-instances"></a>HANA'yı Azure'da SAP HANA'ya yükleme (Büyük Örnekler)

HANA'yı Azure'da SAP HANA'ya (Büyük Örnekler) yüklemek için öncelikle aşağıdakileri yapmanız gerekir:
- Microsoft'a sap HANA Büyük Örneği'nde sizin için dağıtılaması gereken tüm verileri sağlarsınız.
- SAP HANA Büyük Örneği'ni Microsoft'tan alırsınız.
- Şirket içi ağınıza bağlı bir Azure sanal ağı oluşturursunuz.
- HANA Büyük Örnekleri için ExpressRoute devresini aynı Azure sanal ağına bağlarsınız.
- HANA Büyük Örnekleri için atlama kutusu olarak kullandığınız bir Azure sanal makinesini yüklersiniz.
- Atlama kutusundan HANA Büyük Örnek biriminize bağlanabileceğinizden emin olun ve bunun tersi de geçerlidir.
- Gerekli tüm paketlerin ve yamaların yüklü olup olmadığını kontrol elabilirsiniz.
- Kullanmakta olduğunuz işletim sisteminde HANA yüklemesi ile ilgili SAP notlarını ve belgelerini okudunuz. Hana'nın seçtiği sürümün işletim sistemi sürümünde desteklendikten emin olun.

Sonraki bölümde, HANA yükleme paketlerini atlama kutusu sanal makinesine indirme örneği gösterilmektedir. Bu durumda, işletim sistemi Windows'dur.

## <a name="download-the-sap-hana-installation-bits"></a>SAP HANA yükleme bitlerini indirin
HANA Büyük Örnek birimleri doğrudan internete bağlı değildir. Yükleme paketlerini SAP'den HANA Large Instance sanal makinesine doğrudan indiremezsiniz. Bunun yerine, atlama kutusu sanal makineye paketleri indirin.

SAP Marketi'ne erişmenizi sağlayan bir SAP S kullanıcısına veya başka bir kullanıcıya ihtiyacınız vardır.

1. Oturum açın ve [SAP Service Marketplace'e](https://support.sap.com/en/index.html)gidin. Yazılım > **Yüklemeleri İndir'i** > seçin ve **Download Software****Alfabetik Dizine Göre**Yükseltin. Sonra H altında seçin **- SAP HANA Platform Edition** > **SAP HANA Platform Edition 2.0** > **Kurulum**. Aşağıdaki ekran görüntüsünde gösterilen dosyaları indirin.

   ![İndirilen dosyaların ekran görüntüsü](./media/hana-installation/image16_download_hana.PNG)

2. Bu örnekte SAP HANA 2.0 kurulum paketlerini indirdik. Azure atlama kutusu sanal makinesinde, kendi kendini ayıklayan arşivleri aşağıda gösterildiği gibi dizine genişletin.

   ![Kendi kendini ayıklayan arşivin ekran görüntüsü](./media/hana-installation/image17_extract_hana.PNG)

3. Arşivler ayıklanırgibi, çıkarma tarafından oluşturulan dizini kopyalayın (bu durumda, 51052030). Dizini HANA Büyük Örnek biriminden /hana/paylaşılan birimden oluşturduğunuz bir dizine kopyalayın.

   > [!Important]
   > Alan sınırlı olduğundan ve diğer işlemler tarafından da kullanılması gerektiğinden, yükleme paketlerini kök veya önyükleme LUN'a kopyalamayın.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA'yı HANA Büyük Örnek birimine yükleyin
SAP HANA'yı yüklemek için kullanıcı kökü olarak oturum açın. Yalnızca root SAP HANA yüklemek için yeterli izinlere sahiptir. /hana/shared olarak kopyaladığınız dizinde izinleri ayarlayın.

```
chmod –R 744 <Installation bits folder>
```

Grafikkullanıcı arabirimi kurulumlarını kullanarak SAP HANA'yı yüklemek istiyorsanız, GTK2 paketinin HANA Large Instances'a yüklenmesi gerekir. Yüklü olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

```
rpm –qa | grep gtk2
```

(Daha sonraki adımlarda, sap HANA kurulumünü grafik kullanıcı arabirimi ile gösteririz.)

Yükleme dizinine gidin ve alt dizine gidin HDB_LCM_LINUX_X86_64. 

Bu dizinin dışında, başlayın:

```
./hdblcmgui 
```
Bu noktada, yükleme için verileri sağladığınız bir dizi ekran boyunca ilerleme sağlarsınız. Bu örnekte, SAP HANA veritabanı sunucusunu ve SAP HANA istemci bileşenlerini yüklüyoruz. Bu nedenle, bizim seçim **SAP HANA Veritabanı.**

![SAP HANA Veritabanı seçili SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü](./media/hana-installation/image18_hana_selection.PNG)

Sonraki ekranda Yeni **Sistemi Yükle'yi**seçin.

![Sap HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü, Install New System seçili](./media/hana-installation/image19_select_new.PNG)

Ardından, yükleyebileceğiniz birkaç ek bileşen arasından birini seçin.

![SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü, ek bileşenlerin listesi ile](./media/hana-installation/image20_select_components.PNG)

Burada SAP HANA Client ve SAP HANA Studio'yu seçiyoruz. Ayrıca bir ölçeklendirme örneği de yükleriz. Sonra **Tek Ana Bilgisayar Sistemi**seçin. 

![SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü, Tek Ana Bilgisayar Sistemi seçili](./media/hana-installation/image21_single_host.PNG)

Ardından, bazı veriler sağlayın.

![SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü, tanımlayacak sistem özellikleri alanları ile](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> HANA System ID (SID) olarak, HANA Büyük Örnek dağıtımını sipariş ettiğinizde Microsoft'a sağladığınız SID'yi sağlamanız gerekir. Farklı bir SID seçmek, farklı birimlerdeki izin sorunlarına erişmek nedeniyle yüklemenin başarısız olmasını neden olur.

Yükleme yolu için /hana/paylaşılan dizinini kullanın. Bir sonraki adımda, HANA veri dosyaları ve HANA günlük dosyaları için konumları sağlar.


![Veri ve günlük alanı alanları yla SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Sistem özelliklerini tanımladığınızda belirttiğiniz SID (iki ekran önce) montaj noktalarının SID'si ile eşleşmelidir. Bir uyumsuzluk varsa, geri gidin ve SID'yi montaj noktalarındaki değere göre ayarlayın.

Bir sonraki adımda, ana bilgisayar adını gözden geçirin ve sonunda düzeltin. 

![SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü, ev sahibi adı ile](./media/hana-installation/image24_review_host_name.PNG)

Bir sonraki adımda, HANA Büyük Örnek dağıtımını sipariş ettiğinizde Microsoft'a vermiş olduğunuz verileri de almanız gerekir. 

![SAP HANA Yaşam Döngüsü Yönetimi ekran görüntüsü, sistem yöneticisi alanları tanımlamak için](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Birim dağıtımını sipariş ederken, Microsoft'a sağladığınız aynı **Sistem Yöneticisi Kullanıcı Kimliği** ve Kullanıcı Grubu **Kimliğini** sağlayın. Aksi takdirde, SAP HANA'nın HANA Büyük Örnek birimine yüklenmesi başarısız olur.

Sonraki iki ekran burada gösterilmez. SAP HANA veritabanının SİsteM kullanıcısını ve sapadm kullanıcısının parolasını sağlamanızı sağlarlar. İkincisi, SAP HANA veritabanı örneğinin bir parçası olarak yüklenen SAP Ana Bilgisayar Aracısı için kullanılır.

Parolayı tanımladıktan sonra bir onay ekranı görürsünüz. listelenen tüm verileri kontrol edin ve yüklemeye devam edin. Yükleme ilerlemesini belgeleyen bir ilerleme ekranına ulaşırsınız, bu gibi:

![KURULUM ilerleme göstergeleriyle SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü](./media/hana-installation/image27_show_progress.PNG)

Yükleme sona ererken, bunun gibi bir ekran görmeniz gerekir:

![Kurulumun bittiğini belirten SAP HANA Yaşam Döngüsü Yönetimi ekranının ekran görüntüsü](./media/hana-installation/image28_install_finished.PNG)

SAP HANA örneği şimdi çalışır durumda ve kullanıma hazır olmalıdır. SAP HANA Studio'dan bağlanabilmelisin. Ayrıca, en son güncelleştirmeleri kontrol ettiğinizden ve uyguladığınıza emin olun.


## <a name="next-steps"></a>Sonraki adımlar

- [SAP HANA Azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma örnekleri](hana-overview-high-availability-disaster-recovery.md)

