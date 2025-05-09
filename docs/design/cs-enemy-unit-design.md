# # SRPG 적 유닛 디자인 v1.1
**버전**: 1.1 **일자**: 2025-05-05 **작성자**: 젠타님 **문서 유형**: 기획서
## 1. 개요
본 문서는 SRPG 게임의 적 유닛 시스템 설계에 관한 내용을 담고 있습니다. 적 유닛의 카테고리, 특성, 스탯 구성, AI 패턴 및 성장 시스템 등을 정의합니다. 본 문서는 "SRPG 게임 전술 전투 디자인 문서"와 연계되어 사용됩니다.
## 2. 적 유닛 카테고리화
### 2.1 난이도 등급
적 유닛은 전투 난이도에 따라 4가지 등급으로 분류됩니다:
| **등급** | **스탯 배율** | **특징** |
|:-:|:-:|:-:|
| **일반 (Normal)** | 기본 스탯(1.0배) | 기본 전투력, 특별한 능력 없음 |
| **정예 (Elite)** | 일반의 1.6배 | 강화된 스탯, 더 높은 위협도 |
| **챔피언 (Champion)** | 일반의 2.6배 | 고급 스탯, 높은 위협도 |
| **보스 (Boss)** | 일반의 3.0배 + HP×3 추가 | 최상위 스탯, 높은 전투력 |
**참고**: MOV(이동력) 값은 난이도와 관계없이 기본값을 유지합니다. 단, 민첩(DEX)에 따른 이동력 보너스는 적용됩니다.
**스탯 계산 예시**
10레벨 오크 일반 등급이 다음과 같은 스탯을 가질 경우:
"hp": 30, "str": 8, "dex": 5, "int": 0, "def": 6, "mdef": 2, "spd": 4, "luk": 1, "mov": 5
정예 오크는 아래와 같이 계산됩니다(소수점 반올림):
"hp": 48, "str": 13, "dex": 8, "int": 0, "def": 10, "mdef": 3, "spd": 6, "luk": 2, "mov": 5
### 2.2 종족 분류
적 유닛의 종족에 따라 고유한 특성과 전투 방식이 결정됩니다:
| **종족** | **특성** |
|:-:|:-:|
| **인간 (Human)** | - 균형 잡힌 능력치<br>- 직업 시스템(플레이어 캐릭터와 동일) 사용<br>- 직업별 특화 스킬 적용 (예: 기사 - DEF +3, MDEF +2) |
| **언데드 (Undead)** | - 물리 내성: DEF +3<br>- 화염 약점: 화염 속성 공격 시 데미지 2배<br>- 치유 효과 반전: 치유 마법 시 해당 수치만큼 데미지 입음 |
| **마물 (Magical Beast)** | - 마법 내성: MDEF +3<br>- 높은 이동력: MOV +1<br>- 낮은 방어력: DEF -3 |
| **드래곤류 (Draconic)** | - 특수 브레스 공격:<br> • 독 브레스: 5턴 동안 HP 20% 손상<br> • 화염 브레스: 화염 속성 데미지, MDEF 무력화<br> • 번개 브레스: 데미지 + 2턴 마비<br> • 얼음 브레스: 얼음 데미지 + 5턴 동안 민첩 0 |
| **정령 (Elemental)** | - 마법 공격으로만 데미지를 입음<br>- 물리 공격 완전 내성 |
| **기계 (Mechanical)** | - STR +5<br>- DEF +10<br>- MDEF +5<br>- 레벨업 성장 없음(고정 스탯) |
### 2.3 공격 유형
적 유닛과 플레이어 캐릭터 모두에게 적용되는 공격 유형입니다:
| **유형** | **범위 및 특성** |
|:-:|:-:|
| **근접 (Melee)** | - 1타일 범위, 8방향<br>- 높은 공격력 |
| **원거리 (Ranged)** | - 2~3타일 범위<br>- 근접 전투 불가<br>- 중간 공격력 |
| **장거리 (Long Range)** | - 3~6타일 범위<br>- 명중률 -30%<br>- 근접 전투 불가 |
| **근/원거리 (Hybrid)** | - 1~3타일 범위<br>- 상황에 따른 전투 스타일 변경 가능 |
| **마법 (Magic)** | - 다양한 범위와 효과<br>- 속성별 대상 방어력(MDEF) 차등 적용 |
| **지원 (Support)** | - 아군 강화/회복<br>- 적군 약화/상태이상 부여 |
## 3. 적 유닛 데이터 구조
### 3.1 기본 구조
모든 적 유닛은 다음과 같은 JSON 형식의 데이터 구조를 가집니다:
{
  "id": "enemy_001",
  "name": "언데드 병사",
  "category": "normal",
  "race": "undead",
  "job": "Skeleton Warrior",
  "level": 3,
  "exp": 50,
  "expType": "Normal",
  "stats": {
    "hp": 30, "str": 8, "dex": 5, "int": 0,
    "def": 6, "mdef": 2, "spd": 4, "luk": 1, "mov": 5
  },
  "strategy": {
    "aiPattern": ["AggroNearest"],
    "isLeader": false,
    "isEnemy": true,
    "attackRange": 1,
    "attackType": "physical"
  },
  "equipment": {
    "weapon": "녹슨 검",
    "armor": "뼈 갑옷",
    "ring1": null,
    "ring2": null,
    "necklace": null
  }
}
### 3.2 성장 시스템
적 유닛의 레벨업 시 스탯 성장을 위한 개선된 시스템:
"growth": {
  "base": {
    "hp": 8, "str": 4, "dex": 2, "int": 0,
    "def": 4, "mdef": 1, "spd": 2, "luk": 1
  },
  "variation": {
    "hp": 2, "str": 2, "dex": 1, "int": 0,
    "def": 2, "mdef": 1, "spd": 1, "luk": 1
  },
  "min": {
    "hp": 4, "str": 1, "dex": 0, "int": 0,
    "def": 1, "mdef": 0, "spd": 0, "luk": 0
  }
}
* **base**: 기본 성장치
* **variation**: 랜덤 변동 범위 (±variation)
* **min**: 최소 보장 성장치

