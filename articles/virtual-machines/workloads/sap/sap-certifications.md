---
title: SAP için Microsoft Azure sertifikaları | Microsoft Dokümanlar
description: Azure platformunda SAP'nin güncel yapılandırmaları ve sertifikalarının güncelleştirilmiş listesi.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/21/2020
ms.author: juergent
ms.custom: ''
ms.openlocfilehash: c19471fee9235faffba12a12d9f92de77f60fd4d
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770512"
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>Microsoft Azure'da çalışan SAP sertifikaları ve yapılandırmaları

SAP ve Microsoft müşterileri için karşılıklı yararları olan güçlü bir ortaklık birlikte çalışma uzun bir geçmişi var. Microsoft, Microsoft Azure'un SAP iş yüklerinizi çalıştıracak en iyi platform olduğundan emin olmak için platformunu sürekli olarak güncelliyor ve yeni sertifika bilgilerini SAP'ye göndermektedir. Aşağıdaki tablolarda Azure destekli yapılandırmalar ve büyüyen SAP sertifikalarının listesi sıralanmaktadır. Bu liste, resmi SAP listelerinden burada ve orada saptanabilecek genel bir listedir. Ayrıntılı verilere nasıl ulaşılır makalede belgelenir Hangi [SAP yazılımı Azure dağıtımları için desteklenir](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)

## <a name="sap-hana-certifications"></a>SAP HANA sertifikaları
Başvuru:

- SAP HANA, yerel Azure VM'ler ve HANA Büyük Örnekleri için SAP HANA desteği için [IaaS platformlarını sertifikalandırmaktadır.](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

