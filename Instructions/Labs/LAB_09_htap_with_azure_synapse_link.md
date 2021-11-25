---
lab:
    title: 'Azure Synapse Link를 사용한 HTAP(하이브리드 트랜잭션 분석 처리) 지원'
    module: '모듈 9'
---

# 랩 9 - Azure Synapse Link를 사용한 HTAP(하이브리드 트랜잭션 분석 처리) 지원

이 랩에서는 Azure Synapse Link를 사용해 Synapse 작업 영역에 Azure Cosmos DB 계정을 원활하게 연결하는 방법을 알아봅니다. 구체적으로는 Synapse 링크를 사용하도록 설정하고 구성한 다음, Apache Spark 및 SQL Serverless를 사용하여 Azure Cosmos DB 분석 저장소를 쿼리하는 방법을 배웁니다.

이 랩을 마치면 다음과 같은 역량을 갖추게 됩니다.

- Azure Cosmos DB를 사용하여 Azure Synapse Link 구성
- Synapse Analytics용 Apache Spark로 Azure Cosmos DB 쿼리
- Azure Synapse Analytics용 서버리스 SQL 풀로 Azure Cosmos DB 쿼리

## 랩 설정 및 필수 구성 요소

이 랩을 시작하기 전에 **랩 6: *Azure Data Factory 또는 Azure Synapse 파이프라인을 사용하여 데이터 변환***을 완료해야 합니다.

> **참고**: 랩 6을 완료하지 ***않았지만*** 이 과정의 랩 설정을 <u>완료한</u> 경우에는 이 단계를 완료하여 필요한 연결된 서비스 및 데이터 세트를 만들 수 있습니다.
>
> 1. Synapse Studio의 **관리** 허브에서 다음과 같은 설정으로 **Azure Cosmos DB(SQL API)** 에 대해 새 **연결된 서비스**를 추가합니다.
>       - **이름**: asacosmosdb01
>       - **Cosmos DB 계정 이름**: asacosmosdb*xxxxxxx*
>       - **데이터베이스 이름**: CustomerProfile
> 2. **데이터** 허브에서 다음과 같은 **통합 데이터 세트**를 만듭니다.
>       - **원본**: Azure Cosmos DB(SQL API)
>       - **이름**: asal400_customerprofile_cosmosdb
>       - **연결된 서비스**: asacosmosdb01
>       - **컬렉션**: OnlineUserProfile01
>       - **스키마 가져오기**: 연결/저장소에서

## 연습 1 - Azure Cosmos DB를 사용하여 Azure Synapse Link 구성

Tailwind Traders에서는 Azure Cosmos DB를 사용하여 전자 상거래 사이트의 사용자 프로필 데이터를 저장합니다. Azure Cosmos DB SQL API에서 제공하는 NoSQL 문서 저장소를 사용하면 SQL 구문으로 익숙하게 데이터를 관리하는 동시에 글로벌한 범위의 대규모 파일을 읽고 쓸 수 있습니다.

Tailwind Traders에서는 Azure Cosmos DB의 기능과 성능에는 만족하지만 데이터 웨어하우스의 여러 파티션에 걸쳐 대용량 분석 쿼리를 실행(크로스 파티션 쿼리)하는 데 드는 비용을 우려하고 있습니다. 이 기업은 RU(Azure Cosmos DB 요청 단위)를 늘릴 필요 없이 모든 데이터에 효율적으로 액세스하기를 원합니다. Azure Cosmos DB 변경 피드 메커니즘을 통해 데이터를 변경할 때 컨테이너에서 데이터 레이크로 데이터를 추출하는 옵션도 살펴보았습니다. 이 접근 방식에는 추가 서비스와 코드 종속성 및 솔루션의 장기 유지 관리라는 문제점이 있습니다. Synapse 파이프라인에서 대량 내보내기를 수행할 수 있지만 이럴 경우 지정된 순간에 최신 정보는 얻지 못합니다.