⠀레벨업 시 스탯 증가량은 base ± 랜덤(0~variation) 으로 계산되며, min 값보다 작을 경우 min 값으로 보정됩니다.
## 4. AI 패턴
### 4.1 기본 AI 패턴
적 유닛이 사용하는 기본 AI 패턴:
| **패턴** | **설명** |
|:-:|:-:|
| **AggroNearest** | 가장 가까운 적을 공격. 거리가 같다면 HP가 낮은 대상 우선 |
| **FocusWeakest** | 가장 낮은 HP를 가진 적을 추적하여 공격 |
| **CautiousWeakest** | FocusWeakest와 유사하나, HP가 30% 이하이면 안전 지역으로 후퇴 |
| **FocusStrongest** | 가장 높은 공격력(STR/INT)을 가진 적을 우선 공격 |
| **CautiousStrongest** | FocusStrongest와 유사하나, HP가 30% 이하이면 안전 지역으로 후퇴 |
| **FollowLeader** | 리더 유닛의 3타일 이내에 위치하려 시도, 가능하면 리더와 같은 대상 공격 |
| **SnipeNearestRanged** | 사거리 내의 가장 가까운 적 공격, 사거리 밖이면 접근 |
| **SnipeWeakestRanged** | 사거리 내의 가장 약한 적 공격, 사거리 밖이면 접근 |
### 4.2 고급 AI 패턴
보스나 챔피언 유닛이 사용하는 고급 AI 패턴:
| **패턴** | **설명** |
|:-:|:-:|
| **ProtectVIP** | 지정된 아군 유닛을 보호하며, 해당 유닛에 접근하는 적 우선 공격 |
| **AreaControl** | 특정 지역(포탈, 아이템 등)을 지키며 해당 지역에 접근하는 적 우선 공격 |
| **PhasedAttack** | HP 구간에 따라 다른 공격 패턴과 능력 사용 (75%, 50%, 25% 구간) |
| **SummonReinforcement** | HP가 특정 수치 이하로 떨어지면 추가 적 소환 |
| **AdaptiveDefense** | 받은 공격 유형에 따라 해당 방어력 증가 |
## 5. 챕터별 적 스케일링
챕터 진행에 따른 적 유닛 레벨 및 난이도 스케일링:
| **챕터** | **적 평균 레벨** | **유닛 구성** | **보스 레벨** |
|:-:|:-:|:-:|:-:|
| **1** | 2-4 | 일반 100% | 6 |
| **2** | 4-6 | 일반 90%, 정예 10% | 9 |
| **3** | 6-8 | 일반 85%, 정예 15% | 12 |
| **4** | 8-10 | 일반 80%, 정예 15%, 챔피언 5% | 14 |
| **5** | 10-12 | 일반 70%, 정예 25%, 챔피언 5% | 16 |
| **6** | 12-14 | 일반 60%, 정예 30%, 챔피언 10% | 18 |
| **7** | 14-16 | 일반 50%, 정예 35%, 챔피언 15% | 20 |
| **8** | 16-18 | 일반 40%, 정예 40%, 챔피언 20% | 22 |
## 6. 난이도 설정에 따른 조정
게임의 전체 난이도 설정에 따른 적 유닛 스탯 조정:
| **난이도** | **적 스탯 배율** | **경험치 배율** | **특징** |
|:-:|:-:|:-:|:-:|
| **쉬움** | 85% | 120% | 적 특수 능력 빈도 감소 |
| **보통** | 100% | 100% | 기본 밸런스 |
| **어려움** | 115% | 85% | 적 AI 공격성 증가 |
| **하드코어** | 130% | 70% | 적 특수 능력 빈도 증가, 특수 패턴 추가 |
## 7. 적 유닛 예시
다음은 "언데드" 종족 적 유닛의 예시입니다:
### 7.1 해골 전사 (일반)
{
  "id": "skeleton_warrior",
  "name": "해골 전사",
  "category": "normal",
  "race": "undead",
  "job": "Skeleton Warrior",
  "level": 1,
  "exp": 30,
  "expType": "Normal",
  "stats": {
    "hp": 20, "str": 7, "dex": 4, "int": 0,
    "def": 5, "mdef": 1, "spd": 4, "luk": 1, "mov": 4
  },
  "growth": {
    "base": { "hp": 8, "str": 4, "dex": 2, "int": 0, "def": 4, "mdef": 1, "spd": 2, "luk": 1 },
    "variation": { "hp": 2, "str": 1, "dex": 1, "int": 0, "def": 1, "mdef": 1, "spd": 1, "luk": 1 },
    "min": { "hp": 4, "str": 2, "dex": 1, "int": 0, "def": 2, "mdef": 0, "spd": 1, "luk": 0 }
  },
  "strategy": {
    "aiPattern": "AggroNearest",
    "isLeader": false,
    "isEnemy": true,
    "attackRange": 1,
    "attackType": "physical"
  },
  "equipment": {
    "weapon": "녹슨 검",
    "armor": "뼈 갑옷"
  }
}
### 7.2 해골 궁수 (일반)
{
  "id": "skeleton_archer",
  "name": "해골 궁수",
  "category": "normal",
  "race": "undead",
  "job": "Skeleton Archer",
  "level": 2,
  "exp": 35,
  "expType": "Normal",
  "stats": {
    "hp": 18, "str": 5, "dex": 7, "int": 0,
    "def": 3, "mdef": 1, "spd": 5, "luk": 2, "mov": 4
  },
  "growth": {
    "base": { "hp": 7, "str": 3, "dex": 5, "int": 0, "def": 2, "mdef": 1, "spd": 3, "luk": 2 },
    "variation": { "hp": 2, "str": 1, "dex": 1, "int": 0, "def": 1, "mdef": 1, "spd": 1, "luk": 1 },
    "min": { "hp": 3, "str": 1, "dex": 2, "int": 0, "def": 1, "mdef": 0, "spd": 1, "luk": 1 }
  },
  "strategy": {
    "aiPattern": "SnipeWeakestRanged",
    "isLeader": false,
    "isEnemy": true,
    "attackRange": 2,
    "attackType": "physical"
  },
  "equipment": {
    "weapon": "낡은 활",
    "armor": "뼈 갑옷"
  }
}
### 7.3 네크로맨서 (정예)
{
  "id": "necromancer",
  "name": "네크로맨서",
  "category": "elite",
  "race": "human",
  "job": "Necromancer",
  "level": 5,
  "exp": 80,
  "expType": "Slow",
  "stats": {
    "hp": 30, "str": 2, "dex": 4, "int": 10,
    "def": 3, "mdef": 7, "spd": 4, "luk": 3, "mov": 3
  },
  "growth": {
    "base": { "hp": 6, "str": 1, "dex": 2, "int": 7, "def": 2, "mdef": 5, "spd": 2, "luk": 2 },
    "variation": { "hp": 2, "str": 1, "dex": 1, "int": 2, "def": 1, "mdef": 1, "spd": 1, "luk": 1 },
    "min": { "hp": 3, "str": 0, "dex": 1, "int": 3, "def": 1, "mdef": 2, "spd": 1, "luk": 1 }
  },
  "strategy": {
    "aiPattern": "CautiousWeakest",
    "isLeader": true,
    "isEnemy": true,
    "attackRange": 3,
    "attackType": "magic"
  },
  "equipment": {
    "weapon": "죽음의 지팡이",
    "armor": "암흑 로브",
    "ring1": "언데드 컨트롤 링"
  }
}
## 8. 전투 맵 시스템
### 8.1 전투 맵 데이터 구조
{
  "mapId": 3,
  "name": "숲속 전장",
  "width": 16,
  "height": 12,
  "terrain": [...],
  "playerDeploymentArea": {
    "position": "leftCenter",
    "range": {"startX": 0, "startY": 3, "endX": 2, "endY": 8}
  },
  "enemyDeploymentArea": {
    "position": "rightCenter",
    "range": {"startX": 13, "endX": 15, "startY": 3, "endY": 8}
  },
  "objectives": {
    "primary": "모든 적 처치",
    "secondary": "10턴 내 완료"
  }
}
### 8.2 적 유닛 생성 프로세스
**1** **전투 맵 로드 시** 적 그룹 데이터 로드
	* 전투 맵 입장 시 전달되는 적 부대 리스트 사용
	* 적 부대 리스트는 전략 맵에서 결정되어 전달됨
