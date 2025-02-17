---
title: Azure Database for PostgreSQL-단일 서버에 대한 가격 책정 계층
description: 이 문서에서는 PostgreSQL-단일 서버에 대한 Azure Database의 가격 책정 계층을 설명합니다.
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 42f290109ca380464cb07ac9f684cdde25b8fdcd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063760"
---
# <a name="pricing-tiers-in-azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL-단일 서버에서 가격 책정 계층

Azure Database for PostgreSQL 서버는 기본, 범용 및 메모리 최적화의 세 가지 가격 책정 계층 중 하나에서 만들 수 있습니다. 가격 책정 계층은 프로비전할 수 있는 계산의 vCore 수, vCore당 메모리 및 데이터를 저장하는 데 사용되는 스토리지 기술로 구분됩니다. 모든 리소스는 PostgreSQL 서버 수준에서 프로비전됩니다. 서버는 하나 이상의 데이터베이스를 갖출 수 있습니다.

|    | **Basic** | **범용** | **메모리 최적화** |
|:---|:----------|:--------------------|:---------------------|
| 컴퓨팅 세대 | 4세대, 5세대 | 4세대, 5세대 | 5세대 |
| vCore 수 | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| vCore 메모리 | 2GB | 5GB | 10 GB |
| 저장소 크기 | 5GB-1TB | 5GB-4TB | 5GB-4TB |
| 저장소 유형 | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| 데이터베이스 백업 보존 기간 | 7-35일 | 7-35일 | 7-35일 |

가격 책정 계층을 선택하려면 시작 지점으로 다음 표를 사용합니다.

| 가격 책정 계층 | 대상 워크로드 |
|:-------------|:-----------------|
| 기본 | 간단한 계산 및 I/O 성능이 필요한 워크로드. 예를 들어 개발 또는 시험, 또는 자주 사용하지 않는 소규모 애플리케이션에 사용되는 서버가 이에 해당합니다. |
| 범용 | 확장 가능한 I/O 처리량을 갖춘 부하 분산된 컴퓨팅 및 메모리가 필요한 대부분의 비즈니스 워크로드. 예를 들어 웹 및 모바일 앱을 호스트하는 서버와 기타 엔터프라이즈 애플리케이션이 있습니다.|
| 메모리 최적화 | 빠른 트랜잭션 처리와 높은 동시성을 위해 메모리 내 성능이 필요한 고성능 데이터베이스 워크로드. 예를 들어 실시간 데이터를 처리하는 서버 및 고성능 트랜잭션 또는 분석 앱이 있습니다.|