Cosmos DB용 Azure Synapse Link를 활성화하고 Azure Cosmos DB 컨테이너에서 분석 저장소를 사용하도록 설정합니다. 이처럼 구성하면 모든 트랜잭션 데이터가 완전히 격리된 열 저장소에 자동으로 저장됩니다. 이 저장소를 사용하면 트랜잭션 워크로드에 영향을 주거나 RU(리소스 단위) 비용을 발생시키지 않고 Azure Cosmos DB의 작동 데이터를 대상으로 대규모 분석이 가능합니다. Cosmos DB용 Azure Synapse Link를 통해 Azure Cosmos DB와 Azure Synapse Analytics를 긴밀하게 통합할 수 있습니다. 그러면 Tailwind Traders에서는 트랜잭션 워크로드로부터 전체 성능을 격리하고 ETL 없이 작동 데이터에 대한 근 실시간 분석을 실행할 수 있습니다.

Azure Synapse Link는 Cosmos DB의 트랜잭션 처리 및 기본 제공 분석 저장소의 배포 스케일을 Azure Synapse Analytics의 컴퓨팅 성능과 결합하여 Tailwind Traders의 비즈니스 프로세스를 최적화하는 데 사용하는 HTAP(하이브리드 트랜잭션/분석 처리) 아키텍처를 지원합니다. 통합을 통해 ETL 프로세스가 제거되므로 비즈니스 분석가, 데이터 엔지니어 및 데이터 과학자가 작동 데이터에 대해 근 실시간으로 BI, 분석, 기계 학습 파이프라인을 셀프 서비스하고 실행할 수 있습니다.

### 작업 1: Azure Synapse Link 활성화