**2** **적 유닛 데이터 테이블에서** 해당 ID의 적 유닛 템플릿 로드
	* 각 적 유닛의 기본 스탯 및 특성 로드
**3** **난이도 설정 및 종족 특성 적용**
	* 게임 난이도에 따른 스탯 조정 (향후 검토 필요)
	* 종족별 특성은 전투 룰에 반영 (전투 시스템 기획서에 상세 정의)
**4** **장비 및 AI 패턴 설정**
	* 장비는 미리 정의된 값 사용
	* AI 패턴은 적 부대 데이터에 포함
	* 이벤트나 특정 조건에 따라 전투 중 변경 가능
**5** **전투 맵에 배치**
	* 맵 데이터의 적 배치 좌표 범위 내에 자동 배치
	* 기본적으로 적은 맵 오른쪽에 배치 (플레이어는 왼쪽)
	* 특수 맵은 각 맵 데이터에 배치 위치 좌표 정의

⠀9. 구현 가이드라인
### 9.1 파일 구조
Assets/
├── Resources/
│   ├── Data/
│   │   ├── Enemies/
│   │   │   ├── NormalEnemies.json
│   │   │   ├── EliteEnemies.json
│   │   │   ├── Champions.json
│   │   │   ├── Bosses.json
│   │   ├── EnemyGroups/
│   │   │   ├── Chapter1Groups.json
│   │   │   ├── Chapter2Groups.json
│   │   ├── Maps/
│   │   │   ├── BattleMaps.json
## 10. 전술 전투 디자인 문서 업데이트 필요사항
"SRPG 게임 전술 전투 디자인 문서"에서 업데이트가 필요한 부분:
**1** **종족 특성을 반영한 전투 룰 추가**
	* 8. 전투 수학 - 새로운 섹션 추가: "종족 특성 효과"
	* 각 종족별 특수 특성이 전투 계산식에 미치는 영향 상세화