| SAP Ürün | Desteklenen İşletim Sistemi | Azure Teklifleri |
| --- | --- | --- |
| SAP HANA Developer Edition (yalnızca SQLODBC, ODBO-Windows, ODBC, JDBC sürücüleri, HANA stüdyosu ve HANA veritabanından oluşan HANA istemci yazılımı dahil) | Red Hat Enterprise Linux, SUSE Linux Enterprise | D Serisi VM ailesi |
| HANA'da Business One | SUSE Linux Kurumsal | DS14_v2, M32ts, M32ls, M64ls, M64s <br /> [SAP HANA Sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One) |
| SAP S/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 için Kontrollü Kullanılabilirlik. Full support for M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, M416s_v2, M416ms_v2, <br /> SAP HANA on Azure (Büyük örnekler) [SAP HANA Sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| HANA, OLTP üzerinde Süit | Red Hat Enterprise Linux, SUSE Linux Enterprise | M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> M416s_v2, M416ms_v2, SAP HANA On Azure (Büyük örnekler) [SAP HANA Sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| BW, OLAP için HANA Enterprise | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> M416s_v2, M416ms_v2, SAP HANA On Azure (Büyük örnekler) [SAP HANA Sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5, M64s, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, <br /> azure'da M416s_v2, M416ms_v2, SAP HANA (Büyük örnekler) <br /> [SAP HANA Sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) |

SAP'nin [SAP HANA Sertifikalı IaaS Platformlarında](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) 'kümeleme' terimini 'ölçeklendirme' ile eş anlamlı olarak kullandığını ve yüksek kullanılabilirlik 'kümeleme' için NOT'u kullandığını unutmayın

## <a name="sap-netweaver-certifications"></a>SAP NetWeaver sertifikaları
Microsoft Azure, Microsoft ve SAP'nin tam desteğiyle aşağıdaki SAP ürünleri için sertifikalıdır.
Başvuru:

- [1928533 - Azure'da SAP Uygulamaları:](https://launchpad.support.sap.com/#/notes/1928533) SAP TREX, SAP LiveCache ve SAP Content Server dahil olmak üzere tüm SAP NetWeaver tabanlı uygulamalar için Desteklenen Ürünler ve Azure VM türleri. SAP HANA hariç tüm veritabanları.


| SAP Ürün | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business Suite Yazılımı | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (yalnızca Windows ve Oracle Linux), DB2, SAP ASE |A5-A11, D11 d14, DS11 d14, DS15_v2, GS1'den GS5'e DS11_v2, D64s_v3'a, D64s_v3'a D2s_v3, D64as_v4'a D2as_v4, E64s_v3'a E2s_v3, E64as_v4'a E2as_v4, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, M416ms_v2, M416ms_v2, M416s_v2 |
| SAP İş Hepsi Bir | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (yalnızca Windows ve Oracle Linux), DB2, SAP ASE |A5-A11, D11 d14, DS11 d14, DS15_v2, GS1'den GS5'e DS11_v2, D64s_v3'a, D64s_v3'a D2s_v3, D64as_v4'a D2as_v4, E64s_v3'a E2s_v3, E64as_v4'a E2as_v4, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, M416ms_v2, M416ms_v2, M416s_v2 |
| SAP BusinessObjects BI | Windows |Yok |A5-A11, D11 d14, DS11 d14, DS15_v2, GS1'den GS5'e DS11_v2, D64s_v3'a, D64s_v3'a D2s_v3, D64as_v4'a D2as_v4, E64s_v3'a E2s_v3, E64as_v4'a E2as_v4, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, M416ms_v2, M416ms_v2, M416s_v2 |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (yalnızca Windows ve Oracle Linux), DB2, SAP ASE |A5-A11, D11 d14, DS11 d14, DS15_v2, GS1'den GS5'e DS11_v2, D64s_v3'a, D64s_v3'a D2s_v3, D64as_v4'a D2as_v4, E64s_v3'a E2s_v3, E64as_v4'a E2as_v4, M64ms, M128s, M128ms, M64ls, M32ls, M32ts, M208s_v2, M208ms_v2, M416ms_v2, M416ms_v2, M416s_v2 |

## <a name="other-sap-workload-supported-on-azure"></a>Azure'da desteklenen diğer SAP İş Yükü

| SAP Ürün | Konuk işletim sistemi | RDBMS | Sanal Makine Türleri |
| --- | --- | --- | --- |
| SAP Business One SQL Server'da | Windows  | SQL Server | Tüm NetWeaver sertifikalı VM türleri<br /> [SAP Not #928839](https://launchpad.support.sap.com/#/notes/928839) |
| SAP BPC 10.01 MS SP08 | Windows ve Linux | | Tüm NetWeaver Sertifikalı VM türleri<br /> SAP Not #2451795 |
| SAP İş Nesneleri BI platformu | Windows ve Linux | | SAP Not #2145537 |
| SAP Veri Hizmetleri 4.2 | | | SAP Not #2288344 |
| SAP Hybris Ticaret Platformu  | Windows | SQL Server, Oracle | Tüm NetWeaver sertifikalı VM türleri <br /> [Hybris Wiki](https://cxwiki.sap.com/display/cloudss/Using+the+hybris+Platform+with+the+Cloud) |
| SAP Hybris Ticaret Platformu  | SLES 12 veya daha yeni | SAP HANA | Tüm NetWeaver sertifikalı VM türleri <br /> [Hybris Wiki](https://cxwiki.sap.com/display/cloudss/Using+the+hybris+Platform+with+the+Cloud) |
| SAP Hybris Ticaret Platformu  | RHEL 7 veya daha yeni | SAP HANA | Tüm NetWeaver sertifikalı VM türleri <br /> [Hybris Wiki](https://cxwiki.sap.com/display/cloudss/Using+the+hybris+Platform+with+the+Cloud) |
| SAP (Hybris) Ticaret Platformu 1811 ve sonrası  | Windows, SLES veya RHEL | SQL Azure DB | Tüm NetWeaver sertifikalı VM türleri <br /> [Hybris Dokümantasyon](https://help.sap.com/viewer/a74589c3a81a4a95bf51d87258c0ab15/1811/en-US/8c71300f866910149b40c88dfc0de431.html) |