1. Azure Portal(<https://portal.azure.com>)에서 랩 환경용 리소스 그룹을 엽니다.

2. **Azure Cosmos DB 계정**을 선택합니다.

    ![Azure Cosmos DB 계정이 강조 표시되어 있는 그래픽](images/resource-group-cosmos.png "Azure Cosmos DB account")

3. 왼쪽 메뉴에서 **기능**을 선택한 다음 **Azure Synapse Link**를 선택합니다.

    ![기능 블레이드가 표시되어 있는 그래픽](images/cosmos-db-features.png "Features")

4. **사용**을 선택합니다.

    ![사용이 강조 표시되어 있는 그래픽](images/synapse-link-enable.png "Azure Synapse Link")

    분석 저장소를 사용하여 Azure Cosmos DB 컨테이너를 만들려면 먼저 Azure Synapse Link를 활성화해야 합니다.

5. 다음 작업을 계속 진행하려면 이 작업이 완료될 때까지 기다려야 합니다. 이 작업을 완료하려면 약 1분 정도 걸립니다. Azure **알림** 아이콘을 선택하여 상태를 확인합니다.

    ![Synapse Link를 사용하도록 설정하는 프로세스가 실행되고 있는 화면의 스크린샷](images/notifications-running.png "Notifications")

    이 프로세스가 정상적으로 완료되면 "Synapse Link 사용하도록 설정 중" 옆에 녹색 확인 표시가 나타납니다.

    ![작업이 정상적으로 완료된 것으로 표시된 화면의 스크린샷](images/notifications-completed.png "Notifications")

### 작업 2: 새 Azure Cosmos DB 컨테이너 만들기

Tailwind Traders에는 이름이 **OnlineUserProfile01**인 Azure Cosmos DB 컨테이너가 있습니다. 컨테이너가 이미 생성된 후 Azure Synapse Link 기능을 활성화했으므로 컨테이너에서 분석 저장소를 활성화할 수 없습니다. 파티션 키가 동일한 새 컨테이너를 만들고 분석 저장소를 활성화합니다.

컨테이너를 만든 후 새 Synapse Pipeline을 만들어 **OnlineUserProfile01** 컨테이너의 데이터를 새 컨테이너에 복사합니다.

1. 왼쪽 메뉴에서 **Data Explorer**를 선택합니다.

    ![메뉴 항목이 선택되어 있는 그래픽](images/data-explorer-link.png "Data Explorer")

2. **새 컨테이너**를 선택합니다.

    ![단추가 강조 표시되어 있는 그래픽](images/new-container-button.png "New Container")

3. 다음 설정을 사용하여 새 컨테이너를 만듭니다.
    - **데이터베이스 ID**: 기존 **CustomerProfile** 데이터베이스를 사용합니다.
    - **컨테이너 ID**: `UserProfileHTAP`를 입력합니다.
    - **파티션 키**: `/userId`를 입력합니다.
    - **처리량**: **자동 크기 조정**을 선택합니다.
    - **컨테이너 최대 RU/s**: `4000`을 입력합니다.
    - **분석 저장소**: 켜기

    ![설명에 따라 구성한 양식의 그래픽](images/new-container.png "New container")

    여기서는 **파티션 키** 값을 **userId**로 설정합니다. 이 필드가 쿼리에 가장 많이 사용되고 적절한 분할 성능을 위해 비교적 높은 카디널리티(고유 값 수)를 포함하기 때문입니다. 최대 값이 4,000RU(요청 단위)인 자동 크기 조정으로 처리량을 설정합니다. 즉, 컨테이너에는 최소 400RU가 할당되고(최대 10%), 스케일링 엔진에서 처리량 증가를 보장하기에 충분한 수요를 감지하면 최대 4,000까지 스케일 업됩니다. 마지막으로, 컨테이너에서 **분석 저장소**를 활성화하면 이를 통해 Synapse Analytics 내에서 하이브리드 HTAP(트랜잭션/분석 처리) 아키텍처를 최대한 활용할 수 있습니다.

    새 컨테이너에 복사할 데이터를 간략히 살펴보겠습니다.

4. **CustomerProfile** 데이터베이스 아래에 있는 **OnlineUserProfile01** 컨테이너를 확장한 다음 **항목**을 선택합니다. 문서 중 하나를 선택하고 그 내용을 확인합니다. 문서는 JSON 형식으로 저장됩니다.

    ![컨테이너 항목이 표시되어 있는 그래픽](images/existing-items.png "Container items")

5. 왼쪽 메뉴에서 **키**를 선택합니다. 나중에 **기본 키**와 Cosmos DB 계정 이름(왼쪽 상단 모서리에 있음)이 필요하므로 이 탭을 열어 둡니다.

    ![기본 키가 강조 표시되어 있는 그래픽](images/cosmos-keys.png "Keys")

### 작업 3: 복사 파이프라인 만들기 및 실행

이전 작업에서 분석 저장소를 사용하도록 설정한 새 Azure Cosmos DB 컨테이너를 만들었습니다. 이번에는 Synapse 파이프라인을 사용하여 기존 컨테이너의 내용을 복사해야 합니다.

1. 다른 탭에서 Synapse Studio(<https://web.azuresynapse.net/>)를 열고 **통합** 허브로 이동합니다.

    ![통합 메뉴 항목이 강조 표시되어 있는 그래픽](images/integrate-hub.png "Integrate hub")

2. **+** 메뉴에서 **파이프라인**을 선택합니다.

    ![새 파이프라인 링크가 강조 표시되어 있는 그래픽](images/new-pipeline.png "New pipeline")

3. **활동** 아래에서 **이동 및 변환** 그룹을 확장한 다음 **데이터 복사** 활동을 캔버스로 끕니다. **속성** 블레이드에서 **이름**을 **`Copy Cosmos DB Container`** 로 설정합니다.

    ![새 복사 활동이 표시되어 있는 그래픽](images/add-copy-pipeline.png "Add copy activity")

4. 캔버스에 추가한 새 **데이터 복사** 활동을 선택합니다. 그리고 캔버스 아래의 **원본** 탭에서 **asal400_customerprofile_cosmosdb** 원본 데이터 세트를 선택합니다.

    ![원본이 선택되어 있는 그래픽](images/copy-source.png "Source")

5. **싱크** 탭을 선택하고 **+ 새로 만들기**를 선택합니다.

    ![싱크가 선택되어 있는 그래픽](images/copy-sink.png "Sink")

6. **Azure Cosmos DB(SQL API)** 데이터 세트 형식을 선택하고 **계속**을 선택합니다.

    ![Azure Cosmos DB가 선택되어 있는 그래픽](images/dataset-type.png "New dataset")

7. 다음 속성을 설정하고 **확인**을 클릭합니다.
    - **이름**: `cosmos_db_htap`을 입력합니다.
    - **연결된 서비스**: **asacosmosdb01**을 선택합니다.
    - **컬렉션**: **UserProfileHTAP**/를 선택합니다.
    - **스키마 가져오기**: **스키마 가져오기** 아래에서 **연결/저장소에서**를 선택합니다.

    ![설명에 따라 구성한 양식의 그래픽](images/dataset-properties.png "Set properties")

8. 방금 추가한 새 싱크 아래에서 **삽입** 쓰기 동작이 선택되었는지 확인합니다.

    ![싱크 탭이 표시되어 있는 그래픽](images/sink-insert.png "Sink tab")

9. **모두 게시**, **게시**를 차례로 선택하여 새 파이프라인을 저장합니다.

    ![모두 게시가 표시되어 있는 그래픽](images/publish-all-1.png "Publish")

10. 파이프라인 캔버스 위쪽에서 **트리거 추가**와 **지금 트리거**를 차례로 선택합니다. **확인**을 선택하여 실행을 트리거합니다.

    ![트리거 메뉴가 표시되어 있는 그래픽](images/pipeline-trigger.png "Trigger now")

11. **모니터** 허브로 이동합니다.

    ![모니터 허브가 표시되어 있는 그래픽](images/monitor-hub.png "Monitor hub")

12. **파이프라인 실행**을 선택하고 파이프라인 실행이 정상적으로 완료될 때까지 기다립니다. **새로 고침**을 몇 번 선택해야 할 수도 있습니다.

    ![정상적으로 완료된 파이프라인 실행이 표시된 스크린샷](images/pipeline-run-status.png "Pipeline runs")

    > 파이프라인 실행을 완료하려면 **4분 정도** 걸릴 수 있습니다.

## 연습 2 - Synapse Analytics용 Apache Spark로 Azure Cosmos DB 쿼리

Tailwind Traders는 Apache Spark를 사용하여 새 Azure Cosmos DB 컨테이너를 대상으로 분석 쿼리를 실행하려고 합니다. 이 세그먼트에서는 Synapse Studio의 기본 제공 제스처를 사용하여 Synapse Notebook을 만듭니다. 이 Notebook은 트랜잭션 저장소에 영향을 주지 않고 HTAP 사용 컨테이너의 분석 저장소에서 데이터를 로드합니다.

Tailwind Traders는 각 사용자에게서 확인된 선호 제품 목록과 리뷰 기록의 일치하는 제품 ID를 함께 사용하여 모든 선호 제품 리뷰 목록을 표시하는 방법을 파악하고자 합니다.

### 작업 1: Notebook 만들기

1. **데이터** 허브로 이동합니다.

    ![데이터 허브](images/data-hub.png "Data hub")

2. **연결됨** 탭을 선택하고, **Azure Cosmos DB** 섹션을 확장하고(보이지 않을 경우 오른쪽 상단의 **&#8635;** 단추를 사용하여 Synapse Studio를 새로 고침), **asacosmosdb01 (CustomerProfile)** 연결된 서비스를 확장합니다. 그런 다음에 **UserProfileHTAP** 컨테이너를 마우스 오른쪽 단추로 클릭하고 **새 Notebook**, **데이터 프레임에 로드**를 차례로 선택합니다.

    ![새 Notebook 제스처가 강조 표시되어 있는 그래픽](images/new-notebook.png "New notebook")

    앞에서 만든 **UserProfileHTAP** 컨테이너의 아이콘은 다른 컨테이너의 아이콘과 약간 다릅니다. 이 아이콘은 분석 저장소가 사용하도록 설정되어 있음을 나타냅니다.

3. 새 Notebook의 **연결 대상** 드롭다운 목록에서 **SparkPool01** Spark 풀을 선택합니다.

    ![연결 대상 드롭다운 목록이 강조 표시되어 있는 그래픽](images/notebook-attach.png "Attach the Spark pool")

4. **모두 실행**을 선택합니다.

    ![셀 1 출력이 포함된 새 Notebook이 표시되어 있는 그래픽](images/notebook-cell1.png "Cell 1")

    Spark 세션을 처음으로 시작하려면 몇 분 정도 걸립니다.

    셀 1 내에 생성된 코드에서는 **spark.read** 형식이 **cosmos.olap**로 설정되어 있습니다. 이 형식은 컨테이너의 분석 저장소를 사용하도록 Synapse Link에 명령합니다. 분석 저장소가 아닌 트랜잭션 저장소에 연결하거나 데이터를 변경 피드에서 읽거나 컨테이너에 쓰려는 경우에는 **cosmos.oltp**를 대신 사용하면 됩니다.

    > **참고:** 분석 저장소에 데이터를 쓸 수는 없으며 분석 저장소의 데이터 읽기만 가능합니다. 컨테이너에 데이터를 로드하려면 트랜잭션 저장소에 연결해야 합니다.

    첫 번째 option은 Azure Cosmos DB 연결된 서비스의 이름을 구성합니다. 두 번째 `option`은 읽으려는 데이터가 있는 Azure Cosmos DB 컨테이너를 정의합니다.

5. 실행한 셀 아래의 **+ 코드** 단추를 선택합니다. 그러면 첫 번째 코드 셀 아래에 새 코드 셀이 추가됩니다.

6. 데이터 프레임에는 불필요한 추가 열이 포함되어 있습니다. 불필요한 열을 제거하여 정리된 데이터 프레임 버전을 만들어 보겠습니다. 이렇게 하려면 새 코드 셀에 다음을 입력하고 실행합니다.

    ```python
    unwanted_cols = {'_attachments','_etag','_rid','_self','_ts','collectionType','id'}

    # Remove unwanted columns from the columns collection
    cols = list(set(df.columns) - unwanted_cols)

    profiles = df.select(cols)

    display(profiles.limit(10))
    ```

    이제 출력에는 원하는 열만 포함됩니다. **preferredProducts** 및 **productReviews** 열에는 자식 요소가 포함되어 있습니다. 행의 값을 확장하면 자식 요소를 확인할 수 있습니다. 이전 모듈에서 확인한 것처럼, Azure Cosmos DB Data Explorer 내의 **UserProfiles01** 컨테이너에는 원시 JSON 형식이 표시됩니다.

    ![셀 출력이 표시되어 있는 그래픽](images/cell2.png "Cell 2 output")

7. 이제 처리해야 하는 레코드 수를 확인해야 합니다. 이렇게 하려면 새 코드 셀에 다음을 입력하고 실행합니다.

    ```python
    profiles.count()
    ```

    결과로 레코드 수 99,999개가 표시되어야 합니다.

8. 이제 각 사용자의 **preferredProducts** 열 배열과 **productReviews** 열 배열을 사용하여 사용자의 선호 제품 목록에 포함된 제품 중 사용자가 리뷰를 남긴 제품과 일치하는 제품의 그래프를 만들어야 합니다. 이렇게 하려면 해당 2개 열의 평면화된 값이 들어 있는 새 데이터 프레임 2개를 만들어야 합니다. 그래야 이후 단계에서 두 데이터 프레임을 조인할 수 있습니다. 새 코드 셀에 다음을 입력하고 실행합니다.

    ```python
    from pyspark.sql.functions import udf, explode

    preferredProductsFlat=profiles.select('userId',explode('preferredProducts').alias('productId'))
    productReviewsFlat=profiles.select('userId',explode('productReviews').alias('productReviews'))
    display(productReviewsFlat.limit(10))
    ```

    이 셀에서는 특수 PySpark [explode](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=explode#pyspark.sql.functions.explode) 함수를 가져왔습니다. 이 함수는 배열의 각 요소에 해당하는 새 열을 반환합니다. 이 함수를 사용하면 **preferredProducts** 및 **productReviews** 열을 평면화하여 데이터를 더 쉽게 읽거나 쿼리할 수 있습니다.

    ![셀 출력이 표시되어 있는 그래픽](images/cell4.png "Cell 4 output")

    **productReviewFlat** 데이터 프레임 내용이 표시된 셀 출력을 살펴봅니다. 새 **productReviews** 열에 사용자의 선호 제품 목록과 일치 여부를 확인하려는 **productId**가 포함되어 있으며, 표시 또는 저장하려는 **reviewText**도 포함되어 있습니다.

9. 이제 **preferredProductsFlat** 데이터 프레임의 내용을 살펴보겠습니다. 이렇게 하려면 새 셀에 다음 코드를 입력하고 **실행**합니다.

    ```python
    display(preferredProductsFlat.limit(20))
    ```

    ![셀 출력이 표시되어 있는 그래픽](images/cell5.png "Cell 5 results")

    선호 제품 배열에서 **explode** 함수를 사용했으므로 열 값이 **userId** 및 **productId** 행으로 평면화되어 사용자를 기준으로 순서가 지정되었습니다.

10. 이제 **productReviewFlat** 데이터 프레임 내용을 추가로 평면화하여 **productReviews.productId** 및 **productReviews.reviewText** 필드를 추출하고 각 데이터 조합용으로 새 행을 만들어야 합니다. 이렇게 하려면 새 코드 셀에 다음을 입력하고 실행합니다.

    ```python
    productReviews = (productReviewsFlat.select('userId','productReviews.productId','productReviews.reviewText')
        .orderBy('userId'))

    display(productReviews.limit(10))
    ```

    이제 출력에는 각 `userId`에 해당하는 행이 여러 개 표시됩니다.

    ![셀 출력이 표시되어 있는 그래픽](images/cell6.png "Cell 6 results")

11. 마지막 단계에서는 **userId** 및 **productId** 값에서 **preferredProductsFlat** 및 **productReviews** 데이터 프레임을 조인하여 선호 제품 리뷰 그래프를 작성합니다. 이렇게 하려면 새 코드 셀에 다음을 입력하고 실행합니다.

    ```python
    preferredProductReviews = (preferredProductsFlat.join(productReviews,
        (preferredProductsFlat.userId == productReviews.userId) &
        (preferredProductsFlat.productId == productReviews.productId))
    )

    display(preferredProductReviews.limit(100))
    ```

    > **참고**: 테이블 보기에서 열 머리글을 클릭하여 결과 집합을 정렬할 수 있습니다.

    ![셀 출력이 표시되어 있는 그래픽](images/cell7.png "Cell 7 results")

12. Notebook의 오른쪽 상단에서 **세션 중지** 단추를 사용하여 Notebook 세션을 중지합니다. 그런 다음에 Notebook을 닫고 변경 내용을 취소합니다.

## 연습 3 - Azure Synapse Analytics용 서버리스 SQL 풀로 Azure Cosmos DB 쿼리

Tailwind Traders는 T-SQL을 사용하여 Azure Cosmos DB 분석 저장소를 탐색하려고 합니다. 그리고 분석 저장소 탐색용 보기를 만들려고 합니다. 이 보기는 다른 분석 저장소 컨테이너 또는 데이터 레이크의 파일과의 조인에 사용할 수도 있고, Power BI 등의 외부 도구가 액세스할 수도 있습니다.

### 작업 1: 새 SQL 스크립트 만들기

1. **개발** 허브로 이동합니다.

    ![개발 허브](images/develop-hub.png "Develop hub")

2. **+** 메뉴에서 **SQL 스크립트**를 선택합니다.

    ![SQL 스크립트 단추가 강조 표시되어 있는 그래픽](images/new-script.png "SQL script")

3. 스크립트가 열릴 때, 오른쪽의 **속성** 창에서 **이름**을 `User Profile HTAP`로 변경합니다. 그런 다음에 **속성** 단추를 사용하여 창을 닫습니다.

    ![속성 창이 표시되어 있는 그래픽](images/new-script-properties.png "Properties")

4. 서버리스 SQL 풀(**기본 제공**)이 선택되어 있는지 확인합니다.

    ![서버리스 SQL 풀이 선택되어 있는 그래픽](images/built-in-htap.png "Built-in")

5. 다음 SQL 쿼리를 붙여넣습니다. OPENROWSET 문에서 **YOUR_ACCOUNT_NAME**을 Azure Cosmos DB 계정 이름으로 바꾸고 **YOUR_ACCOUNT_KEY**를 Azure Portal(다른 탭에 여전히 열려 있어야 함)의 **키** 페이지에 있는 Azure Cosmos DB 기본 키로 바꿉니다.

    ```sql
    USE master
    GO

    IF DB_ID (N'Profiles') IS NULL
    BEGIN
        CREATE DATABASE Profiles;
    END
    GO

    USE Profiles
    GO

    DROP VIEW IF EXISTS UserProfileHTAP;
    GO

    CREATE VIEW UserProfileHTAP
    AS
    SELECT
        *
    FROM OPENROWSET(
        'CosmosDB',
        N'account=YOUR_ACCOUNT_NAME;database=CustomerProfile;key=YOUR_ACCOUNT_KEY',
        UserProfileHTAP
    )
    WITH (
        userId bigint,
        cartId varchar(50),
        preferredProducts varchar(max),
        productReviews varchar(max)
    ) AS profiles
    CROSS APPLY OPENJSON (productReviews)
    WITH (
        productId bigint,
        reviewText varchar(1000)
    ) AS reviews
    GO
    ```

6. **실행** 단추를 사용하여 쿼리를 실행합니다. 이 쿼리는 다음을 수행합니다.
    - 아직 없을 경우 **Profiles**라는 새로운 서버리스 SQL 풀 데이터베이스를 만듭니다.
    - 데이터베이스 컨텍스트를 **Profiles** 데이터베이스로 변경합니다.
    - **UserProfileHTAP** 보기가 있으면 삭제합니다.
    - SQL 보기 **UserProfileHTAP**를 만듭니다.
    - OPENROWSET 문을 사용하여 데이터 원본 형식을 **CosmosDB**로 설정하고 계정 세부 정보를 설정합니다. 그런 다음 Azure Cosmos DB 분석 저장소 컨테이너 **UserProfileHTAP**를 통해 보기를 만들 것임을 지정합니다.
    - JSON 문서의 속성 이름 일치 여부를 확인한 다음 적절한 SQL 데이터 형식을 적용합니다. **preferredProducts** 및 **productReviews** 필드는 **varchar(max)** 로 설정됩니다. 이 두 속성 내에는 모두 JSON 형식 데이터가 포함되어 있기 때문입니다.
    - JSON 문서의 **productReviews** 속성에는 중첩 하위 배열이 포함되어 있으므로 스크립트는 문서의 모든 속성을 배열의 모든 요소와 "조인"해야 합니다. Synapse SQL에서는 중첩 배열에 OPENJSON 함수를 적용하여 중첩 구조를 평면화할 수 있습니다. 앞에서 Synapse Notebook의 Python **explode** 함수를 사용하여 값을 평면화했던 것처럼 여기서도 **productReviews** 내의 값을 평면화합니다.

7. **데이터** 허브로 이동합니다.

    ![데이터 허브](images/data-hub.png "Data hub")

8. **작업 영역** 탭을 선택하고 **데이터베이스** 그룹을 확장합니다. **Profiles** SQL 주문형 데이터베이스를 확장합니다(목록에 보이지 않으면 **데이터베이스** 목록을 새로 고침). **보기**를 확장한 다음에 **UserProfileHTA** 보기를 마우스 오른쪽 단추로 클릭하고 **새 SQL 스크립트**, **상위 100개 행 선택**을 차례로 선택합니다.

    ![상위 100개 행 선택 쿼리 옵션이 강조 표시되어 있는 그래픽](images/new-select-query.png "New select query")

9. 스크립트가 **기본 제공** SQL 풀에 연결되었는지 확인한 후에 쿼리를 실행하고 결과를 봅니다.

    ![뷰 결과가 표시되어 있는 그래픽](images/select-htap-view.png "Select HTAP view")

    **preferredProducts** 및 **productReviews** 필드가 보기에 포함되어 있습니다. 이 두 필드에는 모두 JSON 형식 값이 들어 있습니다. 보기에서는 CROSS APPLY OPENJSON 문이 **productId** 및 **reviewText** 값을 새 필드에 추출하여 **productReviews** 필드의 중첩 하위 배열 값을 올바르게 평면화했습니다.
