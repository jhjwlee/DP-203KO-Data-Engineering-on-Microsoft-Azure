# DP-203T00: Azure의 데이터 엔지니어링

과정 DP-203: Azure의 데이터 엔지니어링을 시작합니다. 이 과정을 지원하려면 과정 콘텐츠를 업데이트하여 해당 과정에 사용된 Azure 서비스를 최신 상태로 유지해야 합니다.  Microsoft는 Azure 플랫폼의 변경 내용을 반영하여 랩 콘텐츠를 최신 상태로 유지하기 위해 과정 작성자들과 MCT가 GitHub에 자유롭게 콘텐츠를 게시할 수 있도록 GitHub에 랩 지침과 랩 파일을 게시합니다.

## 랩 개요

다음은 각 모듈의 랩 목표를 요약한 것입니다.

### 1일차

#### [모듈 00: 랩 환경 설정](Instructions/Labs/LAB_00_lab_setup_instructions.md)

이 과정용 랩 환경 설정을 완료합니다.

#### [모듈 01: 데이터 엔지니어링 워크로드용 컴퓨팅 및 스토리지 옵션 살펴보기](Instructions/Labs/LAB_01_compute_and_storage_options.md)

이 랩에서는 데이터 레이크의 구조를 지정하는 방법과 탐색, 스트리밍 및 일괄 처리 워크로드용 파일을 최적화하는 방법을 살펴봅니다. 구체적으로는 일괄 처리 및 스트림 처리를 통해 파일을 변환하는 과정에서 데이터 레이크를 데이터 구체화 수준으로 구성하는 방법을 알아봅니다. 그리고 Azure Synapse Analytics에서 Apache Spark를 사용하는 방법도 살펴봅니다.  또한 CSV, JSON, Parquet 파일 등의 데이터 세트에서 인덱스를 만든 다음, Hyperspace 및 MSSParkUtils를 비롯한 Spark 라이브러리를 사용하는 쿼리 및 워크로드 가속화에 해당 인덱스를 사용하는 방법도 알아봅니다.

#### [모듈 02: Azure Synapse Analytics 서버리스 SQL 풀을 사용하여 대화형 쿼리 실행](Instructions/Labs/LAB_02_queries_using_serverless_sql_pools.md)

이 랩에서는 데이터 레이크 및 외부 파일 원본에 저장된 파일을 사용하는 방법을 알아봅니다. 이 과정에서 Azure Synapse Analytics의 서버리스 SQL 풀이 실행하는 T-SQL 문을 사용합니다. 그리고 데이터 레이크에 저장된 Parquet 파일 및 외부 데이터 저장소에 저장된 CSV 파일을 쿼리합니다. 그런 후에는 Azure Active Directory 보안 그룹을 만들고, RBAC(역할 기반 액세스 제어) 및 ACL(액세스 제어 목록)을 통해 데이터 레이크의 파일에 대한 액세스 권한을 적용합니다.

#### [모듈 03: Azure Databricks에서 데이터 탐색 및 변환](Instructions/Labs/LAB_03_data_transformation_in_databricks.md)

이 랩에서는 다양한 Apache Spark DataFrame 메서드를 사용하여 Azure Databricks에서 데이터를 탐색 및 변환하는 방법을 알아봅니다. 구체적으로는 표준 DataFrame 메서드를 수행하여 데이터를 탐색 및 변환하는 방법을 알아봅니다. 그리고 중복 데이터 제거, 날짜/시간 값 조작, 열 이름 바꾸기, 데이터 집계 등의 고급 작업 수행 방법도 배웁니다. 그런 후에는 선택한 수집 기술을 프로비전한 다음 Stream Analytics와 통합하여 스트리밍 데이터를 사용하는 솔루션을 만듭니다.

### 2일차

#### [모듈 04: Apache Spark를 사용하여 데이터를 탐색 및 변환한 후 데이터 웨어하우스에 로드](Instructions/Labs/LAB_04_data_warehouse_using_apache_spark.md)