**2** **공격 유형 확장**
	* 근접, 원거리, 장거리, 근/원거리 하이브리드, 마법, 지원 등의 카테고리 상세화
	* 사거리별 명중률 보정 및 효과 정의
**3** **상태이상 효과 시스템**
	* 드래곤 브레스 등의 상태이상 효과(독, 마비, 동상 등) 정의
	* 상태이상 지속 시간 및 효과 계산 방식 추가
**4** **적 AI 패턴과 연계된 전투 로직**
	* AI 패턴에 따른 적 행동 결정 프로세스 추가
	* 플레이어와 적 유닛 간의 상호작용 규칙 정의
**5** **난이도별 전투 균형 조정 시스템**
	* 각 난이도에 따른 스탯, 경험치, 특수 능력 발동 확률 등의 조정치 정의
	* 동적 난이도 조정 메커니즘(선택 사항)

⠀11. 변경 이력
| **버전** | **날짜** | **변경 내용** |
|:-:|:-:|:-:|
| 1.0 | 2025-05-05 | 초기 문서 작성 |
| 1.1 | 2025-05-05 | 전투 맵 시스템 상세화, 용어 변경, 전투 규칙 연계 보강 |

## 참고 문서
* SRPG 게임 전술 전투 디자인 문서 v2.4
* SRPG 게임 UIUX 설계 문서 v1.0