서버를 만든 후 vCore 수, 하드웨어 생성 및 가격 책정 계층(기본 제외)은 몇 초 이내로 늘리거나 줄일 수 있습니다. 또한 애플리케이션 중단 시간 없이 독립적으로 스토리지 용량을 늘리거나 백업 보존 기간을 늘리거나 줄일 수 있습니다. 서버가 만들어진 후 백업 저장소 유형은 변경할 수 없습니다. 자세한 내용은 [리소스 크기 조정](#scale-resources) 섹션을 참조하세요.

## <a name="compute-generations-and-vcores"></a>세대 및 vCore 수 계산

계산 리소스는 기본 하드웨어의 논리적 CPU를 나타내는 vCore 수로 제공됩니다. 중국 동부 1, 중국 북부 1, 미국 국방부 중부 및 미국 국방부 동부 Intel E5-2673 v3을 기반으로 하는 범용 4 세대 논리 Cpu를 활용 (Haswell) 2.4ghz 프로세서. Intel E5-2673 v4를 기반으로 하는 Gen 5 논리 Cpu를 활용 하는 다른 모든 하위 지역 (Broadwell) 2.3ghz 프로세서.

## <a name="storage"></a>Storage

프로비전하는 저장소는 Azure Database for PostgreSQL 서버에 사용할 수 있는 저장소 용량입니다. 저장소는 데이터베이스 파일, 임시 파일, 트랜잭션 로그 및 PostgreSQL 서버 로그에 사용됩니다. 프로비전하는 총 저장소 용량도 서버에 사용할 수 있는 I/O 용량을 정의합니다.

|    | **Basic** | **범용** | **메모리 최적화** |
|:---|:----------|:--------------------|:---------------------|
| 저장소 유형 | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| 저장소 크기 | 5GB-1TB | 5GB-4TB | 5GB-4TB |
| 저장소 증분 크기 | 1 GB | 1GB | 1 GB |
| IOPS | 변수 |3IOPS/GB<br/>최소 100IOPS<br/>최대 6000IOPS | 3IOPS/GB<br/>최소 100IOPS<br/>최대 6000IOPS |

중 및 서버를 만든 후 추가 저장소 용량을 추가 하 고 시스템 저장소 워크 로드의 저장소 사용량에 따라 자동으로 증가 하도록 허용 합니다. 기본 계층에서는 IOPS 보장을 제공하지 않습니다. 범용 및 메모리 최적화 가격 책정 계층에서 IOPS의 크기는 프로비전된 저장소 크기와 3:1 비율로 조정됩니다.

Azure Portal 또는 Azure CLI 명령을 사용하여 I/O 사용량을 모니터링할 수 있습니다. 모니터링할 관련 메트릭은 [저장소 제한, 저장소 비율, 저장소 사용됨 및 IO 백분율](concepts-monitoring.md)입니다.

### <a name="large-storage-preview"></a>대용량 저장소 (미리 보기)

우리는 우리의 범용 및 메모리 최적화 계층에서 저장소 한도 증가 합니다. 서버를 새로 만든 해당 옵트인 미리 보기에 프로 비전 할 수 최대 16TB의 저장소입니다. IOPS는 3:1 비율로 최대 20,000 개의 IOPS를 확장 합니다. 현재 일반 공급 저장소와 마찬가지로 서버를 만든 후 추가 저장소 용량을 추가할 수 있으며 시스템 워크 로드의 저장소 사용량에 따라 자동으로 저장소 성장을 수 있습니다.

|              | **범용** | **메모리 최적화** |
|:-------------|:--------------------|:---------------------|
| 저장소 유형 | Azure Premium Storage | Azure Premium Storage |
| 저장소 크기 | 16TB를 32GB| 16 32 TB |
| 저장소 증분 크기 | 1 GB | 1 GB |
| IOPS | 3IOPS/GB<br/>최소 100IOPS<br/>최대 20,000 IOPS| 3IOPS/GB<br/>최소 100IOPS<br/>최대 20,000 IOPS |

> [!IMPORTANT]
> 대용량 저장소는 현재 다음 지역에서 공개 미리 보기: 미국 동부, 미국 동부 2, 미국 중부, 미국 서 부, 미국 서 부 2, 북유럽, 유럽 서 부, 동남 아시아, 일본 동부, 한국 중부, 오스트레일리아 동부입니다.
>
>현재 큰 저장소 미리 보기 지원 하지 않습니다.
>
> * Virtual network 서비스 끝점을 통해 들어오는 연결
> * 지역 중복 백업
> * 교차 지역 복제

### <a name="reaching-the-storage-limit"></a>저장소 제한에 도달

사용 가능한 저장소 512MB 또는 프로 비전 된 저장소 크기의 5% 보다 작은 경우 읽기 전용 서버 프로 비전 하는 100 GB 저장소 보다 더 적은 노력으로 표시 됩니다. 사용 가능한 저장소는 5GB 보다 작은 경우에 서버를 프로 비전 하는 100 GB 저장소 보다 읽기 표시 됩니다.

예를 들어, 110 GB의 저장소를 프로 비전 및 실제 사용량을 초과 하는 경우 105GB 서버는 읽기 전용으로 표시 됩니다. 또는 5GB의 저장소를 프로 비전 하는 경우 서버는 읽기 전용 사용 가능한 저장소에 512MB 미만 도달 하면 합니다.

서버가 읽기 전용으로 설정되면 모든 기존 세션은 연결이 끊어지고 커밋되지 않은 트랜잭션이 롤백됩니다. 모든 후속 쓰기 작업 및 트랜잭션 커밋은 실패합니다. 모든 후속 읽기 쿼리는 계속 작동합니다.  

서버에 프로비전된 저장소의 크기를 늘리거나, 읽기/쓰기 모드에서 새 세션을 시작하고 여유 저장 공간을 회수하기 위해 데이터를 삭제할 수 있습니다. `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;`을 실행하면 읽기 쓰기 모드에 현재 세션을 설정할 수 있습니다. 데이터 손상을 방지하려면 서버가 아직 읽기 전용 상태에 있을 경우 어떤 쓰기 작업도 수행하지 마십시오.

저장소에서 사용 하는 것이 좋습니다 자동 증가 또는 서버 저장소 임계값에 도달할 때이 알리도록 경고 설정에 따라서 방지할 수 있습니다 읽기 전용 상태로 시작 합니다. 자세한 내용은 [경고 설정 방법](howto-alert-on-metric.md)에 관한 설명서를 참조하세요.

### <a name="storage-auto-grow"></a>저장소 자동 증가

저장소 자동으로 증가 하는 경우 사용 하도록 설정 저장소를 자동으로 증가 함에 따라 워크 로드에 영향을 주지 않고 합니다. 100GB 프로 비전 된 저장소 보다 더 적은 노력으로 서버에 대 한 사용 가능한 메모리를 1GB 또는 프로 비전된 된 저장소의 10% 중 더 큰 아래는 즉시 프로 비전 된 저장소 크기를 5GB 만큼 늘어납니다. 100GB 이상 프로 비전 된 저장소를 사용 하 여 서버에 대 한 사용 가능한 저장소 공간 프로 비전 된 저장소 크기의 5% 미만인 경우 프로 비전 된 저장소 크기가 5% 증가 합니다. 위에 지정 된 대로 최대 저장소 제한이 적용 됩니다.

예를 들어, 1000GB 저장소를 프로 비전 하 고 실제 사용량을 초과 하는 경우 1050 GB 950 GB, 서버 저장소 크기는 증가 합니다. 또는 10GB의 저장소를 프로 비전를 1GB 미만의 저장소를 사용할 수 없으면 저장소 크기는 15GB 증가 합니다.

## <a name="backup"></a>Backup

서비스에서 서버 백업을 자동으로 수행합니다. 최소 백업 보존 기간은 7일입니다. 보존 기간은 최대 35일까지 설정할 수 있습니다. 보존 기간은 서버 수명 기간 동안 언제든지 조정할 수 있습니다. 로컬 중복 백업과 지역 중복 백업 중에서 선택할 수 있습니다. 지역 중복 백업은 서버가 만들어진 지역의 [쌍으로 연결된 지역](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)에도 저장됩니다. 이 중복성은 재해 발생 시 일정 수준의 보호를 제공합니다. 또한 지역 중복 백업을 통해 서비스를 사용할 수 있는 다른 Azure 지역으로 서버를 복원할 수 있습니다. 서버가 만들어진 후 두 개의 백업 저장소 옵션 간에 변경할 수 없습니다.

## <a name="scale-resources"></a>리소스 크기 조정

서버를 만든 후 vCore 수, 하드웨어 생성, 가격 책정 계층(기본 제외), 저장소 크기 및 백업 보존 기간을 독립적으로 변경할 수 있습니다. 서버가 만들어진 후 백업 저장소 유형은 변경할 수 없습니다. vCore 수는 늘리거나 줄일 수 있습니다. 백업 보존 기간은 7~35일 범위에서 늘리거나 줄일 수 있습니다. 저장소 크기는 늘릴 수 있습니다. 리소스의 크기 조정은 포털 또는 Azure CLI를 통해 수행할 수 있습니다. Azure CLI를 사용하는 크기 조정의 예제는 [Azure CLI를 사용하여 Azure Database for PostgreSQL 서버 모니터링 및 크기 조정](scripts/sample-scale-server-up-or-down.md)을 참조하세요.

vCore 수, 하드웨어 생성 또는 가격 책정 계층을 변경하면 새 계산 할당을 사용하여 원본 서버의 복사본이 만들어집니다. 새 서버가 시작되고 실행된 후 새 서버에 대한 연결로 전환됩니다. 시스템이 새 서버로 전환되면 잠시 동안 새 연결을 설정할 수 없으며, 커밋되지 않은 모든 트랜잭션이 롤백됩니다. 이 기간은 다양하지만, 대부분의 경우 1분 미만입니다.

저장소 크기 조정 및 백업 보존 기간 변경은 온라인 작업입니다. 가동 중지 시간이 없으며 애플리케이션은 영향을 받지 않습니다. IOPS가 프로비전된 저장소 크기로 조정되면 저장소를 확장하여 서버에서 사용할 수 있는 IOPS를 늘릴 수 있습니다.

## <a name="pricing"></a>가격

최신 가격 책정 정보는 서비스 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/PostgreSQL/)를 참조하세요. 원하는 구성 비용을 확인하려면 [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer)에서 선택한 옵션에 따라 **가격 책정 계층** 탭에 월별 비용이 표시됩니다. Azure 구독이 없는 경우 Azure 가격 책정 계산기를 사용하여 예상 가격을 구할 수 있습니다. [Azure 가격 책정 계산기](https://azure.microsoft.com/pricing/calculator/) 웹 사이트에서 **항목 추가**를 선택하고, **데이터베이스** 범주를 확장하고, **Azure Database for PostgreSQL**을 선택하여 옵션을 사용자 지정합니다.

## <a name="next-steps"></a>다음 단계

- [포털에서 PostgreSQL 서버를 만드는](tutorial-design-database-using-azure-portal.md) 방법을 알아봅니다.
- [서비스 제한](concepts-limits.md)에 대해 알아봅니다. 
- [읽기 복제본으로 스케일 아웃](howto-read-replicas-portal.md)하는 방법을 알아봅니다.