이 랩에서는 데이터 레이크에 저장된 데이터를 탐색 및 변환한 다음 관계형 데이터 저장소에 로드하는 방법을 알아봅니다. 구체적으로는 Parquet 및 JSON 파일을 살펴보고, JSON 파일을 쿼리한 다음 계층 구조를 적용하여 변환하는 기술을 사용해 봅니다. 그런 후에는 Apache Spark를 사용하여 데이터 웨어하우스에 데이터를 로드하고, 데이터 레이크의 Parquet 데이터를 전용 SQL 풀의 데이터와 조인합니다.

#### [모듈 05: 데이터 웨어하우스에 데이터 수집 및 로드](Instructions/Labs/LAB_05_load_data_into_the_data_warehouse.md)

이 랩에서는 T-SQL 스크립트와 Synapse Analytics 통합 파이프라인을 통해 데이터 웨어하우스에 데이터를 수집하는 방법을 알아봅니다. 그리고 T-SQL의 COPY 명령과 PolyBase를 사용하여 Synapse 전용 SQL 풀에 데이터를 로드하는 방법을 알아봅니다. 또한 Azure Synapse 파이프라인의 복사 작업과 워크로드 관리 기능을 사용해 페타바이트 단위 데이터 수집을 수행하는 방법도 알아봅니다.

#### [모듈 06: Azure Data Factory 또는 Azure Synapse 파이프라인을 사용하여 데이터 변환](Instructions/Labs/LAB_06_transform_data_with_pipelines.md)

이 랩에서는 여러 데이터 원본에서 데이터를 수집하기 위한 데이터 통합 파이프라인을 작성하고, 매핑 데이터 흐름 및 Notebooks를 사용하여 데이터를 변환하고, 데이터 싱크 하나 이상으로의 데이터 이동을 수행하는 방법을 배웁니다.

### 3일차

#### [모듈 07: Azure Data Factory 또는 Azure Synapse 파이프라인을 사용하여 Notebooks의 데이터 통합](Instructions/Labs/LAB_07_integrate_data_from_notebooks.md)

이 랩에서는 Notebook을 만들어 지난 12개월 동안의 사용자 활동과 구매를 쿼리합니다. 그런 다음 새 Notebook 활동을 사용하여 파이프라인에 Notebook을 추가하고, 오케스트레이션 프로세스의 일환으로 매핑 데이터 흐름 이후에 해당 Notebook을 실행합니다. 또한 Notebook 구성 과정에서 제어 흐름에 동적 콘텐츠를 추가하는 매개 변수를 구현하고, 매개 변수 사용 가능 방법의 유효성을 검사합니다.

#### [모듈 08: Azure Synapse Analytics를 통한 엔드투엔드 보안](Instructions/Labs/LAB_08_security_with_synapse_analytics.md)

이 랩에서는 Synapse Analytics 작업 영역 및 지원 인프라를 보호하는 방법을 알아봅니다. 구체적으로는 SQL Active Directory 관리자를 확인하고, IP 방화벽 규칙을 관리하고, Azure Key Vault를 사용해 비밀을 관리하고, Key Vault 연결된 서비스 및 파이프라인 활동을 통해 이러한 비밀에 액세스합니다. 그런 후에는 전용 SQL 풀 사용 시 열 수준 보안, 행 수준 보안 및 동적 데이터 마스킹을 구현하는 방법을 파악합니다.

#### [모듈 09: Azure Synapse Link를 사용한 HTAP(하이브리드 트랜잭션 분석 처리) 지원](Instructions/Labs/LAB_09_htap_with_azure_synapse_link.md)

이 랩에서는 Azure Synapse Link를 사용해 Synapse 작업 영역에 Azure Cosmos DB 계정을 원활하게 연결하는 방법을 알아봅니다. 구체적으로는 Synapse 링크를 사용하도록 설정하고 구성한 다음, Apache Spark 및 SQL Serverless를 사용하여 Azure Cosmos DB 분석 저장소를 쿼리하는 방법을 파악합니다.
### 4일차
#### [모듈 10: Stream Analytics를 사용하여 실시간 스트림 처리](Instructions/Labs/LAB_10_stream_analytics.md)

이 랩에서는 Azure Stream Analytics를 사용하여 스트리밍 데이터를 처리하는 방법을 알아봅니다. 구체적으로는 Event Hubs에 차량 원격 분석 데이터를 수집한 다음 Azure Stream Analytics의 여러 창 기능을 사용하여 실시간으로 해당 데이터를 처리합니다. 그 후에 Azure Synapse Analytics로 데이터를 출력합니다. 그리고 마지막으로는 처리량을 높이기 위해 Stream Analytics 작업 크기를 조정하는 방법을 알아봅니다.

#### [모듈 11: Event Hubs 및 Azure Databricks를 사용하여 스트림 처리 솔루션 만들기](Instructions/Labs/LAB_11_stream_with_azure_databricks.md)

이 랩에서는 Azure Databricks의 Spark 구조적 스트리밍 및 Event Hubs를 사용하여 스트리밍 데이터를 대규모로 수집 및 처리하는 방법을 알아봅니다. 그리고 구조적 스트리밍의 주요 기능 및 사용 방식을 알아봅니다. 또한 슬라이딩 윈도우를 사용해 데이터 청크를 집계하고, 워터마크를 적용해 부실 데이터를 제거합니다. 그리고 마지막으로 Event Hubs에 연결해 스트림 읽기와 쓰기를 수행합니다.

- **MCT인가요?** - [MCT용 GitHub 사용자 가이드](https://microsoftlearning.github.io/MCT-User-Guide/)를 살펴보세요.
                                                                       
## 릴리스된 MOC 파일과 병행하여 이러한 파일을 사용하는 방법

- 강사 핸드북과 PowerPoint는 여전히 과정 콘텐츠를 가르치는 기본적인 자료로 사용될 것입니다.

- GitHub의 이러한 파일은 수강생 핸드북과 함께 사용할 수 있도록 설계되었지만 중앙 리포지토리 역할을 하는 GitHub에 위치합니다. 따라서 MCT와 과정 작성자가 최신 랩 파일에 대한 소스를 공유할 수 있습니다.

- 각 모듈의 랩 지침은 /Instructions/Labs 폴더에 있습니다. 이 위치의 각 하위 폴더가 개별 모듈에 해당됩니다. 예를 들어 Lab01은 모듈 01에서 진행하는 랩입니다. 그리고 각 폴더에는 수강생들이 랩을 진행할 때 따라야 하는 랩 지침이 포함된 README.md 파일이 있습니다.

- 트레이너는 강의를 할 때마다 GitHub에서 최신 Azure 서비스를 지원하기 위해 변경된 내용을 확인하고 최신 파일을 가져와서 강의에 사용하는 것이 좋습니다.

- 이 랩 지침에 표시되는 이미지 중 일부에는 이 과정에서 사용할 랩 환경의 상태가 정확하게 반영되어 있지 않을 수도 있습니다. 예를 들어 데이터 레이크에서 파일을 찾아볼 때 실제 환경에는 없을 수도 있는 추가 폴더가 이미지에는 표시되어 있을 수도 있습니다. 하지만 이로 인한 문제는 발생하지 않으며 랩 지침은 정상 작동합니다.

## 수강생 핸드북 변경 방식

- 수강생 핸드북은 분기별로 검토되며 필요에 따라 일반 MOC 릴리스 채널을 통해 업데이트됩니다.

## 콘텐츠 제공 방법

- 모든 MCT는 GitHub 리포지토리의 코드 또는 콘텐츠에 대한 문제를 제출할 수 있습니다. Microsoft와 과정 작성자는 콘텐츠 및 랩 코드 변경을 선별하고 필요에 따라 포함합니다.

## 강의 자료

MCT와 파트너는 이러한 자료에 액세스하고 수강생에게 개별적으로 제공하는 것이 좋습니다.  수업 진행 중에 수강생에게 GitHub 랩 단계를 직접 액세스하도록 하면 과정의 일부로 다른 UI에 액세스해야 하므로 수강생이 혼란을 겪을 수 있습니다. 수강생에게 별도의 랩 지침을 사용해야 하는 이유를 설명하면 계속 변경되는 클라우드 기반 인터페이스 및 플랫폼의 특성을 강조하는 데 도움이 됩니다. GitHub 파일 액세스와 GitHub 사이트 탐색에 대한 Microsoft Learning 지원은 이 과정을 가르치는 MCT에게만 제공됩니다.

## Microsoft의 역할

- 이 과정을 지원하려면 과정 콘텐츠를 자주 업데이트하여 해당 과정에 사용된 Azure 서비스를 최신 상태로 유지해야 합니다.  Microsoft는 Azure 플랫폼의 변경 내용을 반영하여 랩 콘텐츠를 최신 상태로 유지하기 위해 과정 작성자들과 MCT가 GitHub에 자유롭게 콘텐츠를 게시할 수 있도록 GitHub에 랩 지침과 랩 파일을 게시합니다.

- 여러분도 이와 같은 새로운 공동 작업 방식 랩 개선 과정에 참여하실 수 있습니다. 실제 강의를 진행하는 과정에서 Azure의 변경 내용을 처음으로 확인하시는 분은 랩 원본을 바로 개선해 주시기 바랍니다.  그러면 다른 MCT가 랩을 더욱 효율적으로 진행할 수 있습니다.

## 릴리스된 MOC 파일과 병행하여 이러한 파일을 사용하는 방법

- 강사 핸드북과 PowerPoint는 여전히 과정 콘텐츠를 가르치는 기본적인 자료로 사용될 것입니다.

- GitHub의 이러한 파일은 수강생 핸드북과 함께 사용할 수 있도록 설계되었지만 중앙 리포지토리 역할을 하는 GitHub에 위치합니다. 따라서 MCT와 과정 작성자가 최신 랩 파일에 대한 소스를 공유할 수 있습니다.

- 트레이너는 강의를 할 때마다 GitHub에서 최신 Azure 서비스를 지원하기 위해 변경된 내용을 확인하고 최신 파일을 가져와서 강의에 사용하는 것이 좋습니다.

## 수강생 핸드북 변경 방식

- 수강생 핸드북은 분기별로 검토되며 필요에 따라 일반 MOC 릴리스 채널을 통해 업데이트됩니다.

## 콘텐츠 제공 방법

- 모든 MCT는 GitHub 리포지토리의 코드 또는 콘텐츠에 대한 끌어오기 요청을 제출할 수 있습니다. Microsoft와 과정 작성자는 콘텐츠 및 랩 코드 변경을 선별하고 필요에 따라 포함합니다.

- MCT는 버그, 변경 사항, 개선 사항 및 아이디어를 제출할 수 있습니다.  Microsoft보다 먼저 새로운 Azure 기능을 찾았다면  새로운 데모를 제출해 주세요!

## 참고 사항

### 강의 자료

MCT와 파트너는 이러한 자료에 액세스하고 수강생에게 개별적으로 제공하는 것이 좋습니다.  수업 진행 중에 수강생에게 GitHub 랩 단계를 직접 액세스하도록 하면 과정의 일부로 다른 UI에 액세스해야 하므로 수강생이 혼란을 겪을 수 있습니다. 수강생에게 별도의 랩 지침을 사용해야 하는 이유를 설명하면 계속 변경되는 클라우드 기반 인터페이스 및 플랫폼의 특성을 강조하는 데 도움이 됩니다. GitHub 파일 액세스와 GitHub 사이트 탐색에 대한 Microsoft Learning 지원은 이 과정을 가르치는 MCT에게만 제공됩니다.
